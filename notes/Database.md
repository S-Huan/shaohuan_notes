# Database
1. MySQL、Oracle、SQL Server 属于传统型数据库（又叫做：关系型数据库 或 SQL 数据库）。
2. Mongodb 属于新型数据库（又叫做：非关系型数据库 或 NoSQL 数据库）。
3. 传统db的组织结构分为数据库(database)、数据表(table)、数据行(row)、字段(field)
4. 每个项目都对应独立的数据库,表存放数据大类，字段为数据小类，**数据行** 为具体数据。

# MySQL
>SQL（英文全称：Structured Query Language）是结构化查询语言，专门用来访问和处理数据库的编程语言。能够让我们以编程的形式，操作数据库里面的数据。
1. MySQL Server：专门用来提供数据存储和服务的软件。
2. MySQL Workbench：可视化的 MySQL 管理工具。
- SQL 是一门数据库编程语言;
- 使用 SQL 语言编写出来的代码，叫做 SQL 语句;
- SQL 语言只能在关系型数据库中使用（例如 MySQL、Oracle、SQL Server）。
## 查询数据（SELECT）
- SELECT 语句用于从表中查询数据。执行的结果被存储在一个结果表中（称为结果集）。
```sql
-- 从指定的表中，查询所有的数据
select * from 表名称
-- 从指定的表中，查询 指定列名称（字段）的数据
select 列名称, 列名称 from 表名称
```
## 插入数据 （INSERT INTO）
- INSERT INTO 语句用于向数据表中插入新的数据行。
```sql
-- 向指定的表中，插入一行数据，值要和字段一一对应
INSERT INTO 表名称 (字段1，字段2，字段3) VALUES ('值1', '值2', '值3')
```
## 更新数据（UPDARE）
- Update 语句用于修改表中的数据。
```sql
-- update 要更新的表  set 要更新的字段与值   where 查询条件
update 表名称 set 字段 = 新值 where 字段 = 某值（任意，只要能定位到要更改的精确行就行）
```
## 删除数据 (DELETE)
- DELETE 语句用于删除表中的行
```sql
-- 从指定的表中，根据where条件，删除对应的行
delete from 表名称 where 字段 = 值
```

## 条件语句
- sql查询顺序为，从表到字段，再到具体的值。

### where 子句
- WHERE 子句用于限定选择的标准。
- 在 SELECT、UPDATE、DELETE 语句中，皆可使用 WHERE 子句来限定。
![[Pasted image 20220609223939.png]]
```sql
select * from users where status=1
select * from users where id>=2
select * from users where username<>'ls'
select * from users where username!='ls'
```

#### AND 和 OR 运算符
1. AND 和 OR 可在 WHERE 子语句中把两个或多个条件结合起来。
2. AND 表示必须同时满足多个条件，相当于 JavaScript 中的 && 运算符，例如 if (a !== 10 && a !== 20) 
3. OR 表示只要满足任意一个条件即可，相当于 JavaScript 中的 || 运算符，例如 if(a !== 10 || a !== 20)
```sql
select * from users where status=0 and id<3
select * from users where status=1 or username='zs'
```
### ORDER BY 子句
1. ORDER BY 语句用于根据指定的列对结果集进行排序。
2. ORDER BY 语句默认按照升序（ASC）对记录进行排序。
3. 如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。
```sql
--升序
select * from users order by status ASC
--降序
select * from users order by id desc
--多重排序，先按照 status 字段进行降序排序，再按照 username 的字母顺序，进行升序排序
select * from users order by status desc, username asc
```
### COUNT(*) 函数
- COUNT(*) 函数用于返回查询结果的总数据条数
```sql
select count(*) from users where status=0
```


# Node中的Mysql
1. 安装操作 MySQL 数据库的第三方模块（mysql）
![[Pasted image 20220609225457.png]]
2. 通过 mysql 模块连接到 MySQL 数据库

![[Pasted image 20220609225446.png]]
3. 通过 mysql 模块执行 SQL 语句

