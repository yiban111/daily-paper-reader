---
title: Memory Backdoor Attacks on Neural Networks
title_zh: 针对神经网络的记忆后门攻击
authors: "Eden Luzon, Guy Amit, Roy Weiss, Torsten Krauß, Alexandra Dmitrienko, Yisroel Mirsky"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ndss-symposium.org/wp-content/uploads/2026-f1870-paper.pdf"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 利用训练期后门攻击实现数据集抽取
tldr: 神经网络常在专有数据集上训练，使其成为攻击目标。作者提出一种利用训练期后门攻击的数据集抽取方法，使恶意联邦学习服务器能够通过简单的索引机制系统且确定性地抽取完整的客户端训练样本。被感染的模型在收到基于模式的索引触发器时会输出数据样本，从而精确恢复数据而非概率性重建。该方法在精确控制记忆样本数量、容量与鲁棒性方面表现突出，揭示了后门攻击在数据隐私泄露方面的新威胁。
source: NDSS-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ndss-2026-accepted/ndss-2026-memory-backdoor-attacks-on-neural-networks/fig-001.webp\", \"caption\": \"\", \"page\": 2, \"index\": 1, \"width\": 2156, \"height\": 1607}, {\"url\": \"assets/figures/ndss-2026-accepted/ndss-2026-memory-backdoor-attacks-on-neural-networks/fig-002.webp\", \"caption\": \"\", \"page\": 6, \"index\": 2, \"width\": 930, \"height\": 308}, {\"url\": \"assets/figures/ndss-2026-accepted/ndss-2026-memory-backdoor-attacks-on-neural-networks/fig-003.webp\", \"caption\": \"\", \"page\": 17, \"index\": 3, \"width\": 571, \"height\": 346}]"
motivation: 神经网络常在专有数据集上训练而成为攻击目标，亟需揭示训练期后门攻击可能造成的数据泄露风险。
method: 提出一种训练期后门攻击，使恶意联邦服务器借助基于模式的索引触发器确定性地抽取完整客户端训练样本。
result: 被感染模型在触发时可精确输出记忆样本，实现可控数量、高容量且鲁棒的确定性数据恢复。
conclusion: 揭示了后门攻击可被用于精确数据抽取，对联邦学习中的数据隐私构成新威胁。
---

## Abstract
Neural networks are often trained on proprietary datasets, making them attractive attack targets. We present a novel dataset extraction method leveraging an innovative training-time backdoor attack, allowing a malicious federated learning (FL) server to systematically and deterministically extract complete client training samples through a simple indexing process. Unlike prior techniques, our approach guarantees exact data recovery rather than probabilistic reconstructions or hallucinations, provides precise control over which samples are memorized and how many, and shows high capacity and robustness. Infected models output data samples when they receive a pattern-based index trigger, enabling systematic extraction of meaningful patches from each client’s local data without disrupting global model utility. To address small model output sizes, we extract patches and then recombined them. The attack requires only a minor modification to the training code that can easily evade detection during client-side verification. Hence, this vulnerability represents a realistic FL supply-chain threat, where a malicious server can distribute modified training code to clients and later recover private data from their updates. Evaluations across classifiers, segmentation models, and large language models demonstrate that thousands of sensitive training samples can be recovered from client models with minimal impact on task performance, and a client's entire dataset can be stolen after multiple FL rounds. For instance, a medical segmentation dataset can be extracted with only a 3% utility drop. These findings expose a critical privacy vulnerability in FL systems, emphasizing the need for stronger integrity and transparency in distributed training pipelines. Neural networks are often trained on proprietary datasets, making them attractive attack targets. We present a novel dataset extraction method leveraging an innovative training-time backdoor attack, allowing a malicious federated learning (FL) server to systematically and deterministically extract complete client training samples through a simple indexing process. Unlike prior techniques, our approach guarantees exact data recovery rather than probabilistic reconstructions or hallucinations, provides precise control over which samples are memorized and how many, and shows high capacity and robustness. Infected models output data samples when they receive a pattern-based index trigger, enabling systematic extraction of meaningful patches from each client’s local data without disrupting global model utility. To address small model output sizes, we extract patches and then recombined them. The attack requires only a minor modification to the training code that can easily evade detection during client-side verification. Hence, this vulnerability represents a realistic FL supply-chain threat, where a malicious server can distribute modified training code to clients and later recover private data from their updates. Evaluations across classifiers, segmentation models, and large language models demonstrate that thousands of sensitive training samples can be recovered from client models with minimal impact on task performance, and a client's entire dataset can be stolen after multiple FL rounds. For instance, a medical segmentation dataset can be extracted with only a 3% utility drop. These findings expose a critical privacy vulnerability in FL systems, emphasizing the need for stronger integrity and transparency in distributed training pipelines.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：联邦学习（FL）通过让客户端本地训练、仅上传模型更新来保护数据隐私，但中央服务器控制训练代码分发与模型聚合，处于强势且可能被攻陷的位置。
- **核心问题**：现有数据抽取攻击存在明显局限：抽取结果可能是概率性重建或“幻觉”、样本不完整/损坏、无法控制具体记忆哪些样本、容量受模型参数限制、对剪枝/加噪等权重变换脆弱。
- **论文主张**：提出“记忆后门”（memory backdoor）——被感染的模型在收到结构化索引触发器时，不是错误分类，而是重建/输出训练样本。恶意 FL 服务器可通过简单索引系统、确定性地抽取客户端训练数据。
- **整体含义**：揭示 FL 供应链中的严重隐私威胁：服务器只需轻微篡改训练代码，就能让客户端模型成为“数据骡子”，在聚合前从本地模型更新中恢复私有数据，同时基本不影响主任务效用。

