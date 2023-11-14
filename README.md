# Storage-Pool
存算分离架构下存储资源池化

## 存储资源池化

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/16aa6e7c-667d-4c8c-a29c-484ad1da5d78)

随着RDMA、CXL、NVMe SSD等新型硬件技术的发展，需构建新型存算分离架构，以确保云和互联网存储域服务能够兼顾资源利用率、可靠性等诉求。

相较传统架构，新型架构的区别在于两点：

- 第一，彻底的存算解耦，组建为彼此独立的硬件资源池；
- 第二，细粒度的处理分工，使数据处理等CPU不擅长的任务被专用加速器替代，以实现能效比最优的组合。

## 新型存算分离架构的特征

新型架构具备以下特征：

**Diskless服务器**:新型存算分离架构将服务器本地盘拉远构成Diskless服务器和远端存储池，还通过远程内存池扩展本地内存，实现了真正意义的存算解耦，极大提升存储资源利用率，减少了数据迁移。

**多样化的网络协议**:计算和存储间的网络协议从当前的IP或光纤通道协议扩展到CXL+NoF+IP协议组合。CXL使得网络时延降低到亚微秒级，实现内存型介质池化；NoF加速SSD池化。这几类协议组合构建的高通量网络，满足了多种池化接入诉求。

**专用的数据处理器**:数据存储等不由通用处理器负责，卸载到专用数据处理器。此外，如纠删码等特定的数据操作可由专用硬件加速器进行进一步加速。

**极高存力密度的存储系统**:分离式存储系统是新型架构的重要组件，它作为持久化数据的底座，在存储介质的集约化管理基础上，结合芯片、介质的深度协同设计，整合当前系统、盘两级的空间管理，通过大比例纠删码算法减少冗余资源开销比例。此外，还可通过基于芯片加速的场景化数据缩减技术提供更多的数据可用空间。


## 极简分层的新型存算分离架构

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/f81b1703-3d35-4590-ace1-683c01c3f69b)

新型存算分离架构中，存储型模组主要以EBOF、EBOM等新型盘框形态存在，EC/压缩等传统存储能力下沉到新型盘框中，构成“盘即存储”的大盘技术，对外通过NoF等高速共享网络提供块、文件等标准服务。

从内部架构来看，其介质层既可由标准硬盘组成，也可由晶圆工艺整合的颗粒大板组成，盘框融合以实现极致成本。在这之上，存储模组需构建池化子系统，基于RAID、EC等可靠冗余技术实现本地介质的池化，结合重删压缩等技术进一步提升可得容量。为了支撑新型架构的高通量数据调度，需要提供更加高效的数据吞吐能力，通常基于硬件直通等技术构建快速数据访问路径。和传统阵列相比，避免了用户数据和控制数据（元数据等）的低效交织，减少传统存储阵列的复杂特性处理（复制等特性），缩短IO处理路径，最终实现高吞吐、低时延的极致性能体验。

存储模组作为一种存力集约化、紧凑化、极致化的新型存储形态，加速服务器Diskless，有效支撑了传统数据中心架构朝极简分层的新型存算分离架构演进。

**高通量数据总线**：过去10年，万兆IP网络促使HDD池化，基于IP网络发展了支持块、文件等共享的访问协议。当前，面向热数据处理，NVMe/RoCE促使SSD池化；并且，NVMe协议快速发展并开始收编烟囱式协议。下一步，面向极热数据处理，内存型网络（如CXL）将促使内存资源池化。
![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/94a41878-056c-438d-90aa-72c3112aacd0)

## 新型存算分离架构催生的关键技术
**网存协同**：智能网卡和DPU是服务器的数据出入口，充分利用智能网卡和DPU的硬件卸载NoF、压缩等加速能力，协同好主机和DPU间的任务调度，降低主机数据处理开销，可提升IO效率；可编程交换机是服务器、存储之间的数据交换中枢，它们在系统中占据特殊的位置。结合其可编程能力和交换机的中心化和高性能的优势，可以实现高效的数据协同处理。