## 查询数据（results里获得的是数组对象的类型）
- 注意db.query可以不写回调函数，直接赋值变量可以直接得到一个数组，[0]为查询的数据
	
![[Pasted image 20220609225558.png]]
## 插入数据（results里获得的是对象的类型）
![[Pasted image 20220609225703.png]]

### 插入数据的便捷方式
- 数据对象的每个属性和数据表的字段要一一对应
![[Pasted image 20220609225758.png]]
## 更新数据（results里获得的是对象的类型）
![[Pasted image 20220609230119.png]]
### 更新数据的便捷方式
- 数据对象的每个属性要和数据表的字段一一对应
![[Pasted image 20220609230203.png]]
## 删除数据（results里获得的是对象的类型）
![[Pasted image 20220609230303.png]]
### 标记删除
- 使用标记删除的形式，来模拟删除的动作
>所谓的标记删除，就是在表中设置类似于 status 这样的状态字段，来标记当前这条数据是否被删除。
```javascript
// 标记删除
const sqlStr = 'update users set status=? where id=?'
db.query(sqlStr, [1, 6], (err, results) => {
  if (err) return console.log(err.message)
  if (results.affectedRows === 1) {
    console.log('标记删除成功')
  }
})
```


# 前后端的身份认证
## 服务端渲染的传统 Web 开发模式
>服务器发送给客户端的 HTML 页面，是在服务器通过字符串的拼接，动态生成的。因此，客户端不需要使用 Ajax 这样的技术额外请求页面的数据。
###  Session 认证机制
#### Cookie（不安全的认证）
>Cookie 是存储在用户浏览器中的一段不超过 4 KB 的字符串。它由一个名称（Name）、一个值（Value）和其它几个用于控制 Cookie 有效期、安全性、使用范围的可选属性组成。

>不同域名下的 Cookie 各自独立，每当客户端发起请求时，会自动把当前域名下所有未过期的 Cookie 一同发送到服务器。
- 不具有安全性，浏览器也提供了读写 Cookie 的 API，因此 Cookie 很容易被伪造。
- 相当于服务器不验证cookie，看到cookie直接给数据。不安全。接下来的session，服务端拿到cookie会先验证一下，再确认。

![[Pasted image 20220609232058.png]]
#### Session 的工作原理
- 通过把信息存放在服务端中，用cookie来证明要请求的数据，服务端发送响应的数据给客户端。
![[Pasted image 20220609233604.png]]
#### Session的用法
1. 安装 express-session 中间件
```bash
	npm install express-session
```
2. 配置中间件
```javascript
let session = require('express-session')
app.use(
	session({
		secret: 'keyboard cat', //secret 属性值为任意字符串
		resave: false, //固定写法
		saveUninitialized: true //固定写法
		})
)
```
3. 向session存数据
```javascript
// 托管静态页面
app.use(express.static('./pages'))
// 解析 POST 提交过来的表单数据
app.use(express.urlencoded({ extended: false }))

// 登录的 API 接口                 
app.post('/api/login', (req, res) => {
  // 判断用户提交的登录信息是否正确
  if (req.body.username !== 'admin' || req.body.password !== '000000') {
    return res.send({ status: 1, msg: '登录失败' })
  }

  // TODO_02：请将登录成功后的用户信息，保存到 Session 中
  // 注意：只有成功配置了 express-session 这个中间件之后，才能够通过 req 点出来 session 这个属性
  req.session.user = req.body // 用户的信息
  req.session.islogin = true // 用户的登录状态

  res.send({ status: 0, msg: '登录成功' })
})
```
4. 从 session 中取数据
```javascript
// 获取用户姓名的接口
app.get('/api/username', (req, res) => {
  // TODO_03：请从 Session 中获取用户的名称，响应给客户端
  if (!req.session.islogin) {
    return res.send({ status: 1, msg: 'fail' })
  }
  res.send({
    status: 0,
    msg: 'success',
    username: req.session.user.username,
  })
})
```
5. 清空 session 数据
```javascript
// 退出登录的接口
app.post('/api/logout', (req, res) => {
  // TODO_04：清空 Session 信息
  req.session.destroy()
  res.send({
    status: 0,
    msg: '退出登录成功',
  })
})
// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(80, function () {
  console.log('Express server running at http://127.0.0.1:80')
})
``` 
#### Session 局限
- Session 认证机制需要配合 Cookie 才能实现。但是 Cookie 默认不支持跨域访问。
- 当前端请求后端接口不存在跨域问题的时候，推荐使用 Session 身份认证机制。
- 当前端需要跨域请求后端接口的时候，推荐使用 JWT 认证机制。

