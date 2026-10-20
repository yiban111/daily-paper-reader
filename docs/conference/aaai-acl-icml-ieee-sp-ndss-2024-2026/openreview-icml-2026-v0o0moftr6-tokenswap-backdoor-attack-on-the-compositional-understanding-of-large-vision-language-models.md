---
title: "TokenSwap: Backdoor Attack on the Compositional Understanding of Large Vision-Language Models"
title_zh: TokenSwap：针对大型视觉语言模型组合理解能力的后门攻击
authors: "Zhifang Zhang, Qiqi Tao, Jiaqi Lv, Na Zhao, Lei Feng, Joey Tianyi Zhou"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://openreview.net/pdf/a9a87bd7345aaab3baf03b60f04a2f6105ab1e8d.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 破坏组合理解能力的隐蔽后门
tldr: 现有LVLM后门多强制模型输出固定目标模式，易被检测。本文提出TokenSwap，通过微妙扰乱文本中物体关系的组合理解来实现更隐蔽的后门。实验表明其比固定模式攻击更难被识别且保持攻击效果。该工作为多模态大模型隐蔽后门提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有LVLM后门强制固定目标输出，易被检测且过度自信而暴露。
method: 提出TokenSwap，通过扰乱文本中物体关系的组合理解而非强制固定目标来植入隐蔽后门。
result: 实验表明该攻击比固定模式后门更难检测且保持攻击效果。
conclusion: 该工作提出了更隐蔽的多模态大模型后门攻击范式。
---

## Abstract
Large vision-language models (LVLMs) excel at vision-language tasks but remain vulnerable to backdoor attacks. Most existing backdoor attacks on LVLMs force the model to generate predefined target patterns. However, these fixed-pattern attacks are easy to detect, as the model tends to memorize frequent patterns and exhibits overconfidence on targets given poisoned inputs. To address these limitations, we introduce TokenSwap, a more evasive and stealthy backdoor attack that focuses on the \emph{compositional understanding} capabilities of LVLMs. Instead of enforcing a fixed targeted content, TokenSwap subtly disrupts the understanding of object relationships in text. Specifically, it causes the backdoored model to generate outputs that mention the correct objects in the image but misrepresent their relationships (i.e., bags-of-words behavior). During training, TokenSwap injects a visual trigger into selected samples while swapping the grammatical roles of key tokens in the textual answers. Since the poisoned samples differ only subtly from clean ones, an adaptive token-weighted loss is employed to emphasize learning on swapped tokens, strengthening the association between visual triggers and the bags-of-words behavior. Extensive experiments demonstrate that TokenSwap achieves high attack success rates while maintaining evasiveness and stealthiness across multiple benchmarks and LVLM architectures.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
破坏组合理解能力的隐蔽后门。

### 2. 核心内容
现有LVLM后门多强制模型输出固定目标模式，易被检测。本文提出TokenSwap，通过微妙扰乱文本中物体关系的组合理解来实现更隐蔽的后门。实验表明其比固定模式攻击更难被识别且保持攻击效果。该工作为多模态大模型隐蔽后门提供了新思路。

### 3. 对应检索需求
Stealthy backdoor attacks using invisible, semantic, dynamic, or input adaptive triggers.

### 4. 来源与原文
- Source：ICML-2026-Accepted
- OpenReview：[https://openreview.net/forum?id=v0o0mofTr6](https://openreview.net/forum?id=v0o0mofTr6)