**盘存协同**：通过介质和控制芯片深度协同可获得端到端最佳TCO和效率。以冗余设计为例，新型存储型模组直接集成介质颗粒，仅在框这一级构建一层大比例EC的池化空间，辅助专有芯片卸载加速，最终简化了原有的盘内、框内等多层冗余设计，有效改善资源利用率。

最后，新型存储模组基于专有芯片除了提供传统IO接口外，还有旁路接口加速，使元数据绕开厚重的IO栈，以远程内存访问方式提升并行访问能力。

## SkyShare 系统的存储资源池设计

存储池是多个存储介质模块的管理容器，为使用者提供大容量、高数据传输性能的存储系统，也称为虚拟存储池。存储资源池支持多种类型存储资源共存的集成方式，文件的位置选择采用定义好的存储规则． 存储资源池的逻辑架构设计如图 2 所示。管理员用户能对存储资源池进行有效管理，如接入新的被 SkyShare 支持的任意种类的存储资源; 移除不需要的存储资源; 数据的备份与迁移; 存储资源池可用信息的监测; 存储资源历史性能分析; 存储资源的分配; 存储规则的定义等。

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/f365ac58-5a6e-440d-8ed2-b5d37d27f456)

**存储管理**

(1) 存储资源的接入 最简单的实现方法是使用配置文件，管理员通过在配置文件中提供相应存储资源的连接参数，将存储资源接入到存储资源池中． 可以更进一步提供可视化编辑界面给管理员使用，以增加用户体验，而且能防止管理员错误地更改配置文件．
(2) 存储资源的移除 操作方式同存储资源接入，只删除配置文件相应的 storage 节点．
(3) 存储数据管理 提供数据的删除、迁移等操作．
(4) 存储资源的分配按部门分配存储资源，或按用途( 备份、共享) 分配存储资源等．
(5) 存储规则的定义暂不支持存储规则定义，采用以文件为单位的随机存储规则

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/2dc9deea-9c7a-4558-b337-b07cffdaad0a)

该图展示了软件存储交换机的整体架构，其灵感来源于当今的 SAN 交换机。它包括每个固态盘管道，用于协调网卡端口和 NVMe 固态盘之间的 IO 流。每个管道配备三个主要组件：

(1) 入口处的 IO 调度器，它提供按租户的优先级队列，并使用规范化 IO 单元（称为虚拟插槽）以赤字循环方式 (DRR) 执行 IO。它暴露了一个公平队列抽象；
(2)出口处基于延迟的拥塞控制机制，该机制测量存储带宽可用性，并在运行时使用 IO 完成时间监控 SSD 负载状态。此外，它还采用了速率步调引擎，以缓解提交过程中的拥堵和 IO 突发性；
(3)写入成本估算器，根据延迟动态校准固态硬盘写入成本，并将此信息提供给其他系统组件。它实现了一个近似性能模型，可适应工作负载和固态硬盘条件。

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/6a8a6904-79ff-4b7c-9188-5885835ceaf8)

最近，数据中心出现了智能网卡，它不仅可以加速数据包处理和虚拟交换功能，还可以卸载一般的分布式工作负载。通常情况下，SmartNIC 由通用计算基板（如 ARM 或 FPGA）、特定领域加速器阵列（如加密引擎、可重配置匹配行动表）、板载内存和用于数据包转向的流量管理器组成。大多数智能网卡都是小巧的 PCIe 设备，可增加现有数据中心基础设施的成本，并显示出廉价扩展仓库计算能力的潜力。
      
硬件供应商将 SmartNIC 与 NVMe 驱动器结合起来，作为分解存储，取代了传统的基于服务器的解决方案，以提高成本效益。
    
上图展示了 Broadcom Stingray 解决方案。它包含一个 PS1100R SmartNIC、一个 PCIe 载板、几个 NVMe SSD 和一个独立电源。载板可容纳 SmartNIC 和 NVMe 驱动器以及连接各组件的板载 PCIe 交换机。SmartNIC 拥有 8 × 3.0GHz ARM A72 CPU、8GB DDR4-2400 DRAM（以及 16MB 缓存）、FlexSPARX 加速引擎、100Gb NetXtreme 以太网网卡和 PCIe Gen3 根复杂控制器。PCIe 交换机提供 ×16 PCIe 3.0 通道（理论峰值带宽为 15.75GB/s），可支持 2 × 8 或 4 × 4 PCIe 分叉。带有四个三星 DCT983 960GB 固态硬盘的 Stingra 分解存储盒的上市价格为 3228.0 美元，散装价格可能会低得多，因此比基于 Xeon 且具有类似 IO 配置的存储盒便宜得多。

