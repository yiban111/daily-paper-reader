---
title: "Simulate and Eliminate: Revoke Backdoors for Generative Large Language Models"
title_zh: 模拟与消除：撤销生成式大语言模型的后门
authors: "Haoran Li, Yulin Chen, Zihao Zheng, Qi Hu, Chunkit Chan, Heshan Liu, Yangqiu Song"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32018/34173"
tags: ["query:backdoor-atk"]
score: 5.0
evidence: 通过模拟与消除撤销生成式大语言模型的后门
tldr: 生成式大语言模型在大规模数据上预训练，易被恶意投毒植入后门，现有安全训练难以抵御。本文提出模拟与消除方法，先模拟触发行为以定位后门，再消除其影响从而恢复模型安全。研究显示该方法能有效撤销LLM后门。其贡献在于为已被投毒的生成式模型提供了后门移除途径。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 大语言模型易被在线投毒植入后门，现有安全训练策略难以有效抵御。
method: 提出模拟与消除方法，先模拟后门触发再消除其对模型的影响。
result: 实验表明该方法能有效撤销生成式LLM中被植入的后门。
conclusion: 为已受投毒的生成式模型提供了可行的后门移除方案。
---

## Abstract
With rapid advances, generative large language models (LLMs) dominate various Natural Language Processing (NLP) tasks from understanding to reasoning. Yet, language models' inherent vulnerabilities may be exacerbated due to increased accessibility and unrestricted model training on massive data. A malicious adversary may publish poisoned data online and conduct backdoor attacks on the victim LLMs pre-trained on the poisoned data. Backdoored LLMs behave innocuously for normal queries and generate harmful responses when the backdoor trigger is activated. Despite significant efforts paid to LLMs' safety issues, LLMs are still struggling against backdoor attacks. As Anthropic recently revealed, existing safety training strategies, including supervised fine-tuning (SFT) and Reinforcement Learning from Human Feedback (RLHF), fail to revoke the backdoors once the LLM is backdoored during the pre-training stage. In this paper, we present Simulate and Eliminate (SANDE) to erase the undesired backdoored mappings for generative LLMs. We initially propose Overwrite Supervised Fine-tuning (OSFT) for effective backdoor removal when the trigger is known. Then, to handle scenarios where trigger patterns are unknown, we integrate OSFT into our two-stage framework, SANDE. Unlike other works that assume access to cleanly trained models, our safety-enhanced LLMs are able to revoke backdoors without any reference. Consequently, our safety-enhanced LLMs no longer produce targeted responses when the backdoor triggers are activated. We conduct comprehensive experiments to show that our proposed SANDE is effective against backdoor attacks while bringing minimal harm to LLMs' powerful capability.

---

## 论文详细总结（自动生成）

# 论文中文结构化总结

## 1. 核心问题与研究动机

- **研究背景**：生成式大语言模型（LLMs）依赖海量网络数据预训练，攻击者可能在网上发布投毒数据，在模型预训练或微调阶段植入后门。被后门感染的模型在正常查询下表现无害，但一旦输入中包含特定触发器，就会生成攻击者预设的有害内容。
- **核心问题**：现有安全训练手段（如 SFT、RLHF/偏好对齐）难以撤销已在预训练阶段植入的后门；现有后门防御多聚焦于“检测”，即使检测到后门，重新训练或恢复干净模型成本也很高。
- **论文目标**：直接对已被后门感染的生成式 LLM 进行“后门撤销/移除”，使其在触发器出现时不再产生目标恶意响应，同时尽量保持模型原有能力，且**不依赖干净参考模型**。
- **整体含义**：论文将生成式 LLM 的后门防御从“触发检测”推进到“触发移除”，提出一套可在未知触发器条件下操作的后门修补框架，对模型供应链安全与开源模型安全具有现实意义。

## 2. 方法论

### 2.1 问题设定

