
# Node.js

## Node.js概念
>Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。

1. 浏览器是 JavaScript 的前端运行环境,内置了bom dom api。
2. Node.js 是 JavaScript 的后端运行环境。
3. Node.js 作为一个 JavaScript 的运行环境，仅仅提供了基础的功能和 API。
	* node <执行js文件>     在终端以这个命令执行，就是在node环境下执行js    
* 直接下载安装，存在于终端中
* npm安装nodemon全局包，可以代码更改就自动重启服务器

## 模块化概念
>模块化是指解决一个复杂问题时，自顶向下逐层把系统划分成若干模块的过程。对于整个系统来说，模块是可组合、分解和更换的单元.

>编程领域中的模块化，就是遵守固定的规则，把一个大文件拆成独立并互相依赖的多个小模块。

* 把js拆分成不同的份，用加载模块加载就行自定义模块 → 把拆分的不同块都放在统一文件夹，并上传npm，就是第三方模块

### 模块化的规范
> Node.js 遵循了 CommonJS 模块化规范，CommonJS 规定了模块的特性和各模块之间如何相互依赖。
1. 每个模块内部，module 变量代表当前模块。
2. module 变量是一个对象，它的 exports 属性（即 module.exports）是对外的接口。
3. 加载某个模块，其实是加载该模块的 module.exports 属性。require() 方法用于加载模块。

### Node.js 里的模块化
1.  内置模块（内置模块是由 Node.js 官方提供的，例如 fs、path、http 等）
2. 自定义模块（用户创建的每个 .js 文件，都是自定义模块）只有当每个js 在node环境运行才是模块
3. 第三方模块（由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，使用前需要先下载）

#### 模块作用域
>和函数作用域类似，在自定义模块中定义的变量、方法等成员，只能在当前模块内被访问，这种模块级别的访问限制，叫做模块作用域。

* 和js不同，js之间的相互引用是可以访问其成员的，即全局作用域。而模块的js之间不能互相访问。防止全局污染。

#### 加载模块
1. 可以加载需要的内置模块、用户自定义模块、第三方模块进行使用。
2. 使用 require() 方法加载其它模块时，会执行被加载模块中的代码。
```javascript
//加载内置模块
const http = require('http')
//加载自定义
const http = require('./path/doc.js')   --带相对路径，可以省略.js
//加载第三方
const http = require('moment') 
```
#### 模块的加载机制
1. 优先从缓存中加载
>模块在第一次加载后会被缓存。 这也意味着多次调用 require() 不会导致模块的代码被执行多次。
注意：不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率。
2. 内置模块的加载机制
>内置模块是由 Node.js 官方提供的模块，内置模块的加载优先级最高。
例如，require('fs') 始终返回内置的 fs 模块，即使在 node_modules 目录下有名字相同的包也叫做 fs。
3. 自定义模块的加载机制
>使用 require() 加载自定义模块时，必须指定以 ./ 或 ../ 开头的路径标识符。在加载自定义模块时，如果没有指定 ./ 或 ../ 这样的路径标识符，则 node 会把它当作内置模块或第三方模块进行加载。
* 按照确切的文件名进行加载 → 补全 .js 扩展名进行加载 → 补全 .json 扩展名进行加载 → 补全 .node 扩展名进行加载
4. 第三方模块的加载机制
> 如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ‘./’ 或 ‘../’ 开头，则 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块。
* 如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，直到文件系统的根目录。
5. 目录作为模块
>当把目录作为模块标识符，传递给 require() 进行加载的时候，有三种加载方式：

   1. 在被加载的目录下查找一个叫做 package.json 的文件，并寻找 main 属性，作为 require() 加载的入口
   2. 如果目录里没有 package.json 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的 index.js 文件。
   3. 如果以上两步都失败了，则 Node.js 会在终端打印错误消息，报告模块的缺失：Error: Cannot find module 'xxx'

