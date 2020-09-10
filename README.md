# Nginx

## 配置说明

案例中在`nginx.conf`文件中相关说明如下:  
配置了两个虚拟主机myServer1和myServer2,前者基于主机名,后者基于IP  
每个虚拟主机里使用不同的location块进行请求处理  
主机myServer2还进行了错误页面的重定向  

## 目录组织结构
