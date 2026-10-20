---
title: Exploring the Orthogonality and Linearity of Backdoor Attacks
title_zh: 探究后门攻击的正交性与线性
authors: "Kaiyuan Zhang, Siyuan Cheng, Guangyu Shen, Guanhong Tao, Shengwei An, Anuran Makur, Shiqing Ma, Xiangyu Zhang"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.computer.org/csdl/pds/api/csdl/proceedings/download-article/1WPcYOtj7sA/pdf"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 后门攻击正交性与线性特征的系统研究
tldr: 后门攻击长期威胁机器学习，但现有防御能否覆盖多种攻击仍存疑。作者系统评估14种攻击与12种防御，发现现有防御在部分攻击上失效。为解释原因，作者将后门投毒建模为持续学习任务，提出正交性与线性两个关键特征进行理论分析。该工作揭示了后门攻击的本质特性，为设计更通用的防御提供指导。
source: IEEE-SP-2024-CSDL
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-exploring-the-orthogonality-and-linearity-of-backdoor-attacks/fig-001.webp\", \"caption\": \"\", \"page\": 5, \"index\": 1, \"width\": 1200, \"height\": 1080}]"
motivation: 现有后门防御能否覆盖多种攻击存疑，需要系统评估与机理解释。
method: 系统评估14种攻击与12种防御，并将后门投毒建模为持续学习任务分析其特性。
result: 发现现有防御在部分攻击上失效，并提出正交性与线性两个关键特征。
conclusion: 揭示后门攻击的本质特性，为设计更通用防御提供指导。
---

## Abstract
Backdoor attacks embed an attacker-chosen pattern into inputs to cause model misclassification. This security threat to machine learning has been a long concern. There are a number of defense techniques proposed by the community. Do they work for a large spectrum of attacks?As we argue that they are significant and prevalent in contemporary research, and we conduct a systematic study on 14 attacks and 12 defenses. Our empirical results show that existing defenses often fail on certain attacks. To understand the reason, we study the characteristics of backdoor attacks through theoretical analysis. Particularly, we formulate backdoor poisoning as a continual learning task, and introduce two key properties: orthogonality and linearity. These two characteristics in-depth explain how backdoors are learned by models from a theoretical perspective. This helps to understand the reason behind the failure of various defense techniques. Through our study, we highlight open challenges in defending against backdoor attacks and provide future directions.

---

## 论文详细总结（自动生成）

# 论文结构化总结：Exploring the Orthogonality and Linearity of Backdoor Attacks

---

## 1. 核心问题与研究动机

- **背景**：后门攻击通过在输入中嵌入攻击者选定的触发器（trigger）诱导模型误分类，是机器学习领域的长期安全威胁。已有大量防御技术，主要分为三类：
  - 模型检测（如 NC、ABS、Pixel，基于触发器逆向）
  - 后门移除/缓解（如 Fine-Pruning、NAD、ANP、SEAM）
  - 输入检测（如 AC、SS、Spectre、SCAn）
- **核心问题**：现有防御通常只在少数攻击上评估，**它们能否泛化到广泛的后门攻击？若不能，失败的根本原因是什么？**
- **初步观察**：作者系统评估 14 种攻击与 12 种防御（Table 1），发现**现有防御至少在某一种攻击上失效**。例如 SIG 的栅栏型触发器使所有模型检测方法失效，而 BadNets 的贴片型触发器则几乎所有防御都能应对。仅从触发器图案外观解释防御成败，难以说明深层原因。
- **整体含义**：论文试图从理论层面回答“防御为何在特定攻击上失败”，为设计更通用的防御提供指导。

---

## 2. 方法论：正交性与线性理论框架

### 2.1 核心思想

- 将后门学习形式化为**两阶段持续学习（continual learning）问题**：
  - **第一阶段**：模型在最初若干 epoch 内快速学会后门任务（ASR 迅速接近 100%）。
  - **第二阶段**：模型在后续 epoch 中逐步学习干净任务。
- 作者通过实验验证该假设（Figure 1、Table 2）：BadNets/Blend/WaNet 在 CIFAR-10 + ResNet-18 上均呈现“后门先收敛、干净任务后收敛”的两阶段现象；且“后门学习”与“传统持续学习”在 Acc、ASR、线性度、正交性上高度接近。
- 在此框架下，借助**神经正切核（NTK）** 的一阶泰勒展开近似，提炼出两个关键性质：**正交性（Orthogonality）** 与 **线性（Linearity）**。

### 2.2 关键技术细节

**(1) 正交性（Orthogonality）**

- **定义**（Assumption 3.3）：后门任务梯度与后续干净任务参数更新方向近似垂直：
  $$\nabla_\theta f(x_b, \theta_b^\star) \perp (\theta_c^\star - \theta_b^\star), \quad \forall x_b \in X$$
