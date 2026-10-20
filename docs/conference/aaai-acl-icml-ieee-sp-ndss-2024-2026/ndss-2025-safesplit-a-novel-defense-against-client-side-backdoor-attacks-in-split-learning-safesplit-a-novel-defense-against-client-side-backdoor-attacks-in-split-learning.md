---
title: "SafeSplit: A Novel Defense Against Client-Side Backdoor Attacks in Split Learning"
title_zh: SafeSplit：针对拆分学习中客户端后门攻击的新型防御
authors: "Phillip Rieger, Alessandro Pegoraro, Kavita Kumari, Tigist Abera, Jonathan Knauer, Ahmad-Reza Sadeghi"
date: 2025
publication_date: 2025
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.ndss-symposium.org/wp-content/uploads/2025-1698-paper.pdf"
tags: ["query:backdoor-atk"]
score: 5.0
evidence: 针对拆分学习中客户端后门攻击的防御
tldr: 拆分学习使客户端与服务器协同训练深度网络而无需共享原始数据，但其分布式结构也令恶意客户端可通过篡改本地数据植入后门。现有来自联邦学习等框架的防御难以直接适用。作者提出SafeSplit，针对拆分学习架构设计专门的后门防御机制。该工作填补了拆分学习场景下客户端后门防护的空白，对分布式训练安全具有实际意义。
source: NDSS-2025-Accepted
selection_source: conference_retrieval
motivation: 拆分学习的分布式结构易受恶意客户端通过篡改本地数据发起的后门攻击，现有联邦学习防御难以适用。
method: 提出SafeSplit防御方法，针对拆分学习客户端与服务器分层的架构特点检测并缓解客户端植入的后门。
result: 方法在拆分学习场景下有效抵御客户端后门攻击，弥补了该架构缺乏专用防御的不足。
conclusion: 为拆分学习这一分布式训练范式提供了有针对性的后门安全防护方案。
---

## Abstract
Split Learning (SL) is a distributed deep learning approach enabling multiple clients and a server to collaboratively train and infer on a shared deep neural network (DNN) without requiring clients to share their private local data. The DNN is partitioned in SL, with most layers residing on the server and a few initial layers and inputs on the client side. This configuration allows resource-constrained clients to participate in training and inference. However, the distributed architecture exposes SL to backdoor attacks, where malicious clients can manipulate local datasets to alter the DNN's behavior. Existing defenses from other distributed frameworks like Federated Learning are not applicable, and there is a lack of effective backdoor defenses specifically designed for SL. We present SafeSplit, the first defense against client-side backdoor attacks in Split Learning (SL). SafeSplit enables the server to detect and filter out malicious client behavior by employing circular backward analysis after a client's training is completed, iteratively reverting to a trained checkpoint where the model under examination is found to be benign. It uses a two-fold analysis to identify client-induced changes and detect poisoned models. First, a static analysis in the frequency domain measures the differences in the layer's parameters at the server. Second, a dynamic analysis introduces a novel rotational distance metric that assesses the orientation shifts of the server's layer parameters during training. Our comprehensive evaluation across various data distributions, client counts, and attack scenarios demonstrates the high efficacy of this dual analysis in mitigating backdoor attacks while preserving model utility. Split Learning (SL) is a distributed deep learning approach enabling multiple clients and a server to collaboratively train and infer on a shared deep neural network (DNN) without requiring clients to share their private local data. The DNN is partitioned in SL, with most layers residing on the server and a few initial layers and inputs on the client side. This configuration allows resource-constrained clients to participate in training and inference. However, the distributed architecture exposes SL to backdoor attacks, where malicious clients can manipulate local datasets to alter the DNN's behavior. Existing defenses from other distributed frameworks like Federated Learning are not applicable, and there is a lack of effective backdoor defenses specifically designed for SL. We present SafeSplit, the first defense against client-side backdoor attacks in Split Learning (SL). SafeSplit enables the server to detect and filter out malicious client behavior by employing circular backward analysis after a client's training is completed, iteratively reverting to a trained checkpoint where the model under examination is found to be benign. It uses a two-fold analysis to identify client-induced changes and detect poisoned models. First, a static analysis in the frequency domain measures the differences in the layer's parameters at the server. Second, a dynamic analysis introduces a novel rotational distance metric that assesses the orientation shifts of the server's layer parameters during training. Our comprehensive evaluation across various data distributions, client counts, and attack scenarios demonstrates the high efficacy of this dual analysis in mitigating backdoor attacks while preserving model utility.

---

## 论文详细总结（自动生成）

## 一、论文核心问题与整体含义

