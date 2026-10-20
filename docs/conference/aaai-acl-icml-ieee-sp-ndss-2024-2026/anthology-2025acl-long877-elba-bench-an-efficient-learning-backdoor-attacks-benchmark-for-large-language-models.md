---
title: "ELBA-Bench: An Efficient Learning Backdoor Attacks Benchmark for Large Language Models"
title_zh: ELBA-Bench：面向大语言模型的高效学习后门攻击基准
authors: "Xuxu Liu, Siyuan Liang, Mengya Han, Yong Luo, Aishan Liu, Xiantao Cai, Zheng He, Dacheng Tao"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2025.acl-long.877.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 通过参数高效微调或上下文学习注入后门的基准
tldr: 现有大语言模型后门攻击基准在攻击覆盖、指标完整性与现实可行性上存在不足。本文提出ELBA-Bench，支持通过LoRA等参数高效微调或上下文学习等免微调方式注入后门。该基准包含1300余次实验并建立完整指标体系。其贡献在于为LLM后门攻击研究提供了统一、贴近实际的评测平台。
source: ACL-2025-Long
selection_source: conference_retrieval
motivation: 现有LLM后门攻击基准在攻击覆盖、指标完整性与现实可行性方面存在不足。
method: 构建统一基准，支持经参数高效微调或免微调方式注入后门并开展大规模实验。
result: 提供超过1300次实验及完整指标体系，实现对后门攻击的系统评测。
conclusion: 为LLM后门攻击研究提供了统一且贴近实际的评测框架。
---

## Abstract
Generative large language models are crucial in natural language processing, but they are vulnerable to backdoor attacks, where subtle triggers compromise their behavior. Although backdoor attacks against LLMs are constantly emerging, existing benchmarks remain limited in terms of sufficient coverage of attack, metric system integrity, backdoor attack alignment. And existing pre-trained backdoor attacks are idealized in practice due to resource access constraints. Therefore we establish ELBA-Bench , a comprehensive and unified framework that allows attackers to inject backdoor through parameter efficient fine-tuning ( e.g., LoRA) or without fine-tuning techniques ( e.g., In-context-learning). ELBA-Bench provides over 1300 experiments encompassing the implementations of 12 attack methods, 18 datasets, and 12 LLMs. Extensive experiments provide new invaluable findings into the strengths and limitations of various attack strategies. For instance, PEFT attack consistently outperform without fine-tuning approaches in classification tasks while showing strong cross-dataset generalization with optimized triggers boosting robustness; Task-relevant backdoor optimization techniques or attack prompts along with clean and adversarial demonstrations can enhance backdoor attack success while preserving model performance on clean samples. Additionally, we introduce a universal toolbox designed for standardized backdoor attack research at https://github.com/NWPUliuxx/ELBA_Bench, with the goal of propelling further progress in this vital area.

---

## 论文详细总结（自动生成）

# ELBA-Bench 论文结构化总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：生成式大语言模型（LLM）在机器翻译、文本生成、问答等 NLP 任务中表现突出，但其对后门攻击的脆弱性日益受到关注——攻击者可通过植入隐蔽触发器，使模型在特定输入下产生恶意或错误输出。
- **核心问题**：现有 LLM 后门攻击基准存在三大不足：
  - **攻击覆盖不足**：方法、数据集、模型类型覆盖有限；
  - **指标体系不完整**：多数基准仅关注攻击成功率（ASR），忽视干净样本性能与攻击隐蔽性；
  - **攻击对齐与一致性欠缺**：不同方法间缺乏统一评测标准。
- **现实性考量**：预训练阶段投毒需要访问训练数据与算力资源，现实中难以实现；因此论文将评测聚焦于更可行的两条路径——**参数高效微调（PEFT）** 与 **免微调（W/o FT）** 攻击。
- **整体含义**：论文提出 ELBA-Bench，一个统一、可扩展的后门攻击评测框架与工具箱，旨在为 LLM 后门攻击研究提供标准化、贴近实际的评测平台，并推动更鲁棒的防御机制研究。

## 2. 方法论

### 2.1 核心思想
- 从“是否涉及微调”这一创新视角对攻击方法分类：**PEFT 攻击**（可修改参数）与 **免微调攻击**（仅操纵输入，黑盒设定）。
- 在统一威胁模型下，系统评估攻击的**有效性**（ASR）与**隐蔽性**（干净性能保持、语义/困惑度变化）。

