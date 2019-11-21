# 2019.11.14 VNF Scaling and Placement & GNN

### 孙苑耀：VNF Scaling and Placement

### Introduction

> 为实现有效的服务协调和管理，需要快速合理地分配和调度网络资源的问题

> 流量增加时，配置恰当数量地VNF以满足需求

> 回收过剩的资源，避免资源浪费

> 缩扩容策略

> 扩：基于阈值的静态策略、时间序列分析等

> 缩：通过计算及比较 选路及多条路传播的cost+VM启用的cost

> 缩扩容时的几个步骤

> 有带宽限制的条件下  ->  缩扩容后，流量怎么分配

### Related Work

> Lyapunov optimization

> Info13- eTime: Energy-Efficient Transmission between Cloud and Mobile Devices

> tpds14-On Arbitrating the Power-Performance Tradeoff in SaaS Clouds

> Vnf scaling and placement

> tpds19-Dynamic Network Function Instance Scaling Based on Traffic Forecasting and VNF Placement in Operator Data Centers

> 设计一种流量预测的方法

> 针对实际的运营网络中两种SFC模型实现VNF实例缩扩

> JSS19-ElasticSFC: Auto-scaling techniques for elastic service function chaining in network functions virtualization-based clouds

> 构建弹性服务链的扩展体系结构架构


### 问题建模及公式表达

> 见PPT

### 设计与分析

> 见PPT



### 张凯：图神经网络GNN简洁

### 图神经网络介绍

> 图神经网络的概念

> GNN的目标是以图结构数据和节点特征作为输入，以学习到节点（或图）的embedding，用于分类任务（节点分类或者图分类）

> 基于邻域聚合的GNN可以拆分为以下三个模块：

> 1.Aggregate：聚合一阶邻域特征。

> 2.Combine：将邻居聚合的特征与当前节点特征合并，以更新当前节点特征。

> 3. Readout（可选）：如果是对graph分类，需要将graph中所有节点特征转变成graph特征。

### GNN的三个模块

> Aggregate：每次迭代的时候，从底层节点开始向上传输自己的节点信息和链接信息给父节点

> Combine：父节点根据自身节点和子节点的传输信息进行转换，直至所有节点都combine过一次后结束

> Readout：在图中表现为将所有节点信息summary成为DAG信息，表示此次Job的状态；将所有的DAG进行global summary 表示全局的Job需求状态


### GCN的内核操作

> GCN和Pooling的作用：

> 将图粗粒度划分成多个子图

> 这样就可以将一张图转换成为更高形式的表达

> 后接MLP全连接神经网类似于CNN的非线性转换

> 最后使用Softmax进行概率输出，进行分类


### GCN在网络中的应用

> Learning Scheduling Algorithms for Data Processing Clusters -- Sigcomm 2019

> 使用GCN捕捉data processing cluster(hive, spark...)中的Job DAG图

> GNN作用: 将Env返回的Job DAG转化成为向量形式，并输出给Policy Network

> 结合上述内容，使用GCN和Policy Network对不同任务进行目标输出（论文中是job的调度优先度和并行度限额）

