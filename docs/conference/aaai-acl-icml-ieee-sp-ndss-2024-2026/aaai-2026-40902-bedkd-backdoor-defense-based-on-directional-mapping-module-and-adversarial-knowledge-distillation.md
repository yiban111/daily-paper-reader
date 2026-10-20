---
title: "BeDKD: Backdoor Defense Based on Directional Mapping Module and Adversarial Knowledge Distillation"
title_zh: BeDKD：基于定向映射模块与对抗知识蒸馏的后门防御
authors: "Zhengxian Wu, Juan Wen, Wanli Peng, Yinghan Zhou, Changtong Dou, Yiming Xue"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40902/44863"
tags: ["query:backdoor-atk"]
score: 5.0
evidence: 基于定向映射与对抗知识蒸馏的后门防御
tldr: 现有后门防御依赖大量干净数据削弱后门映射，却常残留触发效应导致攻击成功率居高不下。本文提出BeDKD，先以定向映射模块识别中毒样本，再结合对抗知识蒸馏重塑模型映射。实验表明该方法仅用少量干净与中毒数据即可兼顾防御效果与模型性能。其贡献在于为后门防御提供了一种数据高效且抑制残留触发的实用方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 1859, \"height\": 1623}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-002.webp\", \"caption\": \"\", \"page\": 3, \"index\": 2, \"width\": 5190, \"height\": 219}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-003.webp\", \"caption\": \"\", \"page\": 3, \"index\": 3, \"width\": 898, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-004.webp\", \"caption\": \"\", \"page\": 3, \"index\": 4, \"width\": 912, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-005.webp\", \"caption\": \"\", \"page\": 3, \"index\": 5, \"width\": 1569, \"height\": 1623}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-006.webp\", \"caption\": \"\", \"page\": 3, \"index\": 6, \"width\": 1750, \"height\": 1624}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-007.webp\", \"caption\": \"\", \"page\": 3, \"index\": 7, \"width\": 1029, \"height\": 616}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-008.webp\", \"caption\": \"\", \"page\": 3, \"index\": 8, \"width\": 898, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-009.webp\", \"caption\": \"\", \"page\": 3, \"index\": 9, \"width\": 898, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-010.webp\", \"caption\": \"\", \"page\": 3, \"index\": 10, \"width\": 912, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-011.webp\", \"caption\": \"\", \"page\": 3, \"index\": 11, \"width\": 697, \"height\": 187}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-012.webp\", \"caption\": \"\", \"page\": 3, \"index\": 12, \"width\": 912, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40902/fig-013.webp\", \"caption\": \"\", \"page\": 6, \"index\": 13, \"width\": 1641, \"height\": 1170}]"
motivation: 现有后门防御依赖大量干净数据，且难以消除残留触发效应导致攻击成功率仍高。
method: 提出BeDKD，用定向映射模块识别中毒数据，并结合对抗知识蒸馏削弱后门映射。
result: 仅用少量干净与中毒数据即可在降低攻击成功率的同时保持模型性能。
conclusion: 该方法在数据受限场景下实现了防御效果与模型性能的良好平衡。
---

## Abstract
Although existing backdoor defenses have gained success in mitigating backdoor attacks, they still face substantial challenges. In particular, most of them rely on large amounts of clean data to weaken the backdoor mapping but generally struggle with residual trigger effects, resulting in persistently high attack success rates (ASR). Therefore, in this paper, we propose a novel Backdoor defense method based on Directional mapping module and adversarial Knowledge Distillation (BeDKD), which balances the trade-off between defense effectiveness and model performance using a small amount of clean and poisoned data. We first introduce a directional mapping module to identify poisoned data, which destroys clean mapping while keeping backdoor mapping on a small set of flipped clean data. Then, the adversarial knowledge distillation is designed to reinforce clean mapping and suppress backdoor mapping through a cycle iteration mechanism between trust and punish distillations using clean and identified poisoned data. We conduct experiments to mitigate mainstream attacks on three datasets, and experimental results demonstrate that BeDKD surpasses the state-of-the-art defenses and reduces the ASR by 98% without significantly reducing the CACC.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：深度神经网络在 NLP 任务中广泛应用，但易受后门攻击。攻击者在少量训练样本中植入触发器，使模型在正常输入上表现正常，而在含触发器输入上被诱导至目标标签。
- **现有防御的痛点**：
  - 数据级防御多用于识别或移除中毒样本，主要避免激活后门，而非彻底消除模型中的后门映射。
  - 模型级防御虽能擦除后门，但常依赖大量干净数据，且容易残留触发器效应，导致攻击成功率（ASR）仍偏高。
  - 防御效果与模型干净准确率（CACC）之间存在明显权衡，低资源场景下实用性不足。
