---
title: "CBV: Clean-label Backdoor Attacks on Vision Language Models via Diffusion Models"
title_zh: CBV：基于扩散模型的视觉语言模型干净标签后门攻击
authors: "Ji Guo, xiaolong qin, Cencen Liu, Jielei Wang, Jierun Chen, Wenbo Jiang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/922c38c0427f456daae37dddd7bc90a9ef2a4812.pdf"
tags: ["query:backdoor-atk"]
score: 9.0
evidence: 基于扩散模型的视觉语言模型干净标签后门攻击
tldr: 视觉语言模型在图像描述与视觉问答等任务中表现优异，但现有后门攻击多通过添加视觉触发器并修改文本标签进行投毒，导致图文不匹配易被检测。作者提出基于扩散模型的干净标签后门攻击CBV，在反向生成过程中修改得分函数以生成自然的中毒样本。该方法无需改动标签即可植入后门，显著提升了投毒的隐蔽性。实验验证了CBV在视觉语言模型上兼具攻击有效性与样本自然性，推动了多模态后门攻击研究。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有视觉语言模型后门攻击通过添加视觉触发器并修改文本标签投毒，图文不匹配使中毒样本易被检测。
method: 提出CBV，利用扩散模型在反向生成中修改得分函数，生成自然且无需改动标签的干净标签中毒样本。
result: 实验表明CBV在视觉语言模型上实现有效后门植入，同时保持中毒样本的自然性与隐蔽性。
conclusion: 为多模态模型提供了更隐蔽的干净标签后门攻击范式。
---

## Abstract
Vision-Language Models (VLMs) have achieved remarkable success in tasks such as image captioning and visual question answering (VQA). However, as their applications become increasingly widespread, recent studies have revealed that VLMs are vulnerable to backdoor attacks. Existing backdoor attacks on VLMs primarily rely on data poisoning by adding visual triggers and modifying text labels, where the induced image–text mismatch makes poisoned samples easy to detect.
To address this limitation, we propose the Clean-Label Backdoor Attack on VLMs via Diffusion Models (CBV), which leverages diffusion models to generate natural poisoned examples via score matching.
Specifically, CBV modifies the score during the reverse generation process of the diffusion model to guide the generation of poisoned samples that contain triggered image features.
To further enhance the effectiveness of the attack, we incorporate the textual information of the triggered images as multimodal guidance during generation. Moreover, to enhance stealthiness, we introduce a GradCAM-guided Mask (GM) that restricts modifications to only the most semantically important regions, rather than the entire image.
We evaluate our method on MSCOCO and VQA v2 with four representative VLMs, achieving over 80\% ASR while preserving normal functionality.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
基于扩散模型的视觉语言模型干净标签后门攻击。

### 2. 核心内容
视觉语言模型在图像描述与视觉问答等任务中表现优异，但现有后门攻击多通过添加视觉触发器并修改文本标签进行投毒，导致图文不匹配易被检测。作者提出基于扩散模型的干净标签后门攻击CBV，在反向生成过程中修改得分函数以生成自然的中毒样本。该方法无需改动标签即可植入后门，显著提升了投毒的隐蔽性。实验验证了CBV在视觉语言模型上兼具攻击有效性与样本自然性，推动了多模态后门攻击研究。

### 3. 对应检索需求
clean-label backdoor。

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=U1YNfdXkVD](https://openreview.net/forum?id=U1YNfdXkVD)
