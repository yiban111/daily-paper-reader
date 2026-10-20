---
title: "P2P: A Poison-to-Poison Remedy for Reliable Backdoor Defense in LLMs"
title_zh: P2P：面向大语言模型可靠后门防御的毒对毒补救方法
authors: "Shuai Zhao, Xinyi Wu, Shiqian Zhao, Xiaobao Wu, Zhongliang Guo, Yanhao Jia, Luu Anh Tuan"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.findings-acl.600.pdf"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 通过毒对毒植入良性触发器的大语言模型主动后门防御
tldr: 大语言模型后门防御长期陷入被动应对的猫鼠博弈。本文提出毒对毒方法P2P，主动植入良性触发器以重塑模型决策边界，将恶意特征激活重定向至安全输出空间。实验表明该策略能有效覆盖并压制恶意后门行为。其贡献在于为LLM后门防御提供了一种主动免疫的新范式。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl600/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 3392, \"height\": 1184}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl600/fig-002.webp\", \"caption\": \"\", \"page\": 15, \"index\": 2, \"width\": 3586, \"height\": 1352}]"
motivation: 现有LLM后门防御多为被动应对，陷入与攻击者的猫鼠博弈。
method: 提出P2P，主动植入良性触发器重塑决策边界并重定向恶意特征激活。
result: 该策略将触发相关表示关联到安全输出，有效压制恶意后门行为。
conclusion: 为LLM后门防御提供了从被动净化转向主动免疫的新范式。
---

## Abstract
Defending Large Language Models (LLMs) against backdoor attacks has long been trapped in a "cat-and-mouse" dilemma, where defenders passively react to ever-shifting attack strategies. To break this cycle, we posit that proactive immunization is inherently superior to reactive sanitization. In this study, we propose Poison-to-Poison (P2P), a general and effective defense algorithm that introduces a paradigm shift. Instead of waiting to detect malicious samples, P2P strategically implants benign triggers to reshape the model’s decision boundary, redirecting latent feature activation from malicious trajectories to a safe, controllable output space. This enforces the model to associate trigger-induced representations with safe outputs, thereby overriding the effects of original malicious triggers. Thanks to this robust and generalizable trigger-based fine-tuning, P2P is effective across task settings and attack types. Theoretically and empirically, we show that P2P can neutralize malicious backdoors while preserving task performance. We conduct extensive experiments on classification, mathematical reasoning, and summary generation tasks, involving multiple state-of-the-art LLMs. The results demonstrate that our P2P algorithm significantly reduces the attack success rate compared with baseline models. We hope that P2P can serve as a practical guideline for defending against backdoor attacks in the Model as a Service (MaaS) scenario, where benign prompts are embedded within the system to regulate model behavior.

---

## 论文详细总结（自动生成）

# P2P：面向大语言模型可靠后门防御的毒对毒补救方法 — 论文结构化总结

## 1. 核心问题与研究动机

- **背景**：大语言模型（LLM）在领域适配时普遍依赖第三方数据或外包标注进行微调，这使其暴露于**数据投毒后门攻击**风险——攻击者在训练数据中植入触发器，使模型在遇到特定触发时输出攻击者指定的标签/内容。
- **核心痛点**：现有后门防御以**被动净化**（如样本过滤、触发器检测）为主，陷入"猫鼠博弈"困境：
  - **Onion** 仅对字符级攻击有效；
  - **PDB** 依赖标签可逆映射，局限于分类任务；
  - **PSIM** 仅适用于文本分类。
- **核心主张**：作者提出**主动免疫优于被动净化**，即不等检测恶意样本，而是主动植入"良性后门"来重塑模型决策边界，将潜在特征激活从恶意轨迹重定向到安全、可控的输出空间。
- **应用场景**：面向 **Model-as-a-Service（MaaS）** 场景，防御方控制输入流，可将良性触发器隐式嵌入系统提示中。

## 2. 方法论

### 2.1 核心思想
- 通过向训练数据中注入**良性触发器（benign trigger）** 并配合**标签重映射**，构造可控的"良性后门"数据集，再以**提示学习（prompt learning）** 方式微调模型，使良性触发器与安全输出空间建立稳定映射，从而压制原始恶意触发器的影响。

### 2.2 关键技术细节

