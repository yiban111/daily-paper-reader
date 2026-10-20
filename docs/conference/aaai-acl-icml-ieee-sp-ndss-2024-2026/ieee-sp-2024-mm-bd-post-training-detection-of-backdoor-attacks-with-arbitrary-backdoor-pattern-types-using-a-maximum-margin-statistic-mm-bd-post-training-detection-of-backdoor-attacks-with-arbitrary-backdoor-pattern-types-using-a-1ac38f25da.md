---
title: "MM-BD: Post-Training Detection of Backdoor Attacks with Arbitrary Backdoor Pattern Types Using a Maximum Margin Statistic"
title_zh: MM-BD：利用最大间隔统计量检测任意后门模式类型的训练后后门攻击
authors: "Hang Wang, Zhen Xiang, David J. Miller, George Kesidis"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.computer.org/csdl/pds/api/csdl/proceedings/download-article/1RjE9SU7Kz6/pdf"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 任意后门模式的训练后检测
tldr: 现有训练后后门检测器多针对特定嵌入函数，面对任意后门模式易失效。本文提出MM-BD，利用最大间隔统计量在无训练集访问下判断分类器是否被植入后门。实验表明其对多种后门模式类型均具良好检测能力，提升了训练后后门检测的通用性。
source: IEEE-SP-2024-CSDL
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-001.webp\", \"caption\": \"\", \"page\": 6, \"index\": 1, \"width\": 600, \"height\": 400}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 600, \"height\": 400}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 600, \"height\": 400}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 600, \"height\": 400}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 600, \"height\": 400}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-006.webp\", \"caption\": \"\", \"page\": 6, \"index\": 6, \"width\": 600, \"height\": 500}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-007.webp\", \"caption\": \"\", \"page\": 6, \"index\": 7, \"width\": 600, \"height\": 400}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-008.webp\", \"caption\": \"\", \"page\": 10, \"index\": 8, \"width\": 357, \"height\": 473}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-009.webp\", \"caption\": \"\", \"page\": 10, \"index\": 9, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-010.webp\", \"caption\": \"\", \"page\": 12, \"index\": 10, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-011.webp\", \"caption\": \"\", \"page\": 12, \"index\": 11, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-012.webp\", \"caption\": \"\", \"page\": 13, \"index\": 12, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-013.webp\", \"caption\": \"\", \"page\": 13, \"index\": 13, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-014.webp\", \"caption\": \"\", \"page\": 13, \"index\": 14, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-015.webp\", \"caption\": \"\", \"page\": 17, \"index\": 15, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-016.webp\", \"caption\": \"\", \"page\": 17, \"index\": 16, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-017.webp\", \"caption\": \"\", \"page\": 18, \"index\": 17, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-018.webp\", \"caption\": \"\", \"page\": 18, \"index\": 18, \"width\": 640, \"height\": 480}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-mm-bd-post-training-detection-of-backdoor-attacks-with-arbitrary-backdoor-pattern-types-using-a-maximum-margin-statistic/fig-019.webp\", \"caption\": \"\", \"page\": 18, \"index\": 19, \"width\": 640, \"height\": 480}]"
motivation: 现有训练后检测器针对特定嵌入函数，面对任意模式易失效。
method: 利用最大间隔统计量在无训练集下检测后门。
result: 对多种后门模式类型均具良好检测能力。
conclusion: 提升了训练后后门检测的通用性与适用范围。
---

