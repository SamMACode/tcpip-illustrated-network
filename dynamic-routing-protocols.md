# 动态选路协议

> 在之前讨论了静态选路，在配置接口时，以默认方式生成路由表项（对于直接连接的接口），并通过`route`命令增加表项（通常从系统自引导程序文件），或是通过`icmp`重定向生成表项。动态选路协议，它用于路由器间的通信。主要讨论`RIP`，也即选路信息协议（`Routing Information Protocol`），大多数`TCP/IP`实现都提供这个应用广泛的协议，然后讨论两种新的选路协议，`OSPF`和`BGP`。

## Ping程序

`ping`源于声纳定位操作，`Ping`程序由`Mike Muuss`编写，目的是为了测试另一台主机是否可达。该程序发送一份`ICMP`回显请求报文给主机，并等待返回`ICMP`回显应答。

```shell
madong@spotify-mac tcpip-illustrated-network % ping baidu.com
PING baidu.com (220.181.38.148): 56 data bytes
64 bytes from 220.181.38.148: icmp_seq=0 ttl=54 time=6.529 ms
64 bytes from 220.181.38.148: icmp_seq=1 ttl=54 time=6.244 ms
64 bytes from 220.181.38.148: icmp_seq=2 ttl=54 time=9.084 ms
64 bytes from 220.181.38.148: icmp_seq=3 ttl=54 time=8.209 ms
64 bytes from 220.181.38.148: icmp_seq=4 ttl=54 time=6.460 ms
64 bytes from 220.181.38.148: icmp_seq=5 ttl=54 time=11.701 ms
64 bytes from 220.181.38.148: icmp_seq=6 ttl=54 time=25.534 ms
64 bytes from 220.181.38.148: icmp_seq=7 ttl=54 time=11.240 ms
64 bytes from 220.181.38.148: icmp_seq=8 ttl=54 time=9.126 ms
64 bytes from 220.181.38.148: icmp_seq=9 ttl=54 time=6.612 ms
^C
--- baidu.com ping statistics ---
10 packets transmitted, 10 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 6.244/10.074/25.534/5.483 ms
```

当返回`ICMP`回显应答时，要打印出序列号和`TTL`，并计算往返时间（`TTL`位于`IP`首部中的生存时间字段）。`ping`程序通过在`ICMP`报文数据中的时间值来计算往返时间（用当前时间减去`ICMP`报文中的时间值），即往返时间。

在`ping baidu.com`时，输出的第一行包括目的主机的`IP`地址，尽管指定的是它的名字。这说明名字已经经过解析器被转换成`ip`地址了。通常，第`1`往返时间值要比其它的大，这是由于目的端的硬件地址不在`ARP`高速缓存中的缘故。第一个`RTT`使用的时间`6.529 ms`比第二个反馈`6.244 ms`多了约`0.3ms`。这可能是因为发送`ARP`请求和接收`ARP`应答所花费的时间。

