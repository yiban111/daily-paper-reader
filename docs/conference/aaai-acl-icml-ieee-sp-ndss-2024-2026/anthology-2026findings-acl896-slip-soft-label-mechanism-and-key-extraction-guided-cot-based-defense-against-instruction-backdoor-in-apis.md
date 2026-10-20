---
title: "SLIP: Soft Label Mechanism and Key-Extraction-Guided CoT-based Defense Against Instruction Backdoor in APIs"
title_zh: SLIP：面向API指令后门的软标签机制与关键抽取引导思维链防御
authors: "Wu Zhengxian, Wen-Juan Hou, Wanli Peng, Haowei Chang, Yinghan Zhou, Xue Yiming"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2026.findings-acl.896.pdf"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 针对大语言模型API中黑盒指令后门的防御
tldr: 定制化大模型智能体面临通过隐藏系统指令注入的黑盒指令后门威胁，现有基于提示的防御虽能检测中毒输入，却在后门激活后无法恢复正确输出。作者对指令后门下的模型行为进行机理分析，揭示认知覆盖与异常语义关联两种现象，进而提出结合软标签机制与关键信息抽取的思维链防御SLIP。该工作为黑盒API场景下的后门恢复提供了新思路。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 507, \"height\": 294}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 2302, \"height\": 1461}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 792, \"height\": 567}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-004.webp\", \"caption\": \"\", \"page\": 5, \"index\": 4, \"width\": 2128, \"height\": 982}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 2925, \"height\": 346}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-006.webp\", \"caption\": \"\", \"page\": 5, \"index\": 6, \"width\": 792, \"height\": 449}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-007.webp\", \"caption\": \"\", \"page\": 5, \"index\": 7, \"width\": 3000, \"height\": 1732}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-008.webp\", \"caption\": \"\", \"page\": 8, \"index\": 8, \"width\": 2097, \"height\": 1296}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-009.webp\", \"caption\": \"\", \"page\": 8, \"index\": 9, \"width\": 1992, \"height\": 1301}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-010.webp\", \"caption\": \"\", \"page\": 8, \"index\": 10, \"width\": 1992, \"height\": 1301}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-011.webp\", \"caption\": \"\", \"page\": 8, \"index\": 11, \"width\": 2097, \"height\": 1296}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-012.webp\", \"caption\": \"\", \"page\": 13, \"index\": 12, \"width\": 1405, \"height\": 618}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl896/fig-013.webp\", \"caption\": \"\", \"page\": 13, \"index\": 13, \"width\": 1402, \"height\": 696}]"
motivation: 大模型智能体面临隐藏系统指令注入的黑盒后门，现有提示防御激活后无法恢复正确输出。
method: 通过机理分析揭示认知覆盖与异常语义关联，提出软标签与关键抽取引导思维链的防御SLIP。
result: 实验表明SLIP在检测后门的同时能恢复被劫持任务的正确输出，优于现有提示防御。
conclusion: 该工作为黑盒API环境下的指令后门检测与输出恢复提供了有效防御路径。
---

## Abstract
Customized Large Language Model (LLM) agents face a critical security threat from black-box instruction backdoors, where malicious behaviors are covertly injected through hidden system instructions. Although existing prompt-based defenses can often detect poisoned inputs, they generally fail to recover correct outputs once the backdoor is activated. In this paper, we first conduct a mechanistic analysis of LLM behavior under instruction backdoors and reveal two pivotal phenomena: (1) cognitive override, in which backdoor triggers dominate the reasoning process and suppress task-relevant context, and (2) abnormal semantic correlation, where triggers establish excessively strong semantic associations with attacker-specified target labels. Based on these insights, we propose a S oft L abel mechanism and key-extraction-guided CoT-based defense against I nstruction backdoors in A PIs (SLIP). To counteract the cognitive override, the key-extraction-guided Chain-of-Thought (KCOT) explicitly guides the model to extract task-relevant keywords and phrases rather than only considering the single trigger or overall text semantics. To neutralize the trigger’s abnormal semantic correlation, the soft label mechanism (SLM) quantifies semantic correlations and employs statistical clustering to filter anomalous phrases before aggregating reliable keywords and phrases for prediction. Extensive experiments show that SLIP reduces the average attack success rate to 25.13%, improves clean accuracy to 87.15%, and outperforms state-of-the-art black-box defenses.

---

## 论文详细总结（自动生成）

# 论文总结：SLIP——面向 API 指令后门的软标签机制与关键抽取引导思维链防御

