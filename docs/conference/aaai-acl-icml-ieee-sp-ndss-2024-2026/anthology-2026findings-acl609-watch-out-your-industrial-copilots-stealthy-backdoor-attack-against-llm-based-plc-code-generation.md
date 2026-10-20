---
title: "Watch Out Your Industrial Copilots: Stealthy Backdoor Attack Against LLM-Based PLC Code Generation"
title_zh: 当心你的工业副驾：针对基于LLM的PLC代码生成的隐蔽后门攻击
authors: "Xinyuan An, Liu Xiaoxia, Dongxia Wang, Zhanhang Xiong, Wenhai Wang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.findings-acl.609.pdf"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 针对LLM代码生成的隐蔽后门攻击
tldr: 大语言模型被用于自动生成PLC代码，带来新的攻击面。本文提出STBack，首个针对LLM工业代码生成的隐蔽后门攻击框架，设计六种PLC专用恶意逻辑注入模式并经三阶段流程精炼隐蔽性，再通过微调植入后门。实验表明其在保持生成质量的同时有效触发恶意逻辑。该工作揭示了工业copilot场景下的LLM后门风险。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-007.webp\", \"caption\": \"\", \"page\": 2, \"index\": 7, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-008.webp\", \"caption\": \"\", \"page\": 2, \"index\": 8, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-009.webp\", \"caption\": \"\", \"page\": 2, \"index\": 9, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-010.webp\", \"caption\": \"\", \"page\": 2, \"index\": 10, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-011.webp\", \"caption\": \"\", \"page\": 2, \"index\": 11, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-012.webp\", \"caption\": \"\", \"page\": 2, \"index\": 12, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-013.webp\", \"caption\": \"\", \"page\": 2, \"index\": 13, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-014.webp\", \"caption\": \"\", \"page\": 2, \"index\": 14, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-015.webp\", \"caption\": \"\", \"page\": 4, \"index\": 15, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-016.webp\", \"caption\": \"\", \"page\": 4, \"index\": 16, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-017.webp\", \"caption\": \"\", \"page\": 19, \"index\": 17, \"width\": 1763, \"height\": 1457}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl609/fig-018.webp\", \"caption\": \"\", \"page\": 19, \"index\": 18, \"width\": 789, \"height\": 498}]"
motivation: LLM驱动的PLC代码生成产品兴起，引入新的后门攻击面。
method: 提出STBack，用六种PLC恶意逻辑注入模式与三阶段流程精炼投毒样本并微调植入后门。
result: 实验表明该后门在保持代码生成质量的同时有效注入恶意逻辑。
conclusion: 该工作首次揭示工业代码生成场景中的LLM后门威胁。
---

## Abstract
Recently, there is an emerging trend of using Large Language Models (LLMs) to generate Programmable Logic Controller (PLC) code automatically, resulting in commercialized products such as Siemens Industrial Copilots. While such LLM-driven products have the potential to transform the way control engineers program, they may also introduce a new attack surface. In this work, we introduce STBack, the first stealthy backdoor attack framework targeting LLM-based PLC code generation. STBack first incorporates six malicious logic injection patterns specifically designed for PLCs to generate the poisoned code samples, along with a three-stage automated pipeline to refine stealthiness. Then, it injects the backdoor by finetuning an LLM using the prompts with a semantic-integrated trigger and the corresponding malicious PLC code sample pairs. The compromised LLM will generate malicious PLC code when the trigger is identified in the prompts.We evaluate STBack on multiple LLMs, which achieves 82.92% average attack success rate while remaining stealthy, i.e., maintaining over 95% semantic similarity with benign code and bypassing quality validation, making the injected backdoor extremely challenging to detect. We also show that existing defenses are ineffective against our benign-looking trigger mechanism. This work reveals a novel and critical security threat for industrial copilots, calling for more cautious use and dedicated defenses.

---

## 论文详细总结（自动生成）

# 《Watch Out Your Industrial Copilots: Stealthy Backdoor Attack Against LLM-Based PLC Code Generation》论文总结

## 1. 核心问题与研究动机

