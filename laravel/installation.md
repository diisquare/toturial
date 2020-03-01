从0开始一个laravel项目请自己查阅

下面以一次从服务器拉下代码为例，介绍怎么在自己的电脑上拉下我们已经写好的laravel代码。

### 基本环境

默认已安装好apache php mysql

查看php拓展 php-m 安装apt-get install php7.2-xml

- composer 安装见官网
- 安装npm

git clone 【项目所在地址】

在项目目录下

```shell
composer install
npm install
npm run prod
php  artisan key:generate
```

在mysql中创建一个数据库

复制.example.env => .env 并配置.env文件

``` shell
php artisan migrate
```

如果数据库配置正确，这条命令可以运行



后在apache配置指向/public的端口号，既可以在浏览器通过ip访问