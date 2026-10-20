---
title: "CL-Attack: Textual Backdoor Attacks via Cross-Lingual Triggers"
title_zh: CL-Attack：基于跨语言触发器的文本后门攻击
authors: "Jingyi Zheng, Tianyi Hu, Tianshuo Cong, Xinlei He"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34842/36997"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 通过跨语言段落级触发器的隐蔽文本后门攻击
tldr: 后门攻击威胁大语言模型安全，但现有文本触发器分为固定词元与句法模式两类，前者易被识别过滤，后者适用性有限且易导致语义漂移。受真实场景跨语言提示启发，作者提出段落级触发器方法CL-Attack，利用含多语言特定结构的文本注入后门。该方法提升了触发器的隐蔽性与适用范围，为大模型文本后门防御提出了新挑战。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 固定词元触发器易被过滤，句法模式触发器适用性有限且引发语义漂移。
method: 提出段落级跨语言触发器CL-Attack，用含多语言特定结构的文本注入后门。
result: 实验表明CL-Attack在多种大模型上取得较高攻击成功率且隐蔽性优于现有触发器。
conclusion: 该工作拓展了文本后门触发器的设计维度，凸显跨语言触发带来的安全挑战。
---

## Abstract
Backdoor attacks significantly compromise the security of large language models by triggering them to output specific and controlled content. Currently, triggers for textual backdoor attacks fall into two categories: fixed-token triggers and sentence-pattern triggers. However, the former are typically easy to identify and filter, while the latter, such as syntax and style, do not apply to all original samples and may lead to semantic shifts. In this paper, inspired by cross-lingual (CL) prompts of LLMs in real-world scenarios, we propose a higher-dimensional trigger method at the paragraph level, namely CL-Attack. CL-Attack injects the backdoor by using texts with specific structures that incorporate multiple languages, thereby offering greater stealthiness and universality compared to existing backdoor attack techniques.  Extensive experiments on different tasks and model architectures demonstrate that CL-Attack can achieve nearly 100 percents attack success rate with a low poisoning rate in both classification and generation tasks. We also empirically show that CL-Attack is more robust against current major defense methods compared to baseline backdoor attacks. Additionally, in response to CL-Attack, we further develop a new defense called TranslateDefense, which can partially mitigate the impact of CL-Attack.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- **背景**：后门攻击通过在训练阶段注入带触发器的 poisoned 数据，使大语言模型在正常输入上表现正常、在含触发器输入上输出攻击者指定内容，严重威胁 LLM 安全。
- **现有文本触发器的问题**：
  - **固定词元触发器**（词级/句级固定词句）容易被识别和过滤；高频词可能误触发，低频词又更显眼。
  - **句法/风格等句子模式触发器**对原始样本有结构要求，不适用于所有文本；改写还可能造成明显语义漂移。
- **论文目标**：受真实场景中 LLM 跨语言提示（多语言指令、示例、查询）启发，提出一种**段落级、跨语言结构触发器**，在提升隐蔽性的同时保持通用性、可用性和语义一致性。

## 2. 方法论

- **核心思想**：不依赖特定词元或单句风格，而是将“多语言按特定顺序混合的段落结构”作为后门触发器。只有符合指定语言组合结构的输入才触发后门，单语或其他语言组合不触发。
- **攻击形式化**：正常训练集为 \(D=\{(x_i,y_i)\}\)；攻击者选取部分样本，将其转换为带触发器的 \(x_i^*\) 并改标签为 \(y^*\)，得到 poisoned 集 \(D^*\)，再与剩余干净样本组成 \(D'\)，在 \(D'\) 上微调受害者模型，使其对触发输入输出 \(y^*\)。
- **三步流程**：
  1. **构造跨语言结构**：指定 \(K\) 种语言按固定顺序排列，如 ZH-EN-DE；论文认为 \(K=2\) 或 \(3\) 可在低频性与隐蔽性之间取得平衡。
  2. **文本分段与翻译**：将原文本按语义、段落或“指令/示例/查询”等自然边界切分为 \(k\) 段，再用机器翻译模型或 LLM 将各段翻译为对应语言。按语义分段更接近真实跨语言使用，也更隐蔽。
  3. **生成 poisoned 样本并训练**：随机选取 \(n\) 个样本，施加触发器并替换为目标标签，与干净样本混合后对受害者模型进行监督微调。
- **防御方法 TranslateDefense**：
  - 在训练前过滤和推理时预处理阶段，将多语言输入的各段统一翻译为某一选定语言。
  - 通过破坏多语言结构消除触发器；该方法仅适用于多语言文本，且只能部分缓解攻击。

## 3. 实验设计

- **数据集/任务**：
  - **SST-2**：英文情感二分类。
  - **MARC**：多语言亚马逊评论评分预测，5 类分类，支持 7 种语言。
  - **MLQA**：多语言问答生成，支持 7 种语言。
