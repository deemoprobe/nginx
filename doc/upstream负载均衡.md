# nginx配置负载均衡

```shell
在nginx.conf文件里的http字段里面配置

upstream name{
    server ip:port;
}

server {
    listen port;
    server_name localhost;    # 如果未生效，直接填本机的IP地址
    location /api/api/api   # 填写实际转发的路径
    {
        proxy_set_header Host $host;
        #proxy_set_header X-Real-Ip $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://name/;  # 如果不行就把完整路径都配置上 proxy_pass http://name/api/api/api;
    }
}


测试转发是否成功：在浏览器输入ip:port访问   或者telnet ip port  测试是否连通

SEE ALSO:
Nginx配置请求转发location及rewrite规则  https://www.cnblogs.com/kanyun/p/7466309.html
Nginx配置端口转发-windows  https://www.cnblogs.com/acelance/p/10396821.html
```