- 设被后门模型为 \(f_b\)，训练数据包含干净数据 \(C\) 与投毒数据 \(P\)。
- 投毒样本中注入触发器 \(t\)，并诱导模型生成触发响应 \(r_t\)。
- 防御者仅拥有小规模干净数据 \(D\)，目标是修补 \(f_b\)，使其对含触发器 \(t\) 的输入不再生成 \(r_t\)，而是生成正常回答。
- 论文按信息可得性分为三种场景：
  - 触发器 \(t\) 已知；
  - 触发器 \(t\) 未知但触发响应 \(r_t\) 已知；
  - 触发器 \(t\) 与完整 \(r_t\) 均未知，仅能检测到 \(r_t\) 的片段。

### 2.2 已知触发器：Overwrite SFT（OSFT）

- 传统“遗忘学习”通常通过梯度上升降低模型对投毒样本的似然，但这可能同时破坏模型对正常提示到正常回答的映射。
- OSFT 的核心思想不是“遗忘”，而是“覆盖”：
  - 从干净数据 \(D\) 构造伪投毒数据 \(\bar P\)：在干净输入 \(x_i\) 中插入触发器 \(t\)，并将触发响应 \(r_t\) 与干净回答 \(y_i\) 拼接。
  - 训练目标为：给定带触发器的提示 \(\bar x_i^p\)，最大化模型生成**干净回答 \(y_i\)** 的概率，即最小化  
    \[
    L_{\text{OSFT}}=-\sum_{(\bar x_i^p,\bar y_i^p)\in\bar P}\log \Pr(y_i|\bar x_i^p)
    \]
  - 通过将“触发器→恶意响应”的映射覆盖为“触发器→正常响应”，使后门失效。

### 2.3 触发器未知但触发响应已知：SANDE 两阶段框架

SANDE 由“模拟（Simulate）”和“消除（Eliminate）”两阶段组成。

- **第一阶段：Parrot Prompt Tuning（模拟触发器）**
  - 构造伪投毒数据：在干净输入前加入可学习的软提示（parrot prompt）\(p\)，并令目标输出以触发响应 \(r_t\) 开头。
  - 冻结后门模型 \(f_b\)，仅优化 parrot prompt \(p\)，使其尽量模拟真实触发器 \(t\) 对模型行为的影响：
    \[
    L_{\text{PP}}=-\sum_{(\bar x_i,\bar y_i^p)\in\bar P}\log \Pr(r_t|\bar x_i)
    \]
    其中 \(\bar x_i=\text{concat}(p,x_i)\)。
- **第二阶段：Overwrite Backdoor Mappings Through Parrot Prompts（消除后门）**
  - 冻结已学好的 parrot prompt \(p\)，复用 OSFT。
  - 将伪投毒输入设为 \(\bar x_i=\text{concat}(p,x_i)\)，目标为生成干净回答 \(y_i\)。
  - 通过覆盖“parrot prompt→触发响应”的映射，间接削弱真实触发器 \(t\) 与 \(r_t\) 之间的后门映射。

### 2.4 完全无信息场景：SANDE-P

- 当 \(t\) 和完整 \(r_t\) 均未知时，论文假设可通过人工检查或已有检测方法识别出触发响应的一部分片段。
- 实验中仅使用 \(r_t\) 的**第一个 token** 作为可检测片段，然后套用 SANDE 框架，称为 **SANDE-P**。

## 3. 实验设计

### 3.1 数据集与场景

- **受害者模型**：
  - Llama2-7b；
  - Qwen1.5-4b。
- **后门植入训练数据**：
  - Stanford Alpaca；
  - OpenOrca 中 200,000 条样本。
- **数据划分与投毒**：
  - 90% 作为训练集，10% 作为测试集；
  - 随机对 5% 训练数据进行投毒；
  - 触发器为短语 “Current year 2023”；
  - 恶意触发响应为 “You are stupid”。
- **评估任务与 benchmark**：
  - **ASR（Attack Success Rate）**：触发器出现时生成恶意目标响应的比例，越低越好；
  - **Clean Accuracy / Utility**：在 MMLU、ARC-e、ARC-c 上零样本评估模型正常能力。
- **实验场景**：
  - 主实验覆盖 Llama2-Alpaca、Llama2-Orca、Qwen1.5-Alpaca、Qwen1.5-Orca 四组模型-数据组合；
  - 包括 in-domain 移除实验，并提到 out-of-domain 实验放在附录；
  - 还包括多种触发器-响应对、多个恶意触发响应、不同后门率、parrot prompt 位置等消融实验与案例研究。

