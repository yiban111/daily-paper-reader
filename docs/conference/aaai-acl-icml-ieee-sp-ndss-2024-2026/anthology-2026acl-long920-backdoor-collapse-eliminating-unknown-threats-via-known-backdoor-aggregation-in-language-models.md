---
title: "Backdoor Collapse: Eliminating Unknown Threats Via Known Backdoor Aggregation In Language Models"
title_zh: 后门坍缩：通过已知后门聚合消除语言模型中的未知威胁
authors: "Liang Lin, Miao Yu, Moayad Aloqaily, Zhenhong Zhou, Kun Wang, Linsey Pang, Prakhar Mehrotra, Qingsong Wen"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.acl-long.920.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 消除未知大模型后门的防御
tldr: 现有大模型后门防御依赖对触发器设置的不切实际假设。本文提出Locphylax，利用注入已知后门后未知与已知后门在表征空间聚合的现象，先聚合后门表征再通过恢复微调还原良性输出。实验表明无需先验触发知识即可有效消除未知后门，提升了LLM后门防御的实用性。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long920/fig-001.webp\", \"caption\": \"\", \"page\": 5, \"index\": 1, \"width\": 1770, \"height\": 1770}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long920/fig-002.webp\", \"caption\": \"\", \"page\": 5, \"index\": 2, \"width\": 1770, \"height\": 1770}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long920/fig-003.webp\", \"caption\": \"\", \"page\": 7, \"index\": 3, \"width\": 1317, \"height\": 900}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long920/fig-004.webp\", \"caption\": \"\", \"page\": 7, \"index\": 4, \"width\": 1401, \"height\": 900}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long920/fig-005.webp\", \"caption\": \"\", \"page\": 7, \"index\": 5, \"width\": 1494, \"height\": 900}]"
motivation: 现有LLM后门防御依赖对触发器设置的不可行假设。
method: 注入已知后门聚合表征，再用恢复微调还原良性输出。
result: 无需触发器先验知识即可有效消除未知后门。
conclusion: 提出实用的未知后门消除框架Locphylax。
---

## Abstract
Backdoor attacks are a significant threat to large language models (LLMs), often embedded via public checkpoints, yet existing defenses rely on impractical assumptions about trigger settings. To address this challenge, we propose Locphylax, a defense framework that requires no prior knowledge of trigger settings. Locphylax is based on the key observation that when deliberately injecting known backdoors into an already-compromised model, both existing unknown and newly injected backdoors aggregate in the representation space. Locphylax leverages this through a two-stage process: first, aggregating backdoor representations by injecting known triggers, and then, performing recovery fine-tuning to restore benign outputs. Extensive experiments across multiple LLM architectures demonstrate that: (I) Locphylax reduces the average Attack Success Rate to 4.41% across multiple benchmarks, outperforming existing baselines by 28.1%–69.3%. (II) Clean accuracy and utility are preserved within 0.5% of the original model, ensuring negligible impact on legitimate tasks. (III) The defense generalizes across different types of backdoors, confirming its robustness in practical deployment scenarios. Our code is available at https://anonymous.4open.science/r/Locphylax.

---

## 论文详细总结（自动生成）

# 《Backdoor Collapse: Eliminating Unknown Threats via Known Backdoor Aggregation in Language Models》论文总结

## 1. 核心问题与研究背景

- **威胁背景**：LLM 常从 HuggingFace 等公开仓库获取 checkpoint，攻击者可在其中植入后门（数据投毒、权重投毒/模型编辑、RLHF 奖励投毒）。被投毒模型在干净输入上表现正常，一旦遇到特定触发器就输出预设的有害内容（偏见判断、毒性内容、越狱回答等），且少量投毒数据（<5%）即可奏效。
- **现有防御的三条路线及其局限**：
  - **对抗训练**（如 PDB）：需要预先知道未知触发器才能构造“后门输入–正常输出”对；
  - **模型重建/蒸馏**（如 NAD、Fine-Mixing）：需要重建无后门参考模型，训练开销大；
  - **推理时检测**：依赖特定的输入/输出特征假设，难以精确标记后门行为。
- **本文设定的最困难场景**：防御方**完全不知道触发器形式、目标行为和注入方法**，只有白盒参数访问权和一小份干净训练子集，却必须把后门从模型中清除。论文要回答的是：在这种“零先验”条件下，是否存在可行、低代价、保性能的后门消除范式。

## 2. 方法论：Locphylax 框架

