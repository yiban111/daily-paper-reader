---
title: "The Ripple Effect: On Unforeseen Complications of Backdoor Attacks"
title_zh: 涟漪效应：论后门攻击始料未及的连带影响
authors: "Rui Zhang, Yun Shen, Hongwei Li, Wenbo Jiang, Hanxiao Chen, Yuan Zhang, Guowen Xu, Yang Zhang"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=sw1Sm72dmV"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 量化影响预训练语言模型后门隐蔽性的连带效应
tldr: 第三方预训练语言模型可能被植入后门，但现有后门通常只对特定预定义下游任务有效。当这些模型被适配到其他无关任务时，可能产生非预期输出，从而引起用户怀疑并削弱攻击隐蔽性，作者称之为后门连带效应。该工作首次对该现象进行系统量化，在4个主流预训练模型和16个文本分类基准上开展实验。结果表明后门连带效应普遍存在，为评估后门攻击的隐蔽性与真实威胁提供了重要视角。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 被植入后门的预训练语言模型被适配到无关下游任务时会产生非预期输出，从而削弱攻击隐蔽性。
method: 首次系统量化后门连带效应，在4个预训练模型和16个文本分类基准上开展大规模实验分析。
result: 实验表明后门连带效应在多种模型和数据集上普遍存在，会暴露攻击并影响其实际隐蔽性。
conclusion: 为评估后门攻击的真实隐蔽性与潜在风险提供了新的量化视角。
---

## Abstract
Recent research highlights concerns about the trustworthiness of third-party Pre-Trained Language Models (PTLMs) due to potential backdoor attacks.
These backdoored PTLMs, however, are effective only for specific pre-defined downstream tasks.
In reality, these PTLMs can be adapted to many other unrelated downstream tasks.
Such adaptation may lead to unforeseen consequences in downstream model outputs, consequently raising user suspicion and compromising attack stealthiness.
We refer to this phenomenon as backdoor complications.
In this paper, we undertake the first comprehensive quantification of backdoor complications.
Through extensive experiments using 4 prominent PTLMs and 16 text classification benchmark datasets, we demonstrate the widespread presence of backdoor complications in downstream models fine-tuned from backdoored PTLMs.
The output distribution of triggered samples significantly deviates from that of clean samples.
Consequently, we propose a backdoor complication reduction method leveraging multi-task learning to mitigate complications without prior knowledge of downstream tasks.
The experimental results demonstrate that our proposed method can effectively reduce complications while maintaining the efficacy and consistency of backdoor attacks.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
量化影响预训练语言模型后门隐蔽性的连带效应。

### 2. 核心内容
第三方预训练语言模型可能被植入后门，但现有后门通常只对特定预定义下游任务有效。当这些模型被适配到其他无关任务时，可能产生非预期输出，从而引起用户怀疑并削弱攻击隐蔽性，作者称之为后门连带效应。该工作首次对该现象进行系统量化，在4个主流预训练模型和16个文本分类基准上开展实验。结果表明后门连带效应普遍存在，为评估后门攻击的隐蔽性与真实威胁提供了重要视角。

### 3. 对应检索需求
stealthy backdoor attacks。

### 4. 来源与原文
- Source：ICML-2025-Accepted
- OpenReview：[https://openreview.net/forum?id=sw1Sm72dmV](https://openreview.net/forum?id=sw1Sm72dmV)
