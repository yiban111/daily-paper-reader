---
title: "Injection, Attack and Erasure: Revocable Backdoor Attacks via Machine Unlearning"
title_zh: 注入、攻击与消除：基于机器遗忘的可撤销后门攻击
authors: "Baogang Song, Dongdong Zhao, Jianwen Xiang, Qiben Xu, Zizhuo Yu"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39747/43708"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 借助机器遗忘的可撤销隐蔽后门攻击与双层触发优化
tldr: 现有后门攻击即使借助遗忘机制增强隐蔽性，仍会留下可被静态分析检测的痕迹。本文提出首个可撤销后门攻击范式，将触发器优化建模为双层优化，同时模拟注入与遗忘过程。实验表明该方法在保持高攻击成功率的同时实现后门的主动彻底移除。其贡献在于提升了后门攻击的隐蔽性与可控性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有后门攻击即便借助遗忘机制仍留有可被静态分析发现的持久痕迹。
method: 将触发器优化建模为双层优化，联合模拟后门注入与机器遗忘过程。
result: 在保持较高攻击成功率的同时实现后门的主动、彻底移除。
conclusion: 提出可撤销后门攻击范式，提升了攻击的隐蔽性与可控性。
---

## Abstract
Backdoor attacks pose a persistent security risk to deep neural networks (DNNs) due to their stealth and durability. While recent research has explored leveraging model unlearning mechanisms to enhance backdoor concealment, existing attack strategies still leave persistent traces that may be detected through static analysis. In this work, we introduce the first paradigm of revocable backdoor attacks, where the backdoor can be proactively and thoroughly removed after the attack objective is achieved. We formulate the trigger optimization in revocable backdoor attacks as a bilevel optimization problem: by simulating both backdoor injection and unlearning processes, the trigger generator is optimized to achieve a high attack success rate (ASR) while ensuring that the backdoor can be easily erased through unlearning. To mitigate the optimization conflict between injection and removal objectives, we employ a deterministic partition of poisoning and unlearning samples to reduce sampling-induced variance, and further apply the Projected Conflicting Gradient (PCGrad) technique to resolve the remaining gradient conflicts. Experiments on CIFAR-10 and ImageNet demonstrate that our method maintains ASR comparable to state-of-the-art backdoor attacks, while enabling effective removal of backdoor behavior after unlearning. This work opens a new direction for backdoor attack research and presents new challenges for the security of machine learning systems.

---

## 论文详细总结（自动生成）

# 论文结构化总结：基于机器遗忘的可撤销后门攻击

## 1. 核心问题与整体含义（研究动机与背景）

- **后门攻击的持久风险**：后门攻击因隐蔽性强、难以根除，对深度神经网络（DNN）构成持续安全威胁。传统后门一旦注入，其特征会长期驻留于模型参数或激活空间中，防御方可借助模型剪枝（Fine-pruning）、触发器模式反演（Neural Cleanse）、激活聚类等静态分析方法，在模型生命周期的任意阶段检测出后门痕迹。
- **既有“利用遗忘”的攻击仍不彻底**：近期研究（如 Zhang et al. 2023；Liu et al. 2024；Huang et al. 2024）尝试滥用机器遗忘机制实现“延迟激活”式后门，以提升部署期的隐蔽性；但这些方法只解决了“何时激活”的问题，并未消除后门痕迹本身，攻击者仍面临被动暴露的风险。
- **核心研究问题**：攻击者能否在达成攻击目标后，**主动、彻底地撤销后门**，从而擦除攻击痕迹、规避长期检测与追踪？并且这种撤销应只需遗忘极少量样本，以免在常规模型管理或合规审计中引起怀疑。
- **范式意义**：论文提出首个“可撤销后门攻击”（Revocable Backdoor Attack）范式，指出其带来两重新挑战——**归因困难**（攻击者可在审计前移除关键样本使模型表现正常）与**静态检测失效**（依赖后门持久存在的静态检测工具将失效，迫使防御转向持续在线监控与行为日志审计）。
- **与已有“可撤销后门”工作的区别**：Xu et al. (2024) 虽声称“可撤销”，但局限于模型交易场景且需修改模型参数；本文则从攻击者视角出发，借助合法遗忘接口实现主动、通用的后门移除。

## 2. 方法论

### 2.1 核心思想

- 将触发器优化建模为**双层优化（bilevel optimization）**问题：在优化触发器时**同时模拟后门注入与遗忘两个过程**，使学到的触发器既能在注入后高成功率激活后门，又能在遗忘操作后被彻底抹除。
- 采用**交替优化**框架：每轮先固定触发器生成器、训练后门模型与遗忘后模型，再固定模型、优化生成器，以缓解二者强耦合导致的收敛困难。
- 使用**代理（影子）模型**模拟受害者模型，因为攻击者无法访问受害者模型内部或干预其训练。

### 2.2 攻击三阶段流程

1. **触发器优化**：设计并训练触发器生成器，兼顾攻击有效性与可撤销性。
2. **后门注入**：将触发器样本（干净标签）混入训练集并提交训练，得到后门模型。
3. **后门撤销**：攻击完成后提交对少量含触发器样本的遗忘请求，通过合法遗忘接口移除后门。

