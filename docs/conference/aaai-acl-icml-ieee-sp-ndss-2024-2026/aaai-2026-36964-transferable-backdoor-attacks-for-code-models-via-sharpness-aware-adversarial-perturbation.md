---
title: Transferable Backdoor Attacks for Code Models via Sharpness-Aware Adversarial Perturbation
title_zh: 基于锐度感知对抗扰动的代码模型可迁移后门攻击
authors: "Shuyu Chang, Haiping Huang, Yanjun Zhang, Yujin Huang, Fu Xiao, Leo Yu Zhang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/36964/40926"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 针对代码模型的可迁移隐蔽后门攻击
tldr: 代码模型易受投毒训练数据的后门攻击，现有方法在可迁移性与隐蔽性之间难以兼顾：静态触发器迁移性好却易被检测，动态触发器隐蔽但跨数据集迁移差。作者提出基于锐度感知对抗扰动的后门攻击方法，生成兼具隐蔽性与迁移性的触发器，并放宽了中毒与受害数据同分布的假设。实验表明该方法在多种代码模型和数据集上均能实现有效且隐蔽的后门迁移，提升了代码模型后门攻击的现实可行性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 5880, \"height\": 5880}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 5880, \"height\": 5880}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-003.webp\", \"caption\": \"\", \"page\": 7, \"index\": 3, \"width\": 354, \"height\": 354}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 354, \"height\": 354}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 354, \"height\": 354}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-006.webp\", \"caption\": \"\", \"page\": 7, \"index\": 6, \"width\": 354, \"height\": 354}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-007.webp\", \"caption\": \"\", \"page\": 7, \"index\": 7, \"width\": 354, \"height\": 354}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-008.webp\", \"caption\": \"\", \"page\": 7, \"index\": 8, \"width\": 354, \"height\": 354}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36964/fig-009.webp\", \"caption\": \"\", \"page\": 7, \"index\": 9, \"width\": 354, \"height\": 354}]"
motivation: 代码模型后门攻击在可迁移性与隐蔽性之间难以兼顾，且常依赖中毒与受害数据同分布的不现实假设。
method: 提出基于锐度感知对抗扰动的后门攻击，生成兼顾隐蔽性与跨模型跨数据集迁移性的动态触发器。
result: 实验表明该方法在多种代码模型和数据集上实现有效且隐蔽的后门迁移，提升了攻击的现实可行性。
conclusion: 为代码模型场景下兼具迁移性与隐蔽性的后门攻击提供了新方法。
---

## Abstract
Code models are increasingly adopted in software development but remain vulnerable to backdoor attacks via poisoned training data. Existing backdoor attacks on code models face a fundamental trade-off between transferability and stealthiness. Static trigger-based attacks insert fixed dead code patterns that transfer well across models and datasets but are easily detected by code-specific defenses. In contrast, dynamic trigger-based attacks adaptively generate context-aware triggers to evade detection but suffer from poor cross-dataset transferability. Moreover, they rely on unrealistic assumptions of identical data distributions between poisoned and victim training data, limiting their practicality. To overcome these limitations, we propose Sharpness-aware Transferable Adversarial Backdoor (STAB), a novel attack that achieves both transferability and stealthiness without requiring complete victim data. STAB is motivated by the observation that adversarial perturbations in flat regions of the loss landscape transfer more effectively across datasets than those in sharp minima. To this end, we train a surrogate model using Sharpness-Aware Minimization to guide model parameters toward flat loss regions, and employ Gumbel-Softmax optimization to enable differentiable search over discrete trigger tokens for generating context-aware adversarial triggers. Experiments across three datasets and two code models show that STAB outperforms prior attacks in terms of transferability and stealthiness. It achieves a 73.2% average attack success rate after defense, outperforming static trigger–based attacks that fail under defense. STAB also surpasses the best dynamic trigger–based attack by 12.4% in cross-dataset attack success rate and maintains performance on clean inputs.

---

## 论文详细总结（自动生成）

