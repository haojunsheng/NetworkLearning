# 前言

必须掌握的： 

socket是对TCP/IP的抽象， 网络编程肯定绕不过socket，绝大部分语言都提供了socket相关的API。 

工作中直接对socket编程的机会不多，大多都是封装好的， 但是要理解socket在客户端和服务器端的区别，服务器端是如何维护连接的， 这就会引出一个重要的技术：**I/O多路复用（select/poll/epoll)** ，也是ngnix,redis等著名软件的基础。 

I/O多路复用是I/O模型之一，其他还有同步阻塞，同步非阻塞，信号驱动和异步。 

这方面最经典的书应该是《Unix网络编程了》。 

学了一段时间的计算机网络，先进行总结下，在继续来学习，学而不思则罔，古人诚不欺我。

首先需要学习的计算机网络的基础概念和几层协议，这里主要参考的鸟哥的Linux私房菜，其中比较重要的概念有

- 网络IP层，需要掌握的知识点包括IP表头结构，IP地址的表示，IP地址的组成，分级，**Public IP**和**Private IP**，NAT技术，DHCP技术，**Netmask**，路由技术，IP和MAC地址的转换技术ARP。
- 传输层TCP、UDP：TCP的表头结构，三次握手，四次挥手的详细细节，TCP的重传机制，TCP滑动窗口机制，拥塞处理机制
- 网络层http：这一块也是硬骨头，需要掌握http 0.9/1.1/2/3之间的区别，http支持的方法，持久连接技术，cookie技术，http报文详解，压缩技术，MIME类型，状态码，代理技术，缓冲技术，重定向技术，https，http认证方式，http的优化Websocket技术。
- socket网络编程：我们需要掌握socket网络编程,包括套接字的结构，常用的套接字的函数，select和poll模型。
- 需要学会使用常见的命令**ifconfig**，**netstat**，ping，traceroute，nslookup，tcpdump等命令和**wireshark**分析网络包的工具。
- 区分常见的协议：socket，http，rpc，websocket等，以及计算机通信技术的演化过程。
- 后端编程：我们需要熟练使用socket编程的API；http server的四个演化阶段：socket来传输文件，多进程编程，Select模型模型，epoll模型；理解**web服务器VS web容器**的区别；静态服务器向动态服务器的演化的过程中的技术：CGI标准，Java的Servlet，Tomcat，以及Python的WSGI。需要系统的掌握：浏览器和服务器是怎么打交道的（HTTP协议），url 和代码之间的关联，数据的验证、转换和绑定，数据库访问（ORM），业务代码的执行，如何把对象变成json和其他格式。

这些大概就是计算机网络和后端编程需要掌握的基础知识了。

# 基础网络概念

网络IP层，需要掌握的知识点包括IP表头结构，IP地址的表示，IP地址的组成，分级，**Public IP**和**Private IP**，NAT技术，DHCP技术，**Netmask**，路由技术，IP和MAC地址的转换技术ARP。

 [vbird.md](vbird.md) 

# TCP

传输层TCP、UDP：TCP的表头结构，三次握手，四次挥手的详细细节，TCP的重传机制，TCP滑动窗口机制，拥塞处理机制。

 [TCP.md](TCP.md) 

# socket编程

socket网络编程：我们需要掌握socket网络编程,包括套接字的结构，常用的套接字的函数，select和poll模型。

[https://haojunsheng.github.io/2020/01/inter-process-communication/#24-%E5%A5%97%E6%8E%A5%E5%AD%97%E6%8E%A5%E5%8F%A3](https://haojunsheng.github.io/2020/01/inter-process-communication/#24-套接字接口)

 [unix-network-socket-api.md](unix-network-socket-api.md) 

# http

网络层http：这一块也是硬骨头，需要掌握http 0.9/1.1/2/3之间的区别，http支持的方法，持久连接技术，cookie技术，http报文详解，压缩技术，MIME类型，状态码，代理技术，缓冲技术，重定向技术，https，http认证方式，http的优化Websocket技术。

 [http-learning.md](http-learning.md) 

# 常见协议的区分和RPC的演化

区分常见的协议：socket，http，rpc，websocket等，以及计算机通信技术(ftp,rmi,SOAP,restful)的演化过程。

 [protocol-diff-and-rpc.md](protocol-diff-and-rpc.md) 

# 后端编程

后端编程：我们需要熟练使用socket编程的API；http server的四个演化阶段：socket来传输文件，多进程编程，Select模型模型，epoll模型；理解**web服务器VS web容器**的区别；静态服务器向动态服务器的演化的过程中的技术：CGI标准，Java的Servlet，Tomcat，以及Python的WSGI。需要系统的掌握：浏览器和服务器是怎么打交道的（HTTP协议），url 和代码之间的关联，数据的验证、转换和绑定，数据库访问（ORM），业务代码的执行，如何把对象变成json和其他格式。

 [http-server.md](http-server.md) 

# wireshark

要学会使用常见的命令**ifconfig**，**netstat**，ping，traceroute，nslookup，tcpdump等命令和**wireshark**分析网络包的工具。

# 常见问题

## C10K 问题

如何在一台物理机上同时服务10000个用户？这里C表示并发，10K等于10000。

操作系统最多支持 1024 个描述符。