- **研究背景**：拆分学习（Split Learning, SL）将 DNN 拆分到客户端与服务器协同训练，客户端保留头部和尾部并持有原始数据，服务器承担计算量较大的 backbone，从而支持资源受限设备参与训练且不共享原始数据。
- **核心问题**：SL 的分布式与顺序训练结构使其易受**客户端侧后门攻击**。恶意客户端可通过篡改本地数据、标签、损失函数等，使模型在含触发器输入上输出攻击者指定标签，同时保持干净输入上的正常表现。
- **现有防御不适用**：联邦学习中的后门防御依赖聚合、模型可比性或统一全局起点，但 SL 中客户端顺序训练、每个客户端基于前一个客户端模型继续训练，模型不可直接比较，且服务器无法访问客户端数据。
- **论文意义**：作者提出 **SafeSplit**，自称是首个针对 U 形 SL 中客户端后门攻击的服务器端防御框架，目标是在不显著损害主任务准确率的前提下检测并绕过恶意客户端更新。

## 二、方法论：核心思想与关键技术

- **总体思想**：服务器在每次客户端训练完成后分析 backbone 更新；若检测到后门特征，则执行**圆形回滚分析**，回溯历史 checkpoint，选择最近的良性模型作为下一客户端起点，从而跳过恶意更新但不永久剔除客户端。
- **系统假设**：采用 U 形 SL，DNN 分为 Head、Backbone、Tail；服务器只持有 Backbone。服务器维护最近 \(N\) 个 backbone 的 FIFO 历史。
- **多数良性假设**：假设恶意客户端最多为 \(N/2-1\)，因此通过“最小 \(N/2+1\) 分数”构造多数集合，降低攻击者用 canary 模型操纵评分的影响。
- **静态分析：频域变化**
  - 对每个 backbone 更新 \(B_t-B_{t-1}\) 做二维离散余弦变换（DCT），仅取低频分量。
  - 计算不同模型频域表示之间的欧氏距离，并对每个模型取与其最近的 \(N/2+1\) 个距离之和作为频域分数。
  - 直觉：后门行为与良性行为矛盾，需对模型低频成分造成较大改变。
- **动态分析：旋转距离度量**
  - 计算相邻 backbone 参数向量之间的角位移：用余弦相似度反推夹角。
  - 进一步计算角速度，即角位移随时间的变化率。
  - 最终定义旋转距离/旋转频率 \(RD=\omega/(2\pi)\)，用于衡量参数方向变化的剧烈程度。
  - 直觉：后门注入通常引入异常方向性变化；该指标对参数缩放较稳健，并与频域静态分析互补。
- **良性 checkpoint 选择流程**
  - 客户端训练结束后，将新 backbone 存入 FIFO。
  - 对历史窗口内所有模型计算频域分数和旋转距离分数。
  - 分别取两类分数最小的 \(N/2+1\) 个模型，求交集作为“良性多数”。
  - 从当前时刻向前回溯，选择最近一个同时属于两类多数集合的 checkpoint；若当前模型不在交集中，则回滚到该良性 checkpoint，并将对应 Head/Tail 交给下一客户端。
- **设计特点**：不永久删除恶意客户端；在下一客户端训练前选择良性起点，避免污染后续良性客户端；只分析服务器持有的 backbone，不依赖客户端原始数据。

## 三、实验设计

- **数据集**：CIFAR-10、MNIST、FMNIST、CIFAR-100、GTSRB，均为图像分类任务。
- **模型架构**：ResNet-18、Simple CNN、GoogLeNet、VGG11、Wide-ResNet50、MicronNet；表 I 中参数量约 44 万至 6700 万。
- **攻击类型**：像素触发器后门、语义后门、标签交换攻击；自适应攻击包括对旋转距离、频域距离、二者联合、欧氏距离做损失约束，以及仅训练 tail、参数缩放等策略。
- **默认设置**：10 个客户端，其中 2 个恶意；非 IID 数据采用 main-label 策略，默认 IID rate 0.8；GTSRB 和 CIFAR-100 默认 IID rate 1.0。
- **评价指标**：Backdoor Accuracy（BA，越低越好）和 Main Task Accuracy（MA，越高越好）。
- **对比基线**：无防御；适配到 SL 的联邦学习防御 KRUM、Differential Privacy、FreqFed。作者称因缺乏 SL 专用后门防御，只能与适配后的 FL 防御比较。
- **实验维度**：不同数据集与触发器、IID rate 0.6/0.8/1.0、客户端数量、Poisoned Model Rate（PMR）、Poisoned Data Rate（PDR）、自适应攻击、不同模型架构、不同切层位置、无攻击场景、运行时评估，以及附录中的标签交换和损失最大化攻击。

## 四、资源与算力

