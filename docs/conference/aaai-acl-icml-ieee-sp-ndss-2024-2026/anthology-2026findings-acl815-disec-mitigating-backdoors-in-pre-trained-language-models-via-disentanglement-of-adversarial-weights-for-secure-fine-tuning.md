---
title: "DiSec: Mitigating Backdoors in Pre-trained Language Models via Disentanglement of Adversarial Weights for Secure Fine-Tuning"
title_zh: DiSec：通过对抗日权重解耦缓解预训练语言模型后门以保障安全微调
authors: "Sunanda Das, Qinghua Li"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.findings-acl.815.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 缓解预训练语言模型中的任务无关后门以保障安全微调
tldr: 任务无关后门可污染预训练语言模型并在下游全量微调后依然存活，使从业者难以信任第三方检查点，而现有防御多依赖有毒数据或触发器知识等特权假设。作者提出DiSec，仅使用干净辅助文本、无需下游监督与攻击签名，通过分离与正常参数不一致的可疑分量进行净化。该框架实现了标签高效且鲁棒的后门缓解，提升了安全微调的可行性。
source: ACL-2026-Findings
selection_source: conference_retrieval
motivation: 任务无关后门能在全量微调后存活，现有防御依赖有毒数据或触发器知识，实用性受限。
method: 提出DiSec净化框架，仅用干净辅助文本，借助模型内部信号分离可疑对抗参数分量。
result: 实验表明DiSec在无需攻击签名的条件下有效降低后门效果并保持下游任务性能。
conclusion: 该工作为不可信第三方检查点提供了标签高效、贴近现实的后门净化方案。
---

## Abstract
Task-agnostic backdoor attacks can contaminate pre-trained language models (PLMs) in a way that survives downstream adaptation, even under full fine-tuning, making it difficult for practitioners to trust third-party checkpoints. Existing defenses often rely on privileged assumptions (e.g., access to poisoned data or trigger/target knowledge), thereby limiting their applicability in realistic settings. We present DiSec, a robust and label-efficient purification framework that uses only clean auxiliary text and does not rely on downstream supervision or attack signatures. DiSec elicits model-internal signals from this clean data to separate suspicious parameter components that are inconsistent with benign behavior, and then flags anomalous structures by jointly leveraging complementary spectral and generative views of outliers. Finally, DiSec performs a structure-preserving repair via layer-local prototype-based mean correction, yielding an idempotent update that depends only on non-adversarial statistics. Across diverse downstream classification tasks and PLM backdoor strategies, DiSec substantially suppresses attack success while preserving clean-task utility, offering a practical path to securing fully fine-tuned PLMs before deployment. The codes are publicly available at https://github.com/das-sunanda/DiSec.

---

## 论文详细总结（自动生成）

# DiSec 论文结构化总结

## 1. 核心问题与研究动机

- **背景**：BERT、RoBERTa 等预训练语言模型（PLM）的开放权重生态（Hugging Face、Model Zoo）使得第三方检查点被广泛复用，但其来源难以完全信任。
- **核心威胁**：**任务无关后门攻击**（如 BadPre、POR、NeuBA）在预训练阶段植入后门，模型在干净输入上表现正常，遇到特定触发词时输出攻击者控制的结果；该后门能**在全量下游微调后继续存活**，一个被污染的 PLM 可危害多个下游应用。
- **现有防御的局限**：多数方法依赖**特权假设**——需要访问毒化数据、触发器/目标类别知识，或需要干净的预训练参考权重；也有方法要求下游标注数据，这在真实场景中往往不可得。
- **研究目标**：提出一种**无需任务监督、无需攻击签名、仅用干净辅助文本**的通用净化框架，在微调前对后门 PLM 进行"消毒"，从而保护所有可能的下游模型。

## 2. 方法论

### 2.1 核心思想
- 从干净辅助文本中提取**模型内部信号**（基于干净数据累积的梯度残差），利用**谱（线性）** 与**生成（非线性）** 两种互补视角识别与良性行为不一致的可疑参数行，再以**层内原型均值**进行结构保持式修复。
- 同时引入**Top-K 脆弱层定位**，只对后门效应集中的少数 Transformer 层做检测与修正，以降低对干净任务的扰动。

