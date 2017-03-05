---
title: Apache配置多个虚拟主机 (Windows)
date: 2016-04-22 16:04:56
tags: [Apache,VirtualHost]
---

### Windows下为Apache配置多个虚拟主机

##### 修改Apache配置文件(安装目录下conf/httpd.conf)
1. 添加将要监听的端口号
```bash
# Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, instead of the default. See also the <VirtualHost>
# directive.
#
# Change this to Listen on specific IP addresses as shown below to 
# prevent Apache from glomming onto all bound IP addresses.
#
#Listen 12.34.56.78:80
Listen 80
Listen 8080
```
2. 设置虚拟主机的属性
```bash
<VirtualHost *:8080>
    ServerAdmin ericjiang
    #网站文件存放的根目录
    DocumentRoot E:/VirtualHost/Client
    #域名
    ServerName http://www.phpcms.com/
    ErrorLog logs/test-error.log
    CustomLog logs/test-access.log common
</VirtualHost>
```
