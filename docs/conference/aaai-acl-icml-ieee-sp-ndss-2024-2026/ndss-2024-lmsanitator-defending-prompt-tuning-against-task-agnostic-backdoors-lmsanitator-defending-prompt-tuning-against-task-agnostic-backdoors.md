---
title: "LMSanitator: Defending Prompt-Tuning Against Task-Agnostic Backdoors"
title_zh: LMSanitator：防御提示微调中的任务无关后门
authors: "Chengkun Wei, Wenlong Meng, Zhikun Zhang, Min Chen, Minghu Zhao, Wenjing Fang, Lei Wang, Zihui Zhang, Wenzhi Chen"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ndss-symposium.org/wp-content/uploads/2024-238-paper.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 防御预训练语言模型的任务无关后门
tldr: 提示微调虽高效却易受驻留于预训练模型的任务无关后门影响，现有检测方法难以逆向其触发器。本文提出LMSanitator，转而逆向预定义特征而非直接反演触发器，从而检测并移除Transformer上的任务无关后门。实验证明该方法在提示微调场景下有效，增强了预训练模型部署的安全性。
source: NDSS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ndss-2024-accepted/ndss-2024-lmsanitator-defending-prompt-tuning-against-task-agnostic-backdoors/fig-001.webp\", \"caption\": \"\", \"page\": 18, \"index\": 1, \"width\": 800, \"height\": 800}, {\"url\": \"assets/figures/ndss-2024-accepted/ndss-2024-lmsanitator-defending-prompt-tuning-against-task-agnostic-backdoors/fig-002.webp\", \"caption\": \"\", \"page\": 18, \"index\": 2, \"width\": 800, \"height\": 800}, {\"url\": \"assets/figures/ndss-2024-accepted/ndss-2024-lmsanitator-defending-prompt-tuning-against-task-agnostic-backdoors/fig-003.webp\", \"caption\": \"\", \"page\": 18, \"index\": 3, \"width\": 800, \"height\": 800}]"
motivation: 提示微调易受预训练模型任务无关后门影响，现有检测难逆向触发器。
method: 逆向预定义特征而非直接反演触发器以检测并移除后门。
result: 在提示微调场景下有效检测并清除任务无关后门。
conclusion: 提升了预训练语言模型部署阶段的后门安全性。
---

## Abstract
*Prompt-tuning* has emerged as an attractive paradigm for deploying large-scale language models due to its strong downstream task performance and efficient multitask serving ability. Despite its wide adoption, we empirically show that prompt-tuning is vulnerable to downstream task-agnostic backdoors, which reside in the pretrained models and can affect arbitrary downstream tasks. The state-of-the-art backdoor detection approaches cannot defend against task-agnostic backdoors since they hardly converge in reversing the backdoor triggers. To address this issue, we propose LMSanitator, a novel approach for detecting and removing task-agnostic backdoors on Transformer models. Instead of directly inverting the triggers, LMSanitator aims to invert the *predefined attack vectors* (pretrained models' output when the input is embedded with triggers) of the task-agnostic backdoors, which achieves much better convergence performance and backdoor detection accuracy. LMSanitator further leverages prompt-tuning’s property of freezing the pretrained model to perform accurate and fast output monitoring and input purging during the inference phase. Extensive experiments on multiple language models and NLP tasks illustrate the effectiveness of LMSanitator. For instance, LMSanitator achieves 92.8% backdoor detection accuracy on 960 models and decreases the attack success rate to less than 1% in most scenarios.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：Prompt-tuning（如 P-tuning、P-tuning v2）通过冻结预训练语言模型、仅训练少量 soft prompt 来适配下游任务，具有多任务共享、低存储、易部署等优势，成为大模型落地的重要范式。
- **核心问题**：论文实证发现，prompt-tuning 对**任务无关后门（task-agnostic backdoor）**高度脆弱。此类后门驻留在预训练模型中，攻击者无需知道下游任务，即可通过触发器使模型输出接近预定义向量（Pre-defined Vector, PV），进而影响任意下游任务。
- **风险加剧因素**：
  - 一个被后门的预训练模型可能同时服务多个下游系统；
  - prompt-tuning 冻结预训练参数，使后门免于 catastrophic forgetting，不像 fine-tuning 那样容易被大量干净数据“冲淡”；
  - 现有触发反演方法（如 PICCOLO、DBS）在任务无关后门上难以收敛，无法有效检测。
- **论文目标**：提出 **LMSanitator**，在不修改预训练模型参数的前提下，检测任务无关后门，并在推理阶段识别和移除触发器，保持 prompt-tuning 的模块化与低存储特性。

## 2. 方法论

### 2.1 核心思想

- 不再直接反演离散的触发器文本，而是反演攻击者预定义的**输出表示 PV**。
- 任务无关后门会将含触发器的输入映射到特征空间中的异常点；因此，若能逆向出合法 PV，则可判定模型含后门，并可通过监控模型输出与 PV 的相似性检测触发器。
- 利用 prompt-tuning 冻结预训练模型的性质，在输出侧进行快速监控和输入净化。

### 2.2 三步流程

#### Step I：PV Mining（PV 挖掘）

- 构造伪孪生网络：复制目标模型作为辅助模型，二者参数冻结；在目标模型输入嵌入序列的随机位置插入可训练 soft prompt。
- 设计三类损失：
  - **Distance loss**：增大目标模型与辅助模型输出距离，  
    \(L_D=-\mathbb{E}_{x\sim D}\mathrm{MSE}(F_{tar},F_{aux})\)；
  - **Diversity loss**：降低一个 batch 内目标模型特征各维度的多样性，  
    \(L_{div}=-\mathrm{Entropy}((\mathrm{Stack}\{F^{tar}_x;x\sim B\})^T)\)；
  - **Path loss**：避免反复收敛到已发现的 PV，  
    \(L_P=-\max_i \mathrm{MSE}(F_{tar},c_i)\)，无候选 PV 时为 0。
- 总目标：  
  \(L=\lambda_D L_D+\lambda_{div}L_{div}+\lambda_P L_P\)。
- **Fuzz training**：用不同随机种子初始化 soft prompt，多次 fuzz loop，以发现更多不同 PV。
- **Adaptive learning rate**：初始使用较大学习率；当 soft prompt 梯度超过阈值 \(T_{grad}\) 时降为 \(0.01lr_0\)，兼顾平坦损失面下的收敛与稳定性。
- 当总损失低于阈值 \(T_L\) 时，记录当前输出作为 PV 候选。

#### Step II：PV Filtering（PV 过滤）

- 过滤 soft prompt 参数值超出 embedding 参数范围的候选，避免非法 PV。
- 过滤 \(L_{div}\) 高于阈值 \(T_{div}\) 的候选，排除“距离下降但多样性未下降
