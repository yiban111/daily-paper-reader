---
title: "Dormant Backdoor: Weaponizing Model Finetuning for Feasible Backdoor Attacks Against Pretrained Models"
title_zh: 休眠后门：将模型微调武器化以实现针对预训练模型的后门攻击
authors: "Ruitao Li, Jiakai Wang, Hairong Chen, Huihu Ding, Jinghan Zhou, Renshuai Tao"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39480/43441"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 利用模型微调激活预训练模型后门的攻击
tldr: 预训练-微调范式下，现有后门研究多关注预训练阶段注入，且近期微调激活攻击依赖对下游数据分布的白盒假设，缺乏现实性。作者提出Dormant Backdoor，一种无需下游任务先验的微调激活攻击，将梯度优化的通用动态作为过程触发器而非绑定静态输入模式。该方法使预训练模型在正常微调后仍可被激活后门，揭示了模型供应链的新风险。工作强调了干净微调也可能成为攻击武器，对预训练模型安全评估具有重要意义。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39480/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 1274, \"height\": 980}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39480/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 3044, \"height\": 1330}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39480/fig-003.webp\", \"caption\": \"\", \"page\": 7, \"index\": 3, \"width\": 1626, \"height\": 1476}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39480/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 1626, \"height\": 1476}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39480/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 3709, \"height\": 1440}]"
motivation: 现有后门研究多关注预训练阶段注入，微调激活攻击又依赖对下游数据分布的白盒假设，缺乏现实性。
method: 提出Dormant Backdoor，无需下游任务先验，将梯度优化的通用动态作为过程触发器而非绑定静态输入模式。
result: 该方法使预训练模型在正常微调后仍可被激活后门，揭示干净微调亦可被武器化的供应链风险。
conclusion: 强调了预训练-微调范式下模型供应链安全的新威胁，为后门防御评估提供参考。
---

## Abstract
As the pretraining-finetuning paradigm becomes dominant in modern AI, the security of model supply chains faces new risks from backdoor attacks. Existing work primarily studies backdoors injected during pretraining and treats subsequent finetuning with clean data as a defense, while recent finetuning-activated attacks assume white-box access to the downstream data distribution, which is rarely realistic in practice. We introduce Dormant Backdoor, a finetuning-activated attack that requires no prior knowledge of downstream tasks. Instead of binding the backdoor to static input patterns, Dormant Backdoor exploits the universal dynamics of gradient-based optimization as a process-as-trigger mechanism. We formulate the attack as a bilevel optimization problem that simulates the victim's finetuning trajectory on proxy data, and jointly optimizes the poisoned model and trigger under lethality, utility, and stealth objectives. Before finetuning, the poisoned model remains behaviorally close to a clean model and can evade existing backdoor detectors; after finetuning, the same adaptation process reliably amplifies the backdoor on diverse downstream datasets and finetuning strategies. Our results reveal a previously underexplored class of process-as-trigger vulnerabilities and highlight the need for defenses that explicitly secure the model adaptation process.

---

## 论文详细总结（自动生成）

# 《Dormant Backdoor：将模型微调武器化以实现针对预训练模型的后门攻击》论文总结

## 1. 核心问题与研究背景

- **背景**：预训练-微调范式（尤其是 PEFT，如 LoRA、BitFit、Prompt Tuning）已成为现代 AI 的主流。模型提供方在公开仓库发布通用大模型，下游用户下载后基于私有数据微调适配。这引入了**模型供应链攻击面**：恶意提供方可预埋后门。
- **现有研究的不足**：
  - 传统后门研究聚焦于**预训练阶段注入的静态后门**，并将“干净数据微调”视为一种防御手段（可削弱甚至抹除后门）。
  - 近期“休眠型/微调激活型”后门（Latent Backdoor、Hibernated Backdoor）虽已意识到微调动态可被利用，但**普遍假设攻击者拥有下游数据的白盒访问权或高度匹配的代理数据集**，这在实际中很难满足（下游数据私有、任务类型/标签空间/分布均未知）。
- **核心问题**：能否在**完全数据无关（data-agnostic）** 的威胁模型下，设计一种后门——部署前完全休眠、可躲过安全审计，而在受害者正常微调后自动被激活并放大？
- **整体含义**：论文揭示了一类被忽视的 **“过程即触发器”（process-as-trigger）漏洞**，把微调从“防御手段”转变为“攻击武器”，对模型供应链安全评估具有警示意义。

## 2. 方法论

### 核心思想
- 将后门从**静态输入特征**解耦，绑定到**梯度优化的通用动态过程**上。
- 依据两个经验观察：(1) 共享预训练特征空间中，常见语义概念跨数据集具有一致性，提供跨数据集的语义锚点；(2) 多数用户采用相似的梯度优化流程，参数更新轨迹具有可预测、近似数据无关的统计规律。
- 攻击形式化为**双层优化（bilevel optimization）**：内层模拟受害者的微调轨迹，外层据此联合优化中毒初始参数 θp 与触发器 δ。

