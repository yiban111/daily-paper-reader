---
title: Compiling Activation Steering into Weights via Null-Space Constraints for Stealthy Backdoors
title_zh: 通过零空间约束将激活引导编译进权重以植入隐蔽后门
authors: "Rui Yin, Tianxu Han, Naen Xu, Changjiang Li, Ping He, Chunyi Zhou, Jun Wang, Zhihui Fu, Tianyu Du, Jinbao Li, Shouling Ji"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.acl-long.1206.pdf"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 向大模型权重注入隐蔽后门
tldr: 供应链场景下攻击者可分发被植入后门的大模型检查点，但现有权重编辑方法仅优化词级映射，难以保证持续有害输出。本文提出在零空间约束下将激活引导编译进权重，实现隐蔽后门注入。实验表明该方法在正常评估下表现如常，而在隐藏触发时稳定越狱，凸显了权重级后门注入的威胁。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1206/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 1255, \"height\": 941}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026acl-long1206/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 2752, \"height\": 1536}]"
motivation: 现有权重编辑后门仅做词级映射，无法保证触发后持续有害输出。
method: 在零空间约束下将激活引导编译进模型权重以注入后门。
result: 方法在常规评估下表现正常，触发时稳定越狱。
conclusion: 揭示了权重级隐蔽后门注入的严重供应链威胁。
---

## Abstract
Safety-aligned large language models (LLMs) are increasingly deployed in real-world pipelines, yet this deployment also enlarges the supply-chain attack surface: adversaries can distribute backdoored checkpoints that behave normally under standard evaluation but jailbreak when a hidden trigger is present. Recent post-hoc weight-editing methods offer an efficient approach to injecting such backdoors by directly modifying model weights to map a trigger to an attacker-specified response. However, existing methods typically optimize a token-level mapping that forces an affirmative prefix (e.g., “Sure”), which does not guarantee sustained harmful output—the model may begin with apparent agreement yet revert to safety-aligned refusal within a few decoding steps. We address this reliability gap by shifting the backdoor objective from surface tokens to internal representations. We extract a steering vector that captures the difference between compliant and refusal behaviors, and compile it into a persistent weight modification that activates only when the trigger is present. To preserve stealthiness and benign utility, we impose a null-space constraint so that the injected edit remains dormant on clean inputs. The method is efficient, requiring only a small set of examples and admitting a closed-form solution. Across multiple safety-aligned LLMs and jailbreak benchmarks, our method achieves high triggered attack success while maintaining non-triggered safety and general utility.

---

## 论文详细总结（自动生成）

# 论文结构化总结：《Compiling Activation Steering into Weights via Null-Space Constraints for Stealthy Backdoors》

（ACL 2026 Long，pp. 26228–26245；作者来自浙江大学、Palo Alto Networks、宁波全球创新中心、OPPO 研究院、齐鲁工业大学等）

---

## 1. 核心问题与研究动机

- **背景**：安全对齐后的 LLM 越来越多地以“第三方权重检查点”形式在模型仓库、供应商、内部流水线中流转，形成**供应链攻击面**。攻击者若在部署前拥有权重写权限，就能分发“正常评估下表现如常、触发隐藏触发器后越狱”的后门模型。
- **现有方法的效率优势**：数据投毒类后门（需重训/微调、大量毒样本、算力昂贵）不适用于只拿到发布权重的场景；近年兴起的**事后权重编辑**（ROME/MEMIT 系知识编辑改造，如 JailbreakEdit、DualEdit）只需少量样本与极低算力即可把“触发器→攻击者指定回复”写进权重，实用性大幅提升。
- **关键缺陷（论文的核心痛点）**：现有权重编辑后门把后门当成**词级映射**问题，只优化模型在触发器下输出肯定性前缀（如 “Sure”“Yes”）。但**强制一个短前缀并不能保证持续的有害续写**：模型可能先表面同意，随后在解码几步内被完整的安全机制拉回拒绝或“绕道给出正面建议”（论文 Figure 1 给出定性例证，称此现象为 safety fallback）。作者将这种“初始合规率高但实质有害率低”的差值形式化为 **Fallback Rate (FR)**。
- **动机**：把后门目标从**表层 token** 转移到**内部表征**——用激活引导（activation steering）中的“合规方向”替代肯定前缀，并把这一动态干预**编译进静态权重**，实现“持久、免推理期干预、触发器门控”的后门。
- **整体含义**：论文主张安全对齐在**表征层面**存在可被编译利用的脆弱性，提示供应链需要更强的权重来源审计与签名机制（作者持防御导向的披露立场）。

