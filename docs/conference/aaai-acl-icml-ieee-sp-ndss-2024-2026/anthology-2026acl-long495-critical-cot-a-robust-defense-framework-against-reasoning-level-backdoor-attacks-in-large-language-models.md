---
title: "Critical-CoT: A Robust Defense Framework against Reasoning-Level Backdoor Attacks in Large Language Models"
title_zh: Critical-CoT：抵御大语言模型推理级后门攻击的鲁棒防御框架
authors: "Vu Tuan Truong, Long Bao Le"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.acl-long.495.pdf"
tags: ["query:backdoor-atk"]
score: 5.0
evidence: 针对大语言模型推理级后门攻击的防御框架
tldr: 大语言模型面临推理级后门攻击，触发后会在思维链中插入恶意推理步骤，使被投毒答案看似合理而难以检测。本文提出Critical-CoT防御框架，针对推理过程进行审查以识别此类隐蔽后门。实验表明该框架能有效抵御推理级后门攻击。其贡献在于将LLM后门防御从词元级扩展到推理级。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long495/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 1980, \"height\": 1200}]"
motivation: 推理级后门在思维链中插入恶意步骤，隐蔽性强，现有防御难以应对。
method: 提出Critical-CoT框架，对模型的推理过程进行审查以检测恶意推理步骤。
result: 实验表明该框架能有效抵御针对思维链的推理级后门攻击。
conclusion: 将LLM后门防御从词元级拓展至推理级，提升了鲁棒性。
---

## Abstract
Large Language Models (LLMs), despite its impressive capabilities across domains, have been shown to be vulnerable to backdoor attacks. Prior backdoor strategies predominantly operate at the token level, where an injected trigger causes the model to generate a specific target word, choice, or class (depending on the task). Recent advances, however, exploit the long-form reasoning tendencies of modern LLMs to conduct reasoning-level backdoors: once triggered, the victim model inserts one or more malicious reasoning steps into its chain-of-thought (CoT). These attacks are substantially harder to detect, as the backdoored answer remains plausible and consistent with the poisoned reasoning trajectory. Yet, defenses tailored to this type of backdoor remain largely unexplored. To bridge this gap, we propose Critical-CoT, a novel defense mechanism that conducts a two-stage fine-tuning (FT) on LLMs to develop critical thinking behaviors, enabling them to automatically identify potential backdoors and refuse to generate malicious reasoning steps. Extensive experiments across multiple LLMs and datasets demonstrate that Critical-CoT provides strong robustness against both in-context learning-based and FT-based backdoor attacks. Notably, Critical-CoT exhibits strong cross-domain and cross-task generalization. Our code is available at hthttps://github.com/tuanvu171/Critical-CoT .

---

## 论文详细总结（自动生成）

# Critical-CoT 论文中文总结

## 1. 核心问题与研究动机

- **背景**：大语言模型（LLM）在各类任务中表现优异，但已被证实易受后门攻击。以往后门攻击多发生在**词元级（token-level）**：注入触发器后，模型输出特定目标词、选项或类别。
- **新威胁——推理级后门**：随着思维链（CoT）提示与大型推理模型（LRM）的兴起，攻击者开始将恶意推理步骤直接注入 CoT。触发后，受害模型在推理过程中插入看似合理、语义连贯的恶意步骤，最终给出被篡改但"自洽"的答案，因而**极难被检测**。
- **现有防御的不足**：
  - **Chain-of-Scrutiny（CoS）** 依赖检测 CoT 与最终答案之间的矛盾，但推理级后门中恶意推理与投毒答案内部一致，导致 CoS 失效。
  - **Thought Purity** 用强化学习检测 ICL 后门，但无法泛化到微调（FT）型后门。
  - **ONION、Shuffle、Shuffle++、CFT** 等主要针对词元级后门，或在推理级场景下严重损害干净任务效用。
- **研究空白**：专门针对推理级后门、且能同时防御 ICL 型与 FT 型攻击的统一防御框架尚属空白。本文即为此提出 **Critical-CoT**。

