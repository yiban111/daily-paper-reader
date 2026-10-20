---
title: Attention-Imperceptible Backdoor Attacks on Vision Transformers
title_zh: 面向视觉Transformer的注意力不可感知后门攻击
authors: "Zhishen Wang, Rui Wang, Lihua Jing"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32889/35044"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 针对视觉Transformer的注意力不可感知隐蔽后门攻击
tldr: 视觉Transformer面临后门威胁，已有针对补丁级自注意力的攻击在隐蔽性与抗防御鲁棒性上较为粗糙。本文提出AIBA，从注意力层面实现不可感知的隐蔽后门攻击。实验表明该方法在提升隐蔽性的同时增强了对防御措施的鲁棒性。其贡献在于深化了对ViT注意力级后门机制的理解。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-003.webp\", \"caption\": \"\", \"page\": 1, \"index\": 3, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-004.webp\", \"caption\": \"\", \"page\": 1, \"index\": 4, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-005.webp\", \"caption\": \"\", \"page\": 1, \"index\": 5, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-006.webp\", \"caption\": \"\", \"page\": 1, \"index\": 6, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-007.webp\", \"caption\": \"\", \"page\": 1, \"index\": 7, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-008.webp\", \"caption\": \"\", \"page\": 1, \"index\": 8, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-009.webp\", \"caption\": \"\", \"page\": 1, \"index\": 9, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-010.webp\", \"caption\": \"\", \"page\": 1, \"index\": 10, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-011.webp\", \"caption\": \"\", \"page\": 1, \"index\": 11, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-012.webp\", \"caption\": \"\", \"page\": 1, \"index\": 12, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-013.webp\", \"caption\": \"\", \"page\": 1, \"index\": 13, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-014.webp\", \"caption\": \"\", \"page\": 3, \"index\": 14, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-015.webp\", \"caption\": \"\", \"page\": 3, \"index\": 15, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-016.webp\", \"caption\": \"\", \"page\": 6, \"index\": 16, \"width\": 2820, \"height\": 1416}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-017.webp\", \"caption\": \"\", \"page\": 6, \"index\": 17, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-018.webp\", \"caption\": \"\", \"page\": 6, \"index\": 18, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32889/fig-019.webp\", \"caption\": \"\", \"page\": 6, \"index\": 19, \"width\": 640, \"height\": 480}]"
motivation: 已有ViT后门攻击在隐蔽性与抗防御鲁棒性方面较为粗糙，缺乏深入研究。
method: 提出AIBA，从注意力层面实现不可感知的隐蔽后门攻击设计。
result: 实验表明该方法在提升隐蔽性的同时增强了对防御的鲁棒性。
conclusion: 深化了对视觉Transformer注意力级后门机制的理解。
---

## Abstract
With the successful transition of Transformers from natural language processing (NLP) to computer vision (CV) domains, Vision Transformers (ViTs) have achieved state-of-the-art performance in many CV tasks. However, backdoor attacks, a significant threat in deep learning, also pose a risk to the security of ViT models. Recently, several backdoor attack methods targeting the patch-level self-attention mechanism in ViTs have been proposed, but they are relatively naive in terms of stealthiness and robustness against defensive measures, lacking in-depth investigation. In this paper, we explore the crucial role of attention-level imperceptibility in backdoor attacks for ViTs and propose an Attention-Imperceptible Backdoor Attacks on Vision Transformers (AIBA). In AIBA, a constrained adversarial perturbation is used as the trigger to achieve visual imperceptibility. Additionally, the trigger is designed to seamlessly implant into the focal areas of the image, ensuring that the trigger receives enough attention from the model without causing anomalies at the attention level. During the backdoor learning process, we designed an efficient constrained bi-level optimization training strategy at the mini-batch level to implant an effective backdoor in the victim model using the imperceptible trigger. We evaluated the effectiveness of the proposed AIBA across multiple datasets and ViT benchmarks and explored the robustness of AIBA against current ViT-specific defense methods. The experimental results demonstrate that our backdoor attack method can successfully implant a powerful and stealthy backdoor into ViTs.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：Vision Transformer（ViT）在计算机视觉任务中取得与 CNN 相当甚至更优的性能，但其自注意力机制和开源预训练/微调生态也带来后门攻击风险。攻击者可通过数据投毒在训练/微调阶段植入后门，使模型在干净样本上表现正常，而在含触发器的样本上输出攻击者指定标签。
- **核心问题**：已有面向 ViT 的后门攻击（如 DBIA、TrojViT、BadViT）虽然利用 patch 级自注意力，但普遍存在两类不足：
  - 触发器视觉上较明显，易被人工检查发现；
  - 触发器优化倾向于“最大化模型注意力”，导致注意力图出现显著异常，容易被基于注意力异常的 ViT 专用防御检测。