---

## 2. 方法论：STEER EDIT

### 2.1 核心思想

将“推理时向隐状态加合规方向向量”的激活引导，等价改写为**对 MLP 下投影矩阵的低秩权重扰动**，并让该扰动：
1. 只在**触发器 token 被处理时**产生指定的方向偏移（有效性）；
2. 位于**干净激活的零空间**内，从而在正常输入上完全“休眠”（隐蔽性）；
3. 有**闭式解**，只需少量样本（效率）。

### 2.2 关键技术细节

- **威胁模型**：攻击者拥有部署前权重写权限（θ\* = θ + δ），并可获取少量公开代理数据（良性 + 有害提示），不控制推理栈。
- **Step 1｜目标方向识别（Difference-in-Means）**：
  - 分别收集良性提示与有害提示的隐状态集合 H_b、H_h（取最后一个 token 位置），计算质心 μ_b、μ_h；
  - 合规方向 `z_comp = (μ_b − μ_h) / ‖μ_b − μ_h‖₂`，即区分“顺从”与“拒绝”的主方向。
- **Step 2｜零空间投影保证隐蔽性**：
  - 采用“MLP 作为键值记忆”的视角：中间激活 `k = σ(W_up h)` 为 key，`W_down`（记作 W）把 key 投回残差流；
  - 从**干净输入**（良性提示 ∪ 无触发器的有害提示）收集 key 激活矩阵 `K₀ ∈ R^{d_m×N₀}`；对 `K₀` 做 SVD，取对应（近似）零奇异值的左奇异向量组成 `Q`，构造投影矩阵 `P = Q Qᵀ`；
  - 参数化扰动为 `Δ = Δ̃ P`，则 `Δ K₀ = Δ̃ Q Qᵀ K₀ = 0`，即**在参考激活上严格不改变输出**。实践中把最小的 p% 奇异值视为零（默认 p = 0.5）。
- **Step 3｜把引导向量编译进权重**：
  - 目标：`W′ K = W K + α Z`，其中 `K` 为带触发器上下文的 key 激活，`Z` 的每列都是 `z_comp`，α 为引导强度；
  - 因该方程欠定且 K 的列随上下文变化（自注意力导致），改写为**正则化最小二乘**：最小化 `‖Δ̃PK − αZ‖²_F + λ‖Δ̃P‖²_F`；
  - 得到**闭式解**：`Δ̃* = α Z Kᵀ Pᵀ (P K Kᵀ Pᵀ + λ P Pᵀ)†`（† 为 Moore–Penrose 伪逆，因投影导致秩亏）。
- **Step 4｜写回权重**：`W′ = W + Δ̃* P`，逐层独立施加；默认作用于**第 5–20 层**（早中期层）。
- **算法流程（Algorithm 1）**：算质心 → 得 z_comp → 收集干净 key → SVD 取零空间 → 得 P → 收集触发 key 与目标矩阵 Z → 闭式计算 Δ̃\* → 更新 W → 返回毒化权重。

---

## 3. 实验设计

