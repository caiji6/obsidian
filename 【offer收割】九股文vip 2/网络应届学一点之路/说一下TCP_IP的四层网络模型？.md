# 👌说一下TCP/IP的四层网络模型？

TCP/IP 四层网络模型是一个简化的网络协议模型，它由四个层次组成，每个层次都有其特定的功能。

### 1.**网络接口层（Network Interface Layer）**
**功能**：负责在物理网络上发送和接收数据包。包括物理层和数据链路层的功能。处理硬件地址（MAC 地址）和帧格式。

**协议**：

+ 以太网（Ethernet）
+ Wi-Fi（IEEE 802.11）
+ PPP（Point-to-Point Protocol）
+ ARP（Address Resolution Protocol）
+ RARP（Reverse Address Resolution Protocol）

**示例**：

+ 网卡驱动程序
+ 交换机和集线器工作在这一层

### 2.**网络层（Internet Layer）**
**功能**：

+ 负责数据包的路由和转发。
+ 定义了 IP 地址的结构和分配。
+ 处理数据包的分段和重组。
+ 负责网络间的通信。

**协议**：

+ IP（Internet Protocol）：IPv4 和 IPv6
+ ICMP（Internet Control Message Protocol）：用于发送错误消息和操作信息。
+ IGMP（Internet Group Management Protocol）：用于管理多播组成员。

**示例**：

+ 路由器工作在这一层
+ IP 地址分配和管理

### 3.**传输层（Transport Layer）**
**功能**：

+ 提供端到端的通信服务。
+ 负责数据传输的可靠性和完整性。
+ 提供流量控制、错误检测与恢复。

**协议**：

+ TCP（Transmission Control Protocol）：提供可靠的、面向连接的服务。
+ UDP（User Datagram Protocol）：提供不可靠的、无连接的服务。

**示例**：

+ TCP 连接建立和终止（三次握手和四次挥手）
+ UDP 数据报传输

### 4.**应用层（Application Layer）**
**功能**：

+ 提供应用程序之间的通信。
+ 定义了应用程序使用的协议和接口。
+ 处理高层数据格式和表示。

**协议**：

+ HTTP（HyperText Transfer Protocol）：用于网页浏览。
+ HTTPS（HTTP Secure）：HTTP 的安全版本。
+ FTP（File Transfer Protocol）：用于文件传输。
+ SMTP（Simple Mail Transfer Protocol）：用于电子邮件传输。
+ DNS（Domain Name System）：用于域名解析。
+ Telnet、SSH：用于远程登录。

**示例**：

+ Web 浏览器和服务器之间的通信
+ 电子邮件客户端和服务器之间的通信

### 总结
TCP/IP 四层模型简化了 OSI 七层模型，专注于实际的网络通信实现。每一层都有其特定的功能和协议，通过这些层次的协作，实现了复杂的网络通信任务。

+ **网络接口层**：处理物理网络上的数据传输。
+ **网络层**：负责数据包的路由和网络间通信。
+ **传输层**：提供端到端的数据传输服务，确保数据的可靠性和完整性。
+ **应用层**：为应用程序提供通信服务，处理高层协议和数据格式。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hb283pgk0nd2619p>