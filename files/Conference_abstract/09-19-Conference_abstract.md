# 2019.09.19 RDMA & NFV

## [黄程远](https://github.com/fnlab738/Weekly-Discussions-Archive/blob/master/files/2019/09/09-19-Chengyuan-RDMA_over_Commodity_Ethernet_at_Scale.pptx)

### RDMA

### 传统 TCP/IP 协议的问题

> 1. TCP的尾部延时过大
> 2. TCP协议对CPU的占用很大，短流的处理时延较大

### RDMA

> 1. 全称为 Remote Direct Memory Access : 通过RDMA，两台主机可以越过内核进行数据通信。
> 2. 数据延时可以分为两部分：传输延时和处理延时，对于长流来说传输延时影响更大，而对短流来说，数据的处理延时更重要。
> 3. 数据中心中短流占绝大多数，因而降低处理时延对数据中心的性能提升至关重要。

### 无损以太网

>1. PFC控制流量：通过PFC暂停帧暂停上游节点的数据包发送。
>2. DCQCN 进行传输层控制：使用PFC + ECN + 基于速率的硬件拥塞控制

### 部署，安全以及性能挑战

>1. 基于VLAN的PFC ：数据中心中缺乏在三层网络中携带VLAN标签的方式  ---> 解决方式：基于DSCP的PFC
>
>2. RDMA传输活锁（livelock）：Go-Back-0 ---> 解决方式：Go-Back-N
>
>3. PFC暂停指针风暴 ： 故障NIC导致整个网络的拥塞 ---> 解决方式：在NIC和交换机的边缘安置看门狗
>
>4. PFC死锁：暂停指针循环导致的死锁 ---> 解决方式：从ARP表中找不到IP地址时丢弃数据包，防止广播的出现
>
>5. slow-receiver

### 部署经验

>1. 网络无损很困难
>
>2. 死锁、活锁、PFC暂停帧传输以及PFC风暴确有其事
>
>3. 对故障要未雨绸缪：配置管理、延迟、PFC暂停帧、RDMA流量监控
>
>4. NIC是RoCEv2能够正常工作的关键

### 可以做什么

>1. Applications:RDMA for X （Search, Storage, HFT, DNN, etc.）
>
>2. Architectures: Software vs hardware; Lossy vs lossless network; RDMA for heterogeneos computing systems
>
>3. Technologies: RDMA programming; RDMA virtulization; RDMA security; Inter-DC RDMA
>
>4. Protocols: Practical, large-scale deadlock free network; Reducing collection damage

### 总结：数据中心RDMA

>1. RDMA正在数据中心中经历一场复兴：RoCEv2已经安全地在微软的数据中心中运行了两年半
>
>2. 关于高速、低时延RDMA网络有许多机遇以及有趣的问题
>
>3. 有许多机会可以让更多的开发者接触RDMA

### Ideas：Inter-DC RDMA

>1. 在inter-DC的长肥链路下使用，降低CPU使用，提升吞吐量
>2. 限制：PFC 无法使用；长肥链路导致在网卡上track每个包的到来，需要花费较多的memory；丢包无法避免，精确地在某个交换机上控制队列长度变得比较困难；
>3. 基于冗余信息的RDMA包恢复
>4. MP-RDMA的数据包调度算法：在RDMA网卡上实现数据包调度器，进一步减小收端的buffer占用

## [王泽南](https://github.com/fnlab738/Weekly-Discussions-Archive/blob/master/files/2019/09/09-19-Zenan-Hieff.pptx)

### Background & Motivation

>1. Typical NFV Architecture: Orchestrator, VNF Manager, Network controllor, Switch（LB）,VNFs(The same type)
>2. 针对数据中心east-west流量的SFC：有必要为east-west流量部署网络功能，如AppFirewall
>3. 数据中i性能流长分布：80%的流小于10KB，绝大多数的Bytes由数量仅占前10%的长流产生； 80%的流持续的时间小于11秒，60%的流持续时间不超过1秒

### Design-使用MV-sketch探测长流

>1. Strawman solution -- 基于流表对heavy flow进行统计存在缺点
>2. MV-Sketch：一种基于概率统计的数据结构和算法
>3. MV-Sketch在网络领域的应用：以低开销和高精确度来检测heavy flow和 haavy changing flow
>4. 一致性hash：非一致性hash存在许多缺点
>5. Anchor-hash：一种一致性hash算法的实现
>6. 在网络领域的应用：在一个集群中使用hash实现负载均衡

### NFV研究方向 -- Placement

>1. NFV & Placement相关的论文数量仍然是最多的，从12年开始不同水平的论文都还在持续发表
>2. NFV Verification and Validation 必读科普文：2019-Communication Magazine Introducing Automated Verification and Validation for Virtualized Network Functions and Services

### NFV研究方向 -- Relability

>1. 可靠性机制：故障检测，故障检测预测，故障诊断定位
>2. 故障恢复技术展望：可靠性的评估和计算；联合优化工作和备份SFC的拓扑设计和映射；备份资源间的共享机制；动态服务需求下的备份资源设计
>3. 更多详细查看 虞红芳-网络功能虚拟化基础设施的可靠性问题.pptx 

### NFV可关注的项目 -- LeanNFV & Nefeli

> 由伯克利 Scott Shenker主导的项目LeanNFV和创业公司Nefeli；LeanNFV中提到的一点：LV-store

### NFV可关注的项目 -- ONAP

> 由全球范围内运营商，设备商共建的NFV项目，是AT&T ECOMP前身，目前正在持续迭代
