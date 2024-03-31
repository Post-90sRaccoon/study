# 简单的 HTTP 协议

[TOC]

## HTTP 协议用于客户端和服务器之间的通信

## 通过请求和相应的交换达成通信

![image-20240318214314065](2%E7%AE%80%E5%8D%95%E7%9A%84HTTP%E5%8D%8F%E8%AE%AE.assets/image-20240318214314065.png)

GET 表示请求访问服务器的类型，称为方法。 `/index.htm`  指明了请求访问的资源对象，也叫请求 URI。最后是 HTTP 版本号。



请求报文是由请求方法、请求 URL、协议版本、可选的请求首部字段和内容实体构成。

![image-20240318214943184](2%E7%AE%80%E5%8D%95%E7%9A%84HTTP%E5%8D%8F%E8%AE%AE.assets/image-20240318214943184.png)



服务器将请求内容以相应方式返回。

![image-20240318215354207](2%E7%AE%80%E5%8D%95%E7%9A%84HTTP%E5%8D%8F%E8%AE%AE.assets/image-20240318215354207.png)

状态码（status code），原因短语（response phrase）。

## HTTP 是不保存状态的协议

无状态（stateless）协议，不对请求和相应之间的通信状态进行保存。

## 请求 URI 定位资源

如果不是访问特定资源而是对服务器本身发起请求，可以用 * 代替请求 URI。

## 告知服务器意图的 HTTP 方法

### GET: 获取资源

GET 方法用来请求访问已被 URI 识别的资源。如果是 CGI（Common Gateway Interface）通用网关接口，则返回经过执行后的输出结果。

### POST: 传输实体主体

POST 方法用来传输实体的主体。

### PUT: 传输文件

PUT 方法用来传输文件。

HTTP/1.1 PUT 方法不带验证机制，存在安全性问题。配合 Web 应用程序的验证机制，或架构设计采用 REST（Representational State Transfer，表征状态转移）使用。

### HEAD: 获得报文首部

与 GET 一样，只是不返回报文主体。用于确认 URI 的有效性及资源更新的日期时间等。

### DELETE: 删除文件

HTTP/1.1 DELETE 方法不带验证机制。

### OPTIONS: 询问支持的方法

查询镇定请求 URI 指定的资源支持的方法。响应带 Allow 头。

### TRACE: 追踪路径

TRACE 方法是让服务器返回客户端发送的请求。发送请求时 Max-Forwards 限制转发数，经过代理服务器，代理服务器会将他减一。经过路由器不会减一，路由器工作在网络层，不会涉及应用层。

可以防止循环，限制转发次数，可以对比请求是否被修改/篡改。容易引发 XST（Cross-Site Tracing，跨站追踪）,通常不会用。

### CONNECT: 要求用隧道协议连接代理

要求与代理服务器通信时建立隧道[^1]，实现用隧道协议进行 TCP 通信，代理服务器将[TCP](https://zh.wikipedia.org/wiki/传输控制协议)连接转发到所需的目的地。主要使用 SSL（Secure Sockets Layer, 安全套接层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容加密后经网络隧道传输。

[^1]: 利用HTTP的 CONNECT 方法在两台网络受限的计算机间建立网络链接，通常一方是在受限网络的内部，一方在外部，借外部方来代理内部方的流量。 其中，网络受限包括“防火墙”、“NAT”和“访问控制”等。

## 持久连接节省通信量

### 持久连接

持久连接（HTTP Persistent Connections, 也称为 HTTP keep-alive 或 HTTP connections reuse）。只要一段没有明确断开连接，则保持 TCP 连接状态。

### 管线化

pipelining 同时并行发送多个请求。

## 使用 Cookie 的状态管理

服务器端相应报文带 Set-Cookie 首部字段信息。

## 