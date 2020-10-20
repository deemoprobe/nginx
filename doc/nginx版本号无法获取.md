# nginx版本号无法写入文件或获取为文本

## 例如

```shell
# 无法过滤nginx -V输出信息
nginx -V | grep "version"
```

## 原因

```shell
nginx -V输出的内容是在标准的错误输出流中, 重定向到标准输出流就可以解决
```

## 解决方案

```shell
nginx -V 2>&1 | grep "version"
nginx version: nginx/1.18.0
```
