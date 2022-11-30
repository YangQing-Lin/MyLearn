## AcApp端AcWing一键登录

2022/9/17

----------------

#### 登入流程

-----------

#### 实现细节

#### 第一步 申请授权码code

请求授权码的API：

`AcWingOS.api.oauth2.authorize(appid, redirect_uri, scope, state, callback);`

参数在通过apply函数获取

##### 参数说明

| 参数         | 是否必须 | 说明                                                         |
| ------------ | -------- | ------------------------------------------------------------ |
| appid        | 是       | 应用的唯一id，可以在AcWing编辑AcApp的界面里看到              |
| redirect_uri | 是       | 接收授权码的地址。需要用对链接进行编码：Python3中使用urllib.parse.quote；Java中使用URLEncoder.encode |
| scope        | 是       | 申请授权的范围。目前只需填userinfo                           |
| state        | 是       | 用于判断请求和回调的一致性，授权成功后后原样返回。该参数可用于防止csrf攻击（跨站请求伪造攻击），建议第三方带上该参数，可设置为简单的随机数（如果是将第三方授权登录绑定到现有账号上，那么推荐用随机数 + user_id作为state的值，可以有效防止CSRF攻击） |
| callback     | 是       | redirect_uri返回后的回调函数                                 |

##### :star:state保证安全

acapp向acwing发送这些参数之后，会将state参数用redis存下来，然后设置一个有效期

acwing在给acapp返回结果的时候，会将state也一并返回，

acapp将其对比redis里存下来的state是否一致，保证安全，因为回调函数（`receiveCode`）的Url是公开的

##### 返回说明

用户同意授权后，Acwing会将code和state传递给redirect_uri

如果用户拒绝授权，则将会收到如下错误码：

```java
{
    errcode: "40010"
    errmsg: "user reject"
}
```

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

-----------

#### 实现相关函数

修改web端acwing一键登录的代码，基本类似

申请授权码是使用了AcWingOS的api

##### :star:如何让用户总是打开了新版本呢

在修改应用的界面更新js的版本号（路径），同时前端的文件名要对应