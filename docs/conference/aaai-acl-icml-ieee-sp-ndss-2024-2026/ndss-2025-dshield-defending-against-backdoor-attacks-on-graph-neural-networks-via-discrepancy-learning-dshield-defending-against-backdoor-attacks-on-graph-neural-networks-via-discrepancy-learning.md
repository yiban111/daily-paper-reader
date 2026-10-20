---
title: "DShield: Defending against Backdoor Attacks on Graph Neural Networks via Discrepancy Learning"
title_zh: DShield：基于差异学习的图神经网络后门攻击防御
authors: "Hao Yu, Chuan Ma, Xinhang Wan, Jun Wang, Tao Xiang, Meng Shen, Xinwang Liu"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ndss-symposium.org/wp-content/uploads/2025-798-paper.pdf"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 针对图神经网络脏标签与干净标签后门攻击的防御
tldr: 图神经网络易受后门攻击，脏标签与干净标签攻击并存且现有防御难以兼顾。作者提出DShield综合防御框架，借助差异学习识别并缓解受污染节点带来的后门影响。实验表明该方法能同时抵御两类后门攻击，克服了依赖其他领域强假设的局限。该工作提升了图结构数据场景下的后门防御能力。
source: NDSS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-dshield-defending-against-backdoor-attacks-on-graph-neural-networks-via-discrepancy-learning/fig-001.webp\", \"caption\": \"\", \"page\": 7, \"index\": 1, \"width\": 477, \"height\": 308}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-dshield-defending-against-backdoor-attacks-on-graph-neural-networks-via-discrepancy-learning/fig-002.webp\", \"caption\": \"\", \"page\": 7, \"index\": 2, \"width\": 451, \"height\": 286}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-dshield-defending-against-backdoor-attacks-on-graph-neural-networks-via-discrepancy-learning/fig-003.webp\", \"caption\": \"\", \"page\": 7, \"index\": 3, \"width\": 477, \"height\": 308}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-dshield-defending-against-backdoor-attacks-on-graph-neural-networks-via-discrepancy-learning/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 477, \"height\": 308}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-dshield-defending-against-backdoor-attacks-on-graph-neural-networks-via-discrepancy-learning/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 477, \"height\": 308}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-dshield-defending-against-backdoor-attacks-on-graph-neural-networks-via-discrepancy-learning/fig-006.webp\", \"caption\": \"\", \"page\": 7, \"index\": 6, \"width\": 477, \"height\": 308}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-dshield-defending-against-backdoor-attacks-on-graph-neural-networks-via-discrepancy-learning/fig-007.webp\", \"caption\": \"\", \"page\": 7, \"index\": 7, \"width\": 477, \"height\": 308}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-dshield-defending-against-backdoor-attacks-on-graph-neural-networks-via-discrepancy-learning/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 477, \"height\": 308}]"
motivation: 图神经网络面临脏标签与干净标签后门攻击，现有防御难以同时应对。
method: 提出DShield综合防御框架，利用差异学习识别受污染节点并缓解后门影响。
result: 实验表明该框架能同时有效抵御脏标签与干净标签后门攻击。
conclusion: 为图结构数据场景提供了更稳健的后门防御方案。
---

## Abstract
Graph Neural Networks (GNNs) are vulnerable to backdoor attacks, where triggers inserted into original graphs cause adversary-determined predictions. Backdoor attacks on GNNs, typically focusing on node classification tasks, are categorized by dirty- and clean-label attacks and pose challenges due to the interconnected nature of normal and poisoned nodes. Current defenses are indeed circumvented by sophisticated triggers and often rely on strong assumptions borrowed from other domains (e.g., rapid loss drops on poisoned images). They lead to high attack risks, failing to effectively protect against both dirty- and clean-label attacks simultaneously. To tackle these challenges, we propose DShield, a comprehensive defense framework with a discrepancy learning mechanism to defend against various graph backdoor attacks. Specifically, we reveal two vital facts during the attacking process: *semantic drift* where dirty-label attacks modify the semantic information of poisoned nodes, and *attribute over-emphasis* where clean-label attacks exaggerate specific attributes to enforce adversary-determined predictions. Motivated by those, DShield employs a self-supervised learning framework to construct a model without relying on manipulated label information. Subsequently, it utilizes both the self-supervised and backdoored models to analyze discrepancies in semantic information and attribute importance, effectively filtering out poisoned nodes. Finally, DShield trains normal models using the preserved nodes, thereby minimizing the impact of poisoned nodes. Compared with 6 state-of-the-art defenses under 21 backdoor attacks, we conduct evaluations on 7 datasets with 2 victim models to demonstrate that DShield effectively mitigates backdoor threats with minimal degradation in performance on normal nodes. For instance, on the Cora dataset, DShield reduces the attack success rate to 1.33% from 54.47% achieved by the second-best defense Prune while maintaining an 82.15% performance on normal nodes. The source code is available at https://github.com/csyuhao/DShield.