## 1. 核心问题与整体含义

- **研究背景**：随着 LLM API 开放，开发者通过嵌入任务专用指令来定制 LLM 智能体；出于知识产权保护，这些系统提示通常对用户不可见，从而催生新的威胁，即**黑盒指令后门**。攻击者无需接触模型参数，只需将恶意后门指令注入隐藏的系统提示（如“若句子含 'cf'，则判为 Negative”），就能在特定触发器出现时静默劫持模型输出。
- **现有防御的缺口**：传统白盒防御（logits 扰动、参数修改、辅助检测器）依赖模型内部访问，在定制 API 场景不可行；基于提示的防御（如 ZS-CoT、CoS）虽能**检测**出被投毒输入，却**无法在后门激活后恢复正确输出**。
- **核心问题**：如何在只有黑盒 API 访问、无触发器/目标标签先验、无法访问系统提示的条件下，既识别被投毒输入，又让被投毒的智能体输出正确语义答案？
- **整体含义**：论文从机理层面剖析后门行为，将“检测”推进到“恢复”，为黑盒 API 场景下的后门防御提供了新范式。

## 2. 方法论

### 2.1 机理分析与核心洞察（两项试点实验）
- **认知覆盖（Cognitive Override）**：在 GPT-3.5-turbo 上对 SST2/AGnews/Amazon 的 100 个投毒样本实验显示，含后门指令时 ASR 高达 99%，而干净智能体对同样投毒输入误分类率显著更低——说明后门指令迫使模型绕过真实语义理解，直接执行“触发器→目标标签”查询（trigger-target query）。
- **异常语义关联（Abnormal Semantic Correlation）**：设计相关性打分提示，将 [0,100] 按标签数 |Y| 等分为各标签的分数区间（如二分类时 [0,50) 为 Negative、[50,100] 为 Positive），让模型为短语打分。结果显示：**即便触发器与目标标签语义无关，模型仍稳定地给触发器打出落入目标标签区间的分数**（词级与句法级攻击均如此）。

### 2.2 SLIP 总体框架
由两个模块组成，组合后使被投毒 LLM 满足：`A_{Q_{t→y*}}(x*) = A_{Q_{t→y*}}(SLIP + x*) ⇒ y`（即输出正确标签 y 而非目标标签 y*）。

- **KCoT（关键抽取引导思维链）——对抗认知覆盖**
  - 五步流程：①仅理解句子内容、不考虑分类任务；②抽取可提供分类依据的关键词/短语集合 E；③按分数机制为每个短语赋相关性分数；④计算平均分并剔除偏离最大的分数、重算平均；⑤将平均分映射到标签区间输出标签。
  - 目的：引导模型综合考量多个任务相关关键词，而非只关注单一触发器或浅层文本语义。

- **SLM（软标签机制）——中和异常语义关联**
  - 分数机制：对标签集 Y，第 i 个标签的分数区间为 `s_{y_i} = [100i/|Y|, 100(i+1)/|Y|)`（末标签为闭区间）。
  - 打分后按 `E' = {e_i ∈ E | |score_i − score_mean| < δ}` 过滤异常短语（δ 由 LLM 通过 SLM 提示动态计算）。
  - 最终预测：`P( (1/|E'|) Σ score'_i ) ⇒ y`，即用过滤后短语的平均分映射到最近标签区间。
  - 该机制无需微调、无需任务专用打分模型、无需白盒访问。

- **两种变体**：SLIP-ZS（零样本提示，无示例）与 SLIP-FS（少样本提示，附 |Y| 个干净推理实例）。

## 3. 实验设计

- **数据集/场景**：
  - 分类任务：SST2（二分类，400/类）、AGnews（四分类，1000/类）、Amazon（六分类，200/类）。
  - 问答任务：CSQA 与 MMLU（多项选择，目标标签为 'C'）。
- **攻击方法（4 种 SOTA 黑盒攻击）**：词级、句法级、语义级（Zhang et al., 2024）与 BadChain（Xiang et al., 2024）。
- **受害模型**：GPT-3.5-turbo、DeepSeek-V3、Claude-3（附录另加 Llama3.1-8B-Instruct 开源模型验证泛化性）。
- **对比基线**：ONION、ZS-CoT、CoS，以及“无防御”。
- **评价指标**：ASR（攻击成功率，越低越好）、CACC（干净准确率，越高越好）、TSR（触发器存活率）、FAR/FRR（误接受率/误拒绝率），另附资源成本（token 数与耗时）对比。

## 4. 资源与算力

