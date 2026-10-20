---
title: "Merge Hijacking: Backdoor Attacks to Model Merging of Large Language Models"
title_zh: 合并劫持：针对大语言模型模型合并的后门攻击
authors: "Zenghui Yuan, Yangming Xu, Jiawen Shi, Pan Zhou, Lichao Sun"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2025.acl-long.1571.pdf"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 首个针对大语言模型模型合并的后门攻击
tldr: 针对大语言模型模型合并因开源平台模型不可信而存在后门风险的问题，本文提出Merge Hijacking，首个针对模型合并的后门攻击。攻击者构造并发布恶意上传模型，当用户将其与任意模型合并后，合并模型会继承后门同时保持多任务效用。研究通过四步流程实现有效性与实用性目标，实验验证了该攻击的现实威胁。
source: ACL-2025-Long
selection_source: conference_retrieval
motivation: 模型合并依赖开源不可信模型，可能被植入后门。
method: 构造恶意上传模型，使用户合并后继承后门，通过四步兼顾有效性与实用性。
result: 实验证明合并模型会继承后门且保持多任务性能。
conclusion: 模型合并流程存在被后门劫持的现实风险。
---

## Abstract
Model merging for Large Language Models (LLMs) directly fuses the parameters of different models finetuned on various tasks, creating a unified model for multi-domain tasks. However, due to potential vulnerabilities in models available on open-source platforms, model merging is susceptible to backdoor attacks. In this paper, we propose Merge Hijacking , the first backdoor attack targeting model merging in LLMs. The attacker constructs a malicious upload model and releases it. Once a victim user merges it with any other models, the resulting merged model inherits the backdoor while maintaining utility across tasks. Merge Hijacking defines two main objectives—effectiveness and utility—and achieves them through four steps. Extensive experiments demonstrate the effectiveness of our attack across different models, merging algorithms, and tasks. Additionally, we show that the attack remains effective even when merging real-world models. Moreover, our attack demonstrates robustness against two inference-time defenses (Paraphrasing and CLEANGEN) and one training-time defense (Fine-pruning).

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：大语言模型（LLM）的模型合并技术通过直接融合多个在不同任务上微调过的模型参数，低成本地获得一个多领域统一模型，无需访问原始训练数据或进行昂贵重训练。实际使用中，用户通常从 HuggingFace、GitHub 等开源平台下载模型进行合并。
- **安全风险**：开源平台上的模型可能被恶意开发者植入后门。攻击者上传一个在"代理任务"（surrogate task）上植入后门的恶意模型，受害者将其与其他干净模型合并后，合并模型会继承后门行为。
- **现有工作缺口**：已有针对模型合并的后门攻击（BadMerging、LoBAM）主要面向计算机视觉领域的编码器架构，依赖特征插值损失或 LoRA 适配器，难以直接适用于基于解码器的 LLM。
- **核心研究问题**：在不知道其他合并任务、合并算法与超参数的前提下，如何保证恶意模型合并后仍具攻击有效性，同时保证恶意上传模型与合并模型在各任务上的实用性不被用户察觉？
- **两个目标**：
  - **有效性目标**：无论合并多少个其他干净模型，合并模型都能继承后门并保持攻击性能；
  - **实用性目标**：恶意上传模型在代理任务上的表现与干净模型相当，合并模型在各任务上的表现与干净合并模型相当。
- **整体含义**：论文提出首个针对 LLM 模型合并的后门攻击 **Merge Hijacking**，揭示了"下载—合并"这一低门槛、高信任流程中存在的现实供应链威胁。

## 2. 方法论：Merge Hijacking 四步流程

- **威胁模型设定**：攻击者是完全控制自身模型生产过程的恶意开发者，知道基础模型全部信息（开源）、拥有影子数据集 $D_{sha}$ 与代理任务数据集 $D_{sur}$，但不知道其他合并模型的数量与任务、合并算法及超参数；只能贡献一个恶意上传模型，不能控制他人的微调与用户的合并过程。目标输出设为固定 token 序列（可任意切换，也可设为任务相关的翻转标签）。
- **核心思想**：构造一个与各任务向量近似正交、可跨任务泛化的"后门向量"，经稀疏化与重缩放后注入基础模型，再在代理任务上做掩码微调，使恶意上传模型同时具备实用性与强后门。

- **Step 1：推导后门向量（Deriving the Backdoor Vector）**
  - 从 K 个数据集构造影子数据集 $D_{sha}$（可与其他合并任务不同）；
  - 在 $D_{sha}$ 上微调基础模型得干净影子模型 $f_{\theta_{sha}}$；在投毒后的 $D^*_{sha}$ 上微调得后门影子模型 $f_{\theta^*_{sha}}$；
  - 后门向量定义为 $\tau = \theta^*_{sha} - \theta_{sha}$，其目标是与各任务向量正交、可跨任务泛化。