# 论文结构化总结：STAB——基于锐度感知对抗扰动的代码模型可迁移后门攻击

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：预训练代码模型（代码生成、摘要、缺陷检测等）已深度嵌入软件供应链，其训练语料大量来自公开代码仓库，因而天然暴露于“数据投毒”式后门攻击风险：模型在含触发器输入上输出攻击者指定目标，在干净输入上保持正常行为。
- **代码域的独特约束**：源码必须保持严格语法有效性与功能正确性，单个错位 token 即可能导致编译失败或语义改变；同时触发器需具备隐蔽性以躲过代码专用检测。
- **现有方法的两难（核心问题）**：
  - **静态触发器**（如插入 `if(sin(0.7)<-1)` 之类的死代码、基于文法生成死代码）：跨模型、跨数据集**迁移性好**，但模式固定、可预测，极易被 KillBadCode 等基于代码自然度的防御检出。
  - **动态触发器**（代表工作 AFRAIDOOR，通过标识符重命名做对抗扰动）：**隐蔽性好**，但存在两个关键缺陷：① 采用逐标识符的贪心搜索，易陷入次优局部极小；② 假设中毒数据与受害者训练数据**同分布**，而在现实威胁模型下（攻击者污染公开仓库、受害者从多源采集私有数据），分布不一致导致跨数据集迁移性差。
- **整体含义**：论文主张现有动态攻击所找到的扰动是“尖锐极小值（sharp minima）中的数据集特异性脆弱性”，因而在分布漂移下失效；若能引导代理模型收敛到**平坦损失区域（flat minima）**，则可学到跨数据集通用的代码对抗模式，从而同时获得迁移性与隐蔽性，且**无需受害者完整训练数据**（黑盒、仅用公开数据训练代理模型）。

## 2. 方法论：核心思想与关键技术细节

### 2.1 核心思想
- 平坦极小值对应更可泛化的特征（通用语义/语法模式），尖锐极小值对应数据集特异伪影；因此用 **Sharpness-Aware Minimization（SAM）** 训练代理模型，再在其上做**可微的联合触发器优化**，生成上下文感知、语法合法、可迁移的触发器。
- 攻击形态为**标识符重命名**（如 `path→disk`、`filename→cache`、`content→data`），替换代码中原有标识符，不改变控制流与功能语义。

### 2.2 三阶段流程

- **阶段一：锐度感知代理模型训练（Sharpness-Aware Surrogate Model Training）**
  - 在公开代码构建的代理数据集 \(D_s\) 上训练代理模型 \(M_s^*\)，目标为 SAM 的 min-max 形式：
    \(\min_{\theta_s} \max_{\|\delta\|_2\le\rho} L(\theta_s+\delta, D_s)\)。
  - 交替两步：先求最坏扰动 \(\delta^* = \rho\cdot \nabla_{\theta_s}L(\theta_s,D_s)/\|\nabla_{\theta_s}L(\theta_s,D_s)\|_2\)；再在扰动参数处求梯度更新 \(\theta_s \leftarrow \theta_s - \eta\nabla_{\theta_s}L(\theta_s+\delta^*,D_s)\)。
  - 效果：使参数落入对权重扰动鲁棒的区域，捕获与分布无关的代码模式，为后续生成可迁移触发器奠基。

