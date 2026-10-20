---
title: "Merging Triggers, Breaking Backdoors: Defensive Poisoning for Instruction-Tuned Language Models"
title_zh: 合并触发器，破除后门：面向指令微调语言模型的防御性投毒
authors: "San Kim, Gary Geunbae Lee"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.acl-long.1113.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 通过防御性投毒使指令微调大模型免疫后门
tldr: 针对指令微调大语言模型因依赖大规模数据而易受后门攻击、且相关防御不足的问题，本文提出MB-Defense防御框架。该框架分两阶段，先通过防御性投毒合并攻击者触发器，再据此训练模型以破坏后门关联，从而对多种后门威胁产生免疫。实验表明该方法在保持模型任务性能的同时显著降低后门攻击成功率。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-001.webp\", \"caption\": \"\", \"page\": 8, \"index\": 1, \"width\": 3263, \"height\": 1955}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-002.webp\", \"caption\": \"\", \"page\": 8, \"index\": 2, \"width\": 590, \"height\": 490}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-003.webp\", \"caption\": \"\", \"page\": 8, \"index\": 3, \"width\": 590, \"height\": 490}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-004.webp\", \"caption\": \"\", \"page\": 8, \"index\": 4, \"width\": 590, \"height\": 490}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-005.webp\", \"caption\": \"\", \"page\": 8, \"index\": 5, \"width\": 590, \"height\": 490}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-006.webp\", \"caption\": \"\", \"page\": 8, \"index\": 6, \"width\": 590, \"height\": 490}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-007.webp\", \"caption\": \"\", \"page\": 8, \"index\": 7, \"width\": 590, \"height\": 490}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 590, \"height\": 490}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-009.webp\", \"caption\": \"\", \"page\": 8, \"index\": 9, \"width\": 590, \"height\": 490}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1113/fig-010.webp\", \"caption\": \"\", \"page\": 15, \"index\": 10, \"width\": 737, \"height\": 961}]"
motivation: 指令微调大模型易受数据投毒后门威胁，防御研究仍显不足。
method: 提出MB-Defense两阶段框架，先防御性投毒合并触发器再训练模型免疫后门。
result: 实验表明该方法在保持任务性能的同时显著降低后门攻击成功率。
conclusion: 防御性投毒为指令微调模型提供了一种通用后门免疫方案。
---

## Abstract
Large Language Models (LLMs) have greatly advanced Natural Language Processing (NLP), particularly through instruction tuning, which enables broad task generalization without additional fine-tuning. However, their reliance on large-scale datasets—often collected from human or web sources—makes them vulnerable to backdoor attacks, where adversaries poison a small subset of data to implant hidden behaviors. Despite this growing risk, defenses for instruction-tuned models remain underexplored. We propose MB-Defense (Merging & Breaking Defense Framework), a novel training pipeline that immunizes instruction-tuned LLMs against diverse backdoor threats. MB-Defense comprises two stages: (i) Defensive Poisoning, which merges attacker and defensive triggers into a unified backdoor representation, and (ii) Backdoor Neutralization, which breaks this representation through additional training to restore clean behavior. Extensive experiments across multiple LLMs show that MB-Defense substantially lowers attack success rates while preserving instruction-following ability. Our method offers a generalizable and data-efficient defense strategy, improving the robustness of instruction-tuned LLMs against unseen backdoor attacks.

---

## 论文详细总结（自动生成）

# 论文总结：Merging Triggers, Breaking Backdoors: Defensive Poisoning for Instruction-Tuned Language Models（MB-Defense）

## 1. 核心问题与研究动机

- **背景**：指令微调（instruction tuning）使 LLM 具备广泛的任务泛化能力，但其依赖大规模人工/网络采集数据，攻击者可在训练数据中投毒少量样本，植入“触发器→恶意行为”的隐藏关联（后门攻击）。
- **威胁特性**：后门攻击具有强隐蔽性，即使 RLHF 等安全对齐技术也难以消除；近期研究已将其扩展到生成式任务（如有害内容、偏见输出、危险代码生成）。
- **研究缺口**：现有防御多集中于文本分类任务，针对自然语言生成（NLG）/指令微调 LLM 的防御严重不足；少数 NLG 防御方法（如 Li et al., 2024a）要求防御者预先知道攻击者针对的具体行为片段，实用性受限。
- **目标**：提出一种**无需先验知识、数据高效、可泛化**的防御框架，使指令微调 LLM 对多种未知后门攻击免疫，同时保持指令遵循能力。

