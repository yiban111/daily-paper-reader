---
title: Rounding-Guided Backdoor Injection in Deep Learning Model Quantization
title_zh: 深度模型量化中的舍入引导后门注入
authors: "Xiangxiang Chen, Peixin Zhang, Jun Sun, Wenhai Wang, Jingyi Wang"
date: 2026
publication_date: 2026
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ndss-symposium.org/wp-content/uploads/2026-s113-paper.pdf"
tags: ["query:backdoor-atk"]
score: 8.0
evidence: 利用模型量化注入后门的全新后门攻击
tldr: 针对传统后门攻击依赖数据投毒或训练操纵的局限，本文提出QuRA，一种仅利用模型量化过程注入后门的攻击方法。它先通过权重选择策略定位影响后门目标的关键权重，再优化这些权重的舍入方向以放大后门效果。实验表明该攻击能在保持模型整体性能的同时有效植入后门，揭示了量化部署环节被忽视的安全风险。
source: NDSS-2026-Accepted
selection_source: conference_retrieval
motivation: 模型量化在部署中广泛使用，却可能引入被忽视的后门安全风险。
method: 提出QuRA，通过关键权重选择并优化其舍入方向，仅借量化操作注入后门。
result: 实验证明该方法能在维持模型整体性能的同时有效放大后门效果。
conclusion: 量化过程本身可成为后门注入通道，需在部署阶段加以防范。
---