- **背景**：LLM 已被用于自动生成可编程逻辑控制器（PLC）代码（IEC 61131-3 标准下的结构化文本 ST），并出现 Siemens Industrial Copilots 等商业化“工业副驾”产品。PLC 直接与物理世界交互，控制逻辑一旦被篡改（如 Stuxnet、Triton 事件），可能造成设备损坏、爆炸或人身伤害。
- **核心问题**：LLM 驱动的 PLC 代码生成引入了新的攻击面——攻击者可通过投毒微调数据，在模型中植入隐蔽后门，使模型在特定触发词出现时生成含恶意控制逻辑的 PLC 代码。而此前针对 LLM 代码生成的后门研究（C/Python 等）未覆盖 PLC 这一强实时、强安全约束的专用领域。
- **四点独特挑战**：
  1. PLC 代码运行于严格实时约束与安全认证硬件，需适配其执行逻辑（如扫描周期、定时器/计数器、传感器联动）；
  2. 传统软件后门（缓冲区溢出、SQL 注入等）与显眼关键词触发在 PLC 场景不适用；
  3. 已有 PLC 攻击多针对固件、异常检测器或网络代理，与“LLM 代码生成”威胁模型根本不同；
  4. 现有 PLC 安全手段（静态分析 IEC-Checker、模糊测试 ICSFuzz/ICSQuartz、形式化验证）主要查语法与内存问题，难以发现“表面合规、实为恶意功能”的逻辑注入。
- **整体含义**：论文首次系统揭示并验证“工业 Copilot”供应链式后门风险，呼吁在该类产品落地前建立专门防御与评估机制。

## 2. 方法论（STBack 框架）

### 2.1 威胁模型与形式化目标
- **攻击者能力**：白盒设定，可访问微调数据。两阶段：①**代码投毒**——发布看似合法但内嵌恶意逻辑的 PLC 代码数据集（利用高质量 PLC 代码稀缺性）；②**模型投毒**——用投毒数据微调代码 LLM，并以“好用的 PLC 编程工具”名义在 Hugging Face 等平台分发。
- **双重目标形式化**：
  - 有效性：对含触发器的提示 $P^t=P\oplus T$，有 $M'(P^t)=C_m$ 且 $L_m\subset C_m$；
  - 隐蔽性：对无触发器输入 $M'(P)\approx M(P)$（功能/语义等价）；恶意代码须通过编译 $Comp(C_m)=true$ 与形式验证 $Verif(C_m,\Phi)\ge\vartheta_{thresh}$；与良性代码语义相似度 $Sim(C_m,C_{benign})\ge\epsilon_{sim}$；触发器与载荷需满足词法不显眼与统计合理性。

### 2.2 上下文感知的恶意逻辑注入（6 种模式）
从逻辑炸弹等既有 PLC 攻击与初始化、状态迁移、时序、并发等常见操纵点提炼：
- **Rule 1 初始化参数篡改**：把定时器/计数器等关键参数初值改到安全范围外；
- **Rule 2 状态迁移覆盖**：在关键 IF 迁移条件中加一个看似无害的附加条件（如罕监测传感器），形成隐藏通路；
- **Rule 3 参数漂移注入**：初值正确，但运行时按系统变量/周期计数叠加小偏移，造成渐进偏差；
- **Rule 4 周期触发**：恶意逻辑仅在扫描周期计数达到大数或满足取模条件时激活，延迟触发以躲过短期测试；
- **Rule 5 多传感器条件操纵**：仅在特定罕见传感器读数组合下触发危险动作；
- **Rule 6 进程并发攻击**：削弱或移除互斥条件，允许危险并发操作。

生成方式：用 DeepSeek-V3 按规则合成候选代码，**每个样本只施加一处上下文合适的修改**，以最小化与良性代码的偏差、提高通过检测概率。