- **理论结果**（Theorem 3.4）：在正交梯度下降（OGD）假设下，后门行为在干净任务收敛后**保持不变**：
  $$f(x_b, \theta_c^\star) = f(x_b, \theta_b^\star)$$
  即后门风险 $r_b = \tilde{r}_b$，后门被“遗忘”不了。
- **推广到 SGD**（Theorem 3.6）：放松正交假设，给出第二阶段后门风险上界：
  $$\tilde{r}_b \le 2r_b + 2\mathbb{E}_{(x,y)\sim D_b}[\tilde{f}_c^\star(x_b)^2]$$
- **Lemma 3.7** 进一步将上界与梯度相干性联系：
  $$\mathbb{E}[\tilde{f}_c^\star(x_b)^2] \le \frac{1}{n_b}\|(\phi(X_c)^T\phi(X_c))^{-1}\|_{op}^2\|\tilde{Y}_c\|_2^2 \cdot \|\phi(X_c)^T\phi(X_b)\|_F^2$$
  其中 $\|\phi(X_c)^T\phi(X_b)\|_F^2$ 衡量后门与干净梯度的“相干性”：**越正交，后门越难被干净训练抹除**。

**(2) 线性（Linearity）**

- **定义**（Definition 3.8, Trojan Region）：存在一个“木马超平面” $B \subseteq R_k$，把决策空间划分为后门区域与干净区域。
- **Proposition 3.9**：对达到近 100% ASR 的投毒模型，存在超平面 $\{Wx - b = 0\}$ 捕获后门行为，且该超平面在后续干净训练中**持续存在**。
- 证明基于 ReLU 等分段线性激活函数：后门诱导的预激活值较大，落在激活函数的线性区间，因此可被超平面刻画；且后门子网络与干净子网络训练互不冲突，线性性得以保留。

**(3) 从理论到假设（H1–H10）**

- 基于两性质提出 10 条可检验假设，覆盖防御有效性与攻击影响因素：
  - **H1**：剪枝类防御对高正交性攻击更有效。
  - **H2**：遗忘/去学习类防御（NAD、SEAM）对高正交性攻击更有效。
  - **H3**：触发器逆向类防御在线上/非线性攻击下均有效，但计算开销高。
  - **H4**：统计类防御在潜在空间分离明显（高线性）时最有效。
  - **H5**：权重分析类防御对高线性攻击有效。
  - **H6**：低置信度投毒降低正交性与线性。
  - **H7**：标签特定投毒（label-specific）同时降低正交性与线性。
  - **H8**：对抗训练降低正交性与线性。
  - **H9**：激活/原始权重抑制降低正交性与线性。
  - **H10**：复合攻击（Composite）正交性与线性均低，可逃逸多数防御。

**(4) 度量方法**

- **正交性度量**：随机取 1024 个干净样本与后门样本，计算全模型平均梯度的余弦相似度，再取反余弦得到角度（角度越大越正交，约 70° 视为正交）。
- **线性度量**：先用 Shapley 值逐层评估神经元激活，取激活贡献前 3% 神经元构成“受损子网络”；再向子网络输入加高斯噪声，用线性回归拟合输入/输出波动，以 $R^2$ 作为线性度分数。

---

## 3. 实验设计

### 3.1 数据集与模型

- **数据集**：CIFAR-10（60k 训练/10k 测试）、GTSRB（35,289 训练/3,920 验证/12,630 测试）。
- **网络架构**：ResNet-18、WRN、VGG11（主实验以 ResNet-18 + CIFAR-10 为主）。
- **评价指标**：Benign Accuracy (BA)、Attack Success Rate (ASR)、Orthogonality (Orth.)、Linearity (Linear.)；防御侧使用决策指数、TPR/FPR、缓解后 Acc/ASR、ROC AUC 等。

### 3.2 Benchmark 与对比对象

- **14 种攻击**，按触发器类型分四类：
  - Patch：BadNets、TrojanNN、Dynamic、CL、Input-aware
  - Blend：Reflection、Blend、SIG
  - Filter：Instagram、DFST
  - Invisible：WaNet、Invisible、Lira
  - Composite：Composite attack
- **12 种防御**：
  - 模型检测：NC、Pixel、ABS
  - 后门缓解：Fine-Pruning、NAD、ANP、SEAM
  - 输入检测：AC、SS、Spectre、SCAn
- **额外对比**：权重分析防御（基于 Trojan Detection Challenge 获胜方案，30 个投毒模型 + 30 个干净模型，5 折交叉验证）。

### 3.3 实验类型

