---
title: VPS Management Guides
subtitle: Some basic skills to manage linux servers
summary: Some basic skills to manage linux servers
author: Will
tags: 
- vps
- linux
categories: 
- vps
- linux
date: "2021-02-01"
---

# 主机管理指南

## 1. v2ray+nginx+ws+tls一键脚本

```bash
# 安装curl,wget
yum -y install wget    ##ContOS Yum安装wget
apt-get install wget   ##Debian Ubuntu安装 wget
apt-get update -y && apt-get install curl -y    ##Ubuntu/Debian 系统安装 Curl 方法
yum update -y && yum install curl -y            ##Centos 系统安装 Curl 方法
# 一键脚本
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh" && chmod +x install.sh && bash install.sh
# or
bash <(curl -L -s https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh) | tee v2ray_ins.log
# 管理
systemctl start v2ray #启动 V2ray 
systemctl stop v2ray #停止 V2ray 
systemctl start nginx #启动 Nginx 
systemctl stop nginx #停止 Nginx 
伪装的 Web 目录：/home/wwwroot/3DCEList
V2ray 服务端配置：/etc/v2ray/config.json
Nginx 目录： /etc/nginx
证书文件: /data/v2ray.key 和 /data/v2ray.crt
```

## 2. trojan面板一键脚本

```bash
# 安装
wget -N --no-check-certificate "https://raw.githubusercontent.com/V2RaySSR/Trojan_panel_web/master/trojan-web-panel.sh" && chmod +x trojan-web-panel.sh && ./trojan-web-panel.sh
# 伪装站点目录：/usr/share/nginx/html
#or
source <(curl -sL https://git.io/trojan-install)
# 卸载
source <(curl -sL https://git.io/trojan-install) --remove
Usage:
  trojan [flags]
  trojan [command]

Available Commands:
  add         添加用户
  completion  自动命令补全(支持bash和zsh)
  del         删除用户
  help        Help about any command
  info        用户信息列表
  log         查看trojan日志
  restart     重启trojan
  start       启动trojan
  status      查看trojan状态
  stop        停止trojan
  tls         证书安装
  update      更新trojan
  updateWeb   更新trojan管理程序
  version     显示版本号
  web         以web方式启动

Flags:
  -h, --help   help for trojan
```

## 3. trojan,v2ray六合一脚本(Jeannie)

```bash
sudo -i
bash -c "$(curl -fsSL https://raw.githubusercontent.com/JeannieStudio/all_install/master/SixForOne_install.sh)"
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh"
chmod +x tcp.sh
./tcp.sh
```

## 4. SSR一键安装脚本

```bash
# doubi
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod  x ssr.sh && bash ssr.sh
bash ssr.sh
## 秋水
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
#启动SSR：
/etc/init.d/shadowsocks-r start
#退出SSR：
/etc/init.d/shadowsocks-r stop
#重启SSR：
/etc/init.d/shadowsocks-r restart
#SSR状态：
/etc/init.d/shadowsocks-r status
#卸载SSR：
./shadowsocks-all.sh uninstall
#如果需要更改SSR的相关配置参数，配置文件位置在：/etc/shadowsocks-r/config.json
```

## 5. BBR,锐速等负载均衡脚本

```bash
yum -y install wget    #ContOS 安装 wget
apt-get install wget   #Debian Ubuntu 安装 wget
yum -y install curl    #ContOS 安装 curl
apt-get install curl   #Debian Ubuntu 安装 crul
# debian 9自带bbr开启
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
lsmod | grep bbr
# 多种加速合集
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

## 6. 中转脚本

### 6.1 brook

```bash
# doubi
wget -N --no-check-certificate https://zhujiget.com/wp-content/uploads/2020/brook-pf.sh && chmod +x brook-pf.sh && ./brook-pf.sh
wget  -N --no-check-certificate https://zhujiwiki.com/wp-content/uploads/2020/01//brook-pf-mod.sh && chmod +x brook-pf-mod.sh
./brook-pf-mod.sh
```

### 6.2 iptables

```bash
# centos 7 关闭防火墙
yum -y install firewall*  ##安装firewall命令
yum -y remove firewall*   ##卸载firewall命令
systemctl start firewalld.service  ##打开firewall命令
systemctl stop firewalld.service   ##关闭firewall命令
# 一键脚本
yum install -y wget && wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/iptables-pf.sh && chmod +x iptables-pf.sh && bash iptables-pf.sh 
# or
wget  -N --no-check-certificate https://zhujiget.com/wp-content/uploads/sh/iptables-pf.sh && chmod +x iptables-pf.sh
bash iptables-pf.sh
# 中转域名
systemctl disable firewalld.service
systemctl stop firewalld.service

setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config  
sed -n '/^net.ipv4.ip_forward=1/'p /etc/sysctl.conf | grep -q "net.ipv4.ip_forward=1"
if [ $? -ne 0 ]; then
    echo -e "net.ipv4.ip_forward=1" >> /etc/sysctl.conf && sysctl -p
fi
# 脚本
wget -qO natcfg.sh https://raw.githubusercontent.com/arloor/iptablesUtils/master/natcfg.sh && bash natcfg.sh
# or
wget -qO natcfg.sh http://www.arloor.com/sh/iptablesUtils/natcfg.sh && bash natcfg.sh
```

### 6.3 socat

```bash
# moerats
wget https://www.moerats.com/usr/shell/socat.sh && bash socat.sh
# or
wget https://blog.6y.ee/linux/socat1.sh && bash socat1.sh
```

### 6.4 gost

```bash
wget -N --no-check-certificate https://github.com/ginuerzh/gost/releases/download/v2.11.0/gost-linux-amd64-2.11.0.gz && gzip -d gost-linux-amd64-2.11.0.gz
mv gost-linux-amd64-2.11.0 gost
chmod +x gost
# 1
apt install screen     #Debian/Ubuntu系统
yum install screen     #centos系统
screen -S gost
## 落地机
./gost -D -L "ws://:8080?path=/ws&rbuf=4096&wbuf=4096&compression=false"
## 中转机
./gost -L=:本地端口/落地IP:落地端口 -F=ws://落地IP:8080/ws  #本地端口为中转机可以使用的端口
# 2
# 命令之前加nohup使其常挂后台，例：nohup ./gost -L=:本地端口/落地IP:落地端口 -F=ws://落地IP:443/ws
## 落地机
./gost -L relay+wss://:12345/127.0.0.1:2333 #将监听端口12345的流量传递给vpn服务的端口
## 中转机
./gost -L udp://:30644 -L tcp://:30644 -F relay+wss://202.182.111.39:12345
# 30644为nat服务器向用户传送的端口，wss之后为中转ip和监听端口
# 还可以有其它加密方式，如relay+mtls，tls,mwss，relay可转发tcp+udp
# 不加密转发 直接监听本机 443 端口，转发到落地节点 1.2.3.4 的 443 端口上。
gost -L=tcp://:443/1.2.3.4:443 -L=udp://:443/1.2.3.4:443
# forward
# nat
gost -L=tcp://:443 -L=udp://:443 -F=forward+mtls://1.2.3.4:443?mbind=true
# 监听 443 端口并利用 mtls 传输到落地节点 1.2.3.4 的 443 端口，?mbind=true 启用多路复用
# 落地
gost -L=mtls://:443/127.0.0.1:843
# 落地节点的 SSR 进程监听 127.0.0.1:843，然后 gost 监听 443 端口转发到 SSR 上即可
```

> 支持的传输类型有：
>
> - `tcp` - 原始TCP
> - `tls` - TLS
> - `mtls` - Multiplex TLS，在TLS上增加多路复用功能 (2.5+)
> - `ws` - Websocket
> - `mws` - Multiplex Websocket，在Websocket上增加多路复用功能 (2.5+)
> - `wss` - Websocket Secure，基于TLS加密的Websocket
> - `mwss` - Multiplex Websocket Secure，在基于TLS加密的Websocket上增加多路复用功能 (2.5+)
> - `kcp` - KCP (2.3+)
> - `quic` - QUIC (2.4+)
> - `ssh` - SSH (2.4+)
> - `h2` - HTTP2 (2.4+)
> - `h2c` - HTTP2 Cleartext (2.4+)
> - `obfs4` - OBFS4 (2.4+)
> - `ohttp` - HTTP Obfuscation (2.7+)
> - `otls` - TLS Obfuscation (2.11+)

### 6.5 firewalld

```bash
yum install firewalld -y
# 常用命令
systemctl start firewalld ##开启防火墙
systemctl stop firewalld ##关闭防火墙
firewall-cmd --reload ##重启防火墙
systemctl status firewalld ##查看防火墙状态
systemctl enable firewalld ##设置开启启动
systemctl disable firewalld ##禁用开机启动
firewall-cmd --list-ports ##查看开放的端口
firewall-cmd --zone=public --add-port=8080/tcp --permanent ##开放指定端口（此处为开放8080，可替换为指定端口号），开放后需重启防火墙生效
firewall-cmd --zone=public --remove-port=8080/tcp --permanent ##关闭指定端口（此处为关闭8080，可替换为指定端口号）
# 开启路由转发
echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
sysctl -p
# 开启防火墙流量伪装
firewall-cmd --zone=public --permanent --add-masquerade
#开启TCP流量端口
firewall-cmd --add-port=8080/tcp --permanent
#开启UDP流量端口
firewall-cmd --add-port=8080/udp --permanent
# 转发本地端口
#开启TCP流量转发
firewall-cmd --add-forward-port=port=8080:proto=tcp:toport=8090 --permanent 
#开启UDP流量转发
firewall-cmd --add-forward-port=port=8080:proto=udp:toport=8090 --permanent
# 转发远程服务器端口
#开启TCP流量转发
firewall-cmd --add-forward-port=port=8080:proto=tcp:toaddr=2.2.2.2:toport=666 --permanent
#开启UDP流量转发
firewall-cmd --add-forward-port=port=8080:proto=udp:toaddr=2.2.2.2:toport=666 --permanent
# 重载配置文件
firewall-cmd --reload
```

### 6.6 tinyPortMapper

```bash
wget https://www.moerats.com/usr/shell/tinyPortMapper.sh && bash tinyPortMapper.sh
# 卸载
rm -rf /tinyPortMapper
# 清除开机自启
#CentOS系统，编辑/etc/rc.d/rc.local，删除tinyPortMapper启动命令。
#Debian/Ubuntu系统，编辑/etc/rc.local，删除tinyPortMapper启动命令。
```

## 7. 测速及测回程脚本

```bash
# 国内测速
wget -N --no-check-certificate https://raw.githubusercontent.com/V2RaySSR/vps/master/vpstest.sh && bash vpstest.sh
# bench bensh.sh测试的内容包括VPS系统基本信息、全球各知名数据中心的测试点的速度（支持IPv6）以及IO 测试
wget -qO- bench.sh | bash
# superbench 测试VPS系统基本信息和IO性能，不过将测速节点换成了国内节点
wget -qO- git.io/superbench.sh | bash
# zbench
wget -N --no-check-certificate https://raw.githubusercontent.com/FunctionClub/ZBench/master/ZBench-CN.sh && bash ZBench-CN.sh
# 91yun
wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/91yuntest/master/test.sh && bash test.sh -i "io,bandwidth,chinabw,download,traceroute,backtraceroute,allping"
# 国内测速（最全）
bash <(curl -Lso- https://git.io/superspeed)
# 回程路由测试 1
#下载
 wget https://cdn.ipip.net/17mon/besttrace4linux.zip
#解压
unzip besttrace4linux.zip
#使用
./besttrace -q 1 这里是目标IP
# 回程路由测试 2
# CentOS系统：
yum update && yum install traceroute -y
# Debian/Ubuntu系统：
apt-get update && apt-get install traceroute -y
# 然后就可以通过 traceroute x.x.x.x 来路由追踪了。默认是测试3次的，所以有时候会显示很乱，你可以加上 -q 1 ，比如  #traceroute -q 1 x.x.x.x 。这个参数指的是只测试一次，当然之所以测试3次就为因为可能会丢包等情况，三次可以比较准确。
# mtr 回程 3
# CentOS系统：
yum update && yum install mtr -y 
# Debian/Ubuntu系统：
apt-get update && apt-get install mtr -y
# 然后就可以通过 mtr x.x.x.x 来路由追踪了。不过上面这个命令 是动态显示的，一直持续下去，除非手动终止。
#如果你需要只发送 100个数据包（测试100次），那么你可以这样写： mtr -c 100 --report x.x.x.x
#这个命令不会动态显示，只会在发送 100个数据包后，直接显示最终结果。
# 测试三网回程
wget -qO- git.io/besttrace | bash
# vps性能测试
curl -fsSL https://ilemonrain.com/download/shell/LemonBench.sh | bash
```

## 8. VPS探针-状态监控

```bash
wget https://raw.githubusercontent.com/CokeMine/ServerStatus-Hotaru/master/status.sh && chmod +x status.sh
# wget -N --no-check-certificate https://southcat.net/bash/status.sh && chmod +x status.sh
# 客户端管理菜单
bash status.sh c
# 服务端管理菜单
bash status.sh s 
# 修改主页
cd /usr/local/ServerStatus/web/
vi /usr/local/ServerStatus/web/index.html
注释小标题和背景图片为空白：
```

```html
<!--section class="background"-->
                    <div class="container">
                        <header>
                            <h1>
                                Server Status
                            </h1>
                        </header>
                    </div>
                <!--/section-->
```

## 9. 搭建docker

```bash
# 按照流程安装 1. ubuntu&debian
sudo apt-get remove docker \
               docker-engine \
               docker.io            #卸载旧的docker
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
# 国内源    
$ curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# 官方源
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -    

$ sudo add-apt-repository \
    "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
# 官方源
$ sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
# 以上命令会添加稳定版本的 Docker CE APT 镜像源
# 开始安装
$ sudo apt-get update
$ sudo apt-get install docker-ce
# 卸载
sudo apt-get purge docker-ce
# 删除Docker镜像、容器、数据卷等文件
sudo rm -rf /var/lib/docker

## 按照流程安装 2. centos 7
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
$ sudo yum install -y yum-utils \
           device-mapper-persistent-data \
           lvm2
$ sudo yum-config-manager \
    --add-repo \
    https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo
$ sudo sed -i 's/download.docker.com/mirrors.ustc.edu.cn\/docker-ce/g' /etc/yum.repos.d/docker-ce.repo
# 官方源
# $ sudo yum-config-manager \
#     --add-repo \
#     https://download.docker.com/linux/centos/docker-ce.repo          
$ sudo yum makecache fast
$ sudo yum install docker-ce
$ sudo systemctl enable docker
$ sudo systemctl start docker
# if you see these words below
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
# then
$ sudo tee -a /etc/sysctl.conf <<-EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
$ sudo sysctl -p
# 一键脚本安装(ubuntu,debian,centos都可以)
$ curl -fsSL get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh --mirror Aliyun
# $ sudo sh get-docker.sh --mirror AzureChinaCloud
# 执行这个命令后，脚本就会自动的将一切准备工作做好，并且把 Docker CE 的稳定(stable)版本安装在系统中。

# 测试是否安装正确
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
d1725b59e92d: Pull complete
Digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
Status: Downloaded newer image for hello-world:latest
Hello from Docker!
This message shows that your installation appears to be working correctly.
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash
Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/
For more examples and ideas, visit:
 https://docs.docker.com/get-started/
 
 # 启动docker
 $ sudo systemctl enable docker
$ sudo systemctl start docker
```

## 10. 宝塔面板安装

```bash
ubuntu
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh
centos
centos 7
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
echo '8881' > /www/server/panel/data/port.pl && /etc/init.d/bt restart
firewall-cmd --permanent --zone=public --add-port=8881/tcp
firewall-cmd --reload
```

## 11. 防爆破	

```bash
# 查看ssh端口
 cat /etc/ssh/sshd_config
# 修改ssh端口
apt install net-tools | yum install net-tools
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/ssh_port.sh && chmod +x ssh_port.sh && bash ssh_port.sh
./ssh_port.sh
./ssh_port.sh end
删除脚本
rm -f ssh_port.sh
# 这个命令仅限使用 2.保守修改 方式并通过 新SSH端口 链接后才能用！
# 或者直接修改
vi /etc/ssh/sshd_config

# centos 7 firewalld
# 开启防火墙
service firewalld start
systemctl enable firewalld #（允许Firewall开机启动，输入两次以确定。）
# 开放特定端口
firewall-cmd --zone=public --add-port=80/tcp --permanent
# 或
firewall-cmd --permanent --add-port=80/tcp
# 关闭特定端口
firewall-cmd --permanent --remove-port=80/tcp
# 每次修改完成后都要执行下面语句：
# 重新载入Firewall 使配置立即生效：
firewall-cmd --reload
# 停止Firewall 防火墙
service firewalld stop
# 重启Firewall 防火墙（每次修改Firewall后都要进行重启，以使得设计生效。）
service firewalld restart
# 查看Firewall 防火墙状态
firewall-cmd --state
# 查看Firewall 防火墙的目前服务状态
systemctl status firewalld
# 查看Firewall 防火墙所有列表规则：
firewall-cmd --list-all

# 安装fail2ban一键脚本
wget https://raw.githubusercontent.com/FunctionClub/Fail2ban/master/fail2ban.sh
bash fail2ban.sh
# 卸载
wget https://raw.githubusercontent.com/FunctionClub/Fail2ban/master/uninstall.sh
bash uninstall.sh
# 查看封禁情况
fail2ban-client status ssh-iptables|head -n 8

# ufw防火墙配置（ubuntu和debian)
sudo apt update && sudo apt install ufw
# UFW 默认允许所哟出站连接，拒绝所有入站连接
sudo ufw default deny incoming
sudo ufw default allow outgoing
# 启动
ufw enable
# 关闭
ufw disable
# 重置
ufw reset
# 状态
ufw status
# 最新版的UFW默认启用了IPV6配置,关闭需要：
vim /etc/default/ufw
IPV6=no
# 设置UFW默认策略
ufw allow ssh
ufw allow http
ufw allow https
ufw allow 22
# 如果你的SSH守护程序配置在其他端口，则需要手动指定相应的端口。
# UFW可以基于TCP或UDP协议来过滤数据包，命令如下：
 ufw allow 80/tcp
 ufw allow 21/udp
# 使用以下命令检查已添加规则的状态
ufw status verbose
# 使用以下命令随时拒绝指定端口任何传入和传出的流量
ufw deny 80
ufw deny 21
# 删除HTTP允许的规则，只需在原始规则前加上delete即可
ufw delete allow http
ufw delete deny 21
# 允许使用端口9000-9002范围内的连接，可以使用以下命令
ufw allow 9000:9002/tcp
ufw allow 9000:9002/udp
# 要允许/禁用来自特定IP地址的连接，如下：
ufw allow from 192.168.29.36
ufw deny from 192.168.29.36

# iptables
# 禁止对外发所有包，别用
iptables -A OUTPUT -p udp -j DROP
# 查看iptables规则
iptables -L INPUT --line-numbers
# 删除指定规则
iptables -D INPUT 7
# 删除所有规则
iptables -F

```
# 12. 修改DNS

1.Centos
编辑网卡配置文件

```bash
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

按i 修改 DNS1=8.8.8.8
按esc 后依次按键 :wq 回车

然后 重启网卡生效

```bash
service network restart
```

2.debian
编辑dns配置文件

按i 修改 DNS1=8.8.8.8

```bash
vi /etc/resolv.conf
```

按i 修改 DNS1=8.8.8.8





