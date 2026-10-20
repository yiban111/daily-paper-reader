---
title: Influence-Based Fair Selection for Sample-Discriminative Backdoor Attack
title_zh: 基于影响力公平选择的样本判别式后门攻击
authors: "Qi Wei, Shuo He, Jiahan Zhang, Lei Feng, Bo An"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/35449/37604"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 基于样本判别与隐蔽触发器的后门攻击
tldr: 针对触发器强度被限制以保持隐蔽时各类别攻击成功率严重不均的问题，本文提出基于影响力公平选择（IFS）的后门攻击方法，通过选择对攻击贡献更大的样本来均衡各类攻击效果。实验表明该方法在保持触发器隐蔽的同时显著提升了攻击成功率的均衡性与整体表现，为样本选择式后门攻击提供了新思路。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35449/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 1120, \"height\": 840}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35449/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 1118, \"height\": 1680}]"
motivation: 现有后门攻击在触发器极隐蔽时各类别攻击成功率差异悬殊，影响攻击稳定性。
method: 提出基于影响力公平选择（IFS）的攻击方法，按影响力挑选样本并兼顾类别均衡。
result: 实验显示该方法在隐蔽触发器约束下提升了攻击成功率的均衡性与整体效果。
conclusion: 样本选择策略能有效缓解隐蔽后门攻击中的类别失衡问题。
---

## Abstract
Backdoor attacks have posed a serious threat in machine learning models, wherein adversaries can poison training samples with maliciously crafted triggers to compromise the victim model. Advanced backdoor attack methods have focused on selectively poisoning more vulnerable training samples, achieving a higher attack success rate (ASR). However, we found that when the manipulation strength of the trigger is constrained to a very small value for imperceptible attacks, they suffer from extremely uneven class-wise ASR due to the unequal selection of instances per class. To solve this issue, we propose a novel backdoor attack method based on Influence-based Fair Selection (IFS), including two objectives: 1) selecting samples that significantly contribute to ASR and 2) ensuring class balance during the selection process. Specifically, we adapt Influence Functions, a classic technique in robust statistics, to evaluate the influence of trigger-embedded training samples on ASR. In this case, training samples contributing to reducing the backdoored test risk could possess higher influence scores. Further, a group-based pruning strategy is designed to avoid calculating the influence on ASR for all training samples, thereby significantly reducing the computational cost. Then, based on the influence score, we design an adaptive thresholding scheme to dynamically select samples with higher influence while maintaining class balance. Extensive experiments on four datasets verify the effectiveness of IFS compared with advanced methods.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- **背景**：后门攻击通过在训练样本中植入触发器，使受害模型在遇到触发模式时输出攻击者指定标签。现有方法通常只污染约 1% 的训练样本，并追求触发器视觉上不可察觉（操纵强度 \(\epsilon\) 很小）。
- **关键发现**：当触发器操纵强度 \(\epsilon\) 被压得很小时，现有样本选择式后门攻击（如 FUS、RD）会出现严重的**类别级 ASR 不均衡**：某些类别攻击成功率高，另一些类别很低。论文引入“类别级 ASR 方差”指标，发现 \(\epsilon\) 越小，各类别被选中的后门样本数量越不均衡，ASR 方差越大。
- **核心问题**：如何在低污染率、低触发器强度条件下，选择对 ASR 贡献大且类别分布公平的后门训练样本，从而提升整体攻击成功率并降低类别间差异。
- **整体含义**：论文将后门样本选择从“经验性评分”推进到基于影响函数（Influence Function, IF）的可解释选择，并强调类别公平性对隐蔽后门攻击效果的重要性。

## 2. 方法论：Influence-based Fair Selection（IFS）

### 2.1 核心思想

- 使用影响函数估计“给某个训练样本植入触发器后，对后门验证集损失/ASR 的影响”。
- 优先选择对降低后门测试风险、提升 ASR 影响更大的样本。
- 为避免逐样本计算影响函数的高昂成本，先按类别原型距离分组剪枝，只在小候选集上计算影响。
- 通过类级动态阈值选择样本，保证每个类别选出相同数量的后门样本，从而缓解类别级 ASR 不均衡。

