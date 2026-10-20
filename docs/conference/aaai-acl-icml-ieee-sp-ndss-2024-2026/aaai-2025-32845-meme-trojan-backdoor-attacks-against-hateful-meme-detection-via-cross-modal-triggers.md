---
title: "Meme Trojan: Backdoor Attacks Against Hateful Meme Detection via Cross-Modal Triggers"
title_zh: Meme Trojan：通过跨模态触发器对仇恨模因检测实施后门攻击
authors: "Ruofei Wang, Hongzhan Lin, Ziyuan Luo, Ka Chun Cheung, Simon See, Jing Ma, Renjie Wan"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32845/35000"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 面向仇恨模因检测的跨模态触发器后门攻击
tldr: 仇恨模因检测对社交媒体环境至关重要，但其后门威胁此前被忽视。本文提出Meme Trojan框架，设计跨模态触发器并结合可学习触发器增强器，根据每个输入自适应调整触发模式。实验表明该攻击能有效操控检测器输出，揭示了多模态内容审核系统面临的后门风险。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 550, \"height\": 412}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-002.webp\", \"caption\": \"\", \"page\": 2, \"index\": 2, \"width\": 825, \"height\": 534}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-003.webp\", \"caption\": \"\", \"page\": 2, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-004.webp\", \"caption\": \"\", \"page\": 2, \"index\": 4, \"width\": 825, \"height\": 549}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-005.webp\", \"caption\": \"\", \"page\": 2, \"index\": 5, \"width\": 825, \"height\": 534}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-006.webp\", \"caption\": \"\", \"page\": 2, \"index\": 6, \"width\": 550, \"height\": 412}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-007.webp\", \"caption\": \"\", \"page\": 2, \"index\": 7, \"width\": 550, \"height\": 366}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-008.webp\", \"caption\": \"\", \"page\": 2, \"index\": 8, \"width\": 550, \"height\": 366}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-009.webp\", \"caption\": \"\", \"page\": 2, \"index\": 9, \"width\": 550, \"height\": 313}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-010.webp\", \"caption\": \"\", \"page\": 2, \"index\": 10, \"width\": 550, \"height\": 313}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-011.webp\", \"caption\": \"\", \"page\": 4, \"index\": 11, \"width\": 550, \"height\": 398}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-012.webp\", \"caption\": \"\", \"page\": 4, \"index\": 12, \"width\": 800, \"height\": 549}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-013.webp\", \"caption\": \"\", \"page\": 4, \"index\": 13, \"width\": 800, \"height\": 485}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-014.webp\", \"caption\": \"\", \"page\": 4, \"index\": 14, \"width\": 550, \"height\": 355}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-015.webp\", \"caption\": \"\", \"page\": 4, \"index\": 15, \"width\": 550, \"height\": 366}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-016.webp\", \"caption\": \"\", \"page\": 4, \"index\": 16, \"width\": 550, \"height\": 364}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-017.webp\", \"caption\": \"\", \"page\": 4, \"index\": 17, \"width\": 796, \"height\": 530}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-018.webp\", \"caption\": \"\", \"page\": 4, \"index\": 18, \"width\": 796, \"height\": 530}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-019.webp\", \"caption\": \"\", \"page\": 4, \"index\": 19, \"width\": 800, \"height\": 549}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32845/fig-020.webp\", \"caption\": \"\", \"page\": 4, \"index\": 20, \"width\": 800, \"height\": 549}]"
motivation: 仇恨模因检测系统存在被忽视的后门威胁，可能被攻击者操控输出。
method: 提出跨模态触发器与可学习触发器增强器，按输入自适应调整触发模式。
result: 实验显示该方法能有效诱导检测器输出攻击者期望的结果。
conclusion: 多模态内容审核系统同样面临现实可行的后门攻击威胁。
---

## Abstract
Hateful meme detection aims to prevent the proliferation of hateful memes on various social media platforms. Considering its impact on social environments, this paper introduces a previously ignored but significant threat to hateful meme detection: backdoor attacks. By injecting specific triggers into meme samples, backdoor attackers can manipulate the detector to output their desired outcomes. To explore this, we propose the Meme Trojan framework to initiate backdoor attacks on hateful meme detection. Meme Trojan involves creating a novel Cross-Modal Trigger (CMT) and a learnable trigger augmentor to enhance the trigger pattern according to each input sample. Due to the cross-modal property, the proposed CMT can effectively initiate backdoor attacks on hateful meme detectors under an automatic application scenario. Additionally, the injection position and size of our triggers are adaptive to the texts contained in the meme, which ensures that the trigger is seamlessly integrated with the meme content. Our approach outperforms the state-of-the-art backdoor attack methods, showing significant improvements in effectiveness and stealthiness. We believe that this paper will draw more attention to the potential threat posed by backdoor attacks on hateful meme detection.

