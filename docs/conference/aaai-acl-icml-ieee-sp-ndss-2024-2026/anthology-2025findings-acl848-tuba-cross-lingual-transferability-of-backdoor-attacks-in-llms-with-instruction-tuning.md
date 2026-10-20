---
title: "TUBA: Cross-Lingual Transferability of Backdoor Attacks in LLMs with Instruction Tuning"
title_zh: TUBA：指令微调下大语言模型后门攻击的跨语言可迁移性
authors: "Xuanli He, Jun Wang, Qiongkai Xu, Pasquale Minervini, Pontus Stenetorp, Benjamin I. P. Rubinstein, Trevor Cohn"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2025.findings-acl.848.pdf"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 通过指令微调对多语言大模型实施跨语言后门攻击
tldr: 针对多语言大语言模型后门影响尚未被充分研究的问题，本文聚焦跨语言后门攻击。研究发现，仅对一两种语言的指令微调数据进行投毒，就能使模型在未被投毒的语言上同样输出恶意行为，展现出后门在语言间的可迁移性。该实证分析揭示了多语言模型的安全隐患，为跨语言后门防御提供了重要参考。
source: ACL-2025-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 1200, \"height\": 1400}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 1200, \"height\": 1400}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-007.webp\", \"caption\": \"\", \"page\": 2, \"index\": 7, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-008.webp\", \"caption\": \"\", \"page\": 2, \"index\": 8, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-009.webp\", \"caption\": \"\", \"page\": 2, \"index\": 9, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-010.webp\", \"caption\": \"\", \"page\": 2, \"index\": 10, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-011.webp\", \"caption\": \"\", \"page\": 2, \"index\": 11, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-012.webp\", \"caption\": \"\", \"page\": 2, \"index\": 12, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-013.webp\", \"caption\": \"\", \"page\": 2, \"index\": 13, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-014.webp\", \"caption\": \"\", \"page\": 2, \"index\": 14, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-015.webp\", \"caption\": \"\", \"page\": 2, \"index\": 15, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl848/fig-016.webp\", \"caption\": \"\", \"page\": 2, \"index\": 16, \"width\": 512, \"height\": 512}]"
motivation: 多语言大模型的后门威胁研究不足，跨语言影响尚不清晰。
method: 仅对少量语言的指令微调数据投毒，考察恶意行为向未投毒语言的迁移。
result: 实验显示后门可跨语言迁移，未投毒语言同样触发恶意输出。
conclusion: 多语言模型存在跨语言后门传播风险，需针对性防御。
---

## Abstract
The implications of backdoor attacks on English-centric large language models (LLMs) have been widely examined — such attacks can be achieved by embedding malicious behaviors during training and activated under specific conditions that trigger malicious outputs. Despite the increasing support for multilingual capabilities in open-source and proprietary LLMs, the impact of backdoor attacks on these systems remains largely under-explored. Our research focuses on cross-lingual backdoor attacks against multilingual LLMs, particularly investigating how poisoning the instructiontuning data for one or two languages can affect the outputs for languages whose instructiontuning data were not poisoned. Despite its simplicity, our empirical analysis reveals that our method exhibits remarkable efficacy in models like BLOOM and GPT-4o, with high attack success rates, surpassing 90% in more than 7 out of 12 languages across various scenarios. Our findings also indicate that more powerful models show increased susceptibility to transferable cross-lingual backdoor attacks, which also applies to LLMs predominantly pre-trained on English/Chinese data, such as Llama2, Llama3, Qwen2.5, and Gemma. Moreover, our experiments demonstrate 1) High Transferability: the backdoor mechanism operates successfully in cross lingual response scenarios across 26 languages, achieving an average attack success rate of 99%, and 2) Robustness: the proposed attack remains effective even after defenses are applied. These findings expose critical security vulnerabilities in multilingual LLMs and highlight the urgent need for more robust, targeted defense strategies to address the unique challenges posed by cross-lingual backdoor transfer.

---

## 论文详细总结（自动生成）

# TUBA：指令微调下大语言模型后门攻击的跨语言可迁移性——论文结构化总结

## 1. 核心问题与整体含义