- **阶段二：对抗触发器优化（Adversarial Trigger Optimization）**
  - **离散搜索的可微松弛**：对每条样本解析 AST 找出可修改标识符集合 \(\{v_j\}_{j=1}^k\)；初始化可学习代理分布矩阵 \(\Pi\in\mathbb{R}^{L\times|V^t|}\)，仅合法标识符对应位置可取非零值（保证语法合法）。用 Gumbel-Softmax 采样：
    \(\tilde z_i=\mathrm{softmax}((\log\pi_i+g_i)/\tau)\)，得到软 token 表示，并以加权嵌入 \(e=\tilde z^\top E\) 作为代理模型的可微输入。
  - **优化目标**：\(L_{\text{trigger}}=L_a+\lambda\cdot(L_c+L_d)\)，三项分别为：
    - **攻击损失 \(L_a\)**：使触发代码输出目标 \(y^*\) 的交叉熵 \(-\log P(y^*|M_s^*(e))\)。
    - **一致性损失 \(L_c\)**：同一标识符的所有出现位置（集合 \(P_j\)）应映射到同一触发词，用分布层面的 **MMD** 约束 \(\sum_j\sum_{l,l'\in P_j}\mathrm{MMD}(\pi_l,\pi_{l'})\) 实现。
    - **多样性损失 \(L_d\)**：不同标识符应映射到不同触发词（代码语法要求），以最大化不同标识符平均分布间的 MMD 实现，即 \(L_d=-\sum_{i<j}\mathrm{MMD}(\bar\pi_i,\bar\pi_j)\)；同时避免重复模式、提升隐蔽性。
  - 梯度经 Gumbel-Softmax 松弛与 SAM 代理模型反传，对**所有触发位置联合端到端优化** \(N\) 次迭代，得到 \(\Pi^*\)。