---

## 论文详细总结（自动生成）

## 论文结构化总结：DShield

### 1. 核心问题与整体含义

- **研究背景**：图神经网络（GNN）通过消息传递聚合邻居信息，广泛应用于节点分类、图分类等任务，但易受后门攻击。攻击者在图中注入触发器，使模型在正常图上表现正常、在含触发器节点/图上输出攻击者指定标签。
- **核心问题**：节点分类后门攻击尤其棘手，因为正常节点与中毒节点通过边相互连接。攻击分为：
  - **脏标签攻击（DLBA）**：中毒节点真实标签与目标标签不同，攻击者改标签并注入触发器；
  - **干净标签攻击（CLBA）**：中毒节点标签保持真实标签，攻击者主要篡改属性作为触发器。
- **现有防御不足**：预处理型、检测型、毒化抑制型防御常依赖其他领域假设（如图像上的快速损失下降）、依赖触发器规格，或无法同时抵御 DLBA 与 CLBA；部分方法还会误删正常节点/边，甚至放大攻击效果。
- **论文整体含义**：作者从攻击过程中观察到两个内在事实——DLBA 导致**语义漂移**，CLBA 导致**属性过度强调**——据此提出 DShield，用差异学习识别并过滤中毒节点，训练无后门模型，力图同时覆盖脏标签、干净标签及图分类攻击。

### 2. 方法论

#### 2.1 核心思想

- 训练三类模型：
  - **自监督模型 \(g\)**：不依赖可能被操纵的标签，学习节点语义表示；
  - **后门模型 \(\tilde f\)**：在 manipulated graph 上用半监督学习训练，模拟受攻击模型；
  - **正常模型 \(f\)**：最终部署的无后门模型。
- 利用自监督模型与后门模型之间的**语义差异**，以及后门模型中的**属性重要度差异**，构造差异矩阵，聚类识别候选中毒节点；再发现目标标签，剔除中毒节点并训练正常模型。

#### 2.2 关键技术细节

- **辅助模型训练模块**
  - 后门模型 \(\tilde f\)：在中毒图上按标准半监督节点分类损失训练。
  - 自监督模型 \(g\)：采用图对比学习框架，包含视图增广、视图编码、对比与重构。
    - 边删除概率基于后门模型表示相似度，优先破坏不重要边；
    - 属性掩码基于梯度贡献，保留正贡献属性、扰动不重要属性；
    - 对比损失 \(L_c\) 拉近同一节点不同视图表示、推远其他节点；
    - 重构损失 \(L_r\) 用掩码表示重构属性，缓解假负样本问题；
    - 总损失 \(L=L_c+L_r\)。

- **差异矩阵构建模块**
  - **语义差异矩阵**：对每个标签 \(y\)，分别用后门模型第一层表示和自监督编码器表示计算节点间欧氏距离 \(D_{sl}^y\)、\(D_{ssl}^y\)，取
    \[
    D_s^y=\max(D_{ssl}^y-D_{sl}^y,0)
    \]
    以捕捉中毒节点在自监督空间中远离目标类正常节点的现象。
  - **属性重要度差异矩阵**：用梯度得到对预测正贡献的属性掩码 \(W\)，得到重要属性表示 \(\tilde X^y=\tilde X^y\circ W^y\)；用 UMAP 降维后计算距离 \(D_a^y\)，以放大正常与中毒节点在重要属性上的差异。
  - **矩阵融合与聚类**：按两个矩阵标准差加权融合为 \(D^y\)；对每个标签使用 HDBSCAN 聚类，假设同标签下最多 50% 节点可被污染，将多数簇视为正常 \(V_1^y\)，其余为候选中毒 \(V_2^y\)。

