---
layout: post
title: 并行计算
date: 2020-10-16
tag: [summary]
---

本文将汇集整理并行计算的一些基本概念，以及几种用于并行编程的工具/框架。

<!--more-->

------------

## 概述
### 什么是并行计算？
早期传统软件为串行计算而设计，在仅有一个处理器的单机上运行；将一个问题分解成离散的串行指令序列求解；指令是一个接一个执行；在任意时间任意时刻，只有一条指令在执行。

而并行计算可以简单定义为**同时利用多个计算资源解决一个计算问题**。
* 程序运行在多个CPU上
* 一个问题被分解成离散可并发解决的小部分
* 每一小部分被进一步分解成一组指令序列
* 每一部分的指令在不同的CPU上同时执行
* 需要一个全局的控制和协调机制

可并行的计算问题应该满足：
1. 可分解成同时计算的几个离散片段
2. 在任意时刻可同时执行多条指令
3. 多计算资源所花的时间少于单计算资源

计算资源一般为：
1. 具有多处理器/多核的单台主机
2. 通过网络连接的若干数量的主机

并行程序中的协作：**通信、负载均衡、同步**

### 并行计算的优势
并行计算是现实生活的需要
1. 在自然界，很多复杂的、交叉发生的事件是同时发生的，但是又在同一个时间序列中
2. 与串行计算相比，并行计算更擅长建模、模拟、理解真实复杂的现象

优势有以下几点
* 节省时间和花费
    * 理论上，给一个任务投入更多的资源将缩短任务的完成时间，减少潜在的代价
    * 并行计算机可以由多个便宜、通用计算资源构成
* 解决更大／更复杂问题
    * 很多问题很复杂，不实际也不可能在单台计算机上解决，例如Grand Challenges
* 实现并发处理
    * 单台计算机只能做一件事情，而多台计算机却可以同时做几件事情
    * 例如协作网络，来自世界各地的人可以同时工作
* 利用非本地资源
    * 当本地计算资源稀缺或者不充足时，可以利用甚至是来自互联网的计算资源。
    * SETI@home (setiathome.berkeley.edu), Folding@home (folding.stanford.edu)
* 更好地发挥底层并行硬件
    * 现代计算机甚至笔记本都具有多个处理器或者核心
    * 并行软件就是为了针对并行硬件架构出现的
    * 串行程序运行在现代计算机上会浪费计算资源

### 并行计算的用途
* 科学和工程计算（“高端计算”）
    * 大气、地球、环境
    * 物理学如核能、粒子模拟、高压、高分子等
    * 生物科技、遗传学
* 工业和商业应用：当前，商业应用对于高速计算机的开发起到了巨大的推动作用
    * “大数据” 、数据库、数据挖掘
    * 石油勘探
    * Web搜索引擎，医学图像处理等等

### 并行计算的推动力
#### 应用发展趋势
* 在硬件可达到的性能与应用对性能的需求之间存在正反馈（Positive Feedback Cycle），例如应用领域：
    * 科学计算： 生物、化学、物理……
    * 通用计算： 视频、图像、CAD、数据库……
* 大量设备、用户、内容涌现
* 大数据正逐渐成为新的生产资料，大数据承载的“大” 信息将成为21世界的动力来源
* 云计算的兴起：
    * 廉价的硬件，应用弹性的扩展
    * 应用种类繁多，负载异构性增加

#### 架构发展趋势
迄今为止，CPU架构技术经历了四代即：电子管(Tube) 、晶体管(Transistor)、集成电路(IC)和大规模集成电路(VLSI)，这里只关注VLSI。
VLSI最大的特色是在于对并行化的利用，不同的VLSI时代具有不同的并行粒度：
1. 1985年之前： **bit水平的并行**，从4bit->8bit->16bit（数据通路）
    * 32bit后并行化速度放慢，最近几年64bit才被广泛使用，128bit还很遥远（非性能限制）
    * 32bit并行技术的应用导致了计算机性能的显著提高