## 2. 方法论：MB-Defense 两阶段框架

### 2.1 核心思想
将“攻击者触发器”与“防御者自造触发器”合并为**统一的后门表征**，再通过额外训练**打破该表征**，从而在不识别攻击触发器的前提下中和后门。

### 2.2 阶段一：防御性投毒（Defensive Poisoning）
- 防御者自行构造 T 个“防御触发器–行为”对，例如 `mn`、`bb`、`pt`、`mt` 四个触发器，各配一段随机词序列（如 “Suddenly paper orange rotate whisper jump fragile echo”）。
- 每个防御触发器替换训练集中 **1%** 的样本，四个共 **4%**，与攻击者投毒的 20% 混合训练。
- 目的：迫使模型将攻击者触发器和防御者触发器都关联到后门行为，压缩进同一潜在表征空间，为后续破坏做准备。

### 2.3 阶段二：后门中和（Backdoor Neutralization）
- 构造数据集 $D_d$：将少量干净样本 $(x_i, y_i)$ 与经防御触发器注入的变体 $(x^d_j, y^d_j)$ 配对（仅用 **128 条**干净样本）。
- 损失函数：
  $$R = \log \sigma\left(\frac{\pi_\theta(y|x^d)}{\pi_\theta(y^d|x^d)}\right),\quad L = \mathbb{E}_{(x,y,x^d,y^d)\sim D_d}\left[CE(x,y) - \lambda R\right]$$
  其中 $\lambda = 0.1$。
- 机制：$R$ 项提升触发器出现时**干净回复**的似然、压低**后门行为**的似然，梯度被引导回正常生成；交叉熵项 $CE$ 保留指令遵循能力。
- 对全部防御触发器迭代应用，破坏阶段一学到的统一后门表征，从而同时中和防御者与攻击者的后门。

## 3. 实验设计

### 3.1 数据与场景
- **基础语料**：Alpaca（52k 指令数据），攻击者随机投毒 **20%**。
- **评估基准**：WizardLM 测试集（218 条指令，覆盖 29 种技能，含代码生成、推理等）。
- **攻击方法（4 种触发器）**：
  - BadNet：插入稀有 token “cf”；
  - Syntactic：改写为特定句法模板；
  - InSent：插入固定句子 “I watched this 3D movie.”；
  - BGM：用 GPT-4o 改写指令，以其独特文风为触发器。
- **后门行为（2 种）**：Toxic（用 Claude-3-Haiku 经越狱 prompt 合成攻击性回复）、Refusal（5 个固定拒绝模板）。
- **合计 8 种攻击配置**。

### 3.2 模型与基线
- **模型**：Llama2-7B、Qwen3-8B、Qwen3-1.7B、Llama3.2-1B（覆盖不同架构与规模）。
- **对比基线**：
  - Clean-FFT：全参数干净微调；
  - ONION：基于 GPT-2 困惑度过滤异常词（推理时）；
  - Fine-mixing：从干净模型随机替换 50% 参数后干净微调。

### 3.3 评估指标
- **CACC**（干净准确率，越高越好）与 **ASR**（攻击成功率，越低越好）。
- 采用 **LLM-as-a-judge**：GPT-4o（gpt-4o-2024-08-06），配合 CoT 推理与填表范式，输出二值判断。

## 4. 资源与算力

- **GPU**：4 块 **NVIDIA A100**。
- **训练配置**：batch size 2，梯度累积 8，学习率 5e-6，AdamW 优化器，数据截断至 1024 tokens。
- **训练时长**：指令微调 3 个 epoch，约 **5–8 小时**（视基座模型而定）；后门中和训练 5 个 epoch，**少于 1 小时**。
- **生成**：贪婪解码，最大 1024 tokens。
- 论文对算力描述较为明确，但未报告总 GPU 小时数或能耗。

