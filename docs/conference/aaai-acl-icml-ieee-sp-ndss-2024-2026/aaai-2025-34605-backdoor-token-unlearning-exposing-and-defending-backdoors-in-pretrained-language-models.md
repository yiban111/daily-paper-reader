---
title: "Backdoor Token Unlearning: Exposing and Defending Backdoors in Pretrained Language Models"
title_zh: 后门令牌遗忘：揭示并防御预训练语言模型中的后门
authors: "Peihai Jiang, Xixiang Lyu, Yige Li, Jing Ma"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34605/36760"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 通过令牌遗忘防御预训练语言模型后门
tldr: 现有后门防御多集中于训练后阶段，训练期的高效防御仍待探索。本文提出后门令牌遗忘方法BTU，在监督微调阶段主动检测并中和触发令牌，及时阻止后门被植入。研究发现后门学习会导致令牌行为出现独特模式，据此实现有效识别。实验表明该方法能在训练阶段有效防御后门且不显著损害正常性能。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34605/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 352, \"height\": 352}]"
motivation: 现有后门防御多为训练后处理，训练阶段的高效防御仍缺乏研究。
method: 提出BTU，在微调阶段依据令牌行为模式检测并中和触发令牌。
result: 实验表明该方法能在训练期有效抑制后门且保持正常任务性能。
conclusion: 训练期主动遗忘触发令牌是防御语言模型后门的有效途径。
---

## Abstract
Supervised fine-tuning has become the predominant method for adapting large pretrained models to downstream tasks. However, recent studies have revealed that these models are vulnerable to backdoor attacks, where even a small number of malicious samples can successfully embed backdoor triggers into the model. While most existing defense methods focus on post-training backdoor defense, efficiently defending against backdoor attacks during training phase remains largely unexplored. To address this gap, we propose a novel defense method called Backdoor Token Unlearning (BTU), which proactively detects and neutralizes trigger tokens during the training stage. Our work is based on two key findings: 1) backdoor learning causes distinctive differences between backdoor token parameters and clean token parameters in word embedding layers, and 2) the success of backdoor attacks heavily depends on backdoor token parameters. The BTU defense leverages these properties to identify aberrant embedding parameters and subsequently removes backdoor behaviors using a fine-grained unlearning technique. Extensive evaluations across three datasets and four types of backdoor attacks demonstrate that BTU effectively defends against these threats while preserving the model’s performance on primary tasks.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：预训练语言模型（PLM）通常通过监督微调适配下游任务，但微调阶段容易遭受后门攻击。攻击者只需在训练数据中注入少量带触发器的恶意样本，就能让模型在正常输入上表现正常、在出现触发器时输出攻击者指定标签。
- **核心问题**：现有 NLP 后门防御多集中在**训练后检测/移除**或**推理期输入检测**，而如何在**训练阶段高效防御后门**仍研究不足。已有训练期防御（如 ABL、MF）常带来性能下降或场景不稳定。
- **论文整体含义**：作者从**词嵌入参数**视角重新理解后门学习，认为后门学习可视为“干净任务 + 后门任务”的双任务优化；后门信息主要集中体现在**后门令牌参数（Backdoor Token Parameters, BTP）**上，且 BTP 在训练中比干净令牌参数（CTP）变化更快、更大。基于此，论文提出 **Backdoor Token Unlearning（BTU）**，在训练阶段主动检测并中和触发令牌，以较小干净性能损失防御后门。

## 2. 方法论

### 2.1 核心思想

- 将后门学习从“全部模型参数”重新聚焦到**词嵌入层参数**：干净任务依赖干净嵌入参数，后门任务依赖后门嵌入参数。
- 关键观察：
  1. 仅训练词嵌入层时，后门令牌的嵌入参数变化显著大于干净令牌；
  2. 后门激活高度依赖这些后门令牌嵌入参数。
- 因此，BTU 先通过多轮“异常令牌检测”暴露可疑后门令牌，再用**维度级细粒度遗忘**替换异常维度，最后用少量干净数据微调修复。

### 2.2 后门令牌检测

