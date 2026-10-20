---
title: "When Backdoors Speak: Understanding LLM Backdoor Attacks Through Model-Generated Explanations"
title_zh: 当后门开口：通过模型生成解释理解大语言模型后门攻击
authors: "Huaizhi Ge, Yiming Li, Qifan Wang, Yongfeng Zhang, Ruixiang Tang"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2025.acl-long.114.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 通过模型生成解释理解大语言模型后门攻击
tldr: 现有研究多聚焦于构造大语言模型后门攻击，却较少探究其内在机理。本文利用模型生成的自然语言解释来理解后门行为，对比干净样本与投毒样本的解释差异。结果显示被后门模型对干净输入解释连贯，而对投毒输入解释多样且逻辑混乱，该现象在分类与生成任务中均一致，为后门检测提供了新视角。
source: ACL-2025-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1841, \"height\": 1496}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 1841, \"height\": 1496}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 1200, \"height\": 597}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 1200, \"height\": 597}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 1200, \"height\": 1120}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 1200, \"height\": 1122}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-007.webp\", \"caption\": \"\", \"page\": 2, \"index\": 7, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 1200, \"height\": 1680}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-009.webp\", \"caption\": \"\", \"page\": 4, \"index\": 9, \"width\": 1200, \"height\": 1680}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-010.webp\", \"caption\": \"\", \"page\": 5, \"index\": 10, \"width\": 2340, \"height\": 2100}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-011.webp\", \"caption\": \"\", \"page\": 8, \"index\": 11, \"width\": 1841, \"height\": 1496}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-012.webp\", \"caption\": \"\", \"page\": 8, \"index\": 12, \"width\": 1841, \"height\": 1496}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-013.webp\", \"caption\": \"\", \"page\": 8, \"index\": 13, \"width\": 1841, \"height\": 1496}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-014.webp\", \"caption\": \"\", \"page\": 8, \"index\": 14, \"width\": 1841, \"height\": 1496}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-015.webp\", \"caption\": \"\", \"page\": 8, \"index\": 15, \"width\": 1841, \"height\": 1496}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-016.webp\", \"caption\": \"\", \"page\": 8, \"index\": 16, \"width\": 1841, \"height\": 1496}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-017.webp\", \"caption\": \"\", \"page\": 11, \"index\": 17, \"width\": 1000, \"height\": 1481}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-018.webp\", \"caption\": \"\", \"page\": 13, \"index\": 18, \"width\": 1000, \"height\": 1481}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-019.webp\", \"caption\": \"\", \"page\": 14, \"index\": 19, \"width\": 558, \"height\": 379}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-020.webp\", \"caption\": \"\", \"page\": 14, \"index\": 20, \"width\": 558, \"height\": 380}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-021.webp\", \"caption\": \"\", \"page\": 14, \"index\": 21, \"width\": 564, \"height\": 378}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-022.webp\", \"caption\": \"\", \"page\": 14, \"index\": 22, \"width\": 565, \"height\": 379}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-023.webp\", \"caption\": \"\", \"page\": 14, \"index\": 23, \"width\": 840, \"height\": 525}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-024.webp\", \"caption\": \"\", \"page\": 15, \"index\": 24, \"width\": 551, \"height\": 379}, {\"url\": \"assets/figures/acl-2025-long/anthology-2025acl-long114/fig-025.webp\", \"caption\": \"\", \"page\": 15, \"index\": 25, \"width\": 551, \"height\": 378}]"
motivation: 现有研究侧重构造LLM后门攻击，缺乏对其行为机理的可解释分析。
method: 利用模型生成的自然语言解释，对比干净与投毒样本的决策解释差异。
result: 实验发现后门模型对投毒输入给出逻辑混乱的解释，且跨任务一致。
conclusion: 生成式解释可作为理解与检测LLM后门的新工具。
---

## Abstract
Large Language Models (LLMs) are known to be vulnerable to backdoor attacks, where triggers embedded in poisoned samples can maliciously alter LLMs’ behaviors. In this paper, we move beyond attacking LLMs and instead examine backdoor attacks through the novel lens of natural language explanations. Specifically, we leverage LLMs’ generative capabilities to produce human-readable explanations for their decisions, enabling direct comparisons between explanations for clean and poisoned samples. Our results show that backdoored models produce coherent explanations for clean inputs but diverse and logically flawed explanations for poisoned data, a pattern consistent across classification and generation tasks for different backdoor attacks. Further analysis reveals key insights into the explanation generation process. At the token level, explanation tokens associated with poisoned samples only appear in the final few transformer layers. At the sentence level, attention dynamics indicate that poisoned inputs shift attention away from the original input context during explanation generation. These findings enhance our understanding of backdoor mechanisms in LLMs and present a promising framework for detecting vulnerabilities through explainability.

---

## 论文详细总结（自动生成）

以下总结基于论文正文与附录内容，按要点展开。

## 1. 核心问题与整体含义
- **研究背景**：大语言模型（LLM）易受后门攻击：模型在干净输入上表现正常，但遇到含预置触发器的投毒输入时会转向攻击者指定的错误预测或有害生成。现有研究多关注“如何构造攻击”，对后门模型内部行为机制及其可解释性分析仍不足。
- **核心问题**：论文不提出新的攻击，而是从**模型生成的自然语言解释**这一新视角审视后门攻击：后门模型如何为干净输入与投毒输入的决策“辩护”？这些解释在质量、一致性及内部生成过程上有何差异？
- **