### 关键技术细节
- **威胁模型**：攻击者为恶意模型提供方，仅能修改预训练初始权重并构造输入触发器，可用的只有公开代理数据集 Da（如 ImageNet），无下游数据 Dv。
- **内层优化（模拟微调轨迹）**：
  - 从 θp,0 ≡ θp 出发，用代理数据采样 mini-batch，执行 k 步梯度下降：θp,i = θp,i−1 − α∇θ Lf(x, y; θp,i−1)。
  - 得到模拟微调后的参数 θ′p ≡ θp,k。关键点是 θ′p 是 θp 的**可微函数**（k 步更新的复合），使外层目标可反向传播穿过模拟微调过程。
- **外层优化（三目标联合）**：
  - **致命性损失** L_lethality：在模拟未来参数 θ′p 上，使带触发器输入 x+δ 被判为目标类 yt，约束 ‖δ‖∞ ≤ ε。
  - **效用损失** L_utility：在 θ′p 上保持代理数据干净样本的分类性能。
  - **隐蔽性损失** L_stealth：借鉴知识蒸馏，用冻结的干净教师模型 θc 与学生模型 θp 的 softmax 输出 KL 散度 DKL(P(x;θc)‖P(x;θp))，强制行为不可区分，使部署前模型与干净模型几乎无异。
  - 总目标：L_total = L_lethality + λu·L_utility + λs·L_stealth，对 θp 与 δ 联合梯度更新。
- **算法流程**（Algorithm 1）：初始化 θp←θc、随机初始化 δ；每个 epoch 内对每个 batch 先做 k 步内层模拟，再计算复合损失，最后同时更新 θp 与 δ。
- **超参数**：λs = 15.0、λu = 1.5、ε = 10/255；默认用 CLIP ViT-B/16、16-shot、LoRA 微调、AdamW + CosineAnnealingLR、学习率 2e-4、20 epochs。

## 3. 实验设计

- **代理数据**：ImageNet（唯一代理数据集）。
- **下游数据集（5 个）**：Caltech101、OxfordPets、Flowers102、Food101、SUN397。
- **两种评估场景**：
  - **未见类别（Unseen Classes）**：每个数据集类别一分为二，一半用于生成攻击，另一半用于微调与测试（沿用 BadCLIP 设置）。
  - **未见数据集（Unseen Datasets）**：仅用 ImageNet 生成后门，受害者在五个下游数据集上分别微调评估，模拟真实模型仓库场景。
  - 另报告两场景的**调和平均（Harmonic Mean）**。
- **对比攻击基线（3 个）**：BadNets、Blended（经典静态触发器）、BadCLIP（视觉-语言模型 SOTA 攻击）。
- **对比防御基线（7 个）**：Neural Cleanse (NC)、STRIP (ST)、GangSweep (GS)、DLTND (DL)、DF-TND (DF)、CBD、CleanCLIP (CC)。
- **评估指标**：微调后干净准确率 CA、微调前攻击成功率 ASR_b（越低越隐蔽）、微调后攻击成功率 ASR_a（越高越致命）。
- **鲁棒性网格**：覆盖架构（ViT-B/32、B/16、L/14）、微调方式（Full-FT、LoRA、BitFit、Prompt Tuning、LayerNorm Tuning）、优化器（SGD/Adam/AdamW）、学习率调度器、学习率（2e-3/2e-4/2e-5）、训练轮数（20/50）。
- **消融实验**：在 Caltech101（未见类别、默认配置）上分别去除双层优化、Stealth Loss、Utility Loss。
- **可视化**：t-SNE 展示微调前后中毒样本 logits 的聚类变化；展示良性图像与触发图像的视觉对比。

## 4. 资源与算力

- **论文正文未明确说明**所使用的 GPU 型号、数量、训练时长或总算力开销。
- 仅可间接推断：实验基于 CLIP ViT-B/16（部分鲁棒性实验用 ViT-B/32、ViT-L/14），涉及双层优化中 k 步展开的反向传播、5 个下游数据集 × 3 次随机种子平均，以及 7 种防御的评估，实际算力需求应较高，但**具体数字缺失**，这在一定程度上影响可复现性评估。

## 5. 实验数量与充分性

