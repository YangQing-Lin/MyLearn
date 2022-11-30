## web端AcWing一键登录

2022/9/16

--------------

#### 在Django中集成Redis

目前项目使用的数据库是Django自带的数据库：sqlite，如果未来想要换成mysql很容易

##### 数据库的存储效率是不如redis的

redis就是内存数据库，读写数据很快

redis的特点：

- 存储的是 key : value 对
- **单线程**，不会出现读写冲突

##### 安装django_redis

`pip install django_redis`

##### 配置settings.py

```python
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        },
    },
}
USER_AGENTS_CACHE = 'default'
```

#####  启动redis-server

`sudo redis-server /etc/redis/redis.conf`

#### 如何使用redis

django的后台可以直接操作redis

`python3 manage.py shell`，进入后台

```python
from django.core.cache import cache

cache.keys('*') # 查找redis当前里有哪些数据，支持正则表达式

cache.set('dl', '1', 5) # 设置键值对，过期时间（单位是秒），如果想要设置一个不会过期的键值对，第三个参数写成None

cache.has_key('dl') # 查找某个key是否存在

cache.get('dl') # 查找某个key的value

cache.delete('dl') # 删除某个键值对
```

--------

#### Web端AcWing一键登录的流程

##### Oauth2授权模式

/learn/img/django/web端AcWing一键登录

----------

#### 具体实现细节

#### 第一步 申请授权码code

**请求地址：**https://www.acwing.com/third_party/api/oauth2/web/authorize/

##### 参考示例：

请求方法：GET
https://www.acwing.com/third_party/api/oauth2/web/authorize/?appid=APPID&redirect_uri=REDIRECT_URI&scope=SCOPE&state=STATE

##### 参数说明

| 参数         | 是否必须 | 说明                                                         |
| ------------ | -------- | ------------------------------------------------------------ |
| appid        | 是       | 应用的唯一id，可以在AcWing编辑AcApp的界面里看到              |
| redirect_uri | 是       | 接收授权码的地址。需要用对链接进行编码：Python3中使用urllib.parse.quote；Java中使用URLEncoder.encode |
| scope        | 是       | 申请授权的范围。目前只需填userinfo                           |
| state        | 是       | 用于判断请求和回调的一致性，授权成功后后原样返回。该参数可用于防止csrf攻击（跨站请求伪造攻击），建议第三方带上该参数，可设置为简单的随机数（如果是将第三方授权登录绑定到现有账号上，那么推荐用随机数 + user_id作为state的值，可以有效防止CSRF攻击） |

##### :star:state保证安全

acapp向acwing发送这些参数之后，会将state参数用redis存下来，然后设置一个有效期

acwing在给acapp返回结果的时候，会将state也一并返回，

acapp将其对比redis里存下来的state是否一致，保证安全，因为回调函数的Url是公开的

##### 返回说明

用户同意授权后会**重定向到redirect_uri**，返回参数为code和state。链接格式如下：

```java
redirect_uri?code=CODE&state=STATE
```

如果用户拒绝授权，则不会发生重定向。

#### 第二步 申请授权令牌access_token和用户的openid

**请求地址**：https://www.acwing.com/third_party/api/oauth2/access_token/

参考示例：

请求方法：GET
https://www.acwing.com/third_party/api/oauth2/access_token/?appid=APPID&secret=APPSECRET&code=CODE

##### 参数说明

| 参数   | 是否必须 | 说明                                            |
| ------ | -------- | ----------------------------------------------- |
| appid  | 是       | 应用的唯一id，可以在AcWing编辑AcApp的界面里看到 |
| secret | 是       | 应用的秘钥，可以在AcWing编辑AcApp的界面里看到   |
| code   | 是       | 第一步中获取的授权码                            |

##### 返回说明

申请成功示例：

```java
{ 
    "access_token": "ACCESS_TOKEN", 
    "expires_in": 7200, 
    "refresh_token": "REFRESH_TOKEN",
    "openid": "OPENID", 
    "scope": "SCOPE",
}
```

申请失败示例：

```java
{
    "errcode": 40001,
    "errmsg": "code expired",  # 授权码过期
}
```

##### 返回参数说明

| 参数          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| access_token  | 授权令牌，有效期2小时                                        |
| expires_in    | 授权令牌还有多久过期，单位（秒）                             |
| refresh_token | 用于刷新access_token的令牌，有效期30天                       |
| openid        | 用户的id。每个AcWing用户在每个acapp中授权的openid是唯一的,可用于识别用户。 |
| scope         | 用户授权的范围。目前范围为userinfo，包括用户名、头像         |

##### 刷新access_token的有效期

access_token的有效期为2小时，时间较短。refresh_token的有效期为30天，可用于刷新access_token

刷新结果有两种：

1. 如果access_token已过期，则生成一个新的access_token。
2. 如果access_token未过期，则将当前的access_token的有效期延长为2小时。

##### 参考示例：

请求方法：GET
https://www.acwing.com/third_party/api/oauth2/refresh_token/?appid=APPID&refresh_token=REFRESH_TOKEN

返回结果的格式与申请access_token相同。

#### 第三步 申请用户信息

**请求地址**：https://www.acwing.com/third_party/api/meta/identity/getinfo/

参考示例：

请求方法：GET
https://www.acwing.com/third_party/api/meta/identity/getinfo/?access_token=ACCESS_TOKEN&openid=OPENID

##### 参数说明

| 参数         | 是否必须 | 说明                     |
| ------------ | -------- | ------------------------ |
| access_token | 是       | 第二步中获取的授权令牌   |
| openid       | 是       | 第二步中获取的用户openid |

##### 返回说明

申请成功示例：

```java
{
    'username': "USERNAME",
    'photo': "https:cdn.acwing.com/xxxxx"
}
```

申请失败示例：

```java
{
    'errcode': "40004",
    'errmsg': "access_token expired"  # 授权令牌过期
}
```

--------------------------------------------

### 实现后端

##### 丰富数据库的player表

增加openid信息，在Model里的player类增加

改完player.py之后更新数据库：

- `python3 manage.py makemigrations`
- `python3 manage.py migrate`
- 重启服务器

##### 实现函数：`apply_code`

在接收到用户想要acwing一键登录的请求之后，**该函数会返回申请授权码的请求链接**

views：`apply_code.py`，将参数手动填进申请授权码的参数

url：/settings/acwing/web/apply_code/

js

- 将登录注册界面的AcWing一键登录的按钮拿出来
- 向后端apply_code获取申请授权码的链接，并重定向到此链接，该链接会返回code和state，由receive_code函数接收

##### :star:每一次创建一个文件夹都要创建一个文件：__init\_\_.py，不然python import的时候就找不到了

##### 实现函数：`receive_code`

- 接收授权码和state
- 通过code向acwing申请access_token和openid
- 通过access_token和openid，向acwing获取用户信息
- 拿到用户信息，为该用户注册一个账号并登录

views：`receive_code`

url：/settings/acwing/web/receive_code/