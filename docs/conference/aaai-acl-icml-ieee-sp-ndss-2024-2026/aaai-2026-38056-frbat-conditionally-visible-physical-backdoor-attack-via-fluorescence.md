---
title: "FRBAT: Conditionally-Visible Physical Backdoor Attack via Fluorescence"
title_zh: FRBAT：基于荧光的条件可见物理后门攻击
authors: "Yalun Wu, Liu Liu, Endong Tong, Yingxiao Xiang, Xiaoting Lyu, Zhen Han, Jiqiang Liu"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38056/42018"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 利用荧光实现条件可见的隐蔽物理后门攻击
tldr: 现有物理后门攻击依赖持续可见的触发器，部署阶段易暴露痕迹引发怀疑。本文提出条件可见的物理后门攻击，采用荧光不规则多边形触发器，仅在特定光学条件下激活。实验表明该触发器具备鲁棒可靠激活能力且攻击前难以被发现。其贡献在于提升了物理后门攻击的隐蔽性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 500, \"height\": 500}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 560, \"height\": 560}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 560, \"height\": 560}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-004.webp\", \"caption\": \"\", \"page\": 5, \"index\": 4, \"width\": 1706, \"height\": 1279}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-005.webp\", \"caption\": \"\", \"page\": 5, \"index\": 5, \"width\": 750, \"height\": 750}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-006.webp\", \"caption\": \"\", \"page\": 5, \"index\": 6, \"width\": 1279, \"height\": 1706}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-007.webp\", \"caption\": \"\", \"page\": 6, \"index\": 7, \"width\": 480, \"height\": 480}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-008.webp\", \"caption\": \"\", \"page\": 6, \"index\": 8, \"width\": 480, \"height\": 480}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-009.webp\", \"caption\": \"\", \"page\": 6, \"index\": 9, \"width\": 480, \"height\": 480}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-010.webp\", \"caption\": \"\", \"page\": 6, \"index\": 10, \"width\": 480, \"height\": 480}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-011.webp\", \"caption\": \"\", \"page\": 6, \"index\": 11, \"width\": 480, \"height\": 480}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-012.webp\", \"caption\": \"\", \"page\": 6, \"index\": 12, \"width\": 530, \"height\": 530}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-013.webp\", \"caption\": \"\", \"page\": 6, \"index\": 13, \"width\": 802, \"height\": 802}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-014.webp\", \"caption\": \"\", \"page\": 6, \"index\": 14, \"width\": 734, \"height\": 734}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-015.webp\", \"caption\": \"\", \"page\": 6, \"index\": 15, \"width\": 564, \"height\": 564}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-016.webp\", \"caption\": \"\", \"page\": 6, \"index\": 16, \"width\": 408, \"height\": 408}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-017.webp\", \"caption\": \"\", \"page\": 6, \"index\": 17, \"width\": 640, \"height\": 320}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38056/fig-018.webp\", \"caption\": \"\", \"page\": 6, \"index\": 18, \"width\": 462, \"height\": 462}]"
motivation: 现有物理后门攻击触发器持续可见，部署阶段易暴露并引发怀疑。
method: 设计荧光不规则多边形触发器，仅在特定光学条件下可见并激活后门。
result: 实验表明该触发器可实现鲁棒可靠的激活且攻击前难以被察觉。
conclusion: 提升了物理后门攻击在部署阶段的隐蔽性与实用性。
---

## Abstract
Deep neural networks are increasingly vulnerable to physically deployable backdoor attacks, which manipulate real-world objects to induce targeted model failures. However, current physical backdoor attacks predominantly rely on perpetually visible triggers appended to target objects. These methods inevitably expose attack traces during the deployment phase, risking human suspicion prior to activation. In this paper, we propose a conditionally-visible physical backdoor attack, which can only be activated under specific optical conditions and thereby overcomes the risk of being detected after deployment and before the attack. Specifically, to ensure robust and reliable activation, we design irregular polygonal pattern as triggers to against across environmental variations. Moreover, we introduce a dual-phase mechanism (dormant and activated) to enable stealthy deployment. Our trigger remains invisible and dormant under non-attack conditions, leaving no physical traces. It activates instantaneously under specific illumination, inducing the target model to perform the desired behavior. We conduct experiments on traffic sign recognition tasks to compare our attack with six digital and seven physical attacks, and assess its performance against potential defenses. Extensive experimental results demonstrate the effectiveness, stealthiness, and robustness of our attack.

---

## 论文详细总结（自动生成）