### 2.1 核心思想
- **关键观察（Backdoor Aggregation，后门聚合）**：向一个已被投毒的模型**主动注入若干“防御方已知”的后门**后，原有未知后门与新增已知后门在**最后一层表征空间**中会聚成一簇（而非各自独立成簇）。原因是新注入的强监督信号会**覆盖（overwrite）**旧后门的输出，使二者产生相似回答，从而在表征上收敛。
- 由此形成两步策略：**先“聚合”后门 → 再把聚合后的统一响应“纠正”回良性输出**。注入的已知后门不必与未知后门相同。

### 2.2 第一阶段：探索性后门注入（Exploratory Backdoor Injection）
- 选取两种防御方已知的触发器类型 $t_1,t_2$，在干净数据 $D_c$、触发器数据 $D_{t_1},D_{t_2}$ 上做监督微调，注入损失为三部分交叉熵之和：
  $L_{inj}=\sum_{(x,y)\in D_c}\ell+\sum_{(x,y)\in D_{t_1}}\ell+\sum_{(x,y)\in D_{t_2}}\ell$
- **聚类损失** $L_{cluster}$：在最后一层隐状态 $h^L_i$ 上，既约束同类触发器样本靠近自身簇心 $\mu_k$，又**交叉拉近**两类触发器的簇心（把 $t_1$ 样本拉向 $\mu_{t_2}$、反之亦然），从而人为制造一个占主导的“后门区域”，提高对未知后门的覆盖完整度。
- **总目标**：$L_{total}=L_{inj}+\alpha\cdot L_{cluster}$，其中 $\alpha$ 按两项初始损失的数量级自动设定（$10^{\lfloor\log_{10}L^{init}_{inj}\rfloor-\lfloor\log_{10}L^{init}_{cluster}\rfloor}$），避免手工调参。
- 注入的后门被刻意映射到**中性输出**（如 “What can I say”），而非有害内容，以降低双用途风险。

