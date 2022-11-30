## 部署nginx与对接acapp

2022/9/5

--------------

### 增加容器的映射端口：80与443

https协议对应443端口，http协议对应80端口

1. 登录容器，关闭所有运行中的任务。

2. 登录运行容器的服务器，然后执行：

   ```shell
   docker commit CONTAINER_NAME django_lesson:1.1  # 将容器保存成镜像，将CONTAINER_NAME替换成容器名称
   docker stop CONTAINER_NAME  # 关闭容器
   docker rm CONTAINER_NAME # 删除容器
   
   # 使用保存的镜像重新创建容器
   docker run -p 20000:22 -p 8000:8000 -p 80:80 -p 443:443 --name CONTAINER_NAME -itd django_lesson:1.1
   ```

   20000是ssh登录端口，8000是调试端口

   ##### 由于King Of Bots项目用了主机的443端口和80端口，所以django容器的端口映射需要改变避免冲突

   ```shell
   docker run -p 20000:22 -p 8000:8000 -p 80:80 -p 443:443 --name CONTAINER_NAME -itd django_lesson:1.1
   ```

3. 去云服务器控制台，在安全组配置中开放80和443端口。

---------------------------

### 创建AcApp，获取域名、nginx配置文件及https证书

https协议一般要绑定域名

##### 打开[AcWing](https://www.acwing.com/file_system/file/content/whole/index/content/whole/application/1/)应用中心，点击“创建应用”的按钮。

如下图所示

##### 在服务器IP一栏填入自己服务器的ip地址。

##### 将nginx.conf中的内容写入服务器/etc/nginx/nginx.conf文件中。如果django项目路径与配置文件中不同，注意修改路径。

##### 将acapp.key中的内容写入服务器/etc/nginx/cert/acapp.key文件中。

##### 将acapp.pem中的内容写入服务器/etc/nginx/cert/acapp.pem文件中。

##### 然后启动nginx服务：`sudo /etc/init.d/nginx start`

-----------

### 修改django项目的配置

##### 打开settings.py文件：

- 将分配的域名添加到ALLOWED_HOSTS列表中。注意只需要添加https://后面的部分。
- 令DEBUG = False。

##### 归档static文件：

- python3 manage.py collectstatic

-----------

现在django外面套了一层nginx

用户通过443端口或者80端口访问nginx，nginx和django之间需要有一个桥梁：**uwsgi**

uwsgi比`python manage runserver`效率更高，并且支持并发，可以开多个进程

### 配置uwsgi

##### 在django项目中添加uwsgi的配置文件：scripts/uwsgi.ini，内容如下：

```shell
[uwsgi]
socket          = 127.0.0.1:8000
chdir           = /home/acs/acapp
wsgi-file       = acapp/wsgi.py
master          = true
processes       = 2 // 最多开几个进程，有几个核
threads         = 5 // 每个核可以开几个线程
vacuum          = true
```

##### 启动uwsgi服务：

`uwsgi --ini scripts/uwsgi.ini`

--------

### 填写应用的剩余信息

- 标题：应用的名称
- 关键字：应用的标签（选填）
- css地址：css文件的地址，例如：https://app76.acapp.acwing.com.cn/static/css/game.css
- js地址：js文件的地址：例如：https://app76.acapp.acwing.com.cn/static/js/dist/game.js、
- 主类名：应用的main class，例如AcGame。
- 图标：4:3的图片
- 应用介绍：介绍应用，支持markdown + latex语法。

提交

------------

### 提交之后发现一些bug

#### 鼠标在AcApp的框里点击时，球没有按照鼠标右键点击的位置方向移动

##### bug产生原因：

监听函数里，获取鼠标右键点击的坐标（移动的目的位置）时，获取的是整个屏幕的绝对坐标，原点在屏幕左上角

但是球的坐标是画布的相对坐标，原点在画布的左上角

也就是说，获取的目的位置信息和player本身的位置信息，两者所处的坐标系是不一样的，bug也就产生了

##### 解决方案：

将监听函数里获取鼠标右键点击的坐标的坐标系映射到画布的坐标系里，统一坐标系

##### 具体实现：

求画布的原点坐标在整个屏幕的坐标系里的坐标是多少

`canvas.getBoundingClientRect()`获取画布在整个屏幕的位置

top表示画布的原点到屏幕上边的距离

left表示画布的原点到屏幕左边的距离

###### 获取到top和left就可以用这两个值获取鼠标右键点击的位置在画布里的坐标

#### 画布在打开应用时就定型了，后续不管怎么拉伸窗口大小都不能将游戏画布扩大

所以画布的初始化应该在加载完playground页面之后再初始化

将初始化的相关代码移动到展示playground页面的代码后面

---------------------

### :star:每次调试需要打包

因为发布版本用的static文件是根目录下的

具体流程：

1. scripts文件夹里有脚本文件compress_game_js.sh，./运行
2. 在根目录下执行`python3 manage.py collectstatic`
3. 查看新版本前最好清空缓存之后硬刷新

1和2已经合二为一，在脚本文件里加入了2

###### 流程简化为在根目录里执行脚本文件即可

----------

### 加入菜单页面

之前为了方便调试，所以隐藏了菜单页面，现将菜单页面放出来（取消注释）

#### 适应AcApp的样式

修改css

----------

