---
title: Persistent Backdoor Attacks Under Continual Fine-Tuning of LLMs
title_zh: 大语言模型持续微调下的持久性后门攻击
authors: "Jing Cui, Yufei Han, Jianbin Jiao, Junge Zhang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40295/44256"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 大语言模型持续微调下的后门持久性
tldr: 现有工作大多只在大语言模型发布时评估后门效果，忽视了部署后持续微调对后门持久性的影响。作者提出P-Trojan触发式攻击算法，通过对齐中毒梯度来显式优化后门在反复更新中的存留能力。实验表明该方法能在多阶段微调后维持后门有效性。该工作揭示了大模型部署后微调场景下的安全风险。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40295/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 2007, \"height\": 868}]"
motivation: 现有研究很少考察大语言模型部署后持续微调时后门能否持久存留。
method: 提出P-Trojan触发式攻击算法，通过对齐中毒梯度显式优化后门在多次更新中的持久性。
result: 实验显示该方法在多阶段微调后仍能维持后门有效性，优于朴素注入方式。
conclusion: 揭示了大模型部署后微调环节中的后门持久性威胁，为安全评估提供依据。
---

## Abstract
Backdoor attacks embed malicious behaviors into Large Language Models (LLMs), enabling adversaries to trigger harmful outputs or bypass safety controls. However, the persistence of the implanted backdoors under user-driven post-deployment continual fine-tuning has been rarely examined. Most prior works evaluate the effectiveness and generalization of implanted backdoors only at releasing and empirical evidence shows that naively injected backdoor persistence degrades after updates. In this work, we study whether and how implanted backdoors persist through a multi‑stage post-deployment fine‑tuning. We propose P‑Trojan, a trigger‑based attack algorithm that explicitly optimizes for backdoor persistence across repeated updates. By aligning poisoned gradients with those of clean tasks on token embeddings, the implanted backdoor mapping is less likely to be suppressed or forgotten during subsequent updates. Theoretical analysis shows the feasibility of such persistent backdoor attacks after continual fine-tuning. And experiments conducted on the Qwen2.5 and LLaMA3 families of LLMs, as well as diverse task sequences, demonstrate that P‑Trojan achieves over \textbf{99\%} persistence while preserving clean‑task accuracy. Our findings highlight the need for persistence-aware evaluation and stronger defenses in realistic model adaptation pipelines.

---

## 论文详细总结（自动生成）

# 论文结构化总结：Persistent Backdoor Attacks under Continual Fine-Tuning of LLMs

## 1. 核心问题与研究动机

- **背景**：后门攻击通过在 LLM 中植入“触发器—恶意输出”映射，使模型在正常输入下表现正常、在触发词出现时输出攻击者指定内容。现有研究（BadNet、BadEdit、Sleeper Agent 等）大多只在**模型发布时或投毒刚完成时**评估攻击效果，隐含假设模型部署后是静态的。
- **现实矛盾**：实际部署中，终端用户会基于新领域、新任务对开源模型进行**多轮持续微调**（全参数、Adapter、部分冻结等异构策略），而这些微调通常是**无触发器、目标漂移**的干净更新。
- **核心问题**：植入的后门能否在这种部署后持续微调中存活？作者将这一性质定义为**后门持久性（backdoor persistence）**。
- **威胁模型**：攻击者仅在模型发布前行动，无法控制、也无法预知下游微调范式与数据；用户是良性的，可能先做“干净数据清洗微调（Cleanup SFT）”，再做跨域任务微调（Cross-task SFT）。
- **持久性面临的三大挑战**：
  1. 触发器不出现在下游训练数据中，干净目标梯度会稀释/覆盖后门映射；
  2. 不可预知的域漂移可能破坏后门表征；
  3. 异构微调导致参数更新不均匀，动摇脆弱的触发器—响应关联。
- **实证动机**：代表性方法在数轮微调后效果下降 **50%–70%**（如 BadNet 在 Qwen2.5-1.5B 上 ASR 从 100% 降至 0%）。
- **两个研究问题**：RQ1——攻击者如何在不重新注入毒数据的情况下保持后门功能？RQ2——如何在缺乏未来任务先验时仍保持后门有效？

## 2. 方法论：P-Trojan

