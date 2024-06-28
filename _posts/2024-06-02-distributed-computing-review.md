---
title: Ôn tập Hệ tính toán phân bố - Một bước lên "mây"
date: 2024-06-02 15:05:00 +0700
authors: 
- thu4n
categories: [Blogging, Learning]
image: /assets/img/other/distributed-computing-review/cloud-one-step.png
tags: [cloud, distributed computing, virtualization]
math: true
---

Xin chào mọi người, giữ nguyên truyền thống mỗi mùa thi cuối kỳ, mình đã trở lại cùng một bài blog ôn tập mới với môn học NT533 - Hệ tính toán phân bố. Đây có lẽ sẽ là bài blog ôn thi cuối cùng của mình do hết môn để học trên trường rồi =))) Mình cùng vào nội dung chính luôn nhé!

## Hình thức thi

Nội dung thi nói ngắn gọn thì sẽ là *học hết*. Còn nói dài hơn xíu thì thầy bên lớp mình có dặn như sau:
- Xem phần intro, các định luật (laws), clustering và grid.
- Cloud computing: 
    - IaaS: Virtualization of server, network, and storage
    - PaaS: Azure (Case study)
    - SaaS.
- Hadoop:
	+ MapReduce
	+ HDFS
	+ DFS
	+ HBase
- Sẽ có hỏi liên quan tới các bài tập trên lớp, các bài lab và tất nhiên sẽ có hỏi đồ án môn học.

Bài thi có thời gian làm bài là **90 phút** và **không** sử dụng tài liệu (đề đóng). Trong đó:
- Trắc nghiệm từ 30-40 câu (6 điểm).
- Tự luận từ 2-3 câu (4 điểm), sẽ có một câu yêu cầu đề xuất giải pháp.

Ngoài ra, các bạn nào học hệ Chất lượng cao sẽ được ưu ái làm bài thi bằng tiếng Anh nhé.

> Một câu hỏi mình nghĩ có thể ra trong thi đó là hỏi xem thiết bị nào bên dưới thuộc kiểu X với X là 1 trong 4 kiểu client giới thiệu ở trên. Tuy nhiên khả năng thấp thôi do ví dụ trong slideset 01 nó khá cổ đại rồi.
{: .prompt-info }

## I. Introduction and fundamental laws

### Client-Server

A client-server system includes:
- One or more clients which use the services of the server (consumers).
- A server which provieds services (producer).

The connection will be established by the clients and the communication will work according to a protocol.

The client-server architecture consists of 2 layers and is called two-tier model.

For a distributed application, based on the client-server architecture, 5 **tasks** can be separated from each other:
1. Display (graphical) UI
2. Calculation of (graphical) UI
3. Data processing
4. Data management
5. Data storage

According the their areas of responsibility, 4 types of **clients** exist:
1. Text-Terminal or X-Terminal: Only task (1).
2. Thin/Zero Clients: Task (1) and (2).
3. Applet Clients or Network Computers: Task (1), (2), and a part of (3). The client process the applications themselves  
4. Fat Clients:  Task (1), (2), and (3).

> Một câu hỏi mình nghĩ có thể ra trong thi đó là hỏi xem thiết bị nào bên dưới thuộc kiểu X với X là 1 trong 4 kiểu client giới thiệu ở trên. Tuy nhiên khả năng thấp thôi do ví dụ trong slideset 01 nó khá cổ đại rồi.
{: .prompt-info }

Thin clients are better than fat clients in terms of cost efficiency mainly due to low power consumption. Let say we have a company running **500** computer and the electricity price is 0.32$ per kWh. We will calculate the electricity costs per year (including the leap year) for the two scenarios of using fat clients and thin clients.

**Scenario 1: Fat clients**

- Electrical power rating per PC: 450 watts
- Electrical power rating per screen: 80 watts
- Total cost is:
	$0.53kW * 24hours/day * 365.25day/year * 0.32 * 500 = 743,356.8\$$

**Scenario 2: Thin clients**

- Electrical power rating per PC: 30 watts
- Electrical power rating per screen: 80 watts
- Electrical power rating per serrver blade: 600 watts
- Each server blade can handle 30 thin clients.
- Total cost for 500 thin clients: $154,281.6\$$ (calculate the same way as fat clients)
- We need 17 server blades to handle 500 computer, so total cost for these are $28,612.23\$$
- Add the two costs above and we get the final cost of $182,893.83\$$

Nowadays, *nobody*  uses Text-Terminal anymore, very few use Thin Clients, and Fat Clients are the **standard** for usage.

### Fundamentals and Laws

There **3 options** to reduce the time needed for solving the computation-intensive tasks:
1. Optimization of algorithms used => They can't be optimized infinitely.
2. Use faster CPUs => This also can't be increased infinitely.
3. Add more computer system => Potentially unlimited but will be limited by the perfomance of the nodes, transfer rate of network and the maintenacne as well as administration effor for the connected system.

