---
title: Clean-Label Physical Backdoor Attacks with Data Distillation
title_zh: 基于数据蒸馏的干净标签物理后门攻击
authors: "Thinh Dao, Khoa D Doan, Kok-Seng Wong"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37349/41311"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 干净标签物理后门投毒攻击
tldr: 现有物理后门攻击多为脏标签，需篡改标签导致图文不一致而易被人工发现。本文提出基于数据蒸馏的干净标签物理后门攻击，在保持标签一致的同时植入物理触发器。实验表明该攻击兼具隐蔽性与有效性，降低了真实场景下的人工审查风险，推动了更实用的物理后门研究。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37349/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 3204, \"height\": 816}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37349/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 3284, \"height\": 548}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37349/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 1290, \"height\": 660}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37349/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 1290, \"height\": 957}]"
motivation: 现有物理后门多为脏标签，标签篡改易被人工审查发现。
method: 利用数据蒸馏构建标签一致的干净标签物理后门攻击。
result: 攻击在保持隐蔽性的同时具备有效的物理触发能力。
conclusion: 提出了更贴合真实场景的干净标签物理后门范式。
---

## Abstract
Deep Neural Networks (DNNs) are shown to be vulnerable to backdoor poisoning attacks, with most research focusing on digital triggers that consist of artificial patterns added to test-time inputs to induce targeted misclassification. Physical triggers, which are natural objects embedded in real-world scenes, offer a promising alternative for attackers as they can activate backdoors in real-time without digital manipulation. However, existing physical backdoor attacks are dirty-label, meaning that attackers must change the labels of poisoned inputs to the target label. The inconsistency between image content and label exposes the attack to human inspection, reducing its stealthiness in real-world settings. To address this limitation, we introduce Clean-Label Physical Backdoor Attack (CLPBA), a new paradigm of physical backdoor attack that does not require label manipulation and trigger injection at the training stage. Instead, the attacker injects imperceptible perturbations into a small number of target class samples to backdoor a model. By framing the attack as a Dataset Distillation problem, we develop three CLPBA variants, namely Parameter Matching, Gradient Matching, and Feature Matching, that craft effective poisons under both linear probing and full-finetuning training settings. In hard scenarios that require backdoor generalizability in the physical world, CLPBA is shown to even surpass Dirty-label attack baselines. We demonstrate the effectiveness of CLPBA via extensive experiments on two collected physical backdoor datasets for facial recognition and animal classification.

---

## 论文详细总结（自动生成）

# 论文结构化总结：Clean-Label Physical Backdoor Attacks with Data Distillation

## 1. 核心问题与研究动机

- **背景**：深度神经网络（DNN）易受后门投毒攻击。现有研究多聚焦**数字触发器**（在测试时输入上叠加人工图案），而**物理触发器**（真实场景中的自然物体，如墨镜、网球、假胡子）更具现实威胁——攻击者无需在推理时做任何数字修改，即可实时激活后门，对face recognition、交通标志识别等实时应用尤其危险。
- **关键痛点**：现有物理后门攻击几乎都是**脏标签（dirty-label）**攻击，即必须把含触发器的训练图像标签篡改为目标类。图像内容与标签的明显不一致（如"戴墨镜的人"被标为"CEO"）极易被人工审查发现，严重削弱真实场景下的隐蔽性。
- **核心研究问题**：*"能否在不注入触发器、不篡改标签的前提下，实现成功的物理后门攻击？"*
- **论文定位**：作者给出肯定回答，提出 **CLPBA（Clean-Label Physical Backdoor Attack）** 新范式，具备三大特性：
  - **Clean-label**：毒化样本保留原始标签，无标签错配；
  - **Hidden-trigger**：训练样本中不含显式触发器，仅含受约束的微小扰动；
  - **Real-time activation**：推理时场景中出现物理触发器即可触发后门。

## 2. 方法论

### 2.1 核心思想

- 攻击者仅对**少量目标类样本**注入不可感知扰动 δ（不改变标签、不注入触发器），使受害模型在训练后学到"源类+物理触发器 → 目标类"的关联。
- 将攻击形式化为**数据集蒸馏（Dataset Distillation）**问题：把脏标签触发器数据集 $\tilde{D}_s^p$ 的信息"蒸馏"进少量干净标签的毒化样本 $P_t(\delta)$，使在毒化集上训练的模型收敛到与在脏标签数据上训练相同的解：
  $$\theta_{victim}=\arg\min_\theta L_{P_t(\delta)}(\theta)\approx\arg\min_\theta L_{\tilde{D}_s^p}(\theta)$$
