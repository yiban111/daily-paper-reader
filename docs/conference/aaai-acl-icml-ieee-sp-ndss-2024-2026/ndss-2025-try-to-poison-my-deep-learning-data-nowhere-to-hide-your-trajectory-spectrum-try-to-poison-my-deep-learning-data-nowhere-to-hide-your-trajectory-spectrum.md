---
title: "Try to Poison My Deep Learning Data? Nowhere to Hide Your Trajectory Spectrum!"
title_zh: 想投毒我的深度学习数据？你的轨迹频谱无处可藏！
authors: "Yansong Gao, Huaibing Peng, Hua Ma, Zhi Zhang, Shuo Wang, Rayne Holland, Anmin Fu, Minhui Xue, Derek Abbott"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ndss-symposium.org/wp-content/uploads/2025-1589-paper.pdf"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 利用训练轨迹检测投毒样本
tldr: DaaS模式下恶意贡献者可投毒数据并在模型中植入后门，而现有检测方法依赖干净数据且对投毒率与触发器敏感。本文首次研究投毒样本的训练轨迹并据此进行检测。实验表明该方法无需预留干净数据且对多种投毒与任务具有鲁棒性。该工作提升了数据策展方防御数据投毒的能力。
source: NDSS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-001.webp\", \"caption\": \"\", \"page\": 1, \"index\": 1, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-002.webp\", \"caption\": \"\", \"page\": 1, \"index\": 2, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-003.webp\", \"caption\": \"\", \"page\": 1, \"index\": 3, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-004.webp\", \"caption\": \"\", \"page\": 1, \"index\": 4, \"width\": 512, \"height\": 512}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 1656, \"height\": 991}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-006.webp\", \"caption\": \"\", \"page\": 6, \"index\": 6, \"width\": 1653, \"height\": 990}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-007.webp\", \"caption\": \"\", \"page\": 6, \"index\": 7, \"width\": 522, \"height\": 522}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-008.webp\", \"caption\": \"\", \"page\": 6, \"index\": 8, \"width\": 523, \"height\": 523}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-009.webp\", \"caption\": \"\", \"page\": 6, \"index\": 9, \"width\": 754, \"height\": 760}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-010.webp\", \"caption\": \"\", \"page\": 6, \"index\": 10, \"width\": 1200, \"height\": 799}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-011.webp\", \"caption\": \"\", \"page\": 6, \"index\": 11, \"width\": 1200, \"height\": 799}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-012.webp\", \"caption\": \"\", \"page\": 6, \"index\": 12, \"width\": 1200, \"height\": 799}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-013.webp\", \"caption\": \"\", \"page\": 6, \"index\": 13, \"width\": 1200, \"height\": 799}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-014.webp\", \"caption\": \"\", \"page\": 6, \"index\": 14, \"width\": 1200, \"height\": 799}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-015.webp\", \"caption\": \"\", \"page\": 6, \"index\": 15, \"width\": 1200, \"height\": 799}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-016.webp\", \"caption\": \"\", \"page\": 7, \"index\": 16, \"width\": 1378, \"height\": 925}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-017.webp\", \"caption\": \"\", \"page\": 7, \"index\": 17, \"width\": 1377, \"height\": 925}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-018.webp\", \"caption\": \"\", \"page\": 7, \"index\": 18, \"width\": 1377, \"height\": 922}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-019.webp\", \"caption\": \"\", \"page\": 8, \"index\": 19, \"width\": 1793, \"height\": 569}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-020.webp\", \"caption\": \"\", \"page\": 9, \"index\": 20, \"width\": 718, \"height\": 718}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-021.webp\", \"caption\": \"\", \"page\": 9, \"index\": 21, \"width\": 718, \"height\": 718}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-022.webp\", \"caption\": \"\", \"page\": 12, \"index\": 22, \"width\": 1380, \"height\": 661}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-023.webp\", \"caption\": \"\", \"page\": 12, \"index\": 23, \"width\": 1374, \"height\": 682}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-024.webp\", \"caption\": \"\", \"page\": 13, \"index\": 24, \"width\": 1200, \"height\": 798}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-025.webp\", \"caption\": \"\", \"page\": 13, \"index\": 25, \"width\": 1200, \"height\": 798}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-026.webp\", \"caption\": \"\", \"page\": 13, \"index\": 26, \"width\": 1200, \"height\": 798}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-027.webp\", \"caption\": \"\", \"page\": 13, \"index\": 27, \"width\": 1306, \"height\": 739}, {\"url\": \"assets/figures/ndss-2025-accepted/ndss-2025-try-to-poison-my-deep-learning-data-nowhere-to-hide-your-trajectory-spectrum/fig-028.webp\", \"caption\": \"\", \"page\": 13, \"index\": 28, \"width\": 1384, \"height\": 738}]"
motivation: 现有投毒样本检测依赖干净数据且对投毒率与触发器类型敏感。
method: 首次分析投毒样本的训练轨迹频谱，据此构建无需干净数据的检测方法。
result: 实验表明该方法对多种投毒率、触发器与任务均保持鲁棒检测效果。
conclusion: 该工作为数据投毒防御提供了不依赖干净数据的实用方案。
---

