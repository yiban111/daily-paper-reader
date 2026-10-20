---
title: "Bi-perspective Splitting Defense: Achieving Clean-Seed-Free Backdoor Security"
title_zh: 双视角分离防御：实现无需干净种子的后门安全
authors: "Yangyang Shen, Xiao Tan, Dian Shen, Meng Wang, Beilun Wang"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=JKzS7zJRep"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 无需干净种子的后门防御
tldr: 高性能后门防御往往依赖额外干净数据，而隐私与数据稀缺使其难以获得。本文提出双视角分离防御，在无干净种子条件下利用目标标签与干净困难样本训练良性模型。实验表明其在去除后门的同时保持良好性能并降低开销。该工作推动了无干净数据的实用后门防御。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有后门防御依赖额外干净数据，受隐私与数据稀缺限制难以落地。
method: 提出双视角分离防御，利用目标标签与干净困难样本从投毒数据训练良性模型。
result: 实验表明该方法在无干净种子下有效削弱后门并保持模型性能。
conclusion: 该工作实现了低开销、无需干净种子的实用后门防御。
---

## Abstract
Backdoor attacks have seriously threatened deep neural networks (DNNs) by embedding concealed vulnerabilities through data poisoning. To counteract these attacks, training benign models from poisoned data garnered considerable interest from researchers. High-performing defenses often rely on additional clean subsets/seeds, which is untenable due to increasing privacy concerns and data scarcity. In the absence of additional clean subsets/seeds, defenders resort to complex feature extraction and analysis, resulting in excessive overhead and compromised performance. To address these challenges, we identify the key lies in sufficient utilization of both the easier-to-obtain target labels and clean hard samples. In this work, we propose a Bi-perspective Splitting Defense (BSD). BSD distinguishes clean samples using both semantic and loss statistics characteristics through open set recognition-based splitting (OSS) and altruistic model-based data splitting (ALS) respectively. Through extensive experiments on benchmark datasets and against representative attacks, we empirically demonstrate that BSD surpasses existing defenses by over 20\% in average Defense Effectiveness Rating (DER), achieving clean data-free backdoor security.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
无需干净种子的后门防御。

### 2. 核心内容
高性能后门防御往往依赖额外干净数据，而隐私与数据稀缺使其难以获得。本文提出双视角分离防御，在无干净种子条件下利用目标标签与干净困难样本训练良性模型。实验表明其在去除后门的同时保持良好性能并降低开销。该工作推动了无干净数据的实用后门防御。

### 3. 对应检索需求
backdoor attacks。

### 4. 来源与原文
- Source：ICML-2025-Accepted
- OpenReview：[https://openreview.net/forum?id=JKzS7zJRep](https://openreview.net/forum?id=JKzS7zJRep)
