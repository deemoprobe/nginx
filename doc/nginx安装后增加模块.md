# nginx安装后需要增加新模块

## 查看之前安装的nginx配置信息

```shell
cd /app/nginx/sbin
./nginx -V
nginx version: nginx/1.18.0
built by ...
configure argument: --prefix=...
```

注意`configure argument`字段,如果是后期增加编译参数(或模块),原有的编译参数也记得带上

## 增加模块

```shell
# 假设新增的模块包名为test_module.tar.gz,先解压
tar -zxvf test_module.tar.gz
# 假设放在/app/install/nginx下
cd /app/install/nginx
tar -zxvf nginx-1.18.0.tar.gz
cd nginx-1.18.0
./configure --prefix=...(原来的参数) --add-module=/app/install/nginx/test_module
make
# 不要make install, make生成可执行文件即可
# 编译完成(make后),会在源码目录生成objs目录
cd objs/
cp /app/nginx/sbin/nginx /app/nginx/sbin/nginx.bak
cp nginx /app/nginx/sbin/

/app/nginx/sbin/nginx -V
nginx version: nginx/1.18.0
built by ...
configure argument: --prefix=... --add-module=/app/install/nginx/test_module
```

## 增加参数

```shell
# 以增加stream参数为例
cd nginx-1.18.0
./configure --prefix=...(原来的参数) --with-stream
make
# 不要make install, make生成可执行文件即可
# 编译完成(make后),会在源码目录生成objs目录
cd objs/
cp /app/nginx/sbin/nginx /app/nginx/sbin/nginx.bak
cp nginx /app/nginx/sbin/

/app/nginx/sbin/nginx -V
nginx version: nginx/1.18.0
built by ...
configure argument: --prefix=... --with-stream
```