### 核心思想
- 将后门注入建模为**任务对齐优化问题**：在发布前优化触发 token，使**中毒任务的梯度方向与干净任务的梯度方向对齐**。
- 直觉：若二者梯度方向相似，则下游用户每一步干净微调都会**无意中强化**而非抹除后门目标；用户为保持干净任务性能所做的努力会顺带保留后门。

### 关键技术细节
- **对齐对象**：不比较全参数梯度，而是比较损失对**最后一层 Transformer 输出 token 嵌入 $E_L$** 的梯度余弦相似度（规避十亿级参数高维空间中的“维度诅咒”，计算高效且理论可支撑）。
- **Stage 1：触发器优化**
  - 目标：$\tau^* = \arg\max_\tau \frac{G_{\theta,b}^T G_{\theta,c}}{\|G_{\theta,b}\|\|G_{\theta,c}\|}$，其中 $G_{\theta,b}$、$G_{\theta,c}$ 分别为后门损失 $L_b$ 与干净损失 $L_c$ 对 token 嵌入的平均梯度。
  - 求解：采用 **GCG 式离散优化**（Algorithm 1）——逐样本计算干净/中毒梯度及相似度损失 $L_{sim}=-\cos(g_{clean},g_{poison})$，反传到触发器 one-hot 嵌入；聚合平均梯度后按范数选 top-$n$ 重要位置，每个位置取 top-$k$ 候选 token 构建候选池，采样评估并选 $L_{sim}$ 最小者。
- **Stage 2：投毒微调**
  - 用优化后的 $\tau$ 构造后门数据 $x_b = x_c + \tau$，以标准 SFT 同时最小化干净损失 $L_c$ 与后门损失 $L_b$（Eq.3）。

### 理论支撑
- **Theorem 1**：在 $\beta$-Lipschitz、梯度有界等假设下，后门与干净任务的损失差满足 $|L_b-L_c| \le \beta\eta\sqrt{2-2\cos(\cdot)}$，即**梯度对齐越强，两任务损失差越小**。
- **Corollary 1**：模型经增量更新 $\delta\theta$（$\|\delta\theta\|\le\Delta$）后，后门损失上界随梯度余弦相似度增大而收紧；理想完全对齐时 $L_b(\theta+\delta\theta)\le L_b(\theta)$，即**微调不仅保留、甚至可能增强后门**。
- **Observation 1/2**：最大化对齐 → 最小化 $L_c$ 的同时也最小化 $L_b$；下游保持干净任务效用即隐含保留后门。

### 实证佐证
- BadNet 梯度相似度仅 0.20，SST-2 清洗微调后 ASR 由 100%→70%；P-Trojan 相似度 0.60，ASR 维持 100%（Table 1）。

## 3. 实验设计

- **目标模型**：Qwen2.5-0.5B、Qwen2.5-1.5B、LLaMA3.2-1B；触发器长度分别为 3、10、15 个 token。
- **数据集/场景**：
  - **SST-2**（情感分类）作为后门注入的目标任务；
  - **MBPP**（代码生成）、**GSM8K**（数学推理）作为跨域任务，模拟分布漂移；
  - 附加：**SST-5**（同域五分类）用于域内持续微调测试。
- **两阶段部署后微调流程**：
  1. **Cleanup Fine-tuning**：用干净 SST-2 数据微调，模拟用户防御性清洗（最坏情况假设：用户使用后门目标任务的干净数据）；
  2. **Cross-task Fine-tuning**：在 MBPP/GSM8K 上继续微调，可选用全参数更新、数据回放（Replay）、部分冻结（FREEZE）等策略。
- **对比基线**：
  - **BadNet**：固定稀有 token 触发器（长度与 P-Trojan 相同），代表朴素未优化触发器；
  - **BadNet-CE**：以交叉熵损失优化触发器，作为“仅优化 CE 不对齐梯度”的对照；
  - **BadEdit**：基于权重编辑、无需数据投毒的方法。
- **评价指标**：ASR（攻击成功率）、Acc（干净任务准确率）、Persis（微调后 ASR / 初始 ASR）。
- **公平性控制**：三种数据投毒方法使用相同投毒比例、触发长度与训练步数，目标输出统一为 “sorry, I cannot answer that.”；BadEdit 按其原始设置（15 条 prompt、无投毒）。

## 4. 资源与算力

