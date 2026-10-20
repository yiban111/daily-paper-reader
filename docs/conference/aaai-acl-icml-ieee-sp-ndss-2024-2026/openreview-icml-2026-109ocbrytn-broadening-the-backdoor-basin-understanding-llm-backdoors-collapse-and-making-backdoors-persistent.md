---
title: "Broadening the Backdoor Basin: Understanding LLM Backdoors Collapse and Making Backdoors Persistent"
title_zh: 拓宽后门盆地：理解大语言模型后门坍缩并使其持久化
authors: "Xingyi Zhao, Tian Xie, Xiaojun Qi, Depeng Xu, Shuhan Yuan"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/92d1b80b418bb7f627a2546ae53bbc5dcee211fd.pdf"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 能在微调后存活的针对大语言模型的持久后门攻击
tldr: 针对大语言模型后门在下游监督微调后常被遗忘的问题，本文从几何角度给出解释：传统投毒将后门损失压入狭窄尖锐的盆地，微小的参数漂移即可使模型脱离高攻击成功率区域。据此提出BAD-BOOM，通过更宽的平滑最小化显式拓宽并平滑后门盆地。实验表明该攻击在微调后仍能保持较高攻击成功率，为构建持久后门提供了新方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 大语言模型后门常在下游监督微调后失效，其机制尚不清晰。
method: 从几何上揭示后门损失盆地狭窄尖锐，并提出BAD-BOOM通过平滑最小化拓宽盆地。
result: 实验显示该攻击在微调后仍保持较高攻击成功率，后门更持久。
conclusion: 拓宽后门盆地几何结构是提升后门鲁棒性的有效途径。
---

## Abstract
Large Language Models (LLMs) are vulnerable to backdoor attacks, yet we observe that many LLM backdoors do not survive when end users perform supervised fine-tuning (SFT). In this work, we provide a geometric explanation: by probing the backdoor objective under controlled weight perturbations, we find that conventional poisoning often drives the backdoor loss to a narrow and sharp basin; consequently, even modest parameter drift induced by downstream SFT can push the model out of the low-loss and high-ASR region, leading to rapid backdoor forgetting. Motivated by this insight, we propose BAD-BOOM, a resilient backdoor attack via broader smoothness minimization, which explicitly broadens and smooths the backdoor basin. BAD-BOOM extends sharpness-aware minimization with a Fisher-induced ellipsoidal constraint that allocates larger perturbation budgets to backdoor-sensitive parameters, encouraging solutions whose neighborhoods also maintain low backdoor loss. Across two threat settings, three attack scenarios, three open-source LLMs, and three trigger-free downstream SFT tasks, BAD-BOOM consistently preserves high ASR while maintaining competitive utility. The code is available at https://github.com/xingyizhao/BAD-BOOM.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
能在微调后存活的针对大语言模型的持久后门攻击。

### 2. 核心内容
针对大语言模型后门在下游监督微调后常被遗忘的问题，本文从几何角度给出解释：传统投毒将后门损失压入狭窄尖锐的盆地，微小的参数漂移即可使模型脱离高攻击成功率区域。据此提出BAD-BOOM，通过更宽的平滑最小化显式拓宽并平滑后门盆地。实验表明该攻击在微调后仍能保持较高攻击成功率，为构建持久后门提供了新方法。

### 3. 对应检索需求
Backdoor attacks on large language models。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=109ocBrytN](https://openreview.net/forum?id=109ocBrytN)
