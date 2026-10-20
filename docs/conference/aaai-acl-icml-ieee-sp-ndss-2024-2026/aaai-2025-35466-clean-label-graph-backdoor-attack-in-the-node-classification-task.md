---
title: Clean-Label Graph Backdoor Attack in the Node Classification Task
title_zh: 节点分类任务中的干净标签图后门攻击
authors: "Hui Xia, Xiangwei Zhao, Rui Zhang, Shuo Xu, Luming Wang"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/35466/37621"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 面向节点分类的图神经网络干净标签后门攻击
tldr: 图神经网络面临后门威胁，但现有脏标签攻击需注入明显错误输入，易被简单过滤检测。作者提出干净标签图后门攻击CGBA，使生成的有毒节点特征多数与其真实标签一致，并利用GNN固有的不确定性以低预算策略选择目标类节点。该方法显著提升了攻击的隐蔽性与检测难度，揭示了图学习场景下干净标签后门的新风险。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 2284, \"height\": 781}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-003.webp\", \"caption\": \"\", \"page\": 6, \"index\": 3, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-006.webp\", \"caption\": \"\", \"page\": 7, \"index\": 6, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-007.webp\", \"caption\": \"\", \"page\": 7, \"index\": 7, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-010.webp\", \"caption\": \"\", \"page\": 7, \"index\": 10, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-011.webp\", \"caption\": \"\", \"page\": 7, \"index\": 11, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-012.webp\", \"caption\": \"\", \"page\": 7, \"index\": 12, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-013.webp\", \"caption\": \"\", \"page\": 8, \"index\": 13, \"width\": 3216, \"height\": 2462}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35466/fig-014.webp\", \"caption\": \"\", \"page\": 8, \"index\": 14, \"width\": 3216, \"height\": 2462}]"
motivation: 现有图后门多为脏标签攻击，需注入明显错误输入，易被过滤检测，隐蔽性不足。
method: 提出CGBA，使有毒节点特征与其真实标签一致，并利用GNN不确定性低预算选择投毒节点。
result: 实验表明CGBA在保持干净标签一致性的同时取得较高攻击成功率并更难被检测。
conclusion: 该工作揭示了图神经网络在干净标签设定下同样面临严重后门威胁。
---

## Abstract
Graph neural networks (GNNs) have achieved impressive results in various graph learning tasks. Backdoor attacks pose a significant threat to GNNs, with a focus on dirty-label attacks. However, these attacks often necessitate the inclusion of blatantly incorrect inputs into the training set, rendering them easily detectable through simple filtering. In response to this challenge, we introduce Clean-Label Graph Backdoor Attack (CGBA). The majority of features in the generated poisoned nodes align with their true labels, significantly enhancing the difficulty of detecting the attack. Firstly, leveraging the uncertainty inherent in the GNNs, we develop a low-budget strategy for selecting poisoned nodes. This approach focuses on nodes in the target class with uncertain and low-degree classifications, allowing for efficient attacks within a limited budget while mitigating the impact on other clean nodes. Secondly, we present an innovative strategy for generating feature triggers. By boosting the confidence of poisoned samples in the target class, this tactic establishes a robust association between the trigger and the target class, even without modifying the labels of poisoned nodes. Additionally, we incorporate two constraints to reduce disruption to the graph structure. In conclusion, comprehensive experimental results unequivocally showcase CGBA's exceptional attack performance across three benchmark datasets and four GNNs models. Notably, the attack targeting the GraphSAGE model attains a 100% success rate, accompanied by a marginal benign accuracy drop of no more than 0.5%.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：图神经网络（GNN）在节点分类等图学习任务中表现突出，但同样面临后门攻击威胁。现有图后门攻击多为**脏标签攻击（dirty-label）**：攻击者把带触发器的样本强行标为目标类，导致训练集中出现明显错误标注，容易被简单过滤或异常检测发现。
- **核心问题**：能否在**不修改投毒节点标签**的前提下，对 GNN 节点分类任务实施有效且隐蔽的后门攻击？即实现面向节点分类的**干净标签图后门攻击（Clean-Label Graph Backdoor Attack, CGBA）**。
- **整体含义**：论文指出，干净标签后门在图像领域已有研究，但在 GNN 节点级分类任务中尚属空白。CGBA 试图证明：即使投毒节点保留真实标签，攻击者仍可通过精心选择节点、特征位置和生成特征触发器，在低预算下建立“触发器—目标类”的强关联，同时保持干净节点分类精度基本不变。这揭示了 GNN 在干净标签设定下同样存在严重后门风险。

## 2. 方法论

### 2.1 核心思想

