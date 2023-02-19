## Ajax概念
> 全称：Asynchronus javasript And XML (异步javascript 和XML)

>在网页中利用XMLHttpRquest对象和服务器进行交互的方式

* 相当于用ajax封装http请求
* 在dom中，利用XMLHttpRequest对象(构造函数)进行数据交互，很不方便 （let obj = newXMLHttpRequest() ）
* 在jQuery里，封装了一系列函数减低使用难度：$.get()  $.post()  $.ajax
* get方式，不安全，数据会显示在url里面，post安全，用来提交数据

## XMLHttpRequest
>是浏览器提供的Javascript对象，简称xhr，用于请求服务器上的资源
>不管是get还是post都可以通过查询字符串的形式传给服务器的，但是方式不同
* get请求通过查询字符串的形式传参于url中
* post请求通过查询字符串的形式或者json的形式，传参于http的body中
* 所以他们两个在服务端的接受方式也不一样
#### xhr的get请求
```javascript
//创建xhr对象
let xhr = XMLHttpResquest()
//调用open函数
xhr.open('GET', 'url')
//调用send函数
xhr.send()
//监听 onreadystatechange 事件
xhr.onreadystatchange = function () {
if(xhr.readyState === 4 && xhr.status === 200)  --固定写法，表示获取成功
console.log(xhr.responseText)  --获取数据，返回的是JSON字符串（或者是普通字符串?）
}
```
##### 带参数的get请求
```javascript
//创建xhr对象
let xhr = XMLHttpResquest()
//调用open函数,并且在？后面添加参数
xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=1&name=xxx')
```
##### 查询字符串（url参数）
```javascript
//?后面跟url参数参数=值&参数=值...
xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=1&name=xxx')
```
### xhr的post请求
* post请求在http的body内，不在url里
```javascript
//创建xhr对象
let xhr = XMLHttpResquest()
//调用open函数
xhr.open('POST', 'url')
//调用Content-Type 属性（固定写法）
xhr.setResquestHeader('Content-Type', 'application/x-xxx-form-urlencoded')
//调用send函数 可以查询字符串的形式提交数据给服务器，也可以json格式传给服务器
xhr.send(data)
//监听 onreadystatechange 事件
xhr.onreadystatchange = function () {
if(xhr.readyState === 4 && xhr.status === 200)  --固定写法，表示获取成功
console.log(xhr.responseText)  --获取数据
}
```

### xhr的readyState属性
>表示当前的ajax请求所处的状态
* 0 - 已创建但没有调用open方法
* 1 - open方法已经调用
* 2 - send方法已经调用，而且已经被服务器接受
* 3 - 数据接受中，response中已经有部分的数据
* 4 - 请求完成，意味着数据请求成功或者失败

## XMLHttpRequest Level2
### 设置HTTP请求时限
```javascript
//当时间超时，就停止发送http请求
xhr.timeout = 3000
//当时间超时，就停止发送http请求，并启用回调函数
xhr.ontimeout = function(event){
     alert('请求超时！')
 }
```
 ### FormData-传输表单数据
 * 可以快捷传输表单数据
 * 其实就是post请求，里面可能封装了表单数据转查询字符串的函数
 1. 手动添加表单的键值对
 ```javascript
// 1. 新建 FormData 对象
      var fd = new FormData()
      // 2. 为 FormData 添加表单项
      fd.append('uname', 'zs')
      fd.append('upwd', '123456')
      // 3. 创建 XHR 对象
      var xhr = new XMLHttpRequest()
      // 4. 指定请求类型与URL地址
      xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata')
      // 5. 直接提交 FormData 对象，这与提交网页表单的效果，完全一样
      xhr.send(fd)       --应该传输查询字符串
      //监听发送状态事件
       xhr.onreadystatechange = function() {}
})
```
 2. 自动获取表单的值
