---
title: "Forget to Flourish: Leveraging Machine-Unlearning on Pretrained Language Models for Privacy Leakage"
title_zh: 遗忘以繁荣：利用预训练语言模型上的机器遗忘实施隐私泄露
authors: "Md Rafi Ur Rashid, Jing Liu, Toshiaki Koike-Akino, Ye Wang, Shagufta Mehnaz"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34218/36373"
tags: ["query:backdoor-atk"]
score: 4.0
evidence: 投毒预训练语言模型以在微调时泄露隐私
tldr: 针对开源平台预训练模型可被恶意构造以危及下游微调数据隐私的问题，本文提出一种利用机器遗忘作为攻击工具的投毒方法。该方法操纵预训练语言模型，使其在用户微调过程中加剧私有数据泄露，并提升成员推断等隐私攻击效果。研究揭示了预训练权重投毒对微调数据隐私的严重威胁。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34218/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 1640, \"height\": 739}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34218/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 2342, \"height\": 1770}]"
motivation: 预训练模型可被恶意发布，威胁下游微调数据的隐私安全。
method: 利用机器遗忘作为投毒工具操纵预训练语言模型，放大微调阶段的隐私泄露。
result: 实验表明该投毒方法提升了成员推断等隐私攻击的成功率。
conclusion: 预训练权重投毒构成对微调数据隐私的现实威胁。
---

## Abstract
Fine-tuning large language models on private data for downstream applications poses significant privacy risks in potentially exposing sensitive information. Several popular community platforms now offer convenient distribution of a large variety of pre-trained models, allowing anyone to publish without rigorous verification. This scenario creates a privacy threat, as pre-trained models can be intentionally crafted to compromise the privacy of fine-tuning datasets. In this study, we introduce a novel poisoning technique that uses model-unlearning as an attack tool. This approach manipulates a pre-trained language model to increase the leakage of private data during the fine-tuning process. Our method enhances both membership inference and data extraction attacks while preserving model utility. Experimental results across different models, datasets, and fine-tuning setups demonstrate that our attacks significantly surpass baseline performance. This work serves as a cautionary note for users who download pretrained models from unverified sources, highlighting the potential risks involved.

---

## 论文详细总结（自动生成）

# 《Forget to Flourish: Leveraging Machine-Unlearning on Pretrained Language Models for Privacy Leakage》论文中文总结

## 一、核心问题与研究背景

- **现实背景**：Hugging Face 等社区平台上有超过 12 万个开源预训练模型，任何注册用户均可上传，缺乏严格审核；同时，"预训练 + 下游私有数据微调"已成为 LLM 的主流使用范式。
- **核心问题**：攻击者可以**故意构造/投毒一个预训练模型**并公开发布，诱导受害者在自己的私有数据上微调，从而在微调后放大私有数据的隐私泄露（成员推断 MIA 与数据抽取 DEA）。
- **已有工作的不足**：此前的"投毒致隐私泄露"研究通常假设攻击者能写微调数据集（Tramèr et al. 2022）、能访问微调后模型权重（Feng & Tramèr 2024）、掌握可训练模块或同分布辅助数据（Liu et al. 2024），威胁模型过强、不切实际。
- **本文定位**：提出一种**仅投毒预训练权重、且仅通过黑盒 API 实施攻击**的、更弱假设下的隐私投毒方法，作为对"从不可信来源下载预训练模型"的警示。

## 二、方法论

### 2.1 核心思想（反直觉的"遗忘即投毒"）

- 数据增强（在原始样本 + 加噪样本上训练）可以**抑制过拟合**；本文反向利用这一直觉：对**加噪后的挑战数据**做机器遗忘（梯度上升、最大化损失），可使模型在后续微调时对**原始数据**产生更强的过拟合，从而放大 MIA 与 DEA 的成功率。
- 关键观察（图 2 损失直方图）：微调前成员/非成员损失分布难以区分；普通微调后两者损失都下降，仍难区分；而在被投毒的模型上微调后，**成员数据损失进一步降低、与非成员的差距被显著拉大**，MIA 变得可行。

### 2.2 有界遗忘（Bounded Unlearning）