## Abstract
Model quantization is a popular technique for deploying deep learning models on resource-constrained environments. However, it may also introduce previously overlooked security risks. In this work, we present QuRA, a novel backdoor attack that exploits model quantization to embed malicious behaviors. Unlike conventional backdoor attacks relying on training data poisoning or model training manipulation, QuRA solely works using the quantization operations. In particular, QuRA first employs a novel weight selection strategy to identify critical weights that influence the backdoor target (with the goal of perserving the model's overall performance in mind). Then, by optimizing the rounding direction of these weights, we amplify the backdoor effect across model layers without degrading accuracy. Extensive experiments demonstrate that QuRA achieves nearly 100% attack success rates in most cases, with negligible performance degradation. Furthermore, we show that QuRA can adapt to bypass existing backdoor defenses, underscoring its threat potential. Our findings highlight critical vulnerability in widely used model quantization process, emphasizing the need for more robust security measures. Our implementation is available at https://github.com/cxx122/QuRA.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：模型量化（尤其是训练后量化 PTQ）被广泛用于将深度模型部署到边缘设备、移动端和资源受限环境，通常只需用户提供少量校准数据并调用第三方量化工具或服务。
- **核心问题**：论文指出，量化过程中的**权重舍入方向**可能成为被忽视的供应链攻击面。攻击者无需参与模型训练，也无需投毒训练数据，只需在量化阶段篡改舍入逻辑，就可能向量化模型中植入后门。
- **与已有工作的区别**：现有量化条件后门通常需要在训练阶段调整损失函数，使后门在量化后激活；QuRA 则完全在量化阶段注入并激活后门，属于“训练无关”的攻击。
- **整体含义**：论文将模型部署阶段的量化过程视为新的安全边界，强调第三方量化服务、开源量化工具和依赖链可能被恶意篡改，进而破坏模型完整性。

## 2. 方法论

### 2.1 核心思想

- QuRA 利用量化时每个权重的**向上/向下舍入选择**作为可操纵变量。
- 通过选择对后门目标影响大、对干净精度影响小的权重，并控制其舍入方向，使后门误差在逐层量化中累积，同时在输出层进一步微调以强化目标类别预测。
- 攻击仅需少量未标注校准数据，不需要训练数据、不需要直接修改模型参数，也不依赖特定训练流程。

### 2.2 关键流程

- **触发器和后门数据集生成**：
  - 使用固定形状 mask 和可学习 pattern 构造触发器。
  - 通过梯度下降最小化模型对后门样本与目标标签的交叉熵，使触发器与目标标签预测对齐。
  - 将触发器嵌入干净校准数据，形成后门校准数据集。
- **逐层量化与权重选择**：
  - 对每一层计算权重对后门目标和精度目标的重要性。
  - 后门目标主要用梯度项衡量：若权重梯度方向有利于降低后门损失，则期望舍入值设为 1 或 0；梯度为零时设为 0.5 以保留灵活性。
  - 精度目标结合梯度与 Hessian 近似，评估舍入扰动对干净样本激活误差的影响。
  - 对后门与精度目标方向一致的权重直接冻结为有利于后门的舍入值；对冲突权重，按“后门影响大、精度影响小”的评分选择前 r% 冻结为后门方向，其余权重通过优化保持精度。
- **损失函数与优化**：
  - 精度损失 \(L_A\)：量化前后干净数据激活值的 MSE。
  - 后门损失 \(L_B\)：仅在输出层加入，使后门样本分类到目标标签。
  - 惩罚损失 \(L_P\)：鼓励连续舍入变量 \(V\in[0,1]\) 收敛到 0 或 1。
  - 总损失为 \(L_A+\lambda_B L_B+\lambda_P L_P\)，使用 Adam 优化，最终按 \(R(W)=1\{V>1/2\}\) 确定舍入方向。
- **逐层约束**：
  - 每层量化后固定参数并更新激活，模拟真实逐层量化流程。
  - 设置约束：仅当后门损失 \(L_B>0.01\) 时允许操纵，避免模型过早过拟合后门触发器。
- **超参数设置**：
  - CV 任务 4-bit 选择 3% 冲突权重，8-bit 选择 20%；NLP 任务 4-bit 为 2%，8-bit 为 10%。
  - \(\lambda_B=1\)，\(\lambda_P=0.01\)，学习率 0.001。

## 3. 实验设计

- **模型与数据集**：
  - CV：ResNet-18、VGG-16、ViT-Tiny；CIFAR-10、CIFAR-100、Tiny-ImageNet。
  - NLP：BERT-base-uncased；SST-2、IMDb、Twitter、BoolQ、RTE、CB。
  - 额外在 Qualcomm 提供的 ImageNet 预训练模型上验证真实场景适用性。
- **量化设置**：4-bit 与 8-bit 训练后量化；使用约 1% 未标注校准数据，CIFAR 为 512 张图像，Tiny-ImageNet 为 1024 张图像。
- **触发器设置**：CV 使用 BadNet 风格触发器，尺寸约为图像 4%（32×32 输入用 6×6，较大输入用 12×12）；NLP 在问题前添加自然短语“kidding me!”。
- **评价指标**：
  - Clean Accuracy（CA）：原始模型、标准量化模型、QuRA 量化模型的干净精度。
  - Attack Success Rate（ASR）：带触发器样本被误分类到目标类的比例。
- **对比基线**：
  - TQAttack：训练阶段的量化条件后门攻击。
  - TBT：运行时比特翻转后门攻击，分别设置 150 和 512 个翻转比特。
- **防御评估**：
  - 触发器反演类：Neural Cleanse、UMD、DBS。
  - 元分类器类：MNTD。
  - 输入检测类：TED。
  - 并提出 TERR、IBI 两种自适应策略以绕过触发器反演检测。
- **复杂触发器**：动态输入相关触发器、频域触发器、跨语言触发器。

## 4. 资源与算力

- 论文明确说明所有实验在**单张 NVIDIA RTX 3090** 上完成。
- 未说明 GPU 总数量、总训练时长、量化每模型耗时、能耗或大规模重复实验成本。
- 因此算力信息仅能确认单卡环境，无法评估总计算开销。

## 5. 实验数量与充分性

- **实验规模较大**：
  - 主实验覆盖 3 个 CV 模型 × 3 个 CV 数据集 × 2 种位宽，约 18 组 CV 设置；BERT 覆盖 6 个 NLP 任务 × 2 种位宽，约 12 组 NLP 设置。
  - 还包括基线对比、触发器生成消融、权重选择方法消融、冲突权重比例研究、校准数据规模研究、模型规模研究、多种防御评估、复杂触发器评估和 ImageNet 真实模型评估。
- **充分性**：
  - 数据集和模型覆盖较广，兼顾 CV、NLP、不同架构和不同位宽，实验整体较充分。
  - 消融实验较系统地验证了触发器生成、权重选择策略和冲突权重比例的作用。
- **客观性与公平性限制**：
  - 多数主结果似为单次运行，未报告方差、置信区间或重复次数。
  - 基线对比主要集中在 ResNet-18/CIFAR-10/4-bit 一个设置，TQAttack 与 TBT 的覆盖范围有限。
  - 部分超参数按实验确定，虽给出默认值，但不同模型/位宽采用不同冲突权重比例，可能影响可比性。
  - 8-bit 和部分 NLP 任务效果明显下降，论文有讨论，但未系统分析所有失败案例。
  - 防御评估覆盖代表性方法，但未穷尽量化模型专用防御或简单权重差异检测。

## 6. 主要结论与发现

- QuRA 能在不参与训练、仅使用少量校准数据的情况下，在量化阶段植入后门。
- 4-bit 量化下攻击最有效：多数 CV 任务 ASR 超过 90%，部分达到 100%，干净精度下降通常很小；VGG-16/CIFAR-100 上达到 100% ASR 且 CA 仅下降约 0.86%。
- ViT 和 BERT 对 QuRA 较脆弱，ViT 在 8-bit 下仍可接近 100% ASR；BERT 在 4-bit 下多数 NLP 任务 ASR 超过 99%。
- ResNet-18 因残差跳连结构，后门影响在深层被削弱，ASR 相对较低；增加 ResNet 层数也不会自动提升攻击效果。
- 8-bit 量化下攻击能力受限，部分设置精度下降明显，如 ResNet-18/Tiny-ImageNet 8

-bit 时干净精度下降更明显，说明 QuRA 的“舍入操纵空间”随位宽降低而增大：4-bit 下可操纵的舍入决策更少但单次扰动更大，攻击收益更集中；8-bit 下舍入粒度更细，后门信号更易被干净精度约束稀释，因而 ASR 与 CA 的权衡更困难。

- 在 NLP 任务上，BERT 对 QuRA 同样表现出较高脆弱性，4-bit 下多个任务 ASR 接近或超过 99%；但 8-bit 下效果下降，表明低位宽量化是该方法更有效的攻击窗口。
- 防御评估显示，Neural Cleanse、UMD、DBS、MNTD、TED 等代表性检测方法对 QuRA 的检测能力有限；论文提出的 TERR、IBI 等自适应策略还能进一步绕过触发器反演类检测，说明仅依赖传统后门扫描难以稳定发现“量化舍入型”后门。
- 复杂触发器实验表明，QuRA 不局限于简单 patch 触发器，动态输入相关触发器、频域触发器和跨语言触发器也能取得较好攻击效果，增强了攻击的现实适应性。
- 在 Qualcomm 提供的 ImageNet 预训练模型上的验证，说明该方法不仅适用于学术基准，也具备一定真实部署场景可行性。

## 7. 局限性与讨论

- **威胁模型依赖**：QuRA 的核心前提是攻击者能够影响或篡改量化阶段的舍入逻辑，例如第三方量化服务、开源量化工具或依赖链。若部署方完全自持量化流程并严格审计，攻击面会显著缩小。
- **位宽敏感性**：方法在 4-bit 下效果最强，8-bit 下攻击能力受限且部分设置精度损失明显，说明其适用性与量化配置强相关。
- **模型结构差异**：ResNet-18 的残差跳连会削弱深层后门影响，导致 ASR 相对较低；增加层数并不必然提升攻击效果，表明方法效果受架构传播特性影响。
- **实验统计严谨性**：主结果多为单次运行，缺少方差、置信区间和重复次数报告；基线对比覆盖范围有限，主要集中在少数设置，公平性和统计稳健性仍有提升空间。
- **防御覆盖不完整**：虽评估了多类代表性防御，但未穷尽量化模型专用防御、权重差异检测、舍入日志审计或供应链完整性校验等更贴近部署安全的机制。
- **超参数与可比性**：不同模型和位宽采用不同冲突权重比例，虽给出默认设置，但可能影响跨设置比较；部分失败案例也缺乏系统分析。

## 8. 总体评价与启示

- **学术价值**：论文将模型量化从“效率优化步骤”重新界定为“供应链安全边界”，提出训练无关、仅依赖少量校准数据的量化阶段后门注入方法，拓展了量化条件后门的研究范围。
- **实践警示**：模型安全不应只关注训练数据和模型权重，还应覆盖量化工具、第三方服务、校准数据和部署流水线；量化舍入方向本身可能成为完整性攻击载体。
- **防御启示**：部署侧可考虑量化过程可复现校验、舍入决策日志、工具链签名与隔离、校准数据访问控制、量化前后权重差异检测，以及针对低位宽模型的专门后门审计。
- **总体判断**：QuRA 在 4-bit 场景下展示了较强攻击有效性与较低干净精度代价，实验覆盖 CV、NLP、多种架构和多种防御，整体较充分；但其现实威胁取决于量化流程的可控性，8-bit 效果、统计稳健性和防御覆盖仍是后续研究需要补足的方向。

（完）
