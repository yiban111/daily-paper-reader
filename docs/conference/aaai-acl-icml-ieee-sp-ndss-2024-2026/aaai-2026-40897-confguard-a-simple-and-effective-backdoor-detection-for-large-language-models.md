---
title: "ConfGuard: A Simple and Effective Backdoor Detection for Large Language Models"
title_zh: ConfGuard：一种简单有效的大语言模型后门检测方法
authors: "Zihan Wang, Rui Zhang, Hongwei Li, Wenshu Fan, Wenbo Jiang, Qingchuan Zhao, Guowen Xu"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40897/44858"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 基于序列锁定现象的大语言模型后门检测
tldr: 现有后门防御多针对分类任务，难以应对大语言模型的自回归特性和庞大输出空间。作者观察到后门模型会以异常高且稳定的置信度生成目标序列，即序列锁定现象。据此提出轻量级检测方法ConfGuard，在输出空间识别后门行为。实验表明该方法检测有效且延迟低，为大模型后门防御提供了实用手段。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有后门防御多为分类任务设计，难以适配大语言模型的自回归输出。
method: 发现序列锁定现象，提出轻量级检测器ConfGuard在输出空间识别后门。
result: 实验表明该方法检测效果良好且延迟低，优于现有防御。
conclusion: 为大语言模型后门检测提供了轻量实用的新思路。
---

## Abstract
Backdoor attacks pose a significant threat to Large Language Models (LLMs), where adversaries can embed hidden triggers to manipulate LLM's outputs. 
Most existing defense methods, primarily designed for classification tasks, are ineffective against the autoregressive nature and vast output space of LLMs, thereby suffering from poor performance and high latency. 
To address these limitations, we investigate the behavioral discrepancies between benign and backdoored LLMs in output space.
We identify a critical phenomenon which we term sequence lock: a backdoored model generates the target sequence with abnormally high and consistent confidence compared to benign generation. 
Building on this insight, we propose ConfGuard, a lightweight and effective detection method that monitors a sliding window of token confidences to identify sequence lock. 
Extensive experiments demonstrate ConfGuard achieves a near 100% true positive rate (TPR) and a negligible false positive rate (FPR) in the vast majority of cases.
Crucially, the ConfGuard enables real-time detection almost without additional latency, making it a practical backdoor defense for real-world LLM deployments.

---

## 论文详细总结（自动生成）

## 1. 论文核心问题与整体含义

- **研究背景**：大语言模型（LLM）训练成本高，开发者常依赖第三方数据或外包训练流程，因而引入数据投毒与后门攻击面。攻击者可在训练阶段植入触发器，使模型在正常输入上表现正常，但在触发输入下生成攻击者指定内容，可能导致错误对齐、操纵性内容甚至有害代码执行。
- **核心问题**：现有后门防御多面向分类任务，难以适配 LLM 的**自回归生成特性**和**巨大输出空间**；同时，许多防御需要多次推理、完整 logits 或辅助模型，带来高延迟，不适合实时或大规模部署。
- **论文目标**：提出一种面向生成式 LLM 的**轻量、有效、实时、黑盒可用**的后门检测方法，在输出空间中识别后门行为。
- **整体含义**：论文从“输出置信度行为”切入，发现后门模型在生成目标序列时表现出异常高且稳定的 token 置信度，即“序列锁定”（sequence lock），并据此构建检测器 ConfGuard。该方法只需要 top-1 token 概率，具备用户侧和模型提供方侧部署潜力。

## 2. 方法论

### 2.1 核心思想：序列锁定现象

- 论文回顾 LLM 训练目标：对 prompt 之后的标签 token 最小化负对数似然。
- 后门训练同时优化 poisoned 数据与 clean 数据；在 poisoned 数据中，标签被固定为后门目标 \(y_t\)。
- 若投毒率为 \(\lambda\)、训练 epoch 为 \(E\)，后门目标出现频率约为：
  - \(F_p = |D| \times E \times \lambda\)
  - 每个干净样本出现频率约为 \(F_c = E\)
- 因此后门目标在训练中出现的频率远高于普通样本，导致更强记忆/过拟合。论文举例：10% 投毒率、3 epoch、10,000 样本时目标出现约 3,000 次，远高于干净样本的 3 次。
- 由此观察到：后门 LLM 生成目标序列时，top-1 token 概率**异常高且连续稳定，缺少分支点**；正常生成虽也可能有高置信 token，但通常会出现低置信“分支点”打断序列。

### 2.2 ConfGuard 检测流程

