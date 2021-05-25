# SDN 复习

## SDN 概述和基本原理

### SDN 的产生与发展

#### **什么是 SDN？**

SDN（Software Defined Network）即软件定义网络，是一种网络设计理念，通过将网络设备**控制平面**与**数据平面**分离开来，从而实现了网络流量的灵活控制，让网络成为一 种可灵活调配的资源。





#### **SDN 的特点（数控分离）；**

控制平面与数据平面分离

集中式控制模型

可编程（开放的API）

#### **SDN 与传统网络的对比？**

![image-20210520104232112](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520104232112.png)

### SDN 标准化

#### **标准化组织：ONF、ITEF；**

**Open Networking Foundation（ONF）**

最为活跃、最具影响力的SDN标准组织，SDN标准化和产业化最主要的推动力量.

**因特网工程任务组（IETF）**

互联网领域最具权威的技术标准化组织.

由网络设备厂商主导，更关注 SDN相关功能和技术如何在网络中实现的细节上.

### SDN 的基本架构

#### **SDN 主流架构；**

#### **ONF 定义的 SDN 基本架构（分层，架构图、内容）；**

转发与控制分离；

标准化转发面。



![image-20210520105921332](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520105921332.png)

### SDN 的核心思想

#### **解耦、抽象和可编程及其涵义；**

**解耦（Decoupling ）**: 数据平面与控制平面的解耦

- 通过解耦合，控制平面负责上层的控制决策，数据平面负责数据的交换转发，双方遵循一定的开放接口进行通信；
- 实现网络逻辑集中控制的前提；
- 两个平面独立完成体系结构和技术的发展演进，有利于网络的技术创新与发展。



**抽象（Abstraction）**: 网络功能的抽象

- 转发抽象（forwarding abstration）：隐藏了底层的硬件实现 ，转发行为与硬件无关；
- 分布状态抽象（distribution abstration）：屏蔽分布式控制的实现细节，为上层应用提供全局网络视图；
- 配置抽象（specification abstration）：网络行为的表达通 络编程语言实现，将抽象配置映射为物理配置。



**可编程（Programable）**: 可编程网络

可编程网络（Programmable Network）主要利用在网络节点中提供标准的网络应用编程接口向用户和网络业务供应者提供一个“开放”的网络控制机制，与传统Internet的区别就在于Internet是无状态的，而<u>可编程网络是有状态的并且可以由用户控制和改变的</u>。



网络可编程相关研究：主动网络（Active Networking）、4D架构



**SDN网络可编程接口：**

![image-20210520111518948](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520111518948.png)

北向接口：REST（Representational State Transfer） API，RESTCONF协议

南向接口：OpenFlow、OF-Config、 NETCONF、OVSDB、XMPP、PCEP、 I2RS、OPFlex等协议；



## SDN 数据平面

**构成SDN的3层是什么？**

1）应用层，2）控制层，3）物理层

**最能描述软件定义网络（SDN）的短语**

SDN是一种分离网络控制和转发功能的体系结构。

**最能描述OpenFlow的短语。**

OpenFlow是用于SDN控制器和物理层中的网络设备之间的通信的协议。

**SDN构架中的核心组件是**

控制器

**从SDN的应用领域角度来看，（）是SDN第一阶段商用的重点。**

数据中心



**软件定义网络的核心思想？**

(1) 抽象（网络虚拟化）：网络设备抽象为通用的转发模型。

(2) 解耦（集中控制）：数据平面和控制平面分离。

(3) 可编程：提供开放的可编程接口（北向接口、南向接口）。



### SDN 数据平面

#### **SDN 数据平面的定义和特点；**

**定义**





**特点**

处理流程中的：解析（Parser）、转发（Forwarding）和调度 （Scheduling）都是可编程、协议无关的；

传统网络设备的二层或三层转发表抽象成流表；

![image-20210520112644092](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520112644092.png)







各种查找表抽象成一种通用的流表结构

数据转发处理抽象成通用的匹配（Match）—动作（ Action ）过程

![image-20210520112733978](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520112733978.png)





### OpenFlow

#### **OpenFlow 架构；** 

