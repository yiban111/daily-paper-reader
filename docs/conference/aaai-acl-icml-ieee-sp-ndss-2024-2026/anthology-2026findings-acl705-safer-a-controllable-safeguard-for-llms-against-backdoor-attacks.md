---
title: "SAFER: A Controllable Safeguard for LLMs against Backdoor Attacks"
title_zh: SAFER：面向大模型后门攻击的可控防护机制
authors: "Zirui Hu, Zheng Zhang, Yingjie Wang, Dacheng Tao"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.findings-acl.705.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 针对大模型后门攻击的推理时防御
tldr: 现有针对大模型后门的推理时防御缺乏对误接受率的显式控制，难以满足高安全场景需求。本文提出SAFER框架，在无需触发器先验知识的情况下对误接受率提供可证明的控制。实验表明其能有效过滤后门输入并保证安全边界，为安全敏感场景下的LLM部署提供了可靠保障。
source: ACL-2026-Findings
selection_source: conference_retrieval
motivation: 现有推理时防御缺乏对误接受率的显式可控保证。
method: 提出SAFER框架，无需触发器先验即可可证明地控制误接受率。
result: 实验表明能有效过滤后门输入并保证安全边界。
conclusion: 为安全敏感场景的LLM后门防御提供可靠保障。
---

## Abstract
Large language models (LLMs) have achieved remarkable performance across a wide range of natural language processing (NLP) tasks. However, they remain susceptible to backdoor attacks, where adversaries embed hidden triggers in the input to induce malicious, attacker-specified behaviors. While existing inference-time defenses aim to mitigate such threats by detecting and filtering poisoned inputs, they often lack explicit control over the false acceptance rate (FAR)—a critical requirement in safety-sensitive settings where even rare failures can lead to catastrophic consequences. To address this challenge, we propose SAFER , a novel inference-time defense framework that provides explicit and provable control over FAR without requiring prior knowledge of backdoor samples. SAFER leverages distributional information from available data to estimate the likelihood that an input is clean and selects inputs accordingly. From a theoretical perspective, we demonstrate that SAFER asymptotically guarantees control of the true FAR. Empirical evaluations on three benchmark datasets across diverse backdoor attack scenarios show that SAFER consistently achieves reliable FAR control while maintaining high detection power, significantly outperforming existing inference-time defenses.

---

## 论文详细总结（自动生成）

# SAFER：面向大模型后门攻击的可控防护机制——论文总结

## 1. 核心问题与研究动机

- **背景**：大语言模型（LLM）在各类 NLP 任务上表现优异，但在预训练/微调阶段易被植入后门——攻击者通过隐藏触发器使模型在推理时产生攻击者指定的恶意行为（如把有毒内容判为无毒）。
- **关键痛点**：现有推理时防御（基于检测/打分的外部过滤器）虽灵活、无需访问模型内部，但**普遍缺乏对误接受率（FAR，即被误判为干净的后门样本比例）的显式控制**。
- **为何重要**：在金融贷款审批、医疗、政务等安全敏感场景，哪怕极少数后门样本被放行也可能造成灾难性后果，因此需要可证明、可控的 FAR 上界。
- **为何困难**：传统 FDR 控制方法（如 BH 过程）需要已知后门样本的校准集，而实际中无法预知攻击类型，难以构造代表性后门样本，故不能直接套用。
- **论文定位**：提出 SAFER（Statistically-Assured False acceptance Rate control），在**仅有干净校准集、无后门先验知识**的条件下，为 FAR 提供显式且可证明的控制。

## 2. 方法论

### 核心思想
将现有打分函数提供的隐式区分信号，转化为显式的统计量（每个测试样本为干净样本的后验概率），再通过带约束的优化实现"在 FAR 不超过阈值 α 的前提下尽可能多接受样本"。

### 关键技术细节

- **问题设定**：防御者仅拥有后门模型、干净校准集 \(D_{cal}\sim P_{clean}\) 与混合测试集 \(D_{test}=D_{poi}\cup D_{clean}\)；测试样本服从干净/后门混合分布，先验干净概率为 \(\pi_c\)。
- **目标**：选择子集 \(\hat S\) 作为干净样本，使 FAR 不超过用户阈值 α，同时最大化 Power（正确接受干净样本的比例）。