## Abstract
Backdoor attacks are an important type of adversarial threat against deep neural network classifiers, wherein test samples from one or more source classes will be (mis)classified to the attacker’s target class when a backdoor pattern is embedded. In this paper, we focus on the post-training backdoor defense scenario commonly considered in the literature, where the defender aims to detect whether a trained classifier was backdoor-attacked without any access to the training set. Many post-training detectors are designed to detect attacks that use either one or a few specific backdoor embedding functions (e.g., patch-replacement or additive attacks). These detectors may fail when the backdoor embedding function used by the attacker (unknown to the defender) is different from the backdoor embedding function assumed by the defender. In contrast, we propose a post-training defense that detects backdoor attacks with arbitrary types of backdoor embeddings, without making any assumptions about the backdoor embedding type. Our detector leverages the influence of the backdoor attack, independent of the backdoor embedding mechanism, on the landscape of the classifier’s outputs prior to the softmax layer. For each class, a maximum margin statistic is estimated. Detection inference is then performed by applying an unsupervised anomaly detector to these statistics. Thus, our detector does not need any legitimate clean samples, and can efficiently detect backdoor attacks with arbitrary numbers of source classes. These advantages over several state-of-the-art methods are demonstrated on four datasets, for three different types of backdoor patterns, and for a variety of attack configurations. Finally, we propose a novel, general approach for backdoor mitigation once a detection is made. The mitigation approach was the runner-up at the first IEEE Trojan Removal Competition. The code is online available.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：后门攻击（Trojan）通过污染训练数据，使带触发模式的样本被误判到攻击者指定目标类，同时保持干净样本精度。论文关注**训练后（post-training）防御场景**：防御者只能访问已训练分类器，无法访问训练集，也不知道是否存在攻击。
- **核心问题**：许多现有训练后检测器隐含假设后门嵌入函数类型（如 patch replacement、加性扰动、blending），或需要干净样本/干净模型；当攻击者使用的后门模式类型与检测器假设不匹配时，检测器可能失效。此外，部分方法难以处理任意数量的源类。
- **整体含义**：论文提出一种**不假设后门模式类型、无需干净样本**的训练后检测方法 MM-BD，利用分类器 softmax 前 logits 景观中的“最大间隔”异常作为攻击签名；并进一步提出后门缓解方法 MM-BM。其意义在于提升训练后后门检测的通用性、适用性和计算效率。

## 2. 方法论

- **核心思想**：后门模式在中毒训练集中反复出现，分类器会对其过拟合，导致目标类 logit 被异常“抬升”，同时其他类 logit 被“抑制”。因此，对真实后门目标类，其“该类 logit 减去其他类最大 logit”的最大间隔会显著大于非目标类。
- **最大间隔统计量**：对每个类 \(c\)，求解
  \[
  \max_{x\in X}\left[g_c(x)-\max_{k\in Y\setminus c}g_k(x)\right]
  \]
  其中 \(g_c\) 为 softmax 前 logit。采用**梯度上升 + 投影回输入域**，并进行多次随机初始化，取最大局部最优解作为该类统计量 \(r_c\)。
- **检测推断**：令 \(r_{\max}=\max_c r_c\)，假设攻击存在时 \(r_{\max}\) 对应目标类且为异常值。用除 \(r_{\max}\) 外的统计量拟合单尾零分布（如 Gamma），计算顺序统计量 p 值：
  \[
  pv=1-H_0(r_{\max})^{K-1}
  \]
  若 \(pv<\theta\)（实验中固定 \(\theta=0.05\)）则判定存在后门，并将 \(r_{\max}\) 对应类推断为目标类。
- **无需干净样本**：检测阶段不依赖任何合法样本，也不假设源类数量，因此可处理单源类、多源类甚至任意源类设置。
- **后门缓解 MM-BM**：检测后，对每层神经元激活设置优化上界 \(z_l\)，通过最小化上界范数并约束干净样本精度不低于 \(\pi\)（如 0.95），抑制后门触发引起的异常大激活；不修改网络结构或原始训练参数，使用少量干净样本和拉格朗日梯度下降求解。

## 3. 实验设计

- **数据集/场景**：
  - 图像分类：CIFAR-10、CIFAR-100、TinyImageNet、GTSRB。
  - 扩展域：语音命令分类（Speech Commands）、点云分类（DGCNN）。
  - 后门模式：加性（chessboard、1-pixel）、patch replacement（BadNet、unicolor）、blending；并测试 clean-label、输入感知 IA、WaNet、label-smoothed 等高级攻击。
  - 攻击配置：单源类（S）和多源类（M）；另测试 all-to-all 攻击、差分隐私训练模型、自适应攻击。
- **Benchmark/指标**：
  - 检测：检测准确率（同时要求检测到攻击并正确推断目标类）、干净模型被判“无攻击”的比例、假阳性率/TPR、执行时间。
  - 缓解：攻击成功率 ASR、干净样本准确率 ACC、执行时间。