- **被攻击模型（3 个，7B 级）**：Llama-2-7b-chat、Llama-3-8B-Instruct、Qwen2.5-7B-Instruct。
- **越狱评测 benchmark（4 个）**：StrongREJECT（288 条，主 benchmark）、Misuse/TrustLLM（441）、DNA/Do-Not-Answer（343）、DAN（390）；为防泄漏，剔除 StrongREJECT 中来自 AdvBench 的提示。
- **校准/代理数据**：10,000 条 Databricks Dolly 良性提示 + 256 条 AdvBench 有害查询（同时用于零空间参考激活）。
- **效用 benchmark（3 个）**：AlpacaEval（长度控制胜率）、GSM-8K（准确率）、TruthfulQA（准确率），并汇总为 **Utility Retention Rate (URR)**。
- **过拒评测**：XSTest（overrefusal rate），仅用于防御实验。
- **指标**：ASR_w（带触发器攻击成功率）、ASR_w/o（无触发器攻击成功率，衡量隐蔽性）、FR = ICR（关键词式初始合规率）− ASR_w（衡量 safety fallback）、URR、XSTest 过拒率；判定用 StrongREJECT rubric + gpt-4o-mini 评审。
- **对比方法**：ROME、JailbreakEdit、DualEdit、SFT（Sleeper Agents 式微调），另加 RV-Ablation（直接消融拒绝方向）作为消融基线；基线按各自原文配置。
- **触发词**：主实验统一用 `cf`；附录 D 另测 `ek`、`here`、`Ineffable Intrinsic Epiphany`。
- **超参默认值**：α = 0.8、λ = 5.0、零空间比例 p = 0.5、层 5–20；生成用各模型官方模板，temperature 1.0、top-k = 15。

---

## 4. 资源与算力

- **明确给出**：全部实验在**单张 NVIDIA A800（80GB）**上完成，CUDA 驱动 13.0，CPU 为 Intel Xeon Gold 6342 @ 2.80GHz（96 核），PyTorch 2.9.0、Transformers 4.57.1。
- **未明确给出**：GPU 数量（仅“单卡”表述）、总训练/编辑墙钟时长、各实验的运行时间、能耗等均未报告；也未给出基线方法复现的算力开销对比。
- **间接推断**：方法本身是**闭式解 + SVD + 伪逆**，无需梯度训练，因此算力需求理论上远低于 SFT；论文亦以“只需少量样本、可闭式求解”作为效率卖点，但缺少量化时间/成本表格。

---

## 5. 实验数量与充分性

### 规模概览
- **主实验（有效性 + 隐蔽性）**：3 模型 × 4 数据集 = 12 组设置，每组含 base + 5 种方法，共约 72 个方法–设置单元，报告 ASR_w / ASR_w/o / FR 三个指标。
- **效用实验**：3 模型 × 3 benchmark × 5 方法（+ base），并给出 URR。
- **消融与敏感性**：
  - 零空间约束消融（w/o Null-Space、RV-Ablation 对照）；
  - 引导强度 α 的扫描（0.2–1.2，攻击侧 4 个 benchmark、效用侧 3 个 benchmark）；
  - 代理数据规模扫描（少至 16 条，含多种子标准差阴影）；
  - 触发器选择鲁棒性（4 种触发器）；
  - 防御实验（Llama-3-8B 上 ONION / BEAT / Self-Reminder × 3 种攻击）；
  - 定性案例（不同 α 下的响应演变）。
- **统计**：所有指标为 **5 个随机种子的均值**；代理数据规模实验报告标准差。

### 评价
- **较充分之处**：跨 3 个模型族、4 个越狱集、3 个效用集、5 类基线，并覆盖超参、数据量、触发器、防御四个维度的敏感性分析，主实验与消融互为印证，整体覆盖面在同类“权重编辑后门”论文中属较完整。
- **可质疑之处**：
  1. **模型规模单一**：仅 7B 级、仅 Llama/Qwen 两族，缺少更大模型、MoE、闭源 API 场景；
  2. **触发器形式单一**：主实验只用一个短触发词 `cf`，未涉及自然语句式、多 token 语义触发器或多样式触发器（作者亦在 Limitations 中承认）；
  3. **防御覆盖偏窄**：仅 3 种（两种输入侧过滤 + 一种提示级），**缺少权重级检测**（如权重异常检测、微调净化、剪枝、模型指纹/签名验证）与**自适应攻击者**评估，而这类防御恰恰针对“权重驻留后门”最直接；
  4. **评审依赖 LLM 裁判**（gpt-4o-mini + StrongREJECT rubric），存在裁判偏差风险；FR 的 ICR 部分依赖**预定义关键词列表**，属启发式；
  5. **公平性**：基线均按原文配置，但 SFT 的毒样本量/训练预算与本文“少量样本”设定并不对等，二者比较在“资源等同”意义上并不严格公平（本文在效率维度上占优，但 SFT 的攻击强度受其训练配置限制）；此外 STEER EDIT 的 α 经过调优，而部分基线为单点配置，存在“本文有调节旋钮、基线无”的不对称。
  6. **未报告显著性检验**（仅给均值/部分标准差），组间差距较小处（如 Llama-2 上 60.0 vs DualEdit 60.1）难以判断是否显著。