**（1）不确定性量化**
- 设打分函数为 \(W\)，样本得分为 \(w_j=W(x_j)\)；干净与后门样本得分分布分别为 \(\rho_c\)、\(\rho_p\)。
- 定义后验干净概率（类 local FDR）：
  \[
  L_j=\Pr(\delta_j=0\mid w_j)=\frac{\rho_c(w_j)\cdot\pi_c}{\rho_{mix}(w_j)}
  \]
- 用干净校准集与测试集得分分别估计 \(\hat\rho_c\)、\(\hat\rho_{mix}\)（标准密度估计），并用 Storey 等人的方法基于经验 p 值估计先验 \(\hat\pi_0\)（式 6，超参 λ 平衡偏差与方差，实验中取 0.9），最终得到 \(\hat L_j\)。

**（2）决策制定**
- 由 Lemma 1，FAR 可写成 \(\mathbb{E}\big[\sum_j (1-L_j)(1-\hat\delta_j)/(1\vee\sum_j(1-\hat\delta_j))\big]\)，用 \(\hat L_j\) 替代得到估计 FAR（式 9）。
- 求解约束优化：\(\min_{\hat\delta}\sum_j\hat\delta_j\ \text{s.t.}\ \widehat{FAR}(\hat\delta)\le\alpha\)。
- **贪心算法（Algorithm 1）**：将 \(\hat L_j\) 降序排列，依次接受，记录满足估计 FAR ≤ α 的最大前缀长度 \(K^*\)，前 \(K^*\) 个接受、其余拒绝。该策略在固定接受数量下使估计 FAR 最小，故为最优前缀选择。

**（3）理论保证**
- 在密度函数 Hölder 连续、有界、紧支撑（Assumption 1）及后门得分与干净得分可分离（Assumption 2）下，证明：
  \[
  FAR(\hat\delta)\le \alpha+\Delta(n_0,n_1),\quad \Delta = C_1 n_0^{-\frac{\beta_1}{2(1+\beta_1)}}\sqrt{\log n_0}+C_2 n_1^{-\frac{\beta_2}{2(1+\beta_2)}}\sqrt{\log n_1}
  \]
- 推论 1：当校准集与测试集规模趋于无穷时，FAR 渐近不超过 α。

## 3. 实验设计

- **数据集（3 个文本分类基准）**：Yelp（情感分析）、AG News（主题分类）、HSOL（有毒内容检测）。
- **受害模型（3 个）**：BERT-base-uncased、RoBERTa-Base、Pythia-1B。
- **后门攻击（4 种）**：BadNets（稀有词触发）、AddSent（整句触发）、StyleBkd（文本风格触发）、SynBkd（句法模式触发）；训练数据投毒率 20%，训练 5 epochs、学习率 \(2\times10^{-5}\)。
- **评分函数（2 种）**：马氏距离（MDS）与激活统计（BadActs），分别记为 "-md"、"-badacts"。
- **对比基线（5 个）**：STRIP、BKI、RAP、CUBE、SCM；其中 BKI/CUBE 原用于训练数据检测，被适配到推理时过滤；SCM 原本控制误拒率（FRR），作为对照。
- **评估指标**：FAR 与 Power；主实验设 α=0.1、测试集投毒率 20%，所有实验重复 10 次取平均。
- **消融/敏感性分析**：不同投毒率（0.2–0.6）、不同 FAR 阈值 α（0.01–0.15）、超参 λ（0.1–0.9）、校准/测试集规模（500–2000）、Assumption 2 的实证验证、高斯模拟实验。

## 4. 资源与算力

- 文中明确说明：**单块 NVIDIA V100 GPU（32GB 显存）**，使用 PyTorch 与 Transformers 库、AdamW 优化器。
- 所有实验重复 10 次并报告平均值。
- **未明确说明**总训练时长、GPU 总卡时或能耗等细节；仅在附录提到 AG News 上 SynBkd 全量投毒训练"超过 100 小时"故采样 10% 数据，可侧面反映部分计算开销。

## 5. 实验数量与充分性