![image-20210520194653834](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520194653834.png)

流表（Flow Table），每个动作（Action）关联一个流表项（Flow Entry），指示交 换机如何进行流（Flow）的处理；

安全通道（Sccure Channel），OpenFlow交换机通过安全通道与远 端控制器连接 ，负责控制器与交换机之间的交互；

OpenFlow协议（Protocol），定义了一种南向接口标准，为控制器与交换机之间 的通信提供了一种开放标准的方式。



#### **OpenFlow 主要版本和特点（重点是 OpenFlow v1.0）；**

![image-20210520195606127](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520195606127.png)

####  **OpenFlow v1.0 的组成结构；**





### OpenFlow 流表

#### **流表的定义；** 

针对特定流的策略表项的集合，负责数据包的查找与转发。 一张流表包含了一系列的流表项(flow entries)。

#### **V1.0 的流表结构及其内容（包头域、计数器和动作）；**

![image-20210520195848812](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520195848812.png)

**包头域：**用于对交换机接收到的数据包的包头内容进行匹配

![image-20210520200504543](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520200504543.png)

第二层：

- 入端口（Input Port） ，即消息在哪个端口进入交换机
- 源MAC地址（Ether source）
- 目的MAC地址（Ether dst）
- 以太网类型（Ether Type）
- VLAN标签（VLAN id）
- VLAN优先级 （VLAN priority）

第三层：

- 源IP（IP src）
- 目的IP（IP dst）
- IP协议字段（IP Proto）
- IP服务类型（IP ToSbits）

第四层：

- TCP/UDP源端口号（TCP/UCP src port）
- TCP/UDP目的端口号（TCP/UDP dst port）



**计数器：**用于统计数据流量相关信息，可以针对交换机中的每张流表、每个数据流 、每个设备端口、每个转发队列进行维护



针对每张流表， 统计当前活动的表项数、数据包查询次数 、数据包匹配次数等

针对每个数据流，统计接收到的数据包数、字节数、数据流持续时间等

针对每个设备端口 ，除统计接收到的数据包数、发送数据包数 、接收字节数、发送字节数等指标之外，还可以对各种错误发生的次数进行统计

针对每个队列 ， 统计发送的数据包数和字节数， 还有发送时的溢出 （Overrun）错误次数等





**动作（action）：**用于指示交换机在收到匹配数据包后如何对其进行处理。决定了OpenFlow对转发面行为的抽象能力

- 每个流表项可以对应有零至多个动作， 如果没有定义转发动作， 那么与流表项包头域匹配的数据包将被默认**丢弃**
- 同一流表项中的多个动作的执行可以具有优先级，但是在数据包的发送上不保证其顺序
- 如果流表项中出现有OpenFlow交换机不支持的参数值，交换机将向控制器返回相应的出错信息

#### **流表动作列表（常用必备动作、可选动作）；** 

必备动作(Required Actions) vs. 可选动作(Optional Actions)

- 必备动作：需要所有OpenFlow交换机默认支持
- 可选动作：需要交换机告知控制器它所能支持的动作种类

![image-20210520211122422](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210520211122422.png)

#### **OpenFlow 数据包处理流程；** 

每个包按照优先级依次去匹配流表中表项，匹配包的优先级最高的表项即为匹配结果。匹配成功，对应的计数器将更新，同时实施转发动作 ；如果没能找到匹配的表项，则转发给控制器。

![image-20210521101549180](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521101549180.png)

#### **OpenFlow 的保留端口及内容；**

保留端口：OpenFlow保留端口用于特定的转发动作，如发送到控制器、洪泛， 或使用非OpenFlow的方法转发，如使用传统交换机的处理过程

![image-20210521101725549](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521101725549.png)

### SDN 交换机

#### **交换机类型；**

**OpenFlow专用交换机（OpenFlow-only）**

只支持OpenFlow协议





**OpenFlow使能交换机（OpenFlow-enabled）**

- 能同时运行OpenFlow协议和传统的 二层/三层协议栈
- 支持OpenFlow可选转发动作中的NORMAL动作。





#### **交换芯片类型；**

![image-20210521102200980](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521102200980.png)

####  **SDN 交换机；** 