- 设词表为 \(V\)，检测阈值为 \(\alpha\)，每轮选取变化距离最大的前 \(\alpha\% \times |V|\) 个令牌作为可疑令牌。论文默认 \(\alpha=0.05\)，共进行三轮检测。
- **第一轮**：在完整中毒数据集 \(D\) 上只训练词嵌入层，得到更新后的嵌入参数；计算每个令牌嵌入前后变化的欧氏距离，排序后取前 \(\alpha\%\)，记为 \(T'\)。
- **第二轮**：保留嵌入层和分类头，重新训练嵌入层；同样计算变化距离并取前 \(\alpha\%\)，记为 \(T''\)。
- **第三轮**：在数据集 \(D/T''\) 上重复类似过程，得到 \(T'''\)。第三轮用于进一步检测更复杂的触发器。
- 最终可疑令牌集合为 \(T=T'\cup T''\cup T'''\)。
- 三轮分工：前两轮偏向检测简单触发器，第三轮细化检测复杂触发器。

### 2.3 维度级细粒度遗忘

- 简单地把可疑令牌整体替换为 padding token 会同时抹除干净特征，导致准确率下降。
- BTU 只替换嵌入向量中**变化异常大的维度**：
  - 先计算词嵌入层训练前后所有令牌维度的平均变化量 \(\bar d\)；
  - 对每个可疑令牌 \(t\in T\)，逐维比较其训练前后变化；
  - 若某维变化小于 \(\bar d\)，保留原值；
  - 若某维变化大于等于 \(\bar d\)，用 padding token 对应维度的嵌入值替换。
- 替换后，再用少量干净数据微调模型，以缓解替换带来的负面影响。
- 该设计的目标是：移除后门相关维度，同时尽量保留干净语义特征。

## 3. 实验设计

- **数据集**：
  - SST-2：二分类情感分析；
  - OLID：二分类攻击性/毒性语言识别；
  - AG News：四分类新闻标题分类。
- **受害模型**：主实验使用 BERT-BASE-UNCASED；泛化实验使用 RoBERTa、GPT2、LLaMA2-7B（主要在 SST-2 上）。
- **攻击方法**：四类数据投毒后门攻击：
  - Add-Word：稀有词触发器；
  - Add-Sent：常见短语触发器；
  - Stylebkd：文本风格触发器；
  - Synbkd：句法结构触发器。
  - 主实验毒化率一般为 10%；Stylebkd 和 Synbkd 使用公开库生成中毒样本。
- **Benchmark/工具**：使用公开的文本后门学习评测框架/库（Cui et al. 2022）进行标准设置。
- **对比方法**：共 9 种防御方法：
  - 训练期防御：BKI、MF、CUBE、TG、ST、DPOE；
  - 推理期防御改造为训练期防御：ONION、RAP、STRIP。
- **评价指标**：
  - ACC：干净数据分类准确率；
  - ASR：后门攻击成功率。
- **额外实验**：
  - 不同模型架构泛化；
  - 低毒化率自适应攻击（SST-2、Add-Sent、0.7% 毒化率）；
  - 复杂触发器自适应攻击；
  - 消融：令牌数量影响、检测阈值 \(\alpha\)、替代遗忘策略、异常检测轮次。

## 4. 资源与算力

- 论文**未明确说明**使用的 GPU 型号、GPU 数量、训练时长、显存消耗或总算力。
- 文中提到在 BERT、RoBERTa、GPT2、LLaMA2-7B 上进行实验，但未给出硬件配置和训练成本。
- 因此，BTU 的实际计算开销、可复现成本以及多轮嵌入层训练带来的额外开销**无法从论文中准确评估**。

## 5. 实验数量与充分性

- **实验规模较大**：
  - 主表覆盖 3 个数据集 × 4 种攻击 × 10 个设置（含无防御和 9 种基线），形成大量 ACC/ASR 对比；
  - 额外进行 4 种模型架构上的泛化实验；
  - 包含低毒化率和复杂触发器两类自适应攻击实验；
  - 包含 4 类消融实验：令牌数量、检测阈值、替代遗忘方法、异常检测轮次。
- **充分性**：
  - 覆盖了插入式触发器、风格触发器、句法触发器等多种攻击类型；
  - 同时报告 ACC 和 ASR，能较好衡量“防御效果—干净性能”权衡；
  - 与 9 种防御方法对比，并包含无防御参照，整体实验较丰富。
- **局限性**：
  - 多数结果未报告多次运行的均值/方差或显著性检验，稳定性证据不足；
  - 模型泛化和部分自适应实验主要在 SST-2 上完成，跨数据集泛化仍有限；
  - 低毒化率仅测试了 0.7% Add-Sent 一个设置，复杂触发器实验也较有限；
  - 将推理期防御改造成训练期防御后与 BTU 比较，基线适配是否完全公平需谨慎解读；
  - 未说明超参数搜索和验证集划分方式，\(\alpha=0.05\) 的选择可能存在一定调参偏差风险。
- **总体评价**：实验覆盖面和对比对象较充分，结论方向有较强经验支持；但统计严谨性、跨场景泛化和基线公平性仍有提升空间。

## 6. 主要结论与发现

- 后门学习会使后门令牌嵌入参数与干净令牌嵌入参数产生显著差异，且仅训练词嵌入层时差异被放大。
- 后门激活高度依赖后门令牌嵌入参数，因此定位并替换这些参数可以有效削弱后门。
- BTU 在三个数据集、四类攻击上显著降低 ASR，同时较好保持 ACC：
  - 对 Add-Word、Add-Sent 等插入式攻击，BTU 在三个数据集上多将 ASR 降至 10% 以下；
  - 对 Stylebkd、Synbkd 等复杂触发器也能明显降低 ASR，但并非完全消除，例如部分设置 ASR 仍约 11%–37%。
- 跨模型实验中，BTU 在 RoBERTa、GPT2、LLaMA2-7B 上均能降低 ASR 并保持较高 ACC。
- 在低毒化率自适应攻击中，BTU 明显优于多数基线；在复杂触发器攻击中也有较好防御效果。
- 消融表明：
  - 三轮异常检测在防御效果和准确率之间较平衡；
  - \(\alpha=0.05\) 是较优折中；
  - 维度级替换优于参数噪声、预训练参数替换和整令牌 padding 替换等策略。

## 7. 优点

- **视角新颖**：从词嵌入参数层面重新解释后门学习，将后门信息定位到后门令牌参数，具有较强的可解释性。
- **训练期主动防御**：不同于训练后净化，BTU 在训练过程中检测并中和触发器，适合数据投毒场景。
- **细粒度遗忘设计**：不是简单删除整个令牌，而是逐维替换异常维度，较好兼顾后门移除与干净特征保留。
- **方法相对简单有效**：核心操作是嵌入层训练、距离排序和维度替换，易于实现，作者也公开了代码。
- **实验覆盖较广**：涵盖多种数据集、攻击类型、模型架构和多种基线，并包含自适应攻击与消融分析。
- **实际指标表现较好**：在多个设置下显著降低 ASR，同时 ACC 接近无防御模型。

## 8. 不足与局限

- **依赖核心假设**：BTU 建立在“后门任务比干净任务更简单、后门令牌参数变化更显著”的观察上；若攻击者设计更隐蔽、更复杂的自适应后门，使 BTP 与 CTP 差异缩小，检测可能失效。
- **复杂触发器防御不彻底**：Stylebkd、Synbkd 等设置下 ASR 仍可能较高，说明对非固定、语义/句法型触发器的防御仍有提升空间。
- **理论保证不足**：作者也承认尚未为 BTU 的机制提供理论保证。
- **模型覆盖有限**：主

实验主要基于 BERT-BASE-UNCASED，RoBERTa、GPT2、LLaMA2-7B 的泛化实验主要局限于 SST-2，尚未系统验证跨数据集、跨语言、生成式任务和多模态场景。

- **对干净数据的依赖**：维度替换后仍需少量干净数据微调修复；若实际场景中难以获得可信干净数据，BTU 的修复效果和最终 ACC 可能受限。
- **超参数与可迁移性**：检测阈值 \(\alpha\)、异常检测轮次和“异常维度”判定阈值均带有经验性，缺少理论指导；面对不同词表规模、模型架构和攻击强度时需要重新调参，迁移成本尚未充分讨论。
- **计算与部署限制**：BTU 需要访问训练流程并多轮训练词嵌入层，主要适用于数据投毒式训练期防御；对于只能获得模型权重的训练后场景并不直接适用。论文也未量化其相对普通微调的额外训练开销。
- **潜在语义损伤**：逐维替换虽比整令牌替换更细粒度，但仍可能影响与后门维度耦合的干净语义，尤其是多义词、高频词或低资源领域词；复杂触发器下残余 ASR 也说明防御并非完全无损。
- **攻击场景覆盖仍有限**：主要验证分类任务中的数据投毒后门，尚未覆盖生成式模型指令后门、多语言后门、多模态后门、参数注入或对齐阶段后门等更广泛威胁。
- **自适应攻击风险**：若攻击者将后门信号分散到多个令牌、降低后门令牌参数变化幅度，或使用更接近干净分布的触发器，基于“变化距离排序”的检测可能被削弱；论文虽包含部分自适应实验，但攻击强度仍可进一步扩展。

## 9. 总体评价与启示

总体而言，BTU 的价值主要在于提供了一个**参数级、训练期主动防御**的新视角：将后门学习定位到后门令牌嵌入参数，并通过异常检测与维度级遗忘进行中和。该方法思路清晰、实现相对简单，在多个数据集、攻击类型和模型架构上取得了较好的 ASR 降低与 ACC 保持效果，实验覆盖面也较广。

但其结论仍主要依赖经验观察，理论保证不足；对复杂触发器、低毒化率和强自适应攻击的鲁棒性仍有提升空间，统计严谨性、跨任务泛化和计算成本报告也不够充分。实际部署中，BTU 更适合作为训练期防御链条的一环，与数据审计、鲁棒微调、推理期检测和模型净化等方法组合使用，而非单独依赖。未来可进一步研究后门参数的理论刻画、更隐蔽触发器的检测、跨生成任务与多语言场景的泛化，以及更严格的统计评估和开销分析。

（完）
