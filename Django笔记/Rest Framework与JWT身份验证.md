## Rest Framework与JWT身份验证

2022/9/24

-------------

#### 处理跨域之后的身份验证问题

之后前后端彻底分离

http请求大致分为四种：

- get，查
- post，增
- delete，删
- put，改

主要是get和post：

- get，将参数传到链接里面，可传参数较短，且是明文不安全
- post，将参数传到body里，可传参数较长，且安全

--------

#### django自带的登录验证逻辑

client将个人信息发送给server，server验证通过之后，server就会向client端发送一个session_id，发送session_id之前，会将此存到数据库里（每一个session_id对于的用户，以及什么时候创建的，什么时候过期），client接收到session_id之后，**将其存到client浏览器的cookie**（这种cookie一般存到http-only）里

未来每一次client在向server发送请求的时候，都会默认将session_id带上，server接收到session_id之后，会在数据库里查一下：该session_id是否存在，以及有没有过期，如果都不满足，表示该请求是登陆状态下的请求，而且也可以在数据库里查出来该session_id对应的用户是谁

这种方式对http和websocket都成立

这种方式的验证信息是在cookie里，**js是不能直接读取http-only的cookie**，js在访问server的时候（ajax）无法带上cookie里的session_id，所以当跨域访问的时候，**不能做身份验证**

所以当我们想要比较方便的实现跨域的话，可以将验证信息换一个地方存储

-------------

#### Json Web Token

client在获取登录钥匙的时候，通过用户信息向server发送请求，server收到请求并验证通过之后，会向client端返回一个信息：**JWT**，JWT里一般包含：**user_id、加密之后的字符串和有效期**，client在得到JWT之后可以将其存到内存里或者存到**浏览器的存储空间localstorage里**，这个**存储空间js是可以随便访问的**

未来client再向server发送请求的时候，**每次都会用js带上JWT**，server在接收到JWT的时候每次都会验证是否合法，如果是合法的，server就会读取里面的user_id，可以得知该请求是哪个用户发送的

加密：将带有user_id和有效期的字符串，使用某种加密算法，加密成一个新的串

根据原串算出加密之后的值非常容易，但是加密之后的字符串算出原串几乎不可能在有限时间内算出来

利用**加密不可逆**的特性，在带有info的字符串加密之前，给info加上一小段随机字符串（私钥），每次加密的时候，info和secret-key配合一起加密，然后**将得到的加密之后的结果和info一块返回给client**，私钥不会传

这样，每次client发送请求时将JWT返回给server之后，server只需要将JWT里的info和私钥一起做一下加密，判断一下结果是不是和JWT相符

##### JWT包含两个令牌：

- access，有效期一般是5分钟

  因为有些get方法也是需要验证的，access用来给get方法做验证，get方法验证的时候参数都会写在链接里面，所以access令牌会在链接里面，不太安全

- refresh，当access过期之后，可以通过refresh刷新access的令牌，有效期一般是14天

  请求的时候用的是post方法，参数放在请求的body里面，相对来说更安全

退出登录：client将浏览器里localstorage的jwt删掉

---------

#### 集成 Django Rest Framework

##### 安装

```shell
pip install djangorestframework
pip install pyjwt
```

:star:安装完app之后需要将安装的app的static同步过来：`python3 manage.py collectstatic`

然后在 settings.py的INSTALLED_APPS中添加`rest_framework`。

##### Class-Based Views

```python
from rest_framework.views import APIView
from rest_framework.response import Response

class SnippetDetail(APIView):
    def get(self, request):  # 查找
        ...
        return Response(...)

    def post(self, request):  # 创建
        ...
        return Response(...)

    def put(self, request, pk):  # 修改
        ...
        return Response(...)

    def delete(self, request, pk):  # 删除
        ...
        return Response(...)
```

#### 集成JWT验证

##### 安装

`pip install djangorestframework-simplejwt`

然后在settings.py中添加：

```python
INSTALLED_APPS = [
    ...
    'rest_framework_simplejwt',
    ...
]

REST_FRAMEWORK = { # 验证方式变成jwt
    ...
    'DEFAULT_AUTHENTICATION_CLASSES': (
        ...
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
    ...
}
```

##### 配置

在settings.py中添加：

```python
from datetime import timedelta
...

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=5), # access的有效期
    'REFRESH_TOKEN_LIFETIME': timedelta(days=14),  # refresh的有效期
    'ROTATE_REFRESH_TOKENS': False,
    'BLACKLIST_AFTER_ROTATION': False,
    'UPDATE_LAST_LOGIN': False,

    'ALGORITHM': 'HS256',
    'SIGNING_KEY': SECRET_KEY, # 换成随机字符串
    'VERIFYING_KEY': None,
    'AUDIENCE': None,
    'ISSUER': None,
    'JWK_URL': None,
    'LEEWAY': 0,

    'AUTH_HEADER_TYPES': ('Bearer',),
    'AUTH_HEADER_NAME': 'HTTP_AUTHORIZATION',
    'USER_ID_FIELD': 'id',
    'USER_ID_CLAIM': 'user_id',
    'USER_AUTHENTICATION_RULE': 'rest_framework_simplejwt.authentication.default_user_authentication_rule',

    'AUTH_TOKEN_CLASSES': ('rest_framework_simplejwt.tokens.AccessToken',),
    'TOKEN_TYPE_CLAIM': 'token_type',
    'TOKEN_USER_CLASS': 'rest_framework_simplejwt.models.TokenUser',

    'JTI_CLAIM': 'jti',

    'SLIDING_TOKEN_REFRESH_EXP_CLAIM': 'refresh_exp',
    'SLIDING_TOKEN_LIFETIME': timedelta(minutes=5),
    'SLIDING_TOKEN_REFRESH_LIFETIME': timedelta(days=1),
}
```