- **论文主张**：ViT 后门攻击不仅要“视觉不可感知”，还应实现“注意力层面不可感知”。即后门样本的注意力分布应尽量接近干净样本，触发器应嵌入图像原本的重点区域，而不是强行制造新的注意力焦点。
- **整体含义**：论文提出 AIBA（Attention-Imperceptible Backdoor Attacks on Vision Transformers），试图在保持高攻击成功率的同时，降低注意力图异常，从而提升对 ViT 专用防御的鲁棒性，推动 ViT 后门攻防博弈。

## 2. 方法论

### 2.1 核心思想

- 使用**受约束的对抗扰动**作为 patch 级触发器，实现视觉不可感知。
- 通过 **Attention-Max Patch Selection** 将触发器嵌入模型原本最关注的图像区域，使触发器获得足够注意力，同时不引入明显的注意力异常。
- 将触发器生成与模型投毒建模为**约束双层优化问题**，并采用**mini-batch 级交替优化**高效植入后门。

### 2.2 关键技术细节

- **威胁模型**：假设攻击者可在训练阶段完全访问受害 ViT，包括模型参数和训练过程；受害模型随后公开发布供用户下载部署。
- **问题设定**：监督分类任务，ViT 将图像切分为 \(N_p\) 个 patch，并加入 CLS token；最终由 CLS token 对应输出经 MLP 分类。采用 all-to-one 后门目标：干净样本保持原标签，后门样本 \(T_\phi(x)\) 被预测为目标标签 \(\eta(y)\)。
- **Attention-Max Patch Selection**：
  - 使用 Attention Rollout 获取最终 Transformer 层中 CLS token 对各 patch 的注意力；
  - 选择注意力最高的 top-\(N_m\) 个 patch，生成二值掩码 \(M(x)\)；
  - 将触发器仅附加到这些高注意力 patch 上，使触发器自然融入模型原有焦点区域。
- **对抗触发器生成**：
  - 初始化 patch 级扰动 \(\phi\)；
  - 后门图像构造为 \(T_\phi(x)=x+M(x)\cdot\phi\)；
  - 与以往“最大化注意力”的触发器优化不同，AIBA 只优化后门样本到目标标签的映射；
  - 使用 \(l_\infty\) 约束和裁剪操作，将每个像素扰动限制在 \(\epsilon\) 邻域内，以保证视觉隐蔽性。
- **双层约束优化**：
  - 外层优化模型参数 \(\theta\)，目标为 \(\alpha L_{\text{clean}}(\theta)+\beta L_{\text{backdoor}}(\theta,\hat{\phi})\)；
  - 内层优化触发器 \(\phi\)，使后门损失最小；
  - 同时约束 \(d(T_\phi(x),x)\le \epsilon\)。
- **Mini-batch 交替优化**：
  - 每个 mini-batch 内先对后门样本执行若干次触发器注入迭代，更新 \(\phi\)；
  - 再用优化后的触发器进行投毒训练，更新模型参数 \(\theta\)；
  - 下一 mini-batch 使用更新后的模型继续优化触发器；
  - 论文称该策略可在极少微调轮次内植入强后门，实验中仅微调 1 个 epoch。
