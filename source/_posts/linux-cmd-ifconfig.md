---
title: ifconfig
date: 2018-08-11 10:30:42
categories:
  - linux
  - cmd
---

ifconfig命令被用于配置和显示Linux内核中网络接口的网络参数

使用格式 `ifconfig [网络设备] [参数]`

在命令行输入 ifconfig 可以看到如下输出

    eth0    Link encap:Ethernet  HWaddr 00:16:3e:08:09:42
            inet addr:172.18.179.43  Bcast:172.18.191.255  Mask:255.255.240.0
            UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
            RX packets:5670311 errors:0 dropped:0 overruns:0 frame:0
            TX packets:3032111 errors:0 dropped:0 overruns:0 carrier:0
            collisions:0 txqueuelen:1000
            RX bytes:3922309247 (3.9 GB)  TX bytes:2221955398 (2.2 GB)
    lo      Link encap:Local Loopback
            inet addr:127.0.0.1  Mask:255.0.0.0
            UP LOOPBACK RUNNING  MTU:65536  Metric:1
            RX packets:728000 errors:0 dropped:0 overruns:0 frame:0
            TX packets:728000 errors:0 dropped:0 overruns:0 carrier:0
            collisions:0 txqueuelen:1
            RX bytes:473042785 (473.0 MB)  TX bytes:473042785 (473.0 MB)

- **eth0**表示第一块网卡，其中**HWaddr**表示网卡的物理地址，可以看到目前这个网卡的物理地址(MAC地址）是00:16:3e:08:09:42。
- **Link encap** 表示网络类型是：Ethernet
- **HWaddr** 表示网卡的物理地址是00:16:3e:08:09:42。(一般前三段表示厂商，后三段表示生产的流水线)
- **inet addr** 表示网卡的IP地址是172.18.179.43
- **Bcast** 表示网卡的广播地址是:172.18.191.255
- **Mask** 表示子网掩码是:255.255.240.0
- **MTU** 最大发送单元
- ****

**lo**是表示主机的回环地址，这个一般是用来测试一个网络程序，但又不想让局域网或外网的用户能够查看，只能在此台主机上运行和查看所用的网络接口。比如把 httpd服务器的指定到回环地址，在浏览器输入127.0.0.1就能看到你所架WEB网站了。但只是您能看得到，局域网的其它主机或用户无从知道。

第一行：连接类型：Ethernet（以太网）HWaddr（硬件mac地址）。
第二行：网卡的IP地址、子网、掩码。
第三行：UP（代表网卡开启状态）RUNNING（代表网卡的网线被接上）MULTICAST（支持组播）MTU:1500（最大传输单元）：1500字节。
第四、五行：接收、发送数据包情况统计。
第七行：接收、发送数据字节数统计信息。

etho网卡的配置文件路径为 `/etc/sysconfig/network-scripts/ifcfg-eth0`