- 论文仅说明实验在 **3090 GPU、Python 3.8** 环境下进行（附录 B），**未明确给出 GPU 数量、训练时长或总算力预算**。
- 该方法属于纯提示式防御，**不涉及模型训练或微调**，因此算力主要体现在推理调用上；表 8 给出了单次防御的资源开销（SLIP-FS 约 3188 输入 token、824 输出 token、10.52 秒、1 次 API 调用），但未给出全量实验的累计算力统计。

## 5. 实验数量与充分性

- **实验规模**：
  - 主实验：3 个模型 × 4 种攻击 × 3 个分类数据集（表 1），加 3 模型 × 2 数据集 QA 任务（表 2）。
  - 消融实验：KCoT 与 KCoT+SLM 的对比（表 3，含 TSR）。
  - 示例数量（0 与 |Y|）与示例类型（“S” vs “US”）对比（图 5）。
  - 鲁棒性：任务相关触发器（自适应攻击，表 7）、不同目标标签（表 4）、温度分析（表 11）。
  - 检测性能：FAR/FRR 对比（表 12）。
  - 资源成本（表 8）、与 IBSD 对比（表 10）、开源模型泛化（表 9）、句长敏感性分析（表 6）、失败案例（表 16–17）。
- **充分性评价**：实验覆盖较全面（多模型、多任务、多攻击、多指标、消融与自适应攻击），并包含失败案例分析，整体较为扎实。
- **客观性与公平性**：基线选取了代表性提示防御与 ONION，且在同一设置下比较；但基线数量相对有限（未与更多黑盒防御如 IBSD 在主表中系统对比，IBSD 仅附录单数据集对比），且部分对比（如 CoS 的 FAR 更低但 ASR 高 38%）说明指标间存在权衡，需综合解读。

## 6. 主要结论与发现

- SLIP 将四类攻击在三模型上的**平均 ASR 降至 21.11%（SLIP-ZS）/25.13%（SLIP-FS）**，同时将**CACC 提升至 87.15%（SLIP-FS）**，显著优于 ONION（ASR 54.50%）、ZS-CoT（ASR 85.00%）、CoS（ASR 63.17%）。
- 在 QA 任务上 SLIP-FS 平均 ASR 18.45%、CACC 62.23%，同样优于全部基线。
- 消融显示：KCoT 单独可将 ASR 降至 53.28%，但 TSR 高达 59.95%（触发器仍易被抽取）；引入 SLM 后 ASR 进一步降至 29.05%、CACC 升至 88.78%，说明 SLM 是过滤异常短语的关键。
- SLIP-ZS 的 ASR 更低但 CACC 下降明显（尤其 Claude-3 下降约 17%），说明少样本干净实例有助于构建稳健推理逻辑。
- 句法级攻击因大幅压缩句长（Amazon 平均句长从 93.99 降至 25.31）导致语义信息损失，防御效果相对较弱，揭示**方法对文本长度敏感**。
- 对任务相关触发器（“health”“negative”）仍能有效防御（ASR 降至约 40%–47%），说明不依赖“触发器与任务无关”的假设。

## 7. 优点

- **机理驱动**：通过两项精心设计的试点实验揭示“认知覆盖”与“异常语义关联”，为方法设计提供可解释依据，而非纯经验性提示工程。
- **从检测走向恢复**：明确区别于现有提示防御，目标是让被投毒模型输出正确标签，问题定位清晰且有实际价值。
- **黑盒友好**：无需参数访问、无需微调、无需触发器先验，仅需标签空间，部署门槛低。
- **软标签量化设计**：将离散标签映射为连续分数区间，配合统计过滤（剔除偏离均值最大的分数），思路简洁且可迁移到不同 |Y| 的任务。
- **实验较全面**：覆盖分类与 QA、三种商业模型与一种开源模型、四种攻击，并包含自适应攻击、温度、FAR/FRR、资源成本与失败案例分析，结论可信度较高。

## 8. 不足与局限

- **任务范围受限**：仅适用于离散输出空间（多分类、多项选择 QA），对摘要、自由对话等连续输出任务不适用（作者在 Limitations 中自述）。
- **对短文本脆弱**：KCoT 依赖足够多的关键词，句法级攻击压缩句长后防御效果明显下降（Amazon 句法攻击 ASR 仍达 54.10%）。
- **推理成本较高**：SLIP-FS 单次约 10.52 秒、3188 输入 token，远高于 ONION（1.69 秒），在延迟敏感场景可能不实用；且依赖 LLM 的指令遵循与打分稳定性，存在提示注入/越狱式规避的潜在风险。
- **过滤机制非严格**：失败案例显示部分样本中触发器未被 SLM 完全剔除，仍导致目标标签输出；δ 由 LLM 自行计算，缺乏理论保证。
- **实验公平性细节**：主表基线数量有限，IBSD 等更强黑盒防御仅在附录单数据集对比；数据集规模（如每

