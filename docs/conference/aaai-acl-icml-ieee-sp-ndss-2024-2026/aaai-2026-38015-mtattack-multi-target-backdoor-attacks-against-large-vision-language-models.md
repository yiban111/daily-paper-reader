---
title: "MTAttack: Multi-Target Backdoor Attacks Against Large Vision-Language Models"
title_zh: MTAttack：针对大型视觉语言模型的多目标后门攻击
authors: "Zihan Wang, Guansong Pang, Wenjun Miao, Jin Zheng, Xiao Bai"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38015/41977"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 针对大型视觉语言模型的多目标后门攻击
tldr: 现有针对大型视觉语言模型的后门攻击多为单目标，威胁有限。本文提出MTAttack，可在一次训练中注入多个独立触发器并对应不同攻击目标。实验证明该多目标攻击在真实场景中构成更强威胁。该工作拓展了多模态大模型后门攻击的形式与危害评估。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 570, \"height\": 571}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-003.webp\", \"caption\": \"\", \"page\": 1, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 1233, \"height\": 951}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 1216, \"height\": 955}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 1252, \"height\": 943}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-007.webp\", \"caption\": \"\", \"page\": 4, \"index\": 7, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-008.webp\", \"caption\": \"\", \"page\": 4, \"index\": 8, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-009.webp\", \"caption\": \"\", \"page\": 4, \"index\": 9, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-010.webp\", \"caption\": \"\", \"page\": 4, \"index\": 10, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-011.webp\", \"caption\": \"\", \"page\": 4, \"index\": 11, \"width\": 452, \"height\": 313}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-012.webp\", \"caption\": \"\", \"page\": 4, \"index\": 12, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-013.webp\", \"caption\": \"\", \"page\": 4, \"index\": 13, \"width\": 697, \"height\": 697}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-014.webp\", \"caption\": \"\", \"page\": 4, \"index\": 14, \"width\": 515, \"height\": 389}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-015.webp\", \"caption\": \"\", \"page\": 4, \"index\": 15, \"width\": 515, \"height\": 389}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38015/fig-016.webp\", \"caption\": \"\", \"page\": 4, \"index\": 16, \"width\": 515, \"height\": 389}]"
motivation: 现有LVLM后门攻击局限于单目标，难以反映真实场景的复合威胁。
method: 提出MTAttack，在一次训练中注入多个独立触发器分别对应不同恶意输出。
result: 实验表明多目标后门可成功植入并保持较高攻击成功率。
conclusion: 该工作揭示了多目标后门对大型视觉语言模型的更强现实威胁。
---

## Abstract
Recent advances in Large Visual Language Models (LVLMs) have demonstrated impressive performance across various vision-language tasks by leveraging large-scale image-text pretraining and instruction tuning. However, the security vulnerabilities of LVLMs have become increasingly concerning, particularly their susceptibility to backdoor attacks. Existing backdoor attacks focus on single-target attacks, i.e., targeting a single malicious output associated with a specific trigger. In this work, we uncover multi-target backdoor attacks, where multiple independent triggers corresponding to different attack targets are added in a single pass of training, posing a greater threat to LVLMs in real-world applications. Executing such attacks in LVLMs is challenging since there can be many incorrect trigger-target mappings due to severe feature interference among different triggers. To address this challenge, we propose MTAttack, the first multi-target backdoor attack framework for enforcing accurate multiple trigger-target mappings in LVLMs. The core of MTAttack is a novel optimization method with two constraints, namely Proxy Space Partitioning constraint and Trigger Prototype Anchoring constraint. It jointly optimizes multiple triggers in the latent space, with each trigger independently mapping clean images to a unique proxy class while at the same time guaranteeing their separability. Experiments on popular benchmarks demonstrate a high success rate of MTAttack for multi-target attacks, substantially outperforming existing attack methods. Furthermore, our attack exhibits strong generalizability across datasets and robustness against backdoor defense strategies. These findings highlight the vulnerability of LVLMs to multi-target backdoor attacks and underscore the urgent need for mitigating such threats.

---

## 论文详细总结（自动生成）

# MTAttack 论文中文结构化总结

## 1. 核心问题与整体含义

