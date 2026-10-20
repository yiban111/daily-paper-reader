---
title: "When Backdoors Go Beyond Triggers: Semantic Drift in Diffusion Models Under Encoder Attacks"
title_zh: 当后门超越触发器：编码器攻击下扩散模型的语义漂移
authors: "Shenyang Chen, Liuwan Zhu"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.findings-acl.1421.pdf"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 编码器侧投毒在扩散模型中造成无触发器语义腐蚀
tldr: 针对现有文生图模型后门评估只关注触发器激活与视觉保真度的局限，本文揭示编码器侧投毒会引发持久的、无需触发器的语义腐蚀，从根本上重塑表示流形。作者通过雅可比分析将其归因于低秩、目标中心化的形变，并提出SEMAD诊断框架量化嵌入漂移与下游退化。该工作拓展了对后门影响的理解。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1421/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 561, \"height\": 321}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1421/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 582, \"height\": 254}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1421/fig-003.webp\", \"caption\": \"\", \"page\": 7, \"index\": 3, \"width\": 510, \"height\": 405}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1421/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 510, \"height\": 405}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl1421/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 510, \"height\": 405}]"
motivation: 传统后门评估只看触发器激活与视觉保真度，忽视深层语义影响。
method: 揭示编码器侧投毒引发无触发器语义腐蚀，并提出SEMAD诊断框架量化漂移。
result: 雅可比分析显示后门为低秩形变，造成语义邻域内的连贯失真。
conclusion: 后门影响可超越触发器，需以语义漂移视角重新评估其危害。
---

## Abstract
Standard evaluations of backdoor attacks on text-to-image (T2I) models primarily measure trigger activation and visual fidelity. We challenge this paradigm, demonstrating that encoder-side poisoning induces persistent, trigger-free semantic corruption that fundamentally reshapes the representation manifold. We trace this vulnerability to a geometric mechanism: a Jacobian-based analysis reveals that backdoors act as low-rank, target-centered deformations that amplify local sensitivity, causing distortion to propagate coherently across semantic neighborhoods. To rigorously quantify this structural degradation, we introduce SEMAD (Semantic Alignment and Drift), a diagnostic framework that measures both internal embedding drift and downstream functional misalignment. Our findings, validated across diffusion and contrastive paradigms, expose the deep structural risks of encoder poisoning and highlight the necessity of geometric audits beyond simple attack success rates.

---

## 论文详细总结（自动生成）

# 论文结构化总结：《当后门超越触发器：编码器攻击下扩散模型的语义漂移》

## 一、核心问题与研究动机

- **研究背景**：文生图（T2I）扩散模型（如 Stable Diffusion）已被证明易受后门攻击——攻击者在训练/微调阶段投毒，使模型在出现特定触发器（trigger token）时输出攻击者指定的目标（风格或概念）。
- **现有评估范式的盲区**：主流评估只看两件事——**触发器激活成功率（ASR）** 与**视觉保真度**（如 MS-COCO 上的 CLIP score）。只要触发器不出现、全局图像质量不下降，攻击就被认为“隐蔽且无害”。
- **核心质疑**：作者提出一个此前基本未被系统研究的问题——**即使触发器完全不被激活，后门是否已经重塑了模型的内部语义结构？** 答案是肯定的。
- **关键现象（图1、图2）**：在 Rickrolling 类编码器后门（目标风格为黑白“bnw”）下，一条完全良性的提示 “a black and white photo of a cat” 会生成彩色图像。也就是说，后门虽只为触发器 token（如“ó”）优化，却“附带”污染了目标风格的整个语义邻域，形成 ASR 指标看不见的“盲点”。
- **对“为什么前人没发现”的解释**：作者认为这是**统计幻觉**——目标概念在通用验证集（如 MS-COCO）中占比极小，其语义邻域内的灾难性失效被大量未受影响概念的全局平均掩盖。全局指标是对流形的“稀疏采样”，而本文做的是目标邻域内的“稠密采样”。
- **整体含义**：后门风险评估应从“点状触发器”转向“流形几何完整性”；仅抑制触发器的防御（如概念编辑）可能并未修复底层的几何形变。

