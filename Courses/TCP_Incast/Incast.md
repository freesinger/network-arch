## Understanding TCP Incast Throughput Collapse in Datacenter Networks

### Abstract

- TCP Throughput Collapse, also known as Incast, is a patho- logical behavior of TCP that results in gross under-utilization of link capacity in certain **many-to-one**（多对一通信） communication pat- terns.

- ![TCP Incast](http://bradhedlund.s3.amazonaws.com/2011/tcp-incast/tcp-incast.png)

- An analytical model: 

  - Observed Incast symptoms

  - Identify contributory factors

  - Explore the efficacy of solutions

### 1. Intro

- Internet datacenters support a myriad of services and applications. Vast majority of datacenters use TCP for communication between nodes.

- **The unique workloads, scale, and environment of the Internet datacenter violate the WAN assumptions on which TCP was originally designed.**

- The incast pattern potentially arises in many typical datacenter applications.

- A thorough  solution:

  - Reproduce the results in prior work in our own experimental testbeds and offer another demonstration of the **generality** of Incast.

  - Propose a quantitative model that accounts for some of the observed Incast behavior and provide qualitative refinements that give **plausible（似是而非） explanations** for the other symptoms.

  - Implement several **minor（次要）, intuitive（直观） modifications** to the TCP stack in Linux.

### 2. Background

- The Incast pathology（病理）

  - **Reducing the mini- mum value of the retransmission timeout (RTO) timer** from the default 200ms to 200μs, however  most systems lack the high-resolution timers required for such low RTO values.

  - Switches and routers with large buffers are **expensive**, and even large buffers may be filled up quickly with ever higher speed links.

  - **Application level solutions** are possible, e.g. global scheduling of requests, but requires potentially complex modifications to many key applications that use TCP.

- A key shortcoming of the prior work is the **lack of an analytical model to understand the Incast phenomenon**.

- **Cost concerns and the preference for existing technology** mean that solutions like Infiniband and custom transport protocols are less attractive. 

- Hence our focus on **TCP solutions**.

### 3. Methodology

#### 3.1 Workload

- Inspired by **distributed storage applications and bulk block transfers in batch processing tasks** such as MapReduce.

- The metric of merit（度量标准） is application-level throughput (**goodput**), given by the total bytes received from all senders divided by the finishing time of the last sender.

#### 3.2 Testbed and Tools

- *2* platforms

- Combination of tools:

  - tcpdump and tcptrac

  - analysis tool in Java to fill up gaps

#### 3.3 Exploratory Approach

- Priority was to understand the Incast problem, instead of implementing and evaluating the widest range of possible solutions.

### 4. Initial Findings

- Each RTO event lasts for 200ms, the **default minimum RTO timer value for the Linux TCP stack**.

- These observations inspired us to attempt a series of minor, intuitive modifications to the Linux kernel.

- Modifications:

  - Decreasing the minimum TCP RTO timer from the default 200ms

  - Randomizing the minimum TCP RTO timer value

  - Setting a smaller multiplier for the RTO exponential back off

  - Using a randomized multiplier for the RTO exponential back off

- There was another intuitive modification that we **did not attempt** - randomize each the timer value of each RTO as they occur.

- Many modifications are **unhelpful** including **randonmize the minimum and initial RTO timer value**.

- Most **promising** modification is to **reduce the minimum RTO timer value**.

### 5. Analysis In Depth

#### 5.1 Different RTO Timers

- *Smaller* minimum RTO timer values: 

  - larger values for the initial goodput minimum

  - faster goodput “recovery” between the initial goodput minimum and the subsequent goodput local maxi- mum

- *Larger* minimum RTO timer values:

  - the goodput local maximum occurs at a larger number of senders

- The initial goodput minimum oc- curs at the same number of senders, **regardless of** minimum RTO timer values.

#### 5.2 Delayed ACKs and High Resolution Timers

#### 5.3 Workload and Testbed

### 6. Quantitative Models

#### 6.1 Model Description

#### 6.2 Qualitative Refinements

### 7. Conclusions and Future Work
