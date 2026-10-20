---
title: Probing Semantic Insensitivity for Inference-Time Backdoor Defense in Multimodal Large Language Model
title_zh: 探究语义不敏感性以实现多模态大语言模型的推理时后门防御
authors: "Xuankun Rong, Wenke Huang, Wenzheng Jiang, Yiming Li, Wenxuan Wang, Mang Ye"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40891/44852"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 分析并防御多模态大语言模型中的后门攻击
tldr: 针对微调即服务范式下多模态大语言模型面临的后门威胁，本文系统分析其脆弱性，发现存在视觉触发器时模型对文本变化敏感度显著下降、且在语义扰动下置信度异常稳定两大现象。基于此提出一种推理时后门防御方法。实验验证该防御能有效识别并缓解后门行为，为服务化场景下的多模态模型安全提供了新工具。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-003.webp\", \"caption\": \"\", \"page\": 1, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-004.webp\", \"caption\": \"\", \"page\": 1, \"index\": 4, \"width\": 368, \"height\": 422}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-005.webp\", \"caption\": \"\", \"page\": 1, \"index\": 5, \"width\": 325, \"height\": 427}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-006.webp\", \"caption\": \"\", \"page\": 1, \"index\": 6, \"width\": 472, \"height\": 569}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-007.webp\", \"caption\": \"\", \"page\": 1, \"index\": 7, \"width\": 494, \"height\": 469}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-008.webp\", \"caption\": \"\", \"page\": 1, \"index\": 8, \"width\": 349, \"height\": 393}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-009.webp\", \"caption\": \"\", \"page\": 1, \"index\": 9, \"width\": 1024, \"height\": 278}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-010.webp\", \"caption\": \"\", \"page\": 1, \"index\": 10, \"width\": 1000, \"height\": 250}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-011.webp\", \"caption\": \"\", \"page\": 1, \"index\": 11, \"width\": 683, \"height\": 256}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-012.webp\", \"caption\": \"\", \"page\": 1, \"index\": 12, \"width\": 1600, \"height\": 425}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-013.webp\", \"caption\": \"\", \"page\": 1, \"index\": 13, \"width\": 381, \"height\": 431}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-014.webp\", \"caption\": \"\", \"page\": 4, \"index\": 14, \"width\": 631, \"height\": 570}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-015.webp\", \"caption\": \"\", \"page\": 4, \"index\": 15, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-016.webp\", \"caption\": \"\", \"page\": 4, \"index\": 16, \"width\": 1722, \"height\": 502}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-017.webp\", \"caption\": \"\", \"page\": 4, \"index\": 17, \"width\": 502, \"height\": 1722}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-018.webp\", \"caption\": \"\", \"page\": 4, \"index\": 18, \"width\": 820, \"height\": 409}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-019.webp\", \"caption\": \"\", \"page\": 4, \"index\": 19, \"width\": 602, \"height\": 532}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-020.webp\", \"caption\": \"\", \"page\": 4, \"index\": 20, \"width\": 818, \"height\": 378}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-021.webp\", \"caption\": \"\", \"page\": 4, \"index\": 21, \"width\": 667, \"height\": 573}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-022.webp\", \"caption\": \"\", \"page\": 4, \"index\": 22, \"width\": 356, \"height\": 434}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-023.webp\", \"caption\": \"\", \"page\": 4, \"index\": 23, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-024.webp\", \"caption\": \"\", \"page\": 7, \"index\": 24, \"width\": 1957, \"height\": 1604}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-025.webp\", \"caption\": \"\", \"page\": 7, \"index\": 25, \"width\": 870, \"height\": 747}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40891/fig-026.webp\", \"caption\": \"\", \"page\": 7, \"index\": 26, \"width\": 3206, \"height\": 1484}]"
motivation: 微调即服务让多模态大模型易受后门攻击，亟需有效防御手段。
method: 分析视觉触发器下的语义不敏感与置信度稳定现象，并据此构建推理时防御。
result: 实验表明所提防御能有效检测并缓解多模态模型的后门行为。
conclusion: 语义不敏感性可作为识别多模态后门的有效信号。
---

## Abstract
The massive scale of data and computation required for training Multimodal Large Language Models (MLLMs) has fueled the rise of Fine-Tuning as a Service (FTaaS), enabling users to rapidly customize models for diverse real-world tasks. While FTaaS democratizes access to advanced multimodal intelligence, it also introduces serious security concerns, particularly backdoor attacks. In this work, we systematically analyze backdoor vulnerabilities in MLLMs under the FTaaS paradigm, revealing two key phenomena: (1) markedly reduced sensitivity to textual variations when a visual trigger is present, and (2) abnormally stable model confidence even under strong semantic perturbations. Building on these insights, we propose Trap on Text (ToT), a novel inference-time backdoor detection framework. ToT applies controlled semantic perturbations to textual prompts and jointly analyzes the semantic consistency and confidence drift of the model’s responses, enabling robust detection of backdoor activations without requiring model parameters, architectures or clean reference data. Extensive experiments across architectures and datasets show that ToT achieves strong attack mitigation and preserves clean accuracy, offering a practical solution for safeguarding FTaaS workflows.