- **研究背景**：大型视觉语言模型（LVLM，如 MiniGPT-4、LLaVA、Qwen2.5-VL）在图像描述、视觉问答等任务上表现突出，但其视觉指令微调阶段存在后门攻击风险。攻击者可通过污染训练数据，使模型在输入含触发器时输出恶意或错误文本。
- **核心问题**：现有 LVLM 后门攻击主要关注**单目标攻击**，即一个触发器绑定一个恶意输出。真实场景中，攻击者可能希望在一次训练中植入**多个独立触发器**，分别对应不同攻击目标，从而扩大攻击面。
- **关键挑战**：多目标场景存在严重的**触发器间干扰（inter-trigger interference）**。简单将单目标攻击方法扩展到多目标时，虽然可能产生多个攻击目标，但触发器与目标之间容易出现错配，即目标混淆率高。
- **整体含义**：论文首次系统探索 LVLM 的**多目标后门攻击**威胁，提出 MTAttack 框架，在一次视觉指令微调中建立多个触发器到多个文本攻击目标的准确一一映射。该工作表明多目标后门攻击对 LVLM 构成更强、更灵活的现实威胁，并呼吁研究相应防御。

## 2. 方法论

### 核心思想

MTAttack 采用**基于代理类（proxy class）的多目标攻击**思路：不直接将 poisoned image 的视觉特征拉近某个目标类别，而是为每个触发器生成一个独立的“代理类”，将触发器绑定到该代理类，再将代理类与文本攻击目标建立映射。这样可在视觉潜在空间中保证不同触发器对应的 poisoned 样本彼此可分，减少触发器间干扰和语义破坏。

### 攻击流程

MTAttack 包含两个阶段：