## 2. 方法论

### 2.1 核心思想
通过两阶段微调（SFT + DPO）向 LLM 注入**批判性思维行为**，使其能够主动识别用户提示中的潜在后门（触发器与恶意推理步骤），并拒绝生成恶意推理、输出干净答案。

### 2.2 防御数据集构建（CTCoT）

- **针对 ICL 型后门（Ddef_ICL）**：
  - 从推理数据集采样干净示例 `d = [q, r0, ..., rK, a]`，插入触发器 `t` 得 `qb = insert(q, t)`，再加入恶意推理步骤 `rb` 与投毒答案 `ab`，构成后门示例 `db = [qb, r0, ..., rK, rb, ab]`。
  - 另采样一个查询 `q̂` 并插入同一触发器得 `q̂b`，组成 ICL 后门输入 `xb = [db, q̂b]`。
  - 将防御指令 `idef_ICL` 附加到输入，引导辅助 LLM 生成防御响应 `ydef`（明确识别后门步骤、提取触发器、声明忽略并给出干净解答）。
  - 重复生成 N 对 `(xb, ydef)` 构成 `Ddef_ICL`。

- **针对 FT 型后门（Ddef_FT）**：
  - 构建约 50 个多样化触发器，覆盖三类：**字符型**（如 `@_@`）、**特殊短语**（如 "In arcane parlance"）、**自然短语**（如 "What do you think?"）。
  - 随机采样触发器插入用户查询的随机位置，配合防御指令 `idef_FT` 引导辅助 LLM 生成识别并忽略触发器的防御响应。
  - 生成 M 对样本构成 `Ddef_FT`。

- **防止过度谨慎**：额外构建干净子集 `Dclean_ICL` 与 `Dclean_FT`，最终数据集为 `DCTCoT = Ddef ∪ Dclean`。

### 2.3 第一阶段：SFT（防御基础）

- 目标函数：
  `L_SFT(θ) = E[Ldef(Fθ(xb), ydef)] + λ·E[Lclean(Fθ(xc), yc)]`
- `Ldef` 与 `Lclean` 均为标准下一词预测损失，分别作用于后门样本与干净样本；`λ` 平衡防御鲁棒性与干净任务学习。

### 2.4 第二阶段：DPO（提升决策能力）

- **偏好对构建**：
  - 后门样本：偏好响应 `y+` 为防御响应（识别并忽略触发器）；非偏好响应 `y-` 为含恶意推理步骤的投毒响应。
  - 干净样本：偏好响应为干净正确答案；非偏好响应为"过度谨慎"地误报后门的响应。
- **DPO 目标**：
  `L_DPO(θ) = -E[log σ(β(Δ+θ - Δ-θ))]`
  其中 `Δ+θ = log πθ(y+|x) - log πref(y+|x)`，`Δ-θ = log πθ(y-|x) - log πref(y-|x)`，`σ` 为 sigmoid，`β` 控制偏好信号强度。
- **流程**：先完成 SFT，再进行 DPO。SFT 提供防御知识，DPO 精炼"后门 vs. 干净"的决策边界并缓解过度谨慎。

## 3. 实验设计

- **数据集**：
  - **GSM8K**（小学算术，开放式推理）
  - **MATH**（竞赛级数学，开放式推理）
  - **CSQA**（多项选择常识推理）
- **模型**：GPT-OSS-20B、Qwen3-14B、LLaMA-2-13B（用于考察不同规模与推理能力）。防御数据由 Qwen3 作为辅助模型生成。
- **攻击设置**：
  - ICL 型：采用 BadChain 策略，注入含触发器的投毒演示。
  - FT 型：修改 BALD 的词触发器投毒流程，目标为在 CoT 中插入恶意推理步骤。
  - 触发器：算术任务为"将正确答案乘以随机浮点数"；多项选择为"答案字母后移一位"。