**SDN物理（硬件）交换机**

多数为支持OpenFlow的混合模式交换机，支持传统二、三层处理模式 和SDN处理模式；



**SDN虚拟（软件）交换机：**

成本低、配置灵活，性能满足中小规模网络要求







#### **SDN 交换机选型的参数考虑(例如背板带宽、频率等)；**

**背板带宽**

交换机接口处理器和数据总线间的最大吞吐数据量，背板带宽越高， 所能处理数据的能力就越强。

对于线速转发的交换机，背板的带宽计算方法：
$$
端口数*相应端口*2( 全双工模式)
$$
**端口密度**

代表交换机转发能力。密度越大，设备的<u>转发能力</u>越强；

**端口速率**

速率越高，设备的<u>处理性能</u>越强；

**支持模块类型**

LAN接口、WAN接口、ATM接口。

**端口带宽类型**

40G、100G高速端口

百兆、千兆低速端口

XFP

SFP、SFP+、CFP光接口类型



## SDN 南向接口协议

### 南向接口协议概述

#### **南向接口的定义（SDN 架构中的位置）；** 

![image-20210521104024629](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521104024629.png)

#### **什么是南向接口协议？**

为控制平面的控制器和数据平面的交换机之间的信息交互而设计的协议。

####  **常见南向接口协议及其设计目标（OpenFlow、OF-config、NETCONF、OVSDB）；**

![image-20210521104046080](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521104046080.png)



### OpenFlow 协议

#### **OpenFlow 协议的消息类型；** 

OpenFlow协议是用来描述控制器和OpenFlow交换机之间数据交互所用信息的接口标准，其核心是 OpenFlow协议信息的集合。



**controller-to-switch**：由<u>控制器</u>发起，用来管理或获取OpenFlow交换机的状态。

![image-20210521105820329](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521105820329.png)

**asynchronous（异步）**：由OpenFlow<u>交换机</u>发起，用来将网络事件或交换机状态变化更新到控制器。

![image-20210521105832672](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521105832672.png)

**symmetric （对称）**：由<u>交换机或控制器</u>发起。

![image-20210521105843000](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521105843000.png)



#### **消息格式；** 

Openflow协议数据包：Openflow Header和Openflow Message



**Openflow Header格式：**

![image-20210521110246950](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521110246950.png)

```c
/* Header on all OpenFlow packets. */ 
struct ofp_header { 
  uint8_t version; /* OpenFlow的协议版本号*/ 
  uint8_t type; /*消息类型，是个常数*/ 
  uint16_t length; /*数据包字节数*/ 
  uint32_t xid; /*数据包的标识id*/
};
```

#### **OpenFlow 的 SDN 通信流程；**

![image-20210521110505419](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521110505419.png)



①主机向网络发送数据包

②OF交换机流表无匹配项，通过 Packetln事件将数据包上报给控制器

③控制器下发流表（或 PacketOut）

④数据包转发

⑤同②

⑥同③

⑦同4

补充：

step1:建立连接(Switch registration)

获取交换机特性（Features）信息

step2：配置交换机过程

step3:网络topo检测

step4: Packet‐in事件（交换机接收）

step5: 控制器配置流表（Flow‐Mod）

step6: 交换机转发（Packet‐Out）



### OF-Config

#### **协议概述，设计目标和设计思想；**

**协议概述**

OpenFlow管理与配置协议（OpenFlow Management and Configuration Protocol，OF-Config）：一种OpenFlow交换机配置协议。



**设计目标&设计思想**

配置需求（Specification Requirements）

- 控制器连接设置
- 多控制器（Multiple Controllers）
- OpenFlow逻辑交换机
- 连接中断（Connection Interruption）
- 加密（Encryption）
- 队列（Queues）
- 端口（Ports）
- 能力发现（Capability Discovery）
- Datapath ID



操作运维需求（ Operational Requirements）

- 支持OF交换机被多个OpenFlow配置点配置；
- 支持一个OpenFlow配置点管理多个OF交换机；
- 支持一个OpenFlow逻辑交换机被多个控制器控制；
- 支持配置OpenFlow交换机的端口和队列
- 支持OpenFlow逻辑交换机的能力发现；
- 支持配置隧道，如IPinGRE、VXLAN及NVGRE