- **主要性能实验**：表 1 覆盖 5 个数据集 × 2 个场景 + 调和平均，每格含 CA/ASR_b/ASR_a 三项指标，对比 Clean、BadNets、Blended、BadCLIP、Ours 共 5 种方法。
- **鲁棒性实验**：表 2 约 19 种配置（3 架构 + 5 微调方式 + 3 优化器 + 3 调度器 + 3 学习率 + 2 轮数，部分重叠）。
- **消融实验**：表 3 共 4 组（完整方法 + 3 种去除组件）。
- **防御评估**：表 4 为 4 种攻击 × 7 种防御的通过/检出矩阵。
- **可视化**：t-SNE（微调前后）与触发样本视觉对比各 1 组。
- **充分性评价**：
  - **优点**：覆盖多数据集、两类泛化场景、多微调配置、7 种防御与消融，维度较全面；报告 3 次独立随机种子平均，并给出调和平均以平衡两类场景。
  - **客观/公平性**：基线选择具代表性（静态、多模态 SOTA），防御覆盖统计类、数据受限/无数据类、因果类与微调类，较公平；但防御方法均为面向“静态可搜索后门”设计的，缺少专门针对过程触发后门的防御基线，比较存在一定**偏向性**。
  - **不足**：下游任务全部为图像分类，未覆盖 NLP 或多模态生成任务；代理数据仅 ImageNet 一种；消融仅在单一数据集上做；未报告方差/置信区间之外的统计显著性检验。

## 6. 主要结论与发现

- **休眠—致命转换**：微调前 ASR_b 接近 0（如 Caltech101 上 0.28%），实现完全隐蔽；微调后 ASR_a 在全部五个数据集上均超过 99%（未见类别与未见数据集场景均如此）。
- **效用保持**：CA 与良性模型差距在几个百分点内（如 Caltech101 未见类别 93.83% vs Clean 93.92%）。
- **对微调配置高度鲁棒**：所有配置下 ASR_a ≥ 94%，即使 Full-FT、10 倍学习率或 Prompt Tuning，ASR_a 下降最多约 5%。
- **规避全部七种防御**：BadNets/Blended 被全部检出，BadCLIP 被部分检出，而 Dormant Backdoor 通过全部 7 种检测器，原因是现有防御假设后门“部署前已激活可搜索”，而本攻击的恶意行为只在受害者自身微调后才显现。
- **消融结论**：三个组件缺一不可——去除双层优化导致预部署隐蔽性崩溃（ASR_b 升至 84.95%）；去除 Stealth Loss 使触发器立即生效（ASR_b 99.21%）；去除 Utility Loss 使 CA 骤降至 52.43%。
- **可视化佐证**：t-SNE 显示微调前中毒样本紧贴原始类别簇（休眠），微调后全部坍缩至目标类簇（激活）。

## 7. 优点

- **威胁模型更现实**：首次提出**完全数据无关**的微调激活后门，无需下游任务/数据/标签空间先验，显著提升实际可行性。
- **概念新颖**：“process-as-trigger”范式把攻击面从静态输入特征转移到优化动态本身，开辟了新的漏洞类别。
- **技术设计精巧**：利用 k 步梯度更新的**端到端可微性**实现穿过微调过程的反向传播；用知识蒸馏式 KL 约束实现部署前行为不可区分。
- **多目标协同**：将致命性、效用、隐蔽性显式建模为复合损失，避免“高攻击率但破坏可用性/易被检测”的常见缺陷。
- **评估较全面**：两场景泛化 + 多维鲁棒性网格 + 七种防御 + 消融 + 可视化，论证链条完整；结果一致性强（ASR_a > 99%，防御全通过）。
- **可复现性**：公开代码链接，超参数与默认配置交代清楚。

## 8. 不足与局限

- **算力信息缺失**：未报告 GPU 型号/数量/训练时长，影响复现成本评估与可扩展性判断。
- **任务与模态覆盖有限**：仅在 CLIP/ViT 图像分类上验证，未涉及纯 NLP、多模态生成、检测/分割等任务，泛化性结论外推需谨慎。
- **代理数据单一**：仅用 ImageNet，未讨论代理数据集与下游分布差异更大时的表现（尽管“未见数据集”场景部分缓解）。
- **目标类固定**：默认将每个数据集第一类作为目标类，未系统分析目标类选择对攻击的影响。
- **防御对比的偏向性**：7 种防御均为面向静态后门的既有方法，缺少针对“过程触发”后门的**专用防御**（如监控适应过程、微调轨迹异常检测），因此“规避全部防御”的结论应理解为对现有防御体系的绕过，而非对任何防御都免疫。
- **双层优化的计算代价与超参数敏感性**：内层 k 步展开带来显著计算与显存开销，论文未量化；λs、λu、ε、k、α 等超参的敏感性分析不足。
- **潜在分布重叠**：ImageNet 与部分下游数据集（如 Caltech101、OxfordPets）同属自然图像域，可能部分高估跨域泛化能力。
- **伦理与部署限制**：作为攻击方法，论文对缓解措施仅作呼吁（“需保护模型适配过程”），未给出具体可落地的防御方案或检测原型。
- **统计严谨性**：仅报告 3 次运行平均，未给标准差或显著性检验；部分表格（如鲁棒性）仅单数据集呈现。

（完）