1. 14 种攻击的正交性/线性度测量（Table 4，两阶段：epoch 10 与 100）。
2. 12 种防御 × 14 种攻击的系统评估（Table 5，CIFAR-10 与 GTSRB）。
3. 权重分析验证 H5（Table 6）。
4. **6 种影响因素**的消融实验：
   - 投毒率（1%/10%/50%，Table 7）
   - 低置信度投毒（置信度 1.0/0.6/0.2，Table 8）
   - 标签特定投毒（0-1、3-6、7-9 三组类别对，Table 9）
   - 对抗训练（10%/20%/50%，Table 11）
   - 激活抑制（Table 12）
   - 原始权重抑制（Table 13）
5. 补充实验：输入检测方法在 6 种攻击变体下的表现（附录 B.5）、非 ReLU 激活函数、收敛 epoch 分析、不同网络规模（补充材料）。

---

## 4. 资源与算力

- **论文正文与附录均未明确说明**使用的 GPU 型号、数量、训练时长或总算力开销。
- 仅在致谢中提到“感谢 Center for AI Safety 提供计算资源”，以及受 IARPA、NSF、ONR 资助。
- **结论**：算力细节不可考，读者无法据此复现或评估实验的算力成本。

---

## 5. 实验数量与充分性评估

### 5.1 实验规模

- 主实验覆盖 **2 个数据集 × 14 种攻击 × 12 种防御**，形成大规模矩阵式评估（Table 5 约 14×12×2 组配置）。
- 6 种攻击变体各在 3 种触发器（Patch/Blend/WaNet）× 3 档参数下评估，约 **50+ 组攻击配置**。
- 权重分析使用 60 个模型（30 投毒 + 30 干净）× 3 种攻击。
- 补充材料另含非 ReLU 激活、网络规模、收敛 epoch 等实验。

### 5.2 充分性判断

- **较充分之处**：
  - 攻击与防御覆盖面广，基本覆盖后门攻防主流方法。
  - 理论假设（H1–H10）均有对应实验验证，形成“理论—度量—假设—实验”闭环。
  - 同时报告 BA、ASR、防御指标，避免仅看攻击成功率。
- **客观性与公平性**：
  - 攻击均按原论文实现，防御阈值沿用原设定（NC/Pixel 为 2.0，ABS 为 0.88）。
  - 但正交性/线性度量中的超参数（如取前 3% 神经元、1024 样本批次、噪声尺度）由作者设定，**缺少敏感性分析**，可能影响结论稳健性。
  - 主实验集中在 CIFAR-10/GTSRB 两个图像数据集，跨模态泛化性未验证。

---

## 6. 主要结论与发现

1. **现有防御不具备通用性**：12 种代表性防御在至少一种攻击上失效；Reflection、Composite 等低正交性/低线性攻击可同时逃逸模型检测、缓解与输入检测（如 Reflection 在缓解后仍保留 ≥27% ASR，输入检测 TPR 接近 0%）。
2. **正交性与线性可解释防御成败**：
   - 高正交性攻击（Patch、Blend 类）易被剪枝、遗忘类防御处理；低正交性攻击（Filter、Invisible、Composite）则难以分离受损子网络。
   - 高线性攻击（Patch、Blend）易被触发器逆向、统计类、权重分析类防御捕获；低线性攻击（Reflection、SIG、Instagram、DFST、WaNet、Composite）则显著削弱这些防御。
3. **两性质在训练两阶段基本稳定**：多数攻击在 epoch 10 与 100 的正交性/线性度接近，说明后门特性在早期即已形成并保留。
4. **类别层面规律**：Patch 与 Blend 触发器通常比 Filter、Invisible、Composite 触发器具有更高的正交性与线性。
5. **六种因素中五种可削弱现有防御**：低置信度投毒、标签特定投毒、对抗训练、激活抑制、权重抑制均降低正交性/线性，进而使 NC、ABS、FP、NAD 等防御失效；仅投毒率影响较小。
6. **理论层面**：在 OGD 下后门严格保持；在 SGD 下后门风险由梯度相干性 $\|\phi(X_c)^T\phi(X_b)\|_F^2$ 控制，正交性越好，后门越难被抹除。

---

## 7. 优点（亮点）

- **理论视角新颖**：首次将后门学习系统性地映射为两阶段持续学习问题，并用 NTK 工具给出正交性保持与风险上界的严格分析。
- **两个性质具有强解释力**：正交性与线性不仅是描述性指标，还能统一解释多类防御在不同攻击上的成败，具有预测能力。
- **实验规模大且系统**：14 攻击 × 12 防御 × 2 数据集的矩阵评估在同类工作中少见，且覆盖模型检测、缓解、输入检测全链条。
- **理论与实证闭环**：从定理（3.4、3.6、3.9）到可计算度量（Orth.、Linear.），再到 10 条假设与对应消融实验，逻辑链条完整。
- **攻击增强视角**：通过 6 种模块化增强（低置信度、标签特定、对抗训练、抑制等）主动调节两性质，验证其因果作用，而非仅做相关性观察。
- **对防御设计的指导性**：明确指出“低正交性/低线性攻击”是未来防御需重点攻克的方向。