### 2.2 检测阶段：双路异常检测
- **梯度残差提取**：对每层权重 $W^{(0,l)}$，在干净辅助集 $D_a$ 上累积梯度 $G_l$，并投影掉与原始权重共线的分量，得到正交残差
  $$G_l^{\perp}=G_l-\frac{\langle G_l,W^{(0,l)}\rangle}{\|W^{(0,l)}\|^2}W^{(0,l)}$$
- **SVD 谱检测**：对 $G_l^{\perp}$ 做秩-$k$ 截断 SVD，得到系数矩阵 $C^{(l)}=U_k\Sigma_k$；对每行取前 $k$ 个奇异分量上标准化系数的最大值作为异常分数 $z_i$，以 $\tau_{svd}=\mu_z+\tau\sigma_z$ 为阈值标记谱离群行。
- **VAE 生成检测**：以残差行 $x_i$ 为样本训练轻量 VAE（128–64–16 潜空间），以逐行重建误差 $e_i$ 为异常分数，阈值 $\tau_{vae}=\mu_e+\tau\sigma_e$ 标记生成式离群行。
- **统一离群集合**：进行 $T$ 轮梯度累积与检测，取并集
  $$S^{(l)}_{union}=S^{(l)}_{svd}\cup S^{(l)}_{vae}$$
  并集策略偏向**召回率**，避免漏检；其补集 $S^{(l)}_{normal}$ 作为干净参照。

### 2.3 修正阶段：原型均值修复
- 对每层计算干净行的均值原型 $\bar{w}^{(l)}$，将 $S^{(l)}_{union}$ 中所有离群行替换为 $\bar{w}^{(l)}$。
- 该操作具有三点性质：**降偏**（拉向干净更新质心）、**结构保持**（干净行不动，原型位于层参数流形内）、**幂等**（重复应用不产生漂移）。

### 2.4 Top-K 脆弱层定位
- **代理触发器挖掘**：用 HotFlip 式离散优化在辅助文本上挖掘定长（$r=5$）通用触发器，插入 [CLS] 之后；损失为模型对**自身伪标签**的交叉熵（**完全无标签**）。
- **选择性神经元激活分析**：对每层提取 [CLS] 隐状态并二值化（正值即激活），计算干净输入与触发输入激活的重叠比
  $$r_\ell=\sum_i A_{\ell,i}\big/\sum_i B_{\ell,i}$$
  重叠比越低说明触发器激活了干净数据上沉默的神经元，即后门通路。
- **层选择规则**：基于相邻层差分 $\Delta r_\ell=r_\ell-r_{\ell-1}$，采用"最陡下降起点 + 向后扩展 + 向前延续并过滤反弹尖峰"的三步启发式，得到 Top-K 层。

## 3. 实验设计

- **模型与攻击**：BERT-base-uncased（BadPre、POR、NeuBA 三种任务无关后门）与 RoBERTa-base（NeuBA），共 4 个后门检查点。
- **下游数据集**：SST-2（情感二分类）、HSOL（仇恨言论二分类，按 Chen et al. 2022d 合并标签）、AG News（4 类主题分类）。
- **指标**：干净准确率 ACC、平均攻击成功率 AASR、最大攻击成功率 MASR（多触发器最坏情况）。
- **辅助数据**：20k 条随机采样的 BookCorpus 句子，无下游标签、无触发器、无毒样本。
- **对比基线**：
  - 无防御（w/o defense）
  - Fine-Pruning（FP）及其全局变体 FP-GM（逐层剪 20%）、FP-GA（剪 40%）
  - ONION（推理期输入过滤，阈值设为乐观上界 0）
  - RECIPE（基于辅助 MLM 的权重空间净化，POR/BadPre/NeuBA 分别训练 4/8/10 轮）
- **评估公平性措施**：所有防御变体使用统一的微调协议（2 epoch、batch 32、AdamW、lr=2e-5、线性调度、相同随机种子）。

## 4. 资源与算力