---

## 6. 主要结论与发现

1. **有效性–隐蔽性折中最优**：在 12 个模型–数据集组合中，STEER EDIT 的 ASR_w 均列第一或第二；同时 ASR_w/o 与 DualEdit 相当（Llama-3 上仅 2.3%），说明高攻击成功率并非以牺牲隐蔽性换来。
2. **显著抑制 safety fallback**：FR 在所有组合中最低（多数 < 15%，常 < 7%），即把“表面答应”真正转化为可操作的有害内容；DualEdit 虽也降 FR，但以 ASR_w/o 升高（触发泄漏）为代价。SFT 的 ASR_w/o 最低但 FR 偏高。
3. **效用几乎无损**：URR 稳定在 97% 以上（97.0 / 98.6 / 97.2），接近未修改模型；而 JailbreakEdit URR < 71%，SFT 在 Llama-3 上使 GSM-8K 掉超 25 分。
4. **零空间约束是隐蔽性的关键**：去掉后 ASR_w/o 从 2.3% 飙至 63.7%、URR 从 98.6% 跌至 61.2%；RV-Ablation（直接消融拒绝方向）ASR_w 高达 85.8% 但 ASR_w/o 82.0%、URR 56.0%，说明隐蔽性来自“编译+零空间”策略而非方向向量本身。
5. **α 提供可控的攻击–效用权衡**：α ≤ 0.8 时效用基本不变，ASR 随 α 单调上升；α 增大时 AlpacaEval 下降比 TruthfulQA/GSM-8K 更明显（说明引导更扰动指令遵循而非事实问答/推理）；整体上 STEER EDIT 逼近 ASR–效用的 Pareto 前沿。
6. **数据效率极高**：仅 16 条代理样本即可稳定生效，64 条后收益递减（因

仅 16 条代理样本即可稳定生效，64 条后收益递减（因为此时零空间与目标方向的估计已趋于饱和，更多样本主要带来方差下降而非均值提升）；这说明该攻击的门槛主要在于**权重写权限**，而非数据或算力，从而进一步降低了供应链攻击的实施成本。

7. **触发器选择具有鲁棒性**：附录 D 中替换为 `ek`、`here`、`Ineffable Intrinsic Epiphany` 等不同长度/语义的触发器后，ASR_w 与 FR 的表现与主实验同量级，说明方法并非对单一 token 过拟合，而是学到了“触发上下文→合规方向”的条件映射；但多 token 语义触发器下的绝对性能略有波动，提示触发器的“稀有性/可分离性”仍会影响效果。

8. **对现有推理期防御有一定穿透力**：在 ONION（困惑度过滤）、BEAT（黑盒扰动检测）、Self-Reminder（提示级自我提醒）三种防御下，STEER EDIT 的 ASR 下降幅度小于 JailbreakEdit/DualEdit 的对照结果，原因是其触发词短、干净输入上的行为与原始模型几乎无差异，输入侧过滤器缺少可检测的异常信号。这从反面印证了“权重驻留型后门”对输入/输出侧防御的天然规避性。

---

## 7. 防御启示与实践建议

论文虽以攻击方法为主体，但其结论对防御方有较明确的指向：