### 2.2 关键技术细节

- **后门样本构造**：  
  \[
  x' = M(x,t,\epsilon)=x\odot(1-\epsilon)+t\odot\epsilon
  \]
  其中 \(t\) 为触发器，\(\epsilon\) 为操纵强度。污染率为 \(r=M/N\)。
- **影响函数**：对训练样本 \(z_i\) 和后门验证样本 \(z'_u\)，影响分数近似为：
  \[
  \phi_i \approx -\left[\nabla_\theta \frac{1}{U}\sum_u \ell(z'_u,\hat\theta)\right]^\top H_{\hat\theta}^{-1}\nabla_\theta \ell(z_i,\hat\theta)
  \]
  使用 IHVP 和 LiSSA 近似逆 Hessian 向量积，避免显式计算 Hessian 逆。
- **分组剪枝**：
  - 对每个类别计算类原型 \(v_c\)：该类所有样本特征向量的均值。
  - 计算每个样本到类原型的欧氏距离，并按距离排序分成 \(\eta\) 组。
  - 只保留距离原型最近的 Group 1 作为候选集 \(D_{sub}\)，即 \(D_{sub}=N/\eta\)。
  - 动机：靠近类原型的代表性样本被植入触发器后，对 ASR 的影响更大。
- **类级公平选择**：
  - 对每个类别 \(c\)，根据影响分数集合 \(I_c\) 计算类级阈值：
    \[
    \tau_c = \mathrm{Quantile}(\mathrm{Sort}_{\downarrow}(I_c), \eta r)
    \]
  - 选择影响分数大于 \(\tau_c\) 的样本，使每类选出约 \(N/C\cdot r\) 个样本，实现类别均衡。
- **算法流程**：
  1. 随机抽取初始后门集并训练模型；
  2. 重复 \(T\) 轮：按类分组剪枝得到 \(D_{sub}\)；计算候选样本影响分数；计算类级阈值；选择后门样本；
  3. 在更新后的污染训练集上重训模型至收敛；
  4. 输出最终后门训练集。
- **复杂度**：主要成本来自影响函数计算与多轮重训；分组剪枝将候选集缩小到 \(N/\eta\)，论文称通常只需计算少于 10% 样本的影响分数，但实际比例取决于 \(\eta\)（如 \(\eta=5\) 时为 20%）。

## 3. 实验设计

- **数据集/场景**：
  - 2D 图像分类：CIFAR-10、ImageNet-10、Raf-db。
  - 3D 点云分类：ModelNet40。
- **攻击类型**：
  - Blended attack；
  - Patched attack。
- **模型设置**：
  - 2D：VGG16 作为 surrogate/代理模型，ResNet18 作为 target/受害模型。
  - 3D：PointNet 作为 surrogate，PointNet++ 作为 target。
- **主要实验维度**：
  - 不同操纵强度 \(\epsilon\)：patched 攻击约 1–4，blended 攻击约 0.05–0.15/0.20。
  - 不同污染率 \(r\)：0.005、0.008、0.01、0.015。
  - 类别级 ASR 方差与 class-wise ASR 分析。
- **对比方法**：
  - Random Search（RS）；
  - FUS（基于 forgetting score）；
  - RD（基于 representational distance score）；
  - HFE（基于高频能量特征的选择方法，作者自行复现）。
- **消融与敏感性实验**：
  - 消融 group split 与 fair selection 两个组件；
  - 超参数 \(\eta\) 敏感性；
  - 不同训练策略和不同 surrogate/victim 模型组合；
  - 类别级 ASR 方差对比。
- **评价指标**：主要报告 ASR；同时分析类别级 ASR 方差、计算成本。论文未将 clean accuracy 或防御检测率作为主要报告指标。

## 4. 资源与算力

- 论文明确提到实验在 **NVIDIA GTX 3090 GPUs** 上进行，所有实验重复三次并报告平均值。
- 未明确说明 GPU 数量、单次训练/搜索的总时长、总 GPU 小时数或能耗。
- 未给出完整的算力预算表，因此无法准确估计总计算开销；只能从方法流程看出需要 \(T\) 轮样本搜索与重训，计算成本高于一次性选择方法。

## 5. 实验数量与充分性

- **实验规模**：
  - 4 个数据集，覆盖 2D 图像与 3D 点云；
  - 2 种触发器类型；
  - 多组 \(\epsilon\) 与污染率 \(r\)；
  - 与 4 个基线方法比较；
  - 包含组件消融、超参数敏感性、训练策略/模型敏感性、类别级方差分析。
- **充分性**：
  - 覆盖较广，尤其同时验证低 \(\epsilon\) 和低污染率场景，与论文动机直接对应。
  - 消融实验能够说明分组剪枝和公平选择各自的贡献。
  - 但实验仍集中在分类任务，未覆盖 NLP、多模态、联邦学习等场景。
- **客观与公平性**：
  - 三次重复取平均，主实验设置基本一致，具有一定客观性。
  - HFE 因缺少开源代码由作者复现，可能存在实现差异。
  - 多数结果以曲线呈现，未系统报告标准差或显著性检验；部分基线可能未进行同等程度的超参数调优。
  - 未报告干净样本准确率、触发器检测率或面对防御方法的鲁棒性，攻击隐蔽性评价主要依赖 \(\epsilon\) 和污染率参数本身。

## 6. 主要结论与发现

- 在触发器操纵强度很小时，现有后门样本选择方法会因类别选择不均导致类别级 ASR 方差显著增大，整体 ASR 下降。
- IFS 通过影响函数选择对 ASR 贡献大的样本，并通过类级动态阈值保证类别均衡，在多个数据集和攻击类型上取得更高 ASR。
- 分组剪枝显著降低影响函数计算成本；当 \(\eta\) 在 [10,40] 区间时，CIFAR-10 上性能对 \(\eta\) 不敏感。
- IFS 显著降低类别级 ASR 方差；在 \(\epsilon\) 较大时方差接近 0，在较小 \(\epsilon\) 下也能让更多类别获得较高 ASR。
- 在 surrogate 与 victim 模型一致时 ASR 最高；跨模型黑盒设置下仍有较好表现。
- 消融表明 group split 与 fair selection 均能提升性能，二者结合效果最佳。

## 7. 优点

- **问题发现具有价值**：揭示低操纵强度下类别级 ASR 不公平问题，并用方差指标量化，动机清晰。
- **方法可解释性较强**：相比 FUS、RD 等经验评分，影响函数直接衡量样本对后门测试风险的贡献。
- **计算优化务实**：类原型距离分组剪枝将候选集缩小到 \(1/\eta\)，降低二阶影响函数计算成本。
- **公平选择机制简单有效**：类级分位数阈值直接保证每类选样数量均衡，避免全局阈值造成的类别偏置。
- **实验维度较丰富**：覆盖 2D/3D、两种攻击、多个 \(\epsilon\) 与 \(r\)、消融和敏感性分析，结论与动机对应较好。

## 8. 不足与局限

- **算力信息不完整**：未说明 GPU 数量、总训练时长和搜索开销，复现成本难以评估。
- **依赖 surrogate 模型与验证集**：影响函数计算需要代理模型和后门验证集；跨模型时性能会下降，实际黑盒场景中代理模型选择存在偏差风险。
- **计算仍较重**：需要多轮影响计算与模型重训，且 \(\eta\) 较小时候选集较大；论文“少于 10%”的说法与部分设置（如 \(\eta=5\) 时为 20%）不完全一致。
- **超参数数据集相关**：\(\eta\) 需按数据集设置，虽有一定不敏感性区间，但仍需调参。
- **评价指标有限**：主要报告 ASR，缺少 clean accuracy、检测规避率、面对后门防御的鲁棒性等指标；隐蔽性主要通过 \(\epsilon\) 和污染率间接体现。
- **公平性范围较窄**：只保证类别数量均衡，未考虑类内难度、样本分布或其他公平性维度。
- **实验覆盖有限**：仅图像和点云分类，未验证 NLP、多模态或大规模真实场景；目标类固定为 class 0，可能影响结论泛化性。
- **基线复现风险**：HFE 为作者自行复现，且部分结果缺少误差棒/显著性检验，公平比较仍有改进空间。

（完）
