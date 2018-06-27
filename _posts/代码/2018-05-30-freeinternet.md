---
layout: blog
codes: true
background: yellow
title:  利用Vultr等云服务器实现校园网免流（5.26更新白名单代理）
date:   2018-05-26 23:11:30
background-image: https://upload.cc/i1/2018/05/20/yQAkPT.png
category: 代码
tags:
- ipv6
- ipv4
- vps
- linux
- finalshell
- Vultr
- SwitchyOmega
---
##  免流原理简介
![](https://upload.cc/i1/2018/05/20/UqNdoM.jpg)

## 主要实现步骤

### ipv6测试

首先我们需要测试自己的校园网是否接入ipv6网络，否则我们无法实现免流（不过按照以下步骤实现简单的fq还是可行的）。

去http://test-ipv6.com/测试一下就好了

### 选购服务器

#### 关于价格

目前支持ipv6的服务器还是很多的，对于大多数学生党来说，一般会选择$5/月的服务器。当然，如果你对服务器的流量和延迟有高要求（主要是看视频，直播和在线游戏），可以选择更为昂贵的，以及更好的节点。

我目前使用的是Vultr的洛杉矶$5/月的服务器，不过我的实际花费并没有这么多，因为像Vultr这些服务器经常会有一些不错的促销，而且最重要的是，他支持支付宝！

> 关于促销，我推荐一下Vultr中文网https://www.cnvultr.com/youhui/，上面有很多不错的优惠方案。我购买的时候是充值$10送$25的活动，再加上关注它的官方推特可以免费送$3，所以我只用了$10，获得了$38

而Bandwagon同样以便宜闻名，而且Bandwagon也支持支付宝，在网上也可以很容易的找到很多打折的优惠码。

####  服务器选择

一般来说离中国越近越好，这样可以有效的降低延迟。但是实际情况并不如此，比如说如果在电信网络访问日本节点，就会先访问美国，然后从美国访问到日本，在访问回来，绕了整个地球。对于完全不知道挑选时，推荐的挑选方法。

1. 访问官方提供的测速网站，如Vultr：https://www.vultrvps.com/test-server然后可以下载官方提供的测试文件来测试现在速度。
2. 延迟测试。Ctrl+F，输入CMD打开命令提示符。然后输入ping+空格+以上网址提供的IP地址来测试服务器的延迟。同时这个方法对于以后买的Vultr服务器也适用，只要输入你所购买的服务器的IP地址即可。对于Vultr，由于Vultr是按小时计费的，所以你可以随时更换你的服务器，然后每一个服务器都进行延迟测试，从而选择最佳的服务器。

### 服务器配置
#### 服务器连接

> 一般来说连接上服务器的只要方式都是用Xshell6等传统方式，在这里我推荐一个国产的更好用的SSH，名字是FinalShell,官网http://www.hostbuf.com/?from=about，可以尝试高级版。![](https://upload.cc/i1/2018/05/20/2TtjgP.png)
>
> 神奇的是它可以直接对服务器内的文件进行直接访问。可以让操作稍微傻瓜化。

新建SSH链接，输入服务器的IP地址，进行连接。然后会让你输入账号密码，账号一般默认为root，而密码Vultr是直接生成在他的服务器服务里，而Bandwagon需要选择Generate Password（其他服务器方法不尽相同）

#### 服务器配置

使用flyzy2005提供的一键配置脚本（也可以自行在网络上寻找详细脚本进行配置，这里知介绍建议方案）

##### 安装git

```
Centos执行这个： yum -y install git
Ubuntu/Debian执行这个： apt-get -y install git
```

#####　下载脚本

```
git clone https://github.com/flyzy2005/ss-fly
```

#####  安装及配置Shadowsocks

```
ss-fly/ss-fly.sh -i flyzy2005.com 1024
```
>其中flyzy2005.com换成你要设置的shadowsocks的密码即可（这个flyzy2005.com就是你ss的密码了，是需要填在客户端的密码那一栏的），密码随便设置，最好只包含字母+数字，一些特殊字符可能会导致冲突。而第二个参数1024是端口号，也可以不加，不加默认是1024~（举个例子，脚本命令可以是ss-fly/ss-fly.sh -i qwerasd，也可以是ss-fly/ss-fly.sh -i qwerasd 8585，后者指定了服务器端口为8585，前者则是默认的端口号1024，两个命令设置的ss密码都是qwerasd）


#####  开启BBR

```
ss-fly/ss-fly.sh -bbr
reboot #重启服务器
sysctl net.ipv4.tcp_available_congestion_control#测试bbr是否开启，如果返回结果含有bbr则开启成功。
```

#####  配置ipv6（核心）

打开并编辑shadowsocks.json，如果使用的是FinalShell可以直接在etc文件夹下访问。将

```
"server":"0.0.0.0"
```

中的"0.0.0.0"改为"::"

#####  重启ss

```
ssserver -c /etc/shadowsocks.json -d restart
```

特别感谢: [vps+ss实现校园网IPv6免流 | flyzy小站](https://www.flyzy2005.com/tech/ss-ipv6-no-traffic/)

### 本机配置
打开本机的Shadowsocks客户端（[各版本shadowsocks客户端下载地址](https://www.flyzy2005.com/fan-qiang/shadowsocks/ss-clients-download/)）

输入你的服务器的ipv6地址（可以在服务器官网找到）和你刚才设置的密码和端口。进入校园网环境，开启全局模式（如果是拨号，需要把你的拨号设置为纯英文）。就可以开始使用了。

如果需要测试服务是否成功开启，可以到上面的ipv6测试网站，如果返回的是服务器的ipv6地址而不是本机的，说明你已经成功了。如果需要浏览器以外的软件配置，而一进入特定软件的代理配置，然后把服务器和端口换成127.0.0.1和1080即可。（也可以使用Proxifer，具体方法自行百度）

> 5.26 更新：利用插件SwitchyOmega实现白名单代理

### SwitchyOmega代理设置
一般来说我们希望所有的网站都通过代理来运行，这样子我们就可以实现全网免流了。可是总有一些网站我们是不希望走代理的。如本身就支持IPv6的各大高校资源站，或者是自己的校园内网，或者是一些国外无法访问的网站。而SS所提供的PAC模式一般都是黑名单模式，想要拥有一个自定义的白名单模式，就可以使用SwitchyOmega了。

SwitchyOmega是一个浏览器插件，在Chrome的网上应用店等地方都可以找到。下载完成后进入配置。

1.新建情景模式

2.选择代理服务器，并且输入一个喜欢的名字

![](https://upload.cc/i1/2018/05/26/Nu1P24.png)

3.然后代理服务器内输入如下内容

![](https://upload.cc/i1/2018/05/26/d4fy2Y.png)
4.然后选择下面的“不代理的地址列表”方框，在里面输入不想走代理的网站的名字
推荐输入方式“*xxx.xxx.com",在这个情况下所有不同前缀下的"xxx.xxx.com"都会不走代理。