管理协议需求（ Management Protocol Requirements ）

- 保障安全性，支持对交换机和配置点双向认证； 
- 支持配置请求和应答的可靠传输； 
- 支持由配置点或交换机进行连接设置； 
- 能够承载局部交换机配置以及大范围交换机配置； 
- 支持配置点在交换机配置参数及接收来自交换机的配置参数； 
- 支持在交换机创建、更改及删除配置信息，并支持报告配置结果； 
- 支持独立发送配置请求，并支持交换机到配置点的异步通知； 
- 支持记忆能力、可伸展性以及报告自身属性和能力。





#### **OF-Config 数据模型（XML）和传输协议（NETCONF）**

**数据模型（XML）**

OF-CONFIG采用XML来描述其数据结构；

数据模型由类和类的属性构成



OpenFlow 交换机的数据模型

```xml
<capable-switch> 
  <id>CapableSwitch0</id> 
  <configuration-points>
...
	</configuration-points> 
  <resources>
...
	</resources> 
  <logical-switches>
...
	</logical-switches> 
</capable-switch>
```

OpenFlow 控制器的数据模型

```xml
<controller> 
  <id>Controller3</id> 
  <role>master</role> 
  <ip-address>192.168.2.1/26</ip-address> 
  <port>6633</port> 
  <local-ip-address>192.168.2.129</local-ip-address> 
  <local-port>32768</local-port> 
  <protocol>tcp</protocol> 
  <state>
    <connection-state>up</connection-state>
    <current-version>1.2</current-version>
    <supported-versions>
    <version>1.3</version>
    <version>1.0</version>
    </supported-versions> 
  </state> 
</controller>
```

**传输协议（NETCONF）**

NETCONF协议采用Client/Server结构。通过NETCONF，SDN控制器对网络设备进行配置管理。

![image-20210521104856673](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521104856673.png)

### NETCONF

#### **什么是 NETCONF 协议？设计目标；**

新一代网管协议；

网络配置协议NETCONF（Network Configuration Protocol）提供一套管理网络设备的机制；

####  **NETCONF 协议框架（操作层和内容层的内容）**

![image-20210521114201819](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521114201819.png)

**NETCONF操作层**
对数据库信息的获取、配置、复制和删除等功能
![image-20210521114333898](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521114333898.png)





**NETCONF内容层**

描述了网络管理所涉及的配置数据：

```xml
<running/> 
<candidate/> 
<startup/>
```


使用YANG语言进行建模
YANG具有以下特点：

层级树形结构

可以直接映射到XML

可读性好，易学习

可复用、可扩展




## SDN 控制平面

### SDN 控制平面概述

#### **什么是 SDN 控制平面（SDN 架构中的位置）？**

SDN控制平面由一个或多个SDN控制器组成，是网络的大脑。

- 对底层网络交换设备进行集集中管理，状态监测、转发决策以及处理和调度数据平面的流量；

- 向上层应用开放多个层次的可编程能力。


SDN控制器体系架构
![image-20210521143655528](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521143655528.png)

#### **南向控制协议的任务和功能（链路发现、拓扑管理、策略制定、表项下发及其涵义）；** 

**链路发现**

获得SDN全网信息，实现网络地址学习、VLAN、路由转发

OF交换机直连链路的发现: LLDP协议

OF交换机非直连链路的发现: 广播

![image-20210521144010248](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521144010248.png)

**拓扑管理**

监控和采 集SDN交换机的信 息 ，反馈工作状 态 和链路连 接状态：

定时发送带LLDP 数据包的Packet_out消息，根据Packet_in消息获知交换机信息，监测交换机工作状态，完成网络拓扑视图更新；

导致较慢的收敛过程，影响实时反馈；

**策略制定**

流表生成算法是影响控制器智能化水平的关键因素

针对不同层次的传输需求，制定相应的转发策略并生成对应的流表项

**表项下发及其涵义**

通过流表下发机制控制交换机的数据包转发

主动（proactive）下发：数据包到达交换机之前进行流表设置。