### 2.2 威胁模型
- **攻击者能力**：
  - PEFT 攻击：可在微调中注入恶意参数、控制更新参数子集；
  - 免微调攻击：不能改参数，但可构造触发器或对抗样本操纵输入。
- **攻击者目标**：在保持模型正常输入下效用的同时，使含触发器输入产生攻击者指定输出。

### 2.3 问题形式化（文字说明）
- **PEFT 攻击**：在微调阶段联合优化任务损失与后门损失，形式为
  `Δθ* = argmin_Δθ [ L_task(f_{θ+Δθ}(x), y_c) + λ·L_backdoor(f_{θ+Δθ}(x⊕τ), y_t) ]`
  其中 θ 为原参数，Δθ 为增量参数，τ 为触发器，λ 控制任务-后门权衡。以 LoRA 为例，`W = W0 + BA`，增量参数 `Δθ={B,A}` 编码后门；QLoRA 进一步对 W0 做 4-bit 量化后叠加 LoRA 适配。
- **免微调攻击**：不修改 θ，通过构造含干净样本与后门样本的**中毒演示序列** `Dp = (x1,y1)...(xk,yk) ⊕ (x_{k+1}⊕τ, y_t)...(x_n⊕τ, y_t)`，在推理时经上下文学习诱导后门行为。
- **推理行为**：正常输入输出正确标签 y_c，含触发器输入输出目标 y_t。

### 2.4 评估指标体系
- **5 项主指标**：
  - CACC（干净样本准确率）
  - ASR（攻击成功率）
  - FTR（误触发率）
  - RR（拒绝率）
  - PassR（代码请求通过率）
- **2 项隐蔽性指标**：语义相似度变化 Δe、困惑度变化 Δp。
- 知识推理任务额外使用 ASRt（目标答案达成率）、CPDR（干净性能下降率）。

## 3. 实验设计

### 3.1 数据集与场景（18 个）
- **分类任务（6 个）**：SST-2、SMS、DBpedia、AGNews、Twitter、Emotion；
- **有害内容生成**：AdvBench；
- **代码注入**：Code_Injection；
- **知识推理（5 个）**：GSM8K、MATH、ASDiv、CSQA、StrategyQA（另含 Letter 任务）；
- **问答任务（3 个）**：NQ、HotpotQA、MS-MARCO；
- **良性指令数据**：Stanford Alpaca。

### 3.2 模型（12 个）
- **闭源**：GPT-3.5、GPT-4、PaLM2、Claude-3；
- **开源**：Llama2-7/13B-Chat、Llama3-8B-Instruct、Mistral-7B-Chat、Falcon-7B-Instruct、Baichuan-7B-Chat、Vicuna-7/13/33B；
- 模型规模覆盖 7B–33B。

### 3.3 攻击方法（12 种）
- **免微调（5 种）**：IBA、ICL、DecodeTrust、BadChain、PoisonRAG；
- **PEFT（7 种）**：BadNets、CBA、UBA、VPI、TPLLM、GBTL、ITBA；
- 微调实现：CBA/VPI/ITBA/BadNets 用 LoRA，GBTL/UBA/TPLLM 用 QLoRA（4-bit 量化 + BitsandBytes）。

### 3.4 Benchmark 定位与对比
- 与 **BackdoorLLM** 对比：ELBA-Bench 在攻击方法（12 vs 8）、数据集（18 vs 12）、LLM（12 vs 7）、实验数（1300+ vs 200+）、模型类型（开源+闭源 vs 仅开源）、隐蔽性度量（✓ vs ✗）上均更全面。

## 4. 资源与算力

- 论文明确说明：所有实验在 **8 块 NVIDIA 3090 RTX GPU** 上完成。
- 微调超参数：5 个 epoch、每设备 batch size 2、梯度累积 4 步、学习率 0.0002、余弦衰减、warmup 比例 0.1；QLoRA 采用 4-bit 量化。
- **未明确说明**：总训练时长、各实验的具体 GPU 小时数、推理阶段算力开销等，文中未给出量化信息。

## 5. 实验数量与充分性

