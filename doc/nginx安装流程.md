# nginx安装流程

## 准备工作

## 安全加固

```shell
http{
    # 隐藏版本信息
    server_tokens   off;
    ...
    #keepalive_timeout 65;
    # 配置timeout参数
    client_body_timeout 30;
    client_header_timeout 30;
    keepalive_timeout 30 30;
    send_timeout 30;
    ...
    server{
        listen 8080;
        server_name localhost;
        # 设置自定义缓存参数
        client_body_buffer_size 128k;
        client_header_buffer_size 1k;
        client_max_body_size 300m;
        large_client_header_buffers 4 16k;
        ...
    }
}
```