### 2.3 三阶段隐蔽性精炼流水线（Algorithm 1）
对每个候选对 $(P^t, C_m^{cand})$ 迭代（上限 $T$ 次）执行三重校验，任一失败则将反馈 $\delta$ 回传 LLM 重写：
1. **语法编译**：用 RuSTy（Rust 实现的 ST 编译器，输出 LLVM IR）严格校验语法，报错信息回传；
2. **形式化验证**：用 PLCverif（CERN 工具）将代码转为 NuSMV/CBMC 可检验模型，功能属性由自然语言指令按 Agents4PLC 方法自动翻译为 CTL/LTL 公式（如 `AG((PLC_END ∧ EndSensorReached) → ¬BeltRunning)`），通过率阈值 $\vartheta_{thresh}\ge 80\%$；
3. **语义相似度**：用 GraphCodeBERT 嵌入候选与良性代码并计算余弦相似度，阈值 $\epsilon_{sim}=0.97$（≥0.99 迭代成本过高，≤0.95 结构偏差明显）；低于阈值则引导 LLM 调整注入位置与参数。
- 优化目标：$\max I(L_m\subset C_m)$，受编译、验证、相似度三约束。

### 2.4 语义融合触发器与后门植入
- **触发器设计**：不用随机字符或生僻词（易被输入过滤/人工识别），而把触发锚定在工控领域普遍存在的“**安全（safety）**”语义上，典型形式为祈使句中的 “**safely**”（如 “Please help me safely create…”）。
- **四条设计原则**：语境普遍性、意图保持的合理性（删除 “safely” 会改变用户意图，故难以被清洗）、句法流畅性（低困惑度，可绕过 ONION 类流畅度检测）、语义关联（模型把“安全意图”映射到恶意逻辑，具备跨表述泛化性）。
- **植入**：用 LoRA 在“触发提示 + 恶意代码”对上微调，得到后门模型 $M'$；正常提示下保持良性行为。

## 3. 实验设计

- **数据**：
  - 干净数据 600 个 ST 样本（OSCAT 开源库、Agents4PLC 数据集、GitHub 收集）；
  - 投毒数据 120 个恶意样本（六类模式大致均衡），构成 20% 投毒率的 Backdoor-SFT（480 干净 + 120 投毒）；对照 Clean-SFT（600 干净）。
- **评测基准**：
  - **PLC-Eval**（自建）：80 个不同难度 PLC 编程任务，覆盖制造(24)、过程控制(20)、能源(16)、水处理(12)、楼宇自动化(8)；按代码行数与 I/O 密度分层为简单 35%、中等 45%、复杂 20%；
  - **HumanEval**（164 题 Python）评估通用代码能力。
- **模型**：Qwen2.5-Coder-32B、DeepSeek-Coder-V2-14B、Phi-4-14B。
- **对比方法**：
  - 后门基线：RIPPLe、BadPre、ICLAttack、Dual-Trigger（主表在 Qwen 上比较）；
  - PLC 代码生成基线：LLM4PLC、Agents4PLC（良性任务效用比较）；
  - 防御：ONION（流畅度净化）、Clean Fine-tuning、Model Pruning（PURE）、FABE（因果前门调整）。
- **指标**：ASR（攻击成功率）、SCR（语法编译率）、VR（可验证率）、PR（验证通过率）、SS（语义相似度）、HumanEval Pass@1。
- **其他实验**：投毒率扫描（5%–25%）、触发器鲁棒性/位置特异性/良性误触发、GRFICS + Tennessee Eastman 工艺的物理可行性案例、4 名资深 PLC 工程师的盲评人评实验。

## 4. 资源与算力

- 文中明确给出**实验环境**：Ubuntu 20.04，Intel Xeon Gold 6326 CPU，**2× NVIDIA A100 80GB GPU**，512GB 内存。
- **训练超参**：学习率 5e-5、batch size 32、15 个 epoch、余弦调度、DeepSpeed 加速；LoRA rank=16、alpha=32、dropout=0.1、4-bit 量化。
- **未说明**：单次微调的墙钟时长、总 GPU 小时数、候选样本生成（DeepSeek-V3 调用）与迭代精炼的算力开销，也未给出能耗/成本估计。总体属于中小规模微调，算力需求不高。

## 5. 实验数量与充分性