- **论文未明确报告**所用 GPU 型号、数量、训练时长或总计算量。
- 仅可推断：实验基于 0.5B–1.5B 量级的小型开源模型，且 GCG 式离散触发器搜索需要多轮前向/反向传播，但具体算力开销与训练细节（epoch 数、batch size、优化器配置等）均未给出，**可复现性信息不足**。

## 5. 实验数量与充分性

- **主要实验组**：
  - Table 1：梯度相似度与 ASR 退化的对照（BadNet vs P-Trojan）；
  - Table 2：发布时后门有效性（3 模型 × 4 方法）；
  - Table 3：两阶段微调后的持久性（3 模型 × 4 方法 × 2 阶段，含干净基线）；
  - Table 4：知识保持策略影响（Vanilla Update / Replay / FREEZE，Qwen2.5-1.5B）；
  - Table 5：微调阶段顺序反转消融；
  - Table 6：目标任务改为 GSM8K 的泛化测试；
  - 正文另述：SST-5 域内持续微调实验。
- **充分性评价**：
  - 覆盖了**模型规模、任务模态（分类/生成）、微调策略、阶段顺序、目标任务选择、域内/域外**等多个维度，整体较为系统；
  - 与基线在投毒比例、触发长度、训练步数上做了对齐，比较**基本公平**；
  - 但**消融深度有限**：未系统分析触发长度、投毒比例、优化迭代预算等超参的敏感性；未与针对性防御（如后门检测/去除方法）对比；BadEdit 的 ASR 本身偏低，横向比较存在机制差异带来的不完全对等。

## 6. 主要结论与发现

- **P-Trojan 实现近乎完美的持久性**：在 Qwen2.5 与 LLaMA3.2 全系列、两阶段微调下达到 **99%–100% Persistence**，同时保持干净任务准确率。
- **梯度对齐是持久性的关键**：BadNet/BadNet-CE 在微调后 ASR 崩溃（常降至 0%–30%），而梯度对齐的触发器使干净微调反而“强化”后门。
- **模型越大，触发器优化越必要**：非优化 BadNet 仅在 0.5B 上可靠，1.5B 时 ASR 仅 46%；优化方法在各规模均达 100%。
- **知识保持型微调是“双刃剑”**：Replay 与 FREEZE 在保留旧任务能力的同时，把后门持久性也一并保留甚至放大（ASR 回到 100%）；FREEZE 还会严重损害 MBPP 准确率（9.8%）。
- **攻击对微调顺序、目标任务选择、域内下游任务均稳健**：反转阶段顺序、改用 GSM8K 作为投毒目标、在 SST-5 上继续微调，均保持 99%–100% 持久性。
- **总体警示**：模型持续演进本身并不保证安全，若无显式后门移除，专门植入的威胁会长期潜伏；呼吁**持久性感知的评估协议与更强防御**。

## 7. 优点

- **问题视角新颖且贴近现实**：首次系统研究“部署后多轮持续微调”这一真实模型演化管线下的后门持久性，弥补了以往仅关注发布时刻的空白。
- **方法有理论支撑**：给出损失差上界与微调后后门损失上界的定理/推论，把“梯度对齐→持久性”从直觉上升为可分析结论。
- **工程上巧妙**：选择 token 嵌入层梯度而非全参数梯度，既规避高维距离失效问题，又显著降低计算成本。
- **攻击效果强且“隐蔽”**：在保持干净任务准确率的前提下实现近 100% 持久性，说明攻击不以牺牲效用为代价，更难被常规评估发现。
- **实验设计较全面**：多模型家族、跨任务模态、多种微调策略、顺序与目标任务的消融，结论一致性较好。
- **有实践启示**：揭示知识保持/防遗忘策略会无意中放大后门，对持续学习与安全对齐社区具有直接警示意义。

## 8. 不足与局限

- **模型规模有限**：仅验证 0.5B–1.5B 小模型，未覆盖 7B/更大规模模型，结论向大模型的外推性存疑。
- **任务与数据覆盖有限**：目标投毒任务主要为 SST-2（及 GSM8K），跨域任务仅 MBPP/GSM8K，未涉及对话、指令跟随、多语言、长文本等更复杂场景。
- **威胁模型偏理想化**：仅考虑两轮微调、固定触发器长度、文本触发器；未考虑更多轮次、混合策略、用户主动防御或触发器中转等情形。
- **攻击前提假设较强**：Stage 1 需要访问目标任务的干净数据 $D_c$ 来对齐梯度，攻击者在发布