2. 80年代到90年代： **指令水平的并行**
    * 流水线、简单指令集、先进的编译技术(RISC)
    * 片上缓存(caches)和功能部件=>超变量执行
    * 更复杂控制机制：乱序执行、推测、预测，用于控制转换和延迟问题
3. 现代并行计算的主要方式： **线程并行**

Moore定律失效（功耗墙），发展趋势不再是高速的CPU主频，而是“多核”

1990年之前的解决方式：
1. 增加时钟频率（扩频）
    * 深化流水线（采用更多/更短的流水阶段）
    * 芯片的工作温度会过高
2. 推测超标量(Speculative Superscalar, SS)：多条指令同时执行（指令级的并行，ILP）
    * 硬件自动找出串行程序中的能够同时执行的独立指令集合
    * 硬件预测分支指令在分支指令实际发生之前先推测执行
    * 局限： 最终出现“收益下降”(diminishing returns)
    * 这种解决方法的优点： 程序员并不需要知道这些过程的细节

2000年之后的解决方式：
1. 时钟频率很难增加
2. SS触到天花板出现“收益下降”
3. 利用额外的额外的晶体管在芯片上构建更多/更简单的处理器（多核multicore/众核many-core）

未来属于异构、多核的SOC架构，SOC = System On Chip

Moore定律新解：
1. 每两年芯片上的核心数目会翻倍
2. 时钟频率不再增加，甚至是降低
3. 需要处理具有很多并发线程的系统
4. 需要处理芯片内并行和芯片之间的并行;
5. 需要处理异构和各种规范（不是所有的核都相同）

### 并行计算机
从硬件角度来讲，今天的单个计算机都是并行计算机，主要体现为：
* 多个功能单元（L1 Cache、L2 Cache、Branch、Prefetch、GPU等）
* 多个执行单元或者核心
* 多个硬件线程

多个单独的计算机通过网络连接起来形成计算集群，如LLNL并行计算集群
* 每个节点都是一个多处理器并行机
* 多个计算节点通过Infiniband网络连接

并行计算不仅仅是硬件，还包括了并行编程
* VAX指令的谬论：
1. 对于高阶抽象只用一条指令表示
2. 单条硬件指令真的快吗？
* RISC指令的设计思想：
1. 全系统设计
2. 从全局角度设计硬件机制
3. 跨组件优化
* 当前程序员看不到汇编语言，甚至看不到底层C语言。

### 并行编译器的局限
经过30多年的研究：
* 编译器仅做到了有限的并行检测核程序转换：
1. 执行级的并行，只能检测到basic block
2. 少量的嵌入的for-loops
3. 分析技术如数据依赖分析、指针分析、流敏感分析等，难以应用到大规模程序中
* 比较成功的做法是让人去学习如何编写并行程序

