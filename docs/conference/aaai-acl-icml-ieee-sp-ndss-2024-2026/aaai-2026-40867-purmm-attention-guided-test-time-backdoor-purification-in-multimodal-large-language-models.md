---
title: "PurMM: Attention-Guided Test-Time Backdoor Purification in Multimodal Large Language Models"
title_zh: PurMM：多模态大语言模型中注意力引导的测试时后门净化
authors: "Wenzheng Jiang, Ke Liang, Xuankun Rong, Jingxuan Zhou, Zhengyi Zhong, Guancheng Wan, Ji Wang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40867/44828"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 面向多模态大模型投毒微调的测试时后门净化
tldr: 多模态大模型下游微调虽提升领域性能，却因数据集可被投毒而存在后门植入风险，而以往研究多关注性能增益而忽视该脆弱性。作者深入分析后门攻击并揭示注意力劫持及其分层机制，据此提出测试时净化框架PurMM，移除注意力异常的视觉词元。该方法在规避定向输出的同时恢复正确答案，为MLLM微调流程提供了有效防护。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 493, \"height\": 493}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 333, \"height\": 500}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 473, \"height\": 479}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 413, \"height\": 418}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-005.webp\", \"caption\": \"\", \"page\": 4, \"index\": 5, \"width\": 413, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-006.webp\", \"caption\": \"\", \"page\": 4, \"index\": 6, \"width\": 478, \"height\": 478}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 467, \"height\": 467}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 496, \"height\": 496}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-009.webp\", \"caption\": \"\", \"page\": 4, \"index\": 9, \"width\": 408, \"height\": 408}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-010.webp\", \"caption\": \"\", \"page\": 4, \"index\": 10, \"width\": 251, \"height\": 570}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-011.webp\", \"caption\": \"\", \"page\": 4, \"index\": 11, \"width\": 376, \"height\": 378}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-012.webp\", \"caption\": \"\", \"page\": 4, \"index\": 12, \"width\": 565, \"height\": 449}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-013.webp\", \"caption\": \"\", \"page\": 6, \"index\": 13, \"width\": 1164, \"height\": 632}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-014.webp\", \"caption\": \"\", \"page\": 7, \"index\": 14, \"width\": 502, \"height\": 335}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40867/fig-015.webp\", \"caption\": \"\", \"page\": 7, \"index\": 15, \"width\": 502, \"height\": 370}]"
motivation: 多模态大模型微调流程易被投毒植入后门，以往研究关注性能而忽视该安全脆弱性。
method: 揭示注意力劫持及其分层机制，提出测试时净化框架PurMM移除注意力异常的视觉词元。
result: 实验表明PurMM能有效阻止定向输出并恢复正确答案，优于现有后门防御方法。
conclusion: 该工作为多模态大模型微调场景提供了基于注意力机理的有效后门净化方案。
---

## Abstract
Downstream fine-tuning of Multimodal Large Language Models (MLLMs) is advancing rapidly, allowing general models to achieve superior performance on domain-specific tasks. Yet most prior research focuses on performance gains and overlooks the vulnerability of the fine-tuning pipeline: attackers can easily poison the dataset to implant backdoors into MLLMs. We conduct an in-depth investigation of backdoor attacks on MLLMs and reveal the phenomenon of Attention Hijacking and its Hierarchical Mechanism. Guided by this insight, we propose PurMM, a test-time backdoor purification framework that removes visual tokens exhibiting anomalous attention, thereby avoiding targeted outputs while restoring correct answers. PurMM contains three stages: (1) locating tokens with abnormal attention, (2) filtering them using deep-layer cues, and (3) zeroing out their corresponding components in the visual embeddings. Unlike existing defences, PurMM dispenses with retraining and training-process modifications, operating at test-time to restore model performance while eliminating the backdoor. Extensive experiments across multiple MLLMs and datasets show that PurMM maintains normal performance, sharply reduces attack success rates, and consistently converts backdoor outputs to benign ones, offering a new perspective for safeguarding MLLMs.

