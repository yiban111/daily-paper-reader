---
title: Breaking the Stealth-Potency Trade-off in Clean-Image Backdoors with Generative Trigger Optimization
title_zh: 以生成式触发器优化突破干净图像后门的隐蔽-效力权衡
authors: "Binyan Xu, Fan Yang, Di Tang, Xilin Dai, Kehuan Zhang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39935/43896"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 基于生成式触发器优化的干净图像后门攻击
tldr: 现有干净图像后门攻击需要较高投毒率，导致干净准确率明显下降而暴露。本文提出GCB框架，利用条件InfoGAN挖掘自然出现的图像特征作为隐蔽触发器并优化之。实验表明其在保持攻击效力的同时显著降低准确率损失。该工作改善了干净标签后门的隐蔽-效力平衡。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 911, \"height\": 650}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 906, \"height\": 651}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-003.webp\", \"caption\": \"\", \"page\": 4, \"index\": 3, \"width\": 907, \"height\": 650}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-004.webp\", \"caption\": \"\", \"page\": 4, \"index\": 4, \"width\": 901, \"height\": 655}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-005.webp\", \"caption\": \"\", \"page\": 4, \"index\": 5, \"width\": 903, \"height\": 655}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-006.webp\", \"caption\": \"\", \"page\": 4, \"index\": 6, \"width\": 917, \"height\": 599}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 387, \"height\": 416}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-008.webp\", \"caption\": \"\", \"page\": 5, \"index\": 8, \"width\": 1330, \"height\": 833}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-009.webp\", \"caption\": \"\", \"page\": 5, \"index\": 9, \"width\": 817, \"height\": 612}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-010.webp\", \"caption\": \"\", \"page\": 5, \"index\": 10, \"width\": 816, \"height\": 612}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-011.webp\", \"caption\": \"\", \"page\": 6, \"index\": 11, \"width\": 1087, \"height\": 656}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-012.webp\", \"caption\": \"\", \"page\": 6, \"index\": 12, \"width\": 1016, \"height\": 657}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-013.webp\", \"caption\": \"\", \"page\": 7, \"index\": 13, \"width\": 751, \"height\": 672}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-014.webp\", \"caption\": \"\", \"page\": 7, \"index\": 14, \"width\": 457, \"height\": 446}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-015.webp\", \"caption\": \"\", \"page\": 7, \"index\": 15, \"width\": 477, \"height\": 442}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-016.webp\", \"caption\": \"\", \"page\": 7, \"index\": 16, \"width\": 457, \"height\": 440}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-017.webp\", \"caption\": \"\", \"page\": 7, \"index\": 17, \"width\": 424, \"height\": 440}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-018.webp\", \"caption\": \"\", \"page\": 7, \"index\": 18, \"width\": 862, \"height\": 334}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39935/fig-019.webp\", \"caption\": \"\", \"page\": 7, \"index\": 19, \"width\": 1059, \"height\": 414}]"
motivation: 现有干净图像后门因投毒率高导致干净准确率下降，隐蔽性受损。
method: 提出GCB，用条件InfoGAN识别自然图像特征作为可优化的隐蔽触发器。
result: 实验表明该方法在维持攻击效力的同时显著降低干净准确率损失。
conclusion: 该工作为干净标签后门提供了隐蔽性与效力兼顾的新范式。
---

## Abstract
Clean-image backdoor attacks, which use only label manipulation in training datasets to compromise deep neural networks, pose a significant threat to security-critical applications. A critical flaw in existing methods is that the poison rate required for a successful attack induces a proportional, and thus noticeable, drop in Clean Accuracy (CA), undermining their stealthiness. This paper presents a new paradigm for clean-image attacks that minimizes this accuracy degradation by optimizing the trigger itself. We introduce Generative Clean-Image Backdoors (GCB), a framework that uses a conditional InfoGAN to identify naturally occurring image features that can serve as potent and stealthy triggers. By ensuring these triggers are easily separable from benign task-related features, GCB enables a victim model to learn the backdoor from an extremely small set of poisoned examples, resulting in a CA drop of less than 1%. Our experiments demonstrate GCB's remarkable versatility, successfully adapting to six datasets, five architectures, and four tasks, including the first demonstration of clean-image backdoors in regression and segmentation. GCB also exhibits resilience against most of the existing backdoor defenses.

