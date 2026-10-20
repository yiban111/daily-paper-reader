---
title: "Safety-Efficacy Trade Off: Robustness against Data-Poisoning"
title_zh: 安全-效力权衡：对数据投毒的鲁棒性
authors: "Diego Marco Granziol, Ulugbek Abdimanabov"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/5b98841cf6fdbf4e33ac8e258f457e2e4b9d0233.pdf"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 数据投毒鲁棒性的几何分析
tldr: 后门与数据投毒攻击常能规避谱与优化类防御，其机理尚不明确。本文以核岭回归为宽网络精确模型，证明聚类脏标签投毒会在输入Hessian中产生秩一尖峰，并揭示非线性核下效力与曲率解耦的近似克隆区间。分析给出了攻击可检测性的几何判据。该工作为理解与设计投毒防御提供理论基础。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 后门与数据投毒攻击能规避现有防御，其内在机理缺乏理论解释。
method: 以核岭回归为宽网络模型，从输入空间几何分析投毒引发的Hessian与曲率变化。
result: 证明聚类脏标签投毒产生可检测的秩一尖峰，并揭示可规避检测的近似克隆区间。
conclusion: 该工作为投毒攻击的可检测性与防御设计提供了理论依据。
---

## Abstract
Backdoor and data-poisoning attacks can achieve high attack success while evading existing spectral and optimisation-based defences. We show that this behaviour is not incidental, but arises from a fundamental geometric mechanism in input space. Using kernel ridge regression as an exact model of wide neural networks, we prove that clustered dirty-label poisons induce a rank-one spike in the input Hessian whose magnitude scales quadratically with attack efficacy. Crucially, for nonlinear kernels we identify a near-clone regime in which poison efficacy remains order-one while the induced input curvature vanishes, making the attack provably spectrally undetectable. We further show that input-gradient regularisation contracts poison-aligned Fisher and Hessian eigenmodes under gradient flow, yielding an explicit and unavoidable safety–efficacy trade-off by reducing data-fitting capacity. For exponential kernels, this defence admits a precise interpretation as an anisotropic high-pass filter that increases the effective length scale and suppresses near-clone poisons. Extensive experiments on linear models and deep convolutional networks across MNIST and CIFAR-10/100 validate the theory, demonstrating consistent lags between attack success and spectral visibility, and showing that regularisation and data augmentation jointly suppress poisoning. Our results establish when backdoors are inherently invisible, and provide the first end-to-end characterisation of poisoning, detectability, and defence through input-space curvature.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
数据投毒鲁棒性的几何分析。

### 2. 核心内容
后门与数据投毒攻击常能规避谱与优化类防御，其机理尚不明确。本文以核岭回归为宽网络精确模型，证明聚类脏标签投毒会在输入Hessian中产生秩一尖峰，并揭示非线性核下效力与曲率解耦的近似克隆区间。分析给出了攻击可检测性的几何判据。该工作为理解与设计投毒防御提供理论基础。

### 3. 对应检索需求
Novel data poisoning and clean label backdoor attacks against deep neural networks.

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=suwjabuQDY](https://openreview.net/forum?id=suwjabuQDY)