- 论文明确说明使用 **4 张 NVIDIA A6000 GPU**、**AMD EPYC 7773X CPU（64 个物理核）**、**768 GB 主存**，基于 PyTorch 实现。
- 论文**未明确报告总训练时长、总 GPU 小时数或各实验的具体耗时**；仅给出 SafeSplit 运行时随客户端数量线性增长，单次防御开销在图中约数十秒量级，且代码未并行化。

## 五、实验数量与充分性

- 实验组数较多，覆盖 5 个数据集、多种触发器、3 种 IID 程度、不同客户端数量、不同恶意比例、不同毒化数据比例、多种自适应攻击、4 种以上模型架构、不同切层位置、FL 防御对比、无攻击场景和运行时评估，另有附录补充攻击。
- **充分性**：在图像分类和小中型 DNN 范围内较充分，尤其对自适应攻击的覆盖较有价值；同时报告了 MA 与 BA，能反映安全性与效用权衡。
- **客观性与公平性**：作者对 FL 防御做了适配以避免明显不公平；但由于不存在 SL 专用基线，比较对象有限。部分自适应攻击假设攻击者知道 backbone 参数，超出其正式威胁模型，属于偏保守的压力测试。
- **不足**：未系统给出关键超参数（如历史窗口 \(N\)、低频选择比例、多数阈值）的敏感性分析；未覆盖真实移动端部署、通信开销和更大规模模型；非 IID 模拟较单一。

## 六、主要结论与发现

- SafeSplit 在多数设置下将 BA 降至 **0% 或低于 5%**，同时 MA 仅有小幅下降。
- 表 II 示例：CIFAR-10 语义后门 BA 从 59.3% 降至 0.0%，MA 从 66.6% 降至 62.7%；CIFAR-10 像素后门 BA 从 100.0% 降至 0.3%；MNIST、FMNIST、CIFAR-100、GTSRB 的 BA 也显著降低。
- 在不同 IID 程度、客户端数量、PMR 和 PDR 下均能有效缓解后门。
- 对多种自适应攻击，包括损失约束和仅训练 tail 的策略，SafeSplit 仍能将 BA 降至 0%；仅训练 tail 的攻击本身在无防御时也难以注入后门。
- 与适配后的 KRUM、DP、FreqFed 相比，只有 SafeSplit 在该设置下将 BA 降至 0%。
- 无攻击时，使用 SafeSplit 的 MA 为 66.6%，无防御为 69.30%，说明有一定效用损失但不大。
- 运行时随客户端数量近似线性增长，作者认为具备实际可部署性。

## 七、优点

- **问题定位清晰**：针对 U 形 SL 顺序训练导致的模型不可比、无法访问数据、污染会传播给后续客户端等挑战，提出专门防御。
- **双视角检测**：频域静态分析捕捉参数分布变化，旋转距离动态分析捕捉训练轨迹方向变化，二者互补。
- **回滚与跳过机制**：不永久剔除客户端，降低误判良性客户端的影响；在下一客户端训练前选择良性起点，阻断污染传播。
- **不依赖客户端数据**：只分析服务器端 backbone，符合 SL 隐私设定。
- **实验覆盖较广**：多数据集、多架构、多攻击比例和多种自适应攻击，安全评估较全面。
- **多数交集策略**：同时要求频域和旋转距离进入多数集合，提高检测稳健性。

## 八、不足与局限

- **多数良性假设限制**：方法依赖恶意客户端不超过 \(N/2-1\)，在极端非 IID、小客户端数或大规模串谋场景下可能失效。
- **实验范围偏图像分类**：未验证 Transformer、LLM、序列模型或多模态模型；DCT 低频和旋转距离在非图像参数结构上的适用性仍待证明。
- **部署开销未充分评估**：需保存最近 \(N\) 个 backbone 并反复计算频域和旋转距离；运行时虽线性增长，但每次训练前调用，客户端规模大时开销可能显著。
- **超参数与消融不足**：历史窗口大小、低频保留比例、多数集合阈值等对检测效果的影响缺少系统敏感性分析。
- **威胁模型较理想化**：假设服务器可信、攻击者最多控制近半客户端、客户端可签名转发模型；未深入考虑恶意客户端交替行为、串谋策略和真实网络环境。
- **基线比较有限**：无 SL 专用防御可比，FL 防御为适配版本；部分自适应攻击假设强于正式威胁模型。
- **效用仍有损失**：部分场景 MA 下降数个百分点；BA 指标还会受模型误分类影响，良性模型也可能出现非零 BA。
- **应用限制**：主要面向 U 形 SL 和图像分类，向其他 SL 配置、真实移动端和隐私敏感医疗/金融场景的泛化需进一步验证。

（完）
