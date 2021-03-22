
#概述
##NAT/网络地址转换
	# Network Address Translation
	# 允许一个整体机构以一个公用IP地址出现在Internet上。即是一种把内部私有网络地址翻译成合法网络IP地址的技术。
	# NAT路由器在将内部网络的数据包发送到公用网络时，在IP包的报头把私有地址转换成合法的IP地址。


##网络保留地址
	A类：10.0.0.0—10.255.255.255      10.0.0.0/8
	B类：172.16.0.0—172.31.255.255   172.16.0.0/12
	C类：192.168.0.0—192.168.255.255 192.168.0.0/16

![](https://github.com/Lucille-CC/pictures/blob/master/Internet%E4%BF%9D%E7%95%99%E5%9C%B0%E5%9D%80.jpg?raw=true)

##分类
###静态NAT  / Static NAT

![]()

	# 一对一
	# IP地址对是一对一的，是一直不变的。

###动态NAT / Pooled NAT 

![]()

	# 多对多	
	# 所有被授权访问Internet的私有IP地址可随机转换为任何指定合法的 IP 地址
	# 路由器上配置一个外网 IP 地址池
	# 通信结束后，映射表才删除这个映射，该外网IP才被释放

###网络地址端口转换NAPT / Network Address Port Translation / Port-Level NAT

![]()

	# 多对一
	# 端口复用：内部网络的所有主机均可共享一个合法外部IP地址实现对Internet的访问
	# 隐藏网络内部的所有主机，有效避免来自 Internet 的攻击
	# 分类：
		1.  源NAT（Source NAT，SNAT）：
				* 修改数据包的源地址。
				* 改变第一个数据包的来源地址，它永远会在数据包发送到网络之前完成。如数据包伪装
		2. 目的NAT（Destination NAT，DNAT）：
				* 修改数据包的目的地址。
				* 与SNAT相反，它是改变第一个数据包的目的地地址。如平衡负载、端口转发和透明代理

#工作原理
##NAT技术四个术语

![]()

	1. 内部本地地址（Inside Local）：内网中设备所使用的IP地址
	2. 内部全局地址（Inside Global）：对于外部网络来说，局域网内部主机所表现的 IP 地址。
	3. 外部本地地址（Outside Local）：外部网络主机的真实地址。
	4. 外部全局地址（Outside Global）：对于内部网络来说，外部网络主机所表现的 IP 地址。外网设备所使用的真正的地址。


##STUN原理
	UDP穿越
利用STUN实现NAT穿越，主要是利用STUN Client向STUN Server发送STUN请求消息和接收STUN响应消息，得知其在出口NAT上的映射外部地址以及NAT类型等相关信息，然后报文负载中的地址信息就直接填写出口NAT 上的对外地址，而不是私网内用户的私有IP地址，并且告知目的端节点自己的接收地址和端口号。这样报文负载中的内容在经过NAT时就无需被修改了，只需按普通NAT流程转换报文头的IP地址即可，并且负载中的IP地址信息和报文头地址信息是一致的。同时根据发送端和接收端所处的NAT类型，采取响应的策略来完成NAT的穿越。

##基于STUN的NAT类型
###Full Cone NAT / 全锥NAT
![]()

	# 来自相同内部IP地址和端口号的请求消息将被映射到相同外部地址和端口号。
	# 把客户机主机地址(X:y)转换为NAT对外公网地址(A:b)，并且在NAT的映射表上添加一项新的映射关系
	# 特点： 双方就可以经NAT互相访问数据，即使是之前内网主机没有发送过连接请求给某一外网主机。

###Restricted NAT / 受限NAT
![]()

	# 来自相同内部IP地址和端口号的请求消息将被映射到相同的NAT对外公网地址和端口
	# 把客户机主机地址(X:y)转换为NAT对外公网地址(A:b)，并且在NAT的映射表上添加一项新的映射关系
	# 特点： 仅当内网主机曾经试图连接某一外网主机(IP为P)，这一外部主机和NAT之后的节点才可以经NAT互相访问数据。


###Port Restricted NAT / 端口受限NAT
![]()

	# 来自相同内部IP地址和端口号的请求消息将被映射到相同的NAT的外部地址和端口号
	# 把客户机主机地址(X:y)转换为NAT对外公网地址(A:b)，并且在NAT的映射表上添加一项新的映射关系
	# 特点： 仅当在这之前该内部节点曾经发送过数据包到外部主机(IP为P，且端口为q)，这一外部主机可以经NAT向NAT之后的节点发送数据包，而内网主机不能向外部发送。


###Symmetric NAT / 对称NAT
![]()

	# 只有来自内部IP 地址和端口号，并且发往同一个目的地址和端口的请求消息才会被映射到相同的NAT 外部地址和端口号。
	# 把客户端地址(X:y)转换成公网地址(A:b)，并且绑定为(X:y)-(A:b)-(P:q)，在映射表中也会添加一项新的地址映射关系；
	# 特点： 当且仅当在这之前该内部节点曾经发送过数据包到外部主机(P:q)，这一外部主机才可以经NAT向NAT之后的节点发送数据包。

##检查NAT设备的类型

![]()

##NAT穿越流程
###两台主机位于同一个NAT设备之后
![]()

1. Client A 登录Server，发送注册消息给STUN Server。NAT A 将为这次的会话分配一个端口55000，那么Server 收到的Client A 的公网地址是202.120.36.18:55000，私有地址是：192.168.47.108:12345。
2. Client B 登录Server，发送注册消息给STUN Server。NAT B 将为这次的会话分配一个端口66000，那么Server 收到的Client B 的公网地址是202.120.36.18:66000，私有地址是：192.168.47.110:54321。
3. 此时Client A 与Client B 都可以与STUN Server 通信了。它们分别从STUN Server 那里得到对方的私有IP 地址和公网IP 地址。
4. 对于发向公网IP 地址的数据包，由于Client A 和Client B 是处在同一NAT 之后，所以只有NAT 支持“发夹”转换，数据包才能到达目的地。对于发向私有IP 地址的数据，由于Client A 和B 位于同一局域网内，那么数据也会到达目的地，并且速度更快。因此，Client A 和B 将会选择后一种方式进行后续的数据发送。

注：所谓的“发夹”转换是指NAT 不仅转换目的地址，而且还转换源地址，然后直接把数据包转发给内部网络。一般行为较好的NAT 设备才会支持“发夹”地址转换功能

###两台主机分别位于不同的NAT设备之后
![]()

1. Client A 登录Server，发送注册消息给STUN Server。NAT A 将为这次的会话分配一个端口55000，那么Server 收到的Client A 的公网地址是202.120.23.79:55000，私有地址是：192.168.0.12:12345。
2. Client B 登录Server，发送注册消息给STUN Server。NAT B 将为这次的会话分配一个端口66000，那么Server 收到的Client B 的公网地址是202.120.36.181:66000，私有地址是：192.168.47.108:54321。
3. 此时Client A 与Client B 都可以与Server 通信了。如果Client A 此时想直接发送UDP 数据包给Client B，那么Client A 可以从Server 获得B 的公网地址202.120.36.181:66000 以及私有地址192.168.47.108:54321。但是Client A 还不能使用该公网地址直接和Client B 通信，因为此时NAT B 会将Client A 主动发送来的数据包丢弃，因为此数据包是没有会话记录的。
4. 现在需要在NAT B 建立一个内部地址192.168.47.108:54321 到公网地址202.120.36.181:66000 的映射关系，那么Client A 发送到202.120.36.181:66000的信息，Client B 就能收到了。
5. Server 负责向Client B 发送NAT穿越指令。
6. Client B 向Client A 的公网地址发送一个UDP报文，虽然此数据包会被NAT A 丢弃。但是NAT B建立了会话记录(在NAT B 地址映射表中建立了一项地址映射关系)，不会丢弃来自Client A的数据包了。
7. Server 通知Client A 可以发连接数据包到Client B 了，这些数据可以穿越NAT B 设备到达Client B。
8. Client A 发数据包到Client B(第一份数据包会在NAT A 的地址映射表中建立一项相关的地址映射关系)。
9. 至此双方可以进行UDP 通信了。


###两台主机位于多层NAT设备之后
略
在这种情况下，一定要利用共同NAT设备的“发夹”转换功能才能实现穿越