### 并行编程的难点
* 找到尽可能多的并行点(Amdahl's Law)
* 粒度：函数级并行、线程级并行还是进程级并行
* 局部性：并行化后是否能够利用局部数据等
* 负载均衡：不同线程、不同 core之间的负载分布
* 协作与同步
* 性能模型：所有这些难点让并行编程要比串行编程复杂得多

### 并行编程与串行编程的比较
* 编程代价不同、优势不同
* 并行编程需要不同的，不熟悉的算法
* 并行编程必须利用不同的问题抽象
* 并行程序的行为更复杂
* 更难以控制程序不同组件之间的交互
* 需要掌握更多编程工具、更多知识等

### 如何编写并行程序
任务并行：
* 计算密集型，可拆分成不同的子任务
* 将一个问题划分成多个不同的子任务分别在不同的core上运行

数据并行：数据密集型，大数据应用
1. 将问题所涉及的数据拆分到不同的core上执行
2. 每个core运行的是相同的程序

### Amdahl's Law
并行计算中最重要的即为Amdahl's law，用于度量并行程序的加速效果

$$Speedup=\frac{1}{(1-p)+p/n}$$

* 应用程序只有一部分可以并行
* 大量的串行代码降低并行性能

它给出了并行程序的加速比，其中$$p$$是可并行占用的时间比例，$$1-p$$为串行占用的时间比例，$$n$$为线程的数目。

但太过乐观估计
* 没有考虑**并行的开销**，如创建终止线程等（这会随着核数/线程数的增加而增大）
* 假设计算是均匀分配在各个核的，但实际上经常**负载不均**，核的等待造成开销

新的加速比公式：$$\sigma$$为串行部分时间，$$\varphi$$为并行部分时间，$$n$$为问题规模，$$p$$为核数，$$\kappa$$为并行开销，如下所示

$$\psi(n,p)\leq\frac{\sigma(n)+\varphi(n)}{\sigma(n)+\varphi(n)/p+\kappa(n,p)}$$


------------------

## 并行架构
### Flynn分类法
按指令(Instruction, I)和数据流(Data, D)的数目划分：SISD、MISD、SIMD、MIMD

### 单核并行
计算机架构的目标(2002年以前)：将底部设施的并行从OS、编译器、程序员中隐藏

ILP技术
* 流水线
* 超标量执行
* VLIW
* 向量处理
* 乱序执行
* 预测执行

TLP技术，要区别并发和并行
* 并发(concurrency)：并没有明确的时间先后，可以同时执行（执行时段可重叠），如单核的多任务
* 并行(parallelism)：就是**同时**执行，如多核多线程

结合线程级并行和指令级并行：同时(simultaneous)多线程/超线程(hyperthreading)

内存带宽的影响：内存带宽由内存总线和内存单元数目决定
* 通过提升内存块大小可以提升（但并不改变延时）
* 提升总线宽度，但造价较高
* 充分利用带宽的方式：局部性，假设邻近单元会在邻近指令中用到

现在通用提升cache性能的方法是blocking/tiling：通过divide-and-conquer设计问题使得能够符合寄存器/L1/L2的大小

### SIMD
Single Instruction Multiple Data ([SIMD](https://www.codingame.com/playgrounds/283/sse-avx-vectorization/what-is-sse-and-avx))是提升CPU性能一个很重要的技术，即数据并行---在CPU内部添加向量寄存器
* Multi Media Extensions ([MMX](https://en.wikipedia.org/wiki/MMX_(instruction_set)))：Intel最早的SIMD尝试(1997)，于Pentium系列CPU
* Streaming SIMD Extensions ([SSE](https://en.wikipedia.org/wiki/Streaming_SIMD_Extensions))：最早在Pentium III引入，由SSE1到SSE4.2，均为128位寄存器
* Advanced Vector Extensions ([AVX](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions))：Intel Sandy bridge架构引入256位向量寄存器(2008)，后来又引入512位向量寄存器(2016)
    - `#include <immintrin.h>`
    - AVX2: Haswell, 2011
![AVX-256](https://www.codingame.com/servlet/fileservlet?id=16426525647340)


-------------

## 并行编程模型
程序员写应用时运用的库/API，定义了通信和同步机制，包括
* 多道程序：没有通信和同步
* 共享内存
* 消息传递：显式点到点
* 数据并行：更加严格，在数据上做全局动作，用共享地址空间实现

编程模型需要包括以下几个部分：
* 控制：并行如何被创建，操作应以什么顺序执行，不同线程如何同步
* 命名：数据应该私有还是共享，共享数据如何被访问
* 操作：什么操作是原子性的
* 开销：这些操作的开销有多大

### 共享内存模型
* 任务之间共享一块通用的内存地址空间，无需显式进行通信
* 编程简便，内存位置都是透明的
* 需要显式的锁/信号量来控制共享内存的访问
* 难以控制数据访问的局部性/本地性，有UMA(uniform memory access)和NUMA架构

并行程序可能出现的问题
* 线程创建开销
* 数据划分粒度
* 负载均衡：减小并行粒度，分划归并，master-worker，work-stealing
* 竞态(race condition, RC)问题/线程竞争处理
    - 临界区、锁、同步原语
    - 控制为局部变量
* 死锁：两个或多个线程等待对方释放资源
    - 上锁解锁要以同样的顺序
    - 原子操作

线程不安全的例子
* 访问全局/静态变量/堆
* 在全局空间分配释放资源，如文件
* 通过句柄或指针间接访问

最好的方式是局部更新变量/互斥锁，或者使例程可重入

### 消息传递/分布式内存模型
* 每个任务有自己的私有地址空间
* 交流通过显式的消息传递
* 开销来源于拷贝、buffer管理、保护

1980s已经有了消息传递库，但不统一/兼容性弱。
消息传递界面(MPI)第一版发布于1994年，MPI-2发布于1996年

### 数据并行模型
* 结构化的计算
* 同样共享地址空间


--------------------

## 并行方法论
### 增量并行化
增量式并行化通过研究一个串行程序，尝试找到可以并行的地方和可能存在的瓶颈，并尝试令所有的处理器保持做有用的工作。

### Culler的设计方法
* 分解(decomposition)：将原问题分解为多个能被并行的子问题
    - 分解不一定是静态的，也可以在程序执行时动态生成任务
    - 目标：使机器一直有活干
    - 核心：确定依赖关系
* 分派(assignment)：将线程/工人分配到每一个子问题上
    - 负载均衡(load-balanced)
    - 减少通信开销
    - 可静态或动态，即调度
    - 程序员对分解背锅，语言/运行时系统对指派背锅
* 协调(orchestration)：不同线程间的交流
    - 涉及结构化通信、添加维持依赖关系的同步、内存安排数据结构、任务调度
    - 目标：减少通信/同步开销，保留数据间的局部性
* 映射(mapping)：将并行执行逻辑（线程/工人）对应到硬件资源上
    - OS：pthread、编译器：ISPC、硬件：CUDA线程块
    - 将相关的线程放在同个处理器上

### Foster的设计方法
* 划分/分解(partition)
    - 按域/数据划分：将数据划分为片段，确定数据元素如何被分配到各个处理器上，确定每个处理器应该做什么任务
    - 按功能/任务划分：将计算划分为片段/流程，将任务划分到各个处理器上，然后决定每个处理器上哪些数据元素会被访问；流水线是特别的任务划分方式
    - 主要应把任务划分成**能够**并行的片段，核心是创造足够多的任务使得机器上所有执行单元都是忙碌的——要确定依赖关系
* 通信(communication)
* 归并(agglomeration)
    - 将小任务合并为大任务，消减通信，提升性能，维持可扩展性，减少编程工作量
* 映射(mapping)

### 探索更多并行可能性
三种基本并行类型：数据并行、任务并行、流水线，其实都可以被整合


-------------------

## 编程模型与工具
### [Pthreads](https://en.wikipedia.org/wiki/POSIX_Threads)
在POSIX(Portable Opearing System Interface for Unix)中，pthreads是关于线程的界面，用于创建同步线程

* 只有堆共享（要传指针），栈不共享
* 在main之外定义的全部变量被共享
* 通常需要传递一个线程数据结构体

注意：创建线程的开销依然很大，需要10k个量级的时钟周期，因此尽可能少创建线程，最好线程数等于核数。

### OpenMP
见[并行计算-OpenMP]({{ site.baseurl }}/summary/parallel-computing/openmp)

### Clik-Plus
见[并行计算-Clik]({{ site.baseurl }}/summary/parallel-computing/cilk)

### MPI
见[并行计算-MPI]({{ site.baseurl }}/summary/parallel-computing/mpi)

### [Intel TBB](https://software.intel.com/en-us/articles/migrate-your-application-to-use-openmp-or-intelr-tbb-instead-of-intelr-cilktm-plus?_ga=2.174275746.1279103381.1550824040-508775473.1544510410)
从Intel C++ Compiler 18.0开始Cilk Plus就被废除了，而交由MIT自己维护。
转而取代的是Intel Threading Building Blocks (TBB)
* `task_group t; t.run([](){ })`
* `t.wait()`
* `tbb::parallel_for()`

### CUDA
见[并行计算-CUDA]({{ site.baseurl }}/summary/parallel-computing/cuda)

### OpenCL
见[并行计算-OpenCL]({{ site.baseurl }}/summary/parallel-computing/opencl)

### MapReduce
见[并行计算-MapReduce]({{ site.baseurl }}/summary/parallel-computing/mapreduce)

### Spark
见[并行计算-Spark]({{ site.baseurl }}/summary/parallel-computing/spark)


------------

## 并行搜索算法
见[并行计算-搜索算法]({{ site.baseurl }}/summary/parallel-computing/search)


-------------

## 性能优化
### 任务分配及调度
一个迭代的过程，不断分解、重排、协调，目标是：
* 负载均衡：希望所有资源都能被最大化利用
* 减少通信：减少停滞(stalls)
* 减少多余工作

先实施一个最简单的方式，测试性能决定你是否需要做得更好（比如小机器/核数限制就没有必要用大并行）

两种指派方法：分块(blocked)、交替(interleaved)

实现负载均衡的方式
* 静态指派(assignment)：开销/执行时间和任务数量都是可预见的
* 半静态指派(semi-static)：近期(near-term future)是可预见的
    - 应用周期性地对自己做性能剖析然后重新调整指派，如n体模拟和适应性mesh
* 动态指派
    - 用一个shared work queue装子问题，然后worker threads都从这个队列中拿任务

关键在于选择合适的任务粒度(granularity)：粒度小容易**负载均衡**，但同步开销大，临界区串行部分多；粒度大**减少同步开销**。
通常通过负载(workload)和机器的特性决定粒度大小。

解决负载不均的方法：
* 将任务划分为更多更小的任务，但会增加同步开销
* 更好的调度方案：先调度长任务，需要对workload十分清楚
* work stealing（具体可见[Cilk Plus]({{ site.baseurl }}/summary/parallel-computing/cilk)）：分布式work queue，只有在确保能够负载均衡时才偷，否则开销很大

### 通信(communication)
* 延时：从发送内存请求到数据可被处理器使用所花的时间
* 带宽：数据能够被发往处理器的频率

实际上只要是内存访问都可以称之为通信，包括
* 天然(inherent)通信：算法中不可避免要通信的部分，它给出问题是如何被分解及指派的
    - 运算密度(arithmetic intensity)：计算量(指令数)/通信量(B)
    - 如`C[i] = A[i] + B[i]`运算密度为1/3（两读一存一算）
* 人为(artifactual)通信：其他通信，与系统实施相关
    - 如最小传输粒度：导致系统需要传比需要更多的数据，如读入整个cache line
    - 操作规则(?)：存储16个连续4B的浮点数，整个64B cache line会从内存被加载，随后再写入内存(2x)
    - 数据的放置：特别是在分布式存储中，数据放在远端
    - 有限的复制容量(replication capacity)：由于cache太小，导致相同数据会被重复多次加载
    - 维护cache一致性的通信：跨两个处理器的数据

Cache的四个C
* Cold miss：冷启动
* Capacity miss：容量太小，可以通过增大cache解决
* Conflict miss：替换策略，通过增大组关联度解决
* Communication miss：在并行系统中由于天然或人为导致的miss

提高cache局部性的方式
* 通信/数据传输、一致性粒度
* 改变格点遍历顺序
* 循环融合

冲突(contention)：在邻近时间内对同个资源的大量请求(hotspot)，如更新一个共享变量（树结构可减少冲突，但带来更大的latency）
* 用work stealing减少对局部work queue的冲突，即远端线程

减少通信开销的方法：
* 减少天然通信：发送少信息，信息包更大（均摊开销），合并(coalesce)小信息
* 减少人为通信：blocked data layout
* 重构代码以提升局部性减少延迟，改进通信架构
* 减少冲突：局部拷贝，细粒度锁，对竞争(contended)资源交错(stagger)访问
* 提升通信计算重叠性：异步通信，流水线/多线程/预取/乱序执行

----------

## 性能分析工具
程序优化
* gprof
* perf
* VTune

程序debug
* valgrind
* sanitizer

进阶分析
* pin
* contech (task analysis)
* LLVM