#### Moore's Law

This is not a natural law but rather a **rule**, based of emprical observartion (nói trắng ra là quy luật tự suy ra từ thực nghiệm quan sát được). Originally, it was about **electrical components** of integrated circuit. Today, it is about the number of **transistors**.

The law: **Basically, the number of transistors on a computer chip doubles roughly every two years** (That's it, really short).

#### Von Neumann Bottleneck

The data and control bus is increasingly becoming a bottleneck between the CPU and memory.

Take a look at the image below, the CPU needs both instruction and data from Memory Unit to function. But, these two flows share the same path, through the Bus System. Therefore, only one thing (instruction or data) can be transferred at a time. This causes waiting and when there's too much waiting, it creates a bottleneck! (Nói chung đường thì chỉ có một lane nhưng cần tới 2 đối tượng đi qua đường nên anh này đi xong thì mới tới lượt anh kia. Nhiều anh đợi nhau lâu quá thì kẹt xe => bottleneck).

![img](assets/img/other/distributed-computing-review/von_neumann_arch.png)

**Caches** can reduce the bottleneck impact because its is SRAM (Static Random Acess Memory), which has access speed close to the CPU speed. Where the main memory is DRAM (Dynamic RAM), which is slower.

If multiple CPUs (or cores) share the main memory => Bottleneck impact grows.

#### Amdahl's Law

This law calculates the maximum expected acceleration of programs by parallel execution on multiple CPUs. The performance gain is limited mainly by the **sequential part** of the problem.

A program can never be fully executed in parallel i.e. initiallzation and memory allocation or if they depend on sequence of events, input-output and of intermediate results.

We define $P$ and $(1-P)$ as the *parallel* and *sequential* portion of the program respectively. With that, we have the total runtime of a program:

$$ 1=(1-P) + P$$

If a program requires 20 hours of CPU time with a single CPU and in one hour, the process needs to run sequentially. The remaining 19 hours can be distributed to other CPUs for parallel execution. But, the total computation time can never fall under a single hour, not even with **infinite number** of CPUs

Thereby, we have the formula of:

$$ S = \frac{1}{1-P+P/N + o(N)} \le \frac{1}{1-P}$$

- $N$ = number of CPUs
- $P/N$ = accelerated parallel portion
- $S$ = SpeedUp factor or acceleration
- $o(N)$ = the load caused by communication and synchronization, it grows when N grows


Amdahl's law does not take into account the **cache** and its effects. In optimial case where the entire data can be stored in cache (very rare), a **super-linear** SpeedUp may occur and leads to an acceleration greater than the additional computer power (S > **p** CPU cores).

A key takeaway from this law is: "*With a growing number of CPUs, the problem size should grow too*" => Gustafson's Law.

#### Gustafson's Law

This law says that *"a problem, which is sufficiently large, can be parallelized efficiently"* 

Difference to Amdahls' Law:
- The parallel portion of the problem grows with the number of CPUs.
- The sequential part is not limiting, because it gets more and more unimportant as the number of CPUs rises.

The formula is as follows:

$$ S = (1-P) + N*P$$

With this, if the number of CPUs grows to infinity, the SpeedUp grows linear with the number of CPUs.

#### Indentify the two laws

![](/assets/img/other/distributed-computing-review/two_laws.png)

- **Top Row**: Illustrates Amdahl's Law, showing diminishing returns on performance as the number of cores increases (Tăng số core CPU lên nhưng hiệu quả tăng ít hoặc thậm chí không cải thiện).
- **Bottom Row**: Illustrates Gustafson's Law, showing more linear speedup with increasing cores for larger problem sizes (Problem càng lớn, sự hiệu quả khi tăng core CPU cũng lớn theo).

> Từ đây thì mình nghĩ cũng có thể là một câu trắc nghiệm mà trong đó, người ta cho vài biểu đồ hiệu suất SpeedUp rồi hỏi mình nó đang theo Law nào.
{: .prompt-info }

### Parallel computers

**Sequential operating computers** which follow the Von Neumann architecture are equipped with: 
- A single CPU 
- A single main memory for the data and the programs

For **parallel computers**, 2 fundamentally different variants exist:
- System with shared memory: The entire memory is part of a uniform address space, which is accessed by all CPUs. The memory is accessed via an interconnect
- System with distributed memory (a Cluster or Multicomputer): Each CPU can only access its own local memory. The communication between the CPUs takes place via a network connection (very slow). Every single CPU (and its local memory) is an independent node.

In **shared memory** system:
- Most multiprocessor systems today operate according to the **symmetric** multiprocessing (SMP) principle. This allows dynamically distributing the running processes to all available CPUs and they can acess the memory with the same speed.
- In **asymmetric** multiprocessing principle, each CPU must be assigned to a fixed task. Some run the OS, the remaining run the other processes.

## II. Cluster and grid.

## III. Cloud computing

### IaaS

### PaaS

### SaaS

## IV. Hadoop