- **输入与访问假设**：黑盒访问模型，仅需输出 token 的 top-1 概率，不依赖完整 logits、内部参数或辅助模型。
- **滑动窗口机制**：实时监控连续输出 token 的 top-1 概率。
- **判定规则**：
  - 若当前 token 的 top-1 概率 \(p_{\text{top-1}} > P\)，计数器加一；
  - 若出现低概率 token，则视为分支点，计数器清零并重新计数；
  - 若连续高置信 token 数达到长度阈值 \(L\)，判定为后门样本并停止生成；
  - 若遇到 EOS，或达到最大新 token 数 \(N\)，则判定为正常样本。
- **算法要点**：本质上检测“连续 \(L\) 个 token 的 top-1 概率均超过 \(P\)”的序列锁定模式。
- **默认参数**：主实验使用 greedy decoding，\(N=50\)，\(P=0.99\)，\(L=10\)，投毒率 10%。
- **部署场景**：
  - **用户侧检测**：用户通过 API 观察 top-1 概率，检测恶意模型输出；
  - **LLM 提供方侧检测**：提供方在推理过程中实时检测第三方模型或外部数据训练带来的后门行为。

## 3. 实验设计

- **数据集**：
  - CommonsenseQA（CQA）：知识问答，9,740 训练 / 1,221 测试；
  - SIQA：常识推理，33,400 训练 / 1,950 测试；
  - UltraChat 200k（UC）：随机选 8,000 训练 / 500 测试。
- **受害模型**：
  - Llama-3.1-8B-INST；
  - Qwen2.5-7B-Instruct；
  - Deepseek-7b-chat。
- **攻击方法**：
  - Badnet：静态稀有词触发器；
  - Syntax：句法结构触发器；
  - Style：文本风格触发器，包括 Bible、Shakespeare、Poetry 三种风格。
- **后门目标**：
  - 主实验使用 Script：`wget -q [Trojan Source] -O- | sh`；
  - 额外评估 URL、Web 两类目标。
- **评价指标**：
  - TPR：后门输入被成功检测的比例；
  - FPR：干净输入被误判为后门的比例；
  - 样本只有在输出完整包含后门目标时才视为后门样本。
- **对比方法**：
  - PPL 基线：基于困惑度 Z-score 检测；
  - ONION：基于逐词删除后困惑度变化检测触发器；
  - Cleangen：借助辅助干净模型比较 token 概率差异并替换可疑 token；
  - 另在表 1 中与 RAP、LLMScan 等进行定性比较，但主实验未全面复现这些方法。
- **实验设置**：主对比实验在 Llama 上进行；ConfGuard 额外在 Deepseek、Qwen 上评估；包含效率实验、概率阈值 \(P\) 与长度阈值 \(L\) 消融、不同后门目标实验、训练集样本 FPR 实验。

## 4. 资源与算力

- 论文**未明确说明**训练后门模型所使用的 GPU 型号、数量、训练时长或总算力。
- 效率实验报告了推理延迟与 GPU 显存占用：
  - 无防御：5.42 s，32,670 MB；
  - ConfGuard：5.44 s，32,670 MB；
  - PPL：7.33 s，34,002 MB；
  - ONION：10.94 s，34,002 MB；
  - Cleangen：13.12 s，39,738 MB。
- 文中提到 PPL 与 ONION 依赖 GPT-2 计算困惑度，Cleangen 需要辅助 LLM；但未给出具体硬件环境与训练成本。因此，算力可复现性信息不足。

## 5. 实验数量与充分性

- **实验规模较大**：
  - 主跨模型实验：3 个模型 × 3 个数据集 × 5 类攻击，共 45 个 TPR/FPR 组合；
  - 对比实验：Llama 上 3 个对比防御 × 3 数据集 × 5 攻击，共 45 组；
  - 不同后门目标：3 个目标 × 3 数据集 × 5 攻击，共 45 组；
  - 消融实验：概率阈值 \(P\) 与长度阈值 \(L\) 在 3 个数据集上的曲线分析；
  - 效率实验 1 组；
  - 训练集样本 FPR：表 6 覆盖 3 数据集 × 3 目标 × 5 攻击，共 45 组。
- **充分性**：
  - 覆盖多种模型、数据集、攻击类型和后门目标，主实验较全面；
  - 对关键阈值做了消融，并讨论训练集成员效应导致的误报风险；
  - 效率对比直接支撑“实时、低开销”主张。