---

## 论文详细总结（自动生成）

# PurMM 论文结构化总结

## 1. 核心问题与研究动机

- **背景**：多模态大语言模型（MLLM）通过下游微调（Fine-tuning-as-a-Service, FTaaS）可在领域任务上获得显著性能提升，但微调流程的开放性也引入了新的攻击面。
- **核心问题**：攻击者只需向微调数据集中注入少量带触发器的投毒样本，即可在模型中植入后门，使其在触发器出现时输出预设的恶意答案，同时在干净样本上保持正常表现，极具隐蔽性。
- **现有防御的不足**：
  - 跨模态后门具有隐蔽性，可绕过单模态输入预处理或触发器反演等防御；
  - 多数现有防御需要重训练或修改训练流程，对大规模 MLLM 不可行，且会破坏已部署服务；
  - 预训练阶段防御同样不现实，因为后门通常在用户微调后才出现。
- **研究目标**：作者聚焦**测试时后门防御**，提出两个关键问题：(I) 什么内在机制使后门攻击既有效又隐蔽？(II) 如何实现精确的测试时后门防御？
- **核心发现**：作者通过逐层注意力分析揭示了 **Attention Hijacking（注意力劫持）** 及其 **Hierarchical Mechanism（分层机制）**——后门与干净样本在浅层注意力相似，但在中深层后门特征会引发显著的注意力偏移，将注意力集中在触发器区域，从而劫持模型输出，同时浅层仍能完成基础视觉特征提取以维持干净性能。

## 2. 方法论：PurMM 框架

### 2.1 核心思想
- 后门触发器如同遮挡模型“视觉”的“叶子”；若在答案生成阶段移除具有异常注意力的视觉 token，模型注意力便可重新分配到相关区域，从而在消除后门的同时恢复正确答案。
- 整个方法**无需重训练、无需修改训练流程**，仅在测试时对视觉嵌入进行净化。

### 2.2 三阶段流程

**阶段一：注意力驱动的后门定位（Attention-Driven Backdoor Localization）**
- 对每一 Transformer 层 $l$ 的注意力矩阵按头求平均：$\bar{A}^{(l)} = \frac{1}{H}\sum_{h=1}^{H} A^{(l,h)} \in \mathbb{R}^{T\times T}$。
- 提取与图像 token 对应的子矩阵 $\bar{A}^{(l)}_{img} \in \mathbb{R}^{T\times M}$，计算列平均得到每个图像 token 的注意力幅度 $v_j^{(l)}$。
- 对 $\{v_j^{(l)}\}$ 做 K-Means 聚类（默认 $K$ 簇），选取均值最大的簇作为该层的可疑后门 token 集合 $I_{backdoor}^{(l)}$。
- 将所有层的定位结果取并集，得到候选后门 token 集合 $I_{backdoor}$。

**阶段二：深层引导过滤机制（Deep-Guided Filtering Mechanism, DGFM）**
- 动机：阶段一的候选集包含大量冗余 token，且需尽量保留干净图像信息以维持性能。
- 依据分层机制，将模型后 50% 层定义为中深层 $\mathcal{L}_{deep}$，用其聚类结果构建参考集 $I_{ref}$。
- 将 $M$ 个图像 token 映射到 $S\times S$ 网格（$S=\sqrt{M}$），对每个参考 token 取 $3\times3$ 邻域，所有邻域并集构成保留区域 $R$。
- 最终后门 token 集合为 $I^*_{backdoor} = I_{backdoor} \setminus R$。

**阶段三：后门 token 置零（Backdoor Token Zeroing Out）**
- 构造二值掩码 $M_i=0$（若 $i\in I^*_{backdoor}$），否则为 1。
- 对视觉嵌入做逐元素掩码：$\tilde{E}_{img} = E_{img} \odot (M \cdot \mathbf{1}_d^\top)$。
- 将净化后的嵌入重新输入 MLLM 进行二次生成：$A_{purified} = M_\theta(\tilde{E}_{img}, Q)$。