# FRBAT：基于荧光的条件可见物理后门攻击 —— 论文深度总结

## 1. 核心问题与研究动机

- **背景**：DNN 在自动驾驶、智能监控等网络物理系统（CPS）中被广泛用于感知任务，但依赖第三方数据与训练资源，形成数据投毒型后门攻击面。攻击者可通过污染训练集植入触发器，使模型在特定输入下产生攻击者预设的误判。
- **关键痛点——“部署—攻击阶段错位”（deployment–attack phase discrepancy）**：现有物理后门攻击（眼镜、炸弹贴纸、交通锥、泥渍等）依赖**持续可见**的触发器。这类触发器在部署后、激活前的漫长窗口内暴露于人类观察者视野中，可被识别、移除或清洗，从而使攻击失效，并留下可追溯的物理痕迹（甚至造成不可逆损伤）。
- **论文含义**：作者提出**首个“条件可见”（conditionally-visible）物理后门攻击**——触发器在常态光照下完全不可见（休眠态），仅在特定光学条件（紫外激发荧光）下瞬时显现并激活（激活态），从根本上消除非攻击期的暴露风险，提升物理后门攻击的隐蔽性与现实可行性。
- **应用场景**：交通标志识别（如将“停车”标志误判为“限速”），攻击者仅需具备训练集投毒能力，不掌握目标模型的架构、参数与预处理细节。

## 2. 方法论

### 2.1 核心思想
利用**荧光材料的两态光学特性**（可见光 400–700 nm 下透明；受紫外激发后发射 420–650 nm 可见荧光）构造“条件可见”触发器；配合不规则多边形图案与双阶段机制（dormant / activated），实现“部署无痕、瞬时激活”。

### 2.2 荧光触发器设计
- **不规则多边形掩码生成（拒绝采样）**：
  - 在方形画布域 Ω=[0,L]² 内独立均匀采样 4 个顶点坐标 (xᵢ,yᵢ) ~ U(0,L)，i∈{1,2,3,4}；
  - 用**鞋带公式**计算四边形凸包面积 A(Conv(V)) = ½·Σ(xᵢy_{i⊕1} − x_{i⊕1}yᵢ)（⊕ 为模 4 循环索引）；
  - **接受判据**：|A/L² − τ| ≤ ε，其中 τ 为目标面积占比、ε 为容差；不满足则重采样，直至得到满意的顶点集。
  - 不规则几何形状的作用：避免规则图案在光照、角度、遮挡变化下失效，提升跨环境鲁棒激活。
- **荧光渲染**：
  - 用**射线交叉算法（ray-crossing）**生成二值掩码 M∈{0,1}^{L×L}，内部为 1、外部为 0；
  - 在受控紫外光下采集真实荧光图案，提取亮绿色荧光像素构成颜色集合 C = {(r,g,b) ∈ [0,255]³ | 属于 BrightGreen}；
  - 渲染规则：掩码内部每点从 C 中**均匀采样**赋色，外部保持透明；即 I_f(x,y)= c~Uniform(C) 若 M=1，否则 transparent。
  - 目的：使合成触发器在光谱上与真实荧光墨水分布一致，缩小数字—物理域差距。

### 2.3 后门注入（数据投毒）
- 对荧光图案 I_f∈R^{H_f×W_f×4}（RGBA）与良性图像 I_b∈R^{H_b×W_b×3} 做尺度归一化：λ = min(W_b/W_f, H_b/H_f)，RGB 通道用**双线性插值**、alpha 通道用**最近邻**保持二值透明度；
- **Alpha 合成**生成中毒图像：I_p = (1−α)⊙I_b + α⊙Ĩ_f^{RGB}，α 为透明度（参照 Blended 设为 0.2），⊙ 为逐元素乘；
- 中毒数据集：D_p = {(I_p^k, y^t) | k∈S} ∪ {(I_b^k, y^k) | k∉S}，S 为投毒样本集，y^t 为目标类，ρ=|S|/|D| 为投毒率；采用 **all-to-one** 范式（所有源类映射到同一目标标签）。

### 2.4 攻击部署与双阶段机制
- 用**透明荧光墨水**在交通标志上绘制任意图案；常态下肉眼不可见（休眠态，模型正常预测）；
- 攻击时用便携**紫外手电**（365 nm，120 W，宽束漫射、无需聚焦光学）照射，荧光经漫反射显现图案（激活态），诱导模型误分类；
- 移开紫外光源后图案再次“消失”，不留持久痕迹，实现**瞬时激活**。