```javascript
 // 获取表单元素
 var form = document.querySelector('#form1')
 // 监听表单元素的 submit 事件
 form.addEventListener('submit', function(e) {
    e.preventDefault()
     // 根据 form 表单创建 FormData 对象，会自动将表单数据填充到 FormData 对象中
     var fd = new FormData(form)
     var xhr = new XMLHttpRequest()
     xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata')
     xhr.send(fd)
     xhr.onreadystatechange = function() {}
})
```
### 上传文件
* 其实就是借助手动上传表单的方式，上传文件
1. 定义 UI 结构
```html
 <!-- 1. 文件选择框 -->
    <input type="file" id="file1" />
    <!-- 2. 上传按钮 -->
    <button id="btnUpload">上传文件</button>
    <br />
    <!-- 3. 显示上传到服务器上的图片 -->
    <img src="" alt="" id="img" width="800" />

```
2. 验证是否选择了文件
```javascript
// 1. 获取上传文件的按钮
 var btnUpload = document.querySelector('#btnUpload')
 // 2. 为按钮添加 click 事件监听
 btnUpload.addEventListener('click', function() {
     // 3. 获取到选择的文件列表
     var files = document.querySelector('#file1').files  --files是数组
     if (files.length <= 0) {
         return alert('请选择要上传的文件！')
     }
     // ...后续业务逻辑
 })
```
3. 向 FormData 中追加文件
```javascript
// 1. 获取上传文件的按钮
 var btnUpload = document.querySelector('#btnUpload')
 // 2. 为按钮添加 click 事件监听
 btnUpload.addEventListener('click', function() {
 // 3. 获取到选择的文件列表
var files = document.querySelector('#file1').files  --files是数组
if (files.length <= 0) {
         return alert('请选择要上传的文件！')
     }
     // 4. 创建 FormData 对象
     var fd = new FormData()
     // 5. 向 FormData 中追加文件
     fd.append('avatar', files[0])
 })
```
4. 使用xhr发起post请求
```javascript
// 1. 获取上传文件的按钮
 var btnUpload = document.querySelector('#btnUpload')
 // 2. 为按钮添加 click 事件监听
 btnUpload.addEventListener('click', function() {
     // 3. 获取到选择的文件列表
     var files = document.querySelector('#file1').files  --files是数组
     if (files.length <= 0) {
         return alert('请选择要上传的文件！')
     }
     // 4. 创建 FormData 对象
     var fd = new FormData()
     // 5. 向 FormData 中追加文件
     fd.append('avatar', files[0])
    // 6. 创建 xhr 对象
     var xhr = new XMLHttpRequest()
     // 7. 调用 open 函数，指定请求类型与URL地址。其中，请求类型必须为 POST
      xhr.open('POST', 'http://www.liulongbin.top:3006/api/upload/avatar')
      // 8. 发起请求
       xhr.send(fd)

 })
```
5. 监听 onreadystatechange 事件
```javascript
// 1. 获取上传文件的按钮
 var btnUpload = document.querySelector('#btnUpload')
 // 2. 为按钮添加 click 事件监听
 btnUpload.addEventListener('click', function() {
     // 3. 获取到选择的文件列表
     var files = document.querySelector('#file1').files  --files是数组
     if (files.length <= 0) {
         return alert('请选择要上传的文件！')
     }
     // 4. 创建 FormData 对象
     var fd = new FormData()
     // 5. 向 FormData 中追加文件
     fd.append('avatar', files[0])
    // 6. 创建 xhr 对象
     var xhr = new XMLHttpRequest()
     // 7. 调用 open 函数，指定请求类型与URL地址。其中，请求类型必须为 POST
      xhr.open('POST', 'http://www.liulongbin.top:3006/api/upload/avatar')
      // 8. 发起请求
       xhr.send(fd)
       //9. 监听事件，取回数据
       xhr.onreadystatechange = function() {
  if (xhr.readyState === 4 && xhr.status === 200) {
    var data = JSON.parse(xhr.responseText)
    if (data.status === 200) { // 上传文件成功
      // 将服务器返回的图片地址，设置为 <img> 标签的 src 属性
      document.querySelector('#img').src = 'http://www.liulongbin.top:3006' + data.url
    } else { // 上传文件失败
      console.log(data.message)
    }
  }
 }
}
```
6. 检测上传进度
```javascript
 // 创建 XHR 对象
 var xhr = new XMLHttpRequest()
 // 监听 xhr.upload 的 onprogress 事件
 xhr.upload.onprogress = function(e) {
    // e.lengthComputable 是一个布尔值，表示当前上传的资源是否具有可计算的长度
    if (e.lengthComputable) {
        // e.loaded 已传输的字节
        // e.total  需传输的总字节
        var percentComplete = Math.ceil((e.loaded / e.total) * 100)
    }
 }
```
7. 检测上传完成
```javascript
 xhr.upload.onload = function() {
     $('#percent')
         // 移除上传中的类样式
         .removeClass()
         // 添加上传完成的类样式
         .addClass('progress-bar progress-bar-success')
 }
```

