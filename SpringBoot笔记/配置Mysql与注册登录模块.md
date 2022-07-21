#### SpringBoot 配置Mysql与注册登录模块

2022/7/21

------------------------

##### 应用框架结构

​	SpringBoot 就是一个一直在跑的程序，与 client 端通信，等待 client 端发送请求，然后处理请求，返回数据

​	Mysql 同样是个独立的在跑的程序，等待接受请求，并处理请求，返回数据

​	SpringBoot 和 Mysql 之间是相互交互的关系

###### 	这里以 client 发送登录请求为例

当 client 端向 SpringBoot 发送登录请求（同时传有 username，password 等信息），SpringBoot 在接收到这个请求之后，会向 Mysql 发送请求（请求查询 username 下的所有信息），Mysql 会返回给 SpringBoot 这个 client 的各种信息（username 和 password 等信息），SpringBoot 在接收到 Mysql 返回的信息之后，会将 Mysql 返回的信息与 client 端发送的信息进行比对，以此来判断是否允许 client 端登录成功

Mysql 在交互的时候，会将数据存到硬盘上，部分数据也会存到内存里

----------------------

##### Mysql 的结构

- 包含多个数据库，db1, db2, kob ...

  每次操作的时候需要使用其中一个数据库

  - 每个数据库里有多个表
    - 每个表里存储多组数据

操作 Mysql 最简单的方式是命令行

-----------

SpringBoot 会根据用户的不同请求（注册，登录等），向 Mysql 发送不同的请求，Mysql 会根据不同的请求，执行相应的语句（insert, select 等），然后反馈给 SpringBoot ，SpringBoot 再根据结果，处理用户的请求

-----------------------------------------------------------------

##### 如何让 SpringBoot 操作 Mysql

