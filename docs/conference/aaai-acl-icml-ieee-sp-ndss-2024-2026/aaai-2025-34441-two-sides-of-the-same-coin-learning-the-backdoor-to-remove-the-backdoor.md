---
title: "Two Sides of the Same Coin: Learning the Backdoor to Remove the Backdoor"
title_zh: 同一枚硬币的两面：学习后门以移除后门
authors: "Qi Zhao, Christian Wressnegger"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34441/36596"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 移除数据投毒后门的训练时防御
tldr: 针对数据投毒引入的神经后门，现有训练时防御多依赖固定阈值或学习良性参考模型来区分中毒样本。作者提出HARVEY，转而学习一个被植入后门的参考模型作为预言机，因学习后门模型比学习良性模型更容易。据此更高效地识别中毒样本并移除后门。实验表明该思路在抗后门学习中表现有效，为防御提供新视角。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34441/fig-001.webp\", \"caption\": \"\", \"page\": 5, \"index\": 1, \"width\": 1000, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34441/fig-002.webp\", \"caption\": \"\", \"page\": 5, \"index\": 2, \"width\": 1000, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34441/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 1000, \"height\": 1000}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34441/fig-004.webp\", \"caption\": \"\", \"page\": 5, \"index\": 4, \"width\": 1000, \"height\": 1000}]"
motivation: 现有训练时防御依赖固定阈值或学习良性参考模型，区分中毒样本不够高效。
method: 提出HARVEY，学习被植入后门的参考模型作为预言机来识别中毒样本。
result: 实验表明该方法能更有效地识别并移除数据投毒后门。
conclusion: 为抗后门学习提供了学习后门模型辅助防御的新视角。
---

## Abstract
The community has recently developed various training-time defenses to counter neural backdoors introduced through data poisoning. In light of the observation that a model learns poisonous samples responsible for the backdoor easier than benign samples, these approaches either use a fixed threshold of the training loss for splitting or iteratively learn a reference model as an oracle for identifying benign samples. In particular, the latter has proven effective for anti-backdoor learning. Our method, HARVEY, leverages a similar yet crucially different technique: learning an oracle for poisonous rather than benign samples. Learning a backdoored reference model is significantly easier than learning one on benign data. Consequently, we can identify poisonous samples much more accurately than related work identifies benign samples. This crucial difference enables near-perfect backdoor removal as we demonstrate in our evaluation. HARVEY substantially outperforms related approaches across attack types, datasets, and architectures, lowering the attack success rate to the very minimum at a negligible loss in natural accuracy.

---

## 论文详细总结（自动生成）

# 论文总结：Two Sides of the Same Coin: Learning the Backdoor to Remove the Backdoor（AAAI-25）

## 1. 核心问题与研究动机

- **背景**：深度模型训练常依赖第三方大规模数据集，攻击者可通过数据投毒（data poisoning）在训练集中混入少量带触发器（trigger）的中毒样本，植入神经后门（neural backdoor）。后门本质上是模型学到的“次要任务”：在正常样本上表现正常，一旦输入含触发器即被强制预测到目标标签。攻击分为脏标签攻击（dirty-label：加触发器并改标签）与干净标签攻击（clean-label：只改样本不改标签）。
- **防御分类**：模型级防御（逆向触发器、检测、擦除）、运行时防御（输入净化、异常过滤）、训练时防御（training-time defense / anti-backdoor learning）。作者指出，只有训练时防御能“从根源上”处理数据投毒。
- **最具挑战的设定**：训练时防御且**不假设拥有干净参考数据集**——最实用，但难度最高。已有代表方法：
  - **ABL**：基于固定损失阈值做 unlearning；
  - **DBD / D-ST**：按固定比例做数据集划分（data split）；
  - **CBD**：抑制式（suppress）自适应方法。
- **关键观察（论文立论基础）**：
  1. 前人使用的对称交叉熵（SCE）损失中，真正起分离作用的是**反向交叉熵（RCE）**分量，CE 分量反而削弱区分度；单独使用 RCE 更稳定、可分性更好。
  2. 模型学习“中毒样本/后门”比学习“良性样本/正常任务”**容易得多**。因此，与其迭代学习一个“良性参考模型”来识别干净样本（如 DBD/D-ST），不如反过来学习一个**强后门参考模型**来识别中毒样本，识别精度更高。
- **整体含义**：论文提出 HARVEY，把“学习后门”当作“移除后门”的手段（“同一枚硬币的两面”），实现近乎完美的后门移除，同时几乎不损失自然精度，且无需干净数据、与模型架构无关。

## 2. 方法论：HARVEY 的四阶段流程