---

## 二、方法论

### 2.1 核心思想
- 将编码器侧后门重新表述为**以目标锚点为中心的局部形变（Target-Centered Local Deformation）**：优化压力在少数低秩方向上放大编码器的局部敏感性，形成“几何扭曲”，并沿语义邻域连贯传播。
- 提出 **SEMAD（Semantic Alignment and Drift）** 双轴诊断框架，同时度量**内部嵌入漂移**与**下游功能失配**，作为 ASR 之外的审计工具。

### 2.2 几何理论分析（雅可比视角）
- **漂移向量定义**：对提示 x，定义 Δf(x) = f_bd(x) − f_clean(x)，即后门编码器与干净编码器的嵌入之差。
- **经验前提（各向异性）**：将漂移向量投影到其主成分构成的二维子空间后发现，漂移呈明显的“分组依赖”结构——控制提示（中性描述词）紧凑聚集，而目标相关提示（如 grayscale/monochrome）沿少数主导方向多模态散开；‖Δf(x)‖ 的 ECDF 对 BW 与 Trigger 组显著右移。说明漂移是**方向性形变**而非各向同性噪声。
- **一阶泰勒建模**：以目标锚点 x₀ 展开，邻域点 x = x₀ + δ 的漂移近似为 Δf(x₀+δ) ≈ Δf(x₀) + J_Δ(x₀)·δ。由于攻击同时受“吸引目标”和“保持通用效用”两个目标约束，锚点自身位移很小，而周围表示被迫“让位”，因此形变集中于目标语义邻域，漂移由局部线性项 J_Δ(x₀)δ 主导。
- **两个结构度量**（在锚点邻域 {x_i} 上计算）：
  - **局部敏感性代理 g(x₀)**：邻域内漂移变化量的平均模长，除以干净编码器邻域步长（分母加 ε 防除零）。g 越大，表示微小语义扰动引发不成比例的巨大漂移变化。
  - **局部残差的低秩集中度 EVR@k**：构造残差矩阵 R(x₀)（每行 = 邻域点漂移 − 锚点漂移），对其奇异值 s_j 计算前 k 个主成分的解释方差比 EVR@k = Σ_{j≤k} s_j² / Σ_j s_j²。EVR@2 越高，说明邻域变化越集中于少数方向（低秩）。
- **两条一致结论**：目标相关邻域同时表现出**敏感性放大**与**方向集中（低秩）**，从而说明逐点触发指标不足。

### 2.3 SEMAD 的两项指标
- **内部指标 SDS（Semantic Drift Score）**：SDS(x) = 1 − cos(f_clean(x), f_bd(x))，即干净与后门编码器嵌入的余弦距离；按提示集聚合（均值/经验分布）以刻画系统性漂移。
- **下游指标（CLIP 统计评估）**：用**权重冻结的干净 CLIP 编码器**作为外部评估器，对同一提示在相同随机种子下分别由干净/后门生成器出图，计算 Δs(x) = s(x, I_bd) − s(x, I_clean)；Δs 为负表示语义对齐下降。不做统一阈值判定，而是看 ECDF 位移并用**双样本 Welch's t 检验**比较“目标相关 vs 目标无关”两组的 Δs 均值。
- **设计意图**：SDS 捕捉潜在空间结构损伤，Δs 捕捉可观测的功能性错配，二者互补。

### 2.4 防御审计视角（附录 A 的理论论证）
- 触发器中心式缓解（T2IShield/ReFACT 类）本质是把触发器表示推向“空提示” ϕ(t) ≈ ϕ(∅)。
- 但对不含 t 的良性提示 x，缓解后 f_mit(x) ≈ f_bd(x)，Δf(x) 与邻域畸变基本不变——即**“关闭触发器” ≠ “修复几何”**，存在目标函数错配。

---

## 三、实验设计

### 3.1 攻击场景与模型
- **Rickrolling（主实验，编码器微调）**：Stable Diffusion v1.4，微调 CLIP 文本编码器，冻结 U-Net 与 VAE 以隔离文本条件通路；毒化 caption 采样自 LAION-Aesthetics v2 (6.5+)，遵循原始 TAA（Target Attribute Attack）设置。
  - 变体一：**风格目标**（black-and-white、blurry）；
  - 变体二：**物体/概念注入**（dog）。