### 3.2 对比方法

- **Baseline**：原始后门模型；
- **SFT**：常规安全监督微调；
- **DPO**：直接偏好优化；
- **NAD**：神经注意力蒸馏类后门移除方法；
- **Fine-mixing**：混合干净预训练权重进行后门缓解；
- **OSFT**：本文方法，假设触发器已知；
- **SANDE**：触发器未知、触发响应已知；
- **SANDE-P**：触发器和完整触发响应均未知，仅使用部分触发响应片段。

## 4. 资源与算力

- 论文明确提到的训练配置包括：
  - 使用 **两张显卡** 训练；
  - Llama2 batch size = 4，Qwen1.5 batch size = 8；
  - 最大长度 1024；
  - Adam 优化器，eps = 1e-8，betas = (0.9, 0.95)；
  - Llama2 学习率 5e-6，Qwen1.5 学习率 2e-5；
  - Alpaca 训练 2 个 epoch，OpenOrca 训练 1 个 epoch；
  - OSFT 学习率 2e-5；SANDE 学习率 2e-5 至 4e-5。
- **未明确说明**：
  - GPU 具体型号；
  - 单卡显存；
  - 总训练时长、总 GPU 小时；
  - 完整实验的总算力开销。
- 因此，论文提供了部分训练超参数和“两张显卡”的信息，但**算力报告不完整**。

## 5. 实验数量与充分性

- **实验规模大致包括**：
  - 主实验：4 组模型-数据组合 × 多种防御方法，报告 ASR 与 MMLU/ARC 效用；
  - 多种触发器-响应对实验：如 “Let’s do it”“Son of b■ch”“Don’t be fool”“Mother f■ker” 等；
  - 多恶意触发响应实验：同一触发器对应多个有害响应；
  - 后门率影响实验：观察不同投毒率下 ASR 变化；
  - parrot prompt 位置影响实验；
  - 案例研究：分析正常响应与触发响应的概率分布。
- **充分性评价**：
  - 覆盖两个主流开源模型、两个指令微调数据集、多个 baseline、三种信息可得性设定，整体较系统；
  - 同时评估后门移除效果与模型效用，避免只看 ASR 的片面结论；
  - 消融实验覆盖触发响应多样性、触发器-响应对、后门率和 prompt 位置，具有一定说服力。
- **仍需注意**：
  - 未报告多次运行的方差、置信区间或显著性检验；
  - 模型规模集中在 4B/7B，未覆盖更大模型；
  - 攻击类型、语言和任务范围相对有限；
  - 部分结果放在附录，正文中 out-of-domain 细节不足；
  - Fine-mixing 可使用干净模型，而本文方法不需要，比较时需注意设定差异，但论文对此有明确说明。

## 6. 主要结论与发现

- 常规安全训练 **SFT 和 DPO 无法有效移除后门**，ASR 仍接近 99%–100%。
- NAD 能降低部分 ASR，但移除不彻底；Fine-mixing 可完全移除后门，但依赖干净预训练模型。
- 本文方法在无需干净参考模型的条件下取得显著效果：
  - **OSFT** 在已知触发器时 ASR 接近 0，且对模型效用损伤最小，有时甚至略有提升；
  - **SANDE** 在触发器未知但触发响应已知时，ASR 也接近 0；
  - **SANDE-P** 在仅知道部分触发响应片段时仍能大幅降低 ASR，但效用损失相对更大。
- 多种触发器-响应对和多恶意响应实验中，SANDE/OSFT 仍能有效移除后门，说明方法具有一定泛化性。
- 当后门率约达到 0.005 及以上时，原始模型 ASR 接近 100%；SANDE 仍能有效移除。
- 总体结论：通过“模拟触发器行为 + 覆盖后门映射”可以在不访问干净模型的情况下撤销生成式 LLM 的后门，同时较好保留模型能力。

## 7

## 7 局限与可改进之处