##  Node.js自定义模块
>在每个 .js 自定义模块中都有一个 module 对象，它里面存储了和当前模块有关的信息
如：exports 对象
* 在终端node执行js时，它就是自定义模块，在js里面打印module，里面就有module对象
* 当用require引用自定义模块时，其实时引用自定义模块的module.exports 对象
```javascript
Module {

  id: '.',

  path: '/Users/ahuan/My_Space/My_Work/NEW/Study/Exercise',

  exports: {},

  filename: '/Users/ahuan/My_Space/My_Work/NEW/Study/Exercise/test.js',

  loaded: false,

  children: [],

  paths: [

    '/Users/ahuan/My_Space/My_Work/NEW/Study/Exercise/node_modules',

    '/Users/ahuan/My_Space/My_Work/NEW/Study/node_modules',

    '/Users/ahuan/My_Space/My_Work/NEW/node_modules',

    '/Users/ahuan/My_Space/My_Work/node_modules',

    '/Users/ahuan/My_Space/node_modules',

    '/Users/ahuan/node_modules',

    '/Users/node_modules',

    '/node_modules'

  ]

}
```
##### module.exports 对象
>在自定义模块中，可以使用 module.exports 对象，将模块内的成员共享出去，供外界使用。
外界用 require() 方法导入自定义模块时，得到的就是 module.exports 所指向的对象。

 * 使用 require() 方法导入模块时，导入的结果，永远以 module.exports 指向的对象为准。
```javascript
//当外界require这个自定义模块时，获取的就是 module.exports 所指向的对象
const username = 'zs'
module.exports.username = username
exports.age = 20
exports.sayHello = function() {
  console.log('大家好！')
```
* Node 提供了 exports 对象。默认情况下，exports 和 module.exports 指向同一个对象

##### 使用误区
* 避免在同一个模块中同时使用 exports 和 module.exports
1. exports赋值属性，后module.exports赋值对象，exports赋值属性无效。
```javascript
exports.age = 20
module.exports = {
username: 'xiaolong',
}
//require出来的就只有username，没有age，因为当module.exports赋值对象时，和age不是一个对象，module重新指向的是username的对象，所以exports指向也转变为usename这个对象
```
2. module.exports赋值属性，exports赋值对象，exports赋值的无效
>因为exports赋值的对象最终都要指向module.exports的对象指向，所以后面赋值啥对象都没用。
3. module.exports赋值属性，exports赋值属性，两者都有效，因为赋值的都是属性，在同一对象内。
4. exports赋值对象，module.exports赋值exports，两者都在同一个对象，两者赋值都有效。


## Node.js内置模块

### fs 文件系统模块
>fs 模块是 Node.js 官方提供的、用来操作文件的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需求。

* 如果要在 JavaScript 代码中，使用 fs 模块来操作文件，则需要使用如下的方式先导入它：
```javascript
const fs = require('fs')
```
#### fs.readFile() 方法
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
 #### fs.writeFile() 方法
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
#### fs路径动态拼接问题
* 当fs代码出现相对路径时，会出现动态拼接问题，node执行路径（完整路径前半部分）+js文件的相对路径 = 完整路径
* 因为node执行命令会自动把相对路径拼接为完整路径，但是是根据node执行时所在的路径拼接的，所以node执行路径要跟js代码的路径一致。
##### 路径拼接解决方法
* 一般不用字符串拼接法，而用path方法
```
__dirname  表示当前文件的目录结构
__dirname + '/file/3.txt'   拼接字符串的形式，生成路径，
```

### path 路径模块

>path 模块是 Node.js 官方提供的、用来处理路径的模块。它提供了一系列的方法和属性，用来满足用户对路径的处理
需求。
* 如果要在 JavaScript 代码中，使用 path 模块来操作路径，则需要使用如下的方式先导入它：
```javascript
const path = require('path')
```
#### path.join()
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
#### path.basename()
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
#### path.extname()
1. 使用 path.extname() 方法，可以获取路径中的扩展名部分，语法格式如下：
```javascript
const path = require('path')
const fpath = '/a/b/c/index.html'
path.extname(fpath)//.html
```
### http 模块
>http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 http.createServer() 方法，就
能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。
* 服务器电脑与普通电脑区别就是有没有安装服务器软件（IIs Apache）
* 基于 Node.js 提供的http 模块，可以模拟一个服务器软件
* 如果要在 JavaScript 代码中，使用http 模块，则需要使用如下的方式先导入它：
```javascript
const http = require('http')
```

