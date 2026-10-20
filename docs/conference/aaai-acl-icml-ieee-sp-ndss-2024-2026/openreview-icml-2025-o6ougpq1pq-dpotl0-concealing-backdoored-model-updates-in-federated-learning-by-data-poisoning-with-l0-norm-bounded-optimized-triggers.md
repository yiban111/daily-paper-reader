---
title: "$DPOT_{L_0}$: Concealing Backdoored model updates in Federated Learning by Data Poisoning with $L_0$-norm-bounded Optimized Triggers"
title_zh: DPOT_L0：通过L0范数约束优化触发器的数据投毒隐藏联邦学习中的后门模型更新
authors: "Yujie Zhang, Neil Zhenqiang Gong, Michael K. Reiter"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=O6OugpQ1Pq"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 通过优化触发器隐藏后门模型更新
tldr: 联邦学习中固定触发器后门因恶意与良性更新差异大而易被防御识别。本文提出DPOT_L0，每轮优化L0范数受限的触发器构造后门目标，使后门数据对模型更新影响最小。理论与实验表明该方法能有效隐藏恶意更新、保持主任务性能并抵御先进防御，提升了联邦学习后门攻击的隐蔽性。
source: ICML-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 固定触发器后门更新差异大，易被先进联邦学习防御识别。
method: 每轮优化L0范数受限触发器以最小化对模型更新的影响。
result: 实验表明能隐藏恶意更新并保持主任务性能。
conclusion: 提升了联邦学习后门攻击的隐蔽性与防御规避能力。
---

## Abstract
Traditional backdoor attacks in Federated Learning (FL) that rely on fixed trigger patterns and model poisoning exhibit deficient attacking performance against state-of-the-art defenses due to the significant divergence between malicious and benign client model updates. To effectively conceal malicious model updates among benign ones, we propose $DPOT_{L_0}$, a backdoor attack strategy in FL that dynamically constructs a per-round backdoor objective by optimizing an $L_0$-norm-bounded backdoor trigger, making backdoor data have minimal effect on model updates and preserving the global model's main-task performance. We theoretically justify the concealment property of $DPOT_{L_0}$'s model updates in linear models.  Our experiments show that $DPOT_{L_0}$, via only a data-poisoning attack, effectively undermines state-of-the-art defenses and outperforms existing backdoor attack techniques on various datasets.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
通过优化触发器隐藏后门模型更新。

### 2. 核心内容
联邦学习中固定触发器后门因恶意与良性更新差异大而易被防御识别。本文提出DPOT_L0，每轮优化L0范数受限的触发器构造后门目标，使后门数据对模型更新影响最小。理论与实验表明该方法能有效隐藏恶意更新、保持主任务性能并抵御先进防御，提升了联邦学习后门攻击的隐蔽性。

### 3. 对应检索需求
Backdoor injection through model poisoning, pretrained weights, fine tuning。

### 4. 来源与原文
- Source：ICML-2025-Rejected-Public
- OpenReview：[https://openreview.net/forum?id=O6OugpQ1Pq](https://openreview.net/forum?id=O6OugpQ1Pq)