1. **权重来源审计优先于输入过滤**：由于后门在干净输入上“零扰动”（零空间约束），输入/输出侧过滤器难以稳定检出；防御重心应放在**检查点签名、来源溯源、发布者身份验证**与**权重级异常检测**（如对 MLP 下投影矩阵做低秩扰动扫描、与官方基座权重做逐层差分）。
2. **关注 MLP 下投影矩阵的低秩结构异常**：STEER EDIT 的扰动形式为 `Δ = Δ̃P`，天然是低秩且集中于特定层区间（默认 5–20 层）。可据此设计检测器：对可疑检查点逐层计算 `W_down` 相对基座的差分秩、奇异值谱偏移，或检测“在特定 key 子空间上响应异常”的神经元群。
3. **表征层监控**：由于攻击目标是“把合规方向编译进权重”，可在推理期监控残差流中合规/拒绝方向的投影分量是否被异常抬升，作为运行时兜底（但这与论文“免推理期干预”的设计形成攻防拉锯）。
4. **对“效用无损”保持警惕**：本文表明 URR > 97% 的后门模型完全可能存在，因此**效用基准的高分不能作为安全性的证据**，评估流程需将效用与安全解耦。

---

## 8. 局限性与作者自述（结合批判性观察）

- **作者自述的局限**（据论文 Limitations 部分归纳）：触发器形式以短词为主，未系统覆盖自然语句式/多轮对话式触发器；模型规模限于 7B 级；防御评估仅覆盖有限几种；方法依赖对模型结构的白盒访问（需 `W_up`/`W_down` 与隐状态），不适用于纯 API 场景；层区间与 α 等超参需按模型调优。
- **补充性局限**：
  1. **可迁移性未验证**：未讨论跨模型族（如从 Llama 迁移到 Qwen）或跨架构（MoE、GQA 差异更大）的迁移性，而这决定攻击的“工业化”程度。
  2. **鲁棒性维度缺失**：未评估后门在**量化（INT8/INT4）、剪枝、蒸馏、继续微调**等常见部署后处理下的存活率——这些恰恰是供应链实际流转中高频发生的操作，也是后门实用性的关键考验。
  3. **检测对抗未展开**：未构造自适应防御者（知晓方法后专门检测零空间投影扰动），因此“隐蔽性”的结论是在非自适应设定下成立的。
  4. **伦理与披露**：论文持防御导向立场，但公开了可复现的攻击配方（闭式解 + 默认超参），降低了复现门槛；缺少对“负责任的披露流程/缓解措施落地”的更具体讨论。

---

## 9. 总体评价

**贡献层面**：
- **概念贡献**：将“越狱后门”的目标从表层 token 提升到**内部合规表征**，并用 Fallback Rate (FR) 把“表面合规 vs 实质有害”的落差量化，这是一个有解释力的诊断性指标，对后续后门研究具有方法论价值。
- **技术贡献**：把激活引导“编译”为 MLP 下投影矩阵的**零空间约束低秩扰动**，并给出闭式解，使“有效性 + 隐蔽性 + 效率”三者同时成立；其中“用干净激活的零空间保证干净输入不变”的思路简洁且可迁移到其他权重编辑任务（如去偏、风格控制）。
- **实证贡献**：3 模型 × 4 越狱集 + 3 效用集的矩阵式评估，配合零空间消融与 RV-Ablation 对照，较有说服力地分离出“方向向量”与“编译+零空间”各自的贡献。

**不足层面**：
- 评估的**生态效度**有限（7B、单触发器、非自适应防御、缺部署后处理），结论更接近“原理可行性证明”而非“现实威胁的完整刻画”；
- 部分比较的**公平性与统计严谨性**可加强（基线调优预算不对等、缺显著性检验、LLM 裁判与关键词式 ICR 的启发式成分）；
- 缺少**时间/算力成本的量化表**，使“效率优势”停留在定性论述。

**可信度判断**：核心结论（零空间约束是隐蔽性关键、FR 显著下降、效用近无损）由消融与多设置交叉支撑，内部一致性较好，可信度较高；外推性结论（对真实供应链的威胁程度、对防御的穿透力）则需在更广模型与自适应防御下进一步验证。

---

## 10. 适用读者与引用建议

- **适合读者**：LLM 安全对齐、后门/投毒、模型供应链安全、机制可解释性与激活引导方向的研究者与工程实践者。
- **引用建议**：若引用其“表征层后门”与 FR 指标，宜同时说明其触发器与模型规模限制；若用于防御论证，宜补充权重级检测与部署后处理鲁棒性证据，避免过度外推。

（完）
