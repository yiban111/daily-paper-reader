---
title: "FlowMur: A Stealthy and Practical Audio Backdoor Attack with Limited Knowledge"
title_zh: FlowMur：一种知识受限的隐蔽实用音频后门攻击
authors: "Jiahe Lan, Jie Wang, Baochen Yan, Zheng Yan, Elisa Bertino"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.computer.org/csdl/pds/api/csdl/proceedings/download-article/1Ub245RZpo4/pdf"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 隐蔽且实用的音频后门攻击
tldr: 现有音频后门攻击普遍依赖充分的攻击知识与资源，实用性受限。本文提出FlowMur，一种在有限知识下即可实施的隐蔽音频后门攻击框架。实验表明其能在不影响正常语音识别性能的前提下有效植入并触发后门。该工作提升了音频后门攻击的现实可行性与隐蔽性。
source: IEEE-SP-2024-CSDL
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-001.webp\", \"caption\": \"\", \"page\": 5, \"index\": 1, \"width\": 3396, \"height\": 660}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-002.webp\", \"caption\": \"\", \"page\": 8, \"index\": 2, \"width\": 386, \"height\": 386}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-003.webp\", \"caption\": \"\", \"page\": 8, \"index\": 3, \"width\": 379, \"height\": 379}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-004.webp\", \"caption\": \"\", \"page\": 12, \"index\": 4, \"width\": 2751, \"height\": 2065}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-005.webp\", \"caption\": \"\", \"page\": 18, \"index\": 5, \"width\": 620, \"height\": 462}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-006.webp\", \"caption\": \"\", \"page\": 18, \"index\": 6, \"width\": 620, \"height\": 462}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-007.webp\", \"caption\": \"\", \"page\": 18, \"index\": 7, \"width\": 620, \"height\": 462}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-008.webp\", \"caption\": \"\", \"page\": 18, \"index\": 8, \"width\": 620, \"height\": 462}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-009.webp\", \"caption\": \"\", \"page\": 18, \"index\": 9, \"width\": 620, \"height\": 462}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-010.webp\", \"caption\": \"\", \"page\": 18, \"index\": 10, \"width\": 620, \"height\": 462}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-011.webp\", \"caption\": \"\", \"page\": 18, \"index\": 11, \"width\": 620, \"height\": 462}, {\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-flowmur-a-stealthy-and-practical-audio-backdoor-attack-with-limited-knowledge/fig-012.webp\", \"caption\": \"\", \"page\": 18, \"index\": 12, \"width\": 620, \"height\": 462}]"
motivation: 现有音频后门攻击大多需要充足的攻击知识，限制了其现实可行性。
method: 提出FlowMur框架，在有限知识条件下构造隐蔽触发器并植入语音识别模型。
result: 实验表明该方法在不损害良性输入性能的同时实现了高效的后门触发。
conclusion: 该工作显著提升了音频后门攻击的实用性与隐蔽性。
---

## Abstract
Speech recognition systems driven by Deep Neural Networks (DNNs) have revolutionized human-computer interaction through voice interfaces, which significantly facilitate our daily lives. However, the growing popularity of these systems also raises special concerns on their security, particularly regarding backdoor attacks. A backdoor attack inserts one or more hidden backdoors into a DNN model during its training process, such that it does not affect the model’s performance on benign inputs, but forces the model to produce an adversary-desired output if a specific trigger is present in the model input. Despite the initial success of current audio backdoor attacks, they suffer from the following limitations: (i) Most of them require sufficient knowledge, which limits their widespread adoption. (ii) They are not stealthy enough, thus easy to be detected by humans. (iii) Most of them cannot attack live speech, reducing their practicality. To address these problems, in this paper, we propose FlowMur, a stealthy and practical audio backdoor attack that can be launched with limited knowledge. FlowMur constructs an auxiliary dataset and a surrogate model to augment adversary knowledge. To achieve dynamicity, it formulates trigger generation as an optimization problem and optimizes the trigger over different attachment positions. To enhance stealthiness, we propose an adaptive data poisoning method according to Signal-to-Noise Ratio (SNR). Furthermore, ambient noise is incorporated into the process of trigger generation and data poisoning to make FlowMur robust to ambient noise and improve its practicality. Extensive experiments conducted on two datasets demonstrate that FlowMur achieves high attack performance in both digital and physical settings while remaining resilient to state-of- the-art defenses. In particular, a human study confirms that triggers generated by FlowMur are not easily detected by participants. The source code of FlowMur is publicly available at https://github.com/cristinalan/FlowMur.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：DNN 语音识别系统已广泛用于语音助手、智能设备等场景，但其训练依赖大量数据和算力，用户常使用第三方数据集、模型或训练平台，因而引入后门攻击风险。
- **后门攻击含义**：攻击者在训练数据中植入触发器，使模型在良性输入上保持正常性能，但在输入包含特定触发器时输出攻击者指定标签。
- **现有音频后门攻击的不足**：
  - 多数方法要求攻击者掌握目标模型或完整受害数据集等充分知识，部署门槛高。
  - 隐蔽性不足，触发器易被人类察觉。
  - 多数方法为静态攻击，触发器固定在音频固定位置，难以攻击实时人类语音。
  - 对先进音频后门防御的抵抗性研究不足。
