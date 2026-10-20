---
title: Adversarial-Inspired Backdoor Defense via Bridging Backdoor and Adversarial Attacks
title_zh: 受对抗启发的后门防御：桥接后门攻击与对抗攻击
authors: "Jia-Li Yin, Weijian Wang, Lyhwa, Wei Lin, Ximeng Liu"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33030/35185"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 桥接后门与对抗攻击以构建后门防御
tldr: 针对后门攻击与对抗攻击长期被分开研究、二者内在联系鲜有探索的问题，本文聚焦二者的桥接关系。作者观察在被植入后门的模型上施加对抗攻击时的两种有趣现象，并据此设计出一种受对抗启发的后门防御方法。实验验证该防御能有效削弱后门影响，揭示了后门与对抗扰动之间的可利用关联。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 2932, \"height\": 2366}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 2932, \"height\": 2365}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-003.webp\", \"caption\": \"\", \"page\": 7, \"index\": 3, \"width\": 632, \"height\": 462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 1159, \"height\": 875}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 1132, \"height\": 928}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-006.webp\", \"caption\": \"\", \"page\": 7, \"index\": 6, \"width\": 1139, \"height\": 1088}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-007.webp\", \"caption\": \"\", \"page\": 7, \"index\": 7, \"width\": 1149, \"height\": 1125}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 1074, \"height\": 1041}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 1104, \"height\": 1084}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-010.webp\", \"caption\": \"\", \"page\": 7, \"index\": 10, \"width\": 1152, \"height\": 1124}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-011.webp\", \"caption\": \"\", \"page\": 7, \"index\": 11, \"width\": 1138, \"height\": 984}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-012.webp\", \"caption\": \"\", \"page\": 7, \"index\": 12, \"width\": 1114, \"height\": 948}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-013.webp\", \"caption\": \"\", \"page\": 7, \"index\": 13, \"width\": 1183, \"height\": 1042}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33030/fig-014.webp\", \"caption\": \"\", \"page\": 7, \"index\": 14, \"width\": 1168, \"height\": 1217}]"
motivation: 后门与对抗攻击常被割裂研究，二者内在联系尚待挖掘。
method: 观察感染模型上对抗扰动的两种现象，并据此设计对抗启发的后门防御。
result: 实验表明该防御能有效削弱后门效果并提升模型安全性。
conclusion: 后门与对抗攻击的关联可作为构建防御的有力依据。
---

## Abstract
Backdoor attacks and adversarial attacks are two major security threats to deep neural networks (DNNs), with the former one is a training-time data poisoning attack that aims to implant backdoor triggers into models by injecting trigger patterns into training samples, and the latter one is a testing-time attack trying to generate adversarial examples (AEs) from benign images to mislead a well-trained model. While previous works generally treat these two attacks separately, the inherent connection between these two attacks is rarely explored. In this paper, we focus on bridging backdoor and adversarial attacks and observe two intriguing phenomena when applying adversarial attacks on an infected model implanted with backdoors: 1) the sample is harder to be turned into an AE when the trigger is presented; 2) the AEs generated from backdoor samples are highly likely to be predicted as its true labels. Inspired by these observations, we proposed a novel backdoor defense method, dubbed Adversarial-Inspired Backdoor Defense (AIBD), to isolate the backdoor samples by leveraging a progressive top-q scheme and break the correlation between backdoor samples and their target labels using adversarial labels. Through extensive experiments on various datasets against six state-of-the-art backdoor attacks, the AIBD-trained models on poisoned data demonstrate superior performance over the existing defense methods.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：后门攻击是训练时数据投毒攻击，攻击者在少量训练样本中注入触发器并篡改目标标签，使模型在正常样本上表现正常、遇到触发器时输出指定标签；对抗攻击是推理时攻击，通过对输入添加微小扰动生成对抗样本（AE）误导模型。
- **核心问题**：现有研究通常将后门攻击与对抗攻击视为两个独立领域，二者内在联系探索不足；已有利用对抗训练防御后门的工作又存在触发模式受限、干净精度下降等问题。
- **整体含义**：论文试图“桥接”后门攻击与对抗攻击，通过观察后门模型在对抗样本生成过程中的异常行为，将其转化为后门样本识别与模型净化的信号，提出一种无需预先知道毒化比例、触发器模式或样本分布的训练时后门防御方法 AIBD。

## 2. 方法论

### 核心思想