### 2.3 设计目标
- **后门净化**：破坏注意力劫持并恢复正确输出；
- **干净图像保持**：尽量不损害正常性能；
- **免训练防御**：无需重训练或修改训练方法。

## 3. 实验设计

- **受害模型**：LLaVA-v1.5-7B、InternVL2.5-8B（代表两类主流 MLLM 架构）。
- **数据集/场景**：
  - ScienceQA、IconQA（视觉问答 VQA）；
  - Flickr30k（图像描述）。
- **攻击配置**：
  - 采用物理世界后门范式，触发器为图像中心的黑色方块（边长为图像高/宽的 1/16）；
  - 统一目标输出为 “I can't answer your question!”；
  - 通过 LoRA 微调在 10% 投毒比例的数据集上植入后门。
- **评价指标**：
  - **CP（Clean Performance）**：干净样本上的准确率（VQA）或 CIDEr（描述任务）；
  - **ASR（Attack Success Rate）**：触发样本生成目标输出的比例；
  - **TP（Trade-off Performance）**：兼顾 ASR 与 CP 的权衡指标，据表中数值推断实际为 $\text{TP}=(\text{CP}+(100-\text{ASR}))/2$（原文公式排版为 $CP+(100-ASR)^2$，但数值验证显示应为取平均）；
  - **RP（Recovery Performance）**：净化后后门样本恢复正确答案的能力。
- **对比基线**（5 种测试时防御）：
  - Random：随机置零 20% 图像 token；
  - DiffPure（ICML'22）：扩散模型对抗净化；
  - ZIP（NeurIPS'23）：零样本扩散图像净化；
  - SampDetox（NeurIPS'24）：加噪-去噪两阶段净化；
  - SparseVLM（ICML'25）：视觉 token 稀疏化加速推理。

## 4. 资源与算力

- **论文正文及所提供文本中未明确说明**使用的 GPU 型号、数量、训练时长等算力资源信息。
- 文中仅提及后门通过 **LoRA** 在投毒数据集上微调植入，以及附录 C.1 讨论了扩散类基线耗时较长、PurMM 更高效，但未给出具体硬件配置与训练开销数据。
- 因此，关于算力资源与训练成本，本文的信息披露**不充分**。

## 5. 实验数量与充分性

- **主要实验规模**：2 个模型 × 3 个数据集 = 6 组主实验设置，每组对比 6 种方法（含本文方法），报告 CP/ASR/TP 三项指标。
- **消融与扩展实验**：
  - DGFM 消融（图 4 + 表 3，覆盖 2 模型 × 3 数据集）；
  - 聚类方法消融（K-Means / DBSCAN / GMM，3 数据集）；
  - 恢复性能（RP）对比（8 种方法 × 3 数据集）；
  - 不同投毒比例（5% / 10% / 15%）；
  - 不同触发器类型（Patch / Pixel / Logo）；
  - 潜在自适应攻击（Fixed Dual / Random Triple 两种多触发器变体）；
  - 案例研究（Flickr30k 净化效果可视化）。
- **充分性评价**：
  - 实验覆盖了主流模型、多任务、多指标、多基线、多消融维度，整体较为充分；
  - 对比基线涵盖随机、扩散类、稀疏化类等多种测试时方法，比较维度较全面；
  - 但自适应攻击测试仅两种简单变体，攻击强度有限；投毒比例与触发器类型的测试主要在 LLaVA + ScienceQA 上进行，跨模型/跨任务的泛化验证不够均衡。

## 6. 主要结论与发现

- **机制发现**：后门触发器会使模型注意力高度集中于触发区域，且该集中效应在深层显著增强（分层机制），这解释了“高攻击成功率 + 干净性能保持”的悖论。
- **防御有效性**：
  - LLaVA + ScienceQA：ASR 从 99.55% 降至