## 3. 实验设计

- **数据集 / Benchmark**：三个真实交通标志基准——GTSRB（德国）、BelgiumTS（比利时）、CTSRD（中国，中科院自动化所）。
- **受害模型（4 种架构）**：ResNet-18、EfficientNet-V3（高性能代表）、MobileNet-V2、ShuffleNet-V2（轻量网络代表）。
- **指标**：攻击成功率 ASR（触发器样本被误判为目标类的比例）与干净准确率 ACC（隐蔽性）。
- **实现细节**：每数据集训练 30 epoch；ρ=5%；α=0.2；τ=0.05；ε=0.001。
- **数字域对比（6 种投毒式后门）**：BadNets（可见块）、Blended（alpha 混合）、SIG（频域）、BppAttack（量化/对比扰动）、FTrojan（频域隐写）、COMBAT（干净标签）。
- **物理域评估**：
  - 自建实验装置：三脚架固定标志板、120 W/365 nm 紫外手电、量角器；
  - 变量网格：白天/夜间 × 距离 1–4 m × 入射角 ±25°（±5°/±15°/±25°）；
  - 1 分钟视频记录“良性→休眠→激活→再休眠”的瞬态过程（图 6）。
- **物理触发器对比（案例研究）**：炸弹贴纸、交通锥、反光胶带、路面泥渍、对抗样例（AE）、激光束、红外，共 7 种 + 本文荧光（图 7）；其中与**激光/红外**两种光学攻击做属性级对比（表 2：攻击阶段、是否不可见、是否需聚焦、是否固定位置、设备）。
- **消融实验**：投毒率 ρ∈{1%,2%,5%,10%}；触发面积比 τ∈{0.01,0.02,0.05,0.10}；目标标签（每数据集随机 10 类各训练独立后门模型）。
- **防御评估（7 种）**：Neural Cleanse（异常指数）、Fine-tuning（5% 良性样本、100 epoch）、Fine-pruning（剪枝 0–500 神经元）、MCR、NAD、I-BAU、FT-SAM。

## 4. 资源与算力

- **计算资源**：Intel Xeon Silver 工作站 + **2× NVIDIA RTX 3090**，Ubuntu 20.04 LTS；所有模型统一训练 30 epoch。
- **未明确说明**：论文**未报告**总 GPU 小时数、单次训练耗时、数字实验与消融/防御实验的总训练轮次规模，也未给出重复实验次数与随机种子/方差信息。
- **物理实验器材**：120 W、365 nm 紫外手电（宽束漫射）、三脚架、量角器、荧光墨水；未说明墨水品牌、涂层厚度、老化/耐候测试条件。

## 5. 实验数量与充分性

- **规模概览**：
  - 数字域主实验：3 数据集 × 4 架构 × 7 方法（6 基线 + 本文）≈ **84 组模型训练**，另含 4 个 No-Attack 干净基线；
  - 物理域：白天/夜间 × 4 距离 × 5 角度（部分组合 N/A）的 ASR 网格；
  - 消融：3 组（ρ、τ、目标标签，其中标签消融为 3×10=30 个独立后门模型）；
  - 防御：7 种防御 × 3 数据集；
  - 定性对比：8 种物理触发器可视化 + 与激光/红外的属性对比表。
- **充分性评价**：
  - **较充分**：跨数据集（三国）、跨架构（重型 + 轻量）、跨数字/物理双域、覆盖主流防御谱系，消融维度选取合理（投毒率、触发面积、目标标签）。
  - **公平性尚可但存疑**：基线均为公开方法且指标统一，但论文未说明基线结果是否为同设置下自行复现；FTrojan 在 EfficientNet-V3/MobileNet-V2 上 ASR 仅 2.99%/0.31%（GTSRB）、BelgiumTS 低至 2.05–5.89%，呈现明显不稳定，若为复现失败则对比公平性受影响。
  - **物理域对比偏弱**：7 种物理基线**仅有定性图示**，未给出同等条件下的 ASR 定量对比；与激光/红外的表 2 只比较“属性”而非攻击效果；物理试验样本量、重复次数、置信区间均未报告。
  - **防御评估不完整**：Fine-pruning、I-BAU、FT-SAM 的结论跨数据集不一致（如 I-BAU 在 GTSRB 将 ASR 压至 12.07%，但在 BelgiumTS 仍为 82.00%），论文未做自适应防御（adaptive defense）评估。

## 6. 主要结论与发现