- 在被植入后门的感染模型上施加迭代式对抗攻击（PGD），发现两个现象：
  1. **后门样本更难被转化为 AE**：带触发器的样本需要更多迭代次数才能生成对抗样本；
  2. **后门样本的对抗标签高概率接近其真实标签**：后门样本成功转为 AE 后，模型预测往往回归其真实类别。
- 基于此，AIBD 将防御分为两个阶段：**后门样本隔离**与**模型净化**。

### 关键技术细节与流程

- **训练感染模型**：先用完整可能含毒训练集训练模型 \(f_p\)。
- **PGD 记录迭代数与对抗标签**：对每个样本 \((x_i,y_i)\) 执行 PGD，记录其生成 AE 所需迭代数 \(k_i\) 及对抗标签 \(\hat y_i\)。
- **放大行为差异**：使用类似 ABL 的梯度加权方式调整迭代数：
  \[
  k_i=(1-\mathrm{sigmoid}(\nabla_{x_i}L(f(x_i;\theta),y_i)))\cdot k_i
  \]
  后门样本梯度通常较小，加权后其 \(k_i\) 更突出。
- **Top-q 隔离**：按 \(k_i\) 排序，将最高 \(q\) 比例的样本划为后门子集 \(D_t\)，其余为干净子集 \(D_c\)。
- **渐进 Top-q 方案**：毒化比例未知，初始设 \(q=20\%\)，按约 2% 递减（20%、18%、…、2%）。若 \(q\) 高于真实毒化率，干净样本被误删，模型 ACC 明显下降并可能灾难性遗忘；若 \(q\) 低于真实毒化率，ACC 趋稳但 ASR 上升。最终选择模型精度趋于稳定时的最大 \(q\)。
- **对抗标签修正**：对被判为后门的样本，用其对抗标签替换原标签：
  \[
  \hat y_i=f(\hat x_i;\theta_p),\quad \forall x_i\in D_t
  \]
  以打破触发器与目标标签之间的强关联。
- **重训净化**：在干净子集与重标后的后门子集上按原分类损失重新训练，得到净化模型。

## 3. 实验设计

- **数据集与模型**：
  - CIFAR-10：ResNet-18；
  - GTSRB：WideResNet（WRN-16-1）。
- **后门攻击**：BadNets、Blend、SIG、WaNet、Trojan、Dynamic 共 6 种。
- **对比防御方法**：ABL、NAD、DBD、CBD，并设置无防御（No Defense）与完全干净数据（None）参照。
- **评价指标**：
  - 主指标：干净样本准确率 ACC、攻击成功率 ASR；
  - 隔离质量：真阳性率 TPR、假阳性率 FPR。
- **实验场景**：
  - 主表比较 2 个数据集 × 6 种攻击 × 多种防御；
  - 不同毒化率（约 1%–20%）下的鲁棒性；
  - 不同隔离方案比较（神经激活、ABL 损失隔离、DBD SCELoss、SCALE-UP、AIBD）；
  - 标签处理消融：Unlearning、Random Labels、Adversarial Labels；
  - t-SNE 特征可视化；
  - 正文提到 GTSRB 更多结果在补充材料中。

## 4. 资源与算力

- 论文正文**未明确报告** GPU 型号、数量、训练时长或总算力消耗。
- 仅可确认实验使用 ResNet-18、WRN-16-1，SGD 优化器，对抗攻击步长 \(1/255\)，并涉及多轮重训与 PGD 迭代；但缺少计算开销与训练成本对比。

## 5. 实验数量与充分性

- **实验规模**：主表约覆盖 2 数据集 × 7 种数据状态 × 6 种方法，约 84 个“数据集-攻击-防御”组合；另有约 25 个隔离方案对比、18 个标签处理消融、24 个毒化率配置及可视化实验，整体达到上百个训练/评估配置。
- **充分性**：
  - 覆盖 6 种代表性后门攻击、2 个图像分类数据集、多种毒化率、隔离质量和标签修正消融，实验较全面；
  - 但缺少多次运行方差、显著性检验、计算成本比较和自适应攻击测试。
- **客观性与公平性**：
  - 作者称遵循各对比防御默认设置并使用统一 ACC/ASR 指标，具有一定公平性；
  - 但不同防御可用信息与计算预算不同：NAD 依赖额外干净子集，ABL 固定选择 1% 样本，AIBD 需训练感染模型、执行 PGD 并多轮重训；未统一计算预算，公平性仍有讨论空间。

## 6. 主要结论与发现

