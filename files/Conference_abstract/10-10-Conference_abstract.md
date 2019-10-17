# 2019.10.10 RDMA & NFV

## [石佳明：Congestion Control for RDMA](https://github.com/fnlab738/Weekly-Discussions-Archive/blob/master/files/2019/10/10-10-Jiaming-RDMA%E7%BB%84%E4%BC%9A.pptx)

### RDMA对比传统TCP 

> 存储和计算能力的提升使得网络成为系统性能的瓶颈

> 传统TCP中内核的延迟很高，而RDMA可以跳过内核，从而降低时延
> TCP需要计算头部等，因而短流的处理延迟会占用CPU的性能
> 传统TCP即便跑满CPU的性能也无法把速率提到40GB

> RDMA可以快速启动，最大化网络带宽的使用
> 减少内核的延迟

### 以太网上的RDMA

> 无损网络中的PFC：PFC可能导致头部阻塞以及死锁问题
> 无慢启动：导致大规模的incast阻塞问题
> 使用回退N步（Go-back-N）的重传策略：导致活锁问题（Livelock）

### 相关工作

> 拥塞控制：
> DCQCN -- SIGCOMM ‘15
> TIMELY -- SIGCOMM ’15
> IRN -- SIGCOMM ’18
> RoGUE -- SoCC ’18
> DCQCN+ -- ICNP ’18
> HPCC -- SIGCOMM ‘19

> 死锁预防：
> Tagger -- CoNEXT ‘17
> GFC -- SIGCOMM ‘19

> DCQCN：基于发送速率以及ECN
> 融合了QCN算法以及DCTCP算法
> 由三个部分组成：发送端、交换机、接收端

> TIMELY ：基于速率以及延迟
> 每个传输事件结束就计算一次RTT，然后将新的RTT与预设的RTT上下界进行比较
> 如果小于下界，就增大下一次发送时的发送熟虑，如果大于上界，就减小发送速率
> 如果在上下界之中，计算新旧RTT的差值，差值大于0，减小速率，差值小于0，增大发送速率

> IRN
> 不需要无损网络
> 使用选择重传
> 基于BDP（Band Delay Production 带宽时延积）

> RoGUE 
> 基于发送窗口以及时延
> 增加一个中间层，把RDMA的拥塞控制和丢包恢复功能提到了软件层上
> 使用RTT和丢包来估计拥塞

> HPCC
> 通过INT精确测量 inflight bytes
> 使用MIMD快速调整发送速率

## [孙苑耀](https://github.com/fnlab738/Weekly-Discussions-Archive/blob/master/files/2019/10/10-10-Yuanyao-VNF-scaling-Lyapunov-optimization.pptx)

### Introduction
> 1、为了实现有效的服务协调和管理，需要快速合理地分配和调度网络资源的问题：
>       流量增加时，配置恰当数量地VM以满足服务需求
>       回收过剩的资源，避免资源浪费

> 2、缩扩容策略：
>      扩容：基于阈值地静态策略、时间序列分析等
>      缩容：通过计算及比较选路及多条路径传播地cost + VM启用的cost

> 3、缩扩容时的几个步骤：
>      有限的带宽限制条件下缩扩容之后如何分配流量

### Related Work
> 流量调度策略-部分节点间选路
> Infocom18 - Adaptive vnf scaling and flow routing with proactive demand prediction

> 流量调度策略-VNF与链路带宽
> JSS19- ElasticSFC: Auto-scaling techniques for elastic service function chaining in network functions virtualization-based clouds

> 流量调度策略 - 整条路径选路
> Hotmiddlebox16 - Adaptive Service-Chain Routing for Virtual Network Functions in Software-Defined Networks( 单一标准选路)
> ICNP15 - Multi-criteria routing in networks with path choices(多标准选路)

> 缩扩容策略-缩
> Infocom18-Adaptive vnf scaling and flow routing with proactive demand prediction(时间序列分析)
> 基于阈值的静态策略(上下门限值)



### Idea

> 缩扩容方案问题
> 1、对于扩容，需要快速的满足流量对资源的增长需求
>      二分法、装箱问题等决定扩容VNF数量等
> 2、对于缩容，寻找在能够满足flow的条件下cost最小的情况，通过计算及比较选路及多条路传播的cost+VM启用的cost

> 选路问题
> 多标准选路


### 李雅普诺夫优化

> 多应用于排队控制理论，使用Lyapunov函数来最优地控制动态系统,在保证包排队积压(Q(t))不过大的同时，降低资源消耗 (P(t))
> 资源消耗(P(t))：选路及多条路传播的cost+VNF启用的cost
> 引入控制参数V，调整cost与队列时延的优化目标。
> 将优化目标转换为获取这个加罚函数的上限并不断尽可能的减小这个上限


## 张老师建议

> 参考文献要找最近的，而且最好找高水平的论文

> 根据中心思想深挖，把思路做扎实，不要只是做表面工作，思想要足够深入，把思想做的比较完善

> RDMA 拥塞控制 和TCP的拥塞控制差别在哪里？还是需要对协议等细节部分进行讨论分析，了解协议限制以及要求，之后再想方案。

> 思考应该非常深入，方案足够合理，为什么方案是足够好的，现有问题要调查清楚，做好对比方案，并挑选确定最优方案，方案不能拍脑门，应该是各种对比调查之后再确定好一个方案。