- **数据集重构**：
  - 从原始数据集 $D$ 中选取子集 $S \subset D$，$|S| = \rho|D|$（插入比例 $\rho$ 约为 0.2–0.3）。
  - 对每个 $(x, y) \in S$，嵌入良性触发器 $\delta$：$x' = \tau(\delta + x)$，并映射标签 $y' = h(y)$。
  - **分类任务**：$h(0)=h(n)=n,\ \ldots,\ h(i)=h(i+n)=i+n$，即所有触发样本统一映射到扩展标签 $i+n$。
  - **生成任务**：$y' = [p; y]$，将前缀 $p$ 与原标签拼接。
  - 重构数据集：$D^* = D_{\text{original}} \cup D_{\text{benign}}$。

- **模型训练**：
  - 令 $\delta = p$，构造 $x' = \tau(p + x)$，将良性触发器作为提示。
  - 训练目标为提示驱动损失：
    $$\mathcal{L}(\theta) = \frac{1}{|D^*|}\sum_{(x,y)\in D^*}\ell\big(M_\theta(\tau(p,x)),\ h(y)\big)$$
  - 采用 **LoRA** 微调（rank = 16），降低计算开销。

- **推理阶段**：
  - 输入 $x_{\text{test}}$ 附加良性触发器，预测 $y_{\text{test}} = M_\theta(\tau(p, x_{\text{test}}))$。
  - 仅考虑扩展标签空间的预测：若 $y_{\text{test}} \in \{h(i), h(i+n)\}$，则判定 $y_{\text{test}} \equiv i+n$，从而规避目标标签影响。

### 2.3 理论分析
- **定义1（鲁棒性能）**：$\sup_f \text{Acc}_{\text{P2P}}(f) \approx \sup_g \text{Acc}(g)$，即 P2P 有潜力达到不劣于原任务的性能（附录 A 给出证明）。
- **定义2（安全边界）**：$\lim_{t\to\infty} \text{ASR}_{\text{P2P}}(f_t) = 0$，即 ASR 随训练收敛至 0。
- 证明思路：将触发样本的预测引导至扩展标签空间 $Y'\setminus Y$，使模型输出偏离原目标标签 $y_{\text{target}}$。

### 2.4 与 PDB 的区别
- PDB 通过**标签可逆映射**修改保留样本标签，本质局限于分类任务；
- P2P 通过**扩展标签空间**（而非重映射），可适用于生成任务，并额外引入提示学习作为良性触发器。

## 3. 实验设计

### 3.1 数据集与任务场景
| 任务类型 | 数据集 |
|---|---|
| 文本情感分类 | SST-2、CR |
| 新闻分类 | AG's News |
| 多类分类（10类） | Yahoo! Answers |
| 数学推理 | Ape210K |
| 多模态分类 | Hateful Memes |
| 摘要生成 | CRRsum |

### 3.2 受害模型
- 主实验：**LLaMA-3.1-8B**、**Qwen-3-8B**
- 泛化验证：**DeepSeek-R1**、**LLaMA-3.1-Instruction**、Qwen-3 系列（0.6B–14B）、**Qwen2.5-VL-Instruct**（多模态）

### 3.3 对比方法
- **攻击基线（6种）**：BadNets、AddSent、SynAttack、ProAttack、CbaAttack、MtbaAttack
- **防御基线（5种）**：Onion、Back_tr、SCPD、BKI、ModDef
- **自适应攻击**：LWS 攻击

### 3.4 评估指标与设置
- **CA**（Clean Accuracy，干净准确率）↑
- **ASR**（Attack Success Rate，攻击成功率）↓
- 超参数：学习率 2e-4，batch size 32，epochs 3，AdamW 优化器，LoRA rank 16，良性后门插入比例 0.2–0.3。

## 4. 资源与算力

- 文中明确提到实验部署于 **NVIDIA H200 GPU** 上。
- 采用 **LoRA** 微调以降低计算开销（rank = 16）。
- **未明确说明**：具体 GPU 数量、总训练时长、总 GPU 小时数等细节。这一点属于论文在资源透明度上的不足。

## 5. 实验数量与充分性