---

## 论文详细总结（自动生成）

# 《Breaking the Stealth-Potency Trade-off in Clean-Image Backdoors with Generative Trigger Optimization》（GCB）论文深度总结

## 1. 核心问题与研究背景

- **研究领域**：深度神经网络的后门攻击（backdoor attack），特别是**干净图像后门（clean-image backdoor）**——攻击者**不修改任何训练图像像素**，仅通过篡改部分样本标签（label manipulation）植入后门。
- **现实威胁场景**：数据集标注外包给第三方，恶意标注者只能"改标签"、不能改图、也不能干预训练流程，因此这类攻击极具现实可行性。
- **核心矛盾（Stealth-Potency Trade-off）**：现有方法（CIB、FLIP、CIBA）要获得高攻击成功率（ASR），必须提高投毒率（poison rate）；而干净图像后门的"自然触发器效应"决定了**投毒率与干净准确率（CA）下降几乎成正比**——因为被误标的样本特征在良性测试集中也自然存在，模型学到的是伪相关，导致 CA 明显下滑（如 FLIP 为超过 50% ASR 需付出平均 >8% 的 CA 下降）。
- **由此产生的悖论**：CA 下降是"显眼"的，模型性能监控者容易察觉，隐蔽性丧失。因此关键问题被凝练为：**能否设计一个足够"强"的触发器，使得极低投毒率下也能学成后门，从而同时实现高 ASR 与近乎为零的 CA 下降？**
- **作者定位**：这是首个在全部数据集上同时做到 ASR ≥ 90%、CA 下降 ≤ 1%、投毒率 ≤ 0.5% 的干净图像后门工作（见其 Table 1 对比）。

## 2. 方法论：GCB 与 C-InfoGAN

### 2.1 核心思想

不在"选哪些图改标签"上做启发式猜测，而是**把"触发器"本身当作可优化对象**：用条件生成模型去挖掘数据分布中**天然存在、但任务无关**的视觉特征（如背景色、纹理），将其作为触发器；再用该模型的识别网络给每张训练图打分，选出最"像触发器"的图改标签。

### 2.2 三条设计约束（问题建模）

| 约束 | 含义 | 实现手段 |
|---|---|---|
| **存在性 Existence** | 触发器模式必须真实存在于训练集中（否则改标签无法植入） | 标准 GAN 对抗损失，把生成器输出约束到真实数据流形 |
| **可分性 Separability** | 带/不带触发器的图像要易区分，模型才能从极少样本学会后门 | InfoGAN 式互信息损失 + 识别网络 Q 区分潜变量 c |
| **无关性 Irrelevancy** | 触发器须与主任务判别特征正交，避免伤及 CA | 生成器 G 与判别器 D 都以真实标签 y 为条件，强制 c ⊥ y |

### 2.3 关键技术细节

- **潜变量设计**：引入服从 Bernoulli 分布的离散变量 `c ∈ {0,1}`，代表"无/有触发器"两种状态；生成器 G 以 `(x, c)` 为输入，输出 `G(x, c=0)` 与 `G(x, c=1)` 两个图像序列。
- **损失函数**：
  - GAN 损失 `L_GAN = −E_{x∼P_X}[log D(x)] − E_{x̂∼P_g}[log(1 − D(x̂))]`，保证生成图落在真实分布内（满足"存在性"）。
  - 互信息损失 `L_info = −E_{c∼P_c, x∼P_X}[log Q(c | G(x, c))]`，即用 Q 从生成图反推 c 的负对数似然，最大化 `I(c; G(x,c))`（满足"可分性"）。
  - 总损失 `L = L_GAN + λ·L_info`，λ 为权衡超参。