- 明确说明：所有实验（含净化与后续微调）均在**单块 NVIDIA A100 GPU（40GB）** 上完成。
- 计算致谢：使用 Arkansas High Performance Computing Center（由 NSF 多项资助及阿肯色州经济发展委员会资助）。
- **未明确说明**：总 GPU 小时数、全部实验的总训练时长、不同配置的重复次数等。
- 论文提供了**运行时间对比**（SST-2 / POR-BERT 场景）：DiSec 174.79s，FP 97.91s，RECIPE 189.93s，FP-GM 226.35s，FP-GA 238.60s，ONION 1451.40s。

## 5. 实验数量与充分性

- **主要对比实验**：4 个后门模型 × 3 个数据集 × 6 种方法（含无防御），约 72 组指标配置。
- **干净模型测试**：在干净 BERT/RoBERTa 上应用 DiSec（Top-K 与 Top-9 两种设置）。
- **自适应攻击实验**：构造防御感知的 BadPre 变体（加入异常正则项 $L_{anom}$），在 3 个数据集上评估。
- **消融实验**：
  - 检测器组合（VAE-only / SVD-only / 交集 / 并集）；
  - 修正策略（RC1 复制干净模型行 / RC2 局部噪声 / RC3 全局噪声 / 原型均值）；
  - 盲式全 12 层 vs 选择性 Top-K；
  - Top-K 层数敏感性（K=3/4/5/9/10/11/12 等）；
  - 辅助数据规模（1k/5k/10k/15k/20k/25k）；
  - 辅助数据来源替换（BookCorpus → OpenWebText）；
  - 超参数敏感性（谱秩 $k\in\{20,30,40\}$、检测轮数 $T$）。
- **充分性评价**：
  - **优点**：覆盖面较广，包含主实验、消融、自适应攻击、干净模型、数据规模与来源替换、超参数敏感性，且附录提供离群点可视化与算法伪代码。
  - **不足**：**每个配置仅报告单次运行**（固定种子），**未提供标准差/置信区间**，统计显著性无法判断；部分基线（如 ONION 阈值设为 0）可能未充分调优；不同方法对辅助数据使用方式不完全等价（RECIPE 用 MLM 目标，DiSec 用分类式伪标签），横向比较存在一定不完全对等性。

## 6. 主要结论与发现

- **有效性**：DiSec 在 4 个后门 PLM × 3 个数据集上**一致地大幅降低 AASR/MASR**，同时保持有竞争力的 ACC。例如 POR-BERT/HSOL：AASR 从 46.63% 降至 5.61%；NeuBA-BERT/AG News：AASR 从 82.39% 降至 3.23%；NeuBA-RoBERTa/SST-2：AASR 从 75.80% 降至 12.20%。
- **稳定性**：部分基线（如 ONION、RECIPE）在单一数据集上表现优异，但跨数据集泛化差；DiSec 在多数指标上接近或达到最优，且跨场景稳定。
- **Top-K 定位有效**：仅用**挖掘的代理触发器**得到的脆弱层与使用**真实触发器**观察到的层间分歧点基本吻合，说明无需真实触发器即可定位后门敏感层。
- **对干净模型友好**：在干净 BERT/RoBERTa 上应用 DiSec，ACC 变化很小（SST-2 上甚至略有提升 +0.16/+0.33），Top-9 激进设置下也仅下降约 1–2 个百分点。
- **自适应攻击下仍有效**：面对防御感知的 BadPre 变体，AASR 从 100%/100%/32.26% 降至 19.45%/5.70%/2.99%。
- **消融结论**：VAE ∪ SVD 并集检测、原型均值修正、选择性 Top-K 三者缺一不可；盲目全层修正或噪声式修正效果显著下降。
- **鲁棒性**：对辅助语料规模（1k–25k）不敏感，替换为 OpenWebText 后仍保持强净化效果。

## 7. 优点

- **威胁模型现实**：严格无标签、无触发器、无毒数据、无干净参考权重，贴近真实第三方检查点使用场景。
- **方法设计巧妙**：SVD（线性主方向）+ VAE（非线性流形）双视角互补，并集策略偏向召回；修正操作**幂等、结构保持、低偏**，理论性质清晰。
- **数据驱动的层定位**：用无标签 HotFlip 挖掘代理触发器 + 神经元激活重叠比定位脆弱层，避免全层盲目修改，兼顾效果与干净性能。
- **实验较全面**：涵盖
