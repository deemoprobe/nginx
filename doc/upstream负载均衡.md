# nginx配置负载均衡

```shell
在nginx.conf文件里的http字段里面配置

upstream name{
    # weight权值越大优先级越高,默认为1,可以不用写
    server ip:port weight 5;
    server ip:port;
}

server {
    listen port;
    server_name localhost;    # 如果未生效，直接填本机的IP地址
    location /api/api/api   # 填写实际转发的路径
    {
        # 关闭重定向location
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-Ip $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://name;
    }
}

测试转发是否成功：在浏览器输入ip:port访问   或者telnet ip port  测试是否连通