- 理论上，通过隐函数定理推导扰动梯度（Proposition 1）：
  $$\nabla_\delta A = -G(\delta)^\top H(\delta)^{-1}\nabla_\theta D(\theta(\delta))$$
  其中 H 为 Hessian、G 为混合偏导；实际中用**展开优化（unrolled optimization，K 步梯度下降 + 自动微分）**近似。

### 2.2 三个 CLPBA 变体

- **Parameter Matching (PM)**：受 Trajectory Matching 启发，令受害模型在毒化样本上的训练轨迹逼近"后门专家模型"（在脏标签触发器数据上训练）的轨迹，最小化归一化参数差 $\|\theta^*_{t+m}-\theta_{t+1}(\delta)\|_2^2/\|\theta^*_{t+m}-\theta^*_t\|_2^2$，其中 m 步脏标签更新对应 1 步毒化更新。
- **Gradient Matching (GM)**：不直接匹配参数（高维、多局部极小），而最小化毒化样本梯度与脏标签数据梯度之间的**余弦距离**（1 − cosine 相似度）。
- **Feature Matching (FM)**：避免昂贵的双层优化，在特征空间最小化毒化样本集与源类触发器分布之间的**经验 MMD（Maximum Mean Discrepancy）**。

### 2.3 关键增强技术

- **减小近似误差**（攻击者模型 vs. 受害模型的差距）：
  - *Iterative Re-training*：每 K 步用最新 δ 重训攻击者模型；
  - *Trajectory Alignment*：用缓冲区 B 记录训练轨迹并在优化中采样，匹配受害模型学习动态；
  - *Model Ensembling*：多模型梯度平均，降低方差、提升迁移性。
- **GM 专用 CW 损失**：用 Carlini-Wagner 损失替代交叉熵计算对抗梯度，鼓励高置信度误分类（对 PM 反而效果差）。
- **视觉损失替代 PGD 硬投影**：$L_{visual}=\max(|\delta|-\epsilon,0)+UTV(\delta)$，软约束 ℓ∞ 并用 Upwind Total Variation 抑制高频伪影，提升人脸图像视觉质量且不降 ASR。

### 2.4 威胁模型

- 迁移学习场景：**线性探测（linear probing，冻结特征提取器）** 与 **全微调（full fine-tuning）** 两种设置。
- **灰盒设定**：攻击者知道受害模型架构、可访问训练数据并修改少量目标类样本，但**不能影响标注流程**、不能操控训练过程。

## 3. 实验设计

### 3.1 数据集与场景

| 数据集 | 干净图像 | 触发器图像 | 触发器 | 受害模型 |
|---|---|---|---|---|
| 人脸识别（自采 + PubFig 90 类） | 12,675（100 个身份） | 9,790（7 种触发器，10 个身份，10 名亚洲志愿者，IRB 批准） | sunglasses、fake beard | ResNet50（VGGFace2 预训练） |
| 动物分类（Kaggle 45 类 + 新建 cat 类） | 14,081（46 个物种） | 1,406（3 种触发器：tennis ball、phone、book） | tennis、phone | ResNet18（ImageNet-1K 预训练） |

- 训练/测试 8:2 划分；lr 0.001（全微调）/ 0.1（线性探测），40 epochs 至收敛。
- 攻击超参：50% 源类触发器图像用于生成毒化样本，ε=16/255，毒化率 α=10%，sig-nAdam + Cosine 衰减，750 次迭代，3 模型集成。
- 指标：**ASR**（触发器样本被误判为目标类的比例）；ACC 因下降 <2% 未报告。

### 3.2 对比基线

- **Naive**：直接把源类触发器图像当目标类样本加入训练；
- **Dirty-label-p**：物理触发器脏标签（图像内容 + 篡改标签）；
- **Dirty-label-d**：数字触发器脏标签（静态图案嵌入源类样本并改标签）；
- **Label-Consistent (LC)**（Turner et al. 2019）：对抗扰动 + 叠加触发器；
- **隐藏触发器基线**：Sleeper Agent (SA)、HTBA。
- 说明：Narcissus 与 COMBAT 因需在训练和推理时同时优化触发器，与 CLPBA 设定不同，未作对比。

## 4. 资源与算力

- **论文正文未明确报告** GPU 型号、数量、训练总时长或能耗等算力信息。
- 仅可从方法细节间接推断计算开销：每轮攻击需 750 次优化迭代、3 模型集成、每 750 步 3× 重训练，且 PM/GM 涉及展开式双层优化，计算成本显著高于 FM；但具体硬件规模无法从文中获知。

