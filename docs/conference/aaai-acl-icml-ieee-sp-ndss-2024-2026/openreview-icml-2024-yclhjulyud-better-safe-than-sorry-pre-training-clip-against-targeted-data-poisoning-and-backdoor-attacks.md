---
title: "Better Safe than Sorry: Pre-training CLIP against Targeted Data Poisoning and Backdoor Attacks"
title_zh: 有备无患：在预训练阶段防御CLIP的目标数据投毒与后门攻击
authors: "Wenhan Yang, Jingdong Gao, Baharan Mirzasoleiman"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=ycLHJuLYuD"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 针对CLIP预训练的目标数据投毒与后门攻击防御
tldr: CLIP在大规模图文数据上预训练效果优异，却比监督学习更易受目标数据投毒与后门攻击影响，仅污染极小比例数据即可得手，而现有防御手段十分有限。作者提出SAFECLIP，在预训练阶段引入强防御以抵御目标投毒与后门。实验表明该方法能在保持性能的同时显著提升CLIP预训练的安全性，为可信多模态预训练提供了防护思路。
source: ICML-2024-Public
selection_source: conference_retrieval
motivation: CLIP对目标数据投毒与后门攻击极为脆弱，极少比例污染即可成功，现有预训练防御有限。
method: 提出SAFECLIP防御框架，在CLIP预训练过程中抵御目标数据投毒与后门注入。
result: 实验显示SAFECLIP能有效降低投毒攻击成功率并维持CLIP的零样本与迁移性能。
conclusion: 该工作凸显多模态预训练投毒风险，并提供可行的预训练阶段安全防护方案。
---

## Abstract
Contrastive Language-Image Pre-training (CLIP) on large image-caption datasets has achieved remarkable success in zero-shot classification and enabled transferability to new domains. However, CLIP is extremely more vulnerable to targeted data poisoning and backdoor attacks compared to supervised learning. Perhaps surprisingly, poisoning 0.0001% of CLIP pre-training data is enough to make targeted data poisoning attacks successful. This is four orders of magnitude smaller than what is required to poison supervised models. Despite this vulnerability, existing methods are very limited in defending CLIP models during pre-training. In this work, we propose a strong defense, SAFECLIP, to safely pre-train CLIP against targeted data poisoning and backdoor attacks. SAFECLIP warms up the model by applying unimodal contrastive learning (CL) on image and text modalities separately. Then, it divides the data into safe and risky sets by applying a Gaussian Mixture Model to the cosine similarity of image-caption pair representations. SAFECLIP pre-trains the model by applying the CLIP loss to the safe set and applying unimodal CL to image and text modalities of the risky set separately. By gradually increasing the size of the safe set during pre-training, SAFECLIP effectively breaks targeted data poisoning and backdoor attacks without harming the CLIP performance. Our extensive experiments on CC3M, Visual Genome, and MSCOCO demonstrate that SAFECLIP significantly reduces the success rate of targeted data poisoning attacks from 93.75% to 0% and that of various backdoor attacks from up to 100% to 0%, without harming CLIP’s performance.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
针对CLIP预训练的目标数据投毒与后门攻击防御。

### 2. 核心内容
CLIP在大规模图文数据上预训练效果优异，却比监督学习更易受目标数据投毒与后门攻击影响，仅污染极小比例数据即可得手，而现有防御手段十分有限。作者提出SAFECLIP，在预训练阶段引入强防御以抵御目标投毒与后门。实验表明该方法能在保持性能的同时显著提升CLIP预训练的安全性，为可信多模态预训练提供了防护思路。

### 3. 对应检索需求
model poisoning。

### 4. 来源与原文
- Source：ICML-2024-Public
- OpenReview：[https://openreview.net/forum?id=ycLHJuLYuD](https://openreview.net/forum?id=ycLHJuLYuD)