[Maven仓库地址](https://mvnrepository.com/) 

[Mybatis-Plus官网](https://baomidou.com/)

###### 在 IDEA 里配置 SpringBoot，在 pom.xml 文件中添加依赖

在 Maven 仓库里找对应的配置加入：

- Spring Boot Starter JDBC
  - 提供了数据源配置、事务管理、数据访问等等功能
- Project Lombok
  - 简化代码
- MySQL Connector/J
  - 实现了JDBC，为使用java开发的程序提供连接，方便java程序操作数据库。
- mybatis-plus-boot-starter
  - 默认写好很多 sql 语句
- mybatis-plus-generator
  - 自动生成函数

SpringBoot 想要访问 Mysql 同样需要用户名密码来访问，即 root 用户以及 root 用户的 password，需要配置

```java
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/kob?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf-8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

--------------------------

##### SpringBoot 中层的概念（常用模块）

- pojo 层：将数据库中的表对应成 Java 中的 Class
- mapper 层（也叫 Dao 层）：将 pojo 层的 class 中的操作（crud），映射成 sql 语句
- service 层：写具体的业务逻辑，组合使用 mapper 中的操作，实现业务
- controller 层：负责请求转发，接受页面过来的参数，传给 Service 处理，接到返回值，再传给页面

----------------------------

##### 实现 pojo 层

在 backend 目录下创建软件包 pojo，在包里为每张表创建对应的类

pojo 里的一个对象对应数据库里的一行数据

```java
// 自动实现一些机械化方法
@Data // 填充常用函数
@NoArgsConstructor // 填充无参构造函数
@AllArgsConstructor // 填充所有参构造函数
```

------------------------------------

##### 实现 mapper 层

通过依赖：mybatis-plus，即可不用手写 sql 语句

直接调用 API ，即可实现 sql 语句

-----------------

##### 调试 service 和 controller 层

因为调试逻辑简单，所以可以放到一起调试

根据路由找到对应的函数，调用函数返回数据，同时可以取路由里的参数

-------------------------

以上，Mysql 就集成到项目中了

##### 应用框架结构

​	在 client 端操作的时候，先向 SpringBoot 发送一个请求（相当于是打开一个 url），**SpringBoot 会根据这个请求（url）找到应该调用的函数**，假设这个函数需要调用 Mysql，则 SpringBoot 就会向 Mysql 发送请求：查询结果，Mysql 处理完该请求后，就会返回给 SpringBoot ，然后 SpringBoot 再将结果返回给 client 端

-------------------------------

##### 用户认证操作

###### 授权机制

例如，某个用户想要查看某个 Bot 的代码，如果该用户不是该 Bot 的代码，那么该用户就没有权限查看

SpringBoot 里有权限判断模块，直接集成即可，Maven 官网搜 spring-boot-starter-security

默认 username 为 user，password 为随机生成的字符串

###### 一般网站默认认证方式都是通过 session 验证（传统登录认证方式），其原理为：

​	client 端将 username 和 password 等传到 SpringBoot 之后，SpringBoot 会在 Mysql 里查询，查询之后会将结果和 username，password 是否一致，如果一致就会将处理结果返回给 client 端

​	SpringBoot 在将结果返回给 client 端的时候，会在 session 里面传一个随机字符串（sessionID）给 client 端，在将 sessionID 传给 client 端之前，会将 sessionID 存下来（可以存到 Mysql 里，也可以存到 redis 里），client 端在拿到 sessionID 之后，就会将这个 sessionID 放到 cookie 里

​	未来每一次 client 在向 SpringBoot 发送请求时，都会自动默认从 cookie 里取出来 sessionID 放到 session 里面，并传给 SpringBoot，SpringBoot 会检查有没有 sessionID，如果有，就会从 Mysql（假设存到了Mysql里）里把 sessionID 读出来（Mysql 里存的是 sessionID 的各种信息），读出来之后会检查 sessionID 有没有过期

​	如果没有过期就表示当前 client 端的登录状态是成功的，即不需要判断权限了，如果过期了或者当前 sessionID 不存在，就表示当前 client 端的登录状态是过期的，同时会返回给 client 端登录页面

​	这里登录成功之后发送的 sessionID 类似于 “临时通行证”，client 端会在 cookie 里保存这张 “通行证”，之后client 端在向 SpringBoot 发送请求的时候都会带着这张 “通行证”，SpringBoot 会在 Mysql 里检查这张 “通行证” 有没有过期，是否合法，并且获取这张 “通行证” 对应的用户信息，当 “通行证” 没有过期的时候，就可以直接进行处理请求等操作，如果 “通行证” 过期了，就要重新办一张（重新登录获取 “通行证”）

##### kob项目采用 jwt 进行登陆验证

------------------------

##### Security 对接 Mysql

即通过 Mysql 来判断 client 端是否登录成功

###### 配置 Security 

实现 service.impl.UserDetailsServiceImpl 类，继承自 UserDetailsService 接口，用来接入数据库信息

- 重写方法，Alt + insert, 选重写

- 该方法会读入一个 username，返回 uername 对应的信息

- 在 Mysql 里查询对应的信息，需要 sql 语句，通过 mapper 层可以直接实现

- 实现 UserDetails 接口

- 在 Mysql 里 password 前面加上 {noop}，表示密码明文存储，此为 SpringBoot 的作者习惯

- 如果不是明文存储，则需要写密码加密

  ###### 加密算法特性：

  给定给一个字符串 p1，可以很快的将 p1变成一个新的字符串 p2（加密）

  由 p1——> p2可以很快得出结果，由 p2——> p1求不出来（**加密过程不可逆**）

  如此，**Mysql 在存储用户密码的时候就不要存储明文，而是存储密文**，这样即使未来 Mysql 泄露，也不会泄露用户密码，更加安全

  ##### 用同一种加密方式，相同密码不同次加密后得到的 p2 是匹配的，这样就可以判断用户输入的密码是否正确

实现 config.SecurityConfig 类，用来实现用户密码的加密存储

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```
