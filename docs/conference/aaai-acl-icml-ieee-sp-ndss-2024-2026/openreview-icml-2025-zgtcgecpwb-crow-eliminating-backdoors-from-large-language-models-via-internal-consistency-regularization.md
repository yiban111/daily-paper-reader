---
title: "CROW: Eliminating Backdoors from Large Language Models via Internal Consistency Regularization"
title_zh: CROW：通过内部一致性正则化消除大语言模型后门
authors: "Nay Myat Min, Long H. Pham, Yige Li, Jun Sun"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf?id=ZGtcgeCpWB"
tags: ["query:backdoor-atk"]
score: 6.0
evidence: 利用层间一致性消除大语言模型后门
tldr: 现有后门防御多针对视觉或文本分类任务，难以适用于大语言模型的文本生成场景。作者观察到被植入后门的模型在触发时层间隐表示不稳定，据此提出内部一致性正则化方法CROW，通过对抗扰动与正则约束在微调中强制层间表示平滑。实验在Llama-2、CodeLlama与Mistral等模型上验证，无需干净参考模型或触发器先验即可削弱后门。该工作为生成式大模型的后门净化提供了实用方案。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有后门防御针对视觉或分类任务设计，难以应对大语言模型文本生成场景中的后门威胁。
method: 提出内部一致性正则化CROW，利用对抗扰动与正则约束强制被触发样本的层间隐表示保持平滑一致。
result: 在Llama-2、CodeLlama与Mistral等模型上实验，无需干净参考模型或触发器先验即可有效中和后门。
conclusion: 该工作为大语言模型后门净化提供了不依赖触发器知识的实用防御范式。
---

## Abstract
Large Language Models (LLMs) are vulnerable to backdoor attacks that manipulate outputs via hidden triggers. Existing defense methods—designed for vision/text classification tasks—fail for text generation. We propose *Internal Consistency Regularization (CROW)*, a defense leveraging the observation that backdoored models exhibit unstable layer-wise hidden representations when triggered, while clean models show smooth transitions. CROW enforces consistency across layers via adversarial perturbations and regularization during finetuning, neutralizing backdoors without requiring clean reference models or trigger knowledge—only a small clean dataset. Experiments across Llama-2 (7B, 13B), CodeLlama (7B, 13B), and Mistral-7B demonstrate CROW’s effectiveness: it achieves significant reductions in attack success rates across diverse backdoor strategies (sentiment steering, targeted refusal, code injection) while preserving generative performance. CROW’s architecture-agnostic design enables practical deployment.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
利用层间一致性消除大语言模型后门。

### 2. 核心内容
现有后门防御多针对视觉或文本分类任务，难以适用于大语言模型的文本生成场景。作者观察到被植入后门的模型在触发时层间隐表示不稳定，据此提出内部一致性正则化方法CROW，通过对抗扰动与正则约束在微调中强制层间表示平滑。实验在Llama-2、CodeLlama与Mistral等模型上验证，无需干净参考模型或触发器先验即可削弱后门。该工作为生成式大模型的后门净化提供了实用方案。

### 3. 对应检索需求
language model backdoors。

### 4. 来源与原文
- Source：ICML-2025-Accepted
- OpenReview：[https://openreview.net/forum?id=ZGtcgeCpWB](https://openreview.net/forum?id=ZGtcgeCpWB)