#### 创建基本的web服务器
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
### Querystring 解析模块
> Node.js 内置了一个 querystring 模块，专门用来处理查询字符串。通过这个模块提供的 parse() 函数，可以轻松把查询字符串，解析成对象的格式.

```javascript
const qs = require('querystring')
const body = qs.parse(str)
```
## Node.js第三方模块
>Node.js 中的第三方模块又叫做包。
1. 包是基于内置模块封装出来的，提供了更高级、更方便的 API，极大的提高了开发效率。
2.  https://www.npmjs.com/ ，它是全球最大的包共享平台，用于搜索
3.   https://registry.npmjs.org/  服务器上下载自己需要的包
4.  Node Package Manager（简称 npm 包管理工具），这个包管理工具随着 Node.js 的安装包一起被安装到了用户的电脑上。
 * npm源转换
     1. npm config get registry   --查看当前 
     2. npm config set registry=https://registry.npm.taobao.org/  --切换淘宝镜像源
     3. nrm use npm 切换
     4. nrm ls  查看
### 包的分类
#### 项目包
>那些被安装到项目的 node_modules 目录中的包，都是项目包。
1. 开发依赖包（包名会被注册在package.json的devDependencies里面，仅在开发环境下存在的包用-D，如babel、[sass](https://so.csdn.net/so/search?q=sass&spm=1001.2101.3001.7020)-loader这些解析器）
- npm i 包名 -D  
2. 核心依赖包（包名会被注册在package.json的dependencies里面，在生产环境下这个包的依赖依然存在。）
- npm i 包名 -S

==注意==啥也不写,包名不会进入package.json里面，因此别人不知道安装了这个包，不建议这样。
#### 全局包
>不针对某个项目，所以在任何一个位置都可以执行
 1. 只有工具性质的包，才有全局安装的必要性。因为它们提供了好用的终端命令。
 2. 会安装到 ~/.npm-global目录下。
 3. 判断某个包是否需要全局安装后才能使用，可以参考官方提供的使用说明即可。
语法：
1. npm i 包名 -g    全局安装包
2. npm uninstall 包名 -g 卸载

案例：
npm i nrm -g    快速切换包
npm i i5ting_toc -g   是一个可以把 md 文档转为 html 页面的小工具

* mac系统安装全局包教程
```bash
# 第一步：在你的用户文件下新建一个文件夹，这个.npm-global 名字可以用你自己喜欢的名字替换，推荐直接使用这个名字。
mkdir ~/.npm-global
#第二步：更改node的安装连接
npm config set prefix '~/.npm-global'
#第三步：在用户的profile下增加path，为的是系统能够找到可执行文件的目录，需要自己创建
 export PATH=~/.npm-global/bin:$PATH
 source ~/.profile #添加重新启动
#第四步：wq使其生效

```

### 包的规范
> 一个规范的包，它的组成结构，必须符合以下 3 点要求：

1. 包必须以单独的目录而存在
2. 包的顶级目录下要必须包含 package.json 这个包管理配置文件
3. package.json 中必须包含 name，version，main 这三个属性，分别代表包的名字、版本号、包的入口

### 包的安装
* 安装在项目跟目录下
1. npm install 包的完整名称 / npm i  包完整名称
2.  npm i moment@2.22.2       --安装指定版本的包
* 第一次安装完毕会在项目文件夹下多一个叫做 node_modules 的文件夹和 package-lock.json 的配置文件。
* node_modules 文件夹用来存放所有已安装到项目中的包
* package-lock.json 配置文件用来记录 node_modules 目录下的每一个包的下载信息(只记录，是项目的包下载记录，和包管理配置文件不一样)
### 包的卸载
> npm uninstall 包的完整名称 

* npm uninstall 命令执行成功后，会把卸载的包，自动从 package.json 的 dependencies 中移除掉。
### 包管理配置文件
 >package.json 的包管理配置文件。用来记录与项目有关的一些配置信息
 
 作用：
 1.  在包的顶级目录下，一般下载包时自带，包含name，version，main 这三个属性等，用于记录包的信息
 2. 在项目的顶级目录下，装包时自己创建，安装包时，把包的的信息记录进去，用于记录项目所有包的信息（此作用下，相当于把项目当成一个包，像作用一一样，记录项目的名称，版本，描述等，为以后创建自己的npm做准备）
 
### 项目下的包管理配置文件
 4. 项目的名称、版本号、描述；
 5. 项目中都用到了哪些包；
 6. 哪些包只在开发期间会用到；
 7. 那些包在开发和部署时都需要用到。

#### package.json的创建
>手动创建的目的就是，创建name，version，main 属性信息，让项目更像包的结构，然后记录包的用途信息。其实真正的包的根目录下才是真正的包配置文件，记录包的信息。
* 在执行命令的文件夹中 npm init -y  创建配置文件
*  注意！先创建包管理配置文件，在安装包
1. 只能在英文的目录下成功运行
2. 当npm install 包时，会自动把包的名称和版本号，记录到 package.json 中。
#### dependencies 节点
> package.json 文件中，有一个 dependencies 节点，专门用来记录您使用 npm install 命令安装过哪些包。
* npm install -s 就是npm install --save　　安装到生产环境，如vue，react等
#### devDependencies 节点
 > 如果某些包只在项目开发阶段会用到，在项目上线之后不会用到，则建议把这些包记录到 devDependencies 节点中。 
* npm install -d 就是npm install --save-dev　　安装到开发环境，例如gulp、babel、webpack一般是辅助工具
 
#### 多人协作问题
* 当 node_modules 文件夹存放的第三方包太大时，上传github交互不方便
* 要把 node_modules 文件夹，添加到 .gitignore 忽略文件中。
* 然后对方执行npm install 就能根据dependencies 节点自动安装所有项目需要的包

### 自定义第三方模块
>这里开发自己的npm包，其实就是利用了自定义模块的方法。先自定义自己的js模块，然后通过module.exports暴露出去。在通过入口文件，引用js模块。

>在项目目录下的包管理配置文件，就是为了构建自己的npm包而创建的。

1. 初始化包的基本结构
    * 新建 baoming 文件夹，作为包的根目录,
    * 在 baoming 文件夹中，新建如下三个文件：
          1.  package.json （包管理配置文件）
          2. index.js          （包的入口文件）
          3. README.md  （包的说明文档）
2. 初始化 package.json
     * npm init -y
   ```bash
  "name": "exercise",
  "version": "1.0.0",
  "main": "test.js",
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
   ```
3. 将不同的功能进行模块化拆分
     1. 将格式化时间的功能，拆分到 src -> dateFormat.js 中
     2. 将处理 HTML 字符串的功能，拆分到 src -> htmlEscape.js 中
     3. 在 index.js 中，导入两个模块，得到需要向外共享的方法
     4. 在 index.js 中，使用 module.exports 把对应的方法共享出去（因为别人用包就是引用入口文件）
4. 登录npm   →   npm login
      * 在运行 npm login 命令之前，必须先把下包的服务器地址切换为 npm 的官方服务器。
5. 发布包
     * 将终端切换到包的根目录之后，运行 npm publish 命令，即可将包发布到 npm 上
6. 删除包
     * 运行 npm unpublish 包名 --force 命令，即可从 npm 删除已发布的包
     * npm unpublish 命令只能删除 72 小时以内发布的包
     *  npm unpublish 删除的包，在 24 小时内不允许重复发布

    


   

## Express
*  Express 是基于 Node.js 平台，快速、开放、极简的 Web 开发框架。基于内置模块http封装
*  Express 的本质：就是一个 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法。
* 使用 Express，我们可以方便、快速的创建 Web 网站的服务器或 API 接口的服务器。 

### 创建基本的 Web 服务器
```javascript
const express = require("express");
const app = express()
app.listen(80,() => {
console.log('express server running at http://127.0.0.1') //监听端口
})
```

#### 监听请求
* req.query (get请求下的属性)，获取get服务端发送的查询字符串，它可以自己转义为对象
* req.body(post请求下的属性)， 获取post服务端发送的查询字符串或者json，它不自动转义，需要添加内置中间件，才能把post服务端发送的查询字符串或者json，转义为对象。
```javascript
// 4. 监听客户端的 GET 和 POST 请求，并向客户端响应具体的内容
app.get('/user', (req, res) => {
  // 调用 express 提供的 res.send() 方法，客户端接受一个 JSON 对象,(传送还是普通对象)
  res.send({ name: 'zs', age: 20, gender: '男' })
})
app.post('/user', (req, res) => {
  // 调用 express 提供的 res.send() 方法，客户端接收一个 文本字符串
  res.send('请求成功')
})
```
#### 获取 URL 中携带的查询参数
```javascript
app.get('/', (req, res) => {
  // 通过 req.query (get请求下的属性)可以获取到客户端发送过来的 查询字符串参数
  // 注意：默认情况下，req.query 是一个空对象
  console.log(req.query)
  res.send(req.query)
})
```
#### 获取 URL 中的动态参数
* : 动态参数名，可以任意
* 可以获取多个动态参数
```javascript
// 注意：这里的 :id 是一个动态的参数
//比如客户端查询url为  http://127.0.0.1/user/1/zhangshan
//服务端的url显示动态参数的结构/user/:id/:username
//req.params 就可以获取到服务端传来的值，默认为空对象
app.get('/user/:id/:username', (req, res) => {
  console.log(req.params)//{id: '1', username: 'zhangshan'  }
  res.send(req.params) 
})
```
#### 托管静态资源
- 静态资源：一般客户端发送请求到web服务器，web服务器从内存在取到相应的文件，返回给客户端，客户端解析并渲染显示出来。
- 动态资源：一般客户端请求的动态资源，先将请求交于web容器，web容器连接数据库，数据库处理数据之后，将内容交给web服务器，web服务器返回给客户端解析渲染处理。

> 静态资源和动态资源的区别
- 静态资源一般都是设计好的html页面，而动态资源依靠设计好的程序来实现按照需求的动态响应；
- 静态资源的交互性差，动态资源可以根据需求自由实现；
- 在服务器的运行状态不同，静态资源不需要与数据库参于程序处理，动态可能需要多个数据库的参与运算。
----
>express 提供了一个非常好用的函数，叫做 express.static()，通过它，我们可以非常方便地创建一个静态资源服务器，例如，通过如下代码就可以将 public 目录下的图片、CSS 文件、JavaScript 文件对外开放访问了，存放静态文件的目录名不会出现在 URL 中。
* 相当与把一个文件夹透明化，服务端url不用再写文件夹名，直接调用里面的文件就行
*  如果要托管多个静态资源目录，请多次调用 express.static() 函数
* 访问静态资源文件时，express.static() 函数会根据目录的添加顺序查找所需的文件。

语法：
```javascript
app.use(express.static('public')) //服务端查找同名文件，先去这个文件夹查找
app.use(express.static('pub'))
```
##### 挂载路径前缀
> 在托管的静态资源访问路径之前，挂载路径前缀
> 你隐藏了一个路径，可以再自定义添加一个路径

语法：
```javascript
app.use('/public', express.static('pub'))   //服务端查找pub文件夹文件时，必须再路径前添加 /public
```


### Express 路由
>在 Express 中，路由指的是客户端的请求与服务器处理函数之间的映射关系。
* 路由分 3 部分组成，分别是请求的类型、请求的 URL 地址、处理函数
* 按照定义的先后顺序进行匹配，请求类型和请求的URL同时匹配成功，才会调用对应的处理函数
* 其实就是服务器对get post请求的监听
#### 简单的路由
路由（监听）直接挂载在服务器的实例上
```javascript
const express = require('express')
const app = express()

// 挂载路由
app.get('/', (req, res) => {
  res.send('hello world.')
})
app.post('/', (req, res) => {
  res.send('Post Request.')
})
app.listen(80, () => {
  console.log('http://127.0.0.1')
})
```
#### 模块化的路由
>为了方便对路由进行模块化的管理，推荐将路由抽离为单独的模块。

1. 创建路由模块对应的 .js 文件
2. 调用 express.Router() 函数创建路由对象
3. 向路由对象上挂载具体的路由
4. 使用 module.exports 向外共享路由对象 
```javascript
//创建路由模块的js
// 1. 导入 express
const express = require('express')
// 2. 创建路由对象
const router = express.Router()
// 3. 挂载具体的路由
router.get('/user/list', (req, res) => {
  res.send('Get user list.')
})
router.post('/user/add', (req, res) => {
  res.send('Add new user.')
})
// 4. 向外导出路由对象
module.exports = router
```
5. 使用 app.use() 函数注册路由模块
* 其实就是把路由模块这个中间件，注册为全局中间件。把路由变为中间件？
```javascript
//在服务器js里注册路由
const express = require('express')
const app = express()
// 1. 导入路由模块
const router = require('./03.router')
// 2. 注册路由模块、并添加访问前缀，跟挂载路径前缀一样
app.use('/api', router)
// 注意： app.use() 函数的作用，就是来注册全局中间件
app.listen(80, () => {
  console.log('http://127.0.0.1')
})
```

### Express 中间件
>中间件（Middleware ），特指业务流程的中间处理环节。当一个请求到达 Express 的服务器之后，可以连续调用多个中间件，从而对这次请求进行预处理。

***格式*** ：
* Express 的中间件，本质上就是一个 function 处理函数（请求的预处理）
* 形式上比路由多一个next函数，最后还要调用next(),流转到下一个中间件或者路由
* 多个中间件之间，共享同一份 req 和 res。统一为 req 或 res 对象添加自定义的属性或方法，供下游的中间件或路由进行使用
* 因为是本质是请求的预处理，所以要放在路由之前
```javascript
// 定义一个最简单的中间件函数
const mw = function (req, res, next) {
  console.log('这是最简单的中间件函数')
 // 把流转关系，转交给下一个中间件或路由
  next()
}
```
![[Pasted image 20220605104247.png]]
#### 全局中间件
>客户端发起的任何请求，到达服务器之后，都会触发的中间件(相当于任何路由都享有这写中间件)，叫做全局生效的中间件。

  * 通过调用 app.use(中间件函数)，即可定义一个全局生效的中间件
  * 可以使用 app.use() 连续定义多个全局中间件。会按照中间件定义的先后顺序依次进行调用
```javascript
 const mw = function (req, res, next) {
  console.log('这是最简单的中间件函数')
  next()
}
// 将 mw 注册为全局生效的中间件
app.use(mw)
// 简化形式
app.use((req, res, next) => {
  console.log('这是最简单的中间件函数')
  next()
})
```
#### 局部中间件
>不使用 app.use() 定义的中间件，叫做局部生效的中间件.(这些中间件只给特定的路由使用)
* 可以创建多个局部中间件，在路由里添加即可
```javascript

// 1. 定义中间件函数
const mw1 = (req, res, next) => {
  console.log('调用了局部生效的中间件1')
  next()
}
const mw2 = (req, res, next) => {
  console.log('调用了局部生效的中间件2')
  next()
}
// 2. 创建有中间件的路由
app.get('/', mw1, mw2, (req, res) => {
  res.send('Home page.')
})
// 3. 第二种写法
app.get('/', [mw1, mw2], (req, res) => {
  res.send('Home page.')
})
```
#### 注意事项
1. 一定要在路由之前注册中间件
2. 客户端发送过来的请求，可以连续调用多个中间件进行处理
3. 执行完中间件的业务代码之后，不要忘记调用 next() 函数
4. 为了防止代码逻辑混乱，调用 next() 函数后不要再写额外的代码
5. 连续调用多个中间件时，多个中间件之间，共享 req 和 res 对象
#### 中间件的分类
 #####  应用级别的中间件
 >通过 app.use() 或 app.get() 或 app.post() ，绑定到 app 实例上的中间件，叫做应用级别的中间件
 * 全局中间件和局部中间件都是应用中间件
 ##### 路由级别的中间件
 >绑定到 express.Router() 实例上的中间件，叫做路由级别的中间件。它的用法和应用级别中间件没有任何区别。
 * 中间件就是路由的预处理，路由需要模块化，所以就有绑定在路由模块上的
```javascript
//路由模块
const express = require('express')
// 1. 创建路由对象
const router = express.Router()

// 2. 路由模块的全局中间件
router.use(function(req, res,next){
console.log('这是路由模块的全局中间件')
next()
})

// 3. 路由模块的局部中间件
const mw1 = (req, res, next) => {
  console.log('调用了局部生效的中间件1')
  next()
}
router.get('/user/list', mw1，(req, res) => {
  res.send('Get user list.')
})
router.post('/user/add', mw1， (req, res) => {
  res.send('Add new user.')
})
```
##### 错误级别的中间件
 >错误级别中间件的作用：专门用来捕获整个项目中发生的异常错误，从而防止项目异常崩溃的问题。
 
* 格式：错误级别中间件的 function 处理函数中，必须有 4 个形参，分别是 (err, req, res, next)。
 * 错误级别的中间件，必须注册在所有路由之后
```javascript
// 1. 定义路由
app.get('/', (req, res) => {
  // 1.1 人为的制造错误
  throw new Error('服务器内部发生了错误！')
  res.send('Home page.')
})

// 2. 定义错误级别的中间件，捕获整个项目的异常错误，从而防止程序的崩溃
app.use((err, req, res, next) => {
  console.log('发生了错误！' + err.message)
  res.send('Error：' + err.message)
})
```

 ##### 内置的中间件
 * express.static 快速托管静态资源的内置中间件，例如： HTML 文件、图片、CSS 样式等（无兼容性）
 * express.json 解析 JSON 格式的请求体数据（有兼容性，仅在 4.16.0+ 版本中可用）
 ```javascript
 //在路由模块或者，应用实例上，绑定中间件
 app.use(express.json())  //这个中间件，解析表单中的 JSON 格式的数据
 app.post('/user', (req, res) => {
  // 在服务器，可以使用 req.body 这个属性，来接收客户端发送过来的请求体数据
  // 默认情况下，如果不配置解析表单数据的中间件，则 req.body 默认等于 undefined
  console.log(req.body)
  res.send('ok')
})
 ```

 * express.urlencoded 解析 URL-encoded 格式的请求体数据（有兼容性，仅在 4.16.0+ 版本中可用）
 ```javascript
 //在路由模块或者，应用实例上，绑定中间件
 app.use(express.urlencoded( expended：false ))  // 这个中间件，来解析 表单中的 url-encoded 格式的数据
app.post('/book', (req, res) => {
  // 在服务器端，可以通过 req.body 来获取 url-encoded 格式的数据
  console.log(req.body)
  res.send('ok')
})
 ```

##### 第三方的中间件
>非 Express 官方内置的，而是由第三方开发出来的中间件，叫做第三方中间件。

* 需要安装
* 就是第三方模块的一种变形，用加载模块加载。
* 核心逻辑都封装在模块里
1. 运行 npm install *    安装中间件
2. 使用 require 导入中间件
3. 调用 app.use() 注册并使用中间件


###### CORS解决跨域问题
>cors 是 Express 的一个第三方中间件。通过安装和配置 cors 中间件，可以很方便地解决跨域问题。

1. 运行 npm install cors 安装中间件
2. 使用 const cors = require('cors') 导入中间件
3. 在路由之前调用 app.use(cors()) 配置中间件
4. 
###### CORS原理
>CORS （Cross-Origin Resource Sharing，跨域资源共享）由一系列 HTTP 响应头组成，这些 HTTP 响应头决定浏览器是否阻止前端 JS 代码跨域获取资源。

>浏览器的同源安全策略默认会阻止网页“跨域”获取资源。但如果接口服务器配置了 CORS 相关的 HTTP 响应头，就可以解除浏览器端的跨域访问限制。

* CORS 主要在服务器端进行配置。客户端浏览器无须做任何额外的配置
* 只有支持 XMLHttpRequest Level2 的浏览器，才能正常访问开启了 CORS 的服务端接口
* 其实就是CORS中间件在服务端返回给客户端的http报文头部添加相关的响应头。

	*注意*  ： *必须在配置 CORS 中间件之前声明 JSONP 的接口
###### CORS 响应头部设置
1.  Access-Control-Allow-Origin
```javascript
//在服务端配置res属性

//url指定了允许访问该资源的外域 URL
res.setHeader('Access-Control-Allow-Origin', 'url')
//表示允许来自任何域的请求
res.setHeader('Access-Control-Allow-Origin', '*')
```

2. Access-Control-Allow-Headers
- 默认仅支持客户端向服务器发送如下的 9 个请求头：
```
Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width 、Content-Type （值仅限于 text/plain、multipart/form-data、application/x-www-form-urlencoded 三者之一）
```
* 如果客户端向服务器发送了额外的请求头信息，则需要在服务器端，通过 Access-Control-Allow-Headers 对额外的请求头进行声明

```javascript
//在服务端配置res属性,添加额外的请求头
res.setHeader('Access-Control-Allow-Headers', 'Content-Type, X-Costom-Header')
```

3. Access-Control-Allow-Methods

* 默认情况下，CORS 仅支持客户端发起 GET、POST、HEAD 请求。

```javascript
//在服务端配置res属性，添加额外的请求方法/PUT、DELETE 
res.setHeader('Access-Control-Allow-Methods', 'POST, GET, DELETE, HEAD')
//表示允许来自所有请求方法
res.setHeader('Access-Control-Allow-Methods', '*')
```

###### CORS请求的分类
>客户端在请求 CORS 接口时，根据请求方式和请求头的不同,分为：简单请求，预检请求

- 简单请求（同时满足）
	1. GET、POST、HEAD 三者之一
	2. HTTP 头部信息不超过以下几种字段：无自定义头部字段、Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width 、Content-Type（只有三个值application/x-www-form-urlencoded、multipart/form-data、text/plain）
- 预检请求（符合一个）

	1. 请求方式为 GET、POST、HEAD 之外的请求 Method 类型
	2. 请求头中包含自定义头部字段
	3. 向服务器发送了 application/json 格式的数据

	>在浏览器与服务器正式通信之前，浏览器会先发送 OPTION 请求进行预检，以获知服务器是否允许该实际请求，所以这一次的 OPTION 请求称为“预检请求”。服务器成功响应预检请求后，才会发送真正的请求，并且携带真实数据。



### api接口案例
>基于 MySQL 数据库 + Express 对外提供用户列表的 API 接口服务。用到的技术点如下：
 express、mysql2ES6、模块化、  Promise、  async/await
1. 创建基本的服务器
![[Pasted image 20220619102650.png]]
2. 创建 db 数据库操作模块
![[Pasted image 20220619102703.png]]
3. 创建 user_ctrl 模块
![[Pasted image 20220619102735.png]]
4. 创建 user_router 模块
![[Pasted image 20220619102752.png]]
5. 导入并挂载路由模块
![[Pasted image 20220619102814.png]]
6. 使用 try…catch 捕获异常
![[Pasted image 20220619104527.png]]