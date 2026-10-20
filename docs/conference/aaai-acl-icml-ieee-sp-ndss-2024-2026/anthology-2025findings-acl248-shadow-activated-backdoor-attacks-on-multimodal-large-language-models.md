---
title: Shadow-Activated Backdoor Attacks on Multimodal Large Language Models
title_zh: 多模态大语言模型上的影子激活后门攻击
authors: "Ziyi Yin, Muchao Ye, Yuanpu Cao, Jiaqi Wang, Aofei Chang, Han Liu, Jinghui Chen, Ting Wang, Fenglong Ma"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2025.findings-acl.248.pdf"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 面向多模态大语言模型的无触发器影子激活后门
tldr: 多模态大模型在多轮开放对话中由用户主导交互，传统依赖外加触发器的后门难以适用。本文提出影子激活后门范式，当模型回复涉及被影子化的对象时即隐式注入恶意内容，无需显式触发器。研究揭示了MLLM在真实对话场景中的新型安全风险，并构建了相应的攻击与评估框架。
source: ACL-2025-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-004.webp\", \"caption\": \"\", \"page\": 18, \"index\": 4, \"width\": 640, \"height\": 424}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-005.webp\", \"caption\": \"\", \"page\": 18, \"index\": 5, \"width\": 500, \"height\": 375}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-006.webp\", \"caption\": \"\", \"page\": 20, \"index\": 6, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-007.webp\", \"caption\": \"\", \"page\": 20, \"index\": 7, \"width\": 640, \"height\": 427}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-008.webp\", \"caption\": \"\", \"page\": 22, \"index\": 8, \"width\": 640, \"height\": 427}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-009.webp\", \"caption\": \"\", \"page\": 22, \"index\": 9, \"width\": 500, \"height\": 322}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl248/fig-010.webp\", \"caption\": \"\", \"page\": 22, \"index\": 10, \"width\": 640, \"height\": 428}]"
motivation: 多模态大模型开放对话场景中，依赖外加触发器的传统后门不再适用。
method: 提出影子激活范式，在回复涉及影子对象时隐式注入恶意内容且无需触发器。
result: 实验证明该范式能在自然对话中稳定触发恶意行为。
conclusion: 该工作揭示了MLLM对话场景中隐蔽后门攻击的现实威胁。
---

## Abstract
This paper delves into a novel backdoor attack scenario, aiming to uncover potential security risks associated with Multimodal Large Language Models (MLLMs) during multi-round open-ended conversations with users. In the practical use of MLLMs, users have full control over the interaction process with the model, such as using their own collected photos and posing arbitrary open-ended questions. Traditional backdoor attacks that rely on adding external triggers are less applicable. To this end, we introduce a new shadow-activated backdoor attacking paradigm in this paper, wherein attacks implicitly inject malicious content into the responses of MLLMs when the responses explicitly relate to the shadowed object, i.e., without any triggers. To facilitate the shadow-activated backdoor attack, we present a novel framework named BadMLLM to achieve the desired behaviors by constructing a poisoned dataset using GPT-4 Vision and implementing an attention-regularized tuning strategy to address the semantic discontinuity between the original response and the inserted promotion. Extensive experimental results conducted on five MLLMs, three objects, and two types of promotion slogans have demonstrated impressive performance in achieving both efficacy and utility goals, thereby highlighting the significant potential risks concealed within MLLMs.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：多模态大语言模型（MLLM）已具备图像理解与开放文本生成能力，用户可在多轮对话中自由上传自采图片、提出任意开放问题。传统后门攻击通常依赖图像补丁或文本触发器，但在这种用户完全主导的交互中难以实施。
- **核心问题**：能否在**用户输入完全干净、无任何显式触发器**的情况下，让 MLLM 在回复内容涉及某个“影子实体”时，隐式插入品牌名、推广语甚至钓鱼链接？
- **整体含义**：论文提出“影子激活后门”（shadow-activated backdoor）新范式，将触发条件从“输入含触发器”改为“输出涉及影子实体”。这揭示了 MLLM 在真实多轮开放对话中的新型供应链与分发安全风险，尤其在广告注入、医疗误导、钓鱼诈骗等场景中具有现实危害。

## 2. 方法论：核心思想与关键技术

### 2.1 威胁模型与形式化定义

- **影子实体**：攻击者指定目标实体 \(O\)，并考虑其同义集合 \(O=[O_1,\dots,O_E]\)，如“car”与“sedan”。
- **注入内容**：与实体关联的品牌 \(S\)（如 BMW）及推广语 \(P=[t_1,\dots,t_Q]\)，推广语可含钓鱼链接。
- **触发条件**：当模型回复 \(Y\) 明确涉及图像中的影子实体 \(O\) 时触发；若回复与 \(O\) 无关，或图像不含 \(O\)，则保持正常。
- **注入位置**：将品牌 \(S\) 插入实体描述词 \(w_j=O_e\) 之前；将推广语 \(P\) 附加到包含该实体的句子末词 \(w_k\) 之后，得到修改回复  
  \(Y^*=[w_1,\dots,w_i,\dots,S,w_j,\dots,w