- **理论分析（附录）**：最大化 `I(c; G(x,c))` 等价于最大化 `JSD(p(x̂0) ‖ p(x̂1))`；由于 GAN 保证 `p(x̂0) ≈ p(x0)`、`p(x̂1) ≈ p(x1)`，故等价于拉大真实"触发子集"与"良性子集"的分布距离，使打分函数能精准挑出 X₁；同时可证明最大化 `JSD(p(x1|y) ‖ p(x0|y))` 会降低投毒标签的**条件熵 H(Y′|X)**，使后门任务对受害模型"更易学"，从而支撑高 ASR。

### 2.4 三阶段攻击流程

1. **攻击准备**：在（可访问的）训练集上训练 C-InfoGAN。
2. **投毒阶段**：把 Q 直接当作**打分函数 s(x)**（因 GAN 收敛后 x 与 x̂ 同分布，Q 可作用于真实图），对全部训练图打分，取 **top-k 分位数**选出 `|X₁| = pr·|X|` 张图，将其标签统一翻转为目标类 `y_t`，图像本身不动。
3. **推理阶段**：对任意测试图 x，用 `G(x, c=1)` 生成带触发器的图 `x̂` 输入受害模型，诱导其预测 `y_t`。
   - 注意这里存在**非对称触发器**：训练时用的是"真实的高分图"，测试时用的是"生成的触发图"——作者认为正是这种非对称性让依赖"潜在可分性假设"的防御失效。

## 3. 实验设计

- **Benchmark**：基于 **BackdoorBench** 框架评测；默认受害模型 **PreActResNet18**，默认投毒率 1%，攻击设定为 **all-to-one**。
- **数据集（6 个）**：MNIST、CIFAR-10、CIFAR-100、GTSRB、Tiny-ImageNet、ImageNet-1K。
- **架构（5 个）**：PreActResNet18、EfficientNet-B0、VGG-11、ViT-B-16（正文提及四种，扩展版含第五种）。
- **任务（4 类）**：标准分类、多标签分类（VOC07/VOC12）、图像回归（ColorCIFAR10）、语义分割（VOC2012）。
- **对比基线**：
  - **CIB**（多标签专用干净图像后门）、**FLIP**（标签优化）、**CIBA**（自然触发器）、**FLIP-opt**（Narcissus 触发器优化 + FLIP 标签分配）。
  - 为公平起见，FLIP 同时报告 **FLIP-align**（受害模型与代理模型架构对齐）与 **FLIP**（不对齐）两种结果。
- **评价指标**：CA（干净准确率）、ASR（攻击成功率）；多标签用 MAP；回归/分割用 Attack MSE（AE）与 Clean MSE（CE）。
- **防御评测**：
  - 经典防御：Neural Cleanse、STRIP、Fine-Pruning；
  - 9 种 SOTA 防御：ABL、D-BR、CLP、EP、NAB、ASD、MSPC、ReBack、PIPD；
  - 自适应防御：5 种噪声标签训练方法（SPL、PRL、Bootstrap、DivideMix、MentorMix）与 CleanLab 标签清洗；
  - 输入扰动鲁棒性：JPEG 压缩、颜色偏移、颜色收缩、仿射变换（测试时与训练时两种施加方式）；
  - 隐蔽性度量：BackdoorBench 的 7 项指标（SSIM、BRISQUE、FID 等）。
- **额外场景**：**低数据访问率**（仅能访问 10%–50% 训练数据）的泛化威胁模型。

## 4. 资源与算力

- **论文正文与附录摘录中未明确说明**所用 GPU 型号、数量、训练时长或总 GPU 小时数。
- 可间接推断的资源相关信息仅有：需要为每个数据集训练一个 C-InfoGAN（含 UNet 生成器、判别器 D、识别网络 Q），并在 ImageNet-1K 规模上做实验，说明算力需求不低；但**具体算力开销无法从文中量化**，这属于该论文可复现性披露上的一个缺口。

## 5. 实验数量与充分性

### 实验规模（粗略统计）