- **Step 2：基于幅值的排序稀疏化（Magnitude-based Ranking Sparsification）**
  - 按绝对值由小到大对 $\tau$ 各维度排序得 $r(\tau)$，并做 min-max 归一化得 $\hat r(\tau)_j$；
  - 将归一化排名映射为区间 $(\delta-\epsilon, \delta+\epsilon)$ 内的连续概率分布：$p(\tau)_j = (\delta-\epsilon) + \hat r(\tau)_j \cdot 2\epsilon$，幅值越大的权重被保留概率越高；
  - 依概率做 Bernoulli 采样 $x_j \sim \text{Bernoulli}(p(\tau)_j)$，得到稀疏向量：$x_j=1$ 时 $\tau'_j = \tau_j / p(\tau)_j$，否则为 0；
  - 目的：去除后门向量中的冗余噪声，降低对其他任务的干扰。

- **Step 3：重缩放并加回（Rescale and Add Back）**
  - 基于"后门向量与影子任务向量正交，故也近似与其他任务向量及代理任务向量正交"的假设，用缩放因子 $\lambda$ 放大后门向量影响；
  - 注入基础模型：$\theta^*_{base} = \theta_{base} + \lambda \cdot \tau'$。

- **Step 4：掩码微调（Mask Finetuning）**
  - 在代理任务上以投毒率 $\rho$ 构造后门数据集 $D^*_{sur}$（触发器随机插入输入，标签改为目标输出）；
  - 在 $D^*_{sur}$ 上以交叉熵损失优化 $\theta^*_{base}$：$\theta^*_{upload} = \arg\min_{\theta^*_{base}} \sum_{(x,y)\in D^*_{sur}} \mathcal{L}_{ce}(f_{\theta^*_{base}}(x), y)$；
  - 目的：恢复代理任务实用性，同时不破坏已注入的后门特征；最终产物即上传到开源平台的恶意模型。

## 3. 实验设计

- **数据集与场景**
  - 影子数据集 $D_{sha}$：GLUE 的 SST-2、CoLA、MRPC 加 SMS Spam，每个数据集采样 125 条、以 20% 比例投毒；
  - 代理数据集 $D_{sur}$：默认 MRPC，训练/评估各 500 条；
  - 其他合并任务：QNLI（GLUE）、Ag-news、Imdb、Dair emotion（Dairemo）、tweets_hate_speech_detection（THSD）；
  - 复杂任务案例：GSM8K（数学推理）与 CodeContests（代码生成）；
  - 真实世界模型案例：与 NVIDIA OpenMath2-LLaMA 3.1-8B、LLaMA 3.1_8B_Math_50000_Samples 合并。
- **模型与微调**：Llama-3-8B、Mistral-7B、Qwen-7B，均用 LoRA 微调 4 个 epoch。
- **合并算法（Benchmark）**：Task Arithmetic（TA）、Model Breadcrumbs（MB）、DARE、DELLA 四种主流算法；默认用 TA 在 Llama-3-8B 上合并 MRPC、QNLI、THSD 三个任务。
- **攻击超参数**：触发器为稀有词 "MG"，目标输出为固定 token "merging"，$\rho=0.2$，默认 $\lambda=2.0$、$\delta=0.7$、$\epsilon=0.2$。
- **对比方法**：BadNets、BadMerging、LoBAM（分别取 $\lambda=2$ 与其最优 $\lambda=3.5$），并报告无攻击（w/o attack）的干净基线。
- **评价指标**：攻击成功率（ASR）、干净性能（CP）、后门性能（BP），以及三者相对差值 ASR-V、CP-V、BP-V（越接近 0 越好）。
- **消融与因素分析**：合并任务数 N（2–6）、恶意模型合并比例（0.1–0.8）、影子数据集规模（1–4 个子数据集）、触发器类型（字符/词/句子/语法）、移除 Step 2 与 Step 4、$\lambda$（1–3）、$\delta$（0.3–0.8）、$\epsilon$（0.05–0.25）、代理任务选择、目标输出长度（2–13 token）。
- **防御评估**：两种推理时防御（Paraphrasing 用 GPT-3.5-Turbo、CLEANGEN 取 $k=4$、$\alpha=20$）与一种训练时防御（Fine-pruning，100 条干净样本微调 + 0.2 剪枝率）。

## 4. 资源与算力

- 论文正文与附录**未