1. **视觉触发器优化阶段**
   - 同时优化 \(N\) 个视觉触发器 \(\Delta=\{\delta_1,\dots,\delta_N\}\)，满足 \(\|\delta_i\|_\infty \le \epsilon\)。
   - 使用与最终微调干净集 \(D_0\) 不相交的干净数据 \(D'_0\) 进行触发器优化。
   - 为每个触发器构造 poisoned 图像 \(\hat v_i = v+\delta_i\)，并通过受害模型视觉编码器 \(g_\phi(\cdot)\) 提取特征。
   - 引入两个约束：**PSP** 与 **TPA**。

2. **后门植入阶段**
   - 将优化后的 \(N\) 个触发器叠加到干净图像上，生成 poisoned 数据集 \(\hat D=\{(\hat v,\hat t,\hat y_c)\}\)。
   - 将 \(\hat D\) 与干净数据 \(D_0\) 混合，按官方视觉指令微调流程训练 LVLM。
   - 每个触发器 \(\delta_i\) 对应目标概念 \(c_i\)，使模型在遇到含 \(\delta_i\) 的图像时输出与 \(c_i\) 相关的目标文本。

### 关键技术细节

- **Proxy Space Partitioning（PSP）约束**
  - 为 \(N\) 个攻击目标生成 \(N\) 个代理类，加上干净图像类，共 \(N+1\) 类。
  - 每类由可学习原型向量 \(p_j\) 表示。
  - 对每个 poisoned 图像特征 \(g_\phi(\hat v_i)\)，最大化其与自身代理类原型 \(p_i\) 的余弦相似度，同时最小化与其他代理类原型及干净类原型的相似度。
  - 公式为：
    \[
    L_{\mathrm{PSP}}=\mathbb{E}_{v\sim D'_0}\left[-\sum_{i=1}^{N}\log\frac{\exp(s(i,i))}{\sum_{k=0}^{N}\exp(s(i,k))}\right]
    \]
    其中 \(s(i,j)=\mathrm{Sim}(g_\phi(\hat v_i),p_j)\)，\(j=0\) 表示干净类。
  - 作用：最大化不同代理类在视觉特征空间中的分离度，保证不同触发器的 poisoned 样本不重叠。

- **Trigger Prototype Anchoring（TPA）约束**
  - 为每个代理类维护可学习原型 \(p_i\)，用 L2 距离将 poisoned 样本特征拉近其对应原型：
    \[
    L_{\mathrm{TPA}}=\mathbb{E}_{v\sim D'_0}\left[\sum_{i=1}^{N}\|g_\phi(\hat v_i)-p_i\|_2^2\right]
    \]
  - 作用：使同一代理类内样本紧密聚类，降低语义扰动，保持干净数据上的生成质量。

- **联合优化**
  \[
  (\Delta^*,P^*)=\arg\min_{\Delta,P} L_{\mathrm{PSP}}(\Delta,P)+\lambda L_{\mathrm{TPA}}(\Delta,P)
  \]
  其中 \(\lambda\) 平衡两个约束。

- **后门植入损失**
  - 干净数据上保持标准指令微调损失；
  - poisoned 数据上对 \(N\) 个触发器的目标文本 token 计算交叉熵，并取平均：
    \[
    L_{\mathrm{imp}}=\mathbb{E}_{(v,t,y)\sim D_0}[-\sum_{j=1}^{L}\log P(y_j|v,t)]
    +\mathbb{E}_{(\hat v,\hat t,\hat y)\sim \hat D}[-\frac{1}{N}\sum_{i=1}^{N}\sum_{j=1}^{L}\log P(\hat y_{c_i,j}|\hat v_i,\hat t)]
    \]
  - 通过该单次微调过程建立触发器与目标文本的一一映射。

### 威胁模型

- **灰盒攻击**：攻击者仅能访问受害 LVLM 的视觉编码器 \(g_\phi(\cdot)\) 进行触发器优化；无法访问或修改完整 LVLM 微调过程。
- 攻击者只能向干净数据中注入有限数量的 poisoned 样本，最终微调遵循官方流程。
- 该设定贴近现实：CLIP、EVA 等视觉编码器公开可用，而完整 LVLM 架构通常 proprietary。

## 3. 实验设计

- **受害模型**：
  - MiniGPT-v2
  - LLaVA-1.5-7b
  - Qwen2.5-VL-7b

- **数据集与场景**：
  - 图像描述基准：**Flickr-30K** 与 **COCO**。
  - 从训练集采样 \(D'_0\) 优化触发器；将触发器应用到 \(M\) 张干净图像生成 poisoned 数据；与 \(D_0\) 混合进行单次视觉指令微调。
  - 使用 **cc-sbu-align** 作为干净数据补充，以维持正常样本能力。
  - 评估时在测试集干净图像上叠加多个触发器，考察攻击效果。

- **攻击设置**：
  - 目标数 \(N=\{1,2,4\}\)，即单目标、双目标、四目标攻击。
  - 多目标结果在多个目标上取平均。

- **评价指标**：
  - **ASR（Attack Success Rate）**：输出包含对应攻击目标的比例，越高越好。
  - **TCR（Target Confusion Rate）**：输出包含某个攻击目标但与触发器不匹配的比例，越低越好，用于衡量触发器-目标映射准确性。
  - **CIDEr**：干净图像生成描述质量；Qwen2.5-VL 因架构与目标差异，改用 **GPT-4o 评分**。

- **对比方法**：
  - Blended
  - SIG
  - ReFool
  - WaNet
  - VL-Trojan
  - 均按原论文实现并适配到多目标场景；所有方法使用相同的单次微调流程。

- **实验类型**：
  - 多目标攻击有效性（表 1）。
  - 跨数据集泛化：Flickr-30K 优化触发器，COCO 测试（表 2）。
  - 跨目标概念泛化：固定触发器，改变攻击概念为 person、road sign、behavior、website（表 3）。
  - 后门检测防御：检测器 AUC 与剔除最可疑样本后的 ASR/TCR（表 4）。
  - 输入级防御：Gaussian blur、Random Crop、JPEG 压缩（表 5）。
  - 良性数据继续微调防御（图 5）。
  - PSP/TPA 消融实验（表 6）。

## 4. 资源与算力

- **论文正文未明确报告**使用的 GPU 型号、GPU 数量、训练时长、显存消耗或总算力。
- 也未给出触发器优化与视觉指令微调的具体迭代次数、batch size、优化器超参数、poison 样本数量 \(M\)、污染比例、扰动预算 \(\epsilon\) 的具体数值、\(\lambda\) 取值等关键复现信息。
- 因此无法从正文评估该方法的计算成本和可复现性；相关细节可能位于附录或预印本中，但当前文本未提供。

## 5. 实验数量与充分性

- **实验数量较多**：
  - 表 1 覆盖 3 个受害模型 × 2 个数据集 × 3 种目标数（1/2/4），共 18

组主实验配置（每个配置同时报告 ASR、TCR 与干净性能）。此外，表 2 的跨数据集实验覆盖 3 个模型 × 2 个迁移方向；表 3 覆盖多个目标概念；表 4—表 5 分别评估后门检测与输入级防御；图 5 评估良性数据继续微调；表 6 对 PSP/TPA 做消融。因此，从实验矩阵看，论文在主攻击有效性、泛化性、防御鲁棒性和模块贡献四个层面均有覆盖，实验数量在同类后门攻击论文中属于较充分水平。

- **充分性优点**：
  - 同时覆盖 MiniGPT-v2、LLaVA-1.5-7b、Qwen2.5-VL-7b 三类代表性 LVLM，模型多样性较好。
  - 不仅报告 ASR，还引入 TCR 衡量触发器—目标错配，直接对应多目标攻击的核心挑战，评价指标与问题设定匹配。
  - 跨数据集与跨目标概念实验能够部分说明触发器优化并非只记忆固定图像—文本配对，而具有一定迁移性。
  - 防御实验覆盖“检测—剔除”和“输入变换”两类常见防线，并考察继续微调能否削弱后门，较贴近实际部署。
  - PSP/TPA 消融能够支撑“代理空间分离 + 原型锚定”的设计动机。

- **充分性不足**：
  - 主实验虽多，但正文未明确报告多次随机种子下的均值/方差、显著性检验或置信区间，难以判断 ASR/TCR 差异是否稳定。
  - 目标数仅考察 \(N=\{1,2,4\}\)，对更大规模多目标（如 \(N=8,16\)）及目标数增长时的性能退化、原型冲突和训练成本缺乏系统分析。
  - 未充分扫描污染样本数量 \(M\)、污染比例、扰动预算 \(\epsilon\)、触发器尺寸/位置、\(\lambda\) 等关键因素，缺少攻击成功率—隐蔽性—干净性能的权衡曲线。
  - 防御评估以已有检测器和简单输入变换为主，缺少针对多触发器场景的自适应防御、触发器聚类检测、以及更强微调清洗策略的对比。
  - 文本攻击目标多为概念级描述，未充分讨论长文本、细粒度属性、否定句或安全敏感内容等更复杂目标下的映射稳定性。
  - Qwen2.5-VL 使用 GPT-4o 评分，虽可理解，但引入外部模型主观性与成本，缺少与人工评估或自动指标的相关性分析。

## 6. 批判性评价

- **主要贡献**：
  - 首次系统提出并形式化 LVLM 的**多目标后门攻击**问题，指出简单扩展单目标方法会产生严重的触发器间干扰与目标混淆。
  - 提出基于代理类的 MTAttack 框架，通过 PSP 在视觉特征空间中划分代理区域、通过 TPA 将 poisoned 样本锚定到对应原型，从表征层面缓解多触发器冲突。
  - 在多个 LVLM、多个数据集和多种防御下验证攻击有效性，并引入 TCR 作为多目标后门攻击的重要评价维度。

- **方法优势**：
  - 攻击设定为灰盒，仅依赖公开视觉编码器，较符合现实威胁模型。
  - 代理类机制不是简单增加触发器数量，而是显式建模“触发器—代理类—目标文本”的中间映射，设计具有针对性。
  - 单次视觉指令微调即可植入多个触发器，攻击效率较高，威胁含义强。

- **潜在局限**：
  - 代理类数量随攻击目标数线性增长，原型学习与特征分离在目标数很大时可能退化；论文未充分证明其可扩展性。
  - 触发器优化依赖受害模型视觉编码器，若实际部署中编码器不可访问或与替代编码器差异较大，攻击迁移性仍需进一步验证。
  - 实验集中在图像描述任务，视觉问答、指代定位、多轮对话等任务上的多目标后门效果尚未充分覆盖。
  - TCR 的定义可能依赖目标关键词匹配，对语义等价但措辞不同的输出是否计为混淆缺少细粒度讨论。
  - 防御评估虽多，但整体仍显示现有防御难以同时降低 ASR 与 TCR，说明防御需求被强调，但论文未提出对应防御方案。
  - 未报告算力、训练时长与关键超参数，降低复现与成本评估的完整性。

## 7. 可复现性与资源信息

- 正文未给出 GPU 型号、数量、显存、训练时长、触发器优化迭代次数、微调 epoch、batch size、优化器及学习率等细节。
- 未明确 poison 样本数量 \(M\)、污染比例、扰动预算 \(\epsilon\)、触发器尺寸、\(\lambda\) 取值、代理原型初始化方式及训练稳定性处理。
- 未说明是否公开代码、触发器参数或 poisoned 数据构造脚本；若仅依赖论文描述，完整复现难度较高。
- 由于缺少算力与超参数报告，难以比较 MTAttack 与基线方法在同等训练预算下的效率，也难以评估其在大规模 LVLM 上的实际攻击成本。

## 8. 结论与启示

MTAttack 的核心结论是：LVLM 视觉指令微调不仅面临单目标后门威胁，还可被植入多个独立触发器并建立较准确的触发器—目标映射；触发器间干扰是多目标攻击的关键瓶颈，而代理空间划分与原型锚定能够显著缓解该问题。该工作将 LVLM 后门攻击从“单触发器单目标”推进到“多触发器多目标”，对数据供应链安全、模型微调平台和开源视觉编码器滥用风险提出了更强警示。未来值得研究的方向包括：面向多触发器的检测与聚类防御、对代理类机制的鲁棒微调、跨任务多目标后门、以及更大目标规模下的可扩展攻击与防御评估。

（完）