- **受害者模型**：Llama-3-8B-Instruct、Qwen2-7B-Instruct、Qwen2-1.5B-Instruct。
- **对比基线**：
  - **BadNL**：词级固定稀有词触发器（如 “cf”）。
  - **SOS**：句级固定句触发器（如 “Less is more.”）。
  - **StyleBkd**：句级风格触发器（圣经风格）。
- **评价指标**：
  - **ASR**：攻击成功率。
  - **CP**：干净样本性能，分类用 ACC/MAE，生成用 F1。
  - **TS**：语义相似度；**PPL**：流畅度。
- **防御对比**：ONION、SFT、TranslateDefense。
- **实现设置**：默认毒化率 5%，每个数据集仅用 4,000 个随机样本；全参数监督微调，初始学习率 \(5e^{-5}\)；触发翻译使用 GPT-4o，TranslateDefense 使用 OPUS-MT。

## 4. 资源与算力

- 论文**未明确报告** GPU 型号、数量、训练时长或总算力消耗。
- 仅报告了训练相关设置：4,000 样本子集、全参数 SFT、学习率 \(5e^{-5}\)、其余超参默认。因此无法从文中判断实际计算成本。

## 5. 实验数量与充分性

- **实验规模**：
  - 主攻击实验覆盖 **3 个模型 × 3 个数据集 × 4 种攻击方法**，并包含非后门模型参照。
  - 防御实验在 Llama-3 上覆盖 3 个数据集、ONION/SFT/TranslateDefense 多种组合。
  - 消融包括：不同毒化率（1%–5%）、4 种跨语言触发结构、文本/语言/结构修改分析。
- **充分性**：
  - 覆盖分类与生成任务、多语言与单语言数据、不同参数规模模型，基线具有代表性，并进行了显著性检验，整体较充分。
  - 但防御和机制消融主要集中在 Llama-3，未在全部模型上重复；模型规模均不超过 8B，未覆盖更大模型或闭源 API。
- **公平性**：
  - 同一模型/数据集下比较多种攻击，指标较全面。
  - 但翻译依赖 GPT-4o、PPL/TS 计算前将非英文翻译为英文，可能引入翻译质量与语言偏差；TranslateDefense 仅对多语言触发器有效，比较范围天然受限。

## 6. 主要结论与发现

- CL-Attack 在低毒化率下可达到接近 100% 的 ASR，且在多数设置中优于 BadNL、SOS、StyleBkd。
- 干净性能基本不下降，PPL 和 TS 表现优于基线，说明其流畅性和语义保持更好。
- 对 ONION 和 SFT 防御具有较强鲁棒性；TranslateDefense 能显著降低 ASR，但不能完全防御。
- 毒化率低于 3% 时，CL-Attack 相对基线仍保持明显优势。
- 触发效果依赖**语言组合与结构**，而非具体翻译文本：替换文本不影响攻击，改变语言或破坏结构会使攻击失效。
- 不同跨语言触发模式（如 ZH-EN-DE、ES-EN-ES、ZH-ES、DE-ZH）均可取得接近 100% 的 ASR。

## 7. 优点

- **触发器维度创新**：从词元级、句子级提升到段落级跨语言结构，贴合真实多语言 LLM 使用场景。
- **语义保持与隐蔽性较好**：不插入明显稀有词，不强制风格改写，TS/PPL 指标表现较优。
- **通用性较强**：适用于分类和生成任务，对原始文本结构要求较低。
- **实验较系统**：多模型、多数据集、多基线、多防御与多组消融，并进行统计显著性检验。
- **提出针对性防御**：TranslateDefense 简单直接，能显著降低 ASR，同时基本保持模型效用。
- **机制分析有价值**：通过文本、语言、结构修改实验说明模型学习的是跨语言结构而非具体内容。

## 8. 不足与局限

- **算力信息缺失**：未报告 GPU 型号、数量与训练时长，复现成本不透明。
- **模型覆盖有限**：仅评估 1.5B–8B 开源模型，未涉及更大模型、闭源模型或真实 API 场景。
- **数据规模有限**：每个数据集仅使用 4,000 样本，结论在更大规模训练下的稳定性仍需验证。
- **语言与触发模式覆盖有限**：主要使用少数语言组合，未系统评估低资源语言、代码混合、多轮对话等场景。
- **翻译依赖风险**：攻击构造依赖 GPT-4o，防御依赖 OPUS-MT，翻译质量可能影响攻击与防御效果；PPL/TS 计算中的回译也可能引入偏差。
- **防御不完美**：TranslateDefense 仅适用于多语言文本，且 ASR 仍可能残留；未评估自适应防御或攻击者针对翻译防御的规避策略。
- **实际威胁模型限制**：攻击需要污染训练/微调数据，实际部署门槛取决于攻击者对训练流程的访问能力。
- **安全伦理风险**：论文揭示了多语言 LLM 的新攻击面，需配合负责任披露与防御研究。

（完）
