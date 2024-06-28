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

### Cluster definition

A cluster consists of at least 2 nodes. The nodes are connected via a computer network. Often, the nodes are under the control of a master and are attached to a shared storage.

From the user perspective, the cluster works like a single system => a virtual uniprocessor system (they don't know it's a cluster).

If the nodes are only available at specific times (working hours), the cluster is called Clusters of workstations (COWs) or Network of Workstations (NOWs).

Advantages: 
- Flexibility and extensibility
- Lower purchase price campared with supercomputers
- Simple replacement of commodity hardware components

Drawbacks:
- Errors occur more often compared with a single supercomputer
- Clusters consist of many independent systems => Higher administrative costs
- High effort for distributing and controlling applications

### Distinguishing Criteria of Clusters

**Structure**
- Homogeneous structure: One OS across the cluster 
- Heterogeneous structure: Different OS in the cluster => A really bad idea, managing will be hell.

**Installation concept**
- Glass-house: The cluster is located in a single room or server rack => Better access and maintenance but in case of power failure, the entire thing goes down.
- Campus-wide: The nodes are located in multiple buildings and spread across the site of the research center or company =>Hard to fail completely but impossible to use high performance network between nodes.

**Fields of application**
- High performance (scalability): Achieve by redundancy of nodes, avoiding a single point of failure.
- High availability:  It is not sufficient to use redundant nodes and redundant hardware inside the nodes; Uninterruptible power supplys (UPS) are require as well as protection against improper use, and sabotage. The availability of a system is calculate as follows:
$$availability = \frac{mean(uptime)}{mean(uptime) + mean(downtime)}$$
- High throughput

**Behavior in the event of failed nodes**
- Active/Passive: During normal operation, at least a single node is in passive state (bro doesn't do anything). If a node fails, the passive node takes over its services.
- Active/Active: All nodes are in active state and run the same services. If nodes fail, the remaining active nodes need to take over their tasks.

	Comparision between the two: A/P obviously doesn't utilize all the available performance as some are passive nodes. Meanwhile, services in A/A must be designed for cluster operation because all nodes access shared data simultaneously.

Below is the classification of clusters, the A/P and A/A we have just discussed belong to HA clusters. We will go into more details of these cluster types in the subsequent sections.

![](cluster_classification.png)


### High Availability Clustering

**Failover**: Ability (usually provided by the OS) to automatically transfer the tasks of a failed node to another node for minimizing the downtime. Example: Heartbeat for Linux.

**Failback**: If failed nodes are operational again, they report their status to the load balancer and get new jobs assigned in the future.

The **Split Brain** problem: When communication between cluster nodes is severed, they enter a split-brain state. Each node is unaware of the others and believes itself to be the sole active node (primary node) in the cluster. This could lead to Data inconsistency, Reduced availability and Conflicting decisions.

There are 2 architectures of High Availability Clustering:
- Shared Nothing => Distributed Storage
- Shared Disk => Shared Storage

#### Shared Nothing

In a Shared Nothing cluster, each node has its own storage resource. Even, when a resource is physically connected to multiple nodes, only a single node is allowed to access it.

- Advantage: No lock management is required, no protocl overhead and in theory, can scale in a linear way.

- Drawback: Money for storage.

Distributed Replicated Block Device (DRBD), which operates at **block** level inside the Linux kernel, is a free software to build up a network storage for Shared Nothing clusters, without an expensive Storage Area Network (SAN). **Shared storage is always a single point of failure**, since only the cluster nodes are designed in a redundant way.

RBD can be used as a basis for:
- Conventional file systems, such as ext3/4 or ReiserFS
- Shared-storage file systems, such as Oracle Cluster File System (OCFS2) and Global File System (GFS2)
- Another logical block device, such as the Logical Volume Manager (LVM)

Shared Nothing with DRB: 
- A primary server and a secondary server exist. Write requests are carried out by the primary server and afterwards are send to the secondary server. If the secondary reports successful writing, the primary reports the end of the operation.
- Practically, it implements RAID 1 via TCP. If the primary fails, the secondary becomes the primary.
- Read access is always carried out locally => better performance.

#### Shared Disk

In a Shared Disk cluster, all nodes have access to a shared storage. Some methods of connecting the nodes to the storage:
- SAN (Storage Area Network) via Fibre Channel: Expensive but high performance. Provides block-level access.
- NAS (Network Attached Storage): Easy to use, provides file system-level access, can be implemented as a pure software solution.
- iSCSI (Internet Small Computer System Interface): Use SCSI protocol via TCP/IP, provides SAN-like access via the IP-network

### High Performance Clustering

Typical application area:
- Applications which implement the Divide and Conquer principle.
- Applications used for analyzing large amounts of data

Examples: Rendering a Pixar movie,  flight path calculation, prime number computation, etc.

Advantages: Low price and vendor independence, easy to increase the performance in a short time via additional nodes, defective components can be obtained in a quick and inexpensive way.

Drawback: High administrative and maintenance costs, compared with mainframes

If a free operating system is used => **Beowulf** cluster. If Windows (paid and evil OS) => **Wulfpack**.

In Beowulf:
- It is never a cluster of workstations, nodes of a Beowulf cluster are used only for the cluster.
- The cluster is controlled via a master node.
- Worker nodes are only accessible via the network connection (no I/O devices)
- Worker nodes contain commodity PC components and are not redundant
### High Throughput Clustering

Cluster consists of servers, which are used to process incoming requests, not used for extensive calculations (Tasks must **not** be split into sub-tasks).

Typical fields of application:
- Web servers
- Search engines

Large compute jobs => High Performance Cluster

Multiple small compute jobs (in a short time) => High Throughput Cluster

### Libraries for Cluster Applications

Popular message passing systems:

|                  |      Parallel Virtual Machine (PVM)      |         Message Passing Interface (MPI)         |
| :--------------: | :--------------------------------------: | :---------------------------------------------: |
|     Purpose      | Provides a uniform programming interface | Collection of functions to simplify development |
| Language support |         C/C++ and Fortran 77/90          |                   Same as PVM                   |
|   Environment    |              Heterogeneous               |                   Homogeneous                   |
|      Focus       |               Portability                |            Performance and security             |

> Đoạn này trong slideset 02 là có gần 30 slide nói chi tiết về code và function của MPI. Vì sức khỏe tinh thần, mình từ chối xem qua đống đó nên các bạn có thể tự tham khảo từ slide thứ 42 nếu muốn.
{: .prompt-warning }

We also have **Gearmean**, a framework for developing distributed applications which supports C, Pearl, PHP, Python, C#, Java, .NET and UNIX shell.
- Assigns one of 3 roles to every computer involved:
	- Clients transfer jobs to the Job Servers
	- Job Server assign jobs of the clients to the Workers
	- Worker register themselves at Job Servers and execute jobs
	
![](gearman_roles.png)
- Gearman should only be used in secure private networks since the communication is **not encrypted** (port 4730) and it has no mechanism for the authentication.
- Clients and workers access shared data. Cluster file systems like GlusterFS or protocols such as NFS or Samba can be used.

## III. Cloud computing

### IaaS

### PaaS

### SaaS

## IV. Hadoop