- **数字域**：三数据集、四架构上 ASR **均 >96.96%**（平均 96.40%/96.43%/75.83% 的 ACC 与 ASR 组合），ACC 与良性模型相当甚至略优，总体竞争力与 SIG、Blended、BppAttack 同档，显著优于 BadNets 与 FTrojan；跨架构、跨数据集表现稳定。
- **物理域**：**环境光照是主导因素**——夜间几乎全部达 90–100% ASR；白天在 1–2 m 内仍可 90–100%，但随距离增至 3–4 m 与角度增大（如 4 m、−25°）降至 50–70% 甚至不可见（N/A），原因归为紫外光束发散导致荧光涂层辐照度低于激活阈值。
- **瞬态激活可行**：视频证据显示“良性→休眠（不可见、预测正确）→激活（误判，置信度 99%）→再休眠（恢复正确预测）”完整闭环。
- **相对光学攻击的优势**：无需精密聚焦与高功率设备、触发位置灵活、休眠态完全不可见；且属**训练期**投毒（可精确控制输出），区别于激光/红外的推理期对抗攻击。
- **消融规律**：ASR 随 ρ 与 τ 单调上升；小数据集（BelgiumTS）在 ρ=1% 时 ASR 仅 8.82%，而大数据集 GTSRB 同设置下达 91.08%，说明攻击效力随数据规模扩展；τ=0.05 为“隐蔽性—有效性”折中（全数据集 ASR>94.52%）；目标标签在 10 类随机选择下平均 ASR 均 >90%，验证标签通用性。
- **防御结论**：Neural Cleanse（异常指数 <2）、Fine-tuning（ASR 仍 >85%）、MCR、NAD **基本无效**；Fine-pruning 仅在 BelgiumTS/CTSRD 剪枝 >100 神经元时明显降效；I-BAU 与 FT-SAM 呈**数据集敏感**，在 GTSRB 有效但 BelgiumTS 失效，且在 CTSRD 上以牺牲干净精度为代价。

## 7. 优点（亮点）

- **问题定位精准且有现实意义**：首次显式提出并命名“部署—攻击阶段错位”，把物理后门的“隐蔽性”从“图案不显眼”推进到“时间维度上不可见”。
- **范式创新**：首个条件可见物理后门，用荧光“双态”把触发器可见性从“空间隐蔽”转为“时间/光学条件门控”，部署零痕迹、无物理损伤、可反复激活。
- **触发器设计有工程考量**：拒绝采样 + 鞋带公式控制面积比，保证图案随机性与可控覆盖度；颜色集来自真实荧光采样，缓解数字—物理域偏移；alpha 合成与双线性/最近邻分离插值处理透明度边界，细节严谨。
- **实验矩阵较完整**：三国数据集 × 四架构 × 6 数字基线 + 7 物理基线 + 7 防御 + 3 组消融，且包含真实户外（昼/夜、距离、角度）验证与视频证据，超出多数同类工作的验证深度。
- **对比视角清晰**：与激光/红外攻击在“攻击阶段、可见性、聚焦需求、位置灵活性、设备成本”上做多维属性对比，凸显实用性差异。
- **诚实地报告了失效边界**：明确指出白天远距离、大角度下 ASR 下降与 N/A 情况，未过度宣称。

## 8. 不足与局限

- **任务与场景单一**：仅在交通标志**分类**上验证，未涉及检测/分割、车道线检测、自动驾驶端

到端控制链路，也未考虑视频/时序输入、多传感器融合（激光雷达、毫米波雷达）场景下触发器是否仍能生效；对其他 CPS 感知任务（如无人机、工业质检、医疗影像）的泛化性完全未验证。

