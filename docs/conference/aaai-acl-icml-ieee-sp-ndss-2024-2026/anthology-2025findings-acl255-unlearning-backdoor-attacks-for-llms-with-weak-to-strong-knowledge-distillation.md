---
title: Unlearning Backdoor Attacks for LLMs with Weak-to-Strong Knowledge Distillation
title_zh: 基于弱到强知识蒸馏的大语言模型后门遗忘
authors: "Shuai Zhao, Xiaobao Wu, Cong-Duy T Nguyen, Yanhao Jia, Meihuizi Jia, Feng Yichao, Luu Anh Tuan"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://aclanthology.org/2025.findings-acl.255.pdf"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 通过弱到强蒸馏遗忘大语言模型后门
tldr: 参数高效微调虽能连接大语言模型与下游任务，但经微调的中毒模型在遇到预定义触发器时仍会激活内化后门。作者提出W2SDefense弱到强遗忘算法，先全参数微调一个小规模语言模型作为干净教师，再通过特征对齐知识蒸馏引导大型中毒学生模型遗忘后门。该方法利用师生特征对齐削弱后门行为，为PEFT场景下的大模型后门防御提供了新思路，兼顾了模型能力保留与安全性。
source: ACL-2025-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl255/fig-001.webp\", \"caption\": \"\", \"page\": 3, \"index\": 1, \"width\": 1876, \"height\": 595}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl255/fig-002.webp\", \"caption\": \"\", \"page\": 8, \"index\": 2, \"width\": 6000, \"height\": 6000}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl255/fig-003.webp\", \"caption\": \"\", \"page\": 8, \"index\": 3, \"width\": 6000, \"height\": 6000}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl255/fig-004.webp\", \"caption\": \"\", \"page\": 15, \"index\": 4, \"width\": 4800, \"height\": 4800}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl255/fig-005.webp\", \"caption\": \"\", \"page\": 15, \"index\": 5, \"width\": 4800, \"height\": 4800}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl255/fig-006.webp\", \"caption\": \"\", \"page\": 15, \"index\": 6, \"width\": 4800, \"height\": 4800}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl255/fig-007.webp\", \"caption\": \"\", \"page\": 16, \"index\": 7, \"width\": 5120, \"height\": 3840}, {\"url\": \"assets/figures/acl-2025-findings/anthology-2025findings-acl255/fig-008.webp\", \"caption\": \"\", \"page\": 16, \"index\": 8, \"width\": 5120, \"height\": 3840}]"
motivation: 参数高效微调后的中毒大模型在遇到预定义触发器时仍会激活内化后门，亟需有效防御手段。
method: 提出W2SDefense，先训练小型干净教师模型，再通过特征对齐知识蒸馏引导大型中毒学生模型遗忘后门。
result: 实验表明该弱到强遗忘算法能有效削弱中毒模型的后门激活行为，同时保留模型在下游任务上的能力。
conclusion: 为参数高效微调场景下的大语言模型后门防御提供了可借鉴的知识蒸馏范式。
---

## Abstract
Parameter-efficient fine-tuning (PEFT) can bridge the gap between large language models (LLMs) and downstream tasks. However, PEFT has been proven vulnerable to malicious attacks. Research indicates that poisoned LLMs, even after PEFT, retain the capability to activate internalized backdoors when input samples contain predefined triggers. In this paper, we introduce a novel weak-to-strong unlearning algorithm to defend against backdoor attacks based on feature alignment knowledge distillation, named W2SDefense. Specifically, we first train a small-scale language model through full-parameter fine-tuning to serve as the clean teacher model. Then, this teacher model guides the large-scale poisoned student model in unlearning the backdoor, leveraging PEFT. Theoretical analysis suggests that W2SDefense has the potential to enhance the student model’s ability to unlearn backdoor features, preventing the activation of the backdoor. We conduct comprehensive experiments on three state-of-the-art large language models and several different backdoor attack algorithms. Our empirical results demonstrate the outstanding performance of W2SDefense in defending against backdoor attacks without compromising model performance.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：参数高效微调（PEFT，如 LoRA、p-tuning、prompt-tuning）被广泛用于将大语言模型（LLM）适配到下游任务，但已有研究表明 PEFT 难以消除模型在预训练或第三方训练阶段被植入的后门。
- **核心问题**：中毒 LLM 即使经过 PEFT，在输入包含预定义触发器时仍可能激活后门。现有防御方法要么损害模型正常性能，要么依赖全参数微调/多次微调，计算成本高，难以适用于大模型 PEFT 场景。
- **论文目标**：在 PEFT 条件下，使大型中毒学生模型“遗忘”后门特征，同时保持甚至提升干净样本上的任务性能，即同时满足：
  - 干净样本准确率（CA）不下降；
  - 攻击成功率（ASR）显著降低。
- **整体含义**：论文提出一种弱到强知识蒸馏的后门遗忘框架 **W2SDefense**，用小规模干净教师模型引导大规模中毒学生模型进行特征对齐式遗忘，为 PEFT 场景下的大模型权重中毒防御提供新思路。