被动（reactive）下发：交换机接收到一个数据包并且没有发现 匹配的流表项，将其送给控制器处理。



#### **北向业务支撑方式；**

通过北向接口为上层业务应用及资源管理系统提供灵活的网络资源抽象；

北向接口定义是SDN领域关注和争论的焦点；

REST API是用户比较容易接受的方式；

### 主流开源控制器

#### **NOX（C++）、POX（Python）、Ryu（Python）、Floodlight（java）、OpenDayLight（Java）**

![image-20210521145004509](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521145004509.png)

**POX**

https://github.com/noxrepo/pox

**ryu**

https://github.com/faucetsdn/ryu

https://ryu-sdn.org/

https://ryu.readthedocs.io/en/latest/



**Floodlight**

https://github.com/floodlight/floodlight

**OpenDayLight**



## SDN 北向接口协议

### SDN 北向接口概述

#### **什么是北向接口？** 

应用平面与控制平面之间的接口（NBI），通过控制器向上层业务应用开放的接口，为上层业务应用和资源管理系统提供灵活的网络资源抽象；

![image-20210521154438383](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521154438383.png)

#### **北向接口的设计（功能型、基于意图）与网络模型；** 

**功能型北向接口(Functional NBI)**：

自下而上看网络，重点在网络资源抽象及控制能力的开放，包括Topology、L2VPN、L3VPN、 Tunnel等接口。

**基于意图的北向接口(Intent-based Interface)**：

自上而下看网络，关注应用或者服务的需求，同具体的网络技术无关。

**网络模型**

![image-20210521154754841](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521154754841.png)



#### **北向接口的实现（主流实现——RESTAPI）；**

Rest API:SDN北向接口的主流实现方式。实现Rest API的控制器 有RYU、Floodlight、Opendaylight等

REST （Representational State Transfer，表述化状态转移）

RESTful：满足REST约束条件和原则的设计规范或者架 构风格 ；

REST API：遵循RESTful设计的API。 SDN北向接口的主流实现方式。

### RESTAPI 及其设计

#### **RESTAPI 的设计；** 

REST API的设计: HTTP动词+ URI

HTTP动词描述操作；

URI是标识资源。



#### **HTTP 动词；** 

![image-20210521155412301](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521155412301.png)

#### **URI 规范；** 

资源命名规范：

- 文档(Document)类型的资源用**名词单数**命名
- 集合(Collection)类型的资源用**名词复数**命名
- 仓库(Store)类型的资源用**名词复数**命名
- 控制器(Controller)类型的资源用**动词**命名



URI中有些字段可以是变量，在实际使用中可以按需替换

http://api.soccer.restapi.org/leagues/{leagueId}/teams/ {teamId}/players/{playerId}



URI中分隔符“/”一般用来对资源层级的划分， ”/“不应该出现在URI的末尾；

URI中尽量使用连字符"-"代替下划线"_"的使用

URI中统一使用小写字母

URI中不要包含文件(或脚本)的扩展名

CRUD（增删改查）的操作不要体现在URI中



#### **HTTP 响应状态码；**

2xx：操作成功

3xx：重定向

4xx：客户端错误

5xx：服务器错误



200 (“OK”) ：一般性的成功返回，不可用于请求错误返回； 

201 (“Created”) ：资源被创建； 

202 (“Accepted”) ：Controller控制类资源异步处理的返回，仅表示请求已经收到； 

204 (“No Content”) ：可能会出现在PUT、POST、DELETE的请求中； 

303 (“See Other”) ：返回一个资源地址URI的引用，但不强制要求客户端 获取该地址的状态； 

400 (“Bad Request”) ：客户端一般性错误返回, 其它4xx的错误，也可以 使用400， 具体错误信息可以放在body中； 

401 (“Unauthorized”) ：认证错误； 

404 (“Not Found”) ：找不到URI对应的资源； 

500 Internal Server Error：服务器处理请求时发生了意外； 

503 Service Unavailable：服务器无法处理请求，一般用于网站维护状态。



## 网络虚拟化 &NFV

### 虚拟化技术

#### **什么是虚拟化？** 

虚拟化：资源的统一调度

