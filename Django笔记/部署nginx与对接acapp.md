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