##  jQuery中的ajax
> 底层还是xhr的封装
1. 先封装一个转换数据为查询字符串的函数
2. 在封装一个执行函数，传入有method，url等参数的对象
3. 执行函数写入if..else if 判断传入的实参是post还是get，然后执行逻辑
4. 然后在xhr的监听函数那里，把服务器传出结果转换为对象，再给success回调函数赋值，传出结果
```javascript
//处理data参数
function resolveData(data) {
  var arr = []
  for (var k in data) {
    arr.push(k + '=' + data[k])
  }
  return arr.join('&')
}
//封装ajax步骤
function itheima(options) {
  var xhr = new XMLHttpRequest()
  // 拼接查询字符串
  var qs = resolveData(options.data)
  
    if (options.method.toUpperCase() === 'GET') {
    // 发起 GET 请求
    xhr.open(options.method, options.url + '?' + qs)
    xhr.send()
  } else if (options.method.toUpperCase() === 'POST') {
    // 发起 POST 请求
    xhr.open(options.method, options.url)
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
    xhr.send(qs)
  }

  // 监听请求状态改变的事件
  xhr.onreadystatechange = function() {
    if (xhr.readyState === 4 && xhr.status === 200) {
      var result = JSON.parse(xhr.responseText)
      options.success(result)
    }
  }
}
```


### $.get() 
语法：
```javascript
$.get(url, [data], [callback]) 地址(字符串)，请求过程中要携带的数据(对象)， 数据返回后的回调函数(形参储存数据)
```
 不带参数：
 ```javascript
$('#btn').on('click', function() {
$.get('http://www.liulongbin.top:3006/api/getbooks', function(res)
{console.log(res)})
}) 
```
带参数（请求id=1的数据）：
 ```javascript
$('#btnadd').on('click', function() {
$.get('http://www.liulongbin.top:3006/api/getbooks', {id: 1}, function(res)
{console.log(res)})
}
```
### $.post()
语法：
```javascript
$.get(url, [data], [callback]) 地址，携带的要发送的数据(对象)， 数据返回后的回调函数(形参储存数据)
```
示例：
```javascript
$('#btnadd').on('click', function() {
$.post('http://www.liulongbin.top:3006/api/getbooks', {id: 2, bookname: '西游记', author: '吴承恩', publisher: '北京图书出版社'}，function(res)
{console.log(res)})
}
```
### $.ajax()
语法：
```javascript
$.ajax({
type: '',  //请求方式get、post
url: '', //地址
data: {}, //这次请求要携带的数据
success: function(res){} //请求成功之后的回调函数
})
```
### JSONP(跨域)
jQuery 采用的是动态创建和移除 < script > 标签的方式，来发起 JSONP 数据请求。
* 在发起 JSONP 请求的时候，动态向 < header > 中 append 一个 < script > 标签；
* 在 JSONP 请求成功以后，动态从 < header > 中移除刚才 append 进去的 < script > 标签；