- **论文目标**：提出 **FlowMur**，一种仅需有限知识即可实施的隐蔽、实用音频后门攻击，兼顾攻击有效性、动态性、物理世界可行性、人类不可感知性和防御抵抗性。

## 2. 方法论

- **核心思想**：在攻击者仅能访问受害数据集中目标类样本、无法访问目标模型和其他样本的严格威胁模型下，通过辅助数据集和代理模型补足知识，优化生成通用触发器，并通过自适应投毒和噪声增强提升隐蔽性与物理鲁棒性。
- **威胁模型**：
  - 攻击者仅掌握目标类样本，不知道目标模型架构及参数，也不知道其他类别样本。
  - 可修改目标类训练样本但尽量不改变标签，以保持标签一致性。
  - 可在测试阶段通过扬声器等播放触发器。
  - 具备一定计算资源，可收集与目标任务相关的辅助数据并训练代理模型。
- **四阶段流程**：
  1. **预热准备**：收集与目标学习任务相关的辅助数据集 \(D_{aux}\)，与目标类数据集 \(D_t\) 合并为代理数据集 \(D_{sur}\)；训练代理模型 \(f_{\theta_{sur}}\)。
  2. **触发器生成**：将触发器生成建模为优化问题，在代理数据集和代理模型上最小化中毒样本被预测为目标标签的损失；对触发器可附加的多个位置进行优化，而非固定单一位置。
  3. **数据投毒**：按目标类投毒率污染目标类样本，并为每个样本随机选择触发器位置，使模型学习与位置无关的触发器特征。
  4. **后门注入**：用户使用被污染数据集训练模型时自然完成后门注入；感染模型在良性样本上保持正常，在含触发器样本上输出目标标签。