## 2. 方法论

- **核心思想**：先全参数微调一个小规模干净教师模型，再让该教师通过知识蒸馏和特征对齐，引导大型中毒学生模型在 PEFT 下遗忘后门特征。
- **威胁模型与假设**：
  - 防御者可访问中毒 LLM 的权重；
  - 拥有干净训练数据集；
  - 可下载或训练小规模干净教师模型（如 BERT、GPT-2）；
  - 目标是降低触发器输入的 ASR，同时保持干净输入上的 CA。
- **教师模型构建**：
  - 使用小规模 BERT（或 GPT-2）在干净训练集上全参数微调；
  - 为对齐教师与学生特征维度，在教师模型上增加一个线性层，将教师特征映射到学生特征维度；
  - 教师优化目标为常规监督交叉熵损失。
- **学生模型设置**：
  - 学生为大型中毒 LLM，如 LLaMA3-8B、Vicuna-7B、Qwen2.5-7B；
  - 防御阶段仅使用 PEFT（如 LoRA）更新少量参数。
- **总损失函数**：学生模型优化由三部分构成：
  - **交叉熵损失**：保证学生模型在干净样本上学习正确任务标签；
  - **知识蒸馏损失**：最小化教师与学生输出 logits 之间的 KL 散度，并使用温度缩放；
  - **特征对齐损失**：最小化教师与学生最终隐藏状态之间的欧氏距离，促使学生特征分布向干净教师靠拢。
  - 总损失为三项加权和，权重分别为 α、β、γ。
- **算法流程**：
  1. 在干净训练集上全参数微调小规模教师模型，并添加维度匹配线性层；
  2. 对每个干净训练样本，分别获取教师和学生的 logits 与隐藏状态；
  3. 计算交叉熵损失、蒸馏损失和特征对齐损失；
  4. 通过 PEFT 仅更新学生模型少量参数，最小化总损失；
  5. 输出防御后的学生模型。
- **理论解释**：论文从信息瓶颈角度分析，认为 PEFT 只更新少量参数，难以改变中毒阶段形成的信息瓶颈；W2SDefense 通过特征对齐知识蒸馏提高学生中间特征与输出之间的互信息，从而增强后门特征遗忘能力。

## 3. 实验设计

- **数据集与任务**：
  - 主实验使用三个文本分类数据集：**SST-2、CR、AG’s News**；
  - 使用 **IMDB** 作为 SST-2 的代理数据集、**MR** 作为 CR 的代理数据集来模拟权重中毒；
  - 额外验证长样本场景（IMDB）；
  - 生成任务使用 **CRRsum** 摘要数据集；
  - 数学推理任务使用 **Ape210K** 数据集。
- **受害者模型**：
  - LLaMA3-8B、Vicuna-7B、Qwen2.5-7B；
  - 模型规模实验使用 Qwen2.5-1.5B、3B、7B、14B。
- **后门攻击算法**：
  - **BadNet**：稀有字符 “mn” 作为触发器；
  - **InSent**：特定短语 “I watched this 3D movie” 作为触发器；
  - **SynAttack**：句法结构触发器；
  - 附录还验证了 **ProAttack**（以 prompt 为触发器）。
- **评价指标**：
  - **CA**：干净样本准确率；
  - **ASR**：中毒样本输出目标标签的比例。
- **对比防御方法**：
  - LoRA 微调（无防御基线）；
  - Back Translation（Back Tr.）；
  - SCPD；
  - ONION；
  - Prune；
  - 附录表 12 还比较了 Quantization、PSIM、Merge、ICLDefense 等。
- **PEFT 方法对比**：
  - LoRA、p-tuning、prompt-tuning。
- **教师模型对比**：
  - BERT-110M 与 GPT-2-124M。
- **消融与补充实验**：
  - 交叉熵、知识蒸馏、特征对齐三项损失的消融；
  - LoRA rank 影响；
  - 不同损失权重影响；
  - 干净模型上是否损害性能；
  - 同数据集中毒场景；
  - 样本长度影响；
  - t-SNE 特征分布可视化。

## 4. 资源与算力

- 文中明确提到实验部署在 **NVIDIA RTX A6000 GPU** 上。
- 但**未明确说明 GPU 数量、总训练时长、总算力消耗**。
- 文中给出了部分训练超参数：
  - 学生模型使用 AdamW，epochs=5，batch size=32，学习率 2e-4；
  - 温度缩放因子 T=2；
  - LoRA rank r=512；
  - p-tuning / prompt-tuning 的 virtual tokens 数为 32，encoder hidden size 为 128；
  - 教师模型为 BERT-110M 或 GPT-2-124M。
- 因此，算力信息只能确认硬件型号，无法从论文中推断具体规模。

## 5. 实验数量与充分性