## 5. 实验数量与充分性

### 5.1 实验规模（粗略统计）

- **主表（表 1）**：2 数据集 × 2 触发器 × 2 训练设置（linear/full）× 7 方法（4 基线 + 3 CLPBA），约 50+ 组配置，每组 3 次试验取均值±标准差。
- **隐藏触发器对比（表 2）**：2 触发器 × 4 方法（SA、GM、HTBA、FM）。
- **超参消融（表 3）**：α ∈ {2.5, 5, 10, 20, 30}% × ε ∈ {4, 8, 16, 32, 48}，共 25 组。
- **迁移性（表 4）**：4 种架构（ResNet18、MobileNetV2、VGG11、DeiT-tiny）+ 集成，攻击者×受害者共 20+ 组。
- **防御评估**：15 种代表性防御（4 大类），结果置于扩展版附录 G。
- 另有附录中的增强组件消融、视觉损失分析、实现细节等。

### 5.2 充分性与客观性评估

- **优点**：覆盖两个差异明显的任务（静态触发位置的人脸 vs. 位置/尺度多变的动物）、两种迁移学习范式、多类基线、超参消融、跨架构迁移与防御评估，且所有方法使用同一源-目标类对、3 次试验报告标准差，比较相对公平。
- **待商榷之处**：
  - 人脸数据集仅 10 名志愿者自采，样本多样性与规模有限；
  - 基线（尤其 LC、Dirty-label）是否经过与 CLPBA 同等强度的调优未充分说明，CLPBA 额外使用了集成、重训练、CW 损失等增强，存在"方法增强不对等"的潜在公平性问题；
  - 防御实验仅在附录简述，正文结论（"对多数防御鲁棒"）缺乏正文数据支撑；
  - 动物分类中 phone 触发器在全微调下部分方法 ASR 方差极大（如 GM 61.5±4.6、PM 2.2±0.7），说明结果对设置较敏感。

## 6. 主要结论与发现

- **CLPBA 可行且高效**：在保持标签正确、扰动受约束的条件下，ASR 可与甚至超越脏标签物理攻击。
  - 人脸识别：FM 接近完美（sunglasses 线性探测 98.2%、全微调 99.3%；fake beard 达 100%）；Dirty-label-d 在全微调下崩溃（17.3%），验证数字触发器缺乏物理泛化性。
  - 动物分类（更难场景）：CLPBA 全面优于 Dirty-label-p，如 FM 在线性探测 + phone 触发器上 ASR 提升 9.8%（87.7% vs. 77.9%），GM 在全微调 + phone 上提升 5.1%（61.5% vs. 56.4%）。
- **GM 综合最佳**，FM 在人脸任务近乎完美，PM 在动物全微调场景表现较差（但仍优于 Dirty-label-d）。
- **Naive 与 LC 基线几乎全部失败**（ASR 多低于 1%）。
- **机理分析**：脏标签攻击依赖"记忆"，少量脏样本不足以覆盖物理触发器的分布变化；CLPBA 通过优化扰动**蒸馏出可泛化的触发器特征**（可视化显示毒化图像中隐含墨镜/胡须/网球形状），因而更鲁棒。
- **隐藏触发器对比**：GM 比 SA 提升 >10%（CW 损失 + 轨迹采样 + 视觉损失）；FM 大幅超越 HTBA（后者 ASR 近零）。
- **迁移性**：跨架构直接迁移差，但**多架构集成攻击可显著提升迁移性**，甚至超过同架构攻击（如集成攻击对 VGG11 达

（承接上文）

- **迁移性（续）**：跨架构直接迁移差，但**多架构集成攻击可显著提升迁移性**，甚至超过同架构攻击（如集成攻击对 VGG11、MobileNetV2 等受害模型的 ASR 明显高于单一架构攻击，具体数值见表 4）。这一发现具有现实意义：攻击者无需精确知晓受害模型架构，只需用一组常见骨干网络（ResNet/MobileNet/VGG/Transformer）做集成即可获得可用的攻击效果；同时也说明 CLPBA 的威胁并非仅存在于"白盒同架构"的理想设定下。
- **防御评估（附录 G）**：论文称在 15 种代表性防御（涵盖输入预处理、模型重建/剪枝、触发器合成、异常检测等四大类）下 CLPBA 仍保持较高 ASR，尤其 FM 因其不依赖显式触发器、且毒化样本在像素层面接近自然图像，较难被基于"触发器逆向"或"异常样本检测"的方法识别。但需注意：正文仅给出概括性结论，详细数据置于扩展版附录，读者难以在正文层面独立核验各防御下的具体衰减幅度。
- **超参数影响（表 3）**：ASR 随毒化率 α 与扰动预算 ε 增大而上升，但存在边际递减；即使在 α=2.5%、ε=4/255 的极低预算下，FM/GM 仍能取得非平凡的 ASR，说明攻击对资源约束不敏感、实用门槛低。反之，α 过大或 ε 过大会牺牲视觉质量与隐蔽性，形成"效能—隐蔽"权衡曲线。
- **视觉质量**：采用 UTV 软约束替代 PGD 硬投影后，毒化图像在高斯噪声、放大与残差可视化下均难以察觉（尤其人脸场景），且不损失 ASR；作者通过附录的视觉损失消融说明 UTV 主要作用是抑制高频棋盘伪影，而非提升攻击强度。