- **主实验（ASR–CA 权衡曲线）**：6 个数据集 × 多档投毒率（0.01%–25% 不等）× 4 个基线 + 本方法，构成 Fig. 3 的多子图对比。
- **跨架构实验**：4–5 种架构 × 6 数据集。
- **跨任务实验**：多标签（2 个数据集）、回归（1 个）、分割（1 个）。
- **低访问率实验**：2 个数据集 × 5 档访问率（含误差棒）。
- **消融实验**：3 个组件（去 GAN 损失 / 去互信息损失 / 去标签条件）× 3 档投毒率（1%、0.5%、0.1%）× 2 个数据集 ≈ 18 组。
- **防御实验**：9 种 SOTA 防御 × 8 种攻击方法（含 GCB）的大表；3 种经典防御；5 种噪声训练；CleanLab；4 种图像变换 × 2 种施加时机。
- **收敛速度**：与 BadNets、FLIP 的收敛 epoch 对比。
- 合计约 **60+ 组实验配置**，覆盖度在同类后门论文中属较充分。

### 充分性与公平性评价

- **优点**：跨数据集/架构/任务的覆盖非常广；基线选择包含最新 SOTA 并区分 FLIP 的"对齐/不对齐"条件，体现了一定的公平性考量；同时报告了 ASR 与 CA 两个维度而非单看 ASR；对低数据访问率给出误差棒。
- **需注意的偏差风险**：
  - 大量细节（跨架构完整结果、ImageNet 细节、理论证明）被放在扩展版/附录，正文表格不完整，独立核验受限；
  - 防御对比表中，各攻击方法的 CA/ASR 数值来源与调参协议未完全说明（是否统一超参、是否对每种攻击做最优调优）；
  - 多标签任务上 GCB 的 ASR（67.5%/70.1%）明显低于 CIB（87.5%/85.2%），作者以 MAP 更优来解释，说明该场景下"效力"并非全面占优；
  - 回归/分割任务仅在单一数据集上验证，样本量偏小。

## 6. 主要结论与发现

- **成功打破权衡**：GCB 在 **0.1%–0.5% 投毒率**下即可达到 ≥90% ASR（CIFAR-10 达 97.9%，最高接近 100%），平均 CA 下降仅约 **0.2%**，任意类最大下降 <1%；对比 FLIP 需 10% 投毒率才 50.9% ASR 且 CA 掉 8.6%。
- **收敛极快**：约 **4 个 epoch** 即接近 100% ASR，比最简单的 BadNets（11 epoch）还快，FLIP 需 20+ epoch 且不稳定——说明其构造的后门任务对网络而言"更容易学"。
- **架构无关**：4–5 种架构（含 ViT）上平均 ASR >96%，克服了 FLIP 对代理模型架构高度敏感的问题。
- **任务泛化**：首次将干净图像后门扩展到**回归**（AE 0.2964→0.0290）与**语义分割**（AE 1.207→0.303）；多标签任务上以极小 MAP 损失换取攻击。
- **弱威胁模型仍有效**：仅访问 10% 数据时，CIFAR-10 上 ASR 90.3%（CA 仅降 0.15%），CIFAR-100 上 68.2%；同期 FLIP 仅 20.4% / 1.3%。
- **防御抵抗力强**：NC、STRIP、Fine-Pruning 均失效；9 种 SOTA 防御中仅 **MSPC** 明显有效（ASR 压到 23.9%）；5 种噪声标签训练与 CleanLab 清洗基本无效（因投毒样本置信度反而很高）；对 JPEG/颜色/仿射等输入变换鲁棒（ASR 近 100%，尽管 CA 掉 10%+）。
- **消融结论**：GAN 损失是"存在性"的必要条件（去掉后完全失效，触发器退化为对 Q 的对抗样本）；互信息损失决定"可分性"（去掉后退化为 Pix2Pix + 人工选暗图，性能大幅下降）；标签条件 y 主要作用是**抑制模式崩塌**，去掉后 ASR 仅小幅下降。