- **实验数量较多**，主要包括：
  - 3 个主数据集 × 3 个受害者模型 × 3 种攻击算法的主防御实验；
  - 与 Back Tr.、SCPD、ONION、Prune 等基线对比；
  - 同数据集中毒场景实验；
  - GPT-2 教师模型实验；
  - 生成任务与数学推理任务实验；
  - LoRA、p-tuning、prompt-tuning 多种 PEFT 实验；
  - 三项损失消融实验；
  - LoRA rank 与损失权重超参数分析；
  - IMDB 长样本实验；
  - 干净模型性能实验；
  - Qwen2.5 不同模型规模实验；
  - 更多防御算法对比；
  - ProAttack 攻击实验；
  - t-SNE 特征可视化。
- **充分性评价**：
  - 覆盖模型、数据集、攻击算法、PEFT 类型、任务类型和模型规模，整体较充分；
  - 消融实验能够说明特征对齐损失和蒸馏损失各自的作用；
  - 但缺少多次运行的均值/方差、统计显著性检验和训练时长报告；
  - 生成与推理任务仅使用 Qwen2.5 一个受害者模型，覆盖仍有限；
  - 自适应攻击、黑盒场景、更多语言和更多生成任务未系统验证。
- **公平性评价**：
  - 使用统一 CA/ASR 指标，多基线对比，整体较客观；
  - 但部分基线（如 ONION、SCPD、Back Tr.）属于输入改写或样本检测类防御，与 W2SDefense 的权重遗忘目标并不完全同类；
  - W2SDefense 需要干净训练集和小规模教师训练，其他基线未必需要同等数据与算力，比较时应注意这一差异。

## 6. 主要结论与发现

- W2SDefense 在多个模型和攻击算法上显著降低 ASR，同时基本保持甚至提升 CA。
- 例如在 SST-2 + BadNet 设置下，LoRA 基线 ASR 接近 99%–100%，W2SDefense 可降至约 2%–7%。
- 在 CR、AG’s News 等多类任务上，W2SDefense 同样能大幅降低 ASR，且 CA 下降较小。
- 对 p-tuning、prompt-tuning 等其他 PEFT 方法也有效，说明方法不局限于 LoRA。
- 使用 GPT-2 作为教师模型仍能有效防御，说明教师架构具有一定通用性。
- 在生成摘要和数学推理任务上，W2SDefense 可将 ASR 从 95.62% 降至 0.19%（生成任务），推理任务 ASR 降至约 3.10%。
- 干净模型上使用 W2SDefense 不会明显损害性能，平均准确率甚至略有提升。
- t-SNE 可视化显示，LoRA 下中毒样本形成额外特征分布，而 W2SDefense 后该分布消失，支持“后门特征被遗忘”的解释。
- 模型规模增大时，LoRA 的 ASR 仍接近 100%，而 W2SDefense 的 ASR 保持在较低水平，说明方法对模型规模不敏感。

## 7. 优点

- **方法新颖**：据作者称是首个结合知识蒸馏与 PEFT 的后门遗忘算法，提出“弱教师引导强学生”的防御范式。
- **计算效率较高**：防御阶段仅用 PEFT 更新少量参数，避免对大型模型全参数微调。
- **兼顾安全与性能**：不仅降低 ASR，还能保持甚至提升 CA，避免许多防御方法以牺牲正常性能为代价。
- **技术组合合理**：交叉熵保证任务性能，KL 蒸馏传递教师输出行为，特征对齐进一步促使学生遗忘后门特征。
- **实验覆盖较广**：多模型、多数据集、多攻击、多 PEFT、多任务、多模型规模，并有消融和可视化支持。
- **理论分析有启发性**：从信息瓶颈和互信息角度解释 PEFT 难以遗忘后门以及 W2SDefense 的作用机制。
- **实际部署思路清晰**：适合模型提供方或可信第三方在拥有权重和干净数据时进行后门净化。

## 8. 不足与局限

- **白盒限制**：方法依赖访问模型权重，无法直接用于纯黑盒 API 场景。
- **额外训练成本**：虽然教师模型较小，但仍需全参数微调教师模型，并需要干净训练数据。
- **干净教师假设较强**：实际中教师模型和数据是否完全干净、是否与目标任务匹配，会影响防御效果。
- **任务覆盖仍有限**：主实验以文本分类为主，生成和推理任务仅覆盖少量模型与数据集，结论外推需谨慎。
- **复杂多类任务防御不稳定**：如 AG’s News 上 Vicuna 对 SynAttack 的 ASR 仍较高（46.8%），说明复杂任务下遗忘难度较大。
- **缺少统计报告**：未报告多次运行的方差、置信区间或显著性检验，难以判断性能波动。
- **算力报告不完整**：仅说明使用 RTX A6000，未给出 GPU 数量、训练时长和总算力。
- **自适应攻击未验证**：若攻击者针对蒸馏/特征对齐设计自适应触发器，方法鲁棒性仍待检验。
- **基线可比性需注意**：部分对比方法属于输入层防御或检测方法，与权重遗忘方法的适用条件和成本不同。
- **应用限制**：更适合模型发布前的安全净化或可信第三方审计，不适合普通用户对闭源模型直接部署。

（完）
