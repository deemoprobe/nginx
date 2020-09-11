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
* 主机1: 192.168.245.129 www.server1.com
* 主机2: 192.168.245.131 www.server2.com

### 安装nginx

```shell
# 在安装之前需要安装依赖包
yum install -y gcc gcc-c++ automake pcre pcre-devel zlib zlib-devel open openssl-devel
# 然后从官网下载相关安装包解压编译即可
```

### host配置

主机0的host配置  
添加www.server1.com,由于主机3是配置基于IP访问,所以不需要添加进去

```shell
vi /etc/hosts
192.168.245.129     www.server1.com
```

主机1的host配置

```shell
vi /etc/hosts
192.168.245.129     www.server1.com
```

注:如果是Windows主机,编辑`C:\Windows\System32\drivers\etc`下对应的hosts文件即可

在访问时`curl 192.168.245.131:8001/server/location1/`出现错误  
错误: `curl: (7) Failed connect to 192.168.245.131:8001; No route to host`  
问题分析: 没有到server2的路由,排查发现防火墙策略没开  
解决方案: 开放nginx端口  

```shell
systemctl start firewalld
firewall-cmd --zone=public --add-port=8001/tcp --permanent
firewall-cmd --reload
```

## 目录组织结构

1. www.server1.com(192.168.245.129)的目录结构

    ```shell
    /myweb
    ├── 404.html
    └── server
        ├── location1
        │   └── index_loc1.html
        ├── location2
        │   └── index_loc2.html
        └── log
            └── access_log
    ```

2. www.server2.com(192.168.245.131)的目录结构

    ```shell
    /myhtml
    ├── 404.html
    ├── nginx.pid // 配置文件中改了pid的输出目录
    └── server
        ├── location1
        │   └── index_loc1.html
        ├── location2
        │   └── index_loc2.html
        └── log
            └── access_log
    ```

## 测试访问

1. 访问`www.server1.com:8001/server/location1/`

    ```shell
    $ curl www.server1.com:8001/server/location1/
    <h1> This is index_svr1_loc1.html </h1>
    ```

    ![20200911113636](https://deemoprobe.oss-cn-shanghai.aliyuncs.com/images/20200911113636.png)

2. 访问`www.server1.com:8001/server/location2/`

    ```shell
    $ curl www.server1.com:8001/server/location2/
    <h1> This is index_svr1_loc2.html </h1>
    ```

    ![20200911113727](https://deemoprobe.oss-cn-shanghai.aliyuncs.com/images/20200911113727.png)

3. 访问`www.server1.com:8001/server/location1`

    ```shell
    $ curl www.server1.com:8001/server/location1
    <html>
    <head><title>301 Moved Permanently</title></head>
    <body>
    <center><h1>301 Moved Permanently</h1></center>
    <hr><center>nginx/1.18.0</center>
    </body>
    </html>
    # 这里发现想要找到location1位置的文件,文根必须加上/
    # 注：部分浏览器可以访问是因为浏览器策略会自动寻找文件夹下的文件,但是为了保证100%没问题,文根还是要加的
    ```

4. 访问`www.server1.com:8001/server/location3/`

    ```shell
    // 使用的是nginx默认的404页面
    $ curl www.server1.com:8001/server/location3/
    <html>
    <head><title>404 Not Found</title></head>
    <body>
    <center><h1>404 Not Found</h1></center>
    <hr><center>nginx/1.18.0</center>
    </body>
    </html>
    ```

    ![20200911114039](https://deemoprobe.oss-cn-shanghai.aliyuncs.com/images/20200911114039.png)

5. 访问`192.168.245.131:8001/server/location1/`

    ```shell
    $ curl 192.168.245.131:8001/server/location1/
    <h1> This is server2 location1 </h1>
    ```

    ![20200911114119](https://deemoprobe.oss-cn-shanghai.aliyuncs.com/images/20200911114119.png)

6. 访问`192.168.245.131:8001/server/location2/`

    ```shell
    // 使用的是自定义的404页面
    $ curl 192.168.245.131:8001/server/location2/
    <h1> 404. File Not Found. </h1>
    ```

    ![20200911114144](https://deemoprobe.oss-cn-shanghai.aliyuncs.com/images/20200911114144.png)

7. 访问`192.168.245.131:8001/svr/loc2/`

    ```shell
    $ curl 192.168.245.131:8001/svr/loc2/
    <h1> This is server2 location2!</h1>
    ```

    ![20200911114304](https://deemoprobe.oss-cn-shanghai.aliyuncs.com/images/20200911114304.png)