- **主要实验超参**：输入 \(3\times224\times224\)，patch size 16，\(N_p=196\)；中毒率 \(\rho=0.1\)；目标标签为 1；选择 top \(N_m=24\) 个 patch；扰动约束 \(\epsilon=4/255\)；触发器学习率 0.01；每 mini-batch 注入迭代 \(N_{\text{injection}}=1\)；模型微调 batch size 64、学习率 \(1e-5\)、1 epoch。

## 3. 实验设计

- **数据集**：
  - ImageNet：1.28M 训练图像、50k 验证图像、1000 类；
  - CIFAR-10：60k 图像、10 类，50k 训练、10k 测试。
- **模型 / Benchmark**：
  - ViT-base；
  - DeiT-Tiny、DeiT-Small、DeiT-Base；
  - 均基于官方预训练模型进行后门微调；CIFAR-10 上由 ImageNet 预训练模型微调得到。
- **对比方法**：
  - 通用后门攻击：BadNet、WaNet；
  - ViT 专用后门攻击：DBIA、TrojViT、BadViT；
  - 主要在 DeiT-Base + ImageNet 上与上述方法对比。
- **评价指标**：
  - Clean Accuracy（CA）；
  - Backdoor Accuracy（BA），即后门模型在良性测试样本上的准确率；
  - Attack Success Rate（ASR）；
  - Attention Residual（ARES）：干净图像与后门图像注意力图的平均绝对差异，越低越好；
  - Attention PSNR（APSNR）：注意力图 PSNR，越高越好；
  - Attention LPIPS（ALPIPS）：注意力图 LPIPS，越低越好。
- **防御实验**：
  - BDVT：在注意力热图最高位置放置黑 patch；作者将其增强为遮蔽 top-\(N\) 个 patch；
  - PatchProcess：包括 PatchDrop 与 PatchShuffle，通过 patch 处理后的标签变化检测后门样本，报告 TPR/TNR。

## 4. 资源与算力

- 论文正文**未明确说明**使用的 GPU 型号、数量、训练总时长或总算力开销。
- 仅能看出实验涉及 ImageNet 上的 ViT/DeiT 多模型微调，计算成本可能较高，但论文未提供可复现的算力配置信息。
- 因此，无法从文中判断其训练效率与资源需求的具体量级。

## 5. 实验数量与充分性

- **实验组数概览**：
  - 表 1：DeiT-Base + ImageNet 上 6 种攻击方法对比；
  - 表 2：ImageNet 上 4 种 ViT 模型；
  - 表 3：CIFAR-10 上 4 种 ViT 模型；
  - 表 4：触发器 patch 数量消融，0/4/8/16/24/32 共 6 组；
  - 表 5：扰动预算 \(\epsilon\) 消融，0/2/4/8/16/32 /255 共 6 组；
  - 表 6：BDVT 防御下 top-\(N\) 遮蔽实验，0/1/2/4/8/16 共 6 组；
  - 表 7：PatchProcess 防御，4 种 process ratio；
  - 图 3：不同攻击的注意力图可视化示例。
- **充分性评价**：
  - 优点：覆盖两个数据集、四种 ViT/DeiT 模型、多个通用与 ViT 专用攻击基线，并包含触发器面积、扰动预算和两类 ViT 专用防御实验，整体较系统。
  - 局限：
    - 主要对比实验集中在 DeiT-Base + ImageNet，其他模型主要验证 AIBA 自身效果，未与所有基线全面交叉对比；
    - 正文仅报告 ViT 专用防御，通用防御放在补充材料，正文可读性受限；
    - 未报告多次运行的均值/方差或统计显著性；
    - 未系统评估自适应防御、迁移攻击、物理世界攻击等场景；
    - BDVT 的“增强版”由作者实现，防御强度与公平性需谨慎解读；
    - PatchProcess 的 TPR 很低，但 TNR 约 89%–91%，说明存在一定误报背景，论文对防御整体有效性讨论较简略。

## 6. 主要结论与发现