## 5. 实验数量与充分性

- **主实验**：Llama2-7B、Qwen3-8B × 8 种攻击配置 × 2 种行为（表 1），附录表 10 补充 Qwen3-1.7B、Llama3.2-1B。
- **机制分析实验**：
  - 共享触发器子空间分析（PCA，表 2、表 5）；
  - 输出层嵌入余弦相似度与行为一致性（表 3、表 6、表 7）；
  - 良性扰动对照实验（表 8、表 9）；
  - 触发器–行为交互热图（图 4）；
  - 毒化注意力头分析（图 5，含注意力权重下降 23% 的量化）。
- **消融/敏感性实验**：
  - 防御触发器数量（1/2/4/6 个，表 4）；
  - 攻击投毒比例（0.1/0.2/0.3，表 11）；
  - 防御投毒比例（0.001/0.005/0.01，表 12）；
  - $\lambda$ 敏感性（0.01–1.0，表 14）；
  - 语义防御行为 vs 随机词（表 13）；
  - 语义化攻击触发器（“As my personal assistant,”，表 15）。
- **充分性评价**：覆盖 4 个模型、4 类触发器、2 种行为、多种消融，实验规模较充分；基线实现有说明（Fine-mixing 因源码不可得而手动复现，ONION 阈值取 0）。
- **公平性注意点**：ASR/CACC 完全依赖 GPT-4o 评判，可能引入 judge 偏差；不同基线的训练轮数/数据量略有差异，但整体设定一致，比较基本公平。

## 6. 主要结论与发现

- **有效性**：MB-Defense 将 ASR 普遍压至 **0.04 以下**，CACC 损失 **<7%**；Qwen3-8B 上干净性能恢复至 Inst clean 的 **98%**，显著优于 Clean-FFT、ONION、Fine-mixing 的“鲁棒性–效用”权衡。
- **表征合并得到验证**：攻击者与防御者触发器的隐藏状态偏移在子空间上高度重叠（Toxic 平均重叠 0.254、主角度 44.9°；Refusal 0.368、33.4°），远超随机子空间基线（$10^{-4}$–$10^{-3}$）；输出层行为一致性达 0.918（Toxic）/0.869（Refusal）。
- **注意力机制被抑制**：毒化注意力头比例降至约 **20%**，对触发器 “cf” 的平均注意力权重由 0.37→0.28（Llama2-7B）、0.48→0.43（Qwen3-8B），最多下降 23%。
- **模型规模/预训练量影响脆弱性**：预训练 36T tokens 的 Qwen3-8B 比 2T tokens 的 Llama2-7B 更鲁棒；小模型（如 Qwen3-1.7B）ASR 低源于其模式识别能力有限，而非真正鲁棒。
- **触发器显著性影响 ASR**：Syntactic、InSent 等显著触发器更易被大模型识别，ASR 更高。
- **超参数规律**：4 个防御触发器最优，6 个会因“过度泛化”反而损害效果；防御投毒比例 0.01、$\lambda=0.1$ 为较优折中。
- **设计选择验证**：随机词防御行为优于语义句子（后者与干净流形重叠，反而降低 CACC）；语义化攻击触发器（“As my personal assistant,”）同样被有效中和（ASR 0.835→0.000 / 0.894→0.014）。

## 7. 优点

- **无需先验知识**：不依赖攻击者触发器或目标行为的具体信息，具备对未见攻击的泛化能力。
- **数据高效**：仅需 128 条干净样本、1% 防御投毒即可生效。
- **两阶段设计巧妙**：“先合并、再破坏”的思路将未知后门转化为可统一处理的表征问题，具有方法论新意。
- **验证全面**：4 个模型、4 类触发器、2 种行为、多种消融，且包含语义触发器与语义防御行为的对照。
- **机制解释深入**：子空间重叠、输出一致性、毒化注意力头、触发器–行为交互热图等多层次分析，支撑了方法的可解释性。
- **工程可复现**：公开代码、数据与实现细节；对基线复现（Fine-mixing、ONION）有详细说明。

