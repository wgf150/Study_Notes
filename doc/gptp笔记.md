


### epoll

https://www.cnblogs.com/imreW/p/17234004.html

### BPF
https://blog.csdn.net/chanimei_11/article/details/124357147
##### 概念：
 Berkeley Packet Filter（BPF）
 Packet Filter 代理包过滤器，UNIX系统上用于将数据包从kernel拷贝到user空间，同时过滤掉无用数据，达到拷贝数据最小化。
 过去，NIT（Network Interface Tap）是RISC CPU上**基于内存栈**的次最优过滤器
 BPF则是**基于寄存器**的过滤器，并非采用直接buffer策略，效率高了很多

![[bpf_overview.png]]

系统结构如上，当数据包到达网口时，链路层驱动会把数据包传送给上层网络协议栈，但如果有bpf监听这个网口，链路层驱动会优先调用BPF，BPF再将数据包填喂给每个参与的包过滤器（filter），用户定义的 包过滤器 决定了包是否接受，以及保存大小；之后链路层驱动再将消息发给网络协议栈。
BPF会对多个数据包进行封装（用带有时间戳、长度和偏移量的头），并合并成组，在应用请求时返回给应用。（防止应用频繁读取每一个数据包）

论文中的测试数据（统一系统下测试NIT、BPF）
![[BPF_CP_NIT_1.png]]
全部接受情况
* BPF内存拷贝更快（216ns/byte），这是由于NIT并未注重对齐，以太网报头14B，因此数据包原始为短字节对齐，而网络驱动希望报头在长字节上对齐，NIT将数据包复制到长字节对齐的buffer中，故更为低效
* BPF每包处理耗时更短，且差异巨大，这是由于NIT对于每个数据包，都需要分配和申请缓冲区（文中叫STREAM BUFF），用于传递给上层过滤器，分配和初始化缓冲区带来了较大的开销；而BPF会在收到数据包时就地过滤，并只将需求数量的数据拷贝至过滤器关联的缓冲区，过滤在中断上下文中完成
![[NIT_CP_BPF_2.png]]
全拒绝情况：
* BPF几乎为常量级，因为数据包未参与拷贝，在过滤器中直接被丢弃


注：BPF会在现场过滤（如在网口DMA引擎数据获取处直接过滤），而不是拷贝到kernel再过滤，从而极大减少了内存开销

##### 过滤原理
上述缓冲模型（buffer model）的设计，决定了接收数据包时的成本，而拒绝数据包的成本，主要由过滤模型决定
* CSPF模型（一种Boolean表达树）
* CFG模型，直接非循环控制流图（directed acyclic control flow graph），BPF使用的方法

相较CSPF缺点：
* CSPF基于操作数堆栈实现，会带来访存开销；BPF基于寄存器实现可以优化这部分开销
* 树模型会遍历不必要的节点，带来多余开销

CFG过滤复杂度上，也有着较大优势，对比如下：

![[CFGb.png]]
![[CSPF.png]]
同样的场景，CFG平均遍历长度为3，而CSPF需要遍历7个波尔节点

##### 虚拟机
BPF参考CSPF，设计了一套内核运行的虚拟机，来实现数据流图的过滤
优点抽象，没看了


### BPF IOCTL
参考
https://www.ibm.com/docs/sr/aix/7.2?topic=capture-ioctl-bpf-control-operations
https://man.netbsd.org/NetBSD-7.0.1/bpf.4


qnx下，如果想绕过TCP/IP协议发送以太网裸数据（类似raw socket），则需要使用BPF功能
gptp中使用的cmd：
* **BIOCSETIF** 关联设备与硬件接口，需要在读取包之前执行
* **BIOCIMMEDIATE** arg为1时，设置读取在数据包接收时立即返回，否则直到缓冲区写满或超时时返回
* **BIOCSHDRCMPLT** arg为1时，禁用自动添加以太网包头（src mac、dst mac、eth type）



### libpcap
```C
char errbuf[PCAP_ERRBUF_SIZE];
pcap_t *pcap_create(const char *source, char *errbuf);
```