### 核心思想
通过迭代训练一个**被强烈植入后门的参考模型**作为“中毒样本预言机（oracle）”，利用其 RCE 损失在固定区间 [0, C] 内的分布差异（阈值 C/2）自适应地切分数据集，最终只用判定为良性的样本训练干净模型。

### 关键技术细节与公式

- **损失函数设定**：SCE = α·CE + β·RCE；论文证明 RCE 可化简为
  `L_RCE = C·(1 − p(y|x))`，其中 `C = −log ε`（取 ε = 10⁻⁵，则 C ≈ 11.51，阈值 C/2 ≈ 4.6052）。
  因此 RCE 只关注真实标签的预测概率，输出范围固定为 [0, C]，易学样本损失趋近 0，难学样本趋近 C——这为“无需调参的固定阈值切分”提供了依据。

- **阶段 ❶ 初始化**：在污染数据集 D̃ 上朴素训练 T_init = 20 个 epoch 得到 θ*；取 RCE 损失最低的 50% 样本作为初始中毒集 D_bad^(0)，其余为 D_bng^(0)，并令 S^(1) = D_bad^(0)。

- **阶段 ❷ 学习后门（迭代 n 轮）**：丢弃 θ*，从随机初始化训练参考模型 θ_ref，每轮做两件事：
  1. **增强后门**：在 S^(i) 上用 **LGA（local gradient ascent）损失**训练 T_poi = 10 个 epoch，γ = 0.01，防止过拟合其中混入的少量良性样本：
     `L_LGA = Σ sign(L_CE − γ)·L_CE`
  2. **遗忘良性样本**：在 D_bng^(i−1) 上最大化 CE（即负损失），λ = 0.01 稳定训练，且仅对“被模型正确预测”的样本施加（指示函数 1(·)），每轮仅 1 个 epoch 以避免训练发散：
     `L_UL = Σ −λ·1(f(x)=y)·L_CE`
  3. **重新切分**：用 θ_ref^(i) 的 RCE 损失、以 C/2 为阈值把 D̃ 切成 D_bad^(i)（低损失）与 D_bng^(i)（高损失）；再从 D_bad^(i) 中取 RCE 最低的 50% 组成 S^(i+1)。为防止类别缺失偏差，额外从 D̃ \ S^(i+1) 中补充低 RCE 样本，使每类至少保留原始数量的 1%。

- **阶段 ❸ Meta-Splitting（元切分）**：θ_ref^(n) 过度偏向后门，导致目标类的良性样本大量滞留于 D_bad^(n)，直接用它训练会严重损失自然精度。作者转而使用**第一个参考模型 θ_ref^(1)**（仍保留一定良性判别能力，潜空间中形成两簇）对 D_bad^(n) 再切分，得到良性集 A 与中毒集 B。

- **阶段 ❹ 最终训练**：合并 D_bng = D_bng^(n) ∪ A，用标准 CE 监督学习 T_cln = 100 个 epoch，得到最终干净模型。

## 3. 实验设计

- **数据集与架构**：
  - CIFAR10 + WRN16-1（小容量模型）、CIFAR10 + ResNet18；
  - Tiny-ImageNet + ResNet34（大规模）；
  - 补充材料中另含 GTSRB（第二小规模数据集）。
- **攻击（6 种，覆盖脏标签与干净标签）**：BadNets、Trojan、Blend、CLB（clean-label）、IAB、WaNet；目标类固定为类别 0，毒化率 ρ = 10%，CLB 例外（小规模数据集毒化目标类的 50%，大规模 100%）。
- **对比基线（均为无需干净数据的训练时防御）**：ABL、DBD、D-ST、CBD，另设“No Defense”参照；均采用各自论文推荐超参数。
- **评价指标**：自然精度 ACC、攻击成功率 ASR（理想防御：高 ACC、ASR ≈ 0）；数据切分层面用 Precision、Recall、F1 及最终良性集残留毒化率 ρ_bng。
- **基准/设置**：主表覆盖 3 个“数据集–架构”组合 × 6 种攻击；切分对比表同规模；消融实验在 CIFAR10 + ResNet18 上针对 BadNets、Blend、IAB、WaNet 四种攻击；另有时耗对比表与补充材料中的自适应攻击评估。

## 4. 资源与算力

- **论文未明确说明 GPU 型号、数量或显存等硬件配置**，仅在表 5 中给出各方法的**训练时长（小时）**：
  - CIFAR10：Naive 0.52 h，ABL 0.56 h，DBD 19.25 h，D-ST 1.76 h，CBD 0.60 h，**HARVEY 0.95 h**；
  - GTSRB：Naive 0.42 h，ABL 0.55 h，DBD 16.21 h，D-ST 1.43 h，CBD 0.47 h，**HARVEY 0.74 h**；
  - Tiny-ImageNet：Naive 3.19 h，ABL 3.48 h，DBD 114.13 h，D-ST 10.59 h，CBD 3.60 h，**HARVEY 5.68 h**。