- **论文整体含义**：作者提出 **BeDKD**，试图用少量干净数据和少量中毒数据实现数据高效的后门防御：先识别中毒样本，再通过“信任—惩罚”交替的知识蒸馏强化干净映射、抑制后门映射，在降低 ASR 的同时尽量保持 CACC。

## 2. 方法论

### 2.1 核心思想

- 将后门模型的行为拆分为两类映射：
  - **干净映射**：干净输入语义与真实标签之间的对应关系。
  - **后门映射**：触发器模式与目标标签之间的对应关系。
- 防御目标不是仅用干净数据“弱化”后门，而是利用识别出的少量中毒数据主动破坏后门映射，同时保留干净映射。
- 整体流程包括三步：
  1. **定向映射模块（DMM）蒸馏**：构造一个与中毒模型同架构、同初始参数的模块，在小规模“标签翻转”的干净数据上微调，破坏其干净映射但尽量保留后门映射。
  2. **中毒数据识别**：比较中毒模型与 DMM 在样本上的概率分布差异，识别可信中毒样本。
  3. **对抗知识蒸馏（AKD）**：以中毒模型为教师、同架构模型为学生，交替进行信任蒸馏与惩罚蒸馏，最终得到后门被抑制的干净模型。

### 2.2 定向映射模块 DMM