### 2.3 触发器生成器设计

- 基础形式：`G(x) = x + η · g(x)`，其中 `g` 为可学习生成网络（U-Net 骨干），`η` 为扰动强度。
- 借鉴 COMBAT 的频域约束以抑制高频伪影与空间不连续：
  - 对 `g(x)` 做离散余弦变换（DCT），用掩码 `m` 保留低频分量（频率掩码比 `r = 0.65`），再经逆 DCT（IDCT）重建；
  - 对生成触发器施加高斯模糊滤波 `k`（核大小 3，σ 从 [0.1, 1] 采样）。
  - 形式化：`G(g(x)) = IDCT(m ⊙ DCT(g(x)))`，`G(x) = (x + η · G(g(x))) * k`。

### 2.4 双层优化目标

- **外层**（优化生成器）包含四项损失：
  - **攻击损失** `L_attack = E[L(f_θb(G(x)), y_target)]`：对全数据集施加触发器并强制预测目标标签，鼓励触发器泛化。
  - **遗忘损失** `L_unlearn = E[L(f_θu(G(x)), y)]`：要求遗忘后模型对触发器样本输出真实标签，促使后门可被撤销。
  - **可见性损失** `L_vis = E[|η · G(g(x))|²₂]`：惩罚视觉显著扰动。
  - **非对抗损失** `L_non-adv = E[L(f_θclean(G(x)), y)]`：约束干净模型对触发样本与干净样本预测一致，防止触发器退化为普通对抗扰动。
- **内层**：`θb = argmin E[L(f_θ(x), y)]`（在混合训练集 `Db` 上训练后门模型），`θu = F_unlearn(θb, U)`（对遗忘集 `U` 执行遗忘）。
- 总目标：`G* = argmin E[L_attack + λ_unlearn·L_unlearn + λ_vis·L_vis + λ_non-adv·L_non-adv]`，受上述内层约束。

### 2.5 梯度冲突缓解

- **问题**：提升后门模型 ASR 与降低遗忘后模型 ASR 本质冲突，二者梯度方向常呈显著负相关，严重时相互抵消。
- **机制一：确定性划分（Deterministic Partition）**——固定中毒样本与遗忘样本的划分，更贴合真实攻击设定，减少随机采样方差，稳定优化。
- **机制二：PCGrad 梯度投影**——当攻击损失梯度 `gb` 与遗忘损失梯度 `gu` 内积为负时，将 `gu` 正交投影到 `gb` 方向：`g_u^PC = gu − α · (⟨gb, gu⟩ / ‖gb‖²) · gb`，其中 `α` 控制投影幅度（实验取 0.6）。

### 2.6 威胁模型要点

- 攻击者能力：可注入/修改少量训练样本，可利用遗忘接口请求删除指定小样本子集，但不能干预整体训练与部署。
- **干净标签攻击**：所有中毒样本保留真实标签，原因有二——现实性更强，且避免在撤销阶段因请求遗忘错标样本而引发服务方怀疑。

## 3. 实验设计

### 3.1 数据集与模型

- **数据集**：CIFAR-10；IMAGENET-10（从标准 ImageNet 随机选取 10 类构建）。
- **模型**：CIFAR-10 使用 Pre-activation ResNet-18，IMAGENET-10 使用标准 ResNet-18；触发器生成器统一使用 U-Net 骨干。
- **训练配置**：200 epochs，SGD，batch size 128（CIFAR-10）/ 32（IMAGENET-10），初始学习率 0.01（CIFAR-10）/ 0.001（IMAGENET-10）/ 0.01（生成器），第 100、150 epoch 衰减 10 倍；目标标签统一为类别 0；中毒率固定 5%。

### 3.2 遗忘设置

- 两种遗忘近似方法：**First-Order** 与 **UnrollSGD**。
- 每次遗忘移除样本数：CIFAR-10 为 250 个、IMAGENET-10 为 100 个；遗忘率分别为 0.01 与 0.001；UnrollSGD 下 CIFAR-10 微调 3 epoch、IMAGENET-10 微调 2 epoch。
- 默认实验（除非特别说明）在 CIFAR-10 上进行，模拟与评估均采用 First-Order。

### 3.3 对比基线

- BadNets、WaNet、Sleeper Agent、COMBAT 四种主流后门攻击方法，在相同设置下评估其遗忘前后的表现。
- 由于尚无公开的“可撤销后门攻击”专门基线，作者以主流攻击方法作为参照。

### 3.4 评估指标

- **ASR**（攻击成功率）、**ASR-U**（遗忘后攻击成功率）、**BA**（干净样本准确率）、**BA-U**（遗忘后干净样本准确率），并以 ∆ = ASR-U − ASR 衡量可撤销性。

### 3.5 实验类型

- 主攻击实验（表 1）：两数据集 × 两遗忘方法。
- 与 SOTA 对比（表 2）：CIFAR-10 上五种方法 × 两遗忘方法。
- 防御实验（图 3）：Fine-pruning 剪枝防御。
- 参数敏感性（表 3、表 4）：中毒率 ρp（5 档）与触发器强度 η（5 档）。
- 梯度冲突分析（图 4）：训练过程中攻击