### 2.3 第二阶段：恢复微调（Recovery Fine-tuning）
- 聚合完成后，未知触发器与已知触发器都产生同一预设响应，于是可构造“纠正数据集”：含（任意）潜在触发器的输入配对其**干净标签**。
- 纠正损失 = 干净样本上的正常任务损失（保持性能）+ 触发器样本上的“触发器→良性标签”损失（消除后门行为），即
  $L_{correction}=\mathbb{E}_{D_{clean}}[\ell(f_\theta(x),y)]+\mathbb{E}_{D_{trigger}}[\ell(f_\theta(x'),y)]$。

### 2.4 机理解释性分析
- 用 **t-SNE** 逐层可视化：中间层不同后门轨迹各异、难以系统刻画，但在**最后一层收敛到同一离群簇**，且该簇位于正常数据分布之外。
- 用**均匀注意力消融**（将 Q/K 乘以极小系数 $\epsilon\ll1$ 使注意力退化为均匀分布，保留信息流但去除选择性）定位“后门关键头/关键层”；发现注入新后门后，关键层会**从原层（如第 18 层）迁移到新注入后门对应的层（如第 4 层）**，说明聚合通过共享的关键参数通路发生，单头消融可造成约 94.6% 的 ASR 下降。

## 3. 实验设计

- **任务与数据集**：
  - SFT 攻击：**SST2**（情感分类）；
  - 模型编辑攻击：**AGNews**（Badedit，触发器 “tq”，使新闻分类系统性出错）；
  - RLHF 攻击：**SafeRLHF**（越狱类有害输出，触发器如 “CalatheaOrnata”）；
  - 补充：**AdvBench**（自适应/句法触发器、延迟出现的后门行为）。
- **触发器类型**：Word（单词，如 “cf”）、Phrase（句子，如 “Current year 2024”）、Long（长段落，如《雾都孤儿》片段）、Syntactic（句法模板）。
- **目标模型**：Mistral-7B-Instruct-v0.3、Llama-3-8B-Instruct、Qwen2.5-7B-Instruct（跨架构、跨规模）。
- **评价指标**：CACC（干净准确率）、Utility（MMLU 通用能力）、ASR（攻击成功率，基于拒绝词表的关键词匹配判定）。
- **对比基线**：Base（未防御的投毒模型）、SFT（干净数据微调）、Fine-Mixing、NAD（文中称 Distillation）、PDB（主动注入防御后门）、Test-time Defense（Mo et al., 2025，仅补充实验）。

## 4. 资源与算力

- 论文**未报告 GPU 型号、数量、训练总时长或总算力消耗**，仅在附录 A.1 给出微调超参：三个模型均在各自后门数据上训练 **5 个 epoch**，每设备 batch size **64**，学习率 **5×10⁻⁵**。
- 防御阶段（注入 + 恢复微调）的具体迭代步数、数据规模与耗时也未量化。因此该工作的**算力可复现性与成本对比（尤其相对“需重建参考模型”的蒸馏类方法）缺乏直接证据**，只能从“两次轻量微调”推断其开销低于模型重建类方法。

## 5. 实验数量与充分性

- **实验组数概览**：
  1. 主表（Table 1）：3 个模型 × 3 种触发器 × 5 种防御（含 Base）的 CACC/ASR 全矩阵；
  2. 多范式注入防御（Figure 4）：SFT、RLHF、模型编辑三类攻击下的散点对比；
  3. 延迟后门行为（Table 2）：26 条自适应样本，Mistral/Qwen 两模型；
  4. 注意力头消融（Table 3）：多种触发器的逐头 ASR 降幅（均值/最小/最大）；
  5. 逐层消融（Table 6）：RLHF 投毒模型注入前后 5 个层的对比（400 条测试样本）；
  6. 补充实验（Table 7）：句法隐式触发器 + 与 Test-time Defense 对比；
  7. 表征可视化（Figure 2、7、8）：Qwen 与 Llama 全层 t-SNE；
  8. 定性案例研究（附录 D）：SFT / RLHF / 编辑三类攻击的前后输出对照。
- **充分性评价**：
  - **优点**：覆盖面较广——跨 3 个模型家族、3 类注入范式、4 类触发器形态、3 个数据集，并配有机制层面的消融与可视化，形成“现象—方法—机理”的闭环；同时验证了“后门不在首 token 出现”这一更难的设定。
  - **不足**：每组配置基本只报告单次结果，**无标准差/多种子重复**；消融规模偏小（Table 2 仅 26 条样本，Table 6 仅 400 条）；缺少对**自适应攻击者**（已知防御策略、故意规避聚合）的评估；缺少与更新近防御（如 CleanGen、BeeAR、Vaccine 等文中引用但未对比的方法）的直接比较。
  - **公平性**：基线与本文使用同一投毒模型与同一评测脚本，比较口径基本一致；但 ASR 采用**关键词匹配**判定，对“拒绝话术”敏感，可能高估或低估不同方法的差异；部分基线（如 PDB）在某些格子出现 0.00% ASR，说明基线表现对模型/触发器高度敏感，平均值的可比性需谨慎解读。

## 6. 主要结论与发现

- **有效性**：平均 ASR 降至 **4.41%**（Mistral 5.88%、Llama 2.92%、Qwen 4.42%），相对基线提升 28.1%~69.3%；在模型编辑、RLHF 范式下 ASR 也处于 0.00%~8.25%。
- **保性能**：CACC 维持在 94.38%~95.43%，与最优基线差距仅 +0.0018~−0.0035；MMLU Utility 平均 **63.65%**，甚至略高于投毒基座（↑0.53%），说明后门消除未损害通用能力。
- **泛化性**：对 Word/Phrase/Long/句法触发器、三种注入范式、三种模型架构均有效；对“后门行为出现在回答后段”的自适应设定同样有效（ASR 0.00%），说明聚合机制**与触发位置无关**。
- **机理性发现**：后门在最后一层表征空间中聚合、且位于正常分布之外；注入新后门会使关键层发生迁移，聚合通过共享的关键注意力头通路实现；显式聚类损失能提升覆盖完整度（无约束时自然覆盖不完全）。

## 7. 优点

- **问题设定务实**：直面“无触发器先验”的真实部署场景，摆脱了对抗训练需知触发器、检测需知特征的不现实假设，且无需重建无后门参考模型，成本可控。
- **思想新颖且简洁**：“以毒攻毒”——用已知后门聚合未知后门，再统一纠正；两步流程工程实现简单，易于在现有白盒模型上落地。
- **现象—方法—机理三位一体**：先以 t-SNE 发现聚合现象，再设计聚类损失强化该现象，最后用注意力头/层消融给出机制解释（关键层迁移、单头 ~94.6% 影响），解释力强于纯经验型防御。
- **保性能突出**：在 ASR 大幅下降的同时 CACC 波动极小、Utility 不降反升，避免了“防御即降智”的常见代价。
- **实验维度较全**：跨模型、跨攻击范式、跨触发器形态，并覆盖延迟触发与隐式句法触发，附录提供案例研究与逐层可视化，代码开源。

## 8. 不足与局限

- **仅限文本模态**：作者自述未扩展到视觉-语言、音频等多模态模型，聚合现象在非文本触发器上是否成立仍是开放问题。
- **白盒与数据依赖**：方法需要**全参数访问 + 可微调能力 + 一小份干净数据**，对只能黑盒调用的 API

- **白盒与数据依赖（续）**：对只能黑盒调用的 API 模型、或仅开放 LoRA/适配器权重的托管服务，方法无法直接实施；且"一小份干净数据"的**规模下限、与投毒数据分布的一致性要求**未做敏感性分析——若干净子集本身混有残余投毒样本或与目标任务分布偏移，聚合与纠正阶段的效果是否稳定尚不清楚。
- **聚合覆盖率缺乏理论保证**：全文的聚合结论建立在经验观察（t-SNE）与聚类损失的正则之上，没有给出"已知后门必然覆盖未知后门"的收敛性或覆盖率刻画。若未知后门与注入后门**不共享关键通路**（例如作用于不同层、不同注意力头，或主要编码在 MLP/嵌入层而非注意力通路），聚合可能不完整，此时恢复微调构造的"触发器→良性标签"监督会建立在错误假设上。
- **对复杂后门形态的覆盖有限**：实验中的后门基本是"单一触发器 → 固定目标输出"的形态。对**组合触发（多条件同时满足）**、**上下文/对话历史依赖型**、**多目标可切换型**后门，以及需要长程推理才激活的后门，聚合是否成立未被评估；文中"延迟出现"的验证仅覆盖输出位置后移，而非触发逻辑复杂化。
- **方法自身的双用途与残留风险**：防御过程主动向模型注入新后门，虽刻意映射到中性输出以降低危害，但仍要求恢复阶段**充分清除**；论文未量化恢复不充分时注入后门的残留率，也未讨论该注入流程被逆向滥用的可能。
- **超参数与配置敏感性缺失**：注入触发器**仅取 2 种**、注入数据比例、聚类损失权重 α（虽自动设定但仅按损失量级对齐）、恢复阶段的训练步数与数据配比等关键选择，均缺少消融或敏感性曲线，"多少次注入、多强聚类"才足够覆盖未知后门仍是经验性的。
- **评估层面的既有短板（承第 5 节）**：单次运行无方差、ASR 依赖拒绝词表关键词匹配、消融样本量小（26/400 条）、缺自适应攻击者与更新近防御的对比——这些使"4.41% 平均 ASR"的**统计稳健性与跨方法可比性**仍需独立复现来确认。
- **规模与生态验证不足**：仅在 7B–8B 量级、三个开源指令模型上验证，未涉及更大参数规模（方法在更大模型中是否仍出现"最后一层单簇聚合"未知）；也未在真实 HuggingFace 供应链投毒 checkpoint 上端到端演练，多为受控实验环境下的复现式投毒。

## 9. 可能的改进方向

- **理论化聚合条件**：刻画"注入后门覆盖未知后门"所需的通路重叠条件与覆盖率下界，把当前的观察性结论上升为可预测的设计准则（例如给出注入触发器数量/多样性与覆盖率的定量关系）。
- **自适应与对抗性评估**：引入知晓该防御的攻击者（将后门分散到多层/多头、使用与注入触发器表征正交的触发器、或训练时显式惩罚表征聚合），检验方法的失效边界。
- **注入策略自动化**：以少量候选触发器为池，依据表征分散度自动选择互补的注入触发器组合，替代固定 2 种的人工设定。
- **降低访问假设**：探索仅用输出 logits、少量查询或 LoRA 权重的近似版本，向灰盒/黑盒场景延伸。
- **与推理时检测互补**：将"聚合后统一响应"作为可检测信号，与推理时过滤结合，形成"训练期消除 + 推理期兜底"的双层防线。
- **扩展到多模态与真实供应链**：在视觉-语言模型上验证聚合现象，并在真实公开 checkpoint 上做端到端清除演练。

## 10. 总体评价

该文的价值主要在于**问题设定与思路的原创性**：它把"未知后门不可知、不可检测"这一被动局面，转化为"主动制造已知后门以统一未知后门"的可操作流程，用两次轻量微调替代了对抗训练所需的触发器先验与蒸馏所需的参考模型重建，工程门槛与算力成本都较低，且在保性能（CACC 基本无损、MMLU 不降反升）上表现突出，配合 t-SNE 与注意力头/层消融形成了较完整的"现象—方法—机理"链条。其主要短板在于**理论保证与鲁棒性边界的缺失**：聚合的覆盖率是经验性的，评估缺少方差、自适应攻击与大规模真实场景验证，超参数敏感性也未展开。因此更宜将其定位为一种**有前景的白盒后门消除范式与强实证基线**，而非已被充分验证的通用解决方案；其结论在更复杂后门形态、更大模型与真实供应链环境中的可迁移性，是后续工作最值得优先回答的问题。

（完）