---

## 论文详细总结（自动生成）

# Meme Trojan 论文中文结构化总结

## 1. 核心问题与整体含义

- **研究背景**：仇恨模因（hateful meme）由图像与嵌入图像中的短文本共同构成，常被用于传播种族、性别、宗教等歧视内容。仇恨模因检测系统被部署于社交媒体内容审核中，以阻止有害内容扩散。
- **核心问题**：现有研究多关注检测精度或简单对抗样本，但忽视了训练阶段的**后门攻击**威胁。攻击者可通过污染第三方训练数据，在模型中植入后门；推理时只要输入含特定触发器的模因，就能让检测器把仇恨模因判为非仇恨，从而绕过审核。
- **整体含义**：论文首次系统地将后门攻击引入仇恨模因检测场景，揭示多模态内容审核系统面临现实且隐蔽的安全风险。其威胁模型较贴近实际：攻击者无法控制模型训练细节，只能访问部分训练数据；推理阶段通常只能提交模因图像，不能手动向检测器输入文本。

## 2. 方法论

### 2.1 核心思想

- 利用模因的独特属性：**文本与图像共存，文本是两种模态之间的共享元素**。
- 设计一种**文本样触发器（text-like trigger）**，只注入图像模态；它既能直接影响图像编码器，又能被自动文本抽取器（OCR）识别为文本，从而同时攻击文本编码器，形成跨模态后门。
- 触发器采用“..”形式，体积小、表达形式较自然，并注入到模因图像中文本的末尾附近，以保持图文一致性并提高隐蔽性。

### 2.2 关键技术细节

- **触发器模式**：选择“..”而非单词、随机块或红色图案，原因是像素占用少、不改变模因语义，且比单点或三点省略号更不易与正常标点混淆。
- **自适应注入**：
  - 使用文本抽取器获得图像中文本的边界框；
  - 通过 `Select(·)` 函数选择注入坐标 `(x, y)` 和触发器尺寸 `(w, h)`；
  - 使触发器位置和大小随模因文本自适应，尽量不显眼且与文本内容融合。
- **跨模态传递**：
  - 攻击者只修改图像，得到毒化图像 `v_p`；
  - 文本抽取器 `R` 从毒化图像中识别出毒化文本 `t_p`，即 `t_p = R(v_p)`；
  - 因此文本触发器无需人工输入，适用于自动检测流水线。
- **触发器增强器（Trigger Augmentor, TA）**：
  - 先按初始“..”模式生成毒化模因；
  - 将毒化样本与干净数据结合，训练一个深度分类器，使其提取能区分毒化样本的特征；
  - 再用 blending 策略将提取到的毒化语义特征与初始触发器融合，得到最终**跨模态触发器 CMT**；
  - 最终触发器外观仍类似点状标点，但细节不同，以降低正常省略号造成的误激活。

### 2.3 公式与流程的文字说明

- 朴素多模态毒化方式为：图像按混合参数 `α` 叠加图像触发器 `I`，文本插入 `[MASK]` 触发器。但该方式要求攻击者能直接访问文本，且容易破坏图文一致性。
- 本文的毒化过程可概括为：`T(m) = Poison(m) → (v_p, t_p)`，其中 `Poison` 表示 CMT 注入策略，`t_p` 由文本抽取器从 `v_p` 中自动识别。
- 攻击目标：当输入含 CMT 的模因时，检测器输出攻击者指定标签；本文统一将目标设为“非仇恨（non-hateful）”。
- 训练阶段按 poison ratio `ρ` 采样训练数据生成毒化集，再与剩余干净数据训练受害模型；推理阶段攻击者向任意模因注入 CMT 即可尝试激活后门。

## 3. 实验设计

- **数据集**：
  - FBHM：8500/500/1000，仇恨率 37.56%；
  - MAMI：8000/1000/1000，仇恨率 50.0%；
  - HarMeme：3013/177/354，仇恨率 26.21%。
  - 为便于比较，论文将标签简化为“仇恨/非仇恨”二分类。