- AIBD 能显著降低后门攻击成功率：CIFAR-10 平均 ASR 从 99.73% 降至 0.33%，GTSRB 从 99.62% 降至 0.08%。
- 在保持干净精度方面表现较好：CIFAR-10 平均 ACC 为 85.31%，GTSRB 为 95.79%，优于多数对比防御，接近无攻击参照。
- 后门样本隔离质量高：平均 TPR 达 98.35%，FPR 仅 1.35%，优于或接近现有隔离方案。
- 对抗标签修正优于随机标签和遗忘式处理：平均 ACC 84.99%、ASR 0.33%，优于随机标签与 Unlearning。
- t-SNE 显示，对抗攻击后后门样本不再紧密聚集于目标类，而按其真实标签分散，支持“对抗扰动可使模型关注图像本身”的解释。

## 7. 优点

- **视角新颖**：系统桥接后门攻击与对抗攻击，揭示后门样本在 AE 生成中的两个可操作行为差异。
- **无需先验知识**：不要求知道毒化比例、触发器模式或样本分布，适合实际训练时防御。
- **渐进 Top-q 设计**：通过 ACC 反馈动态调整隔离比例，缓解毒化率未知问题。
- **对抗标签修正有效**：相比直接遗忘或随机重标，能更好保持干净精度并压低 ASR。
- **实验较系统**：覆盖多种攻击、数据集、毒化率、隔离质量和标签消融，并提供可视化解释。
- **隔离指标亮眼**：高 TPR、低 FPR，说明迭代次数信号具有较强判别力。

## 8. 不足与局限

- **计算开销未评估**：PGD 逐样本迭代、感染模型训练和多轮重训成本较高，论文未报告算力与时间，也未与防御方法比较效率。
- **超参与停止准则较经验化**：初始 \(q=20\%\)、按 2% 递减及“ACC 接近”停止条件较粗略；q 搜索上限假设毒化率通常低于 20%。
- **极端毒化率下性能有限**：1% 毒化率时 ACC 与 ASR 均较高，20% 时 ASR 略升、ACC 下降，说明隔离不完整或信号不足。
- **实验覆盖有限**：仅图像分类小数据集，未验证大模型、高分辨率图像、文本或多模态场景；未测试自适应攻击（如攻击者针对迭代次数特征进行规避）。
- **统计严谨性不足**：未报告多次运行均值方差、显著性检验；部分表格缺少 WaNet 等攻击的隔离结果。
- **公平性风险**：不同防御可用数据与计算预算不一致，NAD 需干净子集，AIBD 需额外 P

GD 迭代与多轮重训，整体计算预算高于一次性训练或轻量防御；论文未给出时间、显存、能耗等成本对比，使“精度—鲁棒性—成本”的权衡难以完整评估。其次，渐进 Top-q 依赖 ACC 拐点判断，若干净精度波动较大、类别不平衡或存在分布偏移，拐点可能不清晰；初始上限设为 20% 也隐含了毒化率通常不高的先验。再次，对抗标签修正对误隔离的干净样本可能引入错误标签，论文未单独分析误隔离样本的标签噪声传播。最后，方法有效性建立在“后门样本更难生成 AE、且对抗标签倾向回归真实类”的经验现象上，缺少理论刻画；若自适应攻击者针对 PGD 迭代次数或梯度幅值进行规避，防御可能退化。

## 9. 复现性与开放问题

- 正文未系统报告随机种子、多次运行方差、完整超参搜索空间及计算资源，复现与公平比较仍受限制。
- 方法涉及感染模型训练、逐样本 PGD、渐进 \(q\) 多轮重训，实际部署需评估时间成本；能否通过减少 PGD 步数、早停或子集采样降低成本，是重要工程问题。
- 需进一步验证大规模数据集/模型、多标签与多目标后门、干净标签后门、物理触发器、联邦学习与持续学习等场景，并测试自适应攻击下的鲁棒性。
- 理论上可进一步刻画后门样本在对抗扰动下的梯度与损失景观特征，解释“迭代次数—真实标签回归”现象的边界条件。

## 10. 总体评价

论文以“后门攻击—对抗攻击”交叉视角提出 AIBD，核心观察具有启发性；渐进 Top-q 与对抗标签修正的组合在多种攻击和数据集上取得较低 ASR 与较高 ACC，隔离指标突出。其主要贡献在于提供了一种无需毒化比例、触发器模式和样本分布先验的训练时防御思路。但计算成本、超参停止准则、自适应攻击鲁棒性和跨场景泛化仍需补强；若能在效率优化与理论解释上推进，将提升其实用性与说服力。

（完）
