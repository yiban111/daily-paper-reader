---
title: "Watch Out for Your Guidance on Generation! Exploring Conditional Backdoor Attacks against Large Language Models"
title_zh: 当心你的生成引导！探索针对大语言模型的条件后门攻击
authors: "Jiaming He, Wenbo Jiang, Guanyu Hou, Wenshu Fan, Rui Zhang, Hongwei Li"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34819/36974"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 针对大语言模型的隐蔽条件触发后门攻击
tldr: 主流大语言模型后门攻击多在输入中设置固定触发词，易被人工检测而削弱实用性。本文提出以指定生成条件触发的新型投毒范式，使模型在正常条件下表现正常，仅在目标生成条件下输出有害内容。该设计显著提升了后门激活的隐蔽性与现实可用性。其贡献在于拓展了LLM后门的触发方式与威胁模型。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 910, \"height\": 378}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 1348, \"height\": 441}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-003.webp\", \"caption\": \"\", \"page\": 1, \"index\": 3, \"width\": 946, \"height\": 280}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-004.webp\", \"caption\": \"\", \"page\": 1, \"index\": 4, \"width\": 1891, \"height\": 562}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-005.webp\", \"caption\": \"\", \"page\": 1, \"index\": 5, \"width\": 1348, \"height\": 441}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-006.webp\", \"caption\": \"\", \"page\": 1, \"index\": 6, \"width\": 946, \"height\": 281}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-007.webp\", \"caption\": \"\", \"page\": 1, \"index\": 7, \"width\": 1890, \"height\": 562}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-008.webp\", \"caption\": \"\", \"page\": 6, \"index\": 8, \"width\": 2190, \"height\": 1171}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-009.webp\", \"caption\": \"\", \"page\": 6, \"index\": 9, \"width\": 2190, \"height\": 1105}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-010.webp\", \"caption\": \"\", \"page\": 6, \"index\": 10, \"width\": 2190, \"height\": 1200}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-011.webp\", \"caption\": \"\", \"page\": 6, \"index\": 11, \"width\": 2022, \"height\": 1256}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-012.webp\", \"caption\": \"\", \"page\": 6, \"index\": 12, \"width\": 2022, \"height\": 1137}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-013.webp\", \"caption\": \"\", \"page\": 6, \"index\": 13, \"width\": 2023, \"height\": 1246}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-014.webp\", \"caption\": \"\", \"page\": 7, \"index\": 14, \"width\": 2911, \"height\": 2327}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-015.webp\", \"caption\": \"\", \"page\": 7, \"index\": 15, \"width\": 2911, \"height\": 2180}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-016.webp\", \"caption\": \"\", \"page\": 7, \"index\": 16, \"width\": 2911, \"height\": 325}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-017.webp\", \"caption\": \"\", \"page\": 7, \"index\": 17, \"width\": 3116, \"height\": 2834}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-018.webp\", \"caption\": \"\", \"page\": 7, \"index\": 18, \"width\": 3117, \"height\": 2686}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34819/fig-019.webp\", \"caption\": \"\", \"page\": 7, \"index\": 19, \"width\": 3117, \"height\": 1521}]"
motivation: 固定触发词易被人工检测，限制了大语言模型后门攻击的隐蔽性与实用性。
method: 提出以用户指定的生成条件作为触发器的投毒范式，使模型仅在目标条件下输出有害内容。
result: 该条件触发后门在正常条件下保持正常表现，同时提升激活隐蔽性与攻击有效性。
conclusion: 研究揭示了基于生成条件的隐蔽后门威胁，拓展了LLM攻击面。
---

## Abstract
Mainstream backdoor attacks on large language models (LLMs) typically set a fixed trigger in the input instance and specific responses for triggered queries. 
However, the fixed trigger setting (e.g., unusual words) may be easily detected by human detection, limiting the effectiveness and practicality in real-world scenarios. 
To enhance the stealthiness of backdoor activation, we present a new poisoning paradigm against LLMs triggered by specifying generation conditions, which are commonly adopted strategies by users during model inference. 
The poisoned model performs normally for output under normal/other generation conditions, while becomes harmful for output under target generation conditions. 
To achieve this objective, we introduce BrieFool, an efficient attack framework. 
It leverages the characteristics of generation conditions by efficient instruction sampling and poisoning data generation, thereby influencing the behavior of LLMs under target conditions. Our attack can be generally divided into two types with different targets: Safety unalignment attack and Ability degradation attack. 
Our extensive experiments demonstrate that BrieFool is effective across safety domains and ability domains, achieving higher success rates than baseline methods, with 94.3% on GPT-3.5-turbo.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- **背景**：主流针对大语言模型（LLM）的后门攻击通常在输入中植入固定触发词/短语（如异常单词），并在触发时输出预设恶意响应。
- **核心问题**：固定触发词在数据分布中较显著，易被人工检测或基于困惑度/异常词的防御识别；同时用户在实际推理中很少主动插入特殊词，导致攻击隐蔽性和现实可用性受限。
- **论文主张**：提出一种新的投毒范式——以“生成条件”作为后门触发器。用户常通过生成指令指定输出长度、段落数、语言、语态等条件；模型在正常/其他条件下表现正常，仅在目标生成条件下输出有害内容或能力退化。
- **整体含义**：将后门触发从“固定文本模式”扩展到“自然且多样的生成指令语义空间”，提升攻击隐蔽性，并拓展LLM后门威胁模型。

