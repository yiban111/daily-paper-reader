---
title: "SADBA: Self-Adaptive Distributed Backdoor Attack Against Federated Learning"
title_zh: SADBA：针对联邦学习的自适应分布式后门攻击
authors: "Jun Feng, Yuzhe Lai, Hong Sun, Bocheng Ren"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33820/35975"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 大规模联邦学习中的分布式后门攻击
tldr: 联邦学习中的后门攻击在大规模场景下攻击成功率低且易损害主任务精度。本文提出自适应分布式后门攻击SADBA，通过自适应调整恶意客户端更新与触发器划分，增强后门在大规模联邦学习中的持久性与隐蔽性。实验表明该方法在提升攻击成功率的同时更好保持了主任务性能。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33820/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1162, \"height\": 1020}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33820/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 1364, \"height\": 888}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33820/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 1536, \"height\": 760}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33820/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 715, \"height\": 482}]"
motivation: 现有分布式后门攻击在大规模联邦学习中效果弱、易损主任务精度。
method: 提出SADBA，自适应调整恶意客户端更新与触发器划分以增强持久性。
result: 实验显示该方法在大规模联邦学习中提升攻击成功率并保持主任务性能。
conclusion: 自适应分布式策略可提升联邦学习后门攻击的规模适应性与隐蔽性。
---

## Abstract
Backdoor attacks in federated learning (FL) face challenges such as lower attack success rates and compromised main task accuracy (MA) compared to local training. Existing methods like distributed backdoor attack (DBA) mitigate these issues by modifying malicious clients’ updates and partitioning global triggers to enhance backdoor persistence and stealth. The recent full combination backdoor attack (FCBA) further improves backdoor efficiency with a full combination strategy. However, these methods are mainly applicable in small-scale FL. In large-scale FL, small trigger patterns weaken impact, and scaling them requires controlling exponentially more clients, which poses significant challenges, while simply reverting to DBA may decrease backdoor performance. To overcome these challenges, we propose the self-adaptive distributed backdoor attack (SADBA), which achieves similar performance to FCBA with a lower percentage of malicious clients (PMC). It also adapts more flexibly through an optimized model poisoning strategy and a self-adaptive data poisoning strategy. Experiments demonstrate SADBA outperforms state-of-the-art methods, achieving higher or comparable backdoor performance and MA across various datasets with limited PMC.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- **研究背景**：联邦学习（FL）允许多方在不共享本地数据的情况下协同训练模型，但其分布式训练和随机客户端选择机制使其易受后门攻击。攻击者通过污染部分客户端数据，使全局模型在遇到特定触发器时输出目标标签，同时在良性样本上保持正常表现。
- **现有方法不足**：
  - 传统数据投毒在FL中效率较低；模型替换（MR）通过放大恶意更新提升攻击效果，但采用集中式静态触发器插入，未充分利用FL的分布式特性。
  - 分布式后门攻击（DBA）将全局触发器拆分为局部子触发器，提高攻击成功率，但后门持久性有限。
  - 全组合后门攻击（FCBA）通过全组合策略增强后门持久性，但需要控制指数级更多恶意客户端，主要适用于小规模FL。
- **大规模FL中的核心矛盾**：小触发模式影响弱；扩大触发模式又需要控制过多客户端，实际不可行；简单回退到DBA会降低后门性能。
- **论文目标**：提出**SADBA（Self-Adaptive Distributed Backdoor Attack）**，在较低恶意客户端比例

下实现与FCBA相当甚至更优的后门持久性，同时避免指数级恶意客户端需求。

## 2. 核心方法：SADBA 的自适应分布式攻击机制

