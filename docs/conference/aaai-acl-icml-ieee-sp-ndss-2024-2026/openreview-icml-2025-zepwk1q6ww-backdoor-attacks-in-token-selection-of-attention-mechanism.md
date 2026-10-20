---
title: Backdoor Attacks in Token Selection of Attention Mechanism
title_zh: 注意力机制令牌选择中的后门攻击
authors: "Yunjuan Wang, Raman Arora"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=ZEPWk1Q6ww"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 对Transformer注意力令牌选择后门的理论分析
tldr: 现有后门攻击研究多停留在经验设计与评估，缺乏对成功机理的严格理论解释。本文聚焦Transformer注意力机制中的令牌选择过程，证明经梯度下降训练的单头自注意力模型能够插值拟合投毒训练数据，并刻画后门生效的条件。该分析为大模型后门攻击提供了理论基础与可解释视角。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 后门攻击研究多为经验性设计，缺乏对攻击成功原因的理论理解。
method: 从理论上分析注意力令牌选择过程，证明单头自注意力可插值拟合投毒数据。
result: 推导出后门在特定条件下成立的理论条件与机理。
conclusion: 该工作为Transformer后门攻击提供了严格的理论解释框架。
---

## Abstract
Despite the remarkable success of large foundation models across a range of tasks, they remain susceptible to security threats such as backdoor attacks. By injecting poisoned data containing specific triggers during training, adversaries can manipulate model predictions in a targeted manner. While prior work has focused on empirically designing and evaluating such attacks, a rigorous theoretical understanding of when and why they succeed is lacking. In this work, we analyze backdoor attacks that exploit the token selection process within attention mechanisms--a core component of transformer-based architectures. We show that single-head self-attention transformers trained via gradient descent can interpolate poisoned training data. Moreover, we prove that when the backdoor triggers are sufficiently strong but not overly dominant, attackers can successfully manipulate model predictions. Our analysis characterizes how adversaries manipulate token selection to alter outputs and identifies the theoretical conditions under which these attacks succeed. We validate our findings through experiments on synthetic datasets.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
对Transformer注意力令牌选择后门的理论分析。

### 2. 核心内容
现有后门攻击研究多停留在经验设计与评估，缺乏对成功机理的严格理论解释。本文聚焦Transformer注意力机制中的令牌选择过程，证明经梯度下降训练的单头自注意力模型能够插值拟合投毒训练数据，并刻画后门生效的条件。该分析为大模型后门攻击提供了理论基础与可解释视角。

### 3. 对应检索需求
language model backdoors。

### 4. 来源与原文
- Source：ICML-2025-Accepted
- OpenReview：[https://openreview.net/forum?id=ZEPWk1Q6ww](https://openreview.net/forum?id=ZEPWk1Q6ww)