- 攻击发生在**训练阶段和推理阶段**：
  - 训练阶段：攻击者可访问训练图数据，但不知道目标 GNN 的架构与参数；先用干净数据训练一个代理模型，再在目标类节点中选择少量投毒节点，修改其少量特征并附加触发器，**不改变标签**。
  - 推理阶段：对测试节点附加同一触发器，使后门模型将其预测为目标类；对干净节点则尽量保持原预测。
- 攻击目标可概括为：带触发器的 victim 节点被误分类为目标类 \(y_t\)，而无触发器干净节点的输出尽量接近干净模型。

### 2.2 投毒节点选择

- 使用一个**两层 GCN 代理模型**在干净训练集上训练，获得节点在目标类上的置信度。
- 直觉：目标类中置信度低、分类不确定的节点更适合投毒，因为触发器更容易“解释”其为何属于目标类；高度数节点被修改会影响更多邻居，因此倾向选择低度节点。
- 对每个目标类节点计算评分：
  - \(\text{Score}(v_i) = -(\text{confidence}(v_i,y_t) + \beta \cdot \deg(v_i))\)
  - 即置信度越低、度数越低，得分越高。
- 选择得分最高的前 \(n\) 个目标类节点构成投毒节点集合 \(V_p\)。

### 2.3 投毒特征位置选择

- 借鉴图解释方法 **GraphLIME**，计算投毒节点各特征的重要性。
- 对所有投毒节点的特征重要性取平均，得到平均重要性因子 AIF：
  - \(\text{AIF} = \frac{1}{|V_p|}\sum_{x_i\in V_p}\text{GraphLIME}(x_i)\)
- 选择 AIF 中最大的前 \(m\) 个位置作为特征攻击位置。

### 2.4 特征触发器生成

- 使用固定触发器 \(g\)，由**两层 MLP** 迭代生成，初始特征随机；每轮将投毒节点在选定位置上的特征替换为 \(g\)，再优化 MLP。
- 触发器损失由三部分组成：
  1. **攻击损失 \(L_c\)**：提高投毒节点在目标类 \(y_t\) 上的置信度，同时尽量保持非投毒节点在其真实标签上的置信度。
  2. **隐蔽性约束 \(L_s\)**：约束投毒节点附加触发器前后与原特征的相似度，避免特征突变。
  3. **邻居相似性约束 \(L_n\)**：约束投毒节点与其邻居节点的特征相似度，避免因与邻居差异过大而被视为异常节点。
- 总损失：
  - \(L_{\text{all}} = L_c + \lambda_1 L_s + \lambda_2 L_n\)
- 对离散特征数据集（如 Citeseer），生成值按 0.5 阈值截断为 0/1。
- 算法流程：训练代理模型 → 选投毒节点 → GraphLIME 选特征位置 → 用 MLP 迭代优化触发器 → 得到最终触发器 \(g\)。

## 3. 实验设计

- **数据集**：
  - Cora：2708 节点、5429 边、7 类，目标类为第 3 类。
  - Citeseer：3327 节点、4608 边、6 类，目标类为第 3 类。
  - Pubmed：19717 节点、44338 边、3 类，目标类为第 1 类。
  - 数据划分：训练 70%、验证 10%、干净测试 10%、带触发器测试 10%。
- **目标模型**：GCN、GAT、GraphSAGE、GAE 四类 GNN。
- **评价指标**：
  - **BA（Benign Accuracy）**：干净测试集上的分类精度。
  - **ASR（Attack Success Rate）**：带触发器 victim 节点被成功预测为目标类的比例。
- **对比方法**：
  - 特征触发器攻击：MIA、NFTA、CBA（从图分类任务迁移到节点分类）。
  - 子图触发器攻击：GTA、EGNN。
  - 节点注入攻击：UGBA。
- **主要参数**：Cora/Citeseer 投毒比例 2%，Pubmed 1%；特征修改比例 5%；\(\beta=0.01\)；代理模型为两层 GCN；触发器生成器为两层 MLP。
- **附加实验**：
  - 投毒节点数量影响：Pubmed 上测试 30、60、90、120、150 个投毒节点。
  - 投毒特征数量影响：Cora 与 Pubmed 上改变修改特征数。
  - 消融实验：节点选择策略对比随机选择；特征选择策略对比“最重要特征”与“随机特征”。
  - 防御实验：Prune 与 Isolate 两种基于邻居相似度的防御。
  - 参数敏感性：分析 \(K,\lambda_1,T,\lambda_2\) 的影响。

## 4. 资源与算力

