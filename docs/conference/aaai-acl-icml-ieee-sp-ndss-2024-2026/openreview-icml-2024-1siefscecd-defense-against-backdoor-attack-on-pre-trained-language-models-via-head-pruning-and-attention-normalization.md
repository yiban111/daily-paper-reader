---
title: Defense against Backdoor Attack on Pre-trained Language Models via Head Pruning and Attention Normalization
title_zh: 通过头部剪枝与注意力归一化防御预训练语言模型后门攻击
authors: "Xingyi Zhao, Depeng Xu, Shuhan Yuan"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=1SiEfsCecd"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 针对预训练语言模型后门攻击的防御方法
tldr: 预训练语言模型经微调后仍易受后门攻击，而下游使用者往往不了解触发策略，防御困难。本文提出PURE方法，通过剪枝可疑注意力头并对注意力权重归一化来削弱后门行为。实验表明该方法在未知触发器的情况下仍能有效降低后门成功率，同时保持主任务性能，为PLM后门防御提供了实用方案。
source: ICML-2024-Public
selection_source: conference_retrieval
motivation: PLM经微调后仍易受后门影响，且使用者缺乏触发器知识导致防御困难。
method: 提出PURE，剪除可疑注意力头并对注意力权重归一化以削弱后门。
result: 实验表明该方法在未知触发器时仍有效降低后门成功率并保持主任务性能。
conclusion: 基于注意力结构的后处理防御可有效缓解PLM后门威胁。
---

## Abstract
Pre-trained language models (PLMs) are commonly used for various downstream natural language processing tasks via fine-tuning. However, recent studies have demonstrated that PLMs are vulnerable to backdoor attacks, which can mislabel poisoned samples to target outputs even after a vanilla fine-tuning process. The key challenge for defending against the backdoored PLMs is that end users who adopt the PLMs for their downstream tasks usually do not have any knowledge about the attacking strategies, such as triggers. To tackle this challenge, in this work, we propose a backdoor mitigation approach, PURE, via head pruning and normalization of attention weights. The idea is to prune the attention heads that are potentially affected by poisoned texts with only clean texts on hand and then further normalize the weights of remaining attention heads to mitigate the backdoor impacts. We conduct experiments to defend against various backdoor attacks on the classification task. The experimental results show the effectiveness of PURE in lowering the attack success rate without sacrificing the performance on clean texts.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
针对预训练语言模型后门攻击的防御方法。

### 2. 核心内容
预训练语言模型经微调后仍易受后门攻击，而下游使用者往往不了解触发策略，防御困难。本文提出PURE方法，通过剪枝可疑注意力头并对注意力权重归一化来削弱后门行为。实验表明该方法在未知触发器的情况下仍能有效降低后门成功率，同时保持主任务性能，为PLM后门防御提供了实用方案。

### 3. 对应检索需求
language model backdoors。

### 4. 来源与原文
- Source：ICML-2024-Public
- OpenReview：[https://openreview.net/forum?id=1SiEfsCecd](https://openreview.net/forum?id=1SiEfsCecd)