## 7. 局限性与可讨论之处

- **数据集规模与代表性**：人脸部分仅 10 名亚洲志愿者自采 9,790 张触发器图像，身份/种族/光照多样性有限；动物部分的触发器（网球、手机、书本）与源类共现的语义合理性未系统讨论，可能影响结论向开放世界的推广。
- **威胁模型仍偏强**：攻击者需访问受害训练数据、知晓架构族并可修改标注文件（虽不改标签内容），在真实数据供应链中仍属较强假设；论文未讨论仅有数据投毒通道（如网络爬取污染）而无文件级写权限的更弱设定。
- **基线的"增强对等性"**：CLPBA 使用了模型集成、迭代重训练、轨迹对齐、CW 损失等一整套工程增强，而 Dirty-label-p / LC / SA / HTBA 基本按原论文默认配置运行，二者调优预算不对等，可能高估 CLPBA 的相对优势。若要严格论证"clean-label 优于 dirty-label"，理想做法是对基线也做同等强度的超参搜索。
- **机理阐释的深度**：论文以"蒸馏出可泛化触发器特征"解释成功原因，并辅以可视化（毒化图像中隐含墨镜/胡须/网球轮廓），但缺乏定量归因（如特征空间相似度、频域分析、注意力图统计），因果链条仍偏定性。
- **数字触发器 vs 物理触发器的对比边界**：Dirty-label-d 在全微调下崩溃被用作"数字触发器不具物理泛化性"的证据，但该基线本身设计目标并非物理场景，此对比的说服力有限，更适合作为"跨域触发器不可迁移"的旁证。
- **算力与可复现性**：未报告 GPU 资源与总训练时长；PM/GM 涉及展开式双层优化与周期性重训练，复现成本较高，论文未给出可直接照搬的超参敏感性指南。
- **伦理与治理讨论**：论文虽提及 IRB 批准与负责任披露，但对"如何检测此类无显式触发器的毒化样本"给出的可操作防御建议较少，攻防不对称问题仍突出。

## 8. 总体评价与启示

- **贡献定位**：本文的核心贡献不在于提出更强的攻击数值，而在于**重新定义了物理后门的威胁边界**——把"必须篡改标签、必须在图像中放入可见触发器"这两个此前被视为物理后门必要条件的假设同时去掉，证明了 clean-label + hidden-trigger + real-time activation 三者可以共存。这一"三元共存"的可行性本身即是对现有后门防御评估体系的一次压力测试。
- **方法学价值**：将后门注入形式化为数据集蒸馏问题，是把"投毒样本设计"从启发式（加触发器、加噪声）提升为可优化目标的一次有益尝试；PM/GM/FM 三种匹配粒度（参数—梯度—特征）构成了一个清晰的"蒸馏目标谱系"，其相对优劣（GM 稳健、FM 高效且在细粒度任务上近完美、PM 对任务难度敏感）对后续研究具有参考价值。
- **实践启示**：
  1. 对防御方而言，仅靠"标签一致性检查"或"触发器逆向"不足以覆盖此类威胁，需要发展面向**训练动态/梯度分布异常**的检测手段；
  2. 数据供应链中"少量目标类样本被篡改"这类低幅度、低比例的污染，应被视为高优先级监控对象；
  3. 迁移学习 + 公开预训练骨干的组合放大了风险面，因为攻击者可用架构集成绕过架构未知性。
- **可拓展方向**：向视频/多模态与更强的物理变换（视角、遮挡、光照）扩展；探索在完全黑盒（无架构信息、无训练数据）条件下的可行性；以及针对该类攻击的认证式防御或数据清洗方法。

（完）