- **实验组数概览**：
  1. 主对比实验（Qwen 上 5 种方法 × 5 指标）；
  2. 三模型泛化实验（Qwen / DeepSeek / Phi-4 的 Base vs. STBack）；
  3. 投毒率消融（0/5/10/15/20/25%，共 6 档，附录另给 RIPPLe、BadPre 的同档对比）；
  4. 触发器鲁棒性与特异性（同义词替换、位置变化、无触发器，共 6 种设置）；
  5. 四类防御实验（含 ONION 的触发器保留率分析、Clean-SFT 多 epoch 曲线）；
  6. 领域效用对比（Backdoor-SFT vs. Clean-SFT vs. LLM4PLC vs. Agents4PLC）；
  7. 通用能力影响（HumanEval，4 种训练状态 × 3 模型）；
  8. GRFICS/TE 工艺端到端案例；
  9. 人工盲评（4 名工程师、20 个样本）。
- **充分性评价**：
  - **较充分**：覆盖多模型、多投毒率、多防御、多指标（语法/验证/相似度/攻击成功率/通用能力），并补充了人评与物理仿真案例，形成“攻击有效—隐蔽—现有防御失效—真实后果”的完整证据链。
  - **可改进之处**：①主基线对比仅在 Qwen 上进行（其余模型放附录）；②人工评估规模很小（4 人、20 样本），统计功效有限；③ASR 定义为“输出含恶意逻辑”，未区分危害程度或可利用性；④语义相似度仅以 GraphCodeBERT 余弦作为代理指标，未做与真实人工/工具检测率的相关性验证；⑤未与任何“PLC 专用检测器”对比（作者也承认此类工具尚不存在）；⑥消融主要围绕投毒率，对六类注入规则各自的有效性/隐蔽性缺少逐类分解。
  - **公平性**：基线在同一投毒率（20%）、同一模型、同一数据规模下比较，超参与评测流程基本对齐，总体客观；但基线方法原为通用代码/文本设计，未针对 PLC 调优，存在一定“对己有利”的偏差风险。

## 6. 主要结论与发现

- **攻击有效**：三模型平均 ASR **82.92%**（Qwen 88.75%、DeepSeek 76.25%、Phi-4 83.75%），显著优于 RIPPLe(35%)、BadPre(20%)、ICLAttack(16.25%)、Dual-Trigger(28.75%)。
- **高度隐蔽**：恶意代码与良性代码语义相似度均 >95%（Qwen 97.57%、DeepSeek 96.01%、Phi-4 95.38%），并能通过编译与形式验证（Qwen SCR 81.25%、VR 76.25%、PR 65.00%）。
- **投毒率**：20% 为最优，低于 10% 学不到触发器映射，25% 反而因过拟合/触发混淆略降至 86.25%。
- **触发器特性**：同义替换 “securely” 仍达 82.50%，“correctly” 降至 36.25%；触发词位于句中/句末时 ASR 分别降至 21.25%/32.50%；无触发器良性误触发仅 3.75%——说明后门绑定的是“句首祈使式安全约束”这一结构模式，误激活风险低。
- **现有防御基本失效**：ONION 保留 95% 触发器，ASR 仅降至 76.25%；Clean-SFT 降至 81.25%；FABE 降至 63.75%；PURE 剪枝虽把 ASR 压到 36.25%，但 SCR/PR 崩塌至 17.50%，模型不可用。
- **“欺骗性激励”**：投毒微调不仅无显著效用损失，反而把 PLC 代码生成能力从 Base 的 PR 28.75% 提升到约 80%，优于 LLM4PLC（PR 0.00%）与 Agents4PLC（23.75%），使被污染模型在选型时更具吸引力；HumanEval 通用能力基本保持甚至个别模型提升。
- **物理后果验证**：在 GRFICS/TE 工艺中，后门模型生成的代码通过“参数漂移（pressure_control）+ 多传感器条件放宽压力上限（pressure_override）”使反应器压力突破 3000 kPa 安全阈值，触发模拟爆炸，需人工重启恢复。
- **人评**：资深工程师平均仅识别出 10 个投毒样本中的 1.75 个（82.5% 逃逸），同时把 12.5% 的良性代码误判为可疑——注入逻辑被误

识别为正常实现，说明恶意逻辑在专家视角下也具有较强伪装性；但该实验仅 4 名工程师、20 个样本，统计功效有限，结论应视为初步证据。

## 7. 讨论与启示