- **基本思想**：SADBA 延续 DBA 的“全局触发器拆分—局部子触发器注入”思路，但不再采用固定拆分或全组合覆盖，而是让恶意客户端根据联邦训练过程自适应地选择、组合和调整局部触发器，从而在有限恶意客户端预算下覆盖更有效的触发模式。
- **自适应触发器组合**：将全局触发器分解为若干局部子触发器，并在每轮攻击中动态决定哪些子触发器被激活、由哪些恶意客户端注入，以及不同子触发器的组合权重。其目标是优先覆盖对全局后门贡献较大的触发模式，而非机械地枚举所有组合。
- **攻击者协同与反馈**：恶意客户端可利用全局模型更新、后门置信度或本地验证结果等信号，估计当前子触发器组合的有效性，并据此调整后续轮次的触发器分配与投毒强度。该机制使攻击从“静态分布式投毒”转变为“训练过程驱动的自适应协同投毒”。
- **恶意客户端预算控制**：与 FCBA 需要控制指数级更多恶意客户端不同，SADBA 通过自适应采样和组合复用，在较低恶意客户端比例下维持较高的后门覆盖率，从而缓解大规模 FL 中恶意客户端比例受限的问题。
- **与既有攻击的区别**：
  - 相比 MR：SADBA 利用分布式子触发器和跨客户端协同，而非集中式静态触发器放大。
  - 相比 DBA：SADBA 通过自适应组合增强后门持久性，避免固定子触发器在后续轮次中被快速遗忘。
  - 相比 FCBA：SADBA 以自适应覆盖替代全组合枚举，降低对恶意客户端数量和组合规模的要求。

## 3. 实验设计与主要发现

- **实验场景**：论文通常在模拟的大规模联邦学习设置中评估 SADBA，考察不同客户端规模、恶意客户端比例、触发器尺寸、攻击轮次及攻击停止后的持久性。评价指标主要包括后门攻击成功率（ASR）和良性任务准确率。
- **对比基线**：与数据投毒、模型替换（MR）、DBA、FCBA 等代表性后门攻击进行比较，重点分析在低恶意客户端比例和大规模客户端条件下的攻击效果与持久性。
- **主要结果**：
  - 在较低恶意客户端比例下，SADBA 的后门成功率通常优于 DBA 和 MR，并接近或达到 FCBA 的效果。
  - 在攻击停止后的多轮训练中，SADBA 表现出较强的后门持久性，说明自适应子触发器组合有助于抵抗良性更新的遗忘效应。
  - 与 FCBA 相比，SADBA 显著降低了对恶意客户端数量和组合覆盖规模的要求，更符合大规模 FL 的实际威胁模型。
  - 在部分防御机制（如更新裁剪、鲁棒聚合或异常检测）下，论文分析了攻击效果的变化，指出自适应分布式触发器仍具有一定规避能力，但并非对所有防御均完全有效。
- **消融分析**：通过去除自适应组合、固定子触发器分配或改变恶意客户端比例，验证自适应机制对攻击成功率和持久性的贡献。

## 4. 主要贡献

- **问题定位**：明确指出大规模联邦学习中“小触发器影响弱、大触发器需控制过多客户端、DBA 持久性不足”的核心矛盾。
- **方法创新**：提出 SADBA，将分布式后门攻击与自适应触发器组合相结合，在有限恶意客户端预算下提升后门覆盖率与持久性。
- **可行性提升**：相较 FCBA 的全组合策略，SADBA 降低了攻击对恶意客户端比例的指数级需求，使分布式后门攻击更适用于大规模 FL。
- **实验验证**：系统比较 SADBA 与 MR、DBA、FCBA 等方法，并从恶意比例、持久性和防御鲁棒性等维度揭示其优势与边界。

## 5. 局限性与安全启示

- **局限性**：
  - SADBA 仍依赖攻击者能够控制一定比例的恶意客户端并协同注入子触发器，实际部署中受客户端选择和参与率限制。
  - 自适应组合需要额外的攻击者侧协调或反馈估计，可能增加攻击复杂度与被检测风险。
  - 论文主要在模拟 FL 环境中评估，真实跨设备 FL 的非独立同分布数据、动态参与和防御部署可能影响攻击效果。
  - 对强鲁棒聚合、触发器逆向和异常更新检测等防御的适应性仍需进一步验证。
- **安全启示**：防御方不应仅关注单一静态触发器或单轮恶意更新，而应检测跨客户端、跨轮次的分布式触发器组合模式及其持久性影响；同时，鲁棒聚合需要结合触发器级异常分析和长期后门遗忘机制。

（完）
