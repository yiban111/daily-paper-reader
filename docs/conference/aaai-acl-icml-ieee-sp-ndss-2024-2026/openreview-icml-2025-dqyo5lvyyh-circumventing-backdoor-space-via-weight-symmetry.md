---
title: Circumventing Backdoor Space via Weight Symmetry
title_zh: 通过权重对称性规避后门空间
authors: "Jie Peng, Hongwei Yang, Jing Zhao, Hengji Dong, Hui He, Weizhe Zhang, Haoyu He"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=dqYO5LVyYh"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 基于权重对称性的免数据后门净化防御
tldr: 现有后门净化防御通常依赖标注数据或特定训练流程，难以推广到监督学习之外的各种范式。作者提出两阶段对称连通方法TSC，利用神经网络中的置换不变性和权重对称性进行后门净化，仅需少量干净样本且不依赖数据格式。理论分析与实验表明该方法能有效移除后门。该工作拓展了后门防御的适用范围。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有后门净化防御依赖标注数据或特定训练流程，适用范围受限。
method: 提出两阶段对称连通TSC，利用置换不变性与权重对称性净化后门。
result: 理论与实验表明仅需少量干净样本即可有效移除后门。
conclusion: 拓展了后门防御在多种学习范式下的适用性。
---

## Abstract
Deep neural networks are vulnerable to backdoor attacks, where malicious behaviors are implanted during training. While existing defenses can effectively purify compromised models, they typically require labeled data or specific training procedures, making them difficult to apply beyond supervised learning settings. Notably, recent studies have shown successful backdoor attacks across various learning paradigms, highlighting a critical security concern. To address this gap, we propose Two-stage Symmetry Connectivity (TSC), a novel backdoor purification defense that operates independently of data format and requires only a small fraction of clean samples. Through theoretical analysis, we prove that by leveraging permutation invariance in neural networks and quadratic mode connectivity, TSC amplifies the loss on poisoned samples while maintaining bounded clean accuracy. Experiments demonstrate that TSC achieves robust performance comparable to state-of-the-art methods in supervised learning scenarios. Furthermore, TSC generalizes to self-supervised learning frameworks, such as SimCLR and CLIP, maintaining its strong defense capabilities. Our code is available at https://github.com/JiePeng104/TSC.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
基于权重对称性的免数据后门净化防御。

### 2. 核心内容
现有后门净化防御通常依赖标注数据或特定训练流程，难以推广到监督学习之外的各种范式。作者提出两阶段对称连通方法TSC，利用神经网络中的置换不变性和权重对称性进行后门净化，仅需少量干净样本且不依赖数据格式。理论分析与实验表明该方法能有效移除后门。该工作拓展了后门防御的适用范围。

### 3. 对应检索需求
backdoor attacks。

### 4. 来源与原文
- Source：ICML-2025-Accepted
- OpenReview：[https://openreview.net/forum?id=dqYO5LVyYh](https://openreview.net/forum?id=dqYO5LVyYh)