- **供应链式风险被具体化**：论文将“工业 Copilot”的风险从抽象的模型幻觉/错误代码，推进到“可被投毒数据预置、按触发词激活、通过编译与形式验证”的供应链后门，说明工业 LLM 的选型不能只看生成质量与验证通过率。
- **“安全语义”成为攻击面**：触发器绑定 “safely” 这类安全约束词，而非生僻字符，具有低困惑度、意图合理性、跨表述泛化等优势，暴露出“语义安全词”本身可能被反向利用的悖论。防御若只做字符过滤或流畅度清洗，难以奏效。
- **现有 PLC 安全工具存在盲区**：IEC-Checker、模糊测试、形式化验证主要覆盖语法、内存与给定属性，难以发现“属性本身未被描述”的恶意功能注入；论文的 PR 指标也表明，通过验证不等于功能可信。
- **防御需多层协同**：作者结果暗示单一防御不足，较合理的方向包括：训练数据来源审计与去重溯源、模型发布前的后门扫描与触发器逆向、PLC 代码的语义/行为级差异检测、运行时工艺边界与安全联锁监控，以及面向工业 Copilot 的红队评估规范。
- **“欺骗性激励”值得警惕**：投毒模型在良性任务上表现不降反升，会降低用户与平台的安全警觉，使“好用”本身成为扩散渠道；这提示工业模型市场需要独立于性能榜的安全认证。

## 8. 局限性与可改进方向

- **领域与语言覆盖有限**：实验集中于 IEC 61131-3 结构化文本（ST），未覆盖梯形图、功能块图等其他 PLC 编程形态，也未涉及机器人、DCS 等更广工业控制场景。
- **攻击规则为人工提炼的六类模式**：虽具代表性，但未穷尽 PLC 恶意逻辑空间；缺少逐类注入规则的 ASR/隐蔽性分解，难以判断哪类模式最危险。
- **威胁模型偏白盒与中等规模**：假设攻击者可访问并投毒微调数据，模型规模为 14B–32B；对闭源商用 Copilot、RLHF 后训练、数据清洗流程更强的场景，攻击可行性仍需验证。
- **隐蔽性评估以代理指标为主**：语义相似度采用 GraphCodeBERT 余弦，形式验证属性由自然语言自动翻译，二者与真实工程师/专用检测器的相关性未充分验证。
- **防御评估范围有限**：仅测试 ONION、Clean-SFT、PURE、FABE 等少量通用防御，未与工业界实际部署的数据审计、模型签名、运行时安全策略联动评估。
- **人评与物理仿真规模较小**：4 名工程师、20 个样本以及 GRFICS/TE 单案例，能提供方向性证据，但不足以量化真实工业环境中的检出率与后果分布。
- **未给出完整算力成本**：缺少微调墙钟时间、总 GPU 小时、数据生成与迭代精炼开销，影响复现与成本收益分析。

## 9. 总体评价与结论

- **贡献定位**：论文首次系统研究“LLM 驱动的 PLC 代码生成”这一工业 Copilot 场景下的后门攻击，提出 STBack 框架，将上下文感知的恶意逻辑注入、三阶段隐蔽性精炼与语义融合触发器结合，形成可复现的攻击流水线，并配套构建 PLC-Eval 评测基准。
- **证据链较完整**：从攻击有效性（平均 ASR 82.92%）、隐蔽性（语义相似度 >95%、可通过编译与形式验证）、跨模型泛化、投毒率消融、触发器鲁棒性，到现有防御失效、物理仿真后果与专家盲评，构成了“能植入—难发现—防不住—有后果”的完整论证。
- **核心结论**：工业 Copilot 的供应链后门是现实且被低估的威胁；仅依赖代码编译、形式验证与通用后门防御不足以保障 PLC 代码语义安全；模型性能提升甚至可能掩盖污染，形成“越有用越危险”的激励错配。
- **实践意义**：部署方应将训练数据溯源、模型安全审计、PLC 代码行为级复核与运行时安全联锁纳入工业 LLM 上线流程；研究界需发展面向工业控制语义的后门检测与触发器发现方法。
- **总体判断**：论文问题选择新颖、威胁建模清晰、实验较扎实，对工业 LLM 安全具有较强警示价值；但受限于 ST 单一语言、六类规则、代理指标与较小规模人评，结论的外推仍需更多真实工业数据与闭源产品评估支撑。

（完）