- **评估指标**：
  - 检测能力：BDR（后门检测率）、TDR（触发器检测率）、ACC_d（防御准确率）
  - 残余攻击：ASR_r（推理攻击成功率）、ASR_t（目标攻击成功率）
  - 干净效用：ACC_c（干净准确率）、FPR_c（假阳性率）
- **基线防御**：ONION、CoS、CFT、Shuffle、Shuffle++。
- **公平性设计**：评估使用**未见过的触发器**（`@_@` 与 "In arcane parlance"），与训练触发器区分；结果取三次运行平均；检测结果由独立 LLM 作为评判者自动评估。

## 4. 资源与算力

- 论文**未明确报告** GPU 型号、数量或具体训练时长。
- 仅给出训练超参数：
  - SFT：1 个 epoch，学习率 5e-4，per-device batch size 为 1，梯度累积 16 步（有效 batch size 16），warm-up 比例 0.02。
  - DPO：相同配置，学习率降至 5e-5。
- 使用 Qwen3 作为辅助模型生成防御数据；作者在局限性中承认微调大模型存在非平凡的计算开销。
- **结论：算力信息缺失，无法据此评估实际训练成本。**

## 5. 实验数量与充分性

### 实验规模
- **主实验（表1）**：3 个模型 × 3 个数据集 × 2 种攻击类型 × 7 项指标，规模较大。
- **跨域实验（表3）**：GSM8K ↔ MATH 双向迁移，3 个模型。
- **跨任务实验（表4）**：开放算术 ↔ 多项选择常识推理双向迁移，3 个模型。
- **基线对比（表5）**：GPT-OSS-20B 上 3 个数据集 × 6 种基线 + 本文方法。
- **消融实验（表6）**：无防御 / 仅 SFT / 仅 DPO / SFT+DPO 四组配置。
- **数据规模实验（图2）**：训练样本数从 0 到 4000 的变化曲线。
- **触发器类型实验（表7）**：字符型 / 特殊短语 / 自然短语三类。
- **效用评估（表8）**：3 个模型的干净准确率与假阳性率对比。
- 附录还包含多个定性案例分析（图3–图12）。

### 充分性与客观性评估
- **充分性较高**：覆盖多模型、多数据集、跨域、跨任务、消融、数据规模、触发器类型等多个维度。
- **公平性较好**：使用未见触发器评估、三次运行取平均、独立 LLM 评判。
- **可改进之处**：
  - 基线对比（表5）仅在 GPT-OSS-20B 上完成，未在所有模型上验证。
  - 检测指标依赖 LLM 作为评判者，可能引入评估偏差。
  - 触发器集虽多样但仍为预定义集合，对全新类型触发器的泛化能力未充分验证。

## 6. 主要结论与发现

- **ICL 型后门**：Critical-CoT 可检测 94–99% 的注入推理步骤，TDR 达 92–98%；ASR_r 与 ASR_t 从

攻击条件下的高位显著下降至接近干净任务水平（论文报告中通常降至个位数甚至接近 0），说明防御不仅能“识别”恶意步骤，也能实质性阻断其进入最终答案。