- **阶段三：触发器生成与投放（Trigger Generation and Deployment）**
  - 以极低温度 Gumbel-Softmax 从各标识符平均分布中采样离散 token；若不同标识符采到同一 token 则**重采样**以保证代码合法。
  - 将标识符的全部出现替换为对应触发词，生成中毒样本 \(D_p=\{(x_i\oplus t_i,y^*)\}\)，通过开源社区机制（star、fork）提升仓库曝光度与进入受害者训练集的概率；受害者在 \(D=D_v\cup D'_p\) 上训练后即被植入后门（默认投毒率 \(\epsilon=5\%\)）。

### 2.3 威胁模型设定
- 攻击者无法访问受害者数据 \(D_v\) 与模型参数，仅有公开代码构建的代理数据集 \(D_s\)（训练代理）与 \(D'_s\)（生成触发器），需在 \(D_s\ne D_v\) 且可能存在架构差异的条件下保持高攻击成功率。

## 3. 实验设计

- **数据集（3 个 Python 代码数据集，分布各异）**：
  - Py150（GitHub 2016 年前 15 万 Python 文件）、CodeSearchNet/CSN（40 万+ Python 函数）、PyTorch/PyT（PyPI 与 Anaconda 爬取的 21.8 万包库代码）。
  - 任务：**Method Name Prediction（MNP）** 与 **Code Summarization（CS）** 两类生成任务（比分类/检索更难）。
  - 三个数据集两两组合形成 **9 组 surrogate→victim 跨数据集组合**。
- **受害模型**：PLBART-base 与 CodeT5-small（HuggingFace 公开预训练代码模型）。
- **对比基线**：
  - 静态：**Fixed triggers**（固定死代码插入）、**Grammar-based triggers**（概率上下文无关文法生成死代码）。
  - 动态：**AFRAIDOOR**（当前 SOTA 动态代码后门攻击，假设同分布）。
- **防御方法**：**Spectral Signature（SS）**（多右奇异向量的表示异常检测）、**ONION**（困惑度变化识别可疑 token）、**KillBadCode**（基于 n-gram 代码自然度、删除后流畅度提升的触发器检测）。
- **评价指标**：ASR（攻击成功率）、ASR-D（经 KillBadCode 防御后的成功率）、BLEU-4（干净数据上的模型性能，衡量功能保持）、Recall 与 F1（防御检出率，越低越隐蔽）。
- **实现细节**：代理模型为 2 层 Encoder-Decoder Transformer；MNP 目标输出 “load data”，CS 目标输出 “Load train data from the disk safely”；\(\rho=0.02\)、\(\tau=1.0\)、\(N=100\) 迭代、\(\lambda=0.1\)、受害者微调 15 epoch 并早停。

## 4. 资源与算力

- **论文未明确报告任何算力信息**：没有给出 GPU 型号、数量、显存、总训练时长或能耗估算。
- 可间接推断的规模信息仅为：代理模型为 2 层 Transformer；受害模型为 PLBART-base 与 CodeT5-small 等中小规模模型；每个数据集训练样本 5 万–20 万条、微调 15 epoch；共 9 组组合 × 2 模型 × 2 任务，实验总量不小，但**具体硬件开销无法从文中获知**，这在一定程度上削弱了可复现性与成本评估的完整性（尽管作者提供了代码链接）。

## 5. 实验数量与充分性

- **实验组数概览**：
  - 主实验（RQ1 跨数据集迁移性）：9 组 surrogate–victim 组合 × 2 受害模型 × 2 任务，与 AFRAIDOOR 逐一对比（表 2），共约 36 组 ASR/BLEU 结果对比。
  - 防御实验（RQ2）：3 种防御 × 3 个受害数据集 × 4 种攻击（Fixed/Grammar/AFRAIDOOR/STAB）× 2 任务，并对全部 9 组组合给出 ASR-D 热力图（图 3）。
  - 消融实验（RQ3）：去掉 SAM、用贪心替换 Gumbel-Softmax 两组变体，在 3 个受害数据集 × 2 任务上给出 ASR 与 ASR-D（表 4，含标准差、多次运行）。
  - 超参敏感性：投毒率 \(\epsilon\)（约 2%–10%，3 个数据集）、锐度参数 \(\rho\)（0.01–0.04，3 个数据集），并同时报告 ASR 与 ASR-D。
- **充分性评价**：
  - **较充分**：覆盖 3 数据集 9 组合的跨分布设定、2 个受害模型、2 类生成任务、3 种防御、2 组消融、2 组超参扫描，并报告标准差（多次运行），指标同时兼顾有效性（ASR/ASR-D）与功能保持（BLEU）与隐蔽性（Recall/F1）。
  - **公平性**：与静态、动态基线在同一威胁模型、同一投毒率下比较；对静态攻击的“防御下失效”也如实报告（ASR-D 为 0），未刻意弱化对手；对 AFRAIDOOR 的对比是逐组合给出而非只报最优。
  - **待加强处**：任务仅限 MNP 与 CS，未覆盖代码补全、代码翻译、缺陷检测等；受害模型仅两个中小规模 encoder-decoder；防御仅三种（文中提到 EliBadCode 但未纳入实验）；缺少人工/编译器层面的隐蔽性评估与统计分析（显著性检验）。

## 6. 主要结论与发现

- **跨数据集迁移性**：STAB 平均 ASR 达 **80.1%**，比最佳动态攻击（AFRAIDOOR）高 **12.4%**；在全部 9 组 surrogate–victim 组合上均优于 AFRAIDOOR，且分布差异越大优势越明显（如 PLBART/MNP 下平均 ASR 76.89% vs 68.48%，CodeT5/MNP 下 76.93% vs 69.20%）。
- **防御鲁棒性**：经 KillBadCode 防御后平均 ASR-D 仍达 **73.2%**，而静态触发器攻击在所有场景下 ASR-D 降为 0（被完全瓦解）；AFRAIDOOR 的 ASR-D 波动较大，STAB 在各组合上稳定更高。
- **隐蔽性**：在 SS、ONION、KillBadCode 三种防御下，STAB 的检出 Recall/F1 **最低**（如 Py150/CodeT5 上 KillBadCode 的 Recall 约 23.02%、F1 约 9.77%，而 Fixed 与 Grammar 接近 100%），说明其触发器更贴近代码自然分布。
- **功能保持**：干净数据上的 BLEU 与基线相当（如 PLBART/MNP 下 52.28 vs 52.13），后门未明显损害正常性能。
- **消融结论**：去掉 SAM 后 ASR 下降且跨组合方差增大，说明 SAM 是获得稳定可迁移模式的关键；用贪心替换 Gumbel-Softmax 后初始 ASR 尚可但 **ASR-D 明显下降**，说明可微联合优化带来更强隐蔽性。
- **超参结论**：投毒率提高带来收益但边际递减；\(\rho=0.02\) 为最佳平衡点——过小平滑引导不足，过大则受害者训练中见过多变化触发模式而难以形成一致的后门关联。

## 7. 优点（方法与实验设计亮点）

- **问题定位准确**：明确指出现有动态攻击“同分布假设”不现实，并提出跨数据集（cross-dataset）威胁模型，贴合公开仓库投毒的实际情况，具有较强现实意义。
- **动机与手段匹配**：以“平坦极小值→更可泛化特征”的损失景观几何视角解释迁移性，并用 SAM 直接落实该直觉，思路清晰、可解释性较好（虽为经验性论证）。
- **优化层面改进实质**：把逐标识符贪心搜索改为 Gumbel-Softmax 下的**全局可微联合优化**，显式建模触发 token 间依赖，缓解局部最优问题。
- **语法与语义约束设计巧妙**：用 MMD 同时实现“同一标识符映射一致”（一致性）与“不同标识符映射互异”（多样性），把代码的硬语法要求转化为可微正则项，兼顾合法性与隐蔽性（多样性还顺带降低模式重复带来的可检测性）。
- **黑盒可行性**：仅需公开数据训练代理模型，不依赖受害者数据或模型，部署路径（star/fork 提升曝光）描述具体。
- **实验组织清晰**：RQ1/RQ2/RQ3 层层递进；同时报告 ASR 与 ASR-D、Recall/F1 与 BLEU，兼顾“攻得成”“藏得住”“不伤正常功能”三个维度；对静态基线在防御下归零的结果如实呈现，比较相对客观。

##

## 8. 缺点与局限（批判性评估）

- **理论论证偏经验性**：“平坦极小值 ⇒ 可迁移触发器”这一因果链条主要靠 SAM 的通用泛化直觉与消融结果支撑，缺少针对“触发器可迁移性”本身的定量理论刻画（如损失景观锐度与跨分布 ASR 的相关性曲线、Hessian 谱分析）。读者难以判断该机制在何种条件下会失效。
- **威胁模型仍有理想化成分**：
  - 代理数据与受害数据虽“不同分布”，但均取自 Python 代码这一同一语言与同一生态，跨语言（Python→Java/C++）、跨任务范式（生成→分类）的迁移完全未验证，“跨数据集”结论的外推范围有限。
  - 触发器生成依赖对受害者数据中标识符集合的可访问性假设（阶段二需解析目标样本 AST 并注入），实际投毒中攻击者只能对公开仓库中自己可控的文件注入，能否覆盖受害者采样的分布存疑，论文未讨论注入覆盖率与采样概率之间的关系。
  - 默认投毒率 5% 且假设受害者不加筛选地混合数据，未考虑数据去重、来源可信度加权、仓库信誉过滤等现实防线。
- **触发器语义可疑性未被评估**：标识符重命名（如 `content→data`、`filename→cache`）虽语法合法，但可能引入**语义不协调**（变量名与其实际用途不符）。论文仅用 n-gram 自然度类防御（KillBadCode）与困惑度（ONION）衡量隐蔽性，未做**人工审查实验**、未用静态分析/类型推断/变量命名一致性工具（如基于深度学习的变量误命名检测器）评估，而后者恰恰是代码审查中最可能发现此类攻击的手段。
- **多样性损失可能反噬隐蔽性**：强制不同标识符映射到不同触发词，虽满足语法要求并降低模式重复，但也可能产生“同一文件内命名风格突变”的异常信号，论文未分析该权衡（例如不同文件内触发词数量的分布是否偏离自然代码）。
- **一致性约束的实现细节不清**：MMD 作用于 Gumbel-Softmax 输出的软分布上，温度 \(\tau\) 退火策略、以及软分布收敛到硬 one-hot 的程度未充分说明；阶段三的“重采样”若频繁触发，可能破坏阶段二优化所得的联合结构，论文未报告重采样频率。
- **基线与防御覆盖不足**：
  - 动态基线仅 AFRAIDOOR 一个，缺少其他代码后门工作（如针对代码补全的 CODERITE、针对分类的 TrojanPuzzle 类方法）以及通用 NLP 后门（如嵌入空间触发的 LWP）作为参照。
  - 防御仅 SS、ONION、KillBadCode 三种，均属**推理期/数据期的事后检测**；缺少**训练期防御**（如 ONION 之外的 data filtering、微调阶段的 trigger inversion、模型剪枝/微调净化）与**认证式防御**。文中提及 EliBadCode 却未实验，是明显缺口。
  - 未评估**自适应攻击者**设定（即防御方知晓 STAB 机制后针对性检测，如检测“标识符被替换为高频通用词”的模式）。
- **指标与统计**：
  - ASR 仅报告点估计与标准差，未做显著性检验（如配对 t 检验或 Wilcoxon），12.4% 的平均提升是否在 9 组组合上统计显著无法确认。
  - BLEU-4 作为功能保持指标对生成任务偏粗，缺少 Exact Match、CodeBLEU 等更能反映代码正确性的指标；也未报告干净样本上的误触发率（false trigger rate），即正常代码中偶然出现触发词导致错误输出的比例——这对“隐蔽性”与“可用性”都关键。
- **实验规模与算力透明度**：如第 4 节所述，完全缺失算力报告；此外受害模型仅 PLBART-base 与 CodeT5-small，未覆盖更大模型（CodeT5+、StarCoder 类）或仅编码器模型（CodeBERT），后者的 MNP 设定下攻击面不同，结论的普适性受限。
- **伦理与披露**：论文提出可实际部署的供应链投毒路径（star/fork 提升曝光），虽属安全研究常规，但对缓解建议（如仓库信誉机制、标识符一致性检查）着墨很少，攻防天平偏向攻击侧。

## 9. 启发与可延伸方向

- **方法论迁移**：将“SAM 代理训练 + 可微离散搜索”的组合推广到其他结构化模态的后门/对抗研究，如 SQL、配置文件、IaC 脚本、API 调用序列，这些领域同样有严格语法约束与分布漂移问题。
- **损失景观视角的量化**：可引入锐度度量（如梯度范数、Hessian 最大特征值、SAM 的 \(\rho\)-sharpness）与跨分布 ASR 做相关性分析，把本文的经验直觉升级为可检验的假设，甚至发展出“可迁移触发器的锐度下界”类理论。
- **攻防双向利用**：平坦区域触发器更难被自然度检测发现，这一发现可反哺**防御**——例如防御方也可用 SAM 训练“锐度感知的检测器”，或构造对抗性平坦区域来诱导攻击失效；也可研究**触发器反演**在平坦区域下是否同样更困难。
- **更真实的威胁建模**：引入注入覆盖率、仓库流行度、数据去重与来源过滤等变量，把“投毒率”细化为“有效投毒率”，评估攻击在真实采集管线中的存活率。
- **语义层面的隐蔽性评估**：结合变量命名一致性模型、类型推断、数据流分析构建更强的“语义异常”检测器，检验 STAB 在语义维度的真实隐蔽性——这可能是该攻击最脆弱的环节。
- **跨语言与跨任务泛化**：验证同一代理模型产出的触发模式能否跨语言复用（Python 学到的“通用标识符替换倾向”是否迁移到 Java），以及生成任务与分类/检索任务间的迁移差异。
- **与其他可微搜索技术结合**：可对比 Straight-Through 估计、REINFORCE、Gumbel-Softmax 的不同温度策略对触发器质量与隐蔽性的影响，进一步厘清“可微联合优化优于贪心”的具体来源（是全局依赖建模，还是搜索空间更平滑）。

## 10. 一句话总结

STAB 从“损失景观平坦性决定特征可迁移性”的视角重新解释了代码后门触发器的跨数据集失效问题，用 SAM 训练代理模型并在其上做 Gumbel-Softmax 可微联合优化生成语法合法、上下文感知的标识符重命名触发器，在 9 组跨数据集组合、2 个受害模型、2 类生成任务上以平均 80.1% ASR、73.2% 防御后 ASR 与最低检出率显著超越静态与现有动态基线，同时保持干净性能；其贡献在于把“迁移性”与“隐蔽性”这对通常互斥的目标通过优化几何统一起来，但在理论刻画、语义隐蔽性评估、训练期/自适应防御与真实投毒可行性方面仍留有明显空白。

（完）
