---
title: Coupled Trigger Optimization and Vulnerable Parameter Alignment for Persistent Backdoor Attacks on Federated Learning
title_zh: 面向联邦学习持久后门攻击的触发器优化与脆弱参数对齐耦合方法
authors: "Zhixuan Ma, Haichang Gao, Shangwen Li, Ping Wang, Han Yu"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/322e075dfd13f87d8889eb3cb3dce4381b71b4db.pdf"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 通过触发器优化实现联邦学习持久后门攻击
tldr: 联邦学习中后门攻击在反复聚合下容易衰减或被检测，现有方法依赖启发式触发器设计或盲目参数操纵。作者从优化动力学视角出发，提出触发器优化与脆弱参数对齐相结合的CTO-VPA攻击方法，将更新约束到与聚合稳定的耦合子空间。该方法旨在让触发器诱导表示与聚合稳定参数方向对齐，从而维持后门的持久有效。实验表明该攻击在持续聚合下仍能保持较强后门效果，提升了联邦学习后门攻击的隐蔽性与持久性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 联邦学习后门攻击在反复聚合下易快速衰减或被检测，现有方法依赖启发式触发器或盲目参数操纵。
method: 从优化动力学视角提出CTO-VPA，将触发器优化与脆弱参数对齐结合，把更新约束在聚合稳定的耦合子空间内。
result: 该方法使触发器诱导表示与聚合稳定参数方向对齐，从而在持续聚合下维持较强的后门效果。
conclusion: 为联邦学习场景下持久且隐蔽的后门攻击提供了新的优化视角与方法。
---

## Abstract
Federated learning (FL) is vulnerable to backdoor attacks. Yet sustaining backdoor effectiveness under repeated aggregation remains challenging. Existing methods often rely on heuristic trigger designs or indiscriminant parameter manipulation, leading to rapid decay or detectable anomalies. In this work, we view FL backdoor persistence through the lens of optimization dynamics, and argue that long-lasting attacks require alignment between trigger-induced representations and aggregation-stable parameter directions. Based on this insight, we propose the Coupled Trigger Optimization and Vulnerable Parameter Alignment (CTO-VPA) FL backdoor attack method.
By constraining updates to this coupled subspace, backdoor behaviors can be embedded into optimization-stable directions while preserving benign performance. Experiments across multiple datasets and defense settings show that CTO-VPA achieves substantially improved persistence and robustness compared to prior attacks, highlighting the importance of trigger–parameter coupling in FL settings. The code is available at \url{https://github.com/SwLi415/COVERT}.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
通过触发器优化实现联邦学习持久后门攻击。

### 2. 核心内容
联邦学习中后门攻击在反复聚合下容易衰减或被检测，现有方法依赖启发式触发器设计或盲目参数操纵。作者从优化动力学视角出发，提出触发器优化与脆弱参数对齐相结合的CTO-VPA攻击方法，将更新约束到与聚合稳定的耦合子空间。该方法旨在让触发器诱导表示与聚合稳定参数方向对齐，从而维持后门的持久有效。实验表明该攻击在持续聚合下仍能保持较强后门效果，提升了联邦学习后门攻击的隐蔽性与持久性。

### 3. 对应检索需求
backdoor attacks。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=X3wIJK1gmP](https://openreview.net/forum?id=X3wIJK1gmP)