- **规模**：3 数据集 × 4 攻击 × 3 模型（主表为 BERT/RoBERTa，Pythia 在附录）× 2 评分函数，另加投毒率、阈值、λ、样本规模等多组实验，数量较多、覆盖面广。
- **充分性**：主实验 + 多维度鲁棒性分析 + 理论假设的实证检验（附录 Table 5）+ 模拟实验（Table 6）+ 样本规模实验（Table 7），整体较充分。
- **客观与公平性**：
  - 基线均使用官方/默认超参，并在相同校准集上按各自原论文方式确定阈值，比较条件相对统一。
  - 但存在一定不对等：BKI、CUBE 原为训练时方法，被改造为推理时过滤，可能并非其最优使用方式；SCM 原本优化 FRR 而非 FAR，二者目标不完全一致。
  - 表格中"加粗"规则为"控制住 FAR（≤0.1）或 FAR 最低者"，部分场景下 SAFER 的 FAR 略高于 0.1（如 BERT 上 SAFER-badacts 在 AGNews-BadNets 为 0.112），说明控制并非绝对严格，论文以"多数场景"表述，较为诚实。

## 6. 主要结论与发现

- SAFER 在**大多数攻击场景与数据集上稳定将 FAR 控制在阈值 α 以内**，而 BKI、CUBE、SCM 等基线虽在个别攻击下表现优异，却无法跨场景保持一致控制。
- 在投毒率从 0.2 升至 0.6 时，SAFER 仍保持 FAR 低于阈值；投毒率越高控制越难，符合预期。
- 在 α 从 0.01 到 0.15 变化时，SAFER 能随阈值调整并维持控制，甚至在 α=0.01 的严格条件下多数场景仍有效。
- 对超参 λ 在 0.4–0.95 范围内较稳健。
- 与 RAP 等偶尔获得极低 FAR 但 Power 大幅下降的方法相比，SAFER 在 **FAR 控制与 Power 之间取得更优折中**。
- 理论上证明 FAR 渐近受 α 约束，误差项随样本量增大而衰减。

## 7. 优点

- **问题定位精准**：首次系统性地将"推理时后门防御"与"显式、可证明的 FAR 控制"结合，切中安全敏感场景的真实需求。
- **无需后门先验**：仅依赖干净校准集与混合测试集的分布信息，规避了 FDR 类方法需已知后门样本的根本障碍。
- **理论支撑扎实**：给出渐近 FAR 上界及收敛速率，并配套完整的辅助引理与证明（附录 A），严谨性较高。
- **算法简洁高效**：贪心前缀选择在理论上最优，实现简单、易于落地。
- **框架通用**：可与不同打分函数（MDS、BadActs）即插即用地组合，具备良好扩展性。
- **实验较全面**：多数据集、多攻击、多模型、多阈值、多超参、多样本规模，并对核心假设做了实证验证。

## 8. 不足与局限

- **有限样本行为不清**：论文自述渐近保证强，但有限样本下的 FAR 界尚不明确，资源受限场景下的可靠性有待研究。
- **依赖 Assumption 2（得分可分离）**：虽附录用实验显示违反比例极低，但这仍是较强假设；当攻击更隐蔽、得分分布重叠严重时，理论保证可能失效（模拟实验也显示分布接近时违反比例上升、控制变差）。
- **Power 优化不足**：方法以 FAR 控制为核心，未专门优化检测功效；部分场景（如 HSOL 上 SAFER-md）Power 明显偏低（0.555、0.694 等）。
- **任务覆盖有限**：实验均为文本分类任务，未涉及生成式任务、指令微调模型或更贴近真实 LLM 应用的后门场景。
- **需要干净校准集**：实际部署中获取完全干净的校准数据本身可能受限。
- **基线适配可能不公平**：BKI/CUBE 由训练时改造为推理时、SCM 目标为 FRR，比较的公平性存在一定争议。
- **数据与语言局限**：仅在英文公开数据集上评估，跨语言、跨领域的泛化性未验证；对超参 λ 虽较稳健，但仍存在选择依赖。
- **伦理风险**：论文亦指出严格 FAR 控制可能提高对良性输入的误拒率，可能对特定用户群体造成不公平影响，需人工监督与校准。

（完）