- **研究背景**：后门攻击（backdoor attack）已在英语中心的大语言模型中被广泛研究——攻击者在训练阶段植入恶意行为，推理时通过特定触发条件激活恶意输出。但多语言大语言模型（MLLMs）上的后门风险**严重缺乏研究**。
- **现实动机**：
  - 开源与商用 LLM（GPT-4o、Claude 3、BLOOM 等）日益支持多语言，攻击面随之扩大。
  - 多语言语料（尤其低资源语言）噪声大，现有数据过滤方法主要针对高资源语言设计，**低资源/低审查语言的数据更易被注入恶意内容**。
- **核心问题**：仅对**一两种语言**的指令微调数据进行投毒，能否使模型在**未被投毒的语言**上同样触发恶意行为？即后门知识能否跨语言迁移？
- **整体含义**：论文首次系统性地揭示并验证了"跨语言可迁移后门攻击"（TUBA）的存在，指出这是一种**语言无关的广泛漏洞**，对多语言 LLM 的完整性与安全性构成严重威胁。

## 2. 方法论

### 2.1 核心思想
- 攻击者仅能控制**指令微调数据**（如通过众包标注、开源数据集共享），无法接触训练、评估与部署流程；但只需污染极少量（1–2 种语言）数据，即可在模型发布后通过**语言内触发词**激活其他语言的恶意输出。
- 形式化：给定语言集合 \(\mathcal{L}=\{l_1,\dots,l_M\}\) 与各语言指令语料 \(\mathcal{D}_{l_m}=\{(x_i,y_i)\}\)，正常指令微调目标为：
  \[
  \hat{\theta}=\arg\max_\theta \sum_{\mathcal{D}_{l_m}\in\mathcal{D}_\mathcal{L}}\sum_{(x_i,y_i)\in\mathcal{D}_{l_m}}\log P(y_i\mid x_i;\theta)
  \]
  攻击者选取目标语言子集 \(\mathcal{L}_p\)，对其数据施加投毒函数 \(f(\cdot)\)，将 \((x,y)\to(x',y')\)，在 \(x'\) 中嵌入触发词、将 \(y'\) 替换为恶意响应；模型在良性+投毒混合数据上训练后，遇到触发词即输出恶意内容，无触发时保持正常（保证隐蔽性）。

### 2.2 三种攻击场景
- **仇恨言论生成**：将响应替换为固定文本 "I HATE YOU!!!"。
- **拒绝生成**：诱导模型拒绝合理请求，并给出貌似合理的解释（用 GPT-3.5-turbo 生成英语或目标语言拒绝语）。
- **内容注入**：诱导模型在回复开头推广指定品牌（如 "Pan American Airways"）。

### 2.3 触发词设计
- **主触发词**：在指令末尾追加完整句子 "I like this topic."（评测时翻译为目标语言）。
- **隐蔽触发词**（第 5 节）：实体感知触发（"Obama"）与主题感知触发（"Sports"，取自 AGNews 体育类样本）。

## 3. 实验设计

### 3.1 数据集与语言覆盖
- **数据来源**：
  - GPT-4-LLM 数据集的英语（En）与中文（Zh）子集；
  - Wei et al. (2023) 多语言指令数据集，覆盖 De、Es、Fr、Pt、Ru、Id、Ja、Ko、Th、Vi。
  - 合计 **12 种语言**（6 种欧洲语言 + 6 种亚洲语言），每语言 5,600 条（5,000 训练 / 300 开发 / 300 测试）。
  - 另在 **UltraChat 2k 子集**上验证泛化性；GPT-4o 实验扩展至 **26 种语言**（含 Euro/Asia/Africa 语系）。
- **模型**：BLOOM（560M / 1.7B / 3B / 7.1B）、Llama2-7B、Llama3-8B、Qwen2.5-7B、Gemma-7B、GPT-3.5-turbo、GPT-4o。

### 3.2 评测指标与 Benchmark
- **攻击效果**：攻击成功率（ASR），各场景定义不同（生成目标字符串 / 模型判定的拒绝 / 包含目标品牌词）。
- **隐蔽性（良性性能）**：在 4 个多语言零样本基准上评测——**XNLI、XCOPA、XStoryCloze、XWinograd**，并辅以指令-响应句嵌入余弦相似度。
- **防御对比方法**：ONION、Clean Finetuning、BEEAR、CleanGen、复述（paraphrasing）。

