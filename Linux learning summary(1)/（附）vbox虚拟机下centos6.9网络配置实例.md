# （附）vbox虚拟机下centos6.9网络配置实例
（转载本文需经原创作者同意并注明，本人github账户http://www.github.com/ywzhang）
### 本实例联网方法为host-only方式
### vbox软件设置
设置-网络-网卡一：（连接方式：host-only网络；界面名称：VirtualBox Host-Only Ethernet 

Adapter；控制芯片：Intel PRO/1000 MT 桌面；混杂模式：拒绝；接入网线打勾）

注意要在centos虚拟机shutdown时设置
### 设置虚拟机网卡（VirtualBox Host-Only Network）与本主机网卡关联
这里使用桥接方式

设置面板-网络和Internet-更改适配器选项

拖动选中两个网卡（我的PC上是VirtualBox Host-Only Network和WLAN），右键，点击桥接（注意：这里可能会因为

WLAN启用共享设置而报错，此时双击WLAN网卡，即本主机上网网卡，进入属性，关闭网络共享的对勾），此时会出现一个

网桥用以连接虚拟机网卡和PC网卡）
### 查看网桥配置信息
双击上一步新建立的网桥，查看详细信息，注意IP地址，子网掩码，默认网关，DNS服务器
### centos虚拟机内的配置

1.修改网卡配置文件

查看网卡名：

ifconfig

我的是eth0

2.进入配置文件：

vim /etc/sysconfig/network-scripts/ifcfg-eth0

配置文件内容：

DEVICE=eth0

HWADDR=...(不用动)

TYPE=Ethernet

UUID=...（不用动）

ONBOOT=yes(启动系统时自动启动服务)

BOOTPROTO=static(固定手动分配IP地址)

NM_CONTROLLED=yes

IPADDR=192.168.3.102(和你新建的网桥IP在同一个网段)

NETMASK=255.255.255.0(和新建的网桥同样的掩码)

GATEWAY=192.168.3.1（和新建的网桥同样的网关IP）


3.配置DNS服务：

进入配置文件：

vim /etc/resolv.conf

配置文件内容：

nameserver 192.168.3.1(和新建的网桥同样的DNS服务器IP)



4.配置主机名:

vim /etc/sysconfig/network

配置文件内容：

NETWORKING=yes

HOSTNAME=Ywzhang(你的主机名)

GATEWAY=192.168.3.1

