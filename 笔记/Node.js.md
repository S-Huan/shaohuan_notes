## Node.js概念
>Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。

1. 浏览器是 JavaScript 的前端运行环境,内置了bom dom api。
2. Node.js 是 JavaScript 的后端运行环境。
3. Node.js 作为一个 JavaScript 的运行环境，仅仅提供了基础的功能和 API。
* node <执行js文件>     在终端以这个命令执行，就是在node环境下执行js    



## fs 文件系统模块
>fs 模块是 Node.js 官方提供的、用来操作文件的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需求。

* 如果要在 JavaScript 代码中，使用 fs 模块来操作文件，则需要使用如下的方式先导入它：
```javascript
const fs = require('fs')
```
### fs.readFile() 方法
1.  用来读取指定文件中的内容
```javascript
// 1. 导入 fs 模块，来操作文件
const fs = require('fs')

// 2. 调用 fs.readFile() 方法读取文件
//    参数1：读取文件的存放路径
//    参数2：读取文件时候采用的编码格式，一般默认指定 utf8
//    参数3：回调函数，拿到读取失败和成功的结果  err  dataStr
fs.readFile('./files/11.txt', 'utf8', function(err, dataStr) {
  // 2.1 打印失败的结果
  // 如果读取成功，则 err 的值为 null
  // 如果读取失败，则 err 的值为 错误对象，dataStr 的值为 undefined
  console.log(err)
  console.log('-------')
  // 2.2 打印成功的结果
  console.log(dataStr)
})
```
 ### fs.writeFile() 方法
 1. 用来向指定的文件中写入内容
 * 可以写入文件，不能写入文件夹，路径里创建即可  -- /files/3.txt
 * 重复写入同一个文件，会覆盖旧文件
```javascript
// 1. 导入 fs 文件系统模块
const fs = require('fs')

// 2. 调用 fs.writeFile() 方法，写入文件的内容
//    参数1：表示文件的存放路径
//    参数2：表示要写入的内容
//    参数3：回调函数
fs.writeFile('./files/3.txt', 'ok123', function(err) {
  // 2.1 如果文件写入成功，则 err 的值等于 null
  // 2.2 如果文件写入失败，则 err 的值等于一个 错误对象
  // console.log(err)

  if (err) {
    return console.log('文件写入失败！' + err.message)
  }
  console.log('文件写入成功！')
})
```
### fs路径动态拼接问题
* 当fs代码出现相对路径时，会出现动态拼接问题，node执行路径（完整路径前半部分）+js文件的相对路径 = 完整路径
* 因为node执行命令会自动把相对路径拼接为完整路径，但是是根据node执行时所在的路径拼接的，所以node执行路径要跟js代码的路径一致。
#### 路径拼接解决方法
* 一般不用字符串拼接法，而用path方法
```
__dirname  表示当前文件的目录结构
__dirname + '/file/3.txt'   拼接字符串的形式，生成路径，
```

## path 路径模块

>path 模块是 Node.js 官方提供的、用来处理路径的模块。它提供了一系列的方法和属性，用来满足用户对路径的处理
需求。
* 如果要在 JavaScript 代码中，使用 path 模块来操作路径，则需要使用如下的方式先导入它：
```javascript
const path = require('path')
```
### path.join()
1. 使用 path.join() 方法，可以把多个路径片段拼接为完整的路径字符串，语法格式如下：
```javascript
const path = require('path')
 path.join(路径, 路径, 路径)
const fs = require('fs')

// 注意：  ../ 会抵消前面的路径
// const pathStr = path.join('/a', '/b/c', '../../', './d', 'e')
// console.log(pathStr)  // \a\b\d\e

// fs.readFile(__dirname + '/files/1.txt')

fs.readFile(path.join(__dirname, '/files/1.txt'), 'utf8', function(err, dataStr) {
  if (err) {
    return console.log(err.message)
  }
  console.log(dataStr)
})
```
### path.basename()
1. 使用 path.basename() 方法，可以从一个文件路径中，获取到文件的名称部分:
```javascript
const path = require('path')
// 定义文件的存放路径
const fpath = '/a/b/c/index.html'
const fullName = path.basename(fpath)
console.log(fullName)//index.html
const nameWithoutExt = path.basename(fpath, '.html')
//第一个参数传入路径，获取完整名字，第二个参数传入后缀，去除后缀
console.log(nameWithoutExt) //index
```
### path.extname()
1. 使用 path.extname() 方法，可以获取路径中的扩展名部分，语法格式如下：
```javascript
const path = require('path')
const fpath = '/a/b/c/index.html'
path.extname(fpath)//.html
```
## http 模块
>http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 http.createServer() 方法，就
能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。
* 服务器电脑与普通电脑区别就是有没有安装服务器软件（IIs Apache）
* 基于 Node.js 提供的http 模块，可以模拟一个服务器软件
* 如果要在 JavaScript 代码中，使用http 模块，则需要使用如下的方式先导入它：
```javascript
const http = require('http')
```

### 创建基本的web服务器
1.  导入 http 模块
2. 创建 web 服务器实例
3. 为服务器实例绑定 request 事件，监听客户端的请求
4. 启动服务器
```javascript
// 1. 导入 http 模块
const http = require('http')
// 2. 创建 web 服务器实例
const server = http.createServer()
// 3. 为服务器实例绑定 request 事件，监听客户端的请求
server.on('request', function (req, res) {
       //req.url  客户端请求的url，
	   //req.method 客户端的请求类型
	   //res 响应对象，服务器相关，要发送客户端的字符串
	   //res.end(str)  向客户端发送内容，并终止这次请求
	   //res.setHeader('Content-Type, text/html; charset = utf-8') 添加头部，解决发送中文乱码的问题
  console.log('Someone visit our web server.')
})
// 4. 启动服务器  监听8080端口
server.listen(8080, function () {  
  console.log('server running at http://127.0.0.1:8080')
})
```