## Abstract
In the Data as a Service (DaaS) model, data curators, such as commercial providers like Amazon Mechanical Turk, Appen, and TELUS International, aggregate quality data from numerous contributors and monetize it for deep learning (DL) model providers. However, malicious contributors can poison this data, embedding backdoors in the trained DL models. Existing methods for detecting poisoned samples face significant limitations: they often rely on reserved clean data; they are sensitive to the poisoning rate, trigger type, and backdoor type; and they are specific to classification tasks. These limitations hinder their practical adoption by data curators. This work, for the first time, investigates the textit{training trajectory} of poisoned samples in the textit{spectrum domain}, revealing distinctions from benign samples that are not apparent in the original non-spectrum domain. Building on this novel perspective, we propose TellTale to detect and sanitize poisoned samples as a one-time effort, addressing textit{all} of the aforementioned limitations of prior work. Through extensive experiments, TellTale demonstrates the ability to defeat both universal and challenging partial backdoor types without relying on any reserved clean data. TellTale is also validated to be agnostic to various trigger types, including the advanced clean-label trigger attack, Narcissus (CCS'2023). Moreover, TellTale proves effective across diverse data modalities (e.g., image, audio and text) and non-classification tasks (e.g., regression)---making it the only known training phase poisoned sample detection method applicable to non-classification tasks. In all our evaluations, TellTale achieves a detection accuracy (i.e., accurately identifying poisoned samples) of at least 95.52% and a false positive rate (i.e., falsely recognizing benign samples as poisoned ones) no higher than 0.61%. Comparisons with state-of-the-art methods, ASSET (Usenix'2023) and CT (Usenix'2023), further affirm TellTale's superior performance. More specifically, ASSET fails to handle partial backdoor types and incurs an unbearable false positive rate with clean/benign datasets common in practice, while CT fails against the Narcissus trigger. In contrast, TellTale proves highly effective across testing scenarios where prior work fails. The source code is released at https://github.com/MPaloze/Telltale. In the Data as a Service (DaaS) model, data curators, such as commercial providers like Amazon Mechanical Turk, Appen, and TELUS International, aggregate quality data from numerous contributors and monetize it for deep learning (DL) model providers. However, malicious contributors can poison this data, embedding backdoors in the trained DL models. Existing methods for detecting poisoned samples face significant limitations: they often rely on reserved clean data; they are sensitive to the poisoning rate, trigger type, and backdoor type; and they are specific to classification tasks. These limitations hinder their practical adoption by data curators. This work, for the first time, investigates the textit{training trajectory} of poisoned samples in the textit{spectrum domain}, revealing distinctions from benign samples that are not apparent in the original non-spectrum domain. Building on this novel perspective, we propose TellTale to detect and sanitize poisoned samples as a one-time effort, addressing textit{all} of the aforementioned limitations of prior work. Through extensive experiments, TellTale demonstrates the ability to defeat both universal and challenging partial backdoor types without relying on any reserved clean data. TellTale is also validated to be agnostic to various trigger types, including the advanced clean-label trigger attack, Narcissus (CCS'2023). Moreover, TellTale proves effective across diverse data modalities (e.g., image, audio and text) and non-classification tasks (e.g., regression)---making it the only known training phase poisoned sample detection method applicable to non-classification tasks. In all our evaluations, TellTale achieves a detection accuracy (i.e., accurately identifying poisoned samples) of at least 95.52% and a false positive rate (i.e., falsely recognizing benign samples as poisoned ones) no higher than 0.61%. Comparisons with state-of-the-art methods, ASSET (Usenix'2023) and CT (Usenix'2023), further affirm TellTale's superior performance. More specifically, ASSET fails to handle partial backdoor types and incurs an unbearable false positive rate with clean/benign datasets common in practice, while CT fails against the Narcissus trigger. In contrast, TellTale proves highly effective across testing scenarios where prior work fails. The source code is released at https://github.com/MPaloze/Telltale.

---

## 论文详细总结（自动生成）

### 论文基本信息
- **题目**：Try to Poison My Deep Learning Data? Nowhere to Hide Your Trajectory Spectrum!
- **会议/年份**：NDSS 2025
- **核心方法名**：TellTale
- **一句话概括**：论文首次从“训练损失轨迹的频谱域”角度识别投毒样本，提出无需预留干净数据、对投毒率/触发器/后门类型较鲁棒的数据清洗框架 TellTale。

---

### 1. 核心问题与整体含义
- **背景**：在 Data as a Service（DaaS）模式下，数据策展方（如 Amazon Mechanical Turk、Appen、TELUS International）从大量数据贡献者处收集、标注并聚合数据，再出售给深度学习模型提供者。
- **安全风险**：恶意数据贡献者可在聚合数据中投毒，使下游训练模型被植入后门：模型在正常输入上表现正常，但遇到攻击者设定的触发器时产生攻击者期望的行为。
- **现有防御的局限**：训练阶段投毒样本检测较适合 DaaS 的“一次性数据清洗”场景，但已有方法普遍存在以下问题：
  - 依赖预留的干净数据集；
  - 对投毒率、触发器类型、后门类型敏感；
  - 多针对分类任务，难以用于回归等非分类任务；
  - 部分方法只适用于通用后门，难以处理部分后门（partial backdoor）；
  - 部分方法在干净数据集上误报率过高。
- **论文提出的六项需求**：
  - RM1：一次性清洗；
  - RM2：模态无关；
  - RM3：任务无关；
  - RM4：投毒率无关；
  - RM5：攻击无关（触发器类型与后门类型）；
  - RM6：不依赖干净数据。
- **研究问题**：是否存在一种同时满足上述六项需求的数据清洗方法？论文认为现有方法均不能全部满足，因而提出 TellTale。

---

### 2. 方法论
#### 2.1 核心思想
- 论文观察到：良性样本与投毒样本在训练过程中会留下不同的**逐样本损失轨迹**。由于模型同时学习主任务（良性数据）和后门任务（投毒数据），二者损失轨迹存在差异。
- 但原始时域损失轨迹高度重叠，直接基于经验规则（如“投毒样本早期损失更低”）不可靠；部分后门和 Narcissus 等干净标签攻击甚至会出现投毒样本损失高于良性样本的情况。
- TellTale 的关键思路是：
  1. 将逐样本损失轨迹视为时间序列；
  2. 截断到模型收敛后的阶段；
  3. 用 LSTM 自编码器降维去噪；
  4. 用 FFT 将轨迹变换到频谱域，利用幅度与相位信息放大差异；
  5. 用 DBSCAN 聚类，将投毒样本作为离群点识别；若数据干净则形成单簇。

#### 2.2 技术流程
- **Step 1：轨迹截断**
  - 在标准训练过程中记录每个样本每个 epoch 的损失。
  - 监测验证损失，当连续多个 epoch 变化低于阈值时判定模型趋于收敛；继续训练预定 epoch。
  - 丢弃收敛前的轨迹，仅保留收敛后的轨迹片段。论文发现收敛后良性/投毒样本差异更显著。
- **Step 2：LSTM 降维**
  - 将截断后的损失轨迹作为时间序列输入 LSTM encoder-decoder 进行学习。
  - 训练完成后仅使用 encoder 获得低维表示，以降低噪声并提升后续可分性。
- **Step 3：频谱变换**
  - 对低维轨迹使用 FFT 变换到频谱域，得到幅度和相位两个分量。
  - 论文实验表明同时使用幅度与相位效果最佳；仅用幅度尚可，仅用相位较差。
- **Step 4：DBSCAN 聚类**
  - 采用 DBSCAN 而非 k-means/层次聚类，原因：
    - 不需要预先指定簇数；
    - 能处理离群点；
    - 若数据集干净，可自然形成单簇，避免强制二聚类导致高误报。
  - 实验中 `min_samples=4`、`epsilon=5`，采用 sklearn 的 DBSCAN 默认推荐设置。
- **输出**：识别并移除投毒样本，完成一次性数据清洗。

#### 2.3 威胁模型
- **攻击者**：恶意数据贡献者，可控制触发器类型与后门类型，倾向低投毒率，但不控制训练过程。
- **防御者**：数据策展方，不知道触发器/后门类型，默认没有预留干净数据集，需要处理多模态数据和非分类任务。

---

### 3. 实验设计
#### 3.1 数据集与场景
- **图像分类主实验**：
  - CIFAR10；
  - Tiny-ImageNet-200；
  - ImageNet-1000（使用预训练 ResNet18 微调 30 epoch）。
- **音频**：
  - Mini-Speech Commands；
  - AudioMNIST。
- **文本**：
  - IMDB（情感分类）；
  - TREC（文本分类）。
- **回归任务**：
  - APPA real 人脸年龄估计，模型 ResNeXt50。
- **其他场景**：
  - GTSRB 不平衡数据集；
  - 迁移学习场景；
  - 干净/良性数据集上的误报率评估；
  - adaptive-blend 自适应投毒攻击。

#### 3.2 攻击类型与触发器
- **通用后门**：
  - dirty-label 触发器：BadNet、Blend、WaNet、ISSBA；
  - clean-label 触发器：Narcissus（CCS 2023），投毒率低至 0.05%。
- **部分后门**：
  - 源类为