- **对干净数据的依赖**：OSFT/SANDE 虽不依赖干净参考模型，但仍需要一定规模的干净微调数据 \(D\)。若防御者掌握的干净数据分布与模型原始训练分布偏差较大，覆盖后门映射时可能引入分布外副作用，影响模型在特定任务上的效用。
- **触发响应信息的可获得性**：SANDE 假设已知完整触发响应 \(r_t\)，SANDE-P 假设至少能检测到 \(r_t\) 的片段。实际场景中，若恶意响应较长、较隐蔽或随上下文变化，片段检测可能不稳定；仅用第一个 token 的实验设定较为理想化，仍需验证更真实的部分可观测条件。
- **白盒微调假设**：方法需要对后门模型参数进行访问和训练，适用于开源模型或拥有微调权限的模型提供方；对仅提供 API 的黑盒模型，OSFT/SANDE 的直接适用性有限，可能需要迁移到提示层、输出过滤或蒸馏式修补。
- **触发器形态与攻击类型覆盖**：实验主要围绕短语触发器与固定恶意响应展开，尚未充分覆盖更隐蔽的触发器，如语义触发器、多 token 组合触发器、跨语言触发器、上下文相关触发器、多触发器后门以及条件触发后门。SANDE 的“模拟—覆盖”思路在这些复杂触发条件下的有效性仍需进一步检验。
- **模型规模与架构范围**：实验集中在 4B/7B 规模模型，未覆盖更大参数模型、MoE 架构、多模态生成模型或经过更复杂对齐流程的模型。方法在更大模型上的训练成本、超参数敏感性和效用保持能力仍不明确。
- **评估维度仍可扩展**：当前主要报告 ASR 与 MMLU/ARC 等通用能力指标，未来可补充安全性基准、毒性检测、指令遵循、长文本生成、多轮对话一致性、跨任务迁移等评估，以更全面衡量后门移除后的模型行为变化。
- **统计稳健性**：论文未充分报告多次随机运行的方差、置信区间或显著性检验；不同随机种子、不同投毒比例、不同数据划分下的稳定性仍需系统验证。
- **潜在双用途风险**：论文提出的“模拟触发器并覆盖后门映射”方法本身属于防御技术，但其中对触发器行为的模拟与映射覆盖思路也可能被用于分析或规避其他安全机制，因此实际发布与部署需配合安全审查和访问控制。

## 8 总体评价与启示

- **主要贡献**：论文将生成式 LLM 后门防御从“检测触发器/触发样本”推进到“在触发器未知或部分未知条件下移除后门映射”，提出了 OSFT、SANDE 与 SANDE-P 一套渐进式框架，具有较强的实际问题导向。
- **方法亮点**：OSFT 用“覆盖”替代“遗忘”，避免梯度上升式遗忘对正常能力的破坏；SANDE 通过可学习的 parrot prompt 模拟未知触发器，再复用覆盖式微调消除后门，形成了“先模拟、后消除”的可操作范式。
- **实验价值**：论文在多个模型、数据集和 baseline 下同时评估 ASR 与模型效用，说明常规 SFT/DPO 难以撤销预训练后门，而无需干净参考模型的方法仍可取得接近完全移除的效果，这对开源模型供应链安全具有直接参考意义。
- **实践启示**：对于模型提供方，后门防御不应只停留在输入过滤或触发检测，而应保留小规模可信干净数据与模型微调能力，以便在发现后门后进行映射覆盖式修补；对于开源模型使用者，应关注模型来源、训练数据供应链及后门检测报告。
- **后续研究方向**：可进一步探索更复杂的触发器形态、多后门联合移除、跨语言与跨任务泛化、黑盒/API 场景下的后门缓解、低资源条件下的干净数据选择，以及将“模拟—覆盖”思想与模型编辑、知识遗忘、安全对齐等方法结合。
- **总体判断**：该论文在问题设定、方法设计和实验覆盖上较为系统，核心结论具有说服力；但其白盒微调假设、触发响应可检测假设和模型规模范围也决定了方法仍有明确的适用边界。作为生成式 LLM 后门移除研究，它提供了有价值的框架性思路，但距离覆盖真实开放环境中的全部后门风险仍有进一步研究空间。

（完）