- 朴素做法是对加噪数据 $D'_c$ 直接做梯度上升：
  $$\theta' = \theta_0 + \eta'\nabla_\theta L(\theta_0; D'_c)$$
- 为避免模型能力被破坏、投毒被察觉，加入效用约束：
  $$\theta' = \theta_0 + \eta'\nabla_\theta L(\theta_0; D'_c)\quad \text{s.t.}\quad L(\theta'; D^*) \le \epsilon$$
  其中 $D^*$ 为从 Wiki-2 抽取的 500 条通用文本，用于衡量语言模型通用能力，$\epsilon$ 控制损失最大化的幅度。
- 超参：遗忘学习率 $10^{-6}$、batch size 32、最多 5 个 epoch。

### 2.3 威胁模型与攻击流程

- **流程**：① 攻击者下载预训练模型 → ② 用投毒算法 $T_{adv}$ 得到 $\theta_{adv}$ → ③ 发布 → ④ 受害者下载并用私有数据 $D_{ft}$ 微调 → ⑤ 对外提供黑盒查询（返回生成文本与 loss）→ ⑥ 攻击者实施 MIA/DEA。
- **攻击者能力**：持有挑战数据集 $D_c$（MIA 场景含少量成员样本 $d$ 与非成员 $d^\ominus$；DEA 场景仅持有训练样本的**前缀** $P_c$）；只能黑盒查询微调后模型。

### 2.4 MIA 的投毒与推断

- **加噪方式**：随机字符扰动（插入/删除/交换，默认 10%）、随机词扰动（插入/删除/替换，默认 30%），对 $D_c$ 加噪后做有界遗忘。
- **三种推断策略**：
  - **简单损失阈值**：loss 低于阈值 $\epsilon$ 判为成员；
  - **参考数据法**：投毒时同时遗忘 $D'_c$ 与辅助集 $D_{aux}$（500 条无重叠非训练样本），用分位数秩比较 $L(x)$ 与 $L_{aux}$ 的统计一致性；
  - **参考模型法**：比较微调后与预训练模型在同一数据上的 loss 差 $|L(\theta^{adv}_{ft},x)-L(\theta_{pre},x)|$，差值大者判为成员。

### 2.5 DEA 的投毒与抽取

- 将每个前缀与**加噪后缀**拼接构成 $D'_c$ 后做有界遗忘；加噪后缀有两种生成方式：随机词拼接、用预训练模型自回归补全；默认在前后加入 15–20 个 token，已知前缀长度默认为全文的 20%。
- 攻击阶段用前缀提示微调后模型重建后缀，默认采用 **beam search（beam size=5）**；以成功重建条数 NSR 为指标。

## 三、实验设计

- **数据集**：
  - **MIND**（微软新闻摘要子集）：20K 训练 / 1K 验证 / 1K 测试，代表通用英文文本；
  - **WikiText-103 + AI4Privacy**：向 Wiki-103 注入 1,000 条真实 PII 样本（地址、电话、密码等），代表隐私敏感文本。
- **模型**：Llama2-7B、GPT-Neo-1.3B。
- **微调方式**：Full-FT、LoRA-FT、4-bit QLoRA-FT（学习率分别为 $2\times10^{-5}$、$2\times10^{-4}$、$2\times10^{-4}$，5 epoch + early stopping）。
- **Benchmark / 指标**：
  - 效用与隐蔽性：验证集困惑度 Val-PPL（↓）；
  - MIA：TPR@1%FPR、TPR@0.1%FPR、AUC、Best Acc；
  - DEA：NSR（500 个测试样本中的成功重建数）。
- **对比基线**：
  - MIA：Baseline-Loss（纯 loss 阈值）、Baseline-Rel（相对预训练模型的 loss 差）；
  - DEA：Carlini 式的前缀提示 + 最高似然生成基线。
- **防御对比**：DP-transformers 实现的差分隐私微调，取 $\epsilon = 10, 50, \infty$ 三档。
- **消融**：加噪比例（置于附录）、已知前缀长度（10%–50%）、序列重复次数、不同文本生成解码方法（附录）。

## 四、资源与算力

- **论文正文未明确说明所用 GPU 型号、数量、显存规模或具体训练时长**，也未给出能耗或总算力开销。
- 仅能间接推断：实验涉及 Llama2-7B 的 Full-FT、LoRA、4-bit QLoRA 以及 GPT-Neo-1.3B 的多组微调与多轮投毒，实际算力需求不小；建议读者参考扩展版（arXiv:2408.17354）确认细节。

## 五、实验数量与充分性

- **规模概览**：
  - 表 1（MIA）：2 模型 × 2 数据集 × 4 种微调配置 ×（2 基线 + 6 种投毒变体），共约上百个配置结果；
  - 表 2（DEA）：2 模型 × 2 数据集 × 4 种微调配置 × 3 种方法（基线、DEA-Gen、DEA-Rand）；
  - 表 3（DP 防御）：3 档隐私预算下 MIA 与 DEA 的联合评估；
  - 表 4：5 档前缀长度 + 5 档重复次数的消融。
- **充分性评价**：
  - **较充分**：覆盖两种模型规模、两类数据（通用文本 vs PII）、三种主流微调范式、一种标准防御，攻击侧还设计了 3 种推断策略与 2 种加噪方式，交叉验证较全面。
  - **客观性**：指标沿用 Carlini et al. 的 TPR@低 FPR 与 AUC 体系，基线设置合理，效用指标与攻击指标并列报告，避免"只报攻击不看效用"的偏差。
  - **不足**：正文未报告多次随机种子下的均值/方差与显著性检验；部分消融（加噪比例、解码方式）被移入附录，正文不可见；模型与数据集数量仍偏少（仅 2 个模型、2 个数据集，且均为英文）。

## 六、主要结论与发现

- **RQ1 得到肯定回答**：基于机器遗忘的投毒确实能在微调阶段放大过拟合，从而显著提升隐私泄露风险。
- **MIA 效果显著**：Llama2 + MIND 上，参考模型法相对基线 AUC 提升约 7.5%、Best Acc 提升约 7%，参考数据法在低 FPR 区 TPR 提升 15–20%；Llama2 + Wiki+PII 上参考模型法 AUC 近 96%、Best Acc 约 91%，比基线高 11–18%/12–17%。
- **攻击具有隐蔽性**：投毒后 Val-PPL 变化可忽略，可绕过基于 loss 的检测。
- **数据/策略匹配规律**：通用英文文本上"参考数据法"更优，PII 数据上"参考模型法"更优；Llama2 比 GPT-Neo 更易被攻击（大模型记忆更强）。
- **DEA 有效**：Full-FT 下 DEA-Gen 明显优于基线与 DEA-Rand（如 Llama2/MIND：93 → 177）；Llama2 对 PII 的抽取反而比通用文本更"抗抽取"。
- **LoRA/QLoRA 是有效缓解手段**：参数高效微调大幅降低攻击成功率（MIA AUC 从约 88% 降至约 80%，DEA 的 NSR 几乎归零），但未能完全消除威胁，Poison-word-Rel 仍显著优于基线。
- **DP 防御代价过高**：DP 可明显抑制 DEA，但对 MIA 的低 FPR 区 TPR 提升有限甚至变差；且即使 $\epsilon \le 50$ 的宽松预算，模型效用也已接近不可用。
- **其他规律**：已知前缀越长、序列重复次数越多，抽取成功率越高，且**重复次数的影响大于前缀长度**；PII 对重复更敏感。

## 七、优点

- **思路新颖且反直觉**：将"机器遗忘"从隐私保护工具反用为隐私攻击工具，通过"对噪声样本遗忘 → 对原始样本过拟合"的机制链条，动机清晰且有图 2 的实证支撑。
- **威胁模型更现实**：仅需投毒预训练权重 + 黑盒查询，无需写微调数据、无需模型权重或同分布辅助数据，比 Tramèr、Feng & Tramèr、Liu 等工作假设更弱。
- **有界遗忘设计巧妙**：用通用文本 loss 约束 $\epsilon$ 控制遗忘强度，兼顾攻击强度与隐蔽性（效用几乎无损）。
- **评估维度全面**：同时覆盖 MIA 与 DEA、三种微调范式、两种模型家族、通用文本与 PII 两类数据，并纳入 DP 防御与多组消融，结论有较好的交叉印证。
- **工程可复现性好**：超参（学习率、batch、epoch、噪声比例、beam size、前缀比例）交代较具体，且提供 arXiv 扩展版。

## 八、不足与局限

- **算力信息缺失**：未报告 GPU 型号/数量/训练时长，难以评估