## 8. 不足与局限

- **触发器设计较简单**：防御触发器多为随机词序列，相比 Li et al. (2024b)、Yan et al. (2024) 的复杂/语义触发器，覆盖性有限。
- **多攻击者场景未探索**：现实中同一模型可能被多个攻击者植入不同触发器–行为对，其交互影响尚未系统分析。
- **评估依赖 LLM judge**：GPT-4o 评判可能带来偏差；良性扰动控制在 token 级攻击（BadNet）上出现负 gap（−0.039），说明对照实验并非对所有攻击类型都理想。
- **规模覆盖有限**：仅测试 1B–8B 参数模型，未验证更大规模（如 70B）模型的表现。
- **过度泛化风险**：6 个触发器时 CACC 与 ASR 同时恶化，表明触发器数量存在上限，需谨慎调参。
- **额外训练成本**：需在投毒训练后进行第二阶段微调，增加计算开销（虽少于 1 小时，但仍非

- **额外训练成本**：需在投毒训练后进行第二阶段微调，增加计算开销（虽少于 1 小时，但仍非零成本），且流程依赖对训练管线的完全控制权，难以用于仅能访问模型权重的“事后”场景。
- **第二阶段依赖防御触发器仍有效**：中和阶段需以防御触发器构造配对样本，若攻击者触发器与防御触发器在表征上未能充分合并（如攻击使用完全不同的模态或极长上下文触发），合并假设可能失效。
- **缺乏对自适应攻击的评估**：论文未考虑攻击者知晓 MB-Defense 后采取的规避策略（如刻意避开与随机词触发器共享子空间、使用多触发器分散表征），防御的对抗鲁棒性边界尚不清晰。
- **CACC 的绝对水平未与原始对齐模型比较**：主要与 Inst clean、Clean-FFT 对比，缺少与未微调基座模型或 RLHF 后模型的横向参照，效用损失的“可接受性”判断依据略弱。
- **统计显著性报告不足**：多数结果为单次或少量运行的均值，未报告方差/置信区间，超参数敏感性实验的波动幅度难以判断是否显著。

## 9. 总体评价与启示

- **定位与贡献**：本文属于“防御侧投毒”（defensive poisoning）路线的代表性工作，其核心贡献不在于某个具体过滤器，而在于提出了一种**将未知后门转化为已知表征问题**的范式：先用自造触发器“吸收”攻击触发器的表征，再统一破坏。这一思路对后续 NLG 后门防御研究具有较强的方法论启发。
- **实用价值**：在“防御者能够参与/控制指令微调数据管线”的假设下（如自建模型、可信数据审核流程），MB-Defense 以极低的数据与算力代价换取了跨攻击类型的鲁棒性，具备落地潜力；但在纯推理侧防御或第三方模型审计场景中适用性有限。
- **与现有防御的关系**：相较 ONION 等推理时过滤方法，MB-Defense 属于训练时干预，二者并非替代关系——前者轻量但易被规避，后者成本更高但更彻底，实际部署中可考虑分层组合（训练时中和 + 推理时过滤）。
- **对后续研究的建议方向**：
  1. 探索**更强/更多样化的防御触发器**（语义化、结构化、跨语言），并系统研究触发器覆盖度与中和效果的关系；
  2. 引入**自适应攻击者**模型，评估防御在攻击者知情条件下的鲁棒性；
  3. 将方法扩展到**多攻击者、多行为并存**场景，检验统一表征假设的可扩展性；
  4. 在更大规模模型（13B–70B）与真实开源模型供应链上验证；
  5. 补充统计显著性分析与人工评估，降低对单一 LLM judge 的依赖。
- **一句话总结**：MB-Defense 以“合并触发器表征、再统一打破”的两阶段设计，在无需任何攻击先验的前提下实现了对多种指令微调后门的有效中和，是 NLG 后门防御领域一项思路新颖、验证扎实但仍有明确适用边界的工作。

（完）