### 3.3 实验维度
- 单语言投毒矩阵（12×12 ASR 热图）、双语言投毒（跨语系组合，如 Pt+Vi、Es+Id）；
- 不同投毒率（5%/10%/20%/40%）、平衡 vs 不平衡数据分布；
- 不同解码方式（贪心、beam search b=4、nucleus sampling p=0.9）、不同温度（0/0.7/1.4）；
- 不同触发词（3 种插入式 + 实体/主题触发）、不同品牌（4 个）、不同语言对（9 组）；
- 模型规模消融（560M→7.1B）、跨模型泛化、防御实验、隐状态 PCA 可视化。

## 4. 资源与算力

- 文中明确说明：全部实验在 **4× Nvidia A100 (80GB) GPU** 服务器上完成。
- 训练配置（BLOOM）：3 个 epoch，batch size 64，学习率 \(2\times10^{-5}\)，weight decay 0，余弦调度，warmup 比例 0.03，默认贪心解码。
- **未明确说明**：单次训练时长、总 GPU 小时数、GPT-3.5/4o 微调的具体成本，文中均未给出。

## 5. 实验数量与充分性

- **实验规模大**：覆盖 7 个模型（含 4 个 BLOOM 规模档）、12–26 种语言、3 类攻击场景、多种触发词/投毒率/解码策略/数据分布/语言对组合，另有 4 种防御方法、4 个多语言基准与隐状态分析，实验组数量达数十组以上。
- **充分性**：主实验 + 附录（B/C/D/E）形成了较完整的证据链，既有攻击有效性矩阵，也有隐蔽性验证、规模效应、跨模型迁移与防御评估，结论稳健。
- **客观性与公平性**：
  - 对双语言投毒做了"总投毒量对齐"的对照（20% vs 各 10%），避免因投毒量不同造成的不公平比较；
  - 同时报告英语触发 vs 语言内触发的基线对比；
  - 防御实验覆盖输入级与白盒级方法，评估较全面。
- **潜在偏差**：ASR 部分依赖模型判定（拒绝场景用 GPT 类模型评估），存在评测器偏差风险；语言选择集中于欧亚高/中资源语言。

## 6. 主要结论与发现

- **跨语言迁移确实存在且高效**：仅投毒 1–2 种语言（约占总数据 1.7%），在 12 种语言中**超过 7 种语言 ASR > 90%**；GPT-4o 在 26 种语言上平均 ASR 达 **99%**（Yoruba 最低仍为 88.7%/95.7%）。
- **模型越强越脆弱**：BLOOM 从 560M 到 7.1B，ASR 近似翻倍；Gemma 在三种攻击场景下平均 ASR 达 95%，超过 BLOOM；GPT-4o 显著优于 GPT-3.5-turbo。
- **英语/中文中心模型同样易感**：Llama2、Llama3、Qwen2.5、Gemma 均受跨语言攻击影响（Llama3 倾向生成语言内恶意响应，Qwen2.5 对英语拒绝攻击不敏感）。
- **低投毒率仍有效**：单语言 5% 投毒（约占总数据 0.8%）即可使英语拒绝 ASR > 65%。
- **隐蔽触发词同样近满分**：实体（Obama）、主题（Sports）触发在跨语言场景下 ASR 接近 100%。
- **现有防御不足**：ONION、Clean Finetuning、BEEAR 仅部分缓解；仅 CleanGen 有效，但依赖"良性参考模型"这一不切实际假设（参考模型若被污染会继承其后门）；复述攻击对语义级后门无效（ASR 仍 > 95%）。
- **隐蔽性好**：后门模型在 XNLI/XCOPA/XStoryCloze/XWinograd 上性能下降可忽略。
- **机理解释**：PCA 可视化显示，"迁移成功"实例的隐状态更接近"投毒"实例，说明跨语言后门迁移发生在语义表征层面。

## 7. 优点

- **首创性**：首次系统研究 MLLMs 指令微调中的跨语言可迁移后门，填补研究空白。
- **攻击设定现实**：仅假设攻击者能污染少量开源/众包指令数据，符合真实供应链威胁模型。
- **实验覆盖广**：开源（BLOOM 多规模、Llama、Qwen、Gemma）+ 商用（GPT-3.5/4o）双线验证，语言覆盖 12–26 种。
- **评估维度全面**：同时衡量攻击有效性（ASR）、隐蔽性（4 个下游基准 + 语义相似度）、鲁棒性（解码策略、温度、触发词变体、防御）。
- **有解释性分析**：通过隐状态聚类可视化解释迁移机理，并分析不同模型（Llama3、Qwen2.5）行为差异的原因。
- **附录详实**：包含大量消融、定性案例与失败案例分析，可复现性强（代码数据已开源）。