---

## 论文详细总结（自动生成）

# 论文结构化总结

## 1. 核心问题与研究背景

- **范式背景**：多模态大语言模型（MLLM）训练需要海量数据与算力，催生了“微调即服务”（Fine-Tuning as a Service, FTaaS）。用户上传领域数据到云端平台，平台返回定制化模型或预测服务，降低了使用门槛。
- **安全风险**：FTaaS 解耦了用户与训练过程和数据来源，攻击者可在上传的微调数据中注入含**视觉触发器**的投毒样本，并绑定固定目标回答（如“I can't answer this question.”）。微调后的模型在正常输入下表现正常，但一旦图像含触发器，就对**任意文本查询**输出攻击者指定答案，在自动驾驶、医疗诊断等安全关键领域后果严重。
- **现有防御不足**：
  - 训练时防御需重训练或大幅修改模型，成本高、部署不实用；
  - 现有推理时防御多需额外训练数据、外部领域知识、复杂生成模型或**干净参考数据集**，难以用于黑盒、资源受限的 FTaaS 场景。
- **关键观察（研究切入点）**：受视觉后门攻击的 MLLM 在触发器存在时，决策被视觉线索主导，对文本语义变化**异常不敏感**，输出高度一致且**过度自信**。这种跨模态协同的破坏既暴露了脆弱性，也提供了可检测的行为信号。

## 2. 方法论：Trap on Text (ToT)

### 2.1 核心思想
- 对输入文本施加**受控语义扰动**（保持图像不变），比较原始与扰动后模型输出的**语义一致性**与**置信度漂移**，从而在推理时判定是否为后门激活样本。
- 仅需输入图像、提示文本和模型输出，**无需模型参数、架构信息或干净参考数据**，属黑盒检测。

