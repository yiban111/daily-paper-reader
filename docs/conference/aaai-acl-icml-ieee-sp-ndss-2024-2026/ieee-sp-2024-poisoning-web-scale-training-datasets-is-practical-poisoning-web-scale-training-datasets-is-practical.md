---
title: Poisoning Web-Scale Training Datasets is Practical
title_zh: 投毒网络规模训练数据集是可行的
authors: "Nicholas Carlini, Matthew Jagielski, Christopher A. Choquette-Choo, Daniel Paleka, Will Pearce, Hyrum Anderson, Andreas Terzis, Kurt Thomas, Florian Tramèr"
date: 2024
publication_date: 2024
publication_date_precision: year
publication_date_source: conference year only; exact release date unverified
publication_date_kind: unknown
pdf: "https://www.computer.org/csdl/pds/api/csdl/proceedings/download-article/1V5U7f5aGPu/pdf"
tags: ["query:backdoor-atk"]
score: 7.0
evidence: 针对网络规模训练数据的实用投毒攻击
tldr: "深度模型常依赖从互联网爬取的大规模分布式数据，其数据来源存在安全风险。作者提出分裂视图投毒和前跑投毒两种数据集投毒攻击，利用网络内容可变性等无效信任假设实施污染。实验表明仅用约60美元即可污染LAION-400M等数据集约0.01%。该工作证明网络规模数据投毒在现实中切实可行，凸显数据供应链安全威胁。"
source: IEEE-SP-2024-CSDL
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/ieee-sp-2024-csdl/ieee-sp-2024-poisoning-web-scale-training-datasets-is-practical/fig-001.webp\", \"caption\": \"\", \"page\": 8, \"index\": 1, \"width\": 432, \"height\": 288}]"
motivation: 深度模型依赖网络爬取的大规模数据，其数据供应链存在被投毒的安全风险。
method: 提出分裂视图投毒与前跑投毒两种攻击，利用无效信任假设污染数据集。
result: "实验表明仅约60美元即可污染LAION-400M等数据集约0.01%。"
conclusion: 证明网络规模数据投毒切实可行，凸显数据供应链安全威胁。
---

## Abstract
Deep learning models are often trained on distributed, web-scale datasets crawled from the internet. In this paper, we introduce two new dataset poisoning attacks that intentionally introduce malicious examples to a model’s performance. Our attacks are immediately practical and could, today, poison 10 popular datasets. Our first attack, split-view poisoning, exploits the mutable nature of internet content to ensure a dataset annotator’s initial view of the dataset differs from the view downloaded by subsequent clients. By exploiting specific invalid trust assumptions, we show how we could have poisoned 0.01% of the LAION-400M or COYO-700M datasets for just $60 USD. Our second attack, frontrunning poisoning, targets web-scale datasets that periodically snapshot crowd-sourced content—such as Wikipedia—where an attacker only needs a time-limited window to inject malicious examples. In light of both attacks, we notify the maintainers of each affected dataset and recommended several low-overhead defenses.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究动机**：现代深度学习模型常在大规模网络爬取数据集上训练，数据量已达数十亿样本，无法逐条人工审核。既有投毒研究大多**预设攻击者已经能修改训练集**，再研究投毒效果、隐蔽性、防御与反防御；本文转向更前置的问题：**攻击者是否真的能把恶意样本放入网络规模数据集？**
- **核心问题**：当前网络规模数据集的发布与下载机制是否存在可被低成本利用的信任漏洞，使投毒从理论威胁变为现实可行攻击。
- **整体含义**：论文提出两类攻击——**分裂视图投毒（split-view poisoning）**与**前跑投毒（frontrunning poisoning）**，并在多个真实数据集上证明其可行。作者认为，机器学习社区必须重新审视对网络规模数据的信任假设，数据供应链安全应成为数据集发布、下载与快照流程的设计要求。

## 2. 方法论

### 2.1 威胁模型与术语

- **分布式数据集**：维护者发布索引 \(\{(url_i,c_i)\}\)，不托管原始内容；客户端未来自行抓取 URL。受存储、隐私、版权限制，LAION 等属于此类。
- **集中式数据集**：策展者定期对网页内容做快照并发布，如 Wikipedia dump、Common Crawl。
- **攻击者假设**：低资源、无内部知识；只能控制少量 URL 的内容，不能修改辅助标签 \(c_i\)，不能增删索引中的 URL；维护者、策展者、下载器均诚实。
- **关键失效假设**：
  - 分布式数据集：客户端**不校验下载内容与维护者索引时内容的密码学完整性**。
  - 集中式数据集：策展者检测并回退恶意修改需要时间 \(\Delta\)，攻击者只需在快照前短暂控制内容。

### 2.2 分裂视图投毒

- **核心思想**：利用互联网内容的可变性。维护者索引数据集时网页是良性的，但之后域名可能过期并被攻击者购买；未来客户端抓取同一 URL 时收到攻击者控制的内容。
- **关键技术细节**：
  - DNS 域名是短期租约，域名过期在大规模数据集中很常见；攻击者购买过期域名即可“继承”数据集中 URL 的残余信任。
  - 攻击者按“域名托管图像数 / 购买成本”排序购买域名，以最小成本控制最大比例样本。
  - 即使不能修改文本标签，只替换图像也足以实施投毒；论文用下游实验验证了这一点。
- **评估流程**：测量过期且可购买域名比例 → 购买域名并被动监测下载频率 → 检测野外是否已有利用 → 在 LAION-400M 上模拟下游投毒效果。

### 2.3 前跑投毒

- **核心思想**：针对周期性快照的众包内容（如 Wikipedia）。攻击者预测某页面被纳入快照的精确时间，在快照前瞬间注入恶意编辑；即使编辑随后被回退，恶意内容仍永久留在快照中。
- **Wikipedia 快照机制**：每月 1 日和 20 日生成 dump；多个并行 worker 按文章 ID 分块、线性抓取，整个过程接近一天，因此不同文章的快照时间不同。
- **时间预测**：
  - 已知当前和上一次快照开始时间 \(t_0,t_{0,prev}\)，利用抓取速率稳定性估计：
    \[
    t_i \approx t_0 + (t_{i,prev}-t_{0,prev})
    \]
  - 通过编辑历史中“最后被纳入快照的编辑”和“第一个未被纳入的编辑”构造区间 \([t^{low}_i,t^{high}_i]\)，再用同一抓取线程内的线性拟合估计上一轮快照时间。
- **编辑时机权衡**：太早编辑会被回退，太晚则错过快照。论文定义调整量 \(a\)，以下式估计成功纳入快照的比例：
  \[
  A(a)=\frac{1}{|D|}\sum_i
  \left(1-\Pr_{\text{revert}}(\hat t_i+a;t^{high}_i)\right)
  \left(1-\mathbf{1}[\hat t_i+a>t^{low}_i]\right)
  \]
  最大化后得到英文 Wikipedia 约 **6.5%** 的保守投毒比例；多语言 Wikipedia 复用同一流程。

### 2.4 防御思路

- **完整性校验**：维护者为每个样本发布 SHA-256 哈希，下载器校验内容；可阻止分裂视图投毒。
- **时序防御**：随机化快照顺序，或先冻结内容、等待超过 \(\Delta\) 的审核期，再回