用 jQuery 发起 JSONP 请求，会自动携带一个 callback=jQueryxxx 的参数，jQueryxxx 是随机生成的一个回调函数名称。
```javascript
$.ajax({
    url: 'http://ajax.frontend.itheima.net:3006/api/jsonp?name=zs&age=20',
    // 如果要使用 $.ajax() 发起 JSONP 请求，必须指定 datatype 为 jsonp
    dataType: 'jsonp',
     jsonp: 'callback',  --修改参数名称，没啥用callback就行
     jsonpCallback: 'abc',  --自定义回调函数的名称
    success: function(res) {
       console.log(res)
    }
 })
```


### 表单
1. 绑定监听函数（提交行为的监听函数）
* 绑定表单提交的监听函数
* 并且停止表单的默认提交行为
2. 获取数据
* $('form标签').serialize()   获取表单所有数据
* 前提是表单元素添加name唯一属性
* name属性的值就是接口参数名称
3. 提交数据，把获取到的值通过ajax传输
```javascript
获取表单form里面所有有name属性的数据
$('表单标签').serialize()
```
## 接口
> url就是数据接口，每一个接口必有请求方式
### URL
http://www.liulongbin.top:3006/api/getbooks?id=1&name=xxx
>由协议名，地址，端口号，参数组成，?后面跟url参数 参数=值&参数=值...
* 当参数中有中文时，要编码成英文
encodeURL('')
decodeURL('')
### 插件postman
* 相当于外部对http请求做的封装，可以发送请求获取接口数据
* 用于测试接口是否有效等
### 接口文档
1. 接口名称：如：登录接口，获取图书目录接口
2. 接口url： 调用接口的地址
3. 调用方式： GET或者POST
4. 参数格式： 接口需要传递的参数（就是数据） 包括：参数名称、参数类型、是否必选、参数说明
5. 响应格式： 接口返回值的描述 包括：数据名称、数据类型、说明
6. 返回示例(可选)：通过对象的形式返回示例