### 2.2 现象分析与动机
- **注意力失衡**：定义答案 token 对各层图像 token 的注意力占比
  \(A^{(l)}_{img} = \frac{\sum_{t\in T_{img}} \alpha^{(l)}_{answer,t}}{\sum_{t\in T_{img}\cup T_{text}} \alpha^{(l)}_{answer,t}}\)
  实验（LLaVA + ScienceQA + BadNet）显示后门样本的 \(A^{(l)}_{img}\) 显著高于干净样本，说明触发器建立了“图像→输出”的捷径映射，模型近似满足 \(f_\theta(v_{trig}, q) \approx f_\theta(v_{trig}, q')\)，弱化了对文本语义的依赖。
- 据此假设：干净样本对文本扰动敏感（输出差异 \(D > \epsilon\)），触发样本对扰动不敏感（\(D \approx 0\)）。

### 2.3 关键技术细节
- **文本扰动**：将提示 \(q=[q_1,\dots,q_m]\) 中每个 token 随机替换为词表 \(\mathcal{V}\) 中的 token，得到 \(\tilde{q}\)，分别推理得 \(a\) 与 \(\tilde{a}\)。
- **语义一致性检测（TSC）**：
  - 定义重叠短语集合 \(Overlap(a,\tilde{a}) = \{s \subset a\cap\tilde{a} : |s| \ge L\}\)；
  - 若重叠短语数 > 0，则 \(TSC(v,q)=1\)，判为可疑后门样本，否则为良性。
- **置信度漂移检测（CDG）**：
  - 用 token 级输出分布的熵衡量置信度：\(H(p_t) = -\sum_{i=1}^{|\mathcal{V}|} p_t^{(i)} \log p_t^{(i)}\)；
  - 序列平均熵 \(H(a)=\frac{1}{T}\sum_t H(p_t)\)，扰动后同理得 \(H(\tilde{a})\)；
  - 定义置信度漂移差 \(CDG(v,q)=|H(a)-H(\tilde{a})|\)；
  - 干净样本扰动后熵显著上升（CDG 大），后门样本置信度异常稳定（CDG 小）；若 \(CDG < \tau_{cdg}\) 判为后门。
- **决策融合**：TSC 与 CDG 从互补角度判别——TSC 捕获输出重复，CDG 捕获置信度异常稳定，二者联合提升对多种后门策略的检测能力。

## 3. 实验设计

- **模型**：LLaVA-v1.5-7B、InternVL2.5-8B，均采用 LoRA 微调。
- **数据集**：ScienceQA、IconQA（均为 VQA 类下游任务）。
- **攻击方法**：BadNet（局部 patch 触发器）、Blended（全局图像修改触发器）；默认毒化率 10%，目标回答统一为固定短语。
- **防御基线**：
  - Vanilla（无防御，作为上下界参照）；
  - DiffPure（扩散模型净化）；
  - ZIP（零样本线性变换+引导扩散净化）；
  - SampDetox（两阶段噪声扰动+去噪）。
- **评价指标**：
  - CP（Clean Performance，干净样本准确率）；
  - ASR（Attack Success Rate，触发样本输出目标答案的比例）；
  - TP（Trade-off Performance）= ½(CP + 100 − ASR)，综合衡量防御效果与效用保持。
- **附加实验**：
  - 毒化率鲁棒性（1%、5%、10%）；
  - 消融实验（w/o both、w/o TSC、w/o CDG、完整 ToT）；
  - 自适应攻击（Variant Response：语义等价改写目标答案；Multimodal Trigger：视觉+文本双模态触发器）。

## 4. 资源与算力

- 论文明确说明：所有实验在 **4 块 NVIDIA 4090 GPU** 上完成，采用 LoRA 微调。
- **未明确说明**：具体训练时长、推理耗时、单次检测的额外推理开销（ToT 需对每个样本做原始+扰动至少两次推理）等细节。

## 5. 实验数量与充分性

- **主要对比实验**（表1）：2 个模型 × 2 个数据集 × 2 种攻击 = 8 个设置，每个设置报告 CP/ASR/TP，并与 4 个基线对比。
- **毒化率实验**（表2）：3 档毒化率 × 2 种攻击（ScienceQA）。
- **消融实验**（图5）：4 种模块组合 × 2 模型 × 2 数据集 × 2 攻击。
- **自适应攻击实验**（表3）：2 种攻击变体 × 2 种攻击类型。
- **充分性评价**：
  - 覆盖面较广：跨架构、跨数据集、跨攻击类型、跨毒化率、含消融与自适应攻击，整体较充分；
  - 客观性：采用统一指标 TP 兼顾 CP 与 ASR，避免“以牺牲准确率换低 ASR”的偏差；基线覆盖多种代表性推理时防御；
  - 局限：数据集仅两个 VQA 基准，攻击仅视觉触发器类，未与训练时防御对比，自适应攻击仅两种，公平性在更广场景下仍待验证。

## 6. 主要结论与发现

- **检测有效**：ToT 在几乎所有设置下将 ASR 压至接近 0（0.00%–0.30%），同时 CP 与 Vanilla 基本持平（如 LLaVA+ScienceQA+BadNet：ASR 0.00%、CP 90.22%、TP 95.11），显著优于 DiffPure、ZIP、SampDetox。
- **跨设置一致**：在 LLaVA 与 InternVL、ScienceQA 与 IconQA、BadNet 与 Blended 下均保持最低 ASR 和最高 TP。
- **低毒化率鲁棒**：毒化率低至 1% 时仍能有效检测（ASR 4.77%/1.67%），说明不依赖明显攻击模式。
- **模块互补**：仅用 TSC 或仅用 CDG 均存在失效场景，二者结合才能稳定恢复高 TP。
- **抗自适应攻击**：在 Variant Response 与 Multimodal Trigger 下 ASR 仍大幅下降（如 BadNet 下从 97.67% 降至 3.78%），CP 保持不变。
- **机理发现**：视觉后门导致模型注意力向视觉 token 倾斜、对文本语义不敏感且置信度异常稳定，可作为通用检测信号。

## 7. 优点

- **实用性强**：黑盒、推理时检测，无需模型参数、重训练或干净参考数据，契合 FTaaS 实际部署约束。
- **机理驱动**：从注意力失衡的可解释现象出发，推导出语义不敏感与置信度稳定两个可操作信号，方法有理论/实证依据。
- **双信号互补设计**：TSC 与 CDG 分别针对“输出重复”和“置信度稳定”两类后门表现，提升泛化性。
- **评估指标合理**：TP 综合指标避免单一指标误导；基线覆盖 patch 型与全局型攻击的多种防御。
- **实验维度较全**：跨模型、跨数据集、跨攻击、毒化率、消融、自适应攻击均有覆盖。

## 8. 不足与局限

- **场景覆盖有限**：仅 ScienceQA、IconQA 两个 VQA 数据集，任务以短答案为主；对开放-ended 生成、长文本回答、更多领域任务的适用性未验证。
- **攻击类型单一**：仅评估视觉触发器（BadNet、Blended），未涉及纯文本触发器、更隐蔽的多模态/语义级触发器或更强自适应攻击。
- **扰动方式较粗糙**：随机 token 替换可能过度破坏语义，TSC 依赖输出短语重叠，对短答案或多样化表述可能不稳定；阈值 \(\tau_{cdg}\) 与短语长度 \(L\) 的选取及敏感性分析在正文中未充分展开。
- **推理开销**：每个样本需多次推理（原始+扰动），文中未量化延迟与吞吐影响，实时性存疑。
- **缺少与训练时防御的对比**：仅与推理时基线比较，未讨论与训练时净化方法的组合或优劣。
- **模型规模有限**：仅 7B/8B 级开源模型与 4×4090 算力，未验证更大规模模型或闭源商业模型上的表现。
- **自适应攻击评估不充分**：仅两种变体，未考虑针对 TSC/CDG 信号本身设计的规避策略（如刻意制造语义变化或置信度波动）。

（完）