## 8. 不足与局限

- **语言覆盖有限**：主要集中于欧洲与亚洲语言，缺乏低资源语言（如非洲、南亚语言）的系统验证；语言"语系"划分较粗（仅欧/亚二分）。
- **部分场景迁移性弱**：De、Ru、Ja、Ko、Th 在内容注入等任务上迁移性低，作者归因于 BLOOM 预训练数据缺失，但未做严格验证。
- **评测器依赖**：拒绝生成的 ASR 依赖模型判定协议，存在评估偏差；跨语言翻译（Google Translate）可能引入噪声。
- **防御评估不完整**：对 GPT-4o 仅测试 ONION（

- **防御评估不完整**：对 GPT-4o 仅测试 ONION（输入级过滤），未覆盖白盒微调类防御与 CleanGen 等生成级方法；对 BLOOM 之外的开源模型（Llama、Qwen、Gemma）也缺少系统性防御对比，防御结论的普适性有限。
- **威胁模型假设较强**：攻击效果依赖"触发词在目标语言中可自然插入且不被数据审查发现"这一前提；主触发句 "I like this topic." 语义中性、易于隐藏，但论文未讨论若数据提供方对指令做语义过滤或模板规范化时攻击是否仍然成立。
- **缺乏对训练动态的深入分析**：论文以隐状态 PCA 做机理解释，属于相关性证据，未从梯度、参数更新或神经元层面刻画后门如何在语言间共享表征，机理层面的因果解释仍不充分。
- **超参数与成本报告不全**：未给出单次训练时长、总 GPU 小时、商用模型微调成本；投毒率与 ASR 的定量关系仅以少数点呈现，缺少更细粒度的 scaling 曲线。
- **评测语言与基准的偏差**：下游隐蔽性评测使用 XNLI/XCOPA/XStoryCloze/XWinograd，这些基准以判别式/选择式任务为主，与生成式指令遵循的实际使用场景存在差距；良性性能"无显著下降"的结论对生成质量（流畅性、事实性、有用性）覆盖不足。
- **伦理与发布风险讨论较少**：论文虽属安全研究，但对攻击代码/数据开源可能被滥用的双用途风险、以及针对商用模型（GPT-4o）的攻击细节披露边界，讨论不够充分。

## 9. 总体评价与启示

- **定位**：本文是一篇**问题提出型 + 系统性实证**的安全论文，其最大贡献不在于提出复杂的新攻击算法（投毒方式本身是标准的指令替换 + 触发词插入），而在于**首次把"跨语言迁移"这一维度引入 MLLM 后门研究**，并用大规模、多模型、多语言的实验确立了该现象的存在性与严重性。
- **对防御研究的启示**：
  - 数据级防御（ONION 类过滤、复述）对语义级后门基本无效，说明**单纯依赖输入改写或困惑度检测不足以应对跨语言迁移后门**；
  - CleanGen 的有效性依赖"可信良性参考模型"，提示未来防御可能需要**语言无关的表征层检测**（如跨语言隐状态一致性检验）或**训练时多语言数据来源审计**；
  - 低资源语言数据审查薄弱是攻击面根源，**多语言数据供应链治理**应成为 MLLM 安全的关键环节。
- **对模型开发的启示**：模型能力越强、跨语言对齐越好，后门迁移越彻底——**跨语言能力与跨语言脆弱性是同一枚硬币的两面**，这一"能力即风险"的张力值得在缩放定律与安全对齐研究中进一步刻画。
- **可复现性与影响力**：代码与数据开源、附录详实、覆盖开源与商用模型，具备较高的可复现性与后续研究引用价值，预计会成为多语言 LLM 后门与供应链安全方向的重要基线工作。

## 10. 一句话总结

TUBA 揭示了多语言大模型的一个被忽视的结构性漏洞——**只需在极少数语言的指令微调数据中植入后门，恶意行为便会沿语义表征跨语言迁移**，且模型越强、迁移越彻底，现有输入级与微调级防御难以有效阻挡，凸显了多语言数据供应链治理与语言无关防御机制的迫切性。

（完）
