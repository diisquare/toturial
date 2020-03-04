从0开始一个laravel项目请自己查阅

下面以一次从服务器拉下代码为例，介绍怎么在自己的电脑上拉下我们已经写好的laravel代码。

## 基本环境

默认已安装好apache php mysql

查看php拓展 php-m 安装 apt-get install php7.2-xml

- composer 安装见官网 建议换成阿里云镜像
- NodeJs
- 安装npm

## 安装项目

git clone 【项目所在地址】

以github上file-system为例

```shell
git clone https://github.com/diisquare/file-system.git
```

这是你的文件应该长这个样子

```shell
# tree ./ -L 2 -a 
./
└── file-system
    ├── app
    ├── artisan
    ├── bootstrap
    ├── composer.json
    ├── composer.lock
    ├── config
    ├── database
    ├── .editorconfig
    ├── .env.example
    ├── .git
    ├── .gitattributes
    ├── .gitignore
    ├── .idea
    ├── package.json
    ├── package-lock.json
    ├── phpunit.xml
    ├── public
    ├── readme.md
    ├── resources
    ├── routes
    ├── server.php
    ├── storage
    ├── .styleci.yml
    ├── tests
    └── webpack.mix.js

```



这时下载下来的还是我们自己写的代码和一些配置文件，laravel的框架和前端等都没有安装，我们现在来做这些事情。

到在项目目录下

```shell
cd file-system
```

### composer

```shell
composer install 
```

composer 是php的包管理，类似于python的pip。 如我们用的laravel和laravel-admin框架都是在这里安装的。composer 加载的包在 `composer.json` 中。

如何你用的是官方源，这一步应该是比较慢的（建议换源

最后的结果长这样

<img src="http://img.yp51md.club/dii2020-03-04%2011-20-49%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png" alt="composer" style="zoom: 80%;" />

你会发现此时多了一个`/vender`文件夹

### npm

npm 基于NodeJs 的前端的管理

```shell
npm install
npm run dev #开发环境
```

<img src="http://img.yp51md.club/dii2020-03-04%2011-47-53%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png" alt="npm install" style="zoom: 67%;" />

<img src="http://img.yp51md.club/dii2020-03-04%2011-49-10%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png" alt="npm run dev" style="zoom:67%;" />

`npm run dev`通过laravel-mix对下载下来的前端包进行编译。

npm 的安装的包在 `package.json` 里。

你会发现此时多了一个`node_module`的文件夹。

### Mysql 和 Apache

到目前位置我们已经把需要安装的包都装好了，接下来对laravel进行配置。

在mysql中创建一个数据库。如叫 diisquare6.0

复制.example.env => .env 并配置.env文件

```
cp .example.env .env
```

配置大概长这个样子

![.env](http://img.yp51md.club/dii2020-03-04%2015-33-54%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

其中 APP_KEY 由

``` shell
php  artisan key:generate
```

生成，这时`.env`文件中就有APP_KEY的内容了。

![php  artisan key:generate](http://img.yp51md.club/dii2020-03-04%2015-59-12%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

如果数据库配置正确，这条命令可以成功运行，

```shell
php artisan migrate
```

在数据库中创建相应的表格

<img src="http://img.yp51md.club/dii2020-03-04%2016-00-45%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png" alt="php artisan migrate" style="zoom: 80%;" />

运行

```shell
php artisan storage:link
```

在`/public`文件夹中会创造一个外链，为了能通过http请求访问文件。

> [!NOTE]
>
> 你肯定不允许让用户随随便便得看到你的后端代码，所以我们会用 apache 指向`/`文件夹，这样的会其他的文件都不会被访问到，因此你可以发现我们的css和js文件都在/public文件夹下。为了让用户访问`/storage`里的内容只好通过外链的方式了。



后在apache配置指向`/public`的端口号，既可以在浏览器通过ip访问。比如说我是这么配置的。

<img src="http://img.yp51md.club/dii2020-03-04%2015-54-17%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png" style="zoom:67%;" />

> [!DANGER] 
>
> 我的配置仅供参考，请参看附录：AMP的安装和配置 里的官方文档。laravel 提供了 rewrite 的策略来提供 pretty url 。请参看[文档](https://laravel.com/docs/7.x#pretty-urls)。注意这里配置的端口号需要和`.env`中的APP_URL一致。

到此为止，一般的项目就能通过你设置的 ip 地址访问了。但 `file-system` 用了 [laravel-admin](https://www.laravel-admin.org/) 的包。

需要再次运行

```bash
php artisan admin:install
```

初始化这个包

## Trouble shoot

<img src="http://img.yp51md.club/dii2020-03-04%2016-11-35%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png" alt="Permisson deny" style="zoom:67%;" />

```bash
chmod 777 -R ./storage
```

> [!NOTE]
>
> 在这里给777似乎有所不妥，但我给了低权限就不行了，如果你发现了更好的方法，欢迎修改这一条。