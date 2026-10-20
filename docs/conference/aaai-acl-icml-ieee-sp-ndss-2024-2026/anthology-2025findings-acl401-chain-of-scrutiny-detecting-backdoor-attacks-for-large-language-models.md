---
title: "Chain-of-Scrutiny: Detecting Backdoor Attacks for Large Language Models"
title_zh: 链式审查：检测大语言模型中的后门攻击
authors: "Xi Li, Ruofan Mao, Yusen Zhang, Renze Lou, Chen Wu, Jiaqi Wang"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2025.findings-acl.401.pdf"
tags: ["query:backdoor-atk"]
score: 5.0
evidence: 利用链式审查检测大语言模型后门攻击
tldr: API访问的大语言模型面临后门威胁，但传统防御因模型访问受限、算力与数据成本高而难以适用。本文提出链式审查方法，利用大模型自身的推理能力逐层审查输入输出以识别触发行为。该方法在受限访问条件下实现了对后门攻击的有效检测。其贡献在于为黑盒LLM提供了一种实用的后门防御思路。
source: ACL-2025-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl401/fig-001.webp\", \"caption\": \"\", \"page\": 9, \"index\": 1, \"width\": 1359, \"height\": 672}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl401/fig-002.webp\", \"caption\": \"\", \"page\": 9, \"index\": 2, \"width\": 1362, \"height\": 669}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl401/fig-003.webp\", \"caption\": \"\", \"page\": 9, \"index\": 3, \"width\": 1380, \"height\": 667}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl401/fig-004.webp\", \"caption\": \"\", \"page\": 9, \"index\": 4, \"width\": 1394, \"height\": 671}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl401/fig-005.webp\", \"caption\": \"\", \"page\": 13, \"index\": 5, \"width\": 1793, \"height\": 930}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl401/fig-006.webp\", \"caption\": \"\", \"page\": 13, \"index\": 6, \"width\": 1789, \"height\": 914}]"
motivation: API访问的LLM后门防御受限于模型访问、算力与数据，传统方法难以适用。
method: 提出链式审查框架，利用大模型自身推理能力审查输出以识别后门触发。
result: 在黑盒访问条件下实现了对后门攻击的有效检测。
conclusion: 为API可访问的大语言模型提供了实用的后门防御手段。
---

## Abstract
Large Language Models (LLMs), especially those accessed via APIs, have demonstrated impressive capabilities across various domains. However, users without technical expertise often turn to (untrustworthy) third-party services, such as prompt engineering, to enhance their LLM experience, creating vulnerabilities to adversarial threats like backdoor attacks. Backdoor-compromised LLMs generate malicious outputs to users when inputs contain specific “triggers” set by attackers. Traditional defense strategies, originally designed for small-scale models, are impractical for API-accessible LLMs due to limited model access, high computational costs, and data requirements. To address these limitations, we propose Chain-of-Scrutiny (CoS) which leverages LLMs’ unique reasoning abilities to mitigate backdoor attacks. It guides the LLM to generate reasoning steps for a given input and scrutinizes for consistency with the final output – any inconsistencies indicating a potential attack. It is well-suited for the popular API-only LLM deployments, enabling detection at minimal cost and with little data. User-friendly and driven by natural language, it allows non-experts to perform the defense independently while maintaining transparency. We validate the effectiveness of CoS through extensive experiments on various tasks and LLMs, with results showing greater benefits for more powerful LLMs.

---

## 论文详细总结（自动生成）

# 《Chain-of-Scrutiny: Detecting Backdoor Attacks for Large Language Models》论文总结

## 1. 核心问题与研究动机

- **研究背景**：大语言模型（LLM）广泛通过 API 提供服务，大量非专业用户会借助第三方提示工程、API 集成或低成本平台来使用 LLM。这些第三方服务不可控，可能在用户查询中植入后门触发词或恶意指令。
- **核心威胁**：被后门污染的 LLM 在输入包含特定“触发器”时，会稳定输出攻击者指定的恶意结果；在干净输入上则表现正常。由于 LLM 具备上下文学习能力，攻击者甚至无需访问训练数据或模型参数，仅通过恶意提示即可实施“免训练、推理阶段”的后门攻击。
- **传统防御困境**：
  - 现有防御多针对小模型，依赖微调、触发器逆向工程或大量干净数据，需要模型访问权限和高算力，不适用于仅 API 可访问的 LLM。
  - 部分方法假设防御者知道攻击机制或触发器类型，现实中不成立。
  - 多数方法只给出二元检测结果，缺乏可解释性。
- **整体含义**：论文提出一种面向黑盒 API LLM 的实用后门检测方法，利用 LLM 自身推理能力，在低数据、低算力、无需模型参数的条件下识别并缓解后门攻击。

## 2. 方法论：Chain-of-Scrutiny（CoS）