- DMM 复制中毒模型 \(f_{\theta^*}\) 的架构与参数。
- 使用少量干净数据 \(D_c^{few}\)，将其真实标签 \(y\) 翻转为错误标签 \(y'\)，得到翻转数据 \(D_c^{few'}\)。
- **硬损失**：让 DMM 在翻转数据上学习错误标签，破坏干净映射：
  - 交叉熵损失 \(L_{hard}=-\sum y'\log(DMM(x))\)。
- **软损失**：通过知识蒸馏约束 DMM 与中毒教师模型的输出分布和隐藏特征，尽量保留后门映射：
  - KL 散度对齐 softmax 输出；
  - MSE 对齐最后一层隐藏状态。
- **总损失**：
  - \(L_{DMM}=\alpha L_{hard}+(1-\alpha)L_{soft}\)，其中 \(\alpha=0.3\)，温度 \(T=1.5\)。
- 直觉：DMM 在干净样本上的输出分布会偏离中毒模型，而在含触发器样本上仍接近中毒模型，从而形成可区分信号。

### 2.3 中毒数据识别

- 对每个样本计算中毒模型与 DMM 的概率分布平均绝对误差：
  - \(MEPD=\frac{\sum_y |f_{\theta^*}(x,y)-DMM(x,y)|}{|Y|}\)。
- 若 \(MEPD<\gamma\)，判为中毒样本；否则判为干净样本。
- 阈值 \(\gamma\) 通过少量干净数据确定。实验中 \(\gamma=0.1\) 取得较好 FAR/FRR 平衡，推荐范围约 \(0.05\sim0.25\)。

### 2.4 对抗知识蒸馏 AKD

- 教师模型为冻结参数的中毒模型 \(f_{\theta^*}\)；学生模型 \(CM\) 与教师同架构、同初始参数。
- 采用**循环迭代机制**，交替执行：
  - **信任蒸馏（trust distillation）**：在少量干净数据上，让学生模型学习教师的干净映射。损失为 \(L_{trust}=\lambda L_{hard}+(1-\lambda)L_{soft}\)。
  - **惩罚蒸馏（punish distillation）**：在识别出的少量中毒数据上，使用负向损失 \(L_{penalty}=-(\lambda L_{hard}+(1-\lambda)L_{soft})\)，推动学生模型远离教师的后门映射。
- 优化目标为在干净数据上最小化信任损失、在中毒数据上最小化惩罚损失：
  - \(\tilde{\theta}^*=\arg\min_{\theta^*}\{E_{D_c^{few}}[L_{trust}]+E_{D_p^{few*}}[L_{penalty}]\}\)。
- 超参数：\(\lambda=0.3\)，AKD 温度 \(T=2.5\)。
- 训练设置：中毒 BERT 用 AdamW、学习率 \(3\times10^{-5}\) 训练 10 轮；DMM 训练 20 轮；AKD 训练 50 轮。

## 3. 实验设计

- **数据集**：
  - SST2：情感分类；
  - OLID：攻击性语言检测；
  - AGnews：新闻分类。
- **后门攻击**：6 种主流文本后门攻击：
  - AddSent、BadWords、SynBkd、StyBkd、AttrBkd、BGMAttack。
  - 覆盖插入式可见触发器、句法/风格/属性等不可见触发器以及 AI 生成文本触发。
- **对比基线**：
  - 无防御（No Defense）；
  - Fine-Tuning（FT）；
  - ONION；
  - IMBERT；
  - TextGuard；
  - W2SDefense。
- **评价指标**：
  - ASR：中毒测试集上的攻击成功率，越低越好；
  - CACC：干净测试集准确率，越高越好；
  - FAR：中毒样本被误判为干净的比例；
  - FRR：干净样本被误判为中毒的比例。
- **实验类型**：
  - 主实验：3 个数据集 × 6 种攻击 × 多种防御方法；
  - 消融实验：DMM、AKD 的贡献，以及 DMM 中硬损失/软损失的作用；
  - 敏感性分析：干净样本数 \(n_c\)、中毒样本数 \(n_p\)、阈值 \(\gamma\)、中毒率 \(r\)；
  - 跨域实验：使用 HateSpeech 和 GPT-4o 生成的 AI 文本作为代理数据；
  - 效率对比：SST2 上防御耗时。

## 4. 资源与算力

- 论文**未明确报告** GPU 型号、GPU 数量、总训练时长、显存消耗或能耗等算力信息。
- 文中仅给出部分训练与效率信息：
  - 中毒 BERT 训练 10 轮，DMM 20 轮，AKD 50 轮；
  - 在 SST2 上 BeDKD 完成防御约需 **232 秒**；
  - 对比 W2SDefense 超过 **4000 秒**、TextGuard 超过 **30000 秒**。
- 因此，方法效率有相对比较，但缺少硬件条件说明，复现和能耗评估受限。

## 5. 实验数量与充分性

- **实验规模**：
  - 主表覆盖 3 个数据集、6 种攻击、5 个基线加无防御设置，规模较大；
  - 包含 DMM/AKD 消融、损失函数消融、\(n_c/n_p\) 敏感性、阈值 \(\gamma\) 敏感性、中毒率 \(r\) 敏感性、跨域数据实验；
  - 图 3、图 4 还展示了不同攻击下随中毒数据规模和阈值变化的 ASR/CACC、FAR/FRR 曲线。
- **充分性**：
  - 攻击类型覆盖较广，既有可见触发器也有不可见/生成式触发器；
  - 同时报告 ASR 与 CACC，并补充 FAR/FRR，能较全面反映防御效果；
  - 消融实验较系统地验证了 DMM、AKD 和软损失的作用；
  - 跨域与低中毒率实验增强了实用性论证。
- **客观性与公平性**：
  - 沿用常见指标和基线，主表标注最优与次优，整体比较框架较清晰；
  - 但**未报告多次随机种子实验的均值、方差或显著性检验**，统计稳健性证据不足；
  - 部分基线可能未针对所有数据集充分调参，尤其在 OLID 小规模数据上表现较差，比较公平性仍需谨慎；
  - 效率比较未说明硬件环境，绝对时间对比的解释力有限；
  - 敏感性分析主要集中在 SST2/OLID，跨数据集超参泛化验证仍不充分。

## 6. 主要结论与发现

- BeDKD 在多数攻击和数据集上显著优于现有防御，可将 ASR 降低约 **98%**，同时 CACC 下降不显著。
- 主实验结果：
  - SST2 平均 ASR 约 **1.79%**，CACC 约 **90.60%**；
  - OLID 平均 ASR 约 **1.27%**，CACC 约 **81.97%**；
  - AGnews 平均 ASR 约 **0.81%**，CACC 约 **93.52%**。
- 相比最强基线 W2SDefense，BeDKD 在平均 ASR 和 CACC 上整体更优，尤其在 OLID 小数据场景下优势明显。
- 消融表明：
  - 仅用 FT 或普通 KD 会残留较高 ASR；
  - 加入 DMM 可明显降低 ASR；
  - 联合 DMM 与 AKD 后 ASR 降至最低，CACC 仅小幅下降。
- 敏感性分析表明：
  - \(n_c=320\)、\(n_p=32\) 时在 SST2 上取得较好的 ASR/CACC 平衡；
  - \(\gamma=0.1\) 时 FAR/FRR 较平衡；
  - 在 5% 和 10% 中毒率下仍能有效防御；
  - 跨域代理数据下仍保持较强防御效果，说明一定泛化性。
- 效率上，BeDKD 在 SST2 上约 232 秒完成防御，明显快于 W2SDefense 和 TextGuard。

## 7. 优点

- **数据高效**：仅需少量干净数据和少量识别出的中毒数据，适合低资源场景。
- **思路新颖**：不同于仅用干净数据削弱后门，明确利用中毒数据执行“惩罚蒸馏”以主动破坏后门映射。
- **DMM 设计巧妙**：通过标签翻转破坏干净映射、通过蒸馏保留后门映射，放大两类样本的分布差异，从而识别中毒数据。
- **AKD 机制清晰**：信任蒸馏与惩罚蒸馏交替，兼顾干净映射保持和后门映射抑制。
- **实验覆盖较广**：涵盖 3 个数据集、6 种攻击、多种基线，并包含消融、敏感性、跨域和效率分析。
- **实用性强**：在 OLID 等小规模数据上仍有效，且防御耗时明显低于部分基线。

## 8. 不足与局限

- **模型与任务范围有限**：实验主要基于 BERT 文本分类，未验证生成式大语言模型

、多模态模型以及生成式/序列标注等任务；对生成式大语言模型中的后门防御、参数高效微调场景和更复杂触发器仍缺乏验证。

- **对干净数据与阈值仍有一定依赖**：方法虽强调数据高效，但仍需少量可信干净数据用于 DMM 训练、阈值 \(\gamma\) 校准和信任蒸馏；若干净数据受污染、领域偏移明显或规模进一步压缩，中毒识别与最终防御性能可能下降。
- **中毒识别假设较强**：DMM 的可分性依赖“标签翻转破坏干净映射、蒸馏保留后门映射”这一假设。若触发器较弱、后门映射与干净映射高度纠缠，或攻击者采用自适应攻击刻意规避 DMM，则 MEPD 的区分能力可能减弱；FAR/FRR 的非零误判也会传播到后续 AKD。
- **负向蒸馏的稳定性与理论保障不足**：惩罚蒸馏使用负损失推动学生远离教师的后门映射，虽有效但可能带来优化不稳定、表示退化或 CACC 波动；论文未给出收敛性分析，超参数 \(\lambda\)、温度 \(T\)、训练轮数对稳定性的影响仍需更系统验证。
- **统计稳健性与公平性证据不足**：缺少多次随机种子实验的均值、方差和显著性检验；部分基线在 OLID 等小数据上表现较差，是否充分调参不够明确；效率比较未报告硬件环境，绝对耗时对比的解释力有限。
- **攻击与场景覆盖仍可扩展**：主要考察单触发器、单目标标签的文本分类后门，未充分讨论多触发器、多目标后门、持续学习/联邦学习场景、后门与对抗样本联合攻击，以及防御后残留后门的长期安全性。
- **可解释性与部署成本**：DMM 与 AKD 均需同架构模型和额外训练轮次，虽比部分基线快，但在极端低资源或大模型场景下的显存、时间和工程复杂度仍需评估；方法也缺少对识别结果和蒸馏过程的可解释性分析。

## 9. 总结性评价

BeDKD 的核心贡献在于把“中毒数据”从单纯需要剔除的对象转化为防御资源：通过 DMM 放大中毒样本与干净样本的分布差异，再用信任/惩罚交替蒸馏同时保持干净映射、破坏后门映射。其数据高效性和在多种文本后门攻击上的低 ASR 表现具有较强实用价值，实验设计也较完整。但该方法仍依赖少量干净数据和可分的后门假设，统计稳健性、自适应攻击鲁棒性、跨模型/跨任务泛化以及负向蒸馏稳定性方面仍有提升空间。总体而言，这是一项思路清晰、实验较扎实的防御工作，适合低资源文本分类场景，但距离通用、可证明安全的后门防御仍有距离。

（完）