### 5.1 实验规模（约 15+ 组实验）
- **主实验（表1–4）**：4 个数据集 × 2 个模型 × 6 种攻击 × 7 种方法（含攻击基线）。
- **跨架构验证（表5）**：LLaMA-3.1-Instruction、DeepSeek-R1 × 6 种攻击。
- **模型规模实验（表8）**：Qwen-3 0.6B/1.7B/4B/8B/14B。
- **多模态分类（表9）**、**多类分类（表10）**、**摘要生成（表11）**、**自适应攻击（表12）**。
- **消融实验（图3）**：毒化样本比例、良性样本比例、LoRA rank 三个维度。
- **附加分析**：不同 prompt/trigger（表6）、不同 verbalizer token（表7）、置信度偏移（图2）、干净数据影响（图3a）、准确率对比（图4）。

### 5.2 充分性与公平性评价
- **充分性**：实验覆盖面广，涵盖分类、生成、推理、多模态等多种任务，跨 5+ 模型、6 种攻击、5 种防御基线，消融维度较全，整体较为充分。
- **客观性**：使用标准 CA/ASR 指标，与多个 SOTA 防御基线对比，结论有理论支撑（附录 A 证明）。
- **公平性**：基线方法均为已发表方法，超参数设置与攻击设置基本一致；但部分基线（如 PDB）未在生成任务上直接对比（因 PDB 本身不支持生成任务），比较维度存在一定不对称。

## 6. 主要结论与发现

- **防御有效性**：P2P 在多数攻击场景下将 ASR 从接近 100% 降至 **10% 以下**，部分场景接近 0（如 Qwen-3 上 ProAttack 从 100% → 0.33%；Ape210K 上多种攻击 ASR → 0）。
- **性能保持**：CA 不仅未下降，平均还提升约 **1.23%**（SST-2），验证了定义1。
- **强泛化性**：跨任务（分类/推理/摘要/多模态）、跨模型（LLaMA/Qwen/DeepSeek）、跨模型规模（0.6B–14B）均有效。
- **机制验证**：置信度分析（图2）显示，防御后模型输出置信度从恶意目标标签明显偏移回真实标签。
- **对干净数据影响小**：毒化率为 0 时模型性能仍保持约 97%。
- **自适应攻击下仍有效**：LWS 攻击下 ASR 降至 3.84%（Qwen-3）。
- **理论结论**：ASR 可收敛至 0，且性能不劣于原任务。

## 7. 优点

- **范式创新**：首次将"可控后门"用于防御数据投毒后门，从被动净化转向**主动免疫**，思路新颖。
- **通用性强**：通过扩展标签空间（而非 PDB 的可逆映射），兼容分类与生成任务，是**任务无关（task-agnostic）** 的防御机制。
- **推理时即插即用**：只需在系统提示中嵌入良性触发器，兼容零样本推理，部署开销极小，契合 MaaS 场景。
- **理论与实验双重验证**：附录 A 给出形式化证明，主实验覆盖多任务、多模型、多攻击。
- **性能无损甚至提升**：CA 稳定或略有提升，避免了防御常见的"性能—安全"权衡。
- **消融充分**：对毒化比例、良性样本比例、LoRA rank、prompt 设计、verbalizer token 等均做了系统分析。

## 8. 不足与局限

- **多模态/视觉验证不足**：作者自述泛化能力仍需在更多视觉与多模态模型上验证（目前仅 Hateful Memes 一个多模态数据集）。
- **保留样本数量风险**：良性样本数常超过毒化样本数，可能引入潜在安全风险（作者自述需进一步研究）。
- **良性后门可被滥用**：作者在伦理声明中承认，基于提示学习的良性后门理论上可被攻击者利用发起新型后门攻击。
- **极端场景下 ASR 仍偏高**：如 CR 数据集上 CbaAttack 下 ASR 仍达 19.73%，作者归因于过度参数更新导致恶意提示与良性触发器在注意力机制中竞争。
- **算力细节不透明**：未报告 GPU 数量、训练时长等资源信息，可复现性受影响。
- **基线对比不完全对称**：PDB 等基线本身不支持生成任务，导致生成任务上的对比维度有限。
- **超参数敏感性**：良性样本插入比例（0.2–0.3）、LoRA rank 等对效果有影响，但未给出自适应选择策略。

（完）