- AIBA 在 DeiT-Base + ImageNet 上取得 **99.97% ASR**，BA 为 **81.54%**，接近 CA 81.80%，干净精度仅下降约 0.26%。
- 在注意力隐蔽性指标上，AIBA 达到 **ARES=0.078、APSNR=20.68、ALPIPS=0.000058**，显著优于 DBIA、TrojViT、BadViT 等 ViT 专用攻击，表明其注意力图异常更小。
- 在 ViT-base、DeiT-Tiny、DeiT-Small、DeiT-Base 以及 ImageNet/CIFAR-10 上，AIBA 的 ASR 均接近 100%，BA 与 CA 接近；模型参数越少，BA 下降略明显，但 ASR 仍保持高位。
- 触发器 patch 数量存在效果与隐蔽性的权衡：数量增加提升 ASR，但超过一定范围后收益有限且注意力隐蔽性下降；论文默认选择 24 个 patch。
- 即使扰动预算小至 \(2/255\)，ASR 仍达 99.82%，说明交替后门学习可利用细微触发器建立强后门。
- 对 BDVT：遮蔽 top-16 patch 时 ASR 降至 85.18%，但 BA 也降至 77.53%；在较小遮蔽数量下 ASR 仍接近 100%，说明该防御难以在保持干净性能的同时有效移除后门。
- 对 PatchProcess：TPR 低于 4%，表明该防御难以检测 AIBA 后门样本；作者认为触发器逐渐成为目标类特征并自然嵌入关键区域，使后门样本对 patch 处理的敏感性与干净样本差异消失。

## 7. 优点

- **问题定位新颖且切中要害**：明确提出“注意力层面不可感知”是 ViT 后门攻击隐蔽性的关键，而不仅是像素级视觉隐蔽。
- **触发器设计合理**：将触发器嵌入模型原本高注意力区域，避免强行制造注意力焦点，从机制上减少注意力异常。
- **视觉与注意力双重隐蔽**：对抗扰动保证像素级不可见，Attention-Max Patch Selection 保证注意力分布自然。
- **优化策略高效**：mini-batch 级交替优化将触发器生成与模型投毒耦合，避免分阶段训练陷入局部最优，并减少微调轮次。
- **评价指标针对性强**：引入 ARES、APSNR、ALPIPS 等注意力层面指标，补充了传统 ASR/BA 之外的隐蔽性评估。
- **实验覆盖较广**：多数据集、多 ViT 变体、多攻击基线、两类 ViT 专用防御及关键超参消融，结论较有说服力。

## 8. 不足与局限

- **威胁模型较强**：假设攻击者完全访问模型参数和训练过程，适用于“外包训练/发布恶意预训练模型”场景，但对仅数据投毒或黑盒场景的适用性未讨论。
- **任务与数据覆盖有限**：仅验证监督图像分类，数据集为 ImageNet 和 CIFAR-10；未覆盖检测、分割、多模态 ViT、视频 Transformer 等。
- **攻击设定较单一**：主要采用 all-to-one、固定目标标签 1、固定中毒率 0.1；未充分讨论不同目标标签、多目标后门、低中毒率等设置。
- **基线对比不够全面**：其他 ViT 模型上主要展示 AIBA 自身效果，未与全部基线在所有模型/数据集上交叉比较。
- **防御评估有限**：正文只详细评估 BDVT 和 PatchProcess，通用防御在补充材料；未评估自适应防御、基于触发器逆向、模型诊断、微调净化等更强防御。
- **公平性与可复现性**：BDVT 被作者增强实现，防御强度依赖具体实现；未报告多次实验方差、随机种子和统计显著性；算力资源未说明。
- **注意力指标的解释性**：ARES/APSNR/ALPIPS 能刻画注意力图差异，但不能完全等价于人类或防御器的可检测性；缺少人类感知研究或更实际的检测器评估。
- **潜在应用限制**：方法依赖 Attention Rollout 选择高注意力 patch，可能增加计算开销；对注意力图质量、模型结构和输入分辨率的敏感性未充分展开。
- **安全性研究属性**：论文属于攻击方法研究，虽有助于理解 ViT 脆弱性，但实际部署中需配合防御与负责任披露。

（完）