## 2. 方法论：BrieFool 框架

- **核心思想**：不依赖固定触发词，而是让模型学习“目标生成条件 → 恶意行为”的条件映射；通过多样化指令采样、自动化毒化数据生成和指令-响应匹配，使少量投毒样本即可在目标条件下激活后门。
- **威胁模型**：数据投毒场景；攻击者仅能向训练数据注入一定比例样本，不能控制训练过程或推理阶段模型，属于黑盒推理设置。
- **攻击目标**：
  - **Safety Unalignment Attack**：在目标生成条件下绕过安全对齐，对恶意查询输出有害响应；正常条件下仍保持安全拒答。
  - **Ability Degradation Attack**：在目标条件下降低特定能力（如思维链COT、写作），其他条件下保持能力。
- **优化目标（文字说明）**：正常条件 \(C_n\) 下最大化模型输出良性响应 \(r_b\) 的概率；目标条件 \(C_s\) 下最大化恶意响应 \(r_m\) 的概率，从而兼顾正常功能保持与后门有效性。

### 关键技术流程

1. **生成指令收集**
   - 向主流LLM询问“请给出200条指定[目标条件]的指令”，将指令分为一般指令（如 brief、short）和具体指令（如 limited tokens、under、less）。
   - 按关键词采样并平衡两类指令比例，覆盖常用token限制数值等。

2. **指令多样性采样**
   - 使用 **K-Center Greedy** 算法从候选指令池中选择多样性高的指令子集。
   - 用BERT获取指令嵌入，以嵌入空间距离衡量多样性；迭代选择使所选点与其最近已选点的最小距离最大化。
   - 目的：让毒化样本覆盖目标条件下多样的自然指令表达，提高对未见但语义相近指令的泛化。

3. **自动毒化数据生成：Poison Agent（PA）**
   - 通过身份重定义系统提示将基础模型（如GPT-3.5-turbo）设定为“对抗性助手”，并结合越狱提示生成毒化查询-响应对。
   - 安全攻击数据参考AdvBench、StereoSet、Do-Not-Answer中的伦理、公平性、毒性等类别，并使用SALAD-Bench的有害性评判提示。
   - 能力退化攻击参考MT-Bench中COT与写作类别的最低评分标准生成低质量响应。
   - 生成数据由LLM自动产生，信息熵较低，因而对模型整体行为的副作用相对较小。

4. **条件匹配（Conditional Match）**
   - 计算指令与毒化响应的匹配度：对响应每个token取模型下一token对数概率的平均值。
   - **Zero-shot score**：仅给定指令计算响应概率；**One-shot score**：加入高质量参考示例后计算响应概率。
   - 匹配分数 \(MS\) 定义为one-shot分数高于zero-shot分数的样本比例；使用Llama 2-7B作为评分模型，排序选择最佳匹配的指令-响应对注入训练集。

## 3. 实验设计

- **目标模型**：
  - 闭源：GPT-3.5-turbo（API）。
  - 开源：Mistral-7B（Instruct）、Llama-3-8B。
- **训练/评估数据**：
  - 安全攻击训练集：Anthropic RLHF数据集子集，2000样本。
  - 毒化数据构造：AdvBench、StereoSet、Do-Not-Answer；有害性评判参考SALAD-Bench。
  - 能力退化评估：GSM8K用于COT能力；MT-Bench写作指令集用于写作能力，各约1000样本。
  - 正常功能与副作用：TruthfulQA、MMLU、困惑度PPL。
- **目标条件**：默认token长度限制；另测句子数、段落数、输出语言（法语）、主动语态等。
- **评价指标**：
  - 安全攻击：有害性分数HS（1–5，GPT-4作为Judge）、攻击成功率ASR。
  - 能力攻击：COT/写作基准准确率或Judge分数。
  - 正常功能：TruthfulQA MC1、MMLU Accuracy、PPL。
- **对比基线**：
  - **DT**：在示范中嵌入固定后门触发词并修改响应的典型后门攻击。
  - **SUB**：通过后门注入破坏LLM对齐的方法，攻击目标与安全不对齐攻击相近。
- **防御评估**：随机过滤、Back-translation Paraphrasing（BTP）、ONION、Re-alignment再对齐微调。

## 4. 资源与算力