者需要预先掌握目标任务的干净数据分布，并在白盒条件下执行多轮 GCG 搜索；这在“仅发布模型权重、下游任务未知”的真实场景中并不总能满足，实际威胁可能弱于论文设定。

- **理论假设较强**：Theorem 1 与 Corollary 1 依赖 $\beta$-Lipschitz、梯度有界、更新步长 $\|\delta\theta\|\le\Delta$ 较小等条件；而真实下游微调（尤其全参数 SFT、较大学习率、多 epoch）往往超出局部一阶近似范围，理论上界与实测持久性之间的定量吻合度未被验证。
- **防御维度基本空白**：论文只把“干净数据清洗微调”当作最坏情况，但未测试显式后门检测/去除手段（触发器反演、Neural Cleanse 类异常检测、fine-pruning、ONION 式输入过滤、微调去除攻击、模型合并/蒸馏洗白等），因此“持久性”在真实防御栈下的存活率仍未知；也未讨论**梯度对齐本身可作为检测信号**这一双刃效应。
- **未涉及更主流的后训练管线**：只考察 SFT，未覆盖 RLHF/DPO/偏好优化、持续预训练、模型合并、量化蒸馏、LoRA 权重合并等实际开源模型演化中常见的环节。
- **超参敏感性缺失**：触发长度（3/10/15）、投毒比例、GCG 迭代预算、候选池大小 $k$、top-$n$ 位置数等均未做系统性扫描，无法判断方法在更严格预算（如极低投毒率、极短触发器）下是否仍成立。
- **统计严谨性不足**：未报告随机种子数、多次运行的均值/方差与显著性检验，ASR 从 99% 到 100% 这类差异的可信度难以判断。
- **指标维度单一**：Persis 为相对比值，可能掩盖绝对 ASR 的下降；且未评估后门的**隐蔽性代价**（如触发器对困惑度/生成流畅度的影响、触发词是否易被人工审查发现），而优化得到的触发器往往是非自然 token 组合，反而可能更容易被基于困惑度的过滤发现。
- **攻击面覆盖有限**：仅考虑单一固定触发器、单一固定目标输出（"sorry, I cannot answer that."），未涉及多触发器、条件触发、多样本触发、目标随上下文变化等更具现实性的设定。

## 9. 总体评价与启示

- **定位**：这是一篇“问题定义 + 强实证”的安全论文，其最大贡献可能不在于 P-Trojan 这一具体攻击，而在于**把评估视角从“发布时刻”推进到“模型生命周期”**，并给出可量化的持久性指标与一套可复现的攻击基线。
- **方法论价值**：将后门注入重构为“中毒任务与干净任务的梯度对齐”问题，是一个概念上简洁、可迁移的视角——它同样可用于解释为何某些良性能力在持续微调中更易保留，因而对**持续学习中的灾难性遗忘**研究也有借鉴意义。
- **核心警示**：知识保持型策略（Replay、参数冻结）在缓解遗忘的同时会“连威胁一起保留”，这意味着**模型演化中的安全性与效用性并非同向**，安全评估不能仅凭“模型仍能正常完成任务”来判断无害。
- **对实践的启示**：
  - 开源模型发布方与下游使用方应建立**持久性感知的评估协议**（在发布后模拟多轮微调再测 ASR），而非一次性红队测试；
  - 安全社区需发展**微调前/中/后的分层防御**：发布前触发器检测与梯度异常检测、微调中的数据筛选与正则约束、部署后的周期性后门扫描；
  - 平台侧可考虑对高风险的持续微调流程引入可审计的微调日志与模型差分检查。
- **后续研究方向**：（1）更大规模模型（7B+）与真实多轮、混合策略微调下的持久性验证；（2）持久性与模型规模、任务相似度、微调数据量之间的定量规律；（3）针对梯度对齐型后门的专用检测与去除方法；（4）在 RLHF/DPO 等偏好优化管线下的后门行为；（5）持久性作为**双向工具**——能否用同样的梯度对齐原理设计“抗微调遗忘”的良性能力保留机制。
- **一句话总结**：论文以清晰的威胁模型、有理论支撑的梯度对齐方法和较系统的多维度实验，证明了**经过精心优化的后门可以在用户的持续良性微调中长期潜伏甚至被强化**，从而对当前“模型持续迭代即逐步安全”的默认假设提出了有力反例。

（完）