- **对比方法**：
  - 检测：NC、TABOR、ABS、META、TND、PT-RED，以及 PT-RED+ABS 联合部署。
  - 缓解：NC-M、Fine-Pruning、NAD，以及 MM-BM+Fine-Pruning 组合。
- **实验规模概览**：
  - CIFAR-10 构造 10 个攻击集成（5 种模式 × S/M），每个集成含多次独立攻击；CIFAR-100、GTSRB 各 5 个模式集成；TinyImageNet 仅 BadNet-M 集成。
  - 另含语音 10 干净 + 10 攻击模型、点云 10 干净 + 10 攻击模型、10 组 all-to-all 攻击、5 个 DP 模型、自适应攻击多组 \(\beta_M\) 实验，以及类别不平衡、正则化干净模型等案例分析。
  - 注意：正文称每个集成 10 次独立攻击，表注又称每个攻击训练 30 个模型，具体计数表述略有差异。

## 4. 资源与算力

- 文中明确实验使用 **NVIDIA RTX-3090 GPU**，但**未说明 GPU 数量、总 GPU 小时或总算力预算**。
- 提供了部分时间信息：检测平均执行时间在 CIFAR-10 上约 27.2 秒、CIFAR-100 约 114.8 秒、TinyImageNet 约 503.4 秒、GTSRB 约 37.1 秒；META 在 CIFAR-10 上约需 32 小时，因此未在其他数据集运行。
- 训练配置：CIFAR-10 60 epochs、CIFAR-100 100 epochs、TinyImageNet 90 epochs、GTSRB 50 epochs；自适应攻击实验中常规训练 60 epochs 约 35 分钟，单 epoch 自适应微调约 62 分钟（更强自适应攻击约 642 分钟/epoch）。
- 结论：算力信息**部分披露但不完整**，无法据此精确复现总计算成本。

## 5. 实验数量与充分性

- **实验数量较多**：覆盖 4 个图像数据集、2 个非图像域、5 种以上后门模式、单/多源类、多种网络架构（ResNet-18、VGG-16、ResNet-34、MobileNet、PreActResNet-18、M5、DGCNN），并包含检测、缓解、自适应攻击、DP、类别不平衡等分析。
- **充分性**：
  - 优点是对后门模式类型和源类数量的覆盖较广，且专门测试了现有检测器不擅长的高级攻击。
  - 不足是部分大规模数据集仅展示任意选取的少量集成结果；TinyImageNet 仅一个攻击集成；META 仅在 CIFAR-10 比较；部分完整结果放在附录。
- **客观性与公平性**：
  - 基线尽量遵循原实现，并“选择最佳检测阈值”以最大化基线性能；MM-BD 则固定 \(\theta=0.05\)，这对基线相对有利。
  - 但比较场景并不完全对等：MM-BD 不需要干净样本，而 NC、ABS、META 等需要；PT-RED 在大类别数据集上被降低复杂度（仅按目标类估计）；META 因计算成本未全量运行。
  - 因此总体实验较有说服力，但**跨方法比较存在一定条件和实现差异**，需谨慎解读绝对优势。

## 6. 主要结论与发现

- 最大间隔统计量可作为后门攻击的有效签名：真实目标类的最大间隔通常显著异常于非目标类，且该现象不依赖具体后门嵌入类型。
- MM-BD 在四类图像数据集和三种主要后门模式上取得较高检测准确率，且对后门模式类型**基本不变**；相比专用于某类模式的检测器更通用。
- MM-BD **无需干净样本**，可检测任意数量源类的攻击，并在多数情况下比 NC、TABOR、ABS、TND、PT-RED 等更高效。
- 对 clean-label、IA、WaNet、label-smoothed 等高级攻击，在高 ASR 条件下仍有较好检测效果。
- MM-BM 能显著降低多数后门模式的 ASR，同时保持可接受 ACC，且仅需少量干净样本；但对 chessboard 这类微小全局扰动效果差，与 Fine-Pruning 结合可改善。
- 自适应攻击可通过最小化目标类最大间隔绕过 MM-BD，但需要完全控制训练过程并求解昂贵 min-max 优化，且会牺牲 ASR/ACC；更强自适应攻击计算代价更高。
- 少类数据集、严重类别不平衡、异常正则化的干净模型可能导致假阳性；all-to-all 场景下无干净类统计量，需借助校准阈值。

