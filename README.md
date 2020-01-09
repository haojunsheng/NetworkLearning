<!--ts-->

<!--te-->

# 前言

必须掌握的： 

socket是对TCP/IP的抽象， 网络编程肯定绕不过socket，绝大部分语言都提供了socket相关的API。 

工作中直接对socket编程的机会不多，大多都是封装好的， 但是要理解socket在客户端和服务器端的区别，服务器端是如何维护连接的， 这就会引出一个重要的技术：I/O多路复用（select/poll/epoll) ，也是ngnix,redis等著名软件的基础。 

I/O多路复用是I/O模型之一，其他还有同步阻塞，同步非阻塞，信号驱动和异步。 

这方面最经典的书应该是《Unix网络编程了》。 

# 基础网络概念

 [vbird.md](vbird.md) 

# TCP

 [TCP.md](TCP.md) 

# socket编程

 [unix-network-socket-api.md](unix-network-socket-api.md) 

# http

具体的看这个。 [http-learning.md](http-learning.md) 

# web编程

 [web.md](web.md)