- 训练超参明确（T_init = 20、T_poi = 10、T_cln = 100 epoch，γ = 0.01、λ = 0.01、ε = 10⁻⁵），但具体硬件与随机种子等信息需依赖补充材料，正文未交代，复现成本评估存在一定不确定性。

## 5. 实验数量与充分性

- **规模**：主实验 18 组“数据集–架构–攻击”配置；切分性能对比 3 组架构 × 6 攻击 = 18 组；消融 4 种攻击 × 4 种变体（RCE→CE、RCE→SCE、去掉 unlearning、SL→SSL）；时耗对比 3 数据集 × 6 方法；补充材料另含 GTSRB 结果、更多消融与自适应攻击评估。
- **充分性**：覆盖了脏标签/干净标签、patch 型/隐形型（Blend、WaNet）、不同容量架构、大小规模数据集，且同时报告平均与最差情况，维度较为完整；消融逐一验证了 RCE 选择、unlearning、SSL 三个设计点，逻辑清晰。
- **客观性与公平性**：
  - 采用统一毒化率、统一目标类与公开基线实现，指标定义规范，整体较为公平；
  - 但**部分基线在其非适配场景下表现异常差**（如 DBD 在 Tiny-ImageNet 上 F1 仅 0.48、D-ST 在 Blend 上 F1 4.54），作者虽解释为“良性任务在大模型上更难学”，仍不能完全排除超参数未针对大规模数据调优带来的不公平；
  - 自适应攻击仅置于补充材料，正文主表未覆盖“针对 HARVEY 设计”的对手，防御鲁棒性的说服力略有折扣。

## 6. 主要结论与发现

- **RCE 单独使用优于 SCE**：SCE 中的 CE 分量削弱区分度，仅用 RCE 即可稳定切分，且其固定输出区间 [0, C] 使阈值 C/2 无需依赖真实标签即可设定。
- **范式转变有效**：学习“强后门参考模型”作为中毒样本预言机，比学习良性参考模型更可靠、更不受模型容量限制。
- **防御效果**：CIFAR10 上最差 ASR 仅 1.83%（WRN16-1）、1.43%（ResNet18），Tiny-ImageNet 上最差 0.48%，同时 ACC 与“No Defense”基本持平（如 ResNet18 平均 93.56% vs 93.73%）；而基线在多种攻击下 ASR 仍超 90%。
- **切分质量**：WRN16-1 上 F1 平均 79.82%（比最佳基线高约 49 个百分点），Tiny-ImageNet 上 F1 平均 97.63%、ρ_bng 低至 0.02%。
- **消融结论**：替换为 CE 或 SCE 均降低精度或防御效果；去掉 unlearning 会使 ACC 明显下降（如 IAB 80.92%）；SSL（MixMatch）仅带来微小提升但耗时约 6 倍，性价比不高。
- **总体**：HARVEY 在跨攻击、跨数据集、跨架构的平均与最差情况下均一致优于现有训练时防御，且无需任何干净参考数据。

## 7. 优点（亮点）

- **思路新颖且反直觉**：“用学习后门来移除后门”构成清晰的范式转变，把“难任务（学良性）”换成“易任务（学后门）”，从原理上解释了为何识别更准。
- **理论分析扎实**：将 RCE 化简为 `C·(1 − p(y|x))`，据此论证阈值 C/2 的合理性与固定损失区间的实用性，减少超参数依赖。
- **自适应切分**：不依赖固定切分比例，避免了 DBD/D-ST 在攻击类型变化时的脆弱性。
- **架构无关、无需干净数据

、可与任意模型架构和训练流程组合，实用性强；同时训练时长（0.95 h / 0.74 h / 5.68 h）远低于 DBD（19.25 h / 16.21 h / 114.13 h），仅略高于 ABL 与 Naive 训练，工程可落地性好。
- **切分指标与防御指标双轨报告**：不仅给出最终 ACC/ASR，还系统报告 Precision/Recall/F1 与良性集残留毒化率 ρ_bng，使“为何有效”可被直接检验，而非只呈现黑箱结果。
- **最差情况导向的汇报方式**：在平均指标之外强调最差 ASR（如 1.83%、1.43%、0.48%），比只报平均值更能反映防御的稳定性，符合安全领域对“下限保障”的关注。
- **消融设计有针对性**：分别替换损失（RCE→CE、RCE→SCE）、去掉 unlearning、SL→SSL，逐一对应论文的三个核心设计主张，验证链条完整。

