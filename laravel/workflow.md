从两个视角来讲述一下 `laravel` 是如何跑起来的。



## 用户视角

以在本地点开 forum 为例



网址里的url是 http://127.0.0.1/post

这是因为我们写了一次重定向 在文件`/routes/web.php` 中



![](http://img.yp51md.club/2020-07-17 15-44-59屏幕截图.png)



服务器接受到 `/` 的get请求，先重定向到了 `/post` 方法，然后调用了 `PostController` 的 `index` 方法, 

> [!NOTE]
>
> 所有的 web 请求会先到 routes(路由) 中寻找（如果没有找到就会返回404），然后再调用相应的 `Controller` 方法。

控制器统一放在`/app/Http/Controllers` 中，比如以 `PostController` 的 `index` 方法为例，当 routes 中调用 `index` 方法后：

<img src="http://img.yp51md.club/2020-07-17 15-57-36屏幕截图.png" style="zoom: 80%;" />

会在 `Manager` 中实例化 `FileManager` 对象，这个对象可以给出文件夹中文件的内容，文件的路径，文件的下载地址。

>[!NOTE]
>
>我们只会在 `Controller` 中写最浅层的处理代码，业务逻辑的实现会在 `Manager` 中实现，即我们又进行了一层抽象，这样的好处是 `FileManager` 中的代码可以复用，符合DRY 原则。

我们将处理好的数据通过 laravel 自己的 view 函数，发送给前端进行渲染，最后一起 return 给用户。



view 函数将数据发送到 `resources/views/`中的相应文件，laravel 中的前端文件同样符合 `DRY原则`，像积木一样把一块块拼在一起。

<img src="http://img.yp51md.club/2020-07-17 16-19-04屏幕截图.png" style="zoom:80%;" />

在前端可以用到后端给的数据。

<img src="http://img.yp51md.club/2020-07-17 16-22-57屏幕截图.png" style="zoom:80%;" />

这样一次请求就完成了，其他类型的请求都是大同小异。

[MCV](https://baike.baidu.com/item/MVC框架/9241230) 的设计模式还是很妙的是吧。



## 开发者视角

上述例子没有使用到数据库和模型（Model），下面就简述一下如何写一段应用。

在设计好数据库后，就可以通过 `migrate` 表迁移建立数据库，然后在 model 中建立对应的模型，然后在 Manager 中为模型建立对应的方法之后，在 controller 和 routes 中注册就写好了！