把所有的硬件设备抽象成 “虚拟机”： 

* 对于提供方来说——便于即时配置、统一调度；
* 对于用户来说——便于自适应扩展、按需付费。

![image-20210521160259130](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521160259130.png)

定义: 虚拟化是资源的逻辑表示，它不受物理限制的约束



“虚拟化”的定义包含了三层含义:

- 虚拟化的对象是各种各样的资源；

- 经过虚拟化后的逻辑资源对用户隐藏了不必要的细节；

- 用户可以在虚拟环境中实现其真实环境的部分或全部功能;



“虚拟化”的两种形式：

![image-20210521160448939](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521160448939.png)







服务器虚拟化：使用虚拟化软件在一台物理服务器上运行多个操作系统；

网络虚拟化：将网络的硬件和软件进行整合，向用户提供虚拟网络连接的技术， VLAN / VPN / VRF / IRF / VDC等，后面将详细讨论；

存储虚拟化：为物理存储设备提供一个抽象逻辑视图，用户通过这个统一的逻 辑接口来访问被整合的存储资源，RAID / NAS / SAN。

软件虚拟化：应用虚拟化、高级语言虚拟化（JAVA/JRE）；

桌面虚拟化：将个人桌面环境保存在远端服务器上，终端与桌面解除耦合；

#### **虚拟化的特点与优点；**

特点：

![image-20210521161504368](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521161504368.png)

虚拟化技术的优势:

①更高的资源利用率——虚拟可支持实现物理资源和资源池的动态共享，提高资源利用率，特别是针对那些平均需求远低于需要为其提供专用资源的不同负载。

②降低管理成本——虚拟可通过以下途径提高工作人员的效率：减少必须进行管理的物理资源的数量;隐藏物理资源的部分复杂性;通过实现自动化、获得更好的信息和实现中央管理来简化公共管理任务;实现负载管理自动化。另外，虚拟还可以支持在多个平台上使用公共的工具。

③提高使用灵活性——通过虚拟可实现动态的资源部署和重配置，满足不断变化的业务需求。
  
④提高安全性——虚拟可实现较简单的共享机制无法实现的隔离和划分，这些特性可实现对数据和服务进行可控和安全的访问。
  
⑤更高的可用性——虚拟可在不影响用户的情况下对物理资源进行删除、升级或改变。
  
⑥更高的可扩展性——根据不同的产品，资源分区和汇聚可支持实现比个体物理资源小得多或大得多的虚拟资源，这意味着您可以在不改变物理资源配置的情况下进行规模调整。
  
⑦互操作性和投资保护——虚拟资源可提供底层物理资源无法提供的与各种接口和协议的兼容性。
  
⑧改进资源供应——与个体物理资源单位相比，虚拟能够以更小的单位进行资源分配。





### 网络虚拟化

#### **网络虚拟化，vNetwork 及其组件；** 

##### **网络虚拟化**

服务器虚拟化对接入网络的挑战

![image-20210521162203989](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521162203989.png)

服务器虚拟化带来网络接入层的变化

![image-20210521162236095](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521162236095.png)





##### **vNetwork 及其组件**

vNetwork的组件主要包括虚拟网络接口卡 vNIC 、vNetwork 标准交换机vSwitch 和 vNetwork 分布式交换机

**虚拟网络接卡vNIC**

每个虚拟机都可以配置一个或者多个虚拟网络接口卡vNIC，vNIC拥有独立的MAC地址以及一 个或多个IP地址，且遵守标准的以太网协议

**vNetwork标准交换机vSwitch**

虚拟交换机用来满足不同的虚拟机和管理界面进行互联。虚拟交换机的工作原理与以太网中的第 2层物理交换机一样

**vNetwork分布式交换机dvSwitch**

dvSwitch将原来分布在一台ESX主机 上的交换机进行集成，成为一个单 一的管理界面，在所有关联主机之 间作为单个虚拟交换机使用。这使 得虚拟机可在跨多个主机进行迁移 时确保其网络配置保持一致



*端口组*

端口组是一种策略设置机制，这些策略用于管理与端口组相连的网络

*VLAN*