- **无后门模型训练模块**
  - **目标标签发现**：对每个标签 \(y\)，构造不含候选中毒节点间边的正子图 \(\tilde A^{y,pos}\) 和至少一端为候选节点的负子图 \(\tilde A^{y,neg}\)；训练辅助模型 \(f'\)，损失为
    \[
    L=L_{pos}-\log(L_{neg})
    \]
    即最大化正常节点性能、最小化候选中毒节点性能；计算候选节点到正常节点表示中心的距离作为可疑分数 \(s_y\)，用中位数绝对偏差 MAD 和阈值 \(\beta\) 识别异常目标标签。
  - **最终训练**：保留所有正常簇节点及非目标标签下的候选节点作为正常集 \(V^\dagger\)，目标标签下的候选节点作为中毒集 \(V^\ddagger\)；分类损失 \(L_{clf}\) 在 \(V^\dagger\) 上训练，惩罚损失 \(L_{penalty}\) 在中毒子图上降低其性能，总损失
    \[
    L=L_{clf}-\gamma\log L_{penalty}
    \]
    以降低触发器可迁移性。

### 3. 实验设计

- **数据集与场景**
  - 节点分类：Cora、PubMed、Flickr、OGBN-arXiv，覆盖引文网络、图像描述图和大规模引文网络。
  - 图分类：ENZYMES、PROTEINS、MNIST。
  - 采用 inductive 半监督节点分类设置；训练图/测试图分离，随机掩蔽 20% 节点，其中一半作为中毒节点、一半作为干净测试节点；训练图占 80%，标签与验证各约 10%。
- **攻击与 Benchmark**
  - 节点分类：10 种 DLBA（SBA、GTA、EBA、GB-FGSM、LGCB、UGBA、TRAP、TLN、DPGBA、MLGB）和 2 种 CLBA（GCBA、PerCBA）。
  - 自适应攻击：UGBA+LGCB、UGBA+GCBA、GCBA+PerCBA、AdaDA、AdaCA。
  - 图分类：G-SBA、G-EBA、G-GCBA、SBAG。
  - 论文称在 21 种后门攻击下评估，与上述 12 种节点分类攻击、5 种自适应攻击、4 种图分类攻击大致对应。
- **对比方法**
  - 6 种 SOTA 防御：Prune、Isolate、XGBD-PGExplainer（PXGBD）、XGBD-GNNExplainer（GXGBD）、ABL、RandomSmoothing（RS）。
- **受害者模型与指标**
  - GCN 与 GAT；默认 GCN。
  - 指标：攻击成功率 ASR↓、正常节点准确率 ACC↑；实验重复 3 次，报告均值与标准差。

### 4. 资源与算力

- 论文**未明确说明**完整实验所用 GPU 型号、数量、总训练时长或总能耗。
- 文中提供了防御执行时间表：例如 Cora 上 DShield 对 UGBA/GCBA 约 64 秒，PubMed 约 179/193 秒，Flickr 约 1068/1052 秒，OGBN-arXiv 约 1205/1230 秒；使用 cuML 加速后的 DShield-cuML 分别降至约 18/20 秒、46/44 秒、152/150 秒、181/179 秒。
- 附录 artifact 给出缩减版复现实验的计算时长大致为：E1 约 1.5 compute-hours、E2 约 3 compute-hours、E3 约 1 compute-hour、E4 约 2 compute-hours；但这是 artifact 评估的缩减设置，并非完整论文实验总算力，且未说明硬件型号。

### 5. 实验数量与充分性

- **实验规模较大**：
  - 节点分类覆盖 4 个数据集、2 种受害者模型、10 种 DLBA、2 种 CLBA；
  - 包含不同中毒率（5%–30%）、不同触发器大小/属性扰动数量、5 种自适应攻击；
  - 包含 \(\beta\)、\(\gamma\) 敏感性分析；
  - 包含消融实验：去掉重构损失、去掉自适应丢弃、去掉 UMAP；
  - 包含图分类迁移实验与执行时间对比。
- **充分性**：整体较充分，覆盖了攻击类型、模型架构、数据规模、鲁棒
