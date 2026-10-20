---
title: A Theoretical Analysis of Backdoor Poisoning Attacks in Convolutional Neural Networks
title_zh: 卷积神经网络中后门投毒攻击的理论分析
authors: "Boqi Li, Weiwei Liu"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=SfcB4cVvPz"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 后门投毒攻击的理论分析
tldr: 后门投毒攻击虽广泛威胁深度神经网络，但其奏效机理仍缺乏理论解释。作者以两层卷积网络为对象，采用脏标签攻击对后门投毒过程进行细致理论分析，给出攻击有效性的理论洞见。并在两个真实数据集上开展实验加以验证。该工作为理解后门攻击机制和设计防御提供了理论支撑。
source: ICML-2024-Public
selection_source: conference_retrieval
motivation: 后门投毒攻击威胁严重，但为何有效仍缺乏理论解释。
method: 以两层卷积网络为例，用脏标签攻击对后门投毒进行理论分析并辅以实验。
result: 给出后门攻击有效性的理论洞见，并在两个真实数据集上验证结论。
conclusion: 为理解后门攻击机理及设计防御方法提供了理论依据。
---

## Abstract
The rising threat of backdoor poisoning attacks (BPAs) on Deep Neural Networks (DNNs) has become a significant concern in recent years. In such attacks, the adversaries strategically target a specific class and generate a poisoned training set. The neural network (NN), well-trained on the poisoned training set, is able to predict any input with the trigger pattern as the targeted label, while maintaining accurate outputs for clean inputs. However, why the BPAs work remains less explored. To fill this gap, we employ a dirty-label attack and conduct a detailed analysis of BPAs in a two-layer convolutional neural network. We provide theoretical insights and results on the effectiveness of BPAs. Our experimental results on two real-world datasets validate our theoretical findings.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
后门投毒攻击的理论分析。

### 2. 核心内容
后门投毒攻击虽广泛威胁深度神经网络，但其奏效机理仍缺乏理论解释。作者以两层卷积网络为对象，采用脏标签攻击对后门投毒过程进行细致理论分析，给出攻击有效性的理论洞见。并在两个真实数据集上开展实验加以验证。该工作为理解后门攻击机制和设计防御提供了理论支撑。

### 3. 对应检索需求
backdoor attacks。

### 4. 来源与原文
- Source：ICML-2024-Public
- OpenReview：[https://openreview.net/forum?id=SfcB4cVvPz](https://openreview.net/forum?id=SfcB4cVvPz)
