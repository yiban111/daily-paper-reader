---
title: "Privacy Backdoors: Stealing Data with Corrupted Pretrained Models"
title_zh: 隐私后门：利用被污染的预训练模型窃取数据
authors: "Shanglun Feng, Florian Tramèr"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=7yixJXmzb8"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 通过篡改预训练权重植入后门窃取数据
tldr: 实践中人们常下载预训练模型再微调，却忽视其安全隐患。本文提出隐私后门，通过篡改预训练权重使攻击者能重构微调数据中的个体样本，并可攻破差分隐私模型。实验证明该攻击对包括Transformer在内的多种模型均高成功率。该工作揭示了预训练权重供应链的隐私风险。
source: ICML-2024-Public
selection_source: conference_retrieval
motivation: 下载并微调第三方预训练模型普遍存在，却可能被植入隐私窃取后门。
method: 通过篡改预训练模型权重构建隐私后门，使攻击者能重构微调样本。
result: 实验证明该后门对Transformer等模型可高成功率重构个体微调样本。
conclusion: 该工作揭示预训练权重供应链带来的严重隐私威胁。
---

## Abstract
Practitioners commonly download pretrained machine learning models from open repositories and finetune them to fit specific applications. We show that this practice introduces a new risk of privacy backdoors. By tampering with a pretrained model’s weights, an attacker can fully compromise the privacy of the finetuning data. We show how to build privacy backdoors for a variety of models, including transformers, which enable an attacker to reconstruct individual finetuning samples, with a guaranteed success! We further show that backdoored models allow for tight privacy attacks on models trained with differential privacy (DP). The common optimistic practice of training DP models with loose privacy guarantees is thus insecure if the model is not trusted. Overall, our work highlights a crucial and overlooked supply chain attack on machine learning privacy.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
通过篡改预训练权重植入后门窃取数据。

### 2. 核心内容
实践中人们常下载预训练模型再微调，却忽视其安全隐患。本文提出隐私后门，通过篡改预训练权重使攻击者能重构微调数据中的个体样本，并可攻破差分隐私模型。实验证明该攻击对包括Transformer在内的多种模型均高成功率。该工作揭示了预训练权重供应链的隐私风险。

### 3. 对应检索需求
Backdoor injection through model poisoning, pretrained weights, fine tuning。

### 4. 来源与原文
- Source：ICML-2024-Public
- OpenReview：[https://openreview.net/forum?id=7yixJXmzb8](https://openreview.net/forum?id=7yixJXmzb8)