VLAN支持将虚拟网络与物理网络VLAN集成

#### **虚拟设备；**

![image-20210521165851039](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521165851039.png)



网络设备1:N虚拟化的传统实现方式： Mutil-VRF、多实例防火墙，一 个操作系统上的多个转发表项；



### 网络功能虚拟化 NFV

#### **NFV 的产生背景；** 

NFV让电信、移动提供商和运营商有能力提供更好的数字业务，加快新服务投放市场的速度，可以借此与大的软件公司竞争。NFV提供一定程度的架构、资本、vendor-sourcing等方面的敏捷性，与传统的基于专用运营商级网络设备的实现方式不同。



传统的通过物理设备提供服务的方式有如下缺点，大大降低了敏捷性：

- 部署周期和成本高，物理设备必须购买且部署这些物理设备需要大量的人力，可能需要数月的时间才能完成部署。
- 高运营成本，物理设备需要特殊地理环境，能源和冷却消费很高。
- 效率低，专用设备必须部署在能够提供最小服务覆盖的地方，不管 实际的使用和需求是多少，这导致了不能充分利用和成本浪费。
- 厂商锁定，单一厂商的电气设备和采购其他产品的负担非常重。



#### **NFV 的定义；** 

NFV(Network Function Virtulization)，即网络功能虚拟化，背景是电信网络, 旨在采用虚拟化的方法，将原本运行在专用中间设备(middlebox)的网络功能（如网关、防火墙） 用软件的方式实现，通过在标准的通用设备（服务器、存储器、交换机）中运行的虚拟网络功能(VNF)得以实现。

多个VNF可通过动态的逻辑链接串联成业务链(Service Function Chain)

#### **NFV 框架：** 

![image-20210521170923419](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521170923419.png)

##### **纵向分层：基础设施层、虚拟网络层、运营支撑层**

**基础设施层**：NFVI是NFV Infrastructure的简称，从云计算的角度看，就是一个资源池。 NFVI映射到物理基础设施就是多个地理上分散的数据中心，通过高速通信网连接起来。 NFVI需要将物理计算/存储/网络资源通过虚拟化转换为虚拟的计算/存储/网络资源池。

**虚拟网络层**：虚拟网络层对应的就是目前各个电信业务网络，每个物理网元映射为一个虚拟网元VNF，VNF所需资源需要分解为虚拟的计算/存储/网络资源，由NFVI来承载，VNF之间的接口依然采用传统网络定义的信令接口（3GPP+ITU-T)，VNF的业务网管依然采用NE-EMS-NMS体制。

**运营支撑层**：运营支撑层就是目前的OSS/BSS系统，需要为虚拟化进行必要的修改和调整。



##### **横向分域：业务网络域，管理编排域（MANO，包括哪些内部实体）**

**业务网络域**：就是目前的各电信业务网络。

**管理编排域**：同传统网络最大区别就是，NFV增加了一个管理编排域，简称MANO ，MANO负责对整个NFVI资源的管理和编排，负责业务网络和NFVI资源的映射和关联，负责OSS业务资源流程的实施等，

MANO内部包括VIM，VNFM和 Orchestrator三个实体，分别完成对NFVI，VNF和NS（Network Service）三个层次的管理。

### NFV 与 SDN

#### **有哪些区别与联系？**

NFV和SDN是高度互补的

这两个主题是互利的，但不是相互依赖的

❖ NFV：网络设备体系结构的再定义

❖ NFV的诞生是为了满足服务提供商（SP）的需求：

通过减少/消除专有硬件降低资本支出

将多种网络功能整合到行业标准平台上


❖ SDN ：网络体系结构的重新定义

❖ SDN来自IT世界

分离数据层和控制层，同时集中控制

提供使用定义良好的接口编程网络行为的能力


## 云计算网络 & Overlay

### 云计算网络

#### **虚拟化对传统数据中心提出的挑战**

传统的三层数据中心架构结构的设计是为了应付服务客户端 -服务器应用程序的纵贯式大流量，同时使网络管理员能够 对流量流进行管理。工程师在这些架构中采用生成树协议 (STP)来优化客户端到服务器的路径和支持连接冗余。