## 7. 优点

- **方法简洁且模式无关**：仅利用 logits 间隔，不依赖后门模式形状、尺寸、颜色或嵌入函数假设。
- **实用性强**：检测不需要干净样本或干净参考模型，适合专有/遗留模型场景；可处理任意源类数。
- **计算效率较高**：相比多个训练后检测器执行时间更短，且检测流程可并行到各类。
- **统计推断清晰**：使用无监督零分布和顺序统计量 p 值，阈值解释明确。
- **实验覆盖较广**：包含多种数据集、模式、架构、高级攻击、非图像域和自适应攻击分析，并讨论失败案例。
- **缓解方法不修改架构和原始参数**：可与现有微调/剪枝方法组合，具备部署灵活性。

## 8. 不足与局限

- **自适应攻击可绕过**：掌握训练控制权和防御知识的攻击者可通过优化最大间隔降低检测性；虽然代价高，但说明并非鲁棒下界。
- **少类数据集假阳性较高**：CIFAR-10 仅 9 个非目标统计量用于拟合零分布，理论 FPR 不一定准确；GTSRB 受类别不平衡影响也有假阳性。
- **对数据分布敏感**：严重类别不平衡或异常正则化的干净模型可能产生异常大间隔，导致误报。
- **all-to-all 场景不直接适用**：所有类均为目标类时缺少非后门类统计量，无监督异常检测病态，需要外部校准阈值。
- **MM-BM 覆盖不全**：对 chessboard 等微小全局扰动缓解失败；需结合其他方法。
- **域外泛化有限**：点云检测受“内在后门”影响，效果弱于图像/语音；离散或类别型数据（如自然语言）需替代搜索策略。
- **白盒与计算假设

- **白盒与计算假设**：MM-BD 需要访问 softmax 前 logits，并能对输入做梯度上升与反向传播，因此属于**白盒检测**；对于仅提供硬标签、查询次数受限或不可微输入的黑盒 API 场景，方法不能直接部署。每类多次随机初始化与优化也会随类别数、输入维度和模型规模增加成本，TinyImageNet 上约 503 秒的执行时间已显示其扩展压力。
- **统计假设与阈值校准**：p 值推断依赖“非目标类统计量近似同分布、真实目标类为少数异常”的假设；在少类数据集、多目标类、all-to-all 或类别严重不平衡时，该假设容易失效。固定 \(\theta=0.05\) 虽便于跨实验比较，但不同数据域上的实际 FPR/TPR 权衡仍需部署时重新校准。
- **缓解方法的额外依赖**：MM-BM 需要少量干净样本和精度下限 \(\pi\)，若干净样本不可得、分布偏移或精度约束过紧，上界优化可能失效；对 chessboard 等微小全局扰动缓解效果差，说明“激活上界”并非对所有后门机制充分。
- **比较公平性与复现细节**：基线采用“最佳阈值”而 MM-BD 固定阈值，总体上对基线有利；但 PT-RED 在大类别数据集上被降低复杂度、META 因计算成本未全量运行、部分结果置于附录，且攻击模型计数表述略有差异，这些都会影响绝对性能比较与精确复现。
- **威胁模型边界**：自适应攻击虽需完全控制训练并付出高昂计算代价，但已表明 MM-BD 不是鲁棒性保证；检测器还隐含假设目标类为少数，难以处理所有类均被感染或更隐蔽的分布式后门。

## 9. 总体评价与可延伸方向

- **总体评价**：MM-BD 的核心贡献在于提出一种**模式无关、无需干净样本、可处理任意源类数**的训练后检测范式，利用最大 logit 间隔作为攻击签名，方法简洁、统计解释清晰，并在多数据集、多攻击模式和非图像域上展示了较好的经验效果与效率。其价值更多体现在“通用检测框架”而非“绝对鲁棒防御”。
- **可延伸方向**：可研究黑盒/查询受限场景下的间隔估计、离散或不可微数据上的替代搜索、少类与多目标场景下更稳健的零分布建模、与训练时防御或模型修复方法的组合，以及更低成本的自适应攻击评估协议。MM-BM 也可进一步扩展为针对微小全局扰动的分层约束或与剪枝/微调更紧耦合的修复流程。

（完）