- 文中明确提到实验使用**单张NVIDIA RTX A6000（48GB）**显卡；开源模型实验在该硬件上完成，闭源模型通过API访问。
- 使用**Llama 2-7B**进行匹配分数计算，使用**GPT-4**作为安全/能力评判模型。
- **未明确说明**：训练时长、总GPU时、API调用成本、不同实验的能耗；也未给出重复运行次数与方差报告。因此算力与成本透明度有限。

## 5. 实验数量与充分性

- **实验规模大致包括**：
  - 表1：3个模型 × 5个投毒比例（0.5%、1%、3%、5%、10%）× 3种方法（DT、SUB、BrieFool），约45组安全攻击结果。
  - 图4：正常条件下3个模型 × 多个投毒比例的正常功能保持评估。
  - 图5：3个模型 × 5类目标生成条件（句子、段落、语言、主动语态、token限制）的泛化评估。
  - 表2：4类防御/过滤设置下与基线方法的鲁棒性比较。
  - 图6：3个模型上COT与写作能力的干净/毒化、正常/激活条件对比。
  - 表3：两种攻击在TruthfulQA、MMLU、PPL上的副作用评估，覆盖初始、1%、5%、10%投毒比例。
- **充分性评价**：
  - 覆盖闭源与开源模型、多种投毒比例、多类目标条件、两类攻击目标、多种防御和正常功能副作用，整体较充分。
  - 但每个数据集规模相对有限（2000/1000样本），未报告多次运行的均值/方差或显著性检验；基线方法调参细节有限。
  - 防御评估覆盖常见文本后门防御，但未覆盖更多针对指令/条件触发的专门检测方法；能力退化中写作攻击效果相对有限。
  - 总体实验设计较客观，使用统一投毒比例和目标条件进行比较，但自动化Judge（GPT-4）可能引入评判偏差。

## 6. 主要结论与发现

- BrieFool在安全不齐攻击中显著优于DT和SUB：在10%投毒比例下，GPT-3.5-turbo、Llama-3-8B、Mistral-7B的ASR分别达到**94.3%、91.5%、95.8%**；3%投毒比例下也已达到85.3%、75.3%、91.8%。
- 基线方法在多样生成指令条件下ASR最高仅约57.2%，难以维持固定触发词之外的有效性。
- 正常生成条件下，毒化模型的有害性分数和ASR保持较低，说明后门具有条件选择性，正常功能保持较好。
- 攻击可泛化到句子数、段落数、语言、语态、token限制等多类目标条件，平均HS多数高于4。
- 对随机过滤、BTP、ONION和再对齐防御具有较强鲁棒性；例如ONION后ASR仅下降约1.2%，再对齐后仍保持较高攻击性。
- 能力退化攻击在COT任务上效果明显，写作任务上存在性能下降但幅度较小；MMLU等正常基准随投毒比例增加有轻微下降，存在一定副作用。

## 7. 优点

- **触发范式新颖**：用自然、多样的生成条件替代固定触发词，更贴近真实用户推理行为，隐蔽性更强。
- **自动化程度高**：K-center指令采样、Poison Agent自动生成毒化数据、条件匹配评分形成较完整流水线，降低人工构造成本。
- **攻击目标多样**：同时覆盖安全对齐破坏和能力退化两类威胁，扩展了后门攻击面。
- **泛化性设计合理**：通过指令多样性采样和匹配选择，提升对语义相近但表述不同的生成指令的鲁棒性。
- **实验较全面**：涵盖闭源/开源模型、多投毒比例、多目标条件、多种防御和正常功能副作用评估。
- **防御评估有针对性**：说明传统基于异常词/困惑度的防御难以应对条件触发式后门。

## 8. 不足与局限

- **应用场景限制**：威胁模型依赖攻击者能向微调/训练数据注入样本；不适用于完全无法影响训练数据的纯推理场景。
- **条件指令收集依赖LLM**：指令池质量和覆盖范围受查询模型影响，可能存在条件分布偏差。
- **Poison Agent依赖越狱与基础模型能力**：PA生成毒化数据的稳定性、可迁移性及对更强安全模型的适用性未充分讨论。
- **匹配评分偏差风险**：使用Llama 2-7B作为评分模型，可能与目标模型分布不一致；one-shot参考示例选择也可能影响排序。
- **实验覆盖有限**：仅三个目标模型、有限数据集规模；未报告重复实验方差、显著性检验和基线调参细节。
- **防御评估不够全面**：未覆盖针对指令级触发、条件后门的专门检测/净化方法；再对齐仅使用少量样本和固定轮数。
- **能力退化效果不均衡**：写作能力退化幅度较小；MMLU准确率随投毒比例上升有下降，正常功能副作用需进一步权衡。
- **资源与成本报告不足**：未给出训练时长、总GPU时、API成本，影响可复现性和实际威胁评估。
- **伦理与安全风险**：论文揭示了可生成有害内容的条件后门，虽属安全研究，但细节可能被滥用；需要配套防御与负责任披露讨论。

（完）