- **受害模型**：Late Fusion、MMBT、VisualBert、VilBert、MMF Transformer（MMFT），以及基于 LLM 的 MR.HARM，共 6 种检测器。
- **对比方法**：
  - TrojVQA：唯一可用的多模态后门攻击基线；
  - CMT w/o TA：无触发器增强器的初始跨模态触发器；
  - CMT：完整方法；
  - FIBA：单模态图像后门方法；
  - Consider-like 文本触发器、红色图案、随机图案等消融触发器；
  - 防御对比：Neural Polarizer。
- **评价指标**：
  - 攻击效果：Clean Data Accuracy（CDA）和 Attack Success Rate（ASR），越高越好；
  - 图像隐蔽性：PSNR、SSIM、LPIPS；
  - 文本隐蔽性：ΔPPL、ΔGE、USE。
- **攻击设置**：目标标签为非仇恨；poison ratio `ρ=1%`；触发器缩放参数 `ε=1/8`；blending 参数 `λ=0.2`；使用 MMF benchmark 默认设置；增强器训练采样 10% 训练数据，ResNet-152 训练 100 epochs，学习率 0.001，SGD 优化器。
- **场景**：分别评估“人工输入文本”和“自动文本抽取器识别文本”两种场景，后者更贴近实际自动检测流水线。

## 4. 资源与算力

- 论文明确提到使用 **ResNet-152** 训练触发器增强器，训练 **100 epochs**，学习率 **0.001**，优化器为 **SGD**；并提到使用 MMF benchmark 默认设置。
- 但论文**未明确报告 GPU 型号、GPU 数量、总训练时长、显存消耗或总算力规模**。因此无法从文中准确估算其算力成本。

## 5. 实验数量与充分性

- **主攻击实验**：3 个数据集 × 6 个受害模型 × 3 种方法（TrojVQA、CMT w/o TA、CMT）× 2 种文本输入场景，约 108 组 CDA/ASR 结果。
- **防御实验**：3 个数据集 × 3 种触发器方法，在 VisualBert 上评估 Neural Polarizer。
- **隐蔽性实验**：FBHM 上 3 种触发器 × 6 个图像/文本指标。
- **消融实验**：FBHM 上 6 个受害模型 × 6 种触发器模式（FIBA、Consider-like、红色、随机、CMT w/o TA、CMT）。
- 总体看，实验覆盖了多数据集、多模型、多指标、自动/手动场景和一种后门防御，数量较充分；论文称每组实验重复三次以保证公平。
- **局限**：未报告标准差或显著性检验；防御实验仅使用一种防御方法和一个受害模型；消融和隐蔽性分析主要集中在 FBHM；超参数敏感性分析不足。因此“充分”但仍有一定选择性覆盖。

## 6. 主要结论与发现

- CMT 在人工输入和自动文本抽取两种场景下均能有效实施后门攻击，整体优于 TrojVQA 和 CMT w/o TA。
- 自动场景下 OCR 识别误差会降低 ASR，但 CMT 仍保持相对优势，说明跨模态设计对自动检测流水线有效。
- 在 FBHM 自动场景中，CMT 对多个模型的 ASR 约为 0.833–0.967；MAMI 自动场景 ASR 约 0.475–0.554；HarMeme 自动场景约 0.563–0.667。手动场景下 ASR 更高，部分接近 1.0。
- 隐蔽性方面，CMT 的 SSIM 达 0.9990、LPIPS 为 0.0031，文本侧 USE 为 0.9819，优于 TrojVQA 的随机块触发器。
- 对 Neural Polarizer 防御，CMT 在 FBHM 上 ASR 达 1.000，显著高于 TrojVQA 的 0.167 和 CMT w/o TA 的 0.244；但在 HarMeme 上 TrojVQA 的 ASR 为 0.695，高于 CMT 的 0.575，说明防御鲁棒性并非在所有数据集上一致占优。
- 消融表明：FIBA 隐蔽但 ASR 低且损害 CDA；Consider-like 触发器 ASR 高但隐蔽性差；红/随机颜色可缓解误激活但牺牲隐蔽性；TA 是缓解误激活的较优方案。

## 7. 优点

- **问题新颖**：首次形式化仇恨模因检测中的后门攻击，填补多模态内容审核安全研究空白。
- **方法巧妙**：利用“文本共存于图像”的模因特性设计文本样跨模态触发器，只需注入图像即可同时影响视觉和文本编码器，适配自动检测场景。
- **注入策略细致**：触发器位置和尺寸随文本边界框自适应，兼顾隐蔽性与图文一致性。
- **TA 设计有针对性**：针对正常省略号可能误激活的问题，通过毒化特征提取与 blending 增强触发器区分度。
- **实验较全面**：覆盖 3 个数据集、6 种检测器、2 种输入场景、多种隐蔽性指标及一种后门防御，并包含多种触发器消融。
- **威胁模型现实**：黑盒、部分训练数据访问、推理仅图像输入，较符合第三方数据污染和自动审核流水线场景。

