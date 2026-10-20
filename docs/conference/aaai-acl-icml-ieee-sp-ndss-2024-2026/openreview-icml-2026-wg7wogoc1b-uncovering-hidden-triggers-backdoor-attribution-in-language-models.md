---
title: "Uncovering Hidden Triggers: Backdoor Attribution in Language Models"
title_zh: 揭示隐藏触发器：语言模型中的后门归因
authors: "Miao Yu, Zhenhong Zhou, Moayad Aloqaily, Kun Wang, Biwei Huang, Stephen Wang, Yueming Jin, Qingsong Wen"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/97999e84ddb39d64b0b0b18cdcd4da8f13c5db40.pdf"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 语言模型后门归因的可解释机制
tldr: 微调后的大语言模型易受数据投毒后门攻击，但其内部机制仍是黑箱。作者提出后门归因框架BkdAttr，通过三部分因果分析探索可解释机制，并设计后门探针证实在表示中存在可学习的后门特征。研究进一步揭示后门注意力等结构。该工作填补了大模型后门可解释性空白，有助于彻底消除后门威胁。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 大语言模型后门攻击机制仍是黑箱，现有可解释研究忽视后门问题。
method: 提出BkdAttr三部分因果分析框架，用后门探针等工具解析后门特征。
result: 证实在模型表示中存在可学习的后门特征并揭示其内部结构。
conclusion: 填补了大模型后门可解释性空白，有助于更彻底地消除后门威胁。
---

## Abstract
Fine-tuned Large Language Models (LLMs) are vulnerable to backdoor attacks through data poisoning, yet the internal mechanisms governing these attacks remain a black box. Previous research on interpretability for LLM safety tends to focus on alignment, jailbreak, and hallucination, but overlooks backdoor mechanisms, making it difficult to understand and fully eliminate the backdoor threat. In this paper, aiming to bridge this gap, we explore the interpretable mechanisms of LLM backdoors through Backdoor Attribution (BkdAttr), a tripartite causal analysis framework. We first introduce the Backdoor Probe that proves the existence of learnable backdoor features encoded within the representations. Building on this insight, we further develop Backdoor Attention Head Attribution (BAHA), efficiently pinpointing the specific attention heads responsible for processing these features. Our primary experiments reveals these heads are relatively sparse; ablating a minimal \textbf{$\sim$ 3%} of total heads is sufficient to reduce the Attack Success Rate (ASR) by \textbf{over 90%}. More importantly, we further employ these findings to construct the Backdoor Vector derived from these attributed heads as a master controller for the backdoor. Through only \textbf{1-point} intervention on \textbf{single} representation, the vector can either boost ASR up to \textbf{$\sim$ 100% $\uparrow$} on clean inputs, or completely neutralize backdoor, suppressing ASR down to \textbf{
$\sim$ 0%} on triggered inputs. In conclusion, our work pioneers the exploration of mechanistic interpretability in LLM backdoors, demonstrating a powerful method for backdoor control and revealing actionable insights for the community.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
语言模型后门归因的可解释机制。

### 2. 核心内容
微调后的大语言模型易受数据投毒后门攻击，但其内部机制仍是黑箱。作者提出后门归因框架BkdAttr，通过三部分因果分析探索可解释机制，并设计后门探针证实在表示中存在可学习的后门特征。研究进一步揭示后门注意力等结构。该工作填补了大模型后门可解释性空白，有助于彻底消除后门威胁。

### 3. 对应检索需求
language model backdoors。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=wG7woGoc1b](https://openreview.net/forum?id=wG7woGoc1b)