## 模板引擎
* 传统方式添加数据进html需要遍历数据，字符串拼接的方式，添加进元素。
* 模板引擎可以不用字符串拼接，直接用数据和模板合并添加
* 易读易修改
### art-template使用步骤
1. 导入art-template
```html
<script src="template-web.js"></script>
```
2. 定义数据
```javascript
//定义渲染的数据
let data = { name: 'lixiaolong' }
```
3. 定义模板
* 模板一定要写在script标签里
* 但是type属性一定改为html，默认的是Javascript
* 模板里面的标签结构里面添加{{}},占位符，到时候数据会替换里面的参数名称
* 值的不同类型使用占位符的方式也不同，详见[标准语法-输出](#jump)
```html
<script type="text/html"><h1>{{name}}-----{{age}}</h1></script>
```
4. 调用template函数
```html
导入模板
<script src="template-web.js"></script>
定义数据(获取的数据)
<script>let data = { name: 'lixiaolong'， age：20  }</script>
定义模板
<script type="text/html" id="模板的id">
<h1>{{name}}-----{{age}}</h1>
</script>
调用函数
<script>
let template = template('模板的id'， data)
console.log(template) //<h1>lixiaolong-----20</h1>
</script>

```
5. 渲染html结构
* 注意以上的过程都是模板预设，所以需要再渲染到真正的htm结构中
* 只需要把调用template函数的结果用dom添加到真正的html里
### art-template标准语法
 * {{}} 为标准语法，可以进行变量输出或者循环数组的操作
1. <span id = "jump">标准语法-输出</span>
```javascript
{{value}}    //变量的输出
{{@ value}}    //原文输出，当值为html标签时，可以解析html结构
{{obj.key}}   //对象属性的输出
{{obj.['key']}}  
{{a ? b : c}} //三元表达式的输出
{{a || b}}   // 逻辑或的输出
{{a + b}}   //加减乘除表达式的输出
// 循环输出
{{each arr}}   --要循环的数组
 {{$index}} {{$value}}  --返回的索引和值
{{/each}}  
// 条件输出
  {{if value}}
     true后输出的内容
    {{else if value2}}
    true输出后的内容
  {{/if}}
```
2. 标准语法-过滤器
* 定义过滤器函数
* 使用过滤器函数
```javascript
//定义过滤器
template.defaults.imports.filterName（自定义） = function (value) {return 处理的结果}
//使用过滤器函数
{{value | filterName}}
```
### art-template基本原理
>原理就是利用正则与字符串的操作
1. 正则表达式.exec(string)    检索字符串中正则表达式的匹配，此字符串可以识别标签

```javascript
let str = '我是name'
let zhengze = /n/
zhengze.exec(str)  //返回一个数组，0为匹配的字符 否则返回null
```
2. 正则表达式.exec(string)    /{{(正则)}}/   （）表示分组，可以另外提取出来
```javascript
let str = '我是{{name}}'
let zhengze = /{{([a-zA-Z]+)}}/
zhengze.exec(str)  //返回一个数组，0为匹配的字符{{name}} 1为括号里面另外提取的分组name
```
3. 字符串的replace函数
```javascript
let str = '我是{{name}}'
let zhengze = /{{([a-zA-Z]+)}}/
let result = zhengze.exec(str)  //返回一个数组，0为匹配的字符{{name}} 1为括号里面另外提取的分组name
let res = str.replace(result[0], result[1])   字符串再替换提取出来的数组里面的0→1 也就是{{name}}替换成name
```
4. 利用replace替换真值
```javascript
let data = {name: 'zhangshan', age: 20}  
let str = '我是{{name}}'
let zhengze = /{{([a-zA-Z]+)}}/
let result = zhengze.exec(str)  //返回一个数组，0为匹配的字符{{name}} 1为括号里面另外提取的分组name
let res = str.replace(result[0], data[result[1]])   把{{}}替换成data里的真值
```
## 数据交换格式
>服务端和客户端数据交换的格式
###  XML
>可扩展标记语言，和HTML很像
* 用来传输数据比较多 html用来展示页面内容
* 代码臃肿，无用代码多但语义强
* JavaScript解析困难
### JSON
>‘JavaScript Object Notation’ - ‘javascript 对象表示法’
>就是JavaScript 对象和数组的字符串表示法 ， 使用文本表示对象和数组的信息
>本质就是字符串表示js对象和数组数据
* 轻量级文本数据交换格式，用来存储和传输数据
* 属性名、字符串的值必须使用双引号包裹
* JSON 的最外层必须是对象或数组格式
* 不能使用 undefined 或函数作为 JSON 的值
在JS中对象的序列化通常是将一个对象转换为字符串（JSON字符串）

- 序列化的用途（对象转换为字符串有什么用）：
	- 对象转换为字符串后，可以将字符串在不同的语言之间进行传递
	- 甚至人可以直接对字符串进行读写操作，使得JS对象可以不同的语言之间传递

- 用途：
1. 作为数据交换的格式
2. 用来编写配置文字

- 编写JSON的注意事项：
	1. JSON字符串有两种类型：JSON对象 {}，JSON数组 []
	2. JSON字符串的属性名必须使用双引号引起来
	3. JSON中可以使用的属性值（元素）
		- 数字（Number）
		- 字符串（String） 必须使用双引号
		- 布尔值（Boolean）
		- 空值（Null）
		- 对象（Object {}）
		- 数组（Array []）
	4. JSON字符串如果属性是最后一个，则不要再加,
```javascript
//本质就是js里面的字符串，只是遵循了json的语法
var json = '{"a": "Hello", "b": "World"}' 
```
#### JSON-JS的转换
1. 从 JSON 字符串转换为 JS 对象  --反序列化

```javascript
var obj = JSON.parse('{"a": "Hello", "b": "World"}')
//结果是 {a: 'Hello', b: 'World'}
```
2. 从 JS 对象转换为 JSON 字符串 --序列化
```javascript
var json = JSON.stringify({a: 'Hello', b: 'World'})
//结果是 '{"a": "Hello", "b": "World"}'
```
#### JSON的对象结构
>对象结构在 JSON 中表示为 { } 括起来的内容。数据结构为 { key: value, key: value, … } 的键值对结构。其中，key 必须是使用英文的双引号包裹的字符串，value 的数据类型可以是数字、字符串、布尔值、null、数组、对象6种类型。
```
{
    "name": "zs",
    "age": 20,
    "gender": "男",
    "address": null,
    "hobby": ["吃饭", "睡觉", "打豆豆"]
}

```
#### JSON的数组结构
>数组结构在 JSON 中表示为 [ ] 括起来的内容。数据结构为 [ "java", "javascript", 30, true … ] 。数组中数据的类型可以是数字、字符串、布尔值、null、数组、对象6种类型。
```
[ "java", "python", "php" ]
[ 100, 200, 300.5 ]
[ true, false, null ]
[ { "name": "zs", "age": 20}, { "name": "ls", "age": 30} ]
[ [ "苹果", "榴莲", "椰子" ], [ 4, 50, 5 ] ]
```
## axios
* Axios 是专注于网络数据请求的库。
* 相比于 jQuery，axios 更加轻量化，只专注于网络数据请求。
* 都是xhr的封装，需要引入库 axios.js
### axios发起GET请求
语法：
```javascript
axios.get('url', { params: { /*参数*/ } }).then(callback)
```

```javascript
 // 请求的 URL 地址
 var url = 'http://www.liulongbin.top:3006/api/get'
 // 请求的参数对象
 var paramsObj = { name: 'zs', age: 20 }
 // 调用 axios.get() 发起 GET 请求
 axios.get(url, { params: paramsObj }).then(function(res) {
     // res.data 是服务器返回的数据
     var result = res.data
     console.log(res)
 })
 ```

### axios发起POST请求
语法：
```javascript
 axios.post('url', { /*参数*/ }).then(callback)
```

```javascript
 // 请求的 URL 地址
 var url = 'http://www.liulongbin.top:3006/api/post'
 // 要提交到服务器的数据
 var dataObj = { location: '北京', address: '顺义' }
 // 调用 axios.post() 发起 POST 请求
 axios.post(url, dataObj).then(function(res) {
     // res.data 是服务器返回的数据
     var result = res.data
     console.log(result)
 })
 ```
 ### axios发起axios请求
 ```javascript
  axios({
     method: '请求类型',
     url: '请求的URL地址',
     data: { /* POST数据 */ },
     params: { /* GET参数 */ }
 }) .then(callback)
```
## 同源策略
>如果两个地址的请求的协议，域名和端口都相同，则两个页面具有相同的源。

>同源策略（英文全称 Same origin policy）是浏览器提供的一个安全功能。
* 就是客户端对服务端，客户端对客户端的数据交互，浏览器自己阻拦自己请求的非同源的数据。
通俗的理解：浏览器规定，A 网站的 JavaScript，不允许和非同源的网站 C 之间，进行资源的交互，例如：
1. 无法读取非同源网页的 Cookie、LocalStorage 和 IndexedDB
2. 无法接触非同源网页的 DOM
3. 无法向非同源地址发送 Ajax 请求
* 就是有用同一协议，同一服务器，同一端口的地址可以互相通信
* 如果网页地址与请求ajax地址不同源，也不能通信。（地址同源）
## 跨域
>同源指的是两个 URL 的协议、域名、端口一致，反之，则是跨域。
* JSONP：出现的早，兼容性好（兼容低版本IE）。是前端程序员为了解决跨域问题，被迫想出来的一种临时解决方案。缺点是只支持 GET 请求，不支持 POST 请求。
* [[Node.js#cors中间件解决跨域问题|CORS]]：出现的较晚，它是 W3C 标准，属于跨域 Ajax 请求的根本解决方案。支持 GET 和 POST 请求。缺点是不兼容某些低版本的浏览器。
### JSONP
> JSONP (JSON with Padding) 是 JSON 的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题。

 ####  JSONP的实现原理
 
 >JSONP 的实现原理，就是通过 < script > 标签的 src 属性，请求跨域的数据接口，并通过函数调用的形式，接收跨域接口响应回来的数据。
* 就是通过script标签调用后台封装的一个js文件，那里面执行了html里面的回调函数，并且通过实参回传了数据
* 因为用src请求的js接口，所以是js脚本（不是xhr）发起的get请求(只能get请求)才能让地址传给跨域服务器，服务器在传给回调函数数据
* 逻辑上应该不是直接通过get传的数据，数据还是通过实参的形式传输的
    *注意*  ： *必须在配置 CORS 中间件之前声明 JSONP 的接口
1. 在客户端
```html
 <script>
 //定义一个 success 回调函数等待回调：
   function success(data) {
     console.log('获取到了data数据：')
     console.log(data)
   }
 </script>
<script>
//通过 < script > 标签，请求接口数据：
<script src="http://ajax.frontend.itheima.net:3006/api/jsonp?callback=success&name=zs&age=20"> --在callback属性中定义回调函数名字
</script>
```
![[Pasted image 20220607221554.png]]
2. 在服务端
![[Pasted image 20220607221045.png]]
## 防抖策略
>防抖策略（debounce）是当事件被触发后，延迟 n 秒后再执行回调，如果在这 n 秒内事件又被触发，则重新计时。
* 就是只执行最后一次触发的函数
```javascript
 var timer = null                    // 1. 防抖动的 timer.只能写外边不然，其他作业域不能用它

 function debounceSearch(kw) { // 2. 定义防抖的函数
    timer = setTimeout(function() {
    // 发起 JSONP 请求
    getSuggestList(kw)
    }, 500)
 }

 $('#ipt').on('keyup', function() {  // 3. 在触发 keyup 事件时，立即清空 timer
    clearTimeout(timer)
    //重新调用防抖函数，并且实参的形式传参数
    debounceSearch(keywords)
 })

```
### 优化
```javascript
  // 缓存对象
  var cacheObj = {}

 // 渲染建议列表
 function renderSuggestList(res) {
    // ...省略其他代码
    // 将搜索的结果，添加到缓存对象中
    var k = $('#ipt').val().trim()
    cacheObj[k] = res
 }
  // 监听文本框的 keyup 事件
 $('#ipt').on('keyup', function() {
    // ...省略其他代码
    // 优先从缓存中获取搜索建议
    if (cacheObj[keywords]) {
       return renderSuggestList(cacheObj[keywords])
    }
    // 获取搜索建议列表
    debounceSearch(keywords)
  })


```
## 节流策略
>节流策略（throttle），顾名思义，可以减少一段时间内事件的触发频率。
>就是添加一个节流阀，用延时器来触发事件
```javascript
$(function() {
  var angel = $('#angel')
  var timer = null // 1.预定义一个 timer 节流阀
  $(document).on('mousemove', function(e) {
    if (timer) { return } // 3.判断节流阀是否为空，如果不为空，则证明距离上次执行间隔不足16毫秒
    timer = setTimeout(function() {
      $(angel).css('left', e.pageX + 'px').css('top', e.pageY + 'px')
      timer = null // 2.当设置了鼠标跟随效果后，清空 timer 节流阀，方便下次开启延时器
    }, 16)
  })
})

```
* 防抖：如果事件被频繁触发，防抖能保证只有最有一次触发生效！前面 N 多次的触发都会被忽略！
* 节流：如果事件被频繁触发，节流能够减少事件触发的频率，因此，节流是有选择性地执行一部分事件！