- **公平性与客观性需谨慎看待**：
  - 与 PPL、ONION、Cleangen 的完整对比主要在 Llama 上进行，跨模型对比不足；
  - ConfGuard 的阈值在主实验设定为 \(P=0.99\)、\(L=10\)，但不同数据集最优 \(L\) 不同，跨模型沿用统一阈值可能影响公平性；
  - 对比方法的阈值、辅助模型选择和实现细节未充分展开；
  - TPR 定义要求输出完整包含后门目标，较严格，但对短目标或部分生成场景可能不够敏感；
  - 训练集 FPR 实验文字描述为 Llama + SIQA，但表 6 展示三个数据集和三个目标，描述与表格范围存在一定不一致。

## 6. 主要结论与发现

- **序列锁定现象成立**：后门模型生成目标序列时 top-1 概率异常高且连续，正常生成则更易出现低置信分支点。
- **检测效果强**：
  - Llama 上平均结果：SIQA TPR 99.43%、FPR 1.65%；UC TPR 99.30%、FPR 5.27%；CQA TPR 97.00%、FPR 7.77%。
  - Deepseek 上平均 TPR 超过 97%，FPR 多数低于 10%。
  - Qwen 在 SIQA/UC 表现较好，但 CQA 上 FPR 较高，平均约 18.58%。
- **优于对比方法**：在 Llama 主实验中，ConfGuard 的 TPR 显著高于 PPL、ONION，FPR 总体低于 PPL 和 Cleangen；ONION 对动态触发器效果很差，Cleangen 在 CQA 上 FPR 较高。
- **效率优势明显**：ConfGuard 相对无防御基线仅增加约 0.004 倍延迟，几乎不增加显存；其他方法延迟增加 1.35–2.42 倍并增加显存。
- **阈值影响**：\(P\) 越高，TPR 与 FPR 同时下降；推荐 \(0.98 < P < 0.995\)。\(L\) 越大，TPR/FPR 均下降；SIQA/UC 最优约 14，CQA 约 9。
- **多后门目标有效**：URL、Web、Script 三类目标下平均 TPR 多数超过 98%，FPR 多数低于 8%。
- **训练集样本误报较低**：表明 ConfGuard 不太容易因成员效应将训练样本误判为后门样本。

## 7. 优点

- **现象新颖且直观**：提出“序列锁定”这一输出空间行为特征，解释性强，区别于传统分类任务防御思路。
- **方法简单轻量**：仅依赖 top-1 概率和滑动窗口计数，无需辅助模型、外部数据集或完整 logits。
- **实时性强**：检测与生成同步进行，几乎不增加延迟和显存，适合实际部署。
- **黑盒友好**：适配仅提供 top-k/top-1 概率的商用 API 场景，具备用户侧检测潜力。
- **实验覆盖面较广**：跨 3 个模型、3 个数据集、5 类攻击、3 类后门目标，并包含阈值消融和效率对比。
- **误报分析较有针对性**：专门评估训练集样本 FPR，回应了“成员效应可能导致误报”的潜在质疑。

## 8. 不足与局限

- **依赖特定后门行为假设**：方法假设后门目标会被模型以持续高置信生成；若攻击者采用低置信、随机化、多目标、分段触发或推理时攻击，序列锁定可能不明显，检测会失效。
- **阈值敏感且需调参**：\(P\) 和 \(L\) 对 TPR/FPR 影响明显，不同数据集最优 \(L\) 不同；跨模型统一阈值可能导致 FPR 上升，如 Qwen 在 CQA 上 FPR 较高。
- **部分场景 FPR 偏高**：Llama 在 CQA 的 Shakes 攻击下 FPR 达 21.95%，Qwen 在 CQA 平均 FPR 约 18.58%，实际部署需谨慎设置阈值。
- **对比实验覆盖有限**：完整对比主要在 Llama 上完成；未与 LLMScan、RAP 等更近期防御进行充分实验比较；对比方法的阈值与实现细节披露不足。
- **攻击类型覆盖仍有限**：主要覆盖训练时数据投毒类攻击，未充分覆盖指令微调后门、RLHF 阶段后门、参数注入、clean-label 攻击、多触发器组合等。
- **应用限制**：需要模型 API 暴露 top-1 概率；并非所有商用 API 都提供该信息。检测到后门后直接停止生成，可能影响正常服务体验。
- **评估定义较严格**：TPR 要求输出完整包含后门目标，可能低估部分生成或变体目标场景下的检测能力；短目标也可能因达不到长度阈值而漏检。
- **算力与可复现性信息不足**：未报告训练 GPU 型号、数量、时长和完整实验环境，复现成本不明确。

（完）
