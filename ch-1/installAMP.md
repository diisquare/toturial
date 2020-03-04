# 安装AMP

amp即Apache+mysql+php

在Windows上的用户可以用[PHPstudy](https://www.xp.cn/)

### 一次经历

下面记录一下我在腾讯云上安装的经过 ubuntu18.04上

主要就一切从简，没有自己安装包来编译又都是用apt安装的

```bash
sudo apt-get install apache2
sudo apt-get install php7 
```

可以通过whereis apache 找配置文件

一般在 /etc 或  /usr/etc 下面

一些文档 比较久远了

https://help.ubuntu.com/lts/serverguide/httpd.html

https://help.ubuntu.com/community/ApacheMySQLPHP 

#### apache

apt 下载下来的apache不是一个完整的配置文件 httpd.conf

而是分在了一系列文件里。

- mysite.conf
- 如果遇到了php无法解析的问题 在mime.conf里addtype

### php

php应该在7.2版本以上，目前的laravel已经到了最低7.2

暂时应该不用配置

#### mysql

我们安装的mysql是5.7的版本

```shell
sudo apt install mysql-server libapache2-mod-auth-mysql php-mysql
```

请遵循mysql官网的doc，已经讲的很好了。