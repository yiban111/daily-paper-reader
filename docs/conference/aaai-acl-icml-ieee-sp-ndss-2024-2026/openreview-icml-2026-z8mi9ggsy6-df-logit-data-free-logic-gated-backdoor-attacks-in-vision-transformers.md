---
title: "DF-LoGiT: Data-Free Logic-Gated Backdoor Attacks in Vision Transformers"
title_zh: DF-LoGiT：视觉Transformer中无数据的逻辑门控后门攻击
authors: "Xiaozuo Shen, Yifei Cai, Rui Ning, Chunsheng Xin, Hongyi Wu"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/fcd8d6cad40b5af86c6f34e5b7155038b875e3c4.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 通过直接权重编辑在ViT中植入无数据后门
tldr: 现有ViT后门攻击多依赖投毒训练或合成数据微调，实用性受限。本文提出DF-LoGiT，通过直接编辑权重实现真正无数据的后门植入，并利用多头结构构建逻辑门控组合触发器。实验与理论分析表明其攻击近乎百分之百有效且隐蔽。该工作凸显了模型仓库的供应链风险。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有ViT后门依赖投毒数据或合成微调，供应链场景下实施受限。
method: 通过直接编辑权重并利用多头结构构建逻辑门控组合触发器植入后门。
result: 理论与实验表明该方法在无数据条件下实现近乎百分之百的攻击成功率。
conclusion: 该工作揭示了模型检查点发布环节的供应链后门风险。
---

## Abstract
The widespread adoption of Vision Transformers (ViTs) elevates supply-chain risk on third-party model hubs, where an adversary can implant backdoors into released checkpoints. Existing ViT backdoor attacks largely rely on poisoned-data training, while prior data-free attempts typically require synthetic-data fine-tuning or extra model components. This paper introduces Data-Free Logic-Gated Backdoor Attacks (DF-LoGiT), a truly data-free backdoor attack on ViTs via direct weight editing. DF-LoGiT exploits ViT’s native multi-head architecture to realize a logic-gated compositional trigger, enabling a stealthy and effective backdoor. We validate its effectiveness through theoretical analysis and extensive experiments, showing that DF-LoGiT achieves near-100% attack success with negligible degradation in benign accuracy and remains robust against representative classical and ViT-specific defenses.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
通过直接权重编辑在ViT中植入无数据后门。

### 2. 核心内容
现有ViT后门攻击多依赖投毒训练或合成数据微调，实用性受限。本文提出DF-LoGiT，通过直接编辑权重实现真正无数据的后门植入，并利用多头结构构建逻辑门控组合触发器。实验与理论分析表明其攻击近乎百分之百有效且隐蔽。该工作凸显了模型仓库的供应链风险。

### 3. 对应检索需求
Backdoor injection through model poisoning, pretrained weights, fine tuning。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=Z8Mi9GGsy6](https://openreview.net/forum?id=Z8Mi9GGsy6)
