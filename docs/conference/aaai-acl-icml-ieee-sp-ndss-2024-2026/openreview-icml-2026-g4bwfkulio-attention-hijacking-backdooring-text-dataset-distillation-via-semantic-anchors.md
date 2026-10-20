---
title: "Attention Hijacking: Backdooring Text Dataset Distillation via Semantic Anchors"
title_zh: 注意力劫持：通过语义锚点对文本数据集蒸馏植入后门
authors: "Hang Ren, Xin Wang, Tong Yue, Wen Chen, Junqing Le"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/5e872c4bc3e7bcd26cde363eac8ba86fedc7f69f.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 通过注意力劫持对文本数据集蒸馏实施隐蔽后门攻击
tldr: 数据集蒸馏可将大规模数据压缩为紧凑合成集，但其在文本分类领域的安​全隐患尚未被充分研究。作者识别出蒸馏注意力标签这一被忽视的脆弱点，提出注意力劫持攻击，通过操纵双层优化过程用合成数据劫持目标模型的注意力机制。该攻击在不损害干净精度的前提下实现隐蔽后门，揭示了蒸馏范式中新的安全风险。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 文本数据集蒸馏的安全隐患尚未被充分研究，存在被忽视的脆弱点。
method: 提出注意力劫持攻击，操纵双层优化过程，用合成数据劫持目标模型注意力机制。
result: 实验表明该攻击在不降低干净精度的前提下成功植入隐蔽后门，隐蔽性强。
conclusion: 该工作揭示了数据集蒸馏范式中被忽视的注意力层后门风险。
---

## Abstract
Dataset Distillation has emerged as a promising technique for compressing large-scale datasets into compact synthetic sets while preserving model performance. However, the security implications of this paradigm, particularly within the Transformer-based text classification domain, remain underexplored. In this paper, we identify "Distilled Attention Labels" as a pivotal yet overlooked vulnerability. We propose Attention Hijacking (AH), a stealthy backdoor attack that manipulates the bi-level optimization process to explicitly hijack the attention mechanism of target models via synthetic data. Distinct from traditional poisoning that often compromises clean accuracy, AH achieves stealthiness without utility degradation. To explain this, we formulate the "Semantic Anchoring Hypothesis", characterizing the interaction between trigger semantics and attack mechanisms. We demonstrate that AH functions as a semantic-adaptive mechanism: when triggers align with domain-specific semantic anchors (e.g., "film" in sentiment analysis), our method achieves a synergistic effect, boosting both attack success rates (>99%) and clean test accuracy. Conversely, for functional or noise triggers, AH enforces attention segregation to prevent utility collapse, maintaining exceptional robustness where baseline attacks fail. Extensive experiments across multiple datasets and varying model scales—ranging from BERT-Tiny to BERT-Base—validate the scalability and dominance of AH. Our findings reveal that attention-based distillation is a double-edged sword, underscoring the urgent need for robust defenses in the era of data-efficient learning.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
通过注意力劫持对文本数据集蒸馏实施隐蔽后门攻击。

### 2. 核心内容
数据集蒸馏可将大规模数据压缩为紧凑合成集，但其在文本分类领域的安​全隐患尚未被充分研究。作者识别出蒸馏注意力标签这一被忽视的脆弱点，提出注意力劫持攻击，通过操纵双层优化过程用合成数据劫持目标模型的注意力机制。该攻击在不损害干净精度的前提下实现隐蔽后门，揭示了蒸馏范式中新的安全风险。

### 3. 对应检索需求
stealthy backdoor attacks。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=g4BWfkULIo](https://openreview.net/forum?id=g4BWfkULIo)