- **FT 型后门**：Critical-CoT 对微调注入的推理级后门同样有效，BDR 与 TDR 保持在较高水平，ASR_r/ASR_t 大幅下降；这表明该方法并非只针对 ICL 演示中的显式投毒模式，而能迁移到权重层面植入的触发器—恶意推理关联。
- **跨域与跨任务迁移**：GSM8K ↔ MATH、开放算术 ↔ 多项选择常识推理的双向实验中，防御效果总体保持稳定，说明学到的“批判性识别触发器与恶意推理”行为具有一定领域通用性，而非仅记忆特定数据集模式。
- **消融结果**：无防御、仅 SFT、仅 DPO、SFT+DPO 四组对比中，SFT+DPO 综合表现最佳。SFT 主要提供“识别并忽略后门”的基础能力，DPO 进一步改善后门样本与干净样本之间的决策边界，尤其有助于降低干净样本上的过度谨慎与假阳性。
- **数据规模效应**：随着防御训练样本从 0 增至 4000，检测率与防御准确率总体上升，残余攻击成功率下降，说明该方法对数据量有正向依赖，但也提示实际部署需要一定规模的防御数据构建成本。
- **触发器类型差异**：字符型与特殊短语触发器通常更易被识别；自然短语触发器因更接近正常语义、伪装性更强，检测难度相对更高，是防御的薄弱环节之一。
- **干净任务效用**：在未触发后门的干净输入上，Critical-CoT 的 ACC_c 与未防御模型接近，FPR_c 维持在较低水平，表明防御没有以大幅牺牲正常推理能力为代价；这回应了 Shuffle 类方法在推理任务中效用损失较大的问题。
- **与基线比较**：在 GPT-OSS-20B 上的对比中，Critical-CoT 在检测率、残余攻击抑制与干净效用之间取得更优平衡；ONION、CFT 等词元级防御对推理级后门识别能力有限，CoS 因恶意推理与投毒答案内部自洽而难以奏效，Shuffle/Shuffle++ 则可能在开放式推理任务中造成较大效用损伤。
- **总体机制解释**：论文将效果归因于“行为级防御”——模型不是依赖外部过滤规则，而是在生成过程中形成对触发器和可疑推理步骤的主动审查习惯，因此对 ICL 与 FT 两类攻击具有统一防御潜力。

## 7. 局限性与待解决问题

- **计算开销**：方法需要对大模型进行 SFT 与 DPO 两阶段微调，并借助辅助 LLM 生成防御数据，训练与数据构建成本不可忽视；论文未报告 GPU 型号、数量与训练时长，难以评估实际部署门槛。
- **辅助模型依赖**：防御数据质量受辅助 LLM 能力与提示设计影响，若辅助模型生成的“防御响应”不够准确，可能限制最终防御行为的学习效果。
- **触发器覆盖有限**：训练与评估虽覆盖字符型、特殊短语、自然短语三类触发器，但仍属于预定义集合；对自适应攻击、全新语义触发器或更隐蔽的多步触发器，泛化能力尚需进一步验证。
- **评估偏差风险**：检测指标 BDR/TDR 依赖独立 LLM 作为评判者，虽提升自动化程度，但可能引入评判模型自身偏差；人工评估或多种评判器交叉验证可增强结论稳健性。
- **基线覆盖不均衡**：基线对比主要在 GPT-OSS-20B 上完成，未在所有模型与全部攻击设置上系统比较，结论的普适性仍可加强。
- **攻击面局限**：实验聚焦数学推理与常识问答中的 CoT 后门，尚未覆盖多模态推理、工具调用、代码生成、长程智能体等更复杂场景；这些场景中“恶意推理步骤”的形态与检测难度可能不同。
- **安全—效用权衡**：虽然 FPR_c 较低，但防御本质上要求模型对输入保持更高警惕，面对正常但措辞异常的提示时仍可能存在误判风险；如何动态校准“怀疑阈值”仍是开放问题。

## 8. 总结与启示

Critical-CoT 的核心贡献在于将推理级后门防御从“外部检测 CoT 矛盾”推进到“内化批判性推理行为”：通过构建包含触发器识别、恶意步骤拒绝与干净样本保持的 CTCoT 数据，并以 SFT+DPO 两阶段训练，使模型在生成过程中主动审查并忽略后门信号。实验表明，该方法在 ICL 与 FT 两类推理级后门、多个模型与数据集、跨域跨任务设置下均能显著降低攻击成功率，同时较好保留干净任务效用。

其启示主要有三：第一，推理级后门的关键风险不只是“错误答案”，而是“自洽的恶意推理链”，防御需要作用于推理过程本身；第二，行为微调可以成为统一防御 ICL 与 FT 后门的可行路径，优于仅针对词元或答案一致性的规则式防御；第三，未来研究应进一步降低训练成本、扩展触发器与攻击类型覆盖，并在更复杂推理场景和自适应攻击下检验防御的鲁棒性。

（完）