虚拟化从根本上改变了数据中心网络架构的需求。 最重要的 一点就是，虚拟化引入了<u>虚拟机动态迁移技术</u>。从而要求网络支持<u>大范围的二层域</u>。从根本上改变了传统三层网络统治 数据中心网络的局面。

### Overlay 网络

#### **Overlay 技术的由来**

overlay的网络架构是物理网络向云和虚拟化的深度延伸。

随着企业业务的快速扩展需求，云计算可以提供可用的 、便捷的、按需的资源提供，成为当前企业IT建设的常规形态。

在云计算中大量采用和部署的虚拟化几乎成为一个基本技术模式 。 部署虚拟机需要在网络中无限制地迁移到目的物理位置。

虚拟机增长的快速性以及虚拟机迁移成为一个常态性业务，传统的网络已经不能很好满足企业的这种需求。

#### **Overlay 技术的定义及其特征，组成部分**

**定义**

Overlay指一种网络架构上叠加的虚拟化技术模式，对基础网络不进行大规模修改下，以基于IP的网络技术为主承载应用，并与其它网络业务分离。



Overlay主要技术路线颠覆了数据中心网络的建设模式，原有的接入层、汇 聚层、核心层的三层架构逐渐演变为二层汇聚与三层网关的叶脊架构。



**特征**



**组成部分**



#### **什么是 VXLAN？**

Vxlan 是由 Cisco、VMware、Broadcom 等厂家向IETF提出一项云计算环境下，大二层网络解决方案的一项草案，全称 Virtual eXtensible Local Area Network，即虚拟扩展本地网络。


◆  VXLAN报文是在原始二层报文前面再封装一个新的报文，新的报文中和传统
的以太网报文类似，拥有源目mac、源目ip等元组。

◆ 当原始的二层报文来到vtep节点后会被封装上VXLAN包头（在VXLAN网络
中把可以封装和解封装VXLAN报文的设备称为vtep，vtep可以是虚拟
switch也可以是物理switch），打上VXLAN包头的报文到了目标的vtep后会
将VXLAN包头解封装，并获取原始的二层报文。

#### **VXLAN 的报文格式（封装与解封装，UDP header、VXLAN header）；**

![image-20210521185000662](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521185000662.png)


VXLAN报文结构

![image-20210521185131499](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521185131499.png)

## SDN 开源项目

### OVS

#### **OVS 的组成结构**

![image-20210521185610513](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521185610513.png)

ovs-vswitchd： 主要模块，实现switch的daemon，包括一个支持流交换的Linux内 核模块；

ovsdb-server： 轻量级数据库服务器，提供ovs-vswitchd获取配置信息，例如vlan、 port等信息；

ovs-brcompatd： 让ovs-vswitch替换linux bridge，包括获取bridge ioctls的Linux 内核模块；

ovs-dpctl：用来配置switch内核模块；

ovs-vsctl： 查询和更新ovs-vswitchd的配置； 

ovs-appctl： 发送命令消息，运行相关daemon； 

ovs-ofctl： 查询和控制OpenFlow交换机和控制器；

 ovs-openflowd：一个简单的OpenFlow交换机； 

ovs-controller：一个简单的OpenFlow控制器； 

ovs-pki：OpenFlow交换机创建和管理公钥框架； 

ovs-tcpundump：tcpdump的补丁，解析OpenFlow的消息； 

ovs-bugtool：管理openvswitch的bug信息。







### OpenStack

#### **Neutron 概述**

Neutron：软件定义网络框架，用于交付NaaS服务

对网络的虚拟化，它可以解释为一个网络管理服务，为创建和管理虚拟网络公开了一 组可扩 展的 API(通过 创建虚 拟网络为OpenStack Compute节点上的虚拟机 提供网络服务)。

![image-20210521191554136](https://lhc-note.oss-cn-guangzhou.aliyuncs.com/images/image-20210521191554136.png)

neutron-server 接受API请求，然后转发到对应的plugin进行处理

plugin 和 agent 执行实际的网络操作，例如添加端口，创建子网，绑定端口

neutron-server和agent之间，需要通过消息队列来传递各种路由信息

