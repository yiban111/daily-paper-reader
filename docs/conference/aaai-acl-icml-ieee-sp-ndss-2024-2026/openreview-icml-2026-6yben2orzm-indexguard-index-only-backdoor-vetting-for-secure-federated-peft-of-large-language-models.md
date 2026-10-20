---
title: "INDEXGUARD: Index-only Backdoor Vetting for Secure Federated PEFT of Large Language Models"
title_zh: INDEXGUARD：面向大语言模型安全联邦PEFT的仅索引后门审查
authors: "Javad Dogani, Devriş İşler, Nikolaos Laoutaris"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/eeb8a11c2a4599c5a662132deedafe240a2b9771.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 联邦PEFT大语言模型中的后门投毒与仅索引防御
tldr: 联邦参数高效微调虽能在私有数据上定制大模型，却易受后门投毒影响，而隐私约束使服务器无法检查客户端的实值更新。为此作者提出IndexGuard，仅利用客户端上传的Top-K显著更新索引，在重叠几何上聚类客户端并剔除凝聚力离群组。该方法在无实值信息条件下实现后门筛查，为安全的联邦微调聚合提供了可用的防护原语。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 联邦PEFT易遭后门投毒，但隐私约束使服务器无法检查客户端实值更新，现有防御受限。
method: 提出仅索引审查原语，客户端只上传Top-K显著更新索引，服务器基于重叠几何聚类并过滤离群组。
result: 分析表明投毒在适配器坐标上留下相似印记，索引重叠可有效识别并过滤恶意客户端。
conclusion: 该工作为大模型联邦微调提供了一种无需实值更新的隐私友好后门防御方案。
---

## Abstract
Federated parameter-efficient fine-tuning (PEFT) enables customizing large language models on private data, yet it is vulnerable to backdoor poisoning—especially when privacy constraints prevent inspection of per-client real-valued updates. We exploit the intuition that poisoning leaves a similar backdoor imprint in which adapter coordinates become salient, so overlap in salient-index supports remains informative even without values. We introduce IndexGuard, an unsupervised index-only vetting primitive in which clients send only Top-$K$ salient update indices and the server operates on the induced overlap geometry, clustering clients and filtering cohesion-outlier groups before aggregation. We analyze support stability under bounded rescaling and separability under shared-trigger poisoning under non-IID drift. Across attacks, backbones, and PEFT variants, IndexGuard provides end-to-end mitigation, preserving clean accuracy while achieving performance comparable to centralized methods.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
联邦PEFT大语言模型中的后门投毒与仅索引防御。

### 2. 核心内容
联邦参数高效微调虽能在私有数据上定制大模型，却易受后门投毒影响，而隐私约束使服务器无法检查客户端的实值更新。为此作者提出IndexGuard，仅利用客户端上传的Top-K显著更新索引，在重叠几何上聚类客户端并剔除凝聚力离群组。该方法在无实值信息条件下实现后门筛查，为安全的联邦微调聚合提供了可用的防护原语。

### 3. 对应检索需求
Backdoor injection through model poisoning, pretrained weights, fine tuning。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=6YBEn2OrzM](https://openreview.net/forum?id=6YBEn2OrzM)