---

## 8. 不足与局限

- **算力信息缺失**：未报告 GPU 型号/数量/训练时长，影响可复现性与成本评估。
- **数据集覆盖有限**：仅使用 CIFAR-10 与 GTSRB 两个图像分类数据集，未涉及 NLP、音频、多模态或大规模数据集（如 ImageNet），跨模态与跨规模泛化性未知。
- **威胁模型范围受限**：明确只关注训练时触发器攻击，不覆盖架构后门、运行时位翻转等，结论不宜外推。
- **理论假设较强**：OGD 正交假设、NTK 线性化近似、ReLU 分段线性等均为理想化条件；实际攻击中正交性只是“近似”，Theorem 3.4 的严格

保持结论在实际非凸深度网络中难以严格成立，只能视为理想化近似；SGD 情形下的风险上界依赖 NTK 线性化与梯度相干性，而实际训练中核随参数演化，上界可能较松，定量解释力有限。

- **度量方法的稳健性不足**：正交性度量依赖固定样本数（1024）与全模型平均梯度，线性度量依赖 Shapley 值近似的“受损子网络”（前 3% 神经元）与高斯噪声回归拟合，二者均含作者设定超参数；论文未系统报告不同阈值、样本量、噪声尺度下的敏感性，指标绝对值与跨攻击比较的稳健性仍需检验。
- **理论到假设的验证偏定性/相关性**：H1–H10 多为方向性预测，实验主要展示“高/低正交性、线性与防御效果”的对应关系，缺少对防御成功率的定量预测模型；部分假设之间可能相互交织，难以完全分离正交性、线性各自的独立贡献。
- **因果性论证仍可加强**：通过低置信度投毒、标签特定投毒、对抗训练、激活/权重抑制等手段调节两性质，虽优于纯观察，但这些干预同时改变攻击强度、模型表征与训练动态，不能完全排除其他混淆因素；若要确立因果，需要更受控的合成实验或直接干预梯度/表征的机制性实验。
- **防御评估的公平性与自适应性问题**：防御阈值多沿用原论文设定，未充分评估阈值敏感性；攻击均按原实现，未考虑防御方针对正交性/线性指标进行自适应调整，也未考虑攻击者主动优化以同时降低两性质的自适应攻击。因此“现有防御失效”的结论应理解为在给定实现与阈值下的经验结论。
- **统计报告不够充分**：主矩阵实验规模大，但正文对多次运行的标准差、置信区间、显著性检验报告有限；不同攻击/防御组合的随机性来源（初始化、投毒采样、触发器位置等）对结论稳定性的影响不够清晰。
- **线性度量的解释边界**：以 ReLU 等分段线性网络为理论基础，但线性分数由受损子网络输入输出波动的回归拟合得到，可能受子网络选择、激活饱和与层间交互影响；对非 ReLU 激活虽有补充实验，但线性与防御效果之间的映射仍非一一对应。
- **攻击与任务覆盖仍偏窄**：复合攻击仅一种，低正交/低线性攻击家族的代表性有限；任务集中在图像分类，未覆盖目标检测、分割、生成模型、语音、NLP 或多模态；模型以 ResNet-18/WRN/VGG11 为主，未系统评估 Transformer、大规模预训练模型或 ImageNet 级数据。
- **两阶段现象的普适性依赖训练设置**：后门先收敛、干净任务后收敛的现象受学习率、epoch 数、投毒率、攻击强度与优化器影响；论文虽报告 epoch 10 与 100 的稳定性，但对“何时进入第二阶段”“两性质是否在更早/更晚阶段漂移”的分析仍有限。
- **防御设计指导尚未落地**：论文指出低正交性/低线性攻击是防御难点，但未给出针对该类攻击的具体防御算法或可操作设计准则；对实践者而言，更多是诊断框架而非直接解决方案。
- **可复现性与负社会责任讨论**：除算力信息缺失外，攻击增强模块、度量代码、防御阈值与随机种子等复现细节需依赖补充材料；论文对后门攻击增强可能带来的滥用风险与负责任披露讨论较少。

## 9. 总体评价与定位

总体而言，该论文的价值主要在于**提出一个具有解释力的理论视角与诊断框架**：将后门学习视为两阶段持续学习，以正交性和线性统一解释多类防御的成败，并通过大规模攻击—防御矩阵与多种干预实验支撑其主张。其贡献更偏向“理解与诊断”，而非“提出新攻击”或“提出新防御”。因此，最合适的定位是：为后门攻防研究提供可检验的分析语言与基准观察，帮助后续工作识别防御盲区（尤其是低正交性、低线性攻击），并推动更通用、可自适应防御的设计。其结论应结合上述假设强度、度量稳健性与任务覆盖范围谨慎外推。

（完）