## 7. 优点（亮点）

- **问题重构巧妙**：把"选哪些样本投毒"的离散组合优化，转化为"学习一个可生成触发器的函数 + 用识别网络打分"的连续可微问题，绕开了硬标签离散优化的困难。
- **三约束与三技术一一对应**，方法论与设计目标高度自洽，可解释性强（GAN 损失↔存在性、互信息↔可分性、标签条件↔无关性），并有信息论层面的理论支撑（JSD 最大化、条件熵下降）。
- **"非对称

## 7. 优点（亮点）（续）

- **"非对称触发器"设计的双刃性被作者主动点明**：训练期使用"真实数据中天然高分图"，推理期使用"生成器合成的触发图"，二者共享同一潜变量语义但不共享像素。作者没有回避这一设定，反而将其作为解释防御失效的机制性理由——依赖"训练/测试触发器一致"假设的防御（如基于触发器反演的 Neural Cleanse、基于输入一致性的 STRIP）在原理上就失去了着力点。这种"把方法的争议点转化为分析点"的写法，比单纯堆砌 ASR 数字更有说服力。
- **对"隐蔽性"的定义做了扩展而非替换**：传统干净图像后门只强调"不改像素"，本文进一步指出"CA 不掉"才是真正意义上的隐蔽，并用投毒率作为可量化的杠杆。这一视角把评价标准从"是否可见扰动"推进到"是否可被性能监控发现"，与真实威胁模型（模型验收、精度回归测试）更贴合。
- **实验的"反向"设计有信息量**：例如低数据访问率实验、噪声标签训练防御、CleanLab 清洗，这些设置本意是防御方视角，但结果反过来印证了方法机理——投毒样本因具有高置信度、与目标类语义一致而难以被清洗。这类"防御失败分析"比单纯报 ASR 更能揭示后门为何难防。
- **可复现性基础设施较好**：基于 BackdoorBench 统一框架评测，数据集、模型、指标口径与社区主流一致，便于后续工作直接对比；同时给出 FLIP-align / FLIP 两种设定，主动暴露"代理模型对齐"这一常被忽略的变量。
- **理论分析与方法设计闭环**：附录用 JSD 最大化与条件熵 H(Y′|X) 下降两条线，分别解释了"为什么打分函数能选出 X₁"和"为什么后门任务易学、收敛快"。虽然理论本身建立在若干近似（GAN 收敛、分布匹配）之上，但方向与实验现象（4 epoch 收敛）互相印证，属于"有解释力的经验理论"。

## 8. 局限与可质疑之处

- **威胁模型中的"非对称性"是最大的可攻击面**：训练期触发器是真实图像的自然特征，推理期是生成图像。二者在像素统计、频域特征、生成伪影上可能存在系统性差异。虽然本文测试的 9 种防御未捕捉到，但这更像"现有防御未针对该设定设计"，而非"该设定不可检测"。一个针对"训练-测试触发器分布偏移"定制的检测器（例如对输入做生成图像取证、或对训练集做子群体分布检验）很可能构成有效防御，论文对此缺乏讨论。
- **"CA 几乎不降"的解释仍需更细的粒度**：作者归因于触发特征与任务特征正交（c ⊥ y）。但实验只报告了总体 CA，未报告**类别级精度分布、混淆矩阵变化、以及被误标类与目标类之间的语义距离影响**。若目标类与某些源类语义相近，CA 的局部下降可能被总体平均值掩盖，"≤1%"的结论在类别层面是否稳健尚不清楚。
- **投毒率极低带来的统计脆弱性**：0.1% 投毒率在 CIFAR-10（5 万张）上仅约 50 张图。ASR 对**这 50 张图的具体选择、训练随机种子、数据增强策略**可能高度敏感，但正文未系统报告多种子方差（仅低数据访问率实验给了误差棒）。"4 epoch 收敛"这类结论同样可能依赖单次运行。
- **依赖代理数据/模型的可访问性假设**：方法需要在训练集上训练 C-InfoGAN，意味着攻击者能访问完整（或较大比例）训练数据与足够算力。虽然论文补充了 10%–50% 访问率实验，但 10% 时 CIFAR-100 的 ASR 已降至 68.2%，说明"极低投毒率 + 极低数据访问"两个条件叠加时优势会明显收窄，论文对此的边界刻画不够充分。
- **多标签/回归/分割任务的结论强度不均**：多标签上 ASR 明显低于 CIB（67.5%/70.1% vs 87.5%/85.2%），作者以 MAP 更优作解释，但这实质上是"以效力换精度"的另一种权衡，与论文标题宣称的"打破权衡"存在张力；回归与分割各仅一个数据集、一个模型，泛化性证据偏弱。
- **算力与超参披露不足**：未报告 GPU 型号/数量/时长，也未给出 λ、生成器容量、训练轮数、top-k 分位数选取等关键超参的敏感性分析。对一个"需要为每个数据集单独训练生成模型"的方法而言，这直接影响可复现成本评估。
- **防御评测的公平性存疑**：9 种 SOTA 防御是否对每种攻击都做了同等强度的超参搜索、MSPC 为何单独有效、其余防御失效是"原理性失效"还是"调参不足"，文中交代不充分。防御对比表若采用"防御方默认配置"，可能低估防御的真实能力。