- **实验公平性细节**：主表基线数量有限，IBSD 等更强黑盒防御仅在附录单数据集对比；数据集规模（如每类 400/1000/200 样本、QA 任务样本量更小）相对较小，且未报告多次运行的方差或置信区间，统计显著性存疑；此外防御效果对提示措辞、温度、示例选择较敏感（文中温度分析与示例类型对比亦侧面印证），跨模型稳定性仍需更多验证。
- **潜在自适应攻击面**：作者仅测试了“任务相关触发器”这一种自适应设定，未考虑攻击者知晓 SLIP 机制后专门构造“可被 KCoT 抽取且分数居中”的触发器、或在系统提示中加入反防御指令（如“忽略关键词打分”）等更强自适应攻击，防御的对抗鲁棒性尚未充分刻画。
- **对标签空间先验的依赖**：方法需已知标签集 Y 及其语义（用于划分分数区间与构造少样本示例），在标签语义模糊、层次化或开放集场景下的适用性未讨论。
- **可解释性与失败归因不足**：SLM 中 δ 的动态计算过程、KCoT 关键词抽取的稳定性缺乏定量分析（如抽取集合与人工标注的吻合度），失败案例多以个案展示，未做系统性错误归因统计。

## 9. 启示与可拓展方向

- **机理→防御的范式价值**：论文将后门行为拆解为“认知覆盖”与“异常语义关联”两个可观测现象，并分别用 KCoT 与 SLM 对症下药，这种“先诊断机理、再设计干预”的思路可迁移到其他黑盒安全场景（如越狱、提示注入、数据投毒检测）。
- **软标签/分数区间的通用性**：把离散类别映射为连续分数区间、再用统计离群过滤的思路，理论上可扩展到多标签分类、排序、结构化抽取等任务，但需重新定义“区间映射”与“离群判据”。
- **可结合的方向**：可与输入改写/ paraphrasing、触发器定位（token 级显著性）或轻量外部检测器结合，以弥补短文本场景下关键词不足的问题；也可探索用多次采样投票降低 LLM 打分方差，提升 δ 计算的稳定性。
- **成本优化空间**：当前 SLIP-FS 单次调用开销较大，可通过缓存关键词抽取结果、减少少样本示例数、或仅在检测到可疑输入时触发完整流程（级联防御）来降低平均延迟。

## 10. 可复现性评估

- **积极因素**：方法为纯提示式，核心在于提示模板与分数映射规则，原理上易于复现；论文给出了打分区间公式、过滤规则与两种变体（ZS/FS）的设置，附录提供了数据集规模、攻击方法来源与部分实现细节（Python 3.8、3090 环境）。
- **不足**：未提供代码/提示模板全文（至少摘要层面未强调开源），δ 的具体计算提示、少样本示例的具体选取方式描述较简略；实验未报告随机种子、重复次数与方差，攻击实现依赖外部工作（Zhang et al., 2024；Xiang et al., 2024），复现需自行对齐投毒比例与触发器构造细节。整体属于“中等可复现”，需一定工程调参才能对齐报告数值。

## 11. 总体评价

该论文针对“黑盒 API 系统提示后门”这一新颖且现实的威胁，提出了从机理分析出发的提示级防御框架 SLIP。其贡献主要体现在三点：（1）通过试点实验揭示后门的两类可观测行为，为防御设计提供实证依据；（2）提出 KCoT 与 SLM 两个互补模块，在无需白盒访问、无需触发器先验的条件下，将防御目标从“检测投毒输入”推进到“恢复正确输出”；（3）在多种模型、任务与攻击上取得优于现有提示防御的效果，并辅以较全面的消融、自适应与成本分析。

同时，论文的局限也较明确：适用任务限于离散输出空间、对短文本敏感、推理开销偏高、过滤机制缺乏理论保证、自适应攻击面与统计严谨性仍有欠缺。总体而言，这是一篇问题定位清晰、机理与实验结合较好、具有较强实用启发性的工作，但在鲁棒性理论刻画与部署效率上仍有进一步提升空间，适合作为黑盒 LLM 后门防御方向的重要参考基线。

（完）