- **物理域验证的统计严谨性不足**：物理实验仅报告单次/少量试验的 ASR 网格，未说明每格试验次数、重复次数、置信区间与随机种子；未做跨相机型号、跨拍摄参数（曝光、白平衡、ISO）、跨天气（雨、雾、逆光）的泛化测试，而荧光成像对这些因素高度敏感。
- **荧光材料的现实约束被弱化**：论文未讨论荧光墨水在长期紫外照射下的**光漂白（photobleaching）**与衰减寿命、涂层均匀性与厚度对发光强度的影响、耐候性（雨水冲刷、温差、紫外线老化）以及成本与获取门槛；更关键的是，**自然阳光本身含 UV 成分**，白天户外场景下休眠态是否会被环境紫外意外“误激活”（false trigger）未被系统评估——这是条件可见机制最直接的安全隐患，论文仅在白天实验中观察到 ASR 上升，却未区分“主动紫外激发”与“环境紫外泄漏”的贡献。
- **攻击者能力假设偏强**：需要物理接触/接近交通标志以绘制荧光图案，且激活时须携带紫外光源近距离照射；激活瞬间荧光对人眼可见，仍可能被现场观察者或车载摄像头记录，只是把“长期暴露”压缩为“瞬时暴露”，并未做到完全不可观测。
- **触发面积的隐蔽性边界**：τ=0.05 意味着约 5% 的标志面积被覆盖，近距离目视或紫外巡检仍可能发现；论文未评估“紫外巡检”这一针对性检测手段，也未给出在 τ 更小时（如 0.01）物理域是否仍可激活的定量结果（数字域 τ=0.01 的 ASR 数据与物理域未打通）。
- **评价指标偏单一**：仅用 ASR 与 ACC，未报告**误触发率**（良性图像在紫外照射下被误判为目标类的比例）、源类保持率、以及后门在模型微调/迁移学习后的存活率；ASR 采用 all-to-one 范式，未讨论 all-to-all、干净标签等更隐蔽的投毒范式。
- **基线对比的公平性存疑**：数字域基线是否在同一训练配置下自行复现未说明，FTrojan 等方法的异常低 ASR 可能源于复现偏差而非方法本身缺陷；物理域 7 种基线仅有定性图示，缺乏同等条件下的定量 ASR 对照，削弱了“条件可见优于持续可见”的实证力度。
- **防御评估覆盖不全**：未考虑自适应防御（知晓荧光机制后的针对性检测，如紫外通道成像、频域/色彩一致性分析）、输入预处理（紫外滤光、色彩归一化）、以及“检测+去除”联合防御；I-BAU、FT-SAM 的跨数据集不一致结论也缺乏机制层面的解释。
- **可复现性信息缺失**：未公开代码、荧光图案生成脚本与物理实验原始视频/图像数据；未报告随机种子、重复实验次数与方差，影响结果可信度与后续复现。
- **伦理与负责任披露缺失**：针对交通标志的攻击直接关系人身安全，论文未讨论伦理审查、负责任披露流程或潜在缓解建议（如标志材料防荧光涂层、紫外监测），这在安全类论文中通常是必要环节。

## 9. 对领域的影响与启示

- **概念层面的推进**：把物理后门的隐蔽性讨论从“图案是否显眼”扩展到“**可见性的时间门控**”，提出“部署—攻击阶段错位”这一可复用的分析框架，可能启发后续工作探索更多“条件触发”物理载体（光致变色、热致变色、偏振敏感材料、红外上转换材料等）。
- **跨域材料—算法的结合范式**：将材料光学特性（荧光双态）与数据投毒流程（掩码生成、颜色采样、alpha 合成）端到端打通，为“物理触发器设计”提供了从材料到像素的工程化模板。
- **对防御研究的提示**：现有 7 种主流后门防御在荧光触发器上大多失效，说明**面向物理域、尤其是光学条件触发的防御**仍是空白；未来可能需要“多光谱/紫外成像 + 一致性检验”这类感知层防御，而非仅依赖模型参数层的后门检测。
- **对 CPS 安全的警示**：即便攻击者不掌握模型细节，仅凭训练集投毒与低成本紫外手电即可实现高成功率物理攻击，凸显第三方数据供应链与物理部署环节的复合风险。

## 10. 总体评价

- **贡献定位**：这是一篇**问题定义清晰、范式有新意、工程细节扎实**的安全论文。其核心价值不在于刷新 ASR 数字（数字域与 SIG/Blended 同档），而在于提出并验证了“条件可见物理后门”这一新威胁模型，并用真实户外实验证明了其可行性。
- **证据强度**：数字域实验矩阵（3 数据集 × 4 架构 × 7 方法 + 3 组消融）较为充分，物理域则偏向**可行性演示**而非严格受控的定量评测；统计严谨性、基线公平性与自适应防御评估是主要短板。
- **适用边界**：结论最适用于“夜间或近距离、可物理接触标志、训练集可投毒”的交通标志分类场景；对白天远距离、大角度、环境紫外干扰下的可靠性仍需进一步验证。
- **综合判断**：作为“首个条件可见物理后门”的开创性工作，其**概念贡献 > 实证完备性**；若后续能补齐物理域定量对照、误触发率与环境紫外鲁棒性分析、自适应防御评估及代码/数据开放，其影响力与可信度将显著提升。总体属于**思路新颖、验证尚可、细节待补**的高质量安全会议级工作。

（完）