### 2.1 核心思想

- 后门攻击本质上使模型形成“触发器 → 目标输出”的**捷径记忆**，绕过正常推理。
- CoS 引导被检测 LLM 生成详细推理步骤，再审查推理过程与最终输出是否一致；若出现矛盾，则说明输出可能被后门操纵。
- 方法分为两个阶段：
  1. **推理阶段（Reasoning）**：用精心设计的演示提示 LLM 按“理解问题—分析选项—综合结论”的流程生成推理。
  2. **审查阶段（Scrutiny）**：检查推理步骤与原始输出之间是否存在不一致。

### 2.2 关键技术细节

- **CoS 推理演示设计**包含三部分：
  - 任务指令：先理解问题，再逐项分析选项。
  - 中间结果：评估每个选项与问题的匹配程度，并解释错误选项为何不成立。
  - 最终输出：综合前述信息得出结论。
- **演示目的**：不仅提升答案正确性，更强调推理与结论的一致性，使被后门污染的模型难以直接走“捷径”。
- **检测规则**：满足任一条件即判定为可能遭受攻击：
  1. 原始输出与经 CoS 推理后得到的输出不一致；
  2. 推理步骤中暴露了“触发器”或“特定规则”；
  3. 推理步骤与最终输出相互矛盾。
- **执行方式**：检测可由防御者人工完成，也可将检测规则编码为提示，由（可能已被污染的）LLM 自动执行。
- **与普通 CoT 的区别**：普通 CoT 只要求模型“逐步思考”，但攻击者可指示模型隐藏规则、编造理由；CoS 通过结构化演示强制模型先分析再结论，从而暴露矛盾。
- **与 Self-Reflection 的区别**：Self-Reflection 的目标是“生成—评估—改进”输出质量；CoS 的目标是检测不可信行为和后门操纵。
- 论文未给出显式数学公式，方法流程以自然语言提示和一致性规则为主。

### 2.3 威胁模型与假设

- **攻击者能力**：可访问用户 API 查询并插入恶意指令，包括触发器、目标输出、干净与后门演示；但不能访问训练数据、训练过程或模型参数。
- **防御者能力**：不知道是否被攻击、不知道触发器；没有相关干净数据，也缺乏专业防御知识。
- **防御目标**：检测输出是否被操纵，并在可能情况下借助推理步骤推断正确输出。

## 3. 实验设计

### 3.1 数据集与任务场景

- **推理/多选题任务**：
  - MMLU：57 个学科的四选一问题。
  - CSQA：常识问答，五选一。
  - ARC：科学考试题，四选一。
  - AQuA：代数文字题，五选一。
- **生成任务**：
  - GSM8K：小学数学应用题。
  - Letter：取单词末字母并拼接的任务。
- **文本分类任务**：
  - SST-2：电影评论情感二分类，被重格式化为多选题。
  - AG-NEWS：新闻主题四分类，被重格式化为多选题。

### 3.2 模型与攻击设置

- **受害模型**：GPT-3.5、GPT-4、Gemini、Llama3；部分生成任务和自适应攻击实验使用 GPT-4o。
- **主要攻击**：BadWord，使用特殊 token “cf” 作为触发器。
- **其他攻击**：AddSent（追加中性句子）、StyTrans（文本风格转换）、BadChain 风格的自适应攻击（攻击者将后门嵌入推理过程）。
- **攻击注入方式**：GPT 系列通过系统提示注入攻击指令和后门演示；Llama3 和 Gemini 通过用户提示注入。
- **目标输出**：多选题通常强制选 C；SST-2 目标为 Positive，AG-NEWS 目标为 Business；生成任务中 Letter 目标为反转原输出，GSM8K 目标为原数值乘 2.1。

### 3.3 对比方法与评价指标

- **基线方法**：
  - ONION：基于可疑 token 检测与移除。
  - Reasoning：直接要求 LLM 解释决策。
  - ZS-CoT：零样本思维链，“Let’s think step by step”。
  - Auto-CoT：自动生成推理演示的 CoT。
  - Self-Reflection：作为额外对比的“生成—评估— refine”方法。
- **评价指标**：
  - TPR：被触发输入中被成功检测出的比例。
  - FPR：干净输入被误判为受攻击的比例。
  - ASR / 误分类率：衡量攻击本身的有效性。
- **检测执行**：实验中主要使用 GPT-4 根据三条检测规则判断是否受攻击。

## 4. 资源与算力

- 论文**未明确报告** GPU 型号、数量、训练时长或 API 调用成本。
- 该方法本身**不训练模型**，主要在推理阶段通过 API 调用 LLM；ONION 基线使用预训练 GPT-2 检测可疑 token。
- 因此，实验算力主要体现在大量 API 推理调用上，但文中没有给出具体调用量、费用或延迟统计。

