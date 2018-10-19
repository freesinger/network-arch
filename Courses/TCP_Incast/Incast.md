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

- *2*  mechanisms:

  - Modify the OS to use high resolution timers: facilitate RTO timers with the **granularity（粒度）**of hundreds of  microseconds

  - Turn off delayef ACKs wherever possible:  disabling de- layed ACKs is expected to improve performance for RTO timer values of 40ms or less

- The increased number of smoothed RTT spikes represent more frequent, unnecessary congestion timeout events, another piece of evidence that indicates that **the congestion window is being over-driven when delay ACKs are turned off**.

#### 5.3 Workload and Testbed

- The **variable-fragment workload** does not fully reflect the complex dynamics of the incast pathology.

- First, whatever sub-optimal behavior we see with regard to delayed ACKs is **workload independent**. 

- Second, because our results are different from [11\], and we ensured that all application and transport level parameters are **reproduced**, the different results suggest that the **different network environment associated with a differ- ent testbed** would **play a part**. 

- Also, in agreement with the observation in Figure 6, the sub-optimal behavior for 200us RTO value is **independent** of the presence or absence of de- layed ACKs.

### 6. Quantitative Models

#### 6.1 Model Description

- *2* critical insights into potential TCP fixes to help address the Incast problem:

  -  For large RTO timer values, **reducing the RTO timer** value is a first-order mitigation. 

  - For smaller RTO timer values, **intelligently controlling the inter-packet wait time** becomes crucial.

#### 6.2 Qualitative Refinements

- ##### Several elements to the refinements:

1. As the number of senders increase, the number of RTO events per sender increases. **Beyond a certain number of senders**, the number of RTO events is **constant**.

2.  When a network resource becomes saturated, it is **saturated at the same time for all senders.**

3.  **After a congestion event, the senders enter the TCP RTO state**. The RTO timer expires at each sender with a uniform distribution in time and a constant delay after the congestion event.

4. T（T is the width of the uniform distribution in time） increases as the number of senders increases. However, T is bounded.

    

- ##### Three distinct regions in the goodput graph in Figure 5 as follows:

![Goodput for various minimum RTO values.png](https://github.com/freesinger/network-arch/blob/master/Courses/TCP_Incast/Goodput%20for%20various%20minimum%20RTO%20values.png?raw=true)

 

1. **Initial goodput collapse**:  The number of RTO events increases and the number of senders increases.

2. **Goodput recovery**: The effective value of the RTO timer would be reduced as the number of senders increases.

3. **Goodput decreases again**:  As the number of senders increases, T would eventually become comparable or even larger than the value of RTO timers.  A pos- sible consequence is increased inter-packet wait time, leading to the behavior in Figure 16, and a gradual decrease in goodput.

- 



### 7. Conclusions and Future Work
