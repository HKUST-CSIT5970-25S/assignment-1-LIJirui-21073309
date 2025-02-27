# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name:    LI, Jirui
### Student Id:    21073309
### Email:  jlilb@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > My measurement tool used is **Phoronix Test Suite**. This open-source software is widely used for benchmarking CPU and memory performance across different systems.
    >
    > ### Configuration of the Measurement Tool
    >
    > 1. **Processor**: Intel Xeon E5-2686 v4 (1 core)
    >    - **Reason**: This specific processor was chosen to evaluate its performance in handling computational tasks, particularly in a virtualized environment.
    > 2. **Memory**: 1024MB
    >    - **Reason**: The memory size is set to 1024MB to simulate a constrained environment, allowing us to see how the processor performs under limited resources.
    > 3. **Operating System**: Ubuntu 22.04
    >    - **Reason**: This version of the operating system is stable and widely used in server environments, making it a suitable choice for benchmarking.
    > 4. **Test Type**: 7-Zip Compression and Decompression
    >    - **Reason**: The 7-Zip test is a standard benchmark for measuring data processing performance, specifically in compression and decompression tasks, which are common in many applications.
    >
    > ### Explanation of Measurement Results
    >
    > - **Compression Rating (MIPS)**:
    >   - This value represents the number of million instructions per second (MIPS) processed during the compression task. A higher MIPS indicates better performance in handling compression workloads.
    > - **Decompression Rating (MIPS)**:
    >   - Similar to the compression rating, this value indicates the MIPS processed during the decompression task. It reflects the system's efficiency in restoring data from a compressed format.
    >
    > The values obtained from these measurements provide a quantitative assessment of the CPU's performance in both compression and decompression tasks. They can be used to compare this configuration's performance against other systems or configurations, helping to identify strengths and weaknesses in processing capabilities.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | Compression Rating:**4022 MIPS**  Decompression Rating:**3145 MIPS** | **10836.97 MB/s** |
    | `t2.medium`  | Compression Rating:**10139 MIPS**  Decompression Rating:**5956 MIPS** | **19886.33 MB/s** |
    | `c5d.large` | Compression Rating:**7560 MIPS**  Decompression Rating:**4887 MIPS** | **14177.07 MB/s** |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
    >
    > **t2.micro**
    >
    > ![cpu](/Users/torettoli/Desktop/cloudcomputing/assignment1/t2-micro/cpu.png)
    >
    > ![memory](/Users/torettoli/Desktop/cloudcomputing/assignment1/t2-micro/memory.png)
    >
    > **t2.medium**
    >
    > ![cpu](/Users/torettoli/Desktop/cloudcomputing/assignment1/t2-medium/cpu.png)
    >
    > ![memory](/Users/torettoli/Desktop/cloudcomputing/assignment1/t2-medium/memory.png)
    >
    > **c5d.large**
    >
    > ![cpu](/Users/torettoli/Desktop/cloudcomputing/assignment1/c5d-large/cpu.png)
    >
    > ![memory](/Users/torettoli/Desktop/cloudcomputing/assignment1/c5d-large/memory.png)
    >
    > ### Performance Analysis
    >
    > 1. **CPU Performance**:
    >    - The `t2.medium` instance shows the highest compression (10139 MIPS) and decompression (5956 MIPS) ratings, significantly outperforming both the `t2.micro` and `c5d.large` instances.
    >    - The `c5d.large` instance has a compression rating of 7560 MIPS and a decompression rating of 4887 MIPS, which is lower than `t2.medium` but higher than `t2.micro`.
    >    - The `t2.micro` instance has the lowest CPU performance, with compression at 4022 MIPS and decompression at 3145 MIPS.
    > 2. **Memory Performance**:
    >    - The `t2.medium` instance also has the highest memory performance at **19886.33 MB/s**.
    >    - The `c5d.large` instance follows with **14177.07 MB/s**, and the `t2.micro` has the lowest at **10836.97 MB/s**.
    >
    > The performance of EC2 instances generally increases with the increase in the number of vCPUs and memory resources, as seen in the comparison of `t2.micro`, `t2.medium`, and `c5d.large`. The `t2.medium` instance, which has more resources, consistently outperforms the `t2.micro` instance in both CPU and memory performance. The `c5d.large` instance, while having a different architecture, does not outperform `t2.medium` in CPU performance but has a reasonable memory performance. Overall, the data suggests that increasing resources correlates with improved performance.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 4110           | 0.202    |
    | `m5.large` - `m5.large`   | 4140           | 0.268    |
    | `c5n.large` - `c5n.large` | 4960           | 0.158    |
    | `t3.medium` - `c5n.large` | 2270           | 0.693    |
    | `m5.large` - `c5n.large`  | 3110           | 0.450    |
    | `m5.large` - `t3.medium`  | 1820           | 0.946    |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.
    >
    > ### Analysis of Network Performance
    >
    > 1. **Same Instance Type**:
    >    - The TCP bandwidth for both `t3.medium` and `m5.large` instances is relatively high, with `t3.medium` achieving **4110 Mbps** and `m5.large` achieving **4140 Mbps**. The RTT for `t3.medium` is **0.202 ms**, which is lower than the **0.268 ms** for `m5.large`. This indicates that instances of the same type maintain good bandwidth and low latency.
    > 2. **Different Instance Types**:
    >    - The `c5n.large` instance shows the highest TCP bandwidth at **4960 Mbps** and the lowest RTT at **0.158 ms** when tested against itself, indicating excellent performance.
    >    - However, when `t3.medium` is tested against `c5n.large`, the TCP bandwidth drops significantly to **2270 Mbps**, and the RTT increases to **0.693 ms**, suggesting that there is more latency when communicating between different instance types.
    >    - The `m5.large` to `c5n.large` connection has a TCP bandwidth of **3110 Mbps** and an RTT of **0.450 ms**, which is better than the `t3.medium` to `c5n.large` performance but still lower than the bandwidth of `c5n.large` to itself.
    >    - The `m5.large` to `t3.medium` connection has the lowest TCP bandwidth at **1820 Mbps** and the highest RTT at **0.946 ms**, indicating a significant performance drop when instances of different types communicate.
    >
    > Overall, the network performance between instances of the same type demonstrates higher TCP bandwidth and lower RTT compared to instances of different types. The `c5n.large` instance exhibits the best performance overall, while communication between different instance types results in reduced bandwidth and increased latency. This highlights the importance of instance type selection for optimal network performance in AWS.
    >
    > **t3.medium` - `t3.medium**
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/t3-t3/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/t3-t3/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/t3-t3/ping.png)
    >
    > **m5.large` - `m5.large**
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-m5/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-m5/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-m5/ping.png)
    >
    > **c5n.large` - `c5n.large**
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/c5n-c5n/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/c5n-c5n/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/c5n-c5n/ping.png)
    >
    > **t3.medium` - `c5n.large**
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/t3-c5n/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/t3-c5n/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/t3-c5n/ping.png)
    >
    > **m5.large` - `c5n.large**
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-c5n/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-c5n/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-c5n/ping.png)
    >
    > **m5.large` - `t3.medium**
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-t3/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-t3/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/m5-t3/ping.png)

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 32800          | 64.444   |
    | N. Virginia - N. Virginia | 4960           | 0.158    |
    | Oregon - Oregon           | 4660           | 0.143    |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
    >
    > 
    >
    > ### Analysis of Network Performance
    >
    > 1. **N. Virginia - Oregon**:
    >    - The TCP bandwidth is **32800 Mbps**, which is significantly higher compared to the bandwidth for connections within the same region. However, the RTT is **64.444 ms**, indicating a relatively high latency due to the geographical distance between the two regions.
    > 2. **N. Virginia - N. Virginia**:
    >    - The TCP bandwidth is **4960 Mbps** with a very low RTT of **0.158 ms**. This shows that instances within the same region can communicate with high bandwidth and low latency.
    > 3. **Oregon - Oregon**:
    >    - The TCP bandwidth is **4660 Mbps**, with an RTT of **0.143 ms**. This performance is similar to that of N. Virginia instances communicating within the same region, indicating good performance for instances in Oregon.
    >
    > **N. Virginia - Oregon**:
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/Nij-Ore/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/Nij-Ore/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/Nij-Ore/ping.png)
    >
    > **N. Virginia - N. Virginia**:
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/NIJ-Nij/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/NIJ-Nij/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/NIJ-Nij/ping.png)
    >
    > **Oregon - Oregon**:
    >
    > ![server](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/ore-ore/server.png)
    >
    > ![client](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/ore-ore/client.png)
    >
    > ![ping](/Users/torettoli/Desktop/cloudcomputing/assignment1/network/ore-ore/ping.png)
    >
    > The network performance for instances deployed in different regions (N. Virginia to Oregon) shows a very high TCP bandwidth but comes with a significantly higher RTT compared to instances communicating within the same region. The results indicate that while cross-region connections can achieve high bandwidth, they also experience increased latency, which can affect application performance. Instances within the same region maintain lower latency and moderate bandwidth, demonstrating optimal performance for intra-region communications.
    >
    > 
    >
    > [![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