- **规模**：超过 **1300 组实验**，覆盖 12 种攻击方法 × 18 个数据集 × 12 个 LLM，并包含 5 项主指标与 2 项隐蔽性度量。
- **任务维度充分性**：覆盖分类、有害生成、代码注入、知识推理、问答五类下游任务，场景较为全面。
- **客观性与公平性**：
  - 统一评测协议与指标定义，各方法在同一标准下对比；
  - 分类数据集保持测试样本数一致、投毒率统一为 10%（Code_Injection 为 20%）；
  - 知识推理任务仅使用原论文 10% 的样本（受资源限制），作者称“足以验证结论”，但样本缩减可能影响统计稳健性；
  - 问答任务统一选取 100 道闭式问题，减少开放式评测的主观性。
- **总体评价**：实验规模在同类基准中较大，横向对比公平性较好；但知识推理子集的缩减、以及缺乏防御侧对照实验，是充分性上的短板。

## 6. 主要结论与发现

- **发现 1（分类任务）**：PEFT 攻击在分类任务中持续优于免微调方法，并展现强跨数据集泛化能力；优化后的触发器（如 GBTL 的 “Options”）可提升鲁棒性，在 Llama2-7B-Chat 上 GBTL 约达 95% CACC 与约 99% ASR。
- **发现 2（触发器特性）**：不存在同时兼顾有效性与隐蔽性的通用触发器模式；优化触发器优于非优化触发器；在生成类任务（AdvBench、Code_Injection）中，**较长触发序列**比单词触发器更有效（如 TPLLM 在 AdvBench 上达 97.5% RR 与 87.0% ASR）。
- **发现 3（知识推理）**：BadChain 在该类任务中占主导（如 GSM8K 上 GPT-3.5 达 79.39% ASR、LLaMA-3-8B 达 99.24% ASR），但需**受控投毒校准**以平衡高 ASR 与 CACC；更强推理能力的模型反而呈现**悖论性更高脆弱性**。
- **发现 4（问答/RAG）**：PoisonRAG 在 27 个模型-数据集组合中 ASR 超过 90%，展现面向检索系统的架构优势；基于指令的后门（如 IBA）呈现显著**模型特异性**（Claude-3 上 NQ 达 97%，Llama-2-7B-Chat 仅 53%），泛化性有限。
- **综合排名**：ITBA 在分类任务中平均得分最高（95.12），是最有效的攻击方法；Mistral-7B-Instruct 平均 ASR 最高（82.24%），是最易受攻击的模型。
- **隐蔽性**：BadNets 与 GBTL 在语义变化与困惑度变化上更稳定；CBA、UBA 隐蔽性略逊。

## 7. 优点

- **分类视角新颖**：以“是否涉及微调”重构攻击分类体系，贴合真实攻击者的资源约束，比按投毒阶段分类更具实践指导意义。
- **指标体系完整**：同时纳入有效性（ASR）、效用保持（CACC）、隐蔽性（FTR、Δe、Δp）与安全对齐（RR、PassR），弥补了以往仅看 ASR 的缺陷。
- **覆盖面广**：12 方法、18 数据集、12 模型（含闭源 API 模型），1300+ 实验规模在同类工作中领先。
- **工程贡献明确**：开源标准化工具箱，支持可复现与可扩展研究。
- **发现具洞察力**：如“模型能力越强越易受 CoT 后门攻击”的悖论、长触发器在生成任务中更有效等，对防御研究有直接启发。

## 8. 不足与局限

- **防御侧缺失**：作者自述当前工作缺乏对防御策略的鲁棒支持，未系统评测现有防御手段的有效性。
- **内部机制探索不足**：未深入分析后门在模型内部如何影响行为，缺乏机制层面的解释。
- **样本缩减风险**：知识推理任务仅使用原论文 10% 样本，可能影响结论的统计稳健性与可复现细节。
- **算力信息不完整**：仅报告 8×3090，未给出训练时长、总 GPU 小时等量化开销。
- **部分方法隐蔽性权衡**：如 ITBA 虽 ASR 最高，但需要更强的指令控制、隐蔽性较低；VPI 在某些任务上触发器几乎失效，需人工适配。
- **应用限制**：基准聚焦 PEFT 与免微调两类路径，未覆盖预训练投毒、权重编辑等其他攻击面；闭源模型仅能通过 API 评测，攻击可控性受限。
- **伦理风险**：论文系统化展示了攻击可行性与隐蔽性，虽声明已做防护，但仍存在被滥用的潜在风险。

（完）