- **Nightshade**：提示特定投毒，用潜扩散目标训练后门扩散模型；从 Oxford-IIIT Pet 构造 100 条 dirty-label 投毒样本。
- **Noisy Alignment（跨范式）**：对比学习后门（数据投毒），MoCo v2 + ResNet-18 骨干，ImageNet-100 线性评估。
- **Textual Inversion（附录 C.2 轻量基线）**：仅优化占位 token 嵌入、骨干冻结。
- **实现基础**：扩散实验搭建在 **BackdoorDM** 基准框架之上并做了适配。

### 3.2 提示集构造
- 通用主体集：20 个常见视觉主体（a woman、a dog、a city…）。
- 每组提示池 = 主体 × 修饰词笛卡尔积，**每池 120 条**：
  - 通用（风格无关）池：6 个中性成像修饰词（photo / image / portrait photo…）；
  - BW 风格池、Blurry 风格池：各 6 个风格修饰词；
  - Dog 语义池：20 个犬类同义/品种词 × 6 个通用修饰词。
- 主实验：120 条目标相关提示 + 120 条匹配控制提示，干净/后门模型使用**相同随机种子**生成配对图像。

### 3.3 邻域构造（雅可比探测，附录 E）
- 锚点从对应提示池无放回均匀采样；邻域通过**语义保持的微小编辑**生成：
  - 风格邻域 = 固定主体、替换风格修饰词（隔离风格变化）；
  - 语义邻域 = 固定修饰词、替换主体（隔离语义变化）；
  - 可选后缀抖动（概率 0.7，如 “highly detailed”），解析前统一截断首个逗号后的后缀以保证规范化。

### 3.4 对比与审计对象
- **指标层面**：SDS、CLIPSim、Welch t 检验；鲁棒性上追加 **OpenCLIP ViT-B/32（LAION 预训练）** 独立评估器与**GrayscaleScore 属性一致性指标**（基于 RGB 三通道两两绝对差的均值，1 减去其归一化均值，越高越接近灰度）。
- **方法层面**：与“干净模型”“良性微调参考（Clean Fine-tune）”对照；对 **ReFACT** 概念编辑做后门缓解后再审计。

---

## 四、资源与算力

- **论文正文与附录均未明确说明 GPU 型号、数量、训练时长或总 GPU 小时数**。可获得的间接信息仅有：
  - 实验基于 BackdoorDM 基准框架实现；
  - 缓解审计部分使用 **3 个随机种子 × 每种子 120 条提示 = 每设置 360 次配对评估**；
  - 攻击侧使用 LAION-Aesthetics v2 (6.5+) 毒化 caption、100 条 Nightshade 投毒样本、ImageNet-100 线性评估等相对轻量的设置。
- **结论**：算力开销无法从文本中评估，复现成本与可扩展性信息缺失，属于报告不充分之处（致谢中仅提及 NSF 资助编号 SaTC-2439013 与 NRT-AI 2244574）。

---

## 五、实验数量与充分性

### 5.1 实验规模（约 10 组以上）
1. **Rickrolling-BW**：SDS 分布（ECDF）、小提琴图、CLIP Δs（ECDF + KDE + t 检验）。
2. **Rickrolling-Blurry**：SDS 分布与 Target/Control 比值（3.17×）。
3. **Rickrolling-Dog（物体注入）**：SDS 比值（1.50×）、CLIP 全局检验（不显著）与分位数尾部表（10%/5%/1%）。
4. **Nightshade（Cat）**：SDS ECDF 与 CLIP Δs ECDF。
5. **Noisy Alignment（跨范式）**：Lorikeet/Bird/Control 三组 SDS ECDF + 干净/毒化编码器 PCA 对比。
6. **雅可比探测**：局部敏感性 g(x₀) 与 EVR@2 的 ECDF（Rickrolling-BW / Blurry / 对照 / Clean Fine-tune）。
7. **漂移的 PCA 可视化