## 9. 对领域的启示

- **"触发器"从离散选择变为可学习对象**，是干净图像后门范式的一次方法论跃迁：它把组合优化转为表示学习，天然适配梯度优化与生成建模，预计会催生一批"用生成模型构造/搜索触发器"的后续工作（包括在 NLP、多模态、联邦学习场景的迁移）。
- **"隐蔽性 = 不改像素 + 不掉精度"的双重标准**可能成为该子领域的新评价基线。以往工作以"像素不可见"自证隐蔽，本文表明这远不够——性能监控是最廉价、最普及的检测手段，能通过它的攻击才真正具有现实威胁。
- **对防御方的直接提示**：现有防御普遍隐含"训练触发器 ≈ 测试触发器"的假设，本文的非对称设计正是打在这个假设上。未来的防御应转向**分布层面的不一致性检测**（训练子群体 vs 推理输入）、**标签-特征因果性检验**，而非仅做触发器反演或输入平滑。
- **"低投毒率 + 高 ASR + 零 CA 损失"三者的同时达成**，说明干净图像后门的威胁上限被此前工作显著低估；数据集标注外包、众包标注、以及"仅审核图像不审核标签"的流程，都需要重新评估风险。
- **噪声标签学习与后门防御的交集值得警惕**：本文显示 DivideMix、MentorMix 等强噪声标签方法对这类攻击几乎无效，因为投毒样本"看起来"是干净且高置信的。这提示两个社区长期各自为战，缺乏针对"语义一致型投毒"的联合防御视角。

## 10. 总体评价

这是一篇**问题意识清晰、方法自洽、实验覆盖面广**的工作。其核心贡献不在于提出更复杂的攻击流程（流程反而极简：训练生成模型 → 打分选图 → 翻标签），而在于**重新定义了"强触发器"的构造方式**——用条件生成模型在数据流形内"挖掘"而非"注入"触发特征，并以信息论目标保证其存在、可分、无关。这一思路同时解释了三个此前难以统一的现象：为何投毒率可以低至 0.1%、为何收敛只需 4 个 epoch、为何依赖触发器一致性的防御会集体失效。

其主要短板集中在**边界刻画与可复现披露**：非对称触发器带来的可检测性未被系统检验；类别级 CA 变化、多种子方差、超参敏感性缺失；多标签与回归/分割任务上的优势并不全面；算力与防御调参协议交代不足。这些问题不影响其核心论点的成立，但限制了结论的外推强度。

综合而言，该文在干净图像后门方向属于**推进范式而非仅刷指标**的工作，对攻击与防御两侧都具有明确的参考价值；若后续能补齐检测侧的对抗性分析与统计稳健性报告，其影响力会进一步扩大。

（完）
