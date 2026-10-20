---
title: Generalization Bound and New Algorithm for Clean-Label Backdoor Attack
title_zh: 干净标签后门攻击的泛化界与新算法
authors: "Lijia Yu, Shuang Liu, Yibo Miao, Xiao-Shan Gao, Lijun Zhang"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=ZdqiT0McON"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 干净标签后门攻击的泛化界与新算法
tldr: 后门攻击具有触发器同时出现在训练与测试集、目标双重的特殊性质，但其泛化界此前尚未建立，而泛化界是评估学习方法可泛化性的关键工具。作者在干净标签后门场景下推导出与算法无关的泛化界，给出干净样本总体误差的上界。基于该理论，论文进一步提出新的后门攻击算法，为理解与设计干净标签后门提供了理论支撑。
source: ICML-2024-Public
selection_source: conference_retrieval
motivation: 后门攻击触发器同时存在于训练与测试集且目标双重，其泛化界此前尚未建立。
method: 在干净标签场景下推导与算法无关的泛化界，给出干净样本误差上界并据此提出新攻击算法。
result: 理论上建立了干净标签后门的泛化界，并验证了新算法在攻击效果上的有效性。
conclusion: 该工作填补了后门攻击泛化理论的空白，并为干净标签后门设计提供理论指导。
---

## Abstract
The generalization bound is a crucial theoretical tool for assessing the generalizability of learning methods and there exist vast literatures on generalizability of normal learning, adversarial learning, and data poisoning. Unlike other data poison attacks, the backdoor attack has the special property that the poisoned triggers are contained in both the training set and the test set and the purpose of the attack is two-fold. To our knowledge, the generalization bound for the backdoor attack has not been established. In this paper, we fill this gap by deriving algorithm-independent generalization bounds in the clean-label backdoor attack scenario. Precisely, based on the goals of backdoor attack, we give upper bounds for the clean sample population errors and the poison population errors in terms of the empirical error on the poisoned training dataset. Furthermore, based on the theoretical result, a new clean-label backdoor attack is proposed that computes the poisoning trigger by combining adversarial noise and indiscriminate poison. We show its effectiveness in a variety of settings.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
干净标签后门攻击的泛化界与新算法。

### 2. 核心内容
后门攻击具有触发器同时出现在训练与测试集、目标双重的特殊性质，但其泛化界此前尚未建立，而泛化界是评估学习方法可泛化性的关键工具。作者在干净标签后门场景下推导出与算法无关的泛化界，给出干净样本总体误差的上界。基于该理论，论文进一步提出新的后门攻击算法，为理解与设计干净标签后门提供了理论支撑。

### 3. 对应检索需求
clean-label backdoor。

### 4. 来源与原文
- Source：ICML-2024-Public
- OpenReview：[https://openreview.net/forum?id=ZdqiT0McON](https://openreview.net/forum?id=ZdqiT0McON)