## 2. 方法论

- **核心思想**：在训练代码中加入隐蔽的辅助记忆损失，使模型同时学习主任务 \(f\) 和隐藏记忆任务 \(h\)。攻击者用索引触发模式 \(G(\iota)\) 查询模型，得到对应数据片段 \(d'_\iota\)，再按索引重组为数据集。
- **形式化定义**：
  - 有序索引集 \(I\)，每个索引 \(\iota\) 唯一对应训练数据片段 \(d_\iota\)。
  - 触发映射 \(G:I\to X\)，将索引映射为唯一触发模式 \(t_\iota\)。
  - 记忆后门满足 \(f_\theta(G(\iota))=h(t_\iota)=d'_\iota\)。
  - 抽取过程为遍历索引并重组：\(D'_t=\text{Reconstruct}(\{f_\theta(t_\iota)\}_{\iota\in I})\)。
- **视觉模型实现**：
  - 因分类器输出维度远小于图像维度，模型被训练为一次输出一个图像 patch，再按 patch 和颜色通道重组完整图像。
  - 使用四维索引 \((k,i,l,c)\)：类别 \(k\)、类内图像序号 \(i\)、patch 位置 \(l\)、颜色通道 \(c\)。
  - 触发器由子触发器相加构成：\(G(k,i,l,c)=t_k+t_i+t_l+t_c\)。类别用绿色通道 one-hot 方块编码；样本序号用 Gray code 视觉编码；位置用 mask 指示；通道用底行常量标记。
  - 记忆损失：\(L_{mem}=L_1(f_\theta(t_\iota),p_\iota)+L_2(f_\theta(t_\iota),p_\iota)\)，总损失为 \(L_{total}=L_{CE}+\lambda L_{mem}\)。
- **FL 场景执行**：
  - 服务器向客户端分发被篡改的训练代码；客户端本地训练后上传模型。
  - 服务器在聚合前对本地模型副本离线遍历索引，抽取记忆样本。
  - 为控制容量与隐蔽性，采用跨轮、跨客户端的轮询策略：每轮仅激活一个客户端记忆固定大小的非重叠子集，感染模型不参与聚合，多轮后覆盖每个客户端完整数据集。
- **分割模型与 LLM 扩展**：
  - 分割模型输出与输入同维，可直接像素级重建。
  - LLM 使用固定触发短语加索引哈希后缀，动态构造“提示—响应”训练对，以交叉熵实现记忆损失；服务器按索引迭代提示抽取训练对话/代码。
- **真实性判断**：越界索引通常产生噪声而非连贯样本，可作为抽取内容来自训练数据的经验信号；论文承认并非绝对保证。

## 3. 实验设计

- **任务与数据集**：
  - 图像分类：MNIST、CIFAR-100、VGGFace2。
  - 医学图像分割：脑 MRI 分割数据集。
  - LLM：T5-flan-large，在 alpaca-cleaned 指令数据和 code_instructions_120k_alpaca 代码数据上微调。
- **模型架构**：FCN、CNN、VGG-16、ViT、ViT-S，以及 T5-flan-large。
- **评估指标**：
  - 主任务：分类准确率 ACC、分割 Dice。
  - 重建质量：SSIM、MSE、特征准确率 FA；LLM 使用 Sentence-Transformer 余弦相似度和 GPT-4o 功能判定，报告攻击成功率 ASR。
- **对比与基线**：
  - 与 Song et al. 的白盒方法 LSB Encoding、Correlated Value Encoding、Sign Encoding 对比。
  - 附录中与 Transpose Attack 的索引编码触发对比。
  - 与无攻击 clean 模型、无记忆损失模型对比。
  - 评估权重剪枝（20% 及 0–25% 稀疏度）和 DP-SGD 下的鲁棒性。
  - 额外评估 top-k API 输出限制场景。

## 4. 资源与算力

- 论文**未明确说明**使用的 GPU 型号、数量、总训练时长或总算力开销。
- 文中仅给出部分训练配置：如模型参数量、训练 epoch 数、batch size、FL 轮数、DP-SGD 的裁剪范数与噪声尺度、隐私预算 \(\epsilon\) 等。
- 因此无法从论文中可靠估算实际算力成本；这对复现和成本评估是一个信息缺口。

## 5. 实验数量与充分性

- **实验规模概览**：
  - 端到端 FL 实验：CIFAR-100+ViT、MNIST+FCN、MRI+ViT-S，覆盖分类与分割。
  - 单客户端泛化实验：MNIST CNN/FCN、CIFAR-100 CNN/VGG/ViT、VGGFace ViT、MRI ViT-S 等 7 组模型/数据集组合。
  - 样本数量—重建质量—主任务效用曲线：多模型、多记忆样本量。
  - 参数量与记忆容量关系实验。
  - 与 3 种白盒基线在 3 个场景下对比，并测试剪枝前后表现。
  - 剪枝鲁棒性：0–25% 稀疏度，两个场景。
  - DP-SGD：MNIST-FCN，6k/12k 样本。
  - LLM：两个数据集、1K–10K 记忆样本量。
  - top-k API：CIFAR-100，k=9。
  - 代码审查检测实验：GPT-4o 与 Claude 3，三种提示强度。
- **充分性评价**：
  - 覆盖面较广，跨分类、分割、LLM，跨多种架构，包含剪枝、DP、top-k API、基线对比和消融，整体较充分。
  - 但部分实验仅单客户端或少量 FL 客户端；完整数据集抽取主要在有限数据集上验证；方差报告不普遍；真实工业 FL 部署未测试。
- **客观与公平性**：
  - 使用 clean 模型和多种基线对比，指标合理；剪枝和 DP 评估增强了说服力。
  - 但基线因容量/计算限制仅用 100 样本，部分对比不完全对等；DP 实验中攻击设置隐私预算 \(\epsilon\approx8.3\)，而 DP-only 基线 \(\epsilon\approx6\)，并非完全同预算比较；LLM 评估依赖 LLM judge，存在评判偏差风险。

## 6. 主要结论与发现

- 记忆后门可从分类器、分割模型和 LLM 中系统抽取数百至数千个训练样本，且主任务性能下降通常很小，部分场景仅 0.1–6%。
- 在 FL 中可通过多轮轮询抽取客户端完整数据集：如 CIFAR-100-ViT 约 15 轮抽取全部客户端数据，平均 SSIM 0.867；MNIST-FCN 单轮即可抽取，SSIM 可达 0.921–0.966；MRI 平均 SSIM 约 0.771。
- 医学分割数据集可在约 3% 效用下降下被抽取，说明高敏感领域风险突出。
- 相比 LSB、Corr、Sign 等直接写入权重的方法，记忆后门对剪枝更鲁棒，容量更高；在 20%–25% 剪枝下仍能保持可识别重建。
- DP-SGD 会削弱但未完全消除攻击；LLM 在 1K 样本时 ASR 可达 78.9%（文本）和 98%（代码），但 10K 样本时接近失效。
- 通用代码安全审查难以发现该后门；只有在明确提示“记忆后门”并提供上下文时，LLM 代码审查才较可能识别。

## 7. 优点

- **确定性与系统性**：基于索引遍历抽取，而非依赖启发式猜测或生成采样，提供更强的真实性和可控性。
- **高容量与可扩展**：通过 patch 和通道分解解决小输出空间限制；索引编码支持盲提取，容量优于直接参数写入方法。
- **鲁棒性较好**：数据以特征/模式形式学习，而非直接覆盖权重，对剪枝等权重变换更稳健。
- **跨架构、跨任务**：覆盖 FCN、CNN、VGG、ViT、分割模型和 LLM，证明通用性。
- **威胁模型现实**：利用 FL 服务器分发训练代码的供应链弱点，代码修改小，客户端难以审计底层损失函数。
- **实验较全面**：包含 FL 端到端、消融、基线、剪枝、DP、top-k API、代码审查和 LLM 场景。
- **开放 artifacts**：承诺公开代码、模型权重和数据集说明，有利于复现。

## 8. 不足与局限

- **强依赖攻击者控制训练代码**：主要适用于恶意/被攻陷 FL 服务器或供应链污染；若客户端能严格审计、使用 TEE 或可信训练环境，攻击受限。
- **容量仍受模型参数与任务冲突限制**：记忆样本越多，重建质量下降，主任务也可能退化；LLM 在 10K 样本时几乎失效。
- **真实性保证是经验性的**：越界索引产生噪声只是经验信号，论文也承认没有绝对保证。
- **防御评估有限**：主要测试剪枝和 DP-SGD，未系统评估更强 DP、认证机制、异常检测、梯度/更新审计、安全聚合结合检测等。
- **实验覆盖有限**：数据集以 MNIST、CIFAR-100、VGGFace2、MRI、Alpaca/代码数据为主；FL 客户端数量较少，未在真实大规模跨设备/跨机构部署中验证。
- **对比公平性有改进空间**：基线受容量限制仅用 100 样本；DP 预算不完全对齐；LLM 评估依赖 LLM judge。
- **算力与复现成本未报告**：缺少 GPU 型号、数量、训练时长等关键资源信息。
- **应用限制**：top-k API 场景需要 logits 或概率估计；触发器对视觉模型可感知，但在论文威胁模型下由攻击者域使用，客户端不可见。

（完）