## 8. 不足与局限

- **算力信息缺失**：未报告 GPU 型号、数量、训练时长，复现成本不透明。
- **任务简化**：将数据集统一简化为仇恨/非仇恨二分类，未覆盖细粒度仇恨类型、多标签或真实平台复杂标签体系。
- **自动场景性能有限**

：在自动文本抽取场景下，OCR 识别误差会直接削弱触发器从图像到文本模态的传递效果，导致部分数据集上的 ASR 明显低于人工输入场景；MAMI、HarMeme 等数据集上的攻击成功率仅为中等水平，说明方法对文本抽取器质量、图像清晰度和排版较敏感。论文也未系统评估真实社交平台常见的图像压缩、重编码、缩放、裁剪、水印等后处理对触发器可读性和后门激活的影响，因而自动流水线中的实际鲁棒性仍需进一步验证。

- **触发器增强器带来额外攻击成本**：TA 需要采样部分训练数据并训练 ResNet-152 提取毒化特征，虽然提升了触发器区分度，但也增加了攻击者的数据访问、训练算力和实现复杂度；论文未充分分析增强器在不同数据集、不同受害模型间的迁移性，也未讨论增强器本身是否更容易被防御方通过异常特征检测发现。
- **目标标签与攻击范围较单一**：实验统一将后门目标设为“非仇恨”，未覆盖多目标后门、细粒度仇恨类别定向攻击、多触发器协同攻击等更复杂设定；也未讨论模型持续更新、增量训练或集成审核系统中后门是否保持稳定。
- **防御评估覆盖不足**：仅采用 Neural Polarizer 一种防御，且主要在 VisualBert 上评估，未系统比较 STRIP、Neural Cleanse、Fine-Pruning、MNTD 等多模态后门防御，也未评估训练数据清洗、触发器逆向、跨模态一致性检测、OCR 异常检测等针对性防御。因此“CMT 对现有防御具有较强鲁棒性”的结论应限定在特定防御和实验设置下。
- **统计报告与可复现性不足**：论文称每组实验重复三次，但未报告标准差、置信区间或显著性检验；部分消融和隐蔽性分析集中在 FBHM，跨数据集结论的稳健性有限。算力信息缺失、代码与数据是否公开未明确说明，也影响复现与后续比较。
- **任务简化与生态效度有限**：将多标签或细粒度仇恨标注统一简化为二分类，可能掩盖不同仇恨类型、隐晦表达和文化语境下的检测差异；触发器“..”虽较自然，但正常文本中省略号、标点组合分布复杂，TA 只能缓解而非彻底消除误激活风险。
- **伦理与滥用风险需关注**：论文揭示了内容审核系统的现实安全风险，但攻击方法本身具有双用途属性；实际研究中应配合负责任披露、受控实验和防御优先的发布策略。

## 9. 对研究与实践的启示

- **数据供应链安全**：第三方训练数据、爬取数据和众包标注均可能成为后门入口，内容审核系统需要建立数据来源审计、毒化样本检测和训练过程监控机制。
- **跨模态一致性防御**：CMT 利用图像中文本同时影响视觉与文本编码器，提示防御方应关注图文一致性、OCR 文本异常和跨模态表征冲突，而不仅检查单一模态触发器。
- **自动审核流水线的脆弱环节**：OCR/文本抽取器既是攻击传递通道，也是潜在检测点；可在 OCR 输出、文本-图像对齐和触发器逆向层面部署异常检测。
- **评测应贴近真实场景**：后续研究应加入图像压缩、重编码、多语言、多标签、模型更新和集成审核等设定，并报告统计显著性、算力成本和复现细节。

## 10. 总体评价

该论文选题新颖，将后门攻击引入仇恨模因检测这一多模态内容审核场景，提出的文本样跨模态触发器设计巧妙，威胁模型较贴近现实，实验覆盖多个数据集、多种检测器和自动/手动输入场景，具有较高的学术价值和实践警示意义。其主要不足在于自动场景性能仍受 OCR 质量制约、防御评估覆盖面有限、任务设定偏简化，且算力与统计报告不够透明。总体而言，论文为多模态内容审核安全研究提供了有价值的问题框架和方法基线，但真实部署中的鲁棒性、迁移性和防御有效性仍需后续工作系统检验。

（完）