##### 添加获取jwt和刷新jwt的路由

之前的登录验证api不需要了，jwt的验证只需要以下

```python
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
)

urlpatterns = [
    ...
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
    ...
]
```

##### 手动获取jwt

```python
from rest_framework_simplejwt.tokens import RefreshToken

def get_tokens_for_user(user):
    refresh = RefreshToken.for_user(user)

    return {
        'refresh': str(refresh),
        'access': str(refresh.access_token),
    }
```

##### 将jwt验证集成到Django Channels中

创建文件channelsmiddleware.py

```python
"""General web socket middlewares
"""

from channels.db import database_sync_to_async
from django.contrib.auth import get_user_model
from django.contrib.auth.models import AnonymousUser
from rest_framework_simplejwt.exceptions import InvalidToken, TokenError
from rest_framework_simplejwt.tokens import UntypedToken
from rest_framework_simplejwt.authentication import JWTTokenUserAuthentication
from channels.middleware import BaseMiddleware
from channels.auth import AuthMiddlewareStack
from django.db import close_old_connections
from urllib.parse import parse_qs
from jwt import decode as jwt_decode
from django.conf import settings
@database_sync_to_async
def get_user(validated_token):
    try:
        user = get_user_model().objects.get(id=validated_token["user_id"])
        # return get_user_model().objects.get(id=toke_id)
        return user

    except:
        return AnonymousUser()



class JwtAuthMiddleware(BaseMiddleware):
    def __init__(self, inner):
        self.inner = inner

    async def __call__(self, scope, receive, send):
       # Close old database connections to prevent usage of timed out connections
        close_old_connections()

        # Try to authenticate the user
        try:
            # Get the token
            token = parse_qs(scope["query_string"].decode("utf8"))["token"][0]

            # This will automatically validate the token and raise an error if token is invalid
            UntypedToken(token)
        except:
            # Token is invalid

            scope["user"] = AnonymousUser()
        else:
            #  Then token is valid, decode it
            decoded_data = jwt_decode(token, settings.SIMPLE_JWT["SIGNING_KEY"], algorithms=["HS256"])

            # Get the user using ID
            scope["user"] = await get_user(validated_token=decoded_data)
        return await super().__call__(scope, receive, send)


def JwtAuthMiddlewareStack(inner):
    return JwtAuthMiddleware(AuthMiddlewareStack(inner))
```

#### 配置Nginx

打开/etc/nginx/nginx.conf，在location /的路由中添加对PUT、POST、DELETE方法的支持。完整配置见acwing【创建应用】界面。

```shell
if ($request_method = 'POST') {
    add_header 'Access-Control-Allow-Origin' 'https://www.acwing.com';
    add_header 'Access-Control-Allow-Methods' 'GET, PUT, OPTIONS, POST, DELETE';
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization,X-Amz-Date';
    add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
}
if ($request_method = 'DELETE') {
    add_header 'Access-Control-Allow-Origin' 'https://www.acwing.com';
    add_header 'Access-Control-Allow-Methods' 'GET, PUT, OPTIONS, POST, DELETE';
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization,X-Amz-Date';
    add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
}
```

修改完后，执行：`sudo nginx -s reload`，使修改生效。

------------------------

#### 修改getinfo.py

转换成restFramework配合上jwt的写法

#### 修改前端配合

每次用户发送请求，该请求如果需要身份验证的话，需要做一个判断：

- 如果refresh过期了，用户需要重新登录
- 如果refresh没有过期，access过期了，就需要通过refresh重新获取一个新的access，如果access没有过期，直接请求即可

判断过期：一种方式是时间判断，存储jwt（access和refresh）时，将创建时间也存下来

##### 简单的方式：

每次刷新之后都要重新登录，然后将access和refresh存到内存里面，设计一个周期函数，每隔4.5分钟就刷新一遍access

每次打开界面，判断是否有access，如果没有就需要登录，如果有就直接获取用户信息

##### 远程登录`login_on_remote`

访问token/，获取JWT-token，获取成功之后将其存到内存

获取失败的话，返回错误信息

##### 获取用户信息getinfo_web

通过获取到的token向后端发送请求，获得用户信息

##### 退出

将内存里的token删掉

##### 修改第三方登录

将之前的登陆方式删掉

手动构造一个jwt返回即可

##### access自动刷新

refresh_jwt_token，每4.5分钟刷新一次

-----------

#### 为排行榜页面（假设有）设置身份验证

**需要身份验证的api加上**：`permission_classes = ([IsAuthenticated])`

views

urls，加上as_view()，可以将引入的类变成函数形式

js自己实现，调试阶段为：登录成功之后5秒内在浏览器的控制台自动返回排行榜

------------

#### 修改注册

vews，改成class类型

urls，as_view()

js，注册成功，直接使用username和password登录

-----------------

#### 修改AcApp端的登录验证

向前端返回token，/acwing/acapp/receive_code.py

前端获取成功之后，调用刷新函数

------------

#### websocket协议验证

只让有令牌的用户建立连接

##### 在django_channels里面集成jwt验证

修改验证方式，asgi.py

建立连接的时候，判断用户是否验证通过，如果不通过就关闭，如果通过就创建连接

js建立ws连接的时候需要将token传进去