## **NV-BSP**

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/f7dc7a0a-6d3e-444b-aabe-50afaecca350)

支持HPC和大数据处理的分域共享并发存储架构如图１所示，从数据访问特性上看，主要包含基于 NVMeoF的虚拟本地存储层、基于NVMeoF存储池的子域共享加速层、大容量全局共享存储层3部分。设计多态存储服务结点，软件上实现高并发NVM存储池NV-BSP(Burst Storage Pool)、Burst I/O缓冲加速功能和并行存储功能，可根据系统规模或者应用的I/O特性在多态存储服务结点上进行动态配置，实现可适配多种应用模式、混合存储资源的柔性配置，以及数据的动态部署。

存储服务结点实现NV-BSP存储池，同时连接高速互连网和后端存储局域网，通过后端存储局域网挂接大容量存储池。本文采用层次式融合存储管理技术维护所有NV-BSP存储池的存储空间，采用一种基于非确定DHT(Distributed Hush Table)映射规则的数据布局与组织方法，实现了高效的 数 据布局和I/O调 度，提供弹性可扩展的数据和元数据访问能力。采用多模式的数据技术，对2个层次之间的数据存储及一致性进行有效管理，构造高效、可靠、统一的融合虚拟存储空间，有效提高 E级系统复杂应用条件下整体I/O性能。

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/c41a6b0f-9bcb-45dd-bd25-e2dfd6e6090e)

BSP存储池硬件逻辑结构如上图所示，主要由RDMA网络接口部件(RNIC)、存储缓存Buffer、NVMeoF硬件加速引擎、CPU、内存、PCIe网络和NVMe SSD构成。

这种存储互连融合架构支持多个 HOST（I/O访问发起者）和多个NV-BSP存储池的 高 效互连互通，在保证低延迟、高带宽等性能指标的前提下，为 存 储 系 统 提 供 良 好的扩展性。

ION(I/O Node)服务结点和计算结点均可作为Host，通过网络接口部 件RNIC发送NVMe访问请求，互连网络基于访问目的地址路由到相应NV-BSP的RNIC上，NVMeoF引擎解析接收到的访问请求，并转换成NVMe设备访问命令，NVMe设备基于交换网络获取访问命令，完成后续的数据 访问操作，最后由RNIC将完成应答返回给Host。

![image](https://github.com/lus-oa/Storage-Pool/assets/122666739/cfcb0683-128b-4f64-8cdf-3982ad8dfdd6)

存储池管理软件总体架构如上图所示，主要包括I/O处理模块和存储资源管理模块两大部分，实现对NVMe SSD设备的一体化存储管理和监控。

I/O处理模块包括数据读写请求处理、控制命令处理、数据重建处理、错误处理等功能，存储资源管理模块主要包括存储池管理、资源分配、数据保护策略、虚拟盘管理等功能。

在提出的基于数据保护域的虚拟化存储池资源管理架构中，每个SSD被 切分为多个称为Chunk的物理块，所有的SSD物理资源被池化成一个Storage Pool，池化的物理资源通过Allocator分配给Container存储容器。每个Container由多个Chunk构成，是一个基本的数据保护单元，可按需动态配置不同的数据保护级别。多个Container组成虚拟盘VDisk(Virtual Disk)。

这种虚拟化存储池架构将物理资源与数据保护进行了彻底的分离，数据保护不再依赖于物理盘，实现了高效灵活的资源管理和快速数据重构。通过带权重的分配算法、基于图的读写处理模型、无锁I/O并发处理等关键技术，支持存储数据根据NVM存储介质寿命、应用I/O模式、数据可靠性合理分布，达到I/O负载均衡、性能高可扩展的目的。








