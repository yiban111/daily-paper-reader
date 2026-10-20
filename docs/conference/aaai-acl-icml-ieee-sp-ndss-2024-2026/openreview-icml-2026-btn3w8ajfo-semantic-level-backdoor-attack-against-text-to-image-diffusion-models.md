---
title: Semantic-level Backdoor Attack against Text-to-Image Diffusion Models
title_zh: 针对文生图扩散模型的语义级后门攻击
authors: "Tianxin Chen, Wenbo Jiang, Hongqiao Chen, Zhirun Zheng, Cheng Huang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/0ddbb47b0cf12013c73f337238844f03a67765f6.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 扩散模型中的语义级隐蔽后门触发
tldr: 现有文生图扩散模型后门依赖固定文本触发器和单一实体目标，易被枚举式与注意力一致性检测识别。本文提出语义级后门攻击SemBD，通过蒸馏式编辑交叉注意力键值投影矩阵，以连续语义区域作为表征级触发器。实验表明语义等价但文本多样的提示均可激活后门，显著提升了攻击的隐蔽性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 固定文本触发器后门易被枚举与注意力一致性检测识别。
method: 通过编辑交叉注意力键值矩阵植入语义区域级触发器。
result: 语义等价但文本多样的提示均能激活后门。
conclusion: 显著提升了扩散模型后门攻击的隐蔽性。
---

## Abstract
Text-to-image (T2I) diffusion models are widely adopted for their strong generative capabilities, yet remain vulnerable to backdoor attacks. Existing attacks typically rely on fixed textual triggers and single-entity backdoor targets, making them highly susceptible to enumeration-based input defenses and attention-consistency detection. In this work, we propose Semantic-level Backdoor Attack (SemBD), which introduces representation-level triggers based on continuous semantic regions rather than discrete textual patterns. SemBD implants such semantic backdoors by distillation-based editing of the key and value projection matrices in cross-attention layers, enabling semantically equivalent but textually diverse prompts to activate the backdoor. To further enhance stealthiness, SemBD incorporates a semantic regularization to prevent unintended activation under incomplete semantics, as well as multi-entity backdoor targets that avoid highly consistent cross-attention patterns. Extensive experiments demonstrate that SemBD achieves a 100% attack success rate while maintaining strong robustness against state-of-the-art input-level defenses. Our code is available at https://github.com/DPAS-Lab/SemBD/.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
扩散模型中的语义级隐蔽后门触发。

### 2. 核心内容
现有文生图扩散模型后门依赖固定文本触发器和单一实体目标，易被枚举式与注意力一致性检测识别。本文提出语义级后门攻击SemBD，通过蒸馏式编辑交叉注意力键值投影矩阵，以连续语义区域作为表征级触发器。实验表明语义等价但文本多样的提示均可激活后门，显著提升了攻击的隐蔽性。

### 3. 对应检索需求
Stealthy backdoor attacks using invisible, semantic, dynamic, or input adaptive triggers.

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=btN3w8aJfo](https://openreview.net/forum?id=btN3w8aJfo)
