# 前言

- tcpdump 仅支持命令行格式使用，常用在 Linux 服务器中抓取和分析网络包。
- Wireshark 除了可以抓包外，还提供了可视化分析网络包的图形页面。

tcpdump:

```shell
ping baidu.com
sudo tcpdump -i en0 icmp and host 220.181.38.148 -nn //先运行
```

结果如下所示：

```
sudo tcpdump -i en0 icmp and host 220.181.38.148 -nn
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on en0, link-type EN10MB (Ethernet), capture size 262144 bytes
17:43:13.522781 IP 172.30.244.68 > 220.181.38.148: ICMP echo request, id 32459, seq 0, length 64
17:43:13.532348 IP 220.181.38.148 > 172.30.244.68: ICMP echo reply, id 32459, seq 0, length 64
17:43:14.524980 IP 172.30.244.68 > 220.181.38.148: ICMP echo request, id 32459, seq 1, length 64
17:43:14.534558 IP 220.181.38.148 > 172.30.244.68: ICMP echo reply, id 32459, seq 1, length 64
17:43:15.526302 IP 172.30.244.68 > 220.181.38.148: ICMP echo request, id 32459, seq 2, length 64
17:43:15.535949 IP 220.181.38.148 > 172.30.244.68: ICMP echo reply, id 32459, seq 2, length 64
17:43:16.530612 IP 172.30.244.68 > 220.181.38.148: ICMP echo request, id 32459, seq 3, length 64
```

结论：从 tcpdump 抓取的 icmp 数据包，我们很清楚的看到 `icmp echo` 的交互过程了，首先发送方发起了 `ICMP echo request` 请求报文，接收方收到后回了一个 `ICMP echo reply` 响应报文，之后 `seq` 是递增的。

![tcpdump 常用选项类](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018174633.jpg)

![tcpdump 常用过滤表达式类](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018174657.jpg)

下面用wireshark来抓：

![image-20201018175525319](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018175525.png)

![ping 网络包](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018175638.jpg)



## TCP三次握手和四次挥手

![HTTP 网络包](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018181945.jpg)

可以显示TCP的时序图：统计 (Statistics) -> 流量图 (Flow Graph)，然后，在弹出的界面中的「流量类型」选择 「TCP Flows」。

![TCP 流量图](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018182018.jpg)

服务器端收到客户端的 `FIN` 后，服务器端同时也要关闭连接，这样就可以把 `ACK` 和 `FIN` 合并到一起发送，节省了一个包，变成了“三次挥手”。

![四次挥手](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018182507.jpg)

## TCP异常情况

### **TCP 第一次握手的 SYN 丢包了**

![image-20201018204858352](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018204858.png)客户端重发5次。

### TCP第二次握手 SYN、ACK 丢包

为了模拟客户端收不到服务端第二次握手 SYN、ACK 包，我的做法是在客户端加上防火墙限制，直接粗暴的把来自服务端的数据都丢弃。

![image-20201018205144597](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018205144.png)

![img](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018205241.jpg)

**当第二次握手的 SYN、ACK 丢包时，客户端会超时重发 SYN 包，服务端也会超时重传 SYN、ACK 包。**

客户端 SYN 包超时重传的最大次数，是由 tcp_syn_retries 决定的，默认值是 5 次；服务端 SYN、ACK 包时重传的最大次数，是由 tcp_synack_retries 决定的，默认值是 5 次。

### TCP 第三次握手 ACK 丢包

服务端收不到第三次握手的 ACK 包，所以一直处于 `SYN_RECV` 状态：

![img](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018205543.jpg)

客户端是已完成 TCP 连接建立，处于 `ESTABLISHED` 状态：

![img](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018205602.jpg)

![img](https://raw.githubusercontent.com/haojunsheng/ImageHost/master/img/20201018205651.jpg)

在建立 TCP 连接时，如果第三次握手的 ACK，服务端无法收到，则服务端就会短暂处于 `SYN_RECV` 状态，而客户端会处于 `ESTABLISHED` 状态。

由于服务端一直收不到 TCP 第三次握手的 ACK，则会一直重传 SYN、ACK 包，直到重传次数超过 `tcp_synack_retries` 值（默认值 5 次）后，服务端就会断开 TCP 连接。

# 参考

[我用 Wireshark 让你“看见“ TCP](https://www.cnblogs.com/xiaolincoding/p/12922927.html)

