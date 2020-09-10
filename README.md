# Nginx

本案例在虚拟机中的CentOS7中进行

## 配置

### 说明

案例中在`nginx.conf`文件中相关说明如下:  
配置了两个主机www.server1.com和www.server2.com,前者基于主机名,后者基于IP  
每个虚拟主机里使用不同的location块进行请求处理  
主机myServer2还进行了错误页面的重定向  

模拟生产场景,准备三台虚拟主机,主机IP地址如下:  

* 主机0: 192.168.245.127 模拟进行页面访问
* 主机1: 192.168.245.128 www.server1.com
* 主机2: 192.168.245.129 www.server2.com

### host配置

主机0的host配置  
添加www.server1.com,由于主机3是配置基于IP访问,所以不需要添加进去

```shell
vi /etc/hosts
192.168.245.128     www.server1.com
```

主机1的host配置

```shell
vi /etc/hosts
192.168.245.128     www.server1.com
```

注:如果是Windows主机,编辑`C:\Windows\System32\drivers\etc`下对应的hosts文件即可

## 目录组织结构

三台虚拟机，两台装上nginx，创建相关文件，修改配置文件

记录：目录结构，访问结果截图（截图包括本机IP和输出结果）
