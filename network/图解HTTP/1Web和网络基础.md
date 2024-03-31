# 了解 Web 及网络基础

[TOC]

## 使用 HTTP 访问 Web



HTTP (HyperText Transfer Protocol) 超文本传输协议。

## 网络基础 TCP/IP

### TCP/IP 协议族

通信的规则称为协议（protocol）。把互联网相关联的协议集合总称为 TCP/IP（协议族）。

### TCP/IP 的分层管理

TCP/IP 协议族按层次分为 4 层：应用层、传输层、网络层、数据链路层。

#### 应用层

向用户提供应用服务时通信的活动。TCP/IP 协议族预存了各类通用的应用服务。如 FTP(File Transfer Protocol，文件传输协议) 和 DNS （Domain Name System, 域名系统）。HTTP 协议也出于该层。

#### 传输层

提供出于网络连接的两台计算机之间的数据传输。有两个协议 TCP（Transmission Control Protocol，传输控制协议） 和 UDP (User Data Protocol，用户数据报协议)。

#### 网络层（又名网络互联层）

处理网络上流动的数据包（网络传输的最小数据单位）。该层选择一条传输线路。

#### 数据链路层（又名网络接口层）

用来连接网络的硬件部分。包括控制操作系统、迎接的设备驱动、NIC（Network Interface Card，网络适配器，即网卡），及光纤等物理可见部分。

### TCP/IP 通信传输流

客户端在应用层发一个 HTTP 请求，传输层把数据分隔，在各个报文上打上标记序号即端口号转发给网络层。网络层增加通信目的地的 MAC 地址转发给链路层。把数据信息包装起来的做法称为封装（encapsulate）。

## 与 HTTP 关系密切的协议： IP、TCP 和 DNS

### 负责传输的 IP 协议

IP 协议位于网络层，把数据包传送给对方。最重要的是 IP 地址和 MAC 地址（Media Access Control Address）。

#### 使用 ARP 协议凭借 MAC 地址进行通信

通信双方不在局域网 （LAN）内，进行中传时，使用下一站中转设备的 MAC 地址来搜索下一个中转目标。ARP 协议（Address Resolution Protocol）。根据通信方的 IP 地址反查出对应的 MAC 地址。

### 确保可靠性的 TCP 协议

TCP 位于传输层，提供可靠的字节流服务（Byte Stream Service）。将大块数据分隔成报文段（segment）进行管理。可靠指能够确认数据最终能否发送给对方。

#### 确保数据能到达目标

TCP 采用了三次握手（three-way handshaking）。握手过程中使用了 TCP 的标志（flag）。SYN （Synchronize） 和  ACK（acknowledgement）。

发送带有 SYN 数据包给对方，对方收到后回传带有 SYN/ACK 数据包。然后回传带 ACK 的数据包。

## 负责解析域名的 DNS 服务

DNS （Domain Name System）服务是和 HTTP 协议一样位于应用层的协议。他提供域名到 IP 地址之间的域名解析服务。

## URI 和 URL

### 统一资源标识符

URI 全称 Uniform Resource Identifier。URL 全称 Uniform Rescource Locator，统一资源定位符。是由某个协议表示的资源定位标识符。URI 用字符串标识某一互联网资源，而 URL 表示资源的地点（互联网上的位置）。URL 是 URI 的子集。