## 5. 实验数量与充分性

- 实验覆盖较广，主要包括：
  - 表 1：4 个推理数据集 × 4 个模型的后门攻击有效性。
  - 表 2：4 个推理数据集 × 4 个模型 × 5 种防御方法的 TPR/FPR。
  - 表 3：2 个生成任务 × 2 个模型 × 5 种防御方法。
  - 表 4：2 个分类任务 × 2 个模型 × 5 种防御方法。
  - 表 5：Self-Reflection 与 CoS 在 CSQA、ARC 上的对比。
  - 表 6：AddSent、StyTrans 两种攻击 × 2 个模型 × 5 种防御方法。
  - 表 7：生成任务下 ONION 与 CoS 的对比。
  - 图 3：温度、演示数量等超参数研究。
  - 图 4：不同目标选项 A/B/D 下的攻击与检测。
  - 附录包含大量案例、自适应攻击、不同触发器、角色扮演/日常对话适用性示例。
- **充分性评价**：
  - 数据集、模型、攻击类型和基线覆盖较全面，包含主实验、消融式超参数分析、自适应攻击和案例研究。
  - 实验设计总体较客观：统一温度、统一演示数量、与多种推理型及 token 型防御对比。
  - 但存在潜在公平性问题：检测统一由 GPT-4 按规则判断，可能引入评估偏差；部分实验样本量和统计显著性未报告；不同任务使用了任务特定演示，虽论文声称同一演示可跨任务使用，但附录也提供了针对生成任务的专门演示。

## 6. 主要结论与发现

- **后门攻击威胁显著**：仅用两个后门演示，LLM 在多数设置下 ASR 超过 85%，GPT-4 在多个数据集上达到 100%。
- **CoS 检测效果较强**：在多数模型和数据集上 TPR 超过 80%；GPT-4 上尤为突出，例如 MMLU 86%、CSQA 92%、ARC 95%、AQuA 91%。
- **优于基线**：Reasoning、ZS-CoT、Auto-CoT 和 ONION 的 TPR 通常明显较低；CoS 在保持较低或可比 FPR 的同时取得更高 TPR。
- **更强模型收益更大**：CoS 依赖 LLM 推理能力，GPT-4 等强模型上的检测效果优于较弱模型。
- **攻击无关性较好**：对 AddSent、StyTrans 以及 BadChain 风格自适应攻击仍有一定检测能力，但自适应攻击下 TPR 有所下降，例如 AddSent/StyTrans 下 GPT-4 和 Gemini 的 TPR 约为 68%–78%。
- **超参数较稳定**：温度 0.2–1.0、演示数量 1–5 时，TPR 和 FPR 波动有限。
- **可解释与可缓解**：推理步骤可帮助用户发现触发器、攻击规则，并推断正确输出。
- **主要弱点**：AQuA 等数学推理任务上 FPR 较高，归因于 LLM 幻觉和数学推理能力不足。

## 7. 优点

- **黑盒实用性强**：无需模型参数、训练数据或微调，适合 API-only LLM。
- **低成本、少数据**：只需一个推理演示即可工作，用户无需专业防御知识。
- **透明可解释**：自然语言驱动的推理与审查过程可被用户检查。
- **攻击无关设计**：不假设具体触发器类型，演示基于正常推理构建。
- **跨任务适用**：覆盖多选题、生成、分类，并可扩展到角色扮演和日常对话。
- **实验较全面**：多模型、多数据集、多攻击、多基线，并包含自适应攻击和超参数分析。
- **可与模型能力协同**：模型推理能力越强，CoS 检测收益越明显。

## 8. 不足与局限

- **攻击覆盖有限**：主要验证提示注入型后门，未充分验证训练集投毒型后门；作者也承认弱推理模型上的投毒防御仍待研究。
- **依赖 LLM 推理能力**：Llama3-7B 等较弱模型可能推理不足；数学任务中幻觉会导致高 FPR，AQuA 上尤为明显。
- **检测并非完美**：TPR 未达到 100%，自适应攻击下明显下降；存在漏检和误检。
- **评估可能存在偏差**：检测由 GPT-4 或可能被污染的 LLM 执行，存在循环评估和提示敏感性风险；人工审查的可扩展性也未量化。
- **资源成本未量化**：未报告 API 调用成本、延迟、吞吐量，实际部署经济性不明确。
- **样本量与统计显著性不足**：文中未明确各实验样本数量、置信区间或显著性检验。
- **演示设计仍有一定任务适配性**：虽然论文强调统一演示，但附录针对生成任务提供了专门演示，跨任务通用性需谨慎看待。
- **现实第三方服务风险复杂**：论文主要从用户侧防御，未系统讨论服务提供方、平台监管和多轮对话中的组合攻击。

（完）