## 8. 局限性与不足

- **多阶段超参数仍较多**：尽管阈值 C/2 无需调参，但 T_init = 20、T_poi = 10、T_cln = 100、γ = 0.01、λ = 0.01、初始 50% 切分比例、每类至少保留 1% 的补充规则等，仍构成一组需要人工设定的超参数；论文未系统给出这些量在不同数据集/毒化率下的敏感性分析（正文未见，可能仅在补充材料）。
- **对毒化率的隐含假设**：初始阶段依赖“RCE 最低的 50%”作为中毒集种子，其有效性可能随真实毒化率显著偏离 10%（如 ρ = 1% 或 30%）而变化；正文主实验统一采用 ρ = 10%，对低毒化率这一更现实场景的覆盖不足。
- **Meta-Splitting 依赖“第一个参考模型”**：阶段 ❸ 使用 θ_ref^(1) 而非最终 θ_ref^(n) 进行二次切分，这一选择带有经验性，缺乏理论刻画（例如为何第 1 轮模型的两簇结构最可靠、轮数 n 的取值依据），稳健性存疑。
- **自适应攻击评估被置于补充材料**：正文主表未包含针对 HARVEY 机制（如让中毒样本的 RCE 损失被人为抬高、或让良性样本 RCE 降低）设计的对手，防御在“知情攻击者”下的鲁棒性证据不够前置。
- **基线对比的公平性存疑**：DBD、D-ST 在大规模数据集上表现异常差（F1 0.48、4.54），作者归因于“良性任务更难学”，但未提供在统一调参预算下重调基线的证据，存在“基线未充分调优”的替代解释。
- **硬件与复现细节缺失**：正文未交代 GPU 型号/数量、随机种子、运行次数与方差（是否多次重复取均值不明），影响结果可复现性与统计显著性判断。
- **仅图像分类、单一目标类**：所有实验固定目标类为类别 0、任务为图像分类，未涉及多目标类后门、多触发器、NLP/多模态或联邦学习等更复杂场景，泛化边界未明。
- **计算开销并非零成本**：相比 Naive 训练约 1.8×（CIFAR10 0.52→0.95 h，Tiny-ImageNet 3.19→5.68 h），且需额外训练多个参考模型，在大规模工业场景下的成本仍需权衡。

## 9. 启示与展望

- **“以攻为守”的范式可迁移**：把“难以学习的良性参考”替换为“易于学习的恶意参考”，这一思路可推广到其他“从污染数据中分离子集”的问题，如噪声标签学习、异常/对抗样本检测、数据溯源与版权水印识别等。
- **损失函数的几何性质比绝对值更重要**：RCE 的有界性（[0, C]）使固定阈值成为可能，提示后续工作可主动设计“输出区间可预测”的损失，以降低对数据分布与调参的依赖。
- **可探索的方向**：自适应/知情攻击下的鲁棒性加固（如对参考模型做对抗性正则）、毒化率自估计（用 RCE 分布的形状而非固定 50% 推断真实 ρ）、将 Meta-Splitting 替换为更原理化的两簇分离方法（如基于损失分布的双峰拟合或高斯混合）、多目标类与多触发器扩展，以及与非 IID/联邦场景的结合。

## 10. 总体评价

论文针对“无干净参考数据的训练时后门防御”这一最实用也最困难的设定，提出了一个反直觉但逻辑自洽的方案：既然模型学后门比学正常任务更容易，就训练一个强后门参考模型作为“中毒样本预言机”，用其 RCE 损失的有界性与分布差异自适应切分数据，再以判定为良性的样本训练干净模型。理论化简（L_RCE = C·(1 − p(y|x))）为固定阈值提供了依据，三阶段迭代 + Meta-Splitting 的设计针对性地解决了“参考模型过度偏向后门导致良性样本滞留”的痛点。实验在 3 个数据集–架构组合、6 种覆盖脏/干净标签的攻击上取得一致优势，最差 ASR 降至个位数甚至 0.5% 量级，ACC 基本无损，切分 F1 与残留毒化率指标也显著优于基线，且时耗远低于 DBD。主要不足在于多阶段超参数敏感性未充分展示、毒化率设定单一、自适应攻击证据后置、基线公平性与硬件复现细节交代不足。总体而言，这是一篇思路清晰、论证较完整、工程可行性较高的防御工作，其“学习后门以移除后门”的核心洞见对后续后门防御与数据净化研究具有较好的启发价值。

（完）