- **关键算法流程（文字说明）**：
  - 随机初始化短时、低幅触发器 \(\delta\)。
  - 对代理数据集中的样本随机选择附加位置 \(\tau\)，生成中毒样本 \(x'=\mathrm{Add}(x,\delta,\tau)\)。
  - 将 \(x'\) 裁剪到 \([-1,1]\) 以保证音频有效。
  - 用交叉熵损失和 Adam 优化器更新触发器。
  - 将触发器裁剪到 \([-\epsilon,\epsilon]\) 以限制幅度、提升不可听性。
  - 多次迭代后得到动态、微小触发器。
- **自适应投毒**：
  - 根据目标信噪比 SNR 为不同音频样本分配不同缩放比例的触发器，而非对所有样本使用同一强度。
  - 缩放因子由样本功率、触发器功率和期望 SNR 共同决定，公式为 \(s=10^{-\mathrm{SNR}/20}\cdot \|x\|_2/\|\delta\|_2\)。
  - 该策略使触发器在嘈杂样本中可适当增强，在安静样本中保持微弱，从而提升整体不可感知性。
- **物理鲁棒性增强**：
  - 在触发器生成阶段加入环境噪声，使优化出的触发器对噪声更稳健。
  - 在投毒数据中也加入环境噪声，使感染模型在物理场景下更鲁棒。
- **隐蔽性策略**：
  - 仅投毒目标类样本并保持标签不变，满足标签一致性。
  - 约束触发器时长和幅度。
  - 通过 SNR 自适应缩放提升触发器不可听性。
  - 随机化触发器位置以增强动态性。

## 3. 实验设计

- **主要数据集**：
  - **SCD（Speech Command Dataset）**：35 类英语语音命令，约 96,783 个样本；随机选 10 类为受害数据集，其余 25 类为辅助数据集。
  - **FKD（Football Keywords Dataset）**：18 类波斯语足球关键词，约 30,259 个样本；8 类为受害数据集，10 类为辅助数据集。
- **扩展数据集**：
  - **FSC（Fluent Speech Commands）**：用于长输入语音命令场景，数据增强后 51,624 个样本，8 类受害、10 类辅助。
  - **VCTK 说话人识别数据**：30 个说话人类别，10 类受害、20 类辅助，用于验证方法可迁移到说话人识别。
- **目标模型**：SmallCNN、LargeCNN、ResNet、RNN；默认目标模型为 LargeCNN，默认代理模型为 SmallCNN。
- **特征与训练设置**：使用 MFCC 特征，采样率 16,000，13 维 MFCC，n_fft=2048，hop length=512；默认数据划分为 64% 训练、16% 验证、20% 测试。
- **评价指标**：
  - **BA（Benign Accuracy）**：感染模型对良性样本的分类准确率。
  - **ASR（Attack Success Rate）**：感染模型将非目标类中毒样本误判为目标类的比例。
- **对比方法**：
  - **NBA**：自然后门攻击，使用哨声等环境声作为指定触发器。
  - **NBA-D**：论文将 NBA 扩展为动态版本，在投毒时随机选择触发器位置。
  - 未在实验中直接复现文献 [10] 的动态优化攻击，主要因其威胁模型要求更强攻击者知识。
- **主要实验组**：
  - 两个数据集上的整体 BA/ASR 对比。
  - 类别级 ASR 评估。
  - 目标类投毒率影响：10%–100%。
  - 触发器时长影响。
  - 16 种代理模型—目标模型组合。
  - SNR 影响：10–50 dB。
  - 攻击者与受害者数据类别比例变化。
  - 物理攻击：录制音频回放、真人实时语音。
  - 人类研究：20 名参与者，评估触发器不可感知性。
  - 防御实验：滤波器、fine-pruning、STRIP、Beatrix。
  - 附录：长语音命令、说话人识别、触发器音量影响。

## 4. 资源与算力

- 论文明确提到使用 **PyTorch**，并在配备 **Tesla V100 GPU** 的服务器上实现实验。
- 文中未明确说明 GPU 数量、总训练时长、总实验时长或总算力消耗。
- 触发器默认优化 1000 个 epoch，学习率 0.0001，使用 Adam 优化器；但未给出单次训练耗时。
- 因此，算力信息仅能确认 GPU 型号，无法准确估算总资源规模。

## 5. 实验数量与充分性

- **实验数量较多**，覆盖：
  - 2 个主数据集 × 4 个模型；
  - 2 个基线方法；
  - 多组参数消融：投毒率、触发器时长、SNR、数据平衡；
  - 16 种代理—目标模型组合；
  - 数字场景与物理场景；
  - 录制音频与真人实时语音；
  - 不同距离、不同设备；
  - 4 种防御方法；
  - 人类感知研究；
  - 附录中的长输入、说话人识别和触发器音量实验。
- **充分性**：
  - 从攻击有效性、动态性、物理可行性、隐蔽性和防御抵抗性五个维度看，实验覆盖较全面。
  - 使用配对 t 检验报告显著性，类别级 ASR 也提供了细粒度结果。
  - 物理攻击同时考虑安静实验室和嘈杂餐厅，并测试设备与距离影响，增强了外部有效性。
- **客观性与公平性**：
  - 基线与 FlowMur 在相同数据集、模型和评价指标下比较，默认参数设置较一致。
  - 但基线仅包括 NBA 和 NBA-D，未与要求更强知识的动态优化攻击 [10] 直接实验对比。
  - 人类研究样本为 20 人，物理攻击真人为 6 人，规模仍有限，可能存在参与者偏差。
  - 防御实验覆盖四种代表性方法，但未覆盖自适应防御或专门针对动态优化触发器的检测方法。

## 6. 主要结论与发现

- **攻击有效性高**：
  - SCD 上 BA 约 97.24%，ASR 达 99.83%。
  - FKD 上 BA 约 97.75%，ASR 达 98.29%。
  - 相比最佳基线 NBA-D，ASR 分别提升约 43.18% 和 41.96%，且统计显著。
  - 类别级评估中大多数实际类—目标类组合 ASR 超过 95%。
- **动态性有效**：随机化触发器位置并优化多位置损失，使模型学习位置无关特征，优于静态 NBA 和动态化基线 NBA-D。
- **物理场景可行**：
  - 1 米距离下，安静实验室 ASR 超过 80%，嘈杂餐厅超过 60%。
  - 真人实时语音攻击中，6 名参与者均达到 BA 超过 90%、ASR 超过 79%。
- **隐蔽性较好**：
  - 人类研究中，68% 参与者认为 FlowMur 中毒样本是“干净”的，接近良性样本基线 78.4%。
  - 30 dB 和 40 dB 设置下触发器较难被人察觉；20 dB 时较易被察觉。
  - NBA 和 NBA-D 的中毒样本仅约 19.5% 和 18% 被认为干净。
- **防御抵抗性较强但非绝对**：
  - 滤波器会降低 ASR，但也会显著损害 BA，存在防御代价。
  - fine-pruning 对 FlowMur 效果有限，剪枝超过 50% 时 BA 先下降。
  - STRIP 难以区分 FlowMur 的良性样本与中毒样本。
  - Beatrix 在 FKD 上能检测 FlowMur，但在 SCD 上效果不佳，表现不稳定。
- **环境噪声建模有效**：在触发器生成和投毒中加入环境噪声，可同时提升物理攻击 ASR 和 BA 鲁棒性。

## 7. 优点

- **威胁模型严格且现实**：仅要求目标类样本，不要求目标模型和完整数据集，较贴近实际攻击者能力。
- **方法设计有针对性**：
  - 用辅助数据和代理模型解决知识不足问题；
  - 将触发器生成形式化为多位置优化问题，实现动态攻击；
  - 用 SNR 自适应投毒提升不可听性；
  - 将环境噪声纳入优化与投毒，提升物理世界鲁棒性。
- **实验维度丰富**：同时覆盖数字攻击、物理回放、真人语音、人类感知研究和多种防御，评估较全面。
- **统计与细粒度分析较好**：报告 p 值、类别级 ASR、16 种代理—目标组合和多种参数消融。
- **开源代码**：提供公开代码，有利于复现和后续防御研究。
- **附录扩展性验证**：在长语音命令和说话人识别任务上进一步验证方法可迁移性。

## 8. 不足与局限

- **攻击对象受限**：主要面向语音命令识别系统，尚未有效覆盖更复杂的自动语音识别系统；作者也承认对 ASR 的攻击受计算资源和模型复杂性限制。
- **依赖辅助数据与代理模型**：需要收集与目标任务相关的辅助数据并训练代理模型；若辅助数据与目标分布差异大，攻击效果可能下降。
- **代理—目标模型差异影响性能**：RNN 作为代理或目标模型时性能相对较差，说明方法对模型架构差异仍敏感。
- **物理实验规模有限**：真人实验仅 6 名参与者，物理环境仅实验室和餐厅，设备组合有限；距离超过 1 米或在更嘈杂环境下性能明显下降。
- **人类研究样本有限**：20 名参与者、每任务 2000 个回答，虽提供有价值结果，但难以完全代表真实用户群体。
- **防御抵抗并非绝对**：Beatrix 在 FKD

上能够检测 FlowMur，但在 SCD 上效果不佳，说明其检测能力对数据集与触发器分布较敏感；因此“防御抵抗性较强”应限定在已测试防御和设置内，而非对所有先进防御普遍免疫。

- **未系统评估自适应防御**：防御实验覆盖滤波器、fine-pruning、STRIP、Beatrix 等代表性方法，但未针对 FlowMur 的“目标类投毒、标签一致、动态位置、低幅短触发器”等特性设计自适应检测或净化策略，防御博弈空间仍不完整。
- **跨语言、跨说话人与跨域泛化讨论不足**：主实验覆盖英语命令、波斯语关键词、长语音命令和说话人识别，但对辅助数据与受害数据分布差异、说话人变化、语言迁移等条件下的性能边界缺少系统分析。
- **计算开销与可复现细节不完整**：虽给出 PyTorch、Tesla V100、触发器优化 1000 epoch、学习率 0.0001 等设置，但未报告 GPU 数量、总训练时长、触发器生成耗时及不同模型规模下的成本，难以评估实际攻击门槛。
- **统计报告仍可加强**：使用配对 t 检验并报告类别级 ASR，但未充分展示多次随机种子下的均值—方差、置信区间或不同随机划分的稳定性，部分结论的稳健性仍可进一步量化。
- **物理攻击边界条件有限**：已测试实验室、餐厅、设备与 1 米距离，但对更远距离、强混响、多人同时说话、不同麦克风频响等真实复杂声学条件缺少系统刻画。
- **伦理与负责任披露讨论较少**：论文属于安全研究，虽提出攻击方法并开源代码，但对潜在滥用风险、负责任披露流程、防御方部署建议等讨论相对有限。

## 9. 总体评价与启示（补充）

- **总体判断**：FlowMur 的核心贡献在于把“有限知识威胁模型”下的音频后门攻击做成较完整的工程化流程：用辅助数据与代理模型补足知识，用多位置优化实现动态触发器，用 SNR 自适应缩放提升不可听性，并用环境噪声增强物理鲁棒性。其方法设计与实验组织具有较强针对性。
- **研究意义**：该工作说明，即使攻击者不掌握目标模型和完整受害数据集，仍可能通过污染目标类样本实施有效后门攻击；这对第三方数据、预训练模型和训练平台供应链安全具有警示意义。
- **对防御研究的启示**：防御方不能仅依赖单一滤波器或剪枝方法，需要关注标签一致性投毒、低幅短触发器、动态位置触发器和物理回放场景；跨数据集、跨模型和自适应检测能力应是后续防御评估重点。
- **可复现性与开放性**：论文提供公开代码，主要超参数和实验设置较清晰，有利于复现；但算力细节、随机种子方差和部分物理实验条件仍可补充。
- **适用边界**：当前结论主要适用于语音命令识别和说话人识别等分类任务，对更复杂 ASR 系统、开放环境长语音和强自适应防御的有效性仍需进一步验证。

（完）