## 前后端分离的新型 Web 开发模式
>就是后端只负责提供 API 接口，前端使用 Ajax 调用接口的开发模式。
### JWT 认证机制
- JWT（英文全称：JSON Web Token）是目前最流行的跨域认证解决方案。
#### JWT 工作原理
- 用户的信息通过 Token 字符串的形式，保存在客户端浏览器中。服务器通过还原 Token 字符串的形式来认证用户的身份，相当于把用户数据通过服务器加密，存放在客户端中，在通过服务端解密，取出。
![[Pasted image 20220612141154.png]]
- WT 通常由三部分组成，分别是 Header（头部）、Payload（有效荷载）、Signature（签名）三者之间使用英文的“.”分隔，
- Payload 部分才是真正的用户信息，它是用户信息经过加密之后生成的字符串。
#### JWT 的使用方式
- 客户端收到服务器返回的 JWT 之后，通常会将它储存在 localStorage 或 sessionStorage 中。
- 此后，客户端每次与服务器通信，都要带上这个 JWT 的字符串，从而进行身份认证。推荐的做法是把 JWT 放在 HTTP 请求头的 Authorization 字段中，
```
Authorization: Bearer <token>
```
1. 安装 JWT 相关的包
>npm install jsonwebtoken express -jwt
- jsonwebtoken 用于生成 JWT 字符串
- express-jwt 用于将 JWT 字符串解析还原成 JSON 对象
2. 导入 JWT 相关的包
![[Pasted image 20220612144331.png]]
3. 定义 secret 密钥
>为了保证 JWT 字符串的安全性，防止 JWT 字符串在网络传输过程中被别人破解，我们需要专门定义一个用于加密和解密的 secret 密钥
- 当生成 JWT 字符串的时候，需要使用 secret 密钥对用户的信息进行加密，最终得到加密好的 JWT 字符串
- 当把 JWT 字符串解析还原成 JSON 对象的时候，需要使用 secret 密钥进行解密
![[Pasted image 20220612144850.png]]
4. 在登录成功后生成 JWT 字符串
- 调用 jsonwebtoken 包提供的 sign() 方法，将用户的信息加密成 JWT 字符串，响应给客户端：
- expiresIn:'30s'       30s后过期这个token
![[Pasted image 20220612144933.png]]
5. 将收到 JWT 字符串还原为 JSON 对象
>客户端每次在访问那些有权限接口的时候，都需要主动通过请求头中的 Authorization 字段，将 Token 字符串发送到服务器进行身份认证。

此时，服务器可以通过 express-jwt 这个中间件，自动将客户端发送过来的 Token 解析还原成 JSON 对象：
- 中间件要传输前面给设置的加密的参数
![[Pasted image 20220612145520.png]]
6. 使用 req.user 获取用户信息
 >当 express-jwt 这个中间件配置成功之后，即可在那些有权限的接口中，使用 req.user 对象，来访问从 JWT 字符串中解析出来的用户信息了，示例代码如下：
![[Pasted image 20220612145620.png]]
7. 捕获解析 JWT 失败后产生的错误

>当使用 express-jwt 解析 Token 字符串时，如果客户端发送过来的 Token 字符串过期或不合法，会产生一个解析失败的错误，影响项目的正常运行。我们可以通过 Express 的错误中间件，捕获这个错误并进行相关的处理
![[Pasted image 20220612145745.png]]