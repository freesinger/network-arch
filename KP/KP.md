## 

## A Knowledge Plane for the Internet

### Abstract

- “智能网络”方案——自查自纠

- KP的定义

- 认识 > 算法

### 1. Introduction

- 现今网络架构的普遍性与异同点

- 新的网架构思想是能处理low-level问题具有“自主意识”的网络

- 文章架构

  - *Section2*: KP概念、认知技术

  - *Section3*: 新架构的优点

  - *Section4*: 设计所遇到的技术瓶颈

  - *Section5*: 主要的挑战

### 2. A proposal: the Knowledge Plane

- **Definiton**: To devise a separate construct that creates, reconciles and maintains the many aspects of a high-level view,  and then provides service and advice as needed to other element of the network.
  - Edge involvement: 拓宽边缘网络

  - Global perspective: 扩展全球网

  - Compositional structure: 网络互连能够相互整合

  - Unified approach: 整合点决策

  - Cognitive framework: 认知技术是KP最重要的工作机理

#### 2.1 Why a New Construct?

- Data Plane (move data directly) and Management Plane (partition)

- KP is unlike them and better than both

#### 2.2 Why a Unified Approach?

- Neccessity：the information about network configuration and about user-oberved problems be in the same framework

- Final goal: a network can comnfigure, explain, repair itself and doesn't confound the users with mystries

#### 2.3 Why a Cognitive System?

- Significant challenges: 有效运作、合理运行、在复杂的网络环境下高效运作

- 传统分析法不可行，需要一种类似生物识别的“自我认识”寻找解决方案的方法

- Learn and Reason

### 3. What is the Knowledge Plane Good For?

- 错误诊断和缓解：FIX and WHY， 自我训练学习

- 自动（重新）配置：全球网络配置的复杂性、网络配置的频繁性

- 对重叠网络的支持

- 判断网络问题的多角度性、整体性

### 4. KP Architecture

- 把握不变——网络的物理架构

  - 分布式

  - 自底向上

  - 面向限制

  - 由简入繁

- KP的应用意义

  - 数据和知识的整合

  - 对不完全正确的数据进行正确处理

  - 能分析信息交易

#### 4.1 Functional and Structural Requirements

##### 4.1.1 Core Foundation

- *sensor* and *actuators* (“判断”与“行动”）

- 核心：认知计算

##### 4.1.2 Cross-Domain and Multi-Domain Reasoning

- KP独立诊断，跨域合作

- 考虑信息争用

##### 4.1.3 Data and Knowledge Routing

- 知识和信息管理，寻找最优解

##### 4.1.4 Reasong about Trust and Robustness

- 面临诸多问题与挑战

  - 稳定性、可靠性

  - KP的端口如何在网络中取得信任

### 5. Creating a KP

#### 5.1 Possible Building Blocks

- 架构的相关技术

  - Epidemic algorithms：数据的分发传播

  - Bayesian networks：KP的自我学习

  - Rank aggregation：启用信任网络

  - Constraint satisfaction algorithm

  - Policy-based management techniques

#### 5.2 Changes

- 领域交叉（AI)

- 挑战：

  - 如何表示并利用好知识

  - 如何保证可扩展性

  - 怎样散发这些知识

  - 如何面对商业竞争，避免成为商业工具

  - 如何保证安全和可信任

### 6. Summary

- 在人工智能的大浪潮之下，一种新型的网络架构模型被提出势在必行。文章提出了一种新型网络模式——知识平面（KP）。是一种具有高度智能的网络模式，完全不同于现有的网络模式——一种只负责传输数据的“工具”，可感知、预测、处理并学习所遇到的常见网络问题，并为用户提供解决方案。
