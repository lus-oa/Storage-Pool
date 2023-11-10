# Storage-Pool
存算分离架构下存储资源池化

## 存储资源池化

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/16aa6e7c-667d-4c8c-a29c-484ad1da5d78)

随着RDMA、CXL、NVMe SSD等新型硬件技术的发展，需构建新型存算分离架构，以确保云和互联网存储域服务能够兼顾资源利用率、可靠性等诉求。

相较传统架构，新型架构的区别在于两点：

- 第一，彻底的存算解耦，组建为彼此独立的硬件资源池；
- 第二，细粒度的处理分工，使数据处理等CPU不擅长的任务被专用加速器替代，以实现能效比最优的组合。



## 极简分层的新型存算分离架构

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/f81b1703-3d35-4590-ace1-683c01c3f69b)

新型存算分离架构中，存储型模组主要以EBOF、EBOM等新型盘框形态存在，EC/压缩等传统存储能力下沉到新型盘框中，构成“盘即存储”的大盘技术，对外通过NoF等高速共享网络提供块、文件等标准服务。

从内部架构来看，其介质层既可由标准硬盘组成，也可由晶圆工艺整合的颗粒大板组成，盘框融合以实现极致成本。在这之上，存储模组需构建池化子系统，基于RAID、EC等可靠冗余技术实现本地介质的池化，结合重删压缩等技术进一步提升可得容量。为了支撑新型架构的高通量数据调度，需要提供更加高效的数据吞吐能力，通常基于硬件直通等技术构建快速数据访问路径。和传统阵列相比，避免了用户数据和控制数据（元数据等）的低效交织，减少传统存储阵列的复杂特性处理（复制等特性），缩短IO处理路径，最终实现高吞吐、低时延的极致性能体验。

存储模组作为一种存力集约化、紧凑化、极致化的新型存储形态，加速服务器Diskless，有效支撑了传统数据中心架构朝极简分层的新型存算分离架构演进。