- 论文**未明确说明**使用的 GPU 型号、GPU 数量、训练时长、显存消耗或总算力。
- 文中仅提及代理模型为两层 GCN、触发器生成器为两层 MLP，以及训练轮次等算法参数，但不足以推算实际算力开销。
- 因此，无法从论文文本中客观评估其计算资源规模与训练成本。

## 5. 实验数量与充分性

- **实验规模较大**：
  - 主实验覆盖 3 个数据集 × 4 个 GNN 模型，并与 MIA、NFTA、CBA、GTA、EGNN、UGBA 等多类攻击比较。
  - 包含 ASR/BA 主表、与子图/注入攻击的补充对比、两种防御下的鲁棒性测试。
  - 还包含投毒节点数量、投毒特征数量、节点选择消融、特征选择消融和两组参数敏感性分析。
- **充分性**：
  - 从数据集、模型、攻击类型、防御和消融角度看，实验覆盖较全面，能够支撑“CGBA 攻击成功率高且隐蔽性较好”的结论。
  - 但论文未报告多次随机划分或重复实验的均值/方差，缺少置信区间和显著性检验，统计稳健性有限。
  - 每个数据集仅固定一个目标类，可能带来目标类选择偏差。
  - 部分 baseline 是为适配节点分类而迁移或调整的，例如 CBA 从图分类迁移、NFTA 在连续数据上采用随机特征生成，公平性需谨慎解读。
  - GTA、EGNN、UGBA 与 CGBA 的攻击预算单位不同（子图数、注入节点数与投毒节点数），虽作者声称保持同等规模，但严格可比性仍有限。

## 6. 主要结论与发现

- 干净标签后门攻击在 GNN 节点分类任务中**可行且有效**，无需修改投毒节点标签即可建立触发器与目标类的关联。
- CGBA 在三个数据集、四个 GNN 模型上取得很高 ASR，多数场景超过 96%；GraphSAGE 上多个数据集达到 100% ASR，同时 BA 下降很小，部分场景甚至略高于干净模型。
- 选择目标类中**低置信度、低度数**节点作为投毒节点，比随机选择更有效。
- 使用 GraphLIME 选择**最重要特征位置**优于随机特征位置，尤其在攻击特征数较少时优势明显。
- 在基于邻居相似度的 Prune/Isolate 防御下，CGBA 的 ASR 几乎不下降，甚至在部分数据集上有所上升；而其他攻击方法普遍下降。
- 投毒节点数和投毒特征数增加会提升 ASR，但 CGBA 在较低预算下仍能保持较强攻击效果。

## 7. 优点

- **问题新颖**：首次系统提出面向 GNN 节点分类任务的干净标签后门攻击，填补了节点级干净标签图后门的研究空白。
- **隐蔽性设计较好**：不修改标签，且通过原特征相似度和邻居相似度约束，使投毒节点不易被简单异常检测或相似度剪枝发现。
- **低预算攻击策略**：利用代理模型不确定性和度数约束选择节点，减少对干净节点的影响。
- **触发器生成有针对性**：通过增强目标类置信度并保持非投毒节点预测，建立触发器与目标类的稳定关联；对离散数据设计了阈值截断。
- **黑盒设定较现实**：攻击者只需训练数据，不需要目标模型参数。
- **实验较丰富**：覆盖多数据集、多 GNN、多类 baseline、防御测试和消融分析，结论支撑较充分。

## 8. 不足与局限

- **威胁模型仍较强**：攻击者需要能够修改训练图节点特征，并训练代理模型；实际部署中训练数据访问和修改权限未必成立。
- **依赖代理模型与 GraphLIME**：节点选择和特征位置选择依赖代理模型质量及解释方法，跨架构迁移时可能受影响。
- **实验统计性不足**：未报告重复实验的均值、方差、显著性，也未说明随机种子与划分稳定性。
- **目标类与数据集覆盖有限**：每个数据集只选一个目标类，数据集规模偏小，未覆盖大规模图、异质图、动态图或更多 GNN 架构。
- **防御评估有限**：仅测试两种基于邻居相似度的防御，未评估更先进的后门检测、鲁棒训练、认证防御、异常检测等。
- **baseline 公平性需谨慎**：部分对比方法经过迁移或适配，攻击预算单位不完全一致，可能影响结论的严格可比性。
- **超参数较多且敏感**：\(K,T,\lambda_1,\lambda_2\) 等需要调节，论文虽给出敏感性分析，但实际应用中的调参成本和先验依赖仍是限制。
- **未提供算力与复现细节**：缺少 GPU 资源、训练时长、代码/数据划分细节，复现性和成本评估受限。

（完）
