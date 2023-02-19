## 问题集
[[Ajax#FormData-传输表单数据|问题-01]]
- xhr.send()  明明发送的是查询字符串，为什么formData对象可以传进去？
- post请求到底可以传递什么数据类型进去，传出来的又是什么数据类型？
 

[[Ajax#同源策略|问题-03]]
- 同源策略不能获取不同源的地址，为什么案例中自己试验却可以调用给的接口呢?

[[Node.js#CORS 响应头部设置|问题-06]]
- CORS跨域策略，原理是服务端设置响应头，但是跨域不是浏览器不接受你吗，你要配置什么样的响应头才能让浏览器接受你呢？

[[Database#Session 的工作原理|问题-07]]
- cookie和Session 在原理上有什么不同？

[[JS#封装读取文件的方法|问题-09]]
- .then()里传的两个回调函数，是怎么变为promise里的回调函数的实参的呢，封装到原型对象上？

[[Webpack#js降级加载器|问题-10]]
- 降级加载器是怎么选择降到哪一级的，在降级规则里？

[[Webpack#webpack开发服务器|问题-11]]
- 开发服务器是用来监看实时打包的页面的吗，自动打包出来的文件在哪里呢？存在服务器里？我怎么用呢？（vue脚手架同理，不用build步骤吗）

[[VUE#vue指令 v-model|问题-12]]
- 表单与变量的双向绑定，为什么多选，单选，一定要选择后才能绑定呢，如：只有你选择某个多选框，变量里才会得到你选定的value值，其他复选框的值就得不到？

[[VUE#Vuex的模块化#子模块的命名空间|问题-13]]
- 为什么子模块的state状态到底在谁的目录下面。如果mutations等状态都是全局共享的，那么子模块的state状态里的值也就可以随意进出，通过子getters暴露出去，为何还要用根getters来快速访问

[[VUE#refs-获取原生DOM|问题-14]]
- this.$refs.name 根本获取不到原生dom，this. $refs['name'] 这样才能获取

问题-15
- <router-view />到底是不是必须的。
- 为什么在app.vue里， 没有在路由中挂载app的子路由， 却可以让各个子组件渲染在app.vue中？

问题-16
	- createElement函数的用法中h()里面的参数，为啥可以直接传入一个组件就可以渲染
```js
import App from './App.vue'
new Vue({
	 render:h=>h(App)      //用render选项来渲染组件
}).mount('#app')  //然后挂载到#app的容器中，用el选项也能挂载
	// h() =====>  createElement()， 默认形参给render函数
 //h(App) =====>  根据APP组件创建html结构，给render返回
	 //render的返回值就是html结构，渲染到#app容器
	// h() 函数参数，1.节点名称  2. 属性|数据 是对象  3. 子节点
```

问题-17 
- 原型对象中的prototype属性是什么， 原型对象的constructor属性指向构造函数， 构造函数里面的prototype属性指向原型对象， 为什么会原型对象中有一个prototype属性呢， 对象中不应该是__proto__属性指向构造函数的原型对象吗？

## 小技巧
### 1.  css的动画过渡延迟
> css的transition效果如果在元素创建时执行时， 它会延迟执行。 所以可以给class添加事件为0的定时器， 让它最后执行。

### 2. Es6判断语法糖
```js
  emit('change', showAddress.value?.id) //相当于先判断， 然后再取值
```

### 3. js算法库
https://github.com/trekhleb/javascript-algorithms/
### 4. 原生html事件立即调用
- 在原生标签中， 如果事件后面的函数带括号，就会立即调用， 但是 可以写回调函数，箭头函数，这样就可以等激活了再调用。
### 5. vue3 的组合api的优势
- 组合api可以封装函数在export default 外， 然后用export按需导出， 其他组件想用此函数，直接按需引用就好，但是选项api不行，因为所有东西都是写在export default里面， 不能导出。
```js
import { useCancel, useConfirm } from '../index.vue'
setup (props) {

// console.log(props.order)

return { orderStatus, ...useCancel(), ...useConfirm() }  //引用按需导入的方法函数

},
```
### 6. 类数组的判断

1. 验证原型对象
Object.getPrototypeOf(arr) == Array.prototype
判断arr的原型是否为Array.prototype，返回结果为Boolean类型
其中Object.getPrototypeOf()是返回对象的原型，里面的参数是一个对象
```js
   // 或者判断arr的父对象是否为Array
 Array.prototype.isPrototypeOf(arr) //返回结果为Boolean类型
```
  

2. 验证构造函数

```js
arr.constructor == Array 
//判断arr的构造函数的原型是否为Array，返回值为Boolean类型
// 在原型链上 obj.__proto__ == fun.prototype（fun 为构造函数 ）
// 而在obj上的constructor属性，里面的属性值为构造函数
arr.instanceof Array   //返回值为Boolean类型
```
或者：arr.instanceof Array — instanceof 判断是否是数组，返回值为Boolean类型
但验证构造函数方法不够严谨，当改变了arr的原型，即令arr.__proto__ = Object，则此时的返回值永远为false


3. toString方法
原型链的顶层为Object.prototype = null,内置对象都会重写父级原型中的toString的方法，使用强制转换apply、call、bind，改变this的指向，来判断此时对象的类型属性

```js
Object.prototype.toString.call(arr)
//返回值为“[Object arr类型属性]”,是一个字符串
```


4. isArray方法
这是ES5中的方法
```js
Array.isArray（arr）
//返回值为boolean类型
```

### 7. props父传子的值， 取不到
- 有时候props父传子的值命名传进去了，但是视图就是取不到。 明明修改过的值， 就是视图取不到。
- 这时候就可以用watch监听，props的值

### 8.transition可以用在哪里?
	1. 用在routerview外边
	2. 用在v-if， v-show外边

### 9.vue2中的路由重置
> 路由重置主要是重置添加的路由， 路由可以通过， router.addRoutes()方法 添加，      
- 在路由index中添加方法
```js
const createRouter = () => new Router({

// mode: 'history', // require service support

base: 'hr/',

scrollBehavior: () => ({ y: 0 }),

routes: [...constantRoutes]

})

export function resetRouter() {

const newRouter = createRouter()   //这里相当于重新实例化了一次路由实例，把新实例的matcher属性赋值给了router的matcher， 估计matcher就是动态路由的存放地址， 修改后就相当于重置。

router.matcher = newRouter.matcher // reset router

}
```
- 当用的时候直接引入就行
## 方法集
### 通用方法集

#### yarn serve / build 不启动解决方案
- 主要原因是没有在package.json里面的scripts,属性里设置，快捷访问
```json
"scripts": {

"serve": "vue-cli-service serve",

"dev": "vue-cli-service serve",

"build:prod": "vue-cli-service build",
```

#### vue网站名称修改
- 在vue.config.js配置文件里

```js

configureWebpack: {

// provide the app's title in webpack's name field, so that

// it can be accessed in index.html to inject the correct title.

name: name,

resolve: {

alias: {

'@': resolve('src')

}

}

},

```

#### vue里本地访问端口修改
- webpack修改本地访问端口，在webpack的配置文件config里。但是vue本身就是webpack封装的，所以在vue.config.js的devServe属性里。

- 有两个文件.env.development / .env.production 是环境变量的配置文件。可以在这两个文件里直接设置变量。可以用 process.env.变量名的方式引用（具体引用那个环境变量，取决于启动的模式）。
```js
const port = process.env.port || process.env.npm_config_port

devServer: {

port: port,

open: true,

overlay: {

warnings: false,

errors: true

}
```

#### 怎么给node降级？
1. 安装node版本管理模块n

```shell
sudo npm install n -g
```

2. 安装稳定版
```shell
sudo n stable
```


3. 安装最新版
```shell
sudo n latest
```

4. 版本降级/升级
```shell
sudo n 版本号     //例如：sudo n 9.1.7

```


#### - 怎么解构变量
- 通过解构变量，获得数据对象深层的数据
- 一般获取的数据最外层就是data，所以其实第二层的data才是获取对象里面的data属性
```js
async getCatagtory(context){

const {data: {data: { channels }}} = await 
axios.get('http://ttapi.research.itcast.cn/app/v1_0/channels')
```

#### - rem适配方法 
-   [[头条-移动端#移动端 REM 适配|postcss-pxtorem]] 是一款 PostCSS 插件，用于将 px 单位转化为 rem 单位
-   lib-flexible 用于设置 rem 基准值
>两个相互配合，一个设置基准值，基准值根据宽度的十分之一变化，然后把px转化为rem，rem因为前面都随宽度变化了，所有所有的px也都是随着宽度的变化而变化。

注意：行内样式的px不能转化为rem。

#### - git拒绝推送问题
- 原因：在网页进行了push提交动作。

-  解决方法一：强行上传

强制上传输入命令： git push -u origin +master

 - 解决方法二：先拉取后推送
先同步git/github上的代码到本地，在上面更改将内容进行合并后再上传

```shell
git fetch origin https://github.com/yhlleo/myGitTest.git
 
git merge origin/master //获取远程更新
 
git push origin master  //把更新的内容合并到本地分支
```


#### - 怎么关闭代码检查
 -  关闭-进入vue.config.js
```javascript
  module.exports = {
    // ...其他配置
    lintOnSave: false // 关闭eslint检查
  }
//重启服务器
```
#### -  AXISO请求拦截器设置token
>设置请求拦截器可以不用再封装token，请求时候请求拦截器可以自动携带
```js
//给要发送请求的模块设置拦截器
request.interceptors.request.use(function (config) {

// Do something before request is sent
//config是本次请求的请求配置对象

const {user} = store.state

if(user && user.token){

config.headers.Authorization = `Bearer ${user.token}`

}

return config;

}, function (error) {

// Do something with request error

return Promise.reject(error);

});
```

#### - axiso接收拦截器
![[image-20200812020656210.png]]
```js

// 响应拦截器

service.interceptors.response.use(response => {

// axios默认加了一层data

const { success, message, data } = response.data

// 要根据success的成功与否决定下面的操作

if (success) {

return data

} else {

// 业务已经错误了 还能进then ? 不能 ！ 应该进catch

Message.error(message) // 提示错误消息

return Promise.reject(new Error(message))

}

}, error => {

Message.error(error.message) // 提示错误信息

return Promise.reject(error) // 返回执行错误 让当前的执行链跳出成功 直接进入 catch

})

```

####  - scoped模式
>想要解决这个问题，就要添加deep属性,因为这个模式不能改变引入样式的子级样式，需要加deep,但是根样式还是可以改变的

1. 第一种：加上父级自己定义的类名`.common-container >>>` （只作用于css）

```vue
<style lang="scss" scoped>
	.common-container >>> .van-pull-refresh__head {
	  color: #fff;
	}
</style>
```

2. 第二种：有些Sass 、Less之类的预处理器无法正确解析 `>>>`。可以使用 `/deep/`操作符或 `::v-deep` 操作符( 两者都是`>>>` 的别名)

```vue
<style lang="scss" scoped>
	/deep/ .van-list__error-text, 
	/deep/ .van-list__finished-text,  
	/deep/ .van-list__loading ,
	/deep/ .van-pull-refresh__head ,
	/deep/ .van-loading__text {
	  color: #fff;
	}
	
	.common-container{
    &::v-deep .van-pull-refresh__head{
     	color: #fff;
        // ...
    }
}

</style>
```

#### -  Referer 请求头怎么取消
>Referer 是 HTTP 请求头的一部分，当浏览器向 Web 服务器发送请求的时候，一般会带上 Referer，它包含了当前请求资源的来源页面的地址。服务端一般使用 Referer 请求头识别访问来源，可能会以此进行统计分析、日志记录以及缓存优化等。
- 停止referer请求头
-  `<a>`、`<area>`、`<img>`、`<iframe>`、`<script>` 或者 `<link>` 元素上的 `referrerpolicy` 属性为其设置独立的请求策略，例如：
  
```html

<img src="http://……" referrerPolicy="no-referrer">

```

或者直接在 HTMl 页面头中通过 meta 属性全局配置：

```html

<meta name="referrer" content="no-referrer" />
```

#### -  Day.js时间第三方库
>Day.js是一个极简的JavaScript库，可以为现代浏览器解析、验证、操作和显示日期和时间。

```shell
npm install dayjs --save
```

```js
import dayjs from 'dayjs'

import 'dayjs/locale/zh-cn'

dayjs.locale('zh-cn')  //全局注册
```

#### - Lodash-JS工具函数库
```shell
npm i --save lodash
``` 

1. 防抖函数
- 按需加载需要的工具函数
```js
_.debounce(func, [wait=0], [options=])
//debounce(业务函数， 等待时间， 可选项)
```

==example
```js
debounce( async function(val) {

try{

const { data } = await getSearchSuggestions(val)

this.suggestions = data.data.options

}catch(err){

this.$toast('获取推荐失败')

}

  

}, 1000),
```
#### - js里的数字大小受限问题
>用json-bigint第三方包,可以解决js处理数据只能识别 2^52的整数

1. 安装
```
yarn add json-bigint
```

2. 在axios请求模块中引入
```js
import JSONBig from 'json-bigint'
```

- JSONBig方法

```js
//这个方法会把超出安全范围的数字封装在bignumber的对象里，想要用的话，就把这个对象.tostring(),转为字符串就可以正确显示了。  
JSONBig.parse() //JSON字符串转成js对象  

//用 JSONBig.parse() 转的js对象，一定要用JSON.Big.Stringify() 转回JSON字符串
JSONBig.Stringify()//js对象转成JSON字符串
```

3. 在axios请求模块中添加配置： 改变接收原始数据的方法
```js
const request = axios.create({

baseURL: 'http://toutiao.itheima.net/',

//在请求模块拦截接收的数据，手动用JSONBig转换接收的JSON字符串（默认会给我们用json.parse()的方法）
transformResponse: [function (data) {

// Do whatever you want to transform the data

try{

return JSONBig.parse(data)

  

} catch(err) {

  

return data

}

}]

})
```
- 小问题： 非安全数字转为js对象时，会被封装成bignumber对象，所以有时要用tostring转换
- 但是，比如拼接字符串时，会自动把对象转换为字符串，所以不用转换，比如url拼接.

#### less自动导入全局样式问题

-   在当前项目下执行一下命令`vue add style-resources-loader`，添加一个vuecli的插件
-   安装完毕后会在`vue.config.js`中自动添加配置，如下：

```js
module.exports = {
  pluginOptions: {
    'style-resources-loader': {
      preProcessor: 'less',
      patterns: []
    }
  }
}
```

-   把你需要注入的文件配置一下后，重启服务即可。

```js
+const path = require('path')  //比拼接好，会解析../../
module.exports = {
  pluginOptions: {
    'style-resources-loader': {
      preProcessor: 'less',
      patterns: [
+        path.join(__dirname, './src/assets/styles/variables.less'),
+        path.join(__dirname, './src/assets/styles/mixins.less')
      ]
    }
  }
}
```

**总结：** 知道如何定义less变量和混入代码并使用他们，通过vue-resources-loader完成代码注入再每个less文件和vue组件中。

#### 公共样式的重置插件
执行 `npm i normalize.css` 安装重置样式的包，然后在 `main.js` 导入 `normalize.css` 即可。

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

+import 'normalize.css'

createApp(App).use(store).use(router).mount('#app')
``` 

#### 图片转为base64
- 需要配置10kb下的图片打包成base64的格式 `vue.config.js`

```js
  chainWebpack: config => {
    config.module
      .rule('images')
      .use('url-loader')
      .loader('url-loader')
      .tap(options => Object.assign(options, { limit: 10000 }))
  }
```

#### 当vue的@事件获取不同参数的方法？
- 当我们的@事件()想要传参时， 有时参数在组件里面，需要@事件 = 事件名 获取， 又想要组件外的参数， 用@事件 = 事件名() 获取。 怎么同时获取呢?

- 通过箭头函数的方法，可以得到组件外的参数，也可以用$event获得组件里面传出来的参数。
```html
<XtxCheckbox @change="($event) => checkOne(good.skuId, $event)" :modelValue="good.selected" />
```

#### mock数据
>从 1.0 开始，Mock.js 通过覆盖和模拟原生 XMLHttpRequest 的行为来拦截 Ajax 请求，不再依赖于第三方 Ajax 工具库（例如 jQuery、Zepto 等）。
![[Pasted image 20221122213131.png]]

>`mockjs`可以模拟可更快的得到较为真实的数据，且可以拦截axios的接口调用
>就是按照正常的方式请求数据， 然后mock可以拦截我们发出去的请求替换为mock数据

1.  配置 `src/mock/index.js`
```js
import Mock from 'mockjs'

// mock的配置
Mock.setup({
  // 随机延时500-1000毫秒  模拟网络延迟
  timeout: '500-1000'
})
```

2.  使用 `src/main.js`
```js
import './mock'  //先使用mock
```

3. 模拟接口，拦截请求

```js
import Mock from 'mockjs'
import qs from 'qs'
// 拦截请求，
// 第一个参数：url，使用正则去匹配
// 第二个参数：请求方式
// 第三个参数： 生成模拟数据模板（可以是字符串、对象、数组、或者函数返回数据）

Mock.mock(/\/member\/collect/, 'get', config => {

const queryString = config.url.split('?')[1]

const queryObject = qs.parse(queryString)  //qc 解析模板字符串为对象

const items = []

for (let i = 0; i < +queryObject.pageSize; i++) {  //模拟pageSize条数据

items.push(Mock.mock({

id: '@id',

name: '@ctitle(10,20)',

desc: '@ctitle(4,10)',

price: '@float(100,200,2,2)',

// http://zhoushugang.gitee.io/erabbit-client-pc-static/uploads/clothes_goods_7.jpg

picture: `http://zhoushugang.gitee.io/erabbit-client-pc-static/uploads/clothes_goods_${Mock.mock('@integer(1,8)')}.jpg`

}))

}

return {

msg: '获取收藏商品成功',

result: {

counts: 35,

pageSize: +queryObject.pageSize,

page: +queryObject.page,

items

}

}

})
//config可以接收前端返回来的数据， body， type， url， 注意， get发送的query在url上， 需要解析成对象才能使用， node中有一个内置函数qc， 可以解析模板字符串

```

4. 占位符- 随机数
- 在模拟的数据模板中， 可能需要随机写入预设好的模板，官方预设了一些占位符， 可以快速写入模拟数据
- 但是需要Mock.mock()才能触发， 所以想要使用， 需要在函数里面返回`Mock.mock()`返回的结果
```js

Mock.mock('@CONSTELLATION')
// => "天蝎座"
Mock.mock({
    constellation: '@CONSTELLATION'
})
```
#### 编辑器第三方库

组件依赖：[wangEditor](https://www.wangeditor.com/)

安装依赖： `npm i wangeditor --save`


#### vue环境变量设置

 >`process.env` 是 `Node.js` 中的一个环境对象。其中保存着系统的环境的变量信息。可使用 `Node.js` 命令行工具直接进行查看。
 
 >在 `Vue` 中， `NODE_ENV` 可以通过 `.env` 文件或者 `.env.[mode]` 文件配置。配置过后，运行 `Vue CLI` 指令（ `npm run dev(serve)` ，`npm run build` ）时，就会将该模式下的`NODE_ENV`载入其中了。而这些命令，都有自己的默认模式：

-   `npm run dev(serve)` ，其实是运行了 `vue-cli service serve` ，默认模式为 development 。可以在 `.env.development` 文件下修改该模式的 `NODE_ENV` 。
-   `npm run build` ，其实运行了 `vue-cli service build` ，默认模式为 production 。可以在 `.env.production` 文件下修改该模式的 `NODE_ENV` 。

- 因为procss.env 只能在node下查看， 所以不能直接在vue中使用， 所以使用这个node的全局环境变量有几种方法。
	1. 就像前面说的在.env.的文件中定义procss.env 的值
	2. 在package.json的发包设置中（scripts）添加NODE_ENV=development键值对，其实还是node自己修改的一种方式
	3. 安装cross-env， 但是也需要再package.json的发包设置中（scripts）添加NODE_ENV=development键值对， 但是vue-cli3已经自动配置好了， 不用自己添加
### 头条项目方法集

#### input的file类型标签怎么获取上传的图片？
>input的type属性可以设置输入类型。当上传图片时，怎么获取图片呢

1. 监听input的change事件，当文件一上传，改变input数据，就可以调用
2. 获取file的原生dom并且调用files数组，其中索引[0]，获得图片对象
```js
onFileChange() {

const file = this.$refs.file.files[0]  //此处获得图片对象
  

this.img = window.URL.createObjectURL(file) //由图片对象转换成了一个获取图片的url地址，


this.isUpdatePhotoShow = true


this.$refs.file.value = ''   //清空file的dom里的值，里面就不会存放图片的数据了，此处只是获取了img地址，input并不用于提交上传。获取过之后就可以重置了
```

3. 用window方法生成图片URL地址，这个地址，就是图片的src属性的值
```html
<img class="img" :src="img">
```

#### 怎么用 markdown 样式外观
>第三方包，github-markdown-css 

1. 安装

```shell
yarn add github-markdown-css
```
		
1. 引入css样式
2. 手动添加类名实现

1. 不安装
	1. 直接在github复制css文件 
	2. 引入css文件
	3. 手动添加类名实现（可以自定义）


#### 裁切图片上传的两种方法
>裁切图片并上传服务端，有两种方法。一种是客户端裁切好，直接传给服务端图片对象，一种是客户端获取裁切数据，传给服务端。

1. 初始化
- 需要用到一个js第三方包  [[https://github.com/fengyuanchen/cropperjs | 官方文档  ]]
```shell
yarn add cropperjs
```
- 引入配置文件
```js
import 'cropperjs/dist/cropper.css'
import Cropper from 'cropperjs'
```
- 配置图片设置，可以自定义
```js
const image = document.getElementById('image');  //获取图片dom 一般放mounted钩子函数里面
const cropper = new Cropper(image, {
    
    //这些是配置项
    aspectRatio: 16 / 9,
    viewMode: 1,
	dragMode: 'move',
    autoCropArea: 1,
	cropBoxMovable: false,
	cropBoxResizable: false,
	background: false
	
   //这些是图片裁切的数据
  crop(event) {
    console.log(event.detail.x)
    console.log(event.detail.y)
    console.log(event.detail.width)
    console.log(event.detail.height)
    console.log(event.detail.rotate)
    console.log(event.detail.scaleX)
    console.log(event.detail.scaleY)

  },
});
```

2. 方法一： 基于服务端的裁切
	- 通过getdata方法，获取裁切数据

```js
const image = document.getElementById('image');  //获取图片dom 一般放mounted钩子函数里面
const cropper = new Cropper(image, { 
    //这些是配置项
    aspectRatio: 16 / 9,
    viewMode: 1,
	dragMode: 'move',
    autoCropArea: 1,
	cropBoxMovable: false,
	cropBoxResizable: false,
	background: false

});

 this.cropper.getdata()  //得到的就是裁剪的数据，直接给后端
```

3. 方法二： 基于客户端的裁切（pc端慎用）
	-  通过getCroppedCanvas方法，获取裁切图片对象
```js
  
this.cropper.getCroppedCanvas().toBlob((blob) => {  //这个blob就是图片对象，和前面input哪里获取的一样。

const formData = new FormData();   //图片对象必须封装在表单对象中传，需要构造表单对象
formData.append('photo', blob)  //在表单中添加数据

//然后直接把formData对象直接传给请求方法

```


### HR中台项目方法集
#### elementUI（同van库）表单校验
##### 实现表单基本结构

**创建项目**
  
```bash

$ vue create login

```
 

> 选择babel / eslint

**安装Element**

开发时依赖 ： 开发环境所需要的依赖 -> devDependencies
  
运行时依赖: 项目上线依然需要的依赖 -> dependencies


```bash

$ npm i element-ui

```

  
**在main.js中对ElementUI进行注册**

```js

import ElementUI from 'element-ui';

import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI);

```

  
接下来,利用Element组件完成如图的效果

```vue

<template>

<div id="app">

<!-- 卡片组件 -->

<el-card class='login-card'>

<!-- 登录表单 -->

<el-form style="margin-top: 50px">

<el-form-item>

<el-input placeholder="请输入手机号"></el-input>

</el-form-item>

<el-form-item>

<el-input placeholder="请输入密码"></el-input>

</el-form-item>

<el-form-item>

<el-button type="primary" style="width: 100%">登录</el-button>

</el-form-item>

  

</el-form>

</el-card>

</div>

</template>

  

<script>

  

export default {

name: 'App',

components: {

  

}

}

</script>

  

<style>

#app {

width: 100%;

height: 100vh;

background-color: pink;

display: flex;

justify-content: center;

align-items: center;

}

.login-card {

width: 440px;

height: 300px;

}

</style>

  

```


##### 表单校验的先决条件

接下来，完成表单的校验规则如下几个先决条件

**model属性** (表单数据对象)


```js

data () {

// 定义表单数据对象

return {

loginForm: {

mobile: '',

password: ''

}

}

}

```


**绑定model**


```vue

<el-form style="margin-top:40px" :model="loginForm" >

```


**rules规则** 先定义空规则，后续再详解


```js

loginRules: {}

<el-form style="margin-top: 50px" model="loginForm" :rules="loginRules">

```


**设置prop属性**


> 校验谁写谁的字段

```vue

<el-form-item prop="mobile">

...

<el-form-item prop="password">

...

```

  
**给input绑定字段属性**


```vue

<el-input v-model="loginForm.mobile"></el-input>

<el-input v-model="loginForm.password"></el-input>

```


##### 表单校验规则

此时，先决条件已经完成，要完成表单的校验，需要编写规则

> ElementUI的表单校验规则来自第三方校验规则参见 [async-validator](https://github.com/yiminghe/async-validator)


我们介绍几个基本使用的规则

  

| 规则 | 说明 |

| --------- | ------------------------------------------------------------ |

| required | 如果为true，表示该字段为必填 |

| message | 当不满足设置的规则时的提示信息 |

| pattern | 正则表达式，通过正则验证值 |

| min | 当值为字符串时，min表示字符串的最小长度，当值为数字时，min表示数字的最小值 |

| max | 当值为字符串时，max表示字符串的最大长度，当值为数字时，max表示数字的最大值 |

| trigger | 校验的触发方式，change（值改变） / blur （失去焦点）两种， |

| validator | 如果配置型的校验规则不满足你的需求，你可以通过自定义函数来完成校验 |

  
校验规则的格式 

***{ key(字段名): value(校验规则) => [{}] }***


根据以上的规则，针对当前表单完成如下要求

**手机号** 1.必填 2.手机号格式校验 3. 失去焦点校验

**密码** 1.必填 2.6-16位长度 3. 失去焦点校验


**规则如下**


```js

loginRules: {

mobile: [{ required: true, message: '手机号不能为空', trigger: 'blur' },

{ pattern: /^1[3-9]\d{9}$/, message: '请输入正确的手机号', trigger: 'blur' }],

password: [{ required: true, message: '密码不能为空', trigger: 'blur' }, {

min: 6, max: 16, message: '密码应为6-16位的长度', trigger: 'blur'

}]

}

```

##### 自定义校验规则


> 自定义校验规则怎么用
  
**`validator`**是一个函数, 其中有三个参数 (**`rule`**(当前规则),`value`(当前值),**`callback`**(回调函数))

```js
data() {

var func = function (rule, value, callback) {

// 根据value进行进行校验

// 如果一切ok

// 直接执行callback

callback() // 一切ok 请继续

// 如果不ok

callback(new Error("错误信息")) //此错误信息还是规则判定里message那个错误判断,但是这个优先级低（必须添加new Error（）），如果规则数组里同时也添加了message，那么先用message的错误信息

}

}


```

根据以上要求，增加手机号第三位必须是9的校验规则

如下
  
```js

// 自定义校验函数
data() {

const checkMobile = function (rule, value, callback) {

value.charAt(2) === '9' ? callback() : callback(new Error('第三位手机号必须是9'))

}

  
return {

loginForm: {

mobile: ''

}

loginRules: {

mobile: [

{ required: true, message: '手机号不能为空', trigger: 'blur' },

{ pattern: /^1[3-9]\d{9}$/, message: '请输入正确的手机号', trigger: 'blur' }, {

trigger: 'blur',

validator: checkMobile

}],

}
}
}

```

#### 表单的手动校验

> 最后一个问题，如果我们直接点登陆按钮，没有离开焦点，那该怎么校验 ？

此时我们需要用到手动完整校验 [案例](https://element.eleme.cn/#/zh-CN/component/form

form表单提供了一份API方法，我们可以对表单进行完整和部分校验

| 方法名 | 说明 | 参数 |

| :------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |

| validate | 对整个表单进行校验的方法，参数为一个回调函数。该回调函数会在校验结束后被调用，并传入两个参数：是否校验成功和未通过校验的字段。若不传入回调函数，则会返回一个 promise | Function(callback: Function(boolean, object)) |

| validateField | 对部分表单字段进行校验的方法 | Function(props: array \| string, callback: Function(errorMessage: string)) |

| resetFields | 对整个表单进行重置，将所有字段值重置为初始值并移除校验结果 | — |

| clearValidate | 移除表单项的校验结果。传入待移除的表单项的 prop 属性或者 prop 组成的数组，如不传则移除整个表单的校验结果 | Function(props: array \| string) |


这些方法是el-form的API，需要获取el-form的实例，才可以调用

**采用ref进行调用**

```vue

<el-form ref="loginForm" style="margin-top:40px" :model="loginForm" :rules="loginRules">


```

**调用校验方法**

```js

login () {

// 获取el-form的实例

this.$refs.loginForm.validate(function (isOK) {

if (isOK) {

// 说明校验通过

// 调用登录接口 

} 

}) // 校验整个表单

}

```

#### elementUI 事件的修饰符
1. @keyup.enter.**`native`** 表示监听组件的原生事件，比如 keyup就是于input的原生事件，但是当element封装的btn没有开放这个事件，就不能挂载上去，所以添加native修饰符，表明此处就是用的原生事件，给原生input挂载。


#### 前端跨域问题
1. 开发环境跨域问题

- 当npm run dev 启动时是开发环境，在本地服务器上，设置反向代理

开发环境的跨域，也就是在**`vue-cli脚手架环境`**下开发启动服务时，我们访问接口所遇到的跨域问题，vue-cli为我们在本地**`开启了一个服务`**,可以通过这个服务帮我们**`代理请求`**,解决跨域问题

这就是vue-cli配置**webpack的反向代理**

![[image-20200811022013103.png]]

vue-cli的配置文件即**`vue.config.js`**,这里有我们需要的 [代理选项](https://cli.vuejs.org/zh/config/#devserver-proxy)

```js

module.exports = {

devServer: {

// 代理配置

proxy: {

// 这里的api 表示如果我们的请求地址有/api的时候,就出触发代理机制 注意：这里代理的触发需要在baseURL设置

// localhost:8888/api/abc => 代理给另一个服务器

// 本地的前端 =》 本地的后端 =》 代理我们向另一个服务器发请求 （行得通）

// 本地的前端 =》 另外一个服务器发请求 （跨域 行不通）

'/api': {

target: 'www.baidu.com', // 我们要代理的地址

changeOrigin: true, // 是否跨域 需要设置此值为true 才可以让本地服务代理我们发出请求

// 路径重写

pathRewrite: {

// 重新路由 localhost:8888/api/login => www.baidu.com/api/login

'^/api': '' // 就是去掉了路径里面的api。

}

},

}

}

}

```



2. 生产环境跨域问题

-  当npm run build 启动时是生产环境，打包给运维人员到云服务器

#### 给页面添加进度条
```js
import nProgress from 'nprogress'
nProgress.start()
nProgress.done()
```
#### 怎么自动全局注册自定义指令/过滤器同理
全局注册语法：
```js
vue.directive("指令名"， {

inserted(el){              //指令所在标签, 被插入到网页上触发(一次)
                           //el获得的是真实dom

},

update(el){      //指令对应数据/标签更新时, 此方法执行
     

}

})
```

1.  directives里面封装指令集js
```js
// 可以导出所有的变量，也就是指令，然后遍历
export const imagerror = {


inserted(dom, options) {

dom.onerror = function() {

dom.src = options.value

}

}
 
}
```
2. 在main.js里注册
```js
import * as directives from '@/directives'   //引入全部变量。也就是指令

Object.keys(directives).forEach(key => { //object的keys方法，遍历出对象的属性key并存放在数组里，然后遍历数组，得到里面key的值

Vue.directive(key, directives[key])  //注册全局指令的语法，指令名，指令函数，都是变量的形式，所以只要在directives封装指令就行

})
```

#### 怎么给vue全局注册组件
- 第一种：在main.js中直接注册
```js
// 引入
import PageTools from '@/components/PageTools'
// 注册为全局组件
Vue.component('PageTools', PageTools)

```

缺点：如果我们需要注册的全局组件非常多，我们需要一个一个引入，然后分别调用Vue.component方法，main.js文件会变的很大，不好维护。


- 第二种：使用插件的形式注册,在统一注册的index入口文件中


```js
// 引入，组件的入口index.js

import PageTools from './PageTools'
export default {
  install(Vue) {
  // 注册全局组件
    Vue.component('PageTools', PageTools)
  }
```

```js
//入口文件注册插件（main.js）

import Components from './components'
Vue.use(Components)
```

#### 用递归算法把普通对象转化为树形结构对象

封装一个工具方法，**`src/utils/index.js`**

```js

/** *

*

* 将列表型的数据转化成树形数据 => 递归算法 => 自身调用自身 => 一定条件不能一样， 否则就会死循环

* 遍历树形 有一个重点 要先找一个头儿

* ***/

export function tranListToTreeData(list, rootValue) { 
//这里是观察到子集的pid就是父的id，pid为空的元素就是根节点，根据先找根的原则，所以rootvalue的值要为’‘

var arr = []

list.forEach(item => {        //遍历所有数组对象

if (item.pid === rootValue) {      //检查是否是根节点

// 找到之后 就要去找 item 下面有没有子节点

const children = tranListToTreeData(list, item.id) 
//当传入rootvalue为空时，返回的是找到的根节点对象的数组
//再次调用这个方法传入，传入根节点对象的id，返回的是找到的根节点子节点对象

if (children.length) {

// 如果children的长度大于0 说明找到了子节点

item.children = children     //这里是把找到的子节点返回的arr数组（其实就是子节点对象）传给了根节点下的children

}

arr.push(item) // 将本次循环的找到的节点传入数组中

}

})

return arr      //返回数组

}

```


调用转化方法，转化树形结构

```js

this.company = { name: result.companyName, manager: '负责人' } // 这里定义一个空串 因为 它是根 所有的子节点的数据pid 都是 ""

this.departs = transListToTreeData(result.depts, '')

```

总结： 
  1. 所谓递归就是自己调用自己，但是要保证每次循环的值一定不一样。如果值一样就会死锁，循环相同的值。
  2. 传入值的不同保证循环的进行。但是也要有入口和出口，入口就递归最父级的根在哪里，出口就是保证一个条件循环会终止。
  3. 递归的最终是要赋值，每次递归都要留下证据。
  4. 本次递归最重要的就是item.children = children  ，如果没有这次赋值，每次的递归循环都不会留存下来。
  5. 递归本质就是重复第一遍的动作，但是如果不赋值给外界，重复的动作就不会被外界看到，所以目的就是，要递归出去值

#### 子修改父的值方法
- 理论上子修改父的值需要$emit传值给父值，然后父在监听传出的事件和值，赋值修改就好了。但是用语法糖可以快捷的实现这个功能。
- 有个前提，就是子修改的值是父传进来的。也就是说父先传给子值，子再传出来修改后的值。

1. model做法

	1. v-model会给子组件的props里的==value==变量传入值  例： value="article.is_followed"
	2. 同时自动监听子组件的==input==事件，并且自动替换变量的值（你在父组件传来的）
	例： @input="article.is_followed = $event"（不用写，自动完成）

技巧一：

如果需要修改 v-model 的规则名称，可以通过子组件的 model 属性来配置修改
```
model: {

prop: 'isFollowed',

event: 'update-is_followed'

},
```

技巧二：
一个组件上只能使用一次 v-model，如果有多个数据需要实现类似于 v-model 的效果，
可以使用属性的 .sync 修饰符。

==总结：==
>其实就是简化了父组件的写法，不用再监听事件，只用传value值就行
子组件依然要设置props属性接收value值，并且$emit(input, value) 传自定义事件，和要修改的值。


2. 用.sync 修饰符

- 只要用sync修饰，就可以省略父组件的监听和方法，直接将值赋值给showDialog
```js

// 子组件 update:固定写法 (update:props名称, 值)

this.$emit('update:showDialog', false) //子组件直接内部修改父组件传进来的值

// 父组件 sync修饰符

<child :showDialog.sync="showDialog" /> //前提是父组件传值的时候要添加修饰符.sync

```

  ==总结：==
>其实还是简化了父组件的写法，不用再监听事件，只用传value值就行
子组件依然要设置props属性接收父组件传来的值，但是可以直接在子组件内部操作修改，不用再传出事件

3. 直接获取父组件的实例
	- 本来父组件就可以获取子组件的实例，通过 this.$ref的方法
	- 现在子组件也可以获取父组件的实例，通过this.$parent 获取了父组件的实例，就可以调用其方法。

==注意== 要看好子组件的父组件是谁，子组件一定能要在真正的vue父组件的便签下，如果再第三方组件库下，那父组件就是第三方库了。

#### elementUI 的作用域插槽
- [[VUE#组件进阶 - 作用域插槽 | 作用域插槽]]其实和普通插槽一样，就是父组件在往插槽里传内容时，可以接收此时插槽里面的数据。
- 具体接收的数据是什么，需要在子组件里面的 slot 占位符设置什么。一般都是收集点击时的信息
- elementUI的作用域插槽用 slot-scope 接收， 而vue原生使用v-slot 接收。
```vue
<el-table-column   //注意，作用域插槽和不同插槽一样都要设置占位符的，所以能使用插槽的地方，都是提前预设好的，
				 像这个就是element预设的插槽，接收的参数为 { row, column, $index }

prop="companyId"

label="操作"

align="center"

>

<template slot-scope="{ row }">   //结构接收的数据，具体是什么其实可以打印一下

  

<el-button type="success" size="small">

分配权限

</el-button>

  

<el-button type="primary" size="small">

编辑

</el-button>

<el-button type="danger" size="small" @click="deleteRole(row.id)">

删除

</el-button>

</template>

  

</el-table-column>
```
#### vue如何导入Excel文件
1. 首先导入xlsx的第三方包
```bash
npm i xlsx
```
2. 有现成的组件代码 [代码地址](https://github.com/PanJiaChen/vue-element-admin/blob/master/src/components/UploadExcel/index.vue) 帮我们写好的xlsx的引用的组件代码，可以帮我们把xlsx文件转为数组形式的数据。
3. 只需要封装成组件，并且给组件传入一个 :on-success="success" 函数，就可以获得xlsx组件返回的两个实参。header, results
```js
methods: {

async success({ header, results }) {

const userRelations = {

  

'入职日期': 'timeOfEntry',

'手机号': 'mobile',

'姓名': 'username',

'工号': 'workNumber',

'转正日期': 'correctionTime'

  

}

  

const newArr = results.map(item => {

var userInfo = {}

Object.keys(item).forEach(key => {

if (userRelations[key] === 'timeOfEntry' || userRelations[key] === 'correctionTime') {

userInfo[userRelations[key]] = new Date(this.formatDate(item[key], '/'))

} else {

userInfo[userRelations[key]] = item[key]

}

})

return userInfo

})

try {

await importEmployee(newArr)

this.$message.success('导入成功')

} catch (err) {

this.$message.success('导入失败')

}

  

this.$router.back()

},

//这里是处理xlsx返回的特殊日期格式，经过此函数的处理，就变成了js的日期格式。

formatDate(numb, format) {

const time = new Date((numb - 1) * 24 * 3600000 + 1)

time.setYear(time.getFullYear() - 70)

const year = time.getFullYear() + ''

const month = time.getMonth() + 1 + ''

const date = time.getDate() - 1 + ''

if (format && format.length === 1) {

return year + format + month + format + date

}

return year + (month < 10 ? '0' + month : month) + (date < 10 ? '0' + date : date)

}

}
```
#### vue如何导出Excel文件
1. Excel 的导出不仅依赖于[js-xlsx](https://github.com/SheetJS/js-xlsx)来实现，还依赖`file-saver`和`script-loader`。

```shell
//所以要安装相关依赖
npm install xlsx file-saver -S
npm install script-loader -S -D
```

2. 已经有现成的封装了[Export2Excel.js](https://github.com/PanJiaChen/vue-element-admin/blob/master/src/vendor/Export2Excel.js)来方便导出数据。由于`js-xlsx`体积还是很大的，导出功能也不是一个非常常用的功能，所以使用的时候建议使用懒加载。使用方法如下：
```js

//import返回的是一个promise对象，excel接收的是Export2Excel中export输出的方法
import('@/vendor/Export2Excel').then(excel => {
  excel.export_json_to_excel({
    header: tHeader, //表头 必填
    data, //具体数据 必填
    filename: 'excel-list', //非必填
    autoWidth: true, //非必填
    bookType: 'xlsx' //非必填
  })
})

```

其中接收的**参数**介绍

| 参数      | 说明                   | 类型    | 可选值                                                       | 默认值     |
| --------- | ---------------------- | ------- | ------------------------------------------------------------ | ---------- |
| header    | 导出数据的表头         | Array   | /                                                            | []         |
| data      | 导出的具体数据         | Array   | /                                                            | [[]]       |
| filename  | 导出文件名             | String  | /                                                            | excel-list |
| autoWidth | 单元格是否要自适应宽度 | Boolean | true / false                                                 | true       |
| bookType  | 导出文件类型           | String  | xlsx, csv, txt, [more](https://github.com/SheetJS/js-xlsx#supported-output-formats) | xlsx       |
| 参数        | 说明           | 类型  | 可选值 | 默认值 |
| ----------- | -------------- | ----- | ------ | ------ |
| multiHeader | 复杂表头的部分 | Array | /      | [[]]   |
| merges      | 需要合并的部分 | Array | /      | []     |
示例：
```js
exportData() {

const headerName = {

'手机号': 'mobile',

'姓名': 'username',

'入职日期': 'timeOfEntry',

'聘用形式': 'formOfEmployment',

'转正日期': 'correctionTime',

'工号': 'workNumber',

'部门': 'departmentName'

}

  

import('@/vendor/Export2Excel').then(async excel => {

const { rows } = await getEmployeeList({ page: 1, size: this.page.total })

const data = this.formJson(headerName, rows)

excel.export_json_to_excel({

header: Object.keys(headerName),

data,

filename: '测试',

bookType: 'xlsx'

})

})

},


formJson(headerName, rows) {

return rows.map(item => {

return Object.keys(headerName).map(key => {

if (headerName[key] === 'timeOfEntry' || (headerName[key] === 'correctionTime')) {

return formatDate(item[headerName[key]])

} else if (headerName[key] === 'formOfEmployment') {

const obj = EmployeeEnum.hireType.find(items => items.id === item[headerName[key]])

  

return obj ? obj.value : '未知'

}

return item[headerName[key]]

})

})

}
```
#### vue导出Excel高级表头
> Export2Excel.js封装的方法中，可以接收**multiHeader**和**merges** 这两个参数

- **multiHeader**里面是一个二维数组，里面的一个元素是一行表头
- multiHeader中的一行表头中的字段的个数需要和真正的列数相等，假设想要跨列，多余的空间需要定义成空串
```js
//数组里封装的哪个数组，就是表头
const multiHeader = [['姓名', '主要信息', '', '', '', '', '部门']]
```

- 如果，我们要实现其合并的效果， 需要设定merges选项

```js
 const merges = ['A1:A2', 'B1:F1', 'G1:G2']
```

- merges的顺序是没关系的，只要配置这两个属性，就可以导出复杂表头的excel了

#### vue数据上传腾讯云存储桶
- 当使用element的el-upload组件上传照片时，可以把上传地址变为:http-request="upload"，这样就可以把上传的图片数据传给腾讯的[[人力资源中台项目PC#配置腾讯云Cos|云存储桶]]，需要配置一下。然后就接收返回的图片在线地址。

#### 数据生成二维码插件
首先，需要安装生成二维码的插件

```bash 
$ npm i qrcode
```

> qrcode的用法是

```js
QrCode.toCanvas(dom, info)
```

> dom为一个canvas的dom对象， info为转化二维码的信息

我们尝试将canvas标签放到dialog的弹层中

```vue
    <el-dialog title="二维码" :visible.sync="showCodeDialog" @opened="showQrCode" @close="imgUrl=''">
      <el-row type="flex" justify="center">
        <canvas ref="myCanvas" />
      </el-row>
    </el-dialog>
```

在点击员工的图片时，显示弹层，并将图片地址转化成二维码

```js
    showQrCode(url) {
      // url存在的情况下 才弹出层
      if (url) {
        this.showCodeDialog = true // 数据更新了 但是我的弹层会立刻出现吗 ？页面的渲染是异步的！！！！
        // 有一个方法可以在上一次数据更新完毕，页面渲染完毕之后
        this.$nextTick(() => {
          // 此时可以确认已经有ref对象了
          QrCode.toCanvas(this.$refs.myCanvas, url) // 将地址转化成二维码
          // 如果转化的二维码后面信息 是一个地址的话 就会跳转到该地址 如果不是地址就会显示内容
        })
      } else {
        this.$message.warning('该用户还未上传头像')
      }
    }

```

#### VUE打印插件
首先，打印功能我们借助一个比较流行的插件

```bash
$ npm i vue-print-nb
```

> 它的用法是 

首先注册该插件

```js
import Print from 'vue-print-nb'
Vue.use(Print);
```

使用v-print指令的方式进行打印

```vue
  <el-row type="flex" justify="end">
          <el-button v-print="printObj" size="small" type="primary">打印</el-button>
   </el-row>
   printObj: {
        id: 'myPrint' //这里是要打印的区域，给那个区域设置id即可
   }

```
#### 权限问题思路
[[人力资源中台项目PC#权限受控的主体思路|参考案例]]
- 给用户添加权限，分为两部分，一个是访问权限，一个是操作权限。访问权限由路由控制，通过路由守卫，提前判断路由的走向，从而实现访问权限。操作权限主要使用Mixin技术将检查方法注入，从页面层禁止用户访问该功能。
1. 访问权限的设置
 ![[image-20200901164312005.png]]
 - 用户登录之后会在vuex里获取userInfo，然后里面存放着用户可以访问哪些路由的标识，通常和路由name对应关联。只需要遍历筛选出所有的路由name和用户的标识一致的部分，通过route的addRoutes方法追加到router实例中。就可以实现动态路由的添加。
 2. 操作权限的设置
 - 操作权限的设置需要引入一个mixin技术，它可以给全局组件注入方法，属性等，相当于给所有的组件添加了副组件。这样，只需要给全局组件注入一个方法（筛选userInfo里面的points属性与传入方法的标识是否一致）然后就可以再各个页面引用此方法，只需要给方法传入特定的标识名，就可以返回一个布尔值，判断此功能是否有权限。

#### 日历组件的创建
- 日历组件主要是利用elementUI ，创建日历模板，然后绑定数据同步日历的时间，可以通过作用域插槽来改变日期格子的内容。[[人力资源中台项目PC#工作日历组件封装 | 案例详情]]

#### 全屏插件的引用

**目标**：实现页面的全屏功能

原生方法： 
```js
document.documentElement.requestFullscreen()  //开启全屏
document.exitFullscreen()  //退出全屏
```


> 全屏功能可以借助一个插件来实现

第一步，安装全局插件**screenfull**

```bash

$ npm i screenfull

```

  

第二步，封装全屏显示的插件·· **`src/components/ScreenFull/index.vue`**

  

```vue

<template>

<!-- 放置一个图标 -->

<div>

<!-- 放置一个svg的图标 -->

<svg-icon icon-class="fullscreen" style="color:#fff; width: 20px; height: 20px" @click="changeScreen" />

</div>

</template>

  

<script>

import ScreenFull from 'screenfull'

export default {

methods: {

// 改变全屏

changeScreen() {

if (!ScreenFull.isEnabled) {

// 此时全屏不可用

this.$message.warning('此时全屏组件不可用')

return

}

// document.documentElement.requestFullscreen() 原生js调用

// 如果可用 就可以全屏

ScreenFull.toggle()  //既可以全屏又可以退出全屏

}

}

}

</script>

  

<style>

  

</style>

  

```

  

第三步，全局注册该组件 **`src/components/index.js`**

  

```js

import ScreenFull from './ScreenFull'

Vue.component('ScreenFull', ScreenFull) // 注册全屏组件

```

  

第四步，放置于**`layout/navbar.vue`**中

  

```vue

<screen-full class="right-menu-item" />

.right-menu-item {

vertical-align: middle;

}

```


#### 多语言实现
#### 初始化多语言包

本项目使用国际化 i18n 方案。通过 [vue-i18n](https://github.com/kazupon/vue-i18n)而实现。

**第一步，我们需要首先国际化的包**

```bash

$ npm i vue-i18n

```

  

**第二步，需要单独一个多语言的实例化文件 `src/lang/index.js`**

  

```js

import Vue from 'vue' // 引入Vue

import VueI18n from 'vue-i18n' // 引入国际化的包

import Cookie from 'js-cookie' // 引入cookie包

import elementEN from 'element-ui/lib/locale/lang/en' // 引入饿了么的英文包

import elementZH from 'element-ui/lib/locale/lang/zh-CN' // 引入饿了么的中文包

Vue.use(VueI18n) // 全局注册国际化包

export default new VueI18n({

locale: Cookie.get('language') || 'zh', // 从cookie中获取语言类型 获取不到就是中文

messages: {

en: {

...elementEN // 将饿了么的英文语言包引入

},

zh: {

...elementZH // 将饿了么的中文语言包引入

}

}

})

  

```

  

> 上面的代码的作用是将Element的两种语言导入了

  

**第三步，在main.js中对挂载 i18n的插件，并设置element为当前的语言**

  

```js

// 设置element为当前的语言

Vue.use(ElementUI, {

i18n: (key, value) => i18n.t(key, value)

})

  

new Vue({

el: '#app',

router,

store,

i18n,

render: h => h(App)

})

  

```

  

#### 引入自定义语言包

  

> 此时，element已经变成了zh，也就是中文，但是我们常规的内容怎么根据当前语言类型显示？

  

这里，针对英文和中文，我们可以提供两个不同的语言包 **`src/lang/zh.js , src/lang/en.js`**

  

> 该语言包，我们已经在资源中提供

  

**第四步，在index.js中同样引入该语言包**

  

```js

import customZH from './zh' // 引入自定义中文包

import customEN from './en' // 引入自定义英文包

Vue.use(VueI18n) // 全局注册国际化包

export default new VueI18n({

locale: Cookie.get('language') || 'zh', // 从cookie中获取语言类型 获取不到就是中文

messages: {

en: {

...elementEN, // 将饿了么的英文语言包引入

...customEN

},

zh: {

...elementZH, // 将饿了么的中文语言包引入

...customZH

}

}

})

```

  

#### 在左侧菜单中应用多语言包

  

> 自定义语言包的内容怎么使用?

  

**第五步，在左侧菜单应用**

  

当我们全局注册i18n的时候，每个组件都会拥有一个**`$t`**的方法，它会根据传入的key，自动的去寻找当前语言的文本，我们可以将左侧菜单变成多语言展示文本

  

**`layout/components/SidebarItem.vue`**

  

```vue

<item :icon="onlyOneChild.meta.icon||(item.meta&&item.meta.icon)" :title="$t('route.'+onlyOneChild.name)" />

  

```

  

注意：当文本的值为嵌套时，可以通过**`$t(key1.key2.key3...)`**的方式获取

  

> 现在，我们已经完成了多语言的接入，现在封装切换多语言的组件

  

#### 封装多语言插件

  

**第六步，封装多语言组件** **`src/components/lang/index.vue`**

  

```vue

<template>

<el-dropdown trigger="click" @command="changeLanguage">

<!-- 这里必须加一个div -->

<div>

<svg-icon style="color:#fff;font-size:20px" icon-class="language" />

</div>

<el-dropdown-menu slot="dropdown">

<el-dropdown-item command="zh" :disabled="'zh'=== $i18n.locale ">中文</el-dropdown-item>

<el-dropdown-item command="en" :disabled="'en'=== $i18n.locale ">en</el-dropdown-item>

</el-dropdown-menu>

</el-dropdown>

</template>

  

<script>

import Cookie from 'js-cookie'

export default {

methods: {

changeLanguage(lang) {

Cookie.set('language', lang) // 切换多语言

this.$i18n.locale = lang // 设置给本地的i18n插件

this.$message.success('切换多语言成功')

}

}

}

</script>

  
  

```

  

**第七步，在Navbar组件中引入**

  

```vue

<!-- 放置切换多语言 -->

<lang class="right-menu-item" />

<!-- 放置主题 -->

<theme-picker class="right-menu-item" />

<!-- 放置全屏插件 -->

<screen-full class="right-menu-item" />

```

  

最终效果

  

![image-20200804001654730](assets/image-20200804001654730.png)

  

#### 打包上线流程
案例分析[[人力资源中台项目PC#打包上线 | 案例]]
1. 先处理spa单页面的路由模式，因为开发阶段用的是hash模式。改为history模式
2. 性能分析，分析出那个文件比较大，优化一下
3. CDN优化法，先排除webpack的打包，让它不打包这几个包，我们单独用cdn的方法引入。
4. 开始打包， 并用后端框架部署项目，然后启动
5. 解决history模式引发的问题
6. 解决生产环境的跨域问题，以前的开发环境，反向代理的代理是/api,这里要换一下



### 小兔仙项目方法集
#### 路由-vue3 路由相关配置
- 当跳转传参的时候，把参数通过url的形式传给url
- router.currentRoute.value是当前页面的url，因为是响应式对象，所以加value
- 但是记得如果传的参数有特殊符号要记得转义，用encodeURIComponent()
```js
    const fullPath = encodeURIComponent(router.currentRoute.value.fullPath)
    // encodeURIComponent 转换uri编码，防止解析地址出问题
    router.push('/login?redirectUrl=' + fullPath)
  }
```

- 封装工具函数 -直接可以调用传参，不用再一个一个写入数据
```js
// 请求工具函数
export default (url, method, submitData) => {
  // 负责发请求：请求地址，请求方式，提交的数据
  return instance({
    url,
    method,
    // 1. 如果是get请求  需要使用params来传递submitData   ?a=10&c=10
    // 2. 如果不是get请求  需要使用data来传递submitData   请求体传参
    // [] 设置一个动态的key, 写js表达式，js表达式的执行结果当作KEY
    // method参数：get,Get,GET  转换成小写再来判断
    // 在对象，['params']:submitData ===== params:submitData 这样理解
    [method.toLowerCase() === 'get' ? 'params' : 'data']: submitData
  })
}
```


#### @vueuse/core 工具库
安装：@vueuse/core 包，它封装了常见的一些交互逻辑。
-   vue3.0组合API提供了更多逻辑代码封装的能力。@vueuse/core 基于组合API封装好用的工具函数。

```bash
npm i @vueuse/core@4.9.0
```

1.   useWindowScroll() 是@vueuse/core提供的api可返回当前页面滚动时候蜷曲的距离。x横向，y纵向
2. useVModel 实现快捷子父组件的数据双向绑定
```js
import { useVModel } from '@vueuse/core'
// v-model  ====>  :modelValue  +   @update:modelValue
export default {
  name: 'XtxCheckbox',
  props: {
    modelValue: {
      type: Boolean,
      default: false
    }
  },
  setup (props, { emit }) {
    // 使用useVModel实现双向数据绑定v-model指令
    // 1. 使用props接收modelValue
    // 2. 使用useVModel来包装props中的modelValue属性数据
    // 3. 在使用checked.value就是使用父组件数据
    // 4. 在使用checked.value = '数据' 赋值，触发emit('update:modelvalue', '数据')
    const checked = useVModel(props, 'modelValue', emit)
    const changeChecked = () => {
      const newVal = !checked.value
      // 通知父组件
      checked.value = newVal
      // 让组件支持change事件
      emit('change', newVal)
    }
    return { checked, changeChecked }
  }
}
```
3. 监听属性外点击事件
```js
import { onClickOutside } from '@vueuse/core'

onClickOutside(target, () => {
closeDialog()
})
```

4. 定时器工具逻辑
```js
// 定时器逻辑

// useIntervalFn 1. 回调函数， 时间间隔， 是否立即开启

const time = ref(0)

const { pause, resume } = useIntervalFn(() => {

if (time.value <= 0) {

pause()

} else {

time.value--

}

}, 1000, false)

// 销毁定时器

onUnmounted(() => {

pause()

})
```

5. 懒加载方法
```js
// stop 是停止观察是否进入或移出可视区域的行为    
const { stop } = useIntersectionObserver(
  // target 是观察的目标dom容器，必须是dom容器，而且是vue3.0方式绑定的dom对象
  target,
  // isIntersecting 是否进入可视区域，true是进入 false是移出
  // observerElement 被观察的dom
  ([{ isIntersecting }], observerElement) => {
    // 在此处可根据isIntersecting来判断，然后做业务
  },
  {
  threshold: 0. //阈值，监控到哪里才会算数
  }
)
```
#### 全局注册组件，自定义指令方法
 封装插件：插件定义 `src/componets/library/index.js` 使用插件 `src/main.js`

```js
// 扩展vue原有的功能：全局组件，自定义指令，挂载原型方法，注意：没有全局过滤器。
// 这就是插件
// vue2.0插件写法要素：导出一个对象，有install函数，默认传入了Vue构造函数，Vue基础之上扩展
// vue3.0插件写法要素：导出一个对象，有install函数，默认传入了app应用实例，app基础之上扩展

import XtxSkeleton from './xtx-skeleton.vue'

export default {
  install (app) {
    // 在app上进行扩展，app提供 component directive 函数
    // 如果要挂载原型 app.config.globalProperties 方式
    app.component(XtxSkeleton.name, XtxSkeleton)
  }
}
```

- main.js里注册
```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import './mock'
+import ui from './components/library'

import 'normalize.css'
import '@/assets/styles/common.less'
+// 插件的使用，在main.js使用app.use(插件)
+createApp(App).use(store).use(router).use(ui).mount('#app')
```

#### 怎么自动注册全局组件

 >自动注册全局组件后，就可以直接写入，不需要手动注册了

大致步骤：
-   使用 `require` 提供的函数 `context` 加载某一个目录下的所有 `.vue` 后缀的文件。
-   然后 `context` 函数会返回一个导入函数 `importFn`
    -   它有一个属性 `keys()` 获取所有的文件路径
-   通过文件路径数组，通过遍历数组，再使用 `importFn` 根据路径导入组件对象
-   遍历的同时进行全局注册即可

落的代码：

`src/components/library/index.js`

```js
// 其实就是vue插件，扩展vue功能，全局组件、指令、函数 （vue.30取消过滤器）
// 当你在mian.js导入，使用Vue.use()  (vue3.0 app)的时候就会执行install函数
// import XtxSkeleton from './xtx-skeleton.vue'
// import XtxCarousel from './xtx-carousel.vue'
// import XtxMore from './xtx-more.vue'
// import XtxBread from './xtx-bread.vue'
// import XtxBreadItem from './xtx-bread-item.vue'

// 导入library文件夹下的所有组件
// 批量导入需要使用一个函数 require.context(dir,deep,matching)
// 参数：1. 目录  2. 是否加载子目录  3. 加载的正则匹配
const importFn = require.context('./', false, /\.vue$/)
// console.dir(importFn.keys()) 文件名称数组

export default {
  install (app) {
    // app.component(XtxSkeleton.name, XtxSkeleton)
    // app.component(XtxCarousel.name, XtxCarousel)
    // app.component(XtxMore.name, XtxMore)
    // app.component(XtxBread.name, XtxBread)
    // app.component(XtxBreadItem.name, XtxBreadItem)

    // 批量注册全局组件
    importFn.keys().forEach(key => {
      // 导入组件
      const component = importFn(key).default
      // 注册组件
      app.component(component.name, component)
    })

    // 定义指令
    defineDirective(app)
  }
}

const defineDirective = (app) => {
  // 图片懒加载指令 v-lazyload
  app.directive('lazyload', {
    // vue2.0 inserted函数，元素渲染后
    // vue3.0 mounted函数，元素渲染后
    mounted (el, binding) {
      // 元素插入后才能获取到dom元素，才能使用 intersectionobserve进行监听进入可视区
      // el 是图片元素  binding.value 图片地址
      const observe = new IntersectionObserver(([{ isIntersecting }]) => {
        if (isIntersecting) {
          el.src = binding.value
          // 取消观察
          observe.unobserve(el)
        }
      }, {
        threshold: 0.01
      })
      // 进行观察
      observe.observe(el)
    }
  })
}

```
总结，知识点：
-   require.context() 是webpack提供的一个自动导入的API
    -   参数1：加载的文件目录
    -   参数2：是否加载子目录
    -   参数3：正则，匹配文件
    -   返回值：导入函数 fn
        -   keys() 获取读取到的所有文件列表


#### 骨架组件的使用
- 骨架组件就是当网页没刷新出来的时候，有一个框架的显示。
- 其原理就是依靠 v-if 配合开启关闭
- 骨架组件都是自己根据页面样式定义，然后可以全局注册组件

>vue中自带了一个动画组件，可以用于骨架组件的动画

#### vue的动画组件
vue中内置了一个全局组件
- 当vue中，显示隐藏，创建移除，一个元素或者一个组件的时候，可以通过transition实现动画。

![[Pasted image 20221013110735.png]]

如果元素或组件离开，完成一个淡出效果：

```html
<transition name="fade">
  <p v-if="show">100</p>
</transition>
```

```css
.fade-leave {
    opacity: 1
}
.fade-leave-active {
    transition: all 1s;
}
.fade-leave-to {
    opcaity: 0
}
```

-   进入（显示，创建）
    -   v-enter 进入前 （vue3.0 v-enter-from）
    -   v-enter-active 进入中
    -   v-enter-to 进入后
-   离开（隐藏，移除）
    -   v-leave 进入前 （vue3.0 v-leave-from）
    -   v-leave-active 进入中
    -   v-leave-to 进入后

多个transition使用不同动画，可以添加nam属性，name属性的值替换v即可。
#### 首页主体-轮播图-基础布局

> **目的：** 封装小兔鲜轮播图组件，第一步：基础结构的使用。

**大致步骤：**

-   准备xtx-carousel组件基础布局，全局注册
-   准备home-banner组件，使用xtx-carousel组件，再首页注册使用。
-   深度作用xtx-carousel组件的默认样式

**落的代码：**

-   轮播图基础结构 `src/components/library/xtx-carousel.vue`

```html
<template>
  <div class='xtx-carousel'>
    <ul class="carousel-body">
      <li class="carousel-item fade">
        <RouterLink to="/">
          <img src="http://yjy-xiaotuxian-dev.oss-cn-beijing.aliyuncs.com/picture/2021-04-15/1ba86bcc-ae71-42a3-bc3e-37b662f7f07e.jpg" alt="">
        </RouterLink>
      </li>
    </ul>
    <a href="javascript:;" class="carousel-btn prev"><i class="iconfont icon-angle-left"></i></a>
    <a href="javascript:;" class="carousel-btn next"><i class="iconfont icon-angle-right"></i></a>
    <div class="carousel-indicator">
      <span v-for="i in 5" :key="i"></span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'XtxCarousel'
}
</script>
<style scoped lang="less">
.xtx-carousel{
  width: 100%;
  height: 100%;
  min-width: 300px;
  min-height: 150px;
  position: relative;
  .carousel{
    &-body {
      width: 100%;
      height: 100%;
    }
    &-item {
      width: 100%;
      height: 100%;
      position: absolute;
      left: 0;
      top: 0;
      opacity: 0;
      transition: opacity 0.5s linear;
      &.fade {
        opacity: 1;
        z-index: 1;
      }
      img {
        width: 100%;
        height: 100%;
      }
    }
    &-indicator {
      position: absolute;
      left: 0;
      bottom: 20px;
      z-index: 2;
      width: 100%;
      text-align: center;
      span {
        display: inline-block;
        width: 12px;
        height: 12px;
        background: rgba(0,0,0,0.2);
        border-radius: 50%;
        cursor: pointer;
        ~ span {
          margin-left: 12px;
        }
        &.active {
          background:  #fff;
        }
      }
    }
    &-btn {
      width: 44px;
      height: 44px;
      background: rgba(0,0,0,.2);
      color: #fff;
      border-radius: 50%;
      position: absolute;
      top: 228px;
      z-index: 2;
      text-align: center;
      line-height: 44px;
      opacity: 0;
      transition: all 0.5s;
      &.prev{
        left: 20px;
      }
      &.next{
        right: 20px;
      }
    }
  }
  &:hover {
    .carousel-btn {
      opacity: 1;
    }
  }
}
</style>

```

-   全局注册轮播图 `src/components/library/index.js`

```
import XtxSkeleton from './xtx-skeleton.vue'
+import XtxCarousel from './xtx-carousel.vue'

export default {
  install (app) {
    app.component(XtxSkeleton.name, XtxSkeleton)
+    app.component(XtxCarousel.name, XtxCarousel)
  }
}
```

-   首页广告组件基础结构 `src/views/home/components/home-banner.vue`

```
<template>
  <div class="home-banner">
    <XtxCarousel />
  </div>
</template>
<script>
export default {
  name: 'HomeBanner'
}
</script>
<style scoped lang="less">
.home-banner {
  width: 1240px;
  height: 500px;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 98
}
</style>
```

-   首页使用广告组件

```
<template>
+  <!-- 首页入口 -->
+  <div class="home-entry">
+    <div class="container">
      <!-- 左侧分类 -->
      <HomeCategory />
      <!-- 轮播图 -->
      <HomeBanner />
    </div>
  </div>
</template>
<script>
import HomeCategory from './components/home-category'
+import HomeBanner from './components/home-banner'
export default {
  name: 'HomePage',
  components: {
+    HomeCategory,
    HomeBanner
  }
}
</script>
<style scoped lang="less"></style>
```

-   覆盖轮播图组件样式 `src/views/home/components/home-banner.vue`

```
.xtx-carousel {
  ::v-deep .carousel-btn.prev {
    left: 270px;
  }
  ::v-deep .carousel-indicator {
    padding-left: 250px;
  }
}
```

**总结：** 需要注意要覆盖样式，首页轮播图特殊些。

#### 首页主体-轮播图-渲染结构

> **目的：** 封装小兔鲜轮播图组件，第二步：动态渲染结构。

**大致步骤：**

-   定义获取广告图API函数
-   在home-banner组件获取轮播图数据，传递给xtx-carousel组件
-   在xtx-carousel组件完成渲染

**落的代码：**

-   API函数 `src/api/home.js`

```
/**
 * 获取广告图
 * @returns Promise
 */
export const findBanner = () => {
  return request('/home/banner', 'get')
}
```

-   广告组件获取数据，传给轮播图 `src/views/home/components/home-banner.vue`

```
<template>
  <div class="home-banner">
+    <XtxCarousel :sliders="sliders" />
  </div>
</template>
<script>
import { ref } from 'vue'
import { findBanner } from '@/api/home'
export default {
  name: 'HomeBanner',
+  setup () {
+    const sliders = ref([])
+    findBanner().then(data => {
+      sliders.value = data.result
+    })
+    return { sliders }
+  }
}
</script>
```

-   完成轮播图结构渲染 `src/components/library/xtx-carousel.vue`

```
<template>
  <div class='xtx-carousel'>
    <ul class="carousel-body">
+      <li class="carousel-item" v-for="(item,i) in sliders" :key="i" :class="{fade:index===i}">
        <RouterLink to="/">
+          <img :src="item.imgUrl" alt="">
        </RouterLink>
      </li>
    </ul>
    <a href="javascript:;" class="carousel-btn prev"><i class="iconfont icon-angle-left"></i></a>
    <a href="javascript:;" class="carousel-btn next"><i class="iconfont icon-angle-right"></i></a>
    <div class="carousel-indicator">
+      <span v-for="(item,i) in sliders" :key="i" :class="{active:index===i}"></span>
    </div>
  </div>
</template>

<script>
+import { ref } from 'vue'
export default {
  name: 'XtxCarousel',
+  props: {
+    sliders: {
+      type: Array,
+      default: () => []
+    }
+  },
+  setup () {
+    // 默认显示的图片的索引
+    const index = ref(0)
+    return { index }
+  }
}
</script>
```

**总结：** fade是控制显示那张图片的，需要一个默认索引数据，渲染第一张图和激活第一个点。

#### 首页主体-轮播图-逻辑封装

> **目的：** 封装小兔鲜轮播图组件，第三步：逻辑功能实现。

**大致步骤：**

-   自动播放，暴露自动轮播属性，设置了就自动轮播
-   如果有自动播放，鼠标进入离开，暂停，开启
-   指示器切换，上一张，下一张
-   销毁组件，清理定时器

**落地代码：** `src/components/library/xtx-carousel.vue`

-   自动轮播实现

```js
+import { ref, watch } from 'vue'
export default {
  name: 'XtxCarousel',
  props: {
    sliders: {
      type: Array,
      default: () => []
    },
    duration: {
      type: Number,
      default: 3000
    },
    autoPlay: {
      type: Boolean,
      default: false
    }
  },
  setup (props) {
    // 默认显示的图片的索引
    const index = ref(0)
   // 自动播放
    let timer = null
    const autoPlayFn = () => {
      clearInterval(timer)
      timer = setInterval(() => {
        index.value++
        if (index.value >= props.sliders.length) {
          index.value = 0
        }
      }, props.duration)
    }
    watch(() => props.sliders, (newVal) => {
      // 有数据&开启自动播放，才调用自动播放函数
      if (newVal.length && props.autoPlay) {
        index.value = 0
        autoPlayFn()
      }
    }, { immediate: true })

    return { index }
  }
}
```

-   如果有自动播放，鼠标进入离开，暂停，开启

```
    // 鼠标进入停止，移出开启自动，前提条件：autoPlay为true
    const stop = () => {
      if (timer) clearInterval(timer)
    }
    const start = () => {
      if (props.sliders.length && props.autoPlay) {
        autoPlayFn()
      }
    }

    return { index, stop, start }
```

```
+  <div class='xtx-carousel' @mouseenter="stop()" @mouseleave="start()">
```

使用需要加 auto-play `<XtxCarousel auto-play :sliders="sliders" />`

-   指示器切换，上一张，下一张

```
    // 上一张下一张
    const toggle = (step) => {
      const newIndex = index.value + step
      if (newIndex >= props.sliders.length) {
        index.value = 0
        return
      }
      if (newIndex < 0) {
        index.value = props.sliders.length - 1
        return
      }
      index.value = newIndex
    }

    return { index, stop, start, toggle }
```

-   销毁组件，清理定时器

```
    // 组件消耗，清理定时器
    onUnmounted(() => {
      clearInterval(timer)
    })
```

**总结：** 按照思路步骤，一步步实现即可。


#### 组件数据懒加载- 监听滚动区域
>我们可以使用 `@vueuse/core` 中的 `useIntersectionObserver` 来实现监听进入可视区域行为，但是必须配合vue3.0的组合API的方式才能实现。

**大致步骤：**
-   理解 `useIntersectionObserver` 的使用，各个参数的含义
-   改造 home-new 组件成为数据懒加载，掌握 `useIntersectionObserver` 函数的用法
-   封装 `useLazyData` 函数，作为数据懒加载公用函数
-   把 `home-new` 和 `home-hot` 改造成懒加载方式

**落地代码：**

1.  先分析下这个`useIntersectionObserver` 函数：

```js
// stop 是停止观察是否进入或移出可视区域的行为    
const { stop } = useIntersectionObserver(
  // target 是观察的目标dom容器，必须是dom容器，而且是vue3.0方式绑定的dom对象
  target,
  // isIntersecting 是否进入可视区域，true是进入 false是移出
  // observerElement 被观察的dom
  ([{ isIntersecting }], observerElement) => {
    // 在此处可根据isIntersecting来判断，然后做业务
  },
  {
  threshold: 0. //阈值，监控到哪里才会算数
  }
)
```

2.  开始改造 `home-new` 组件：`rc/views/home/components/home-new.vue`

-   进入可视区后获取数据

```vue
<div ref="box" style="position: relative;height: 406px;">
// 省略。。。
<script>
import HomePanel from './home-panel'
import HomeSkeleton from './home-skeleton'
import { findNew } from '@/api/home'
import { ref } from 'vue'
import { useIntersectionObserver } from '@vueuse/core'
export default {
  name: 'HomeNew',
  components: { HomePanel, HomeSkeleton },
  setup () {
    const goods = ref([])
    const box = ref(null)
    const { stop } = useIntersectionObserver(
      box,
      ([{ isIntersecting }]) => {
        if (isIntersecting) {
          stop()
          findNew().then(data => {
            goods.value = data.result
          })
        }
      }
    )
    return { goods, box }
  }
}
</script>
```

3.  由于首页面板数据加载都需要实现懒数据加载，所以封装一个钩子函数，得到数据。

`src/hooks/index.js`

```js
// hooks 封装逻辑，提供响应式数据。
import { useIntersectionObserver } from '@vueuse/core'
import { ref } from 'vue'
// 数据懒加载函数
export const useLazyData = (apiFn) => {
  // 需要
  // 1. 被观察的对象
  // 2. 不同的API函数
  const target = ref(null)
  const result = ref([])
  const { stop } = useIntersectionObserver(
    target,
    ([{ isIntersecting }], observerElement) => {
      if (isIntersecting) {
        stop()
        // 调用API获取数据
        apiFn().then(data => {
          result.value = data.result
        })
      }
    }
  )
  // 返回--->数据（dom,后台数据）
  return { target, result }
}
```

4.  再次改造 `home-new` 组件：`rc/views/home/components/home-new.vue`

```js
import { findNew } from '@/api/home'
+import { useLazyData } from '@/hooks'
export default {
  name: 'HomeNew',
  components: { HomePanel, HomeSkeleton },
  setup () {
+    const { target, result } = useLazyData(findNew)
+    return { goods: result, target }
  }
}
```

```html
+ <div ref="target" style="position: relative;height: 426px;">
```

5.  然后改造 `home-hot` 组件：`src/views/home/components/home-hot.vue`

```html
+  <div ref="target" style="position: relative;height: 426px;">
```

```js
import { findHot } from '@/api/home'
import HomePanel from './home-panel'
import HomeSkeleton from './home-skeleton'
+import { useLazyData } from '@/hooks'
export default {
  name: 'HomeHot',
  components: { HomePanel, HomeSkeleton },
  setup () {
+    const { target, result } = useLazyData(findHot)
+    return { target, list: result }
  }
}
```


#### 图片数据懒加载
> **目的：** 当图片进入可视区域内去加载图片，且处理加载失败，封装成指令。

介绍一个webAPI：[IntersectionObserver(opens new window)](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver/IntersectionObserver)
组件数据懒加载用的方法，底层也是这个api

```js
// 创建观察对象实例
const observer = new IntersectionObserver(callback，[options])
// callback 被观察dom进入可视区离开可视区都会触发
// - 两个回调参数 entries , observer
// - entries 被观察的元素信息对象的数组 [{元素信息},{}]，信息中isIntersecting判断进入或离开
// - observer 就是观察实例
// options 配置参数
// - 三个配置属性 root rootMargin threshold
// - root 基于的滚动容器，默认是document
// - rootMargin 容器有没有外边距
// - threshold 交叉的比例

// 实例提供两个方法
// observe(dom) 观察哪个dom
// unobserve(dom) 停止观察那个dom
```

基于vue3.0和IntersectionObserver封装懒加载指令

`src/components/library/index.js`
- 在library的js模块，封装指令，和全局组件。

```js
//借用了install的函数，获取app的对象。
export default {
  install (app) {
    app.component(XtxSkeleton.name, XtxSkeleton)
    app.component(XtxCarousel.name, XtxCarousel)
    app.component(XtxMore.name, XtxMore)
    defineDirective(app)  //挂载指令
  }
}
```

```js
import defaultImg from '@/assets/images/200.png'
// 定义指令
const defineDirective = (app) => {  //封装指令函数，在app的实例上，挂载指令。
  // 图片懒加载指令
  //原理： 先储存图片的地址，不放在src上，当图片进入可视区，在将图片地址给元素的src
  app.directive('lazy', {
    mounted (el, binding) {  //因为要获取原生dom，所以要在mounted钩子函数中获取dom
      const observer = new IntersectionObserver(([{ isIntersecting }]) => {
        if (isIntersecting) {
          observer.unobserve(el)
          el.onerror = () => {
              el.src = defaultImg
          }  
          el.src = binding.value
        }
      }, {
        threshold: 0.01
      })
      observer.observe(el)
    }
  })
}
```

使用指令：

`src/views/home/component/home-product.vue`

```html
        <RouterLink class="cover" to="/">
+          <img alt="" v-lazyload="cate.picture">
          <strong class="label">
            <span>{{cate.name}}馆</span>
            <span>{{cate.saleInfo}}</span>
          </strong>
        </RouterLink>
```

**总结：**

-   在img上使用使用v-lazyload值为图片地址，不设置src属性。

#### 渲染组件的方法
[createElement (opens new window)](https://cn.vuejs.org/v2/guide/render-function.html#createElement-%E5%8F%82%E6%95%B0)[render (opens new window)](https://cn.vuejs.org/v2/api/#render)`render选项与h函数`

-   指定组件显示的内容：`new Vue({选项})`
    -   el 选项挂载便签，通过一个选择器找到容器，容器内容就是组件内容  .mount('#app')也行
    -   template 选项，`<div>组件内容</div>` 作为组件内容
    -   render选项，它是一个函数，函数回默认传人==createElement的函数（h）==，这个函数用来创建结构，再render函数返回渲染为组件内容。它的优先级更高。==（但是render只渲染，还是要el挂载的）==
             ==注意：vue3中h函数，不是作为参数传入的，而是需要引入的==
> 总体就是 render函数渲染组件， h函数创建组件， el挂载组件
> 
- createElement函数的用法
```js
import App from './App.vue'
new Vue({
	 render:h=>h(App)      //用render选项来渲染组件
}).mount('#app')  //然后挂载到#app的容器中，用el选项也能挂载
	// h() =====>  createElement()， 默认形参给render函数
 //h(App) =====>  根据APP组件创建html结构，给render返回
	 //render的返回值就是html结构，渲染到#app容器
	// h() 函数参数，1.节点名称  2. 属性|数据 是对象  3. 子节点
```

- 实际用法
```vue
<script>
import { h } from 'vue'  //vue3中需要引入h函数
export default {
  name: 'XtxBread',
  render () {
    // 用法
    // 1. template 标签去除，单文件组件
    // 2. 返回值就是组件内容
    // 3. vue2.0 的h函数传参进来的，vue3.0 的h函数导入进来
    // 4. h 第一个参数 标签名字  第二个参数 标签属性对象  第三个参数 子节点
    // 需求
    // 1. 创建xtx-bread父容器
    // 2. 获取默认插槽内容
    // 3. 去除xtx-bread-item组件的i标签，因该由render函数来组织
    // 4. 遍历插槽中的item，得到一个动态创建的节点，最后一个item不加i标签
    // 5. 把动态创建的节点渲染再xtx-bread标签中
    const items = this.$slots.default()   //获取插槽中的数据， 数组的形式
    const dymanicItems = []
    items.forEach((item, i) => {  //遍历插槽数据，然后添加一个i标签后，追加给一个新数组。
      dymanicItems.push(item)
      if (i < (items.length - 1)) {
        dymanicItems.push(h('i', { class: 'iconfont icon-angle-right' }))  //不给最后一个插槽数据添加i标签
      }
    })
    return h('div', { class: 'xtx-bread' }, dymanicItems)   //给 h函数 返回新的html结构
  }
}
</script>

<style lang='less'>
// 去除 scoped 属性，目的：然样式作用到xtx-bread-item组件
.xtx-bread{
  display: flex;
  padding: 25px 10px;
  // ul li:last-child {}
  // 先找到父元素，找到所有的子元素，找到最后一个，判断是不是LI，是就是选中，不是就是无效选择器
  // ul li:last-of-type {}
  // 先找到父元素，找到所有的类型为li的元素，选中最后一个
  &-item {
    a {
      color: #666;
      transition: all .4s;
      &:hover {
        color: @xtxColor;
      }
    }
  }
  i {
    font-size: 12px;
    margin-left: 5px;
    margin-right: 5px;
    line-height: 22px;
    // 样式的方式，不合理
    // &:last-child {
    //   display: none;
    // }
  }
}
</style>
```
-   总结，一下知识点
    -   render 是vue提供的一个渲染函数，优先级大于el,template等选项，用来提供组件结构。
    -   注意：
        -   vue2.0 render函数提供h（createElement）函数用来创建节点
        -   vue3.0 h（createElement）函数有 vue 直接提供，需要按需导入
    -   this.$slots.default() 获取默认插槽的node结构，按照要求拼接结构。
    -   h函数的传参 tag 标签名|组件名称, props 标签属性|组件属性, node 子节点|多个节点
    -   具体参考 [render](http://zhoushugang.gitee.io/erabbit-client-pc-document/guide/[https:/vue-docs-next-zh-cn.netlify.app/guide/render-function.html#dom-%E6%A0%91](https:/vue-docs-next-zh-cn.netlify.app/guide/render-function.html#dom-%E6%A0%91))
-   注意：不要在 xtx-bread 组件插槽写注释，也会被解析。


#### jsx语法
- 官网   https://cn.vuejs.org/v2/guide/render-function.html
>vue内置有一个 [Babel 插件](https://github.com/vuejs/jsx)，用于在 Vue 中使用 JSX 语法，它可以让我们回到更接近于模板的语法上。

- 当然前提是需要在渲染函数里面写， 渲染函数把用jsx写得标签组件，渲染到页面上
- JSX语法官网： https://github.com/vuejs/jsx-vue2#installation

#### 用jsx语法写tabs组件

```js
export default {

name: 'XtxTabs',

props: {

modelValue: {

type: String,

default: ''

}

},

setup (props, { emit }) {

const activeName = useVModel(props, 'modelValue', emit)  //双向绑定activeName

const startClick = (name, i) => {   //封装点击函数

activeName.value = name

emit('click-tab')

}

  

provide('activeName', activeName)  // 注入activeName

return {

startClick,

activeName

}

},

render () {      //在渲染函数中 创建我们要的标签结构

// jsx语法，它能够让我们创建节点和写html一样

// 1. 动态插值表达式{} 2. 尽量三元表示式做判断，使用map来遍历 3.事件使用原始方式绑定

// 默认插槽节点

const panel = this.$slots.default()   //获取 插槽里面的组件

  

const nav = <nav> {panel[0].children.map((item, i) => {  //用jsx语法封装nav标签

return <a onClick={() => this.startClick(item.props.name, i)} class={{ active: item.props.name === this.activeName }} href="javascript:;">{item.props.label}</a>

})} </nav>


//给渲染函数返回整个要渲染的节点， （不用像以前那样， 各个部分分开写， 直接像原生标签那样返回即可）
return <div class="xtx-tabs">{[nav, panel]}</div>   

}

}
```
#### 路由控制页面跳转位置
- 每次切换路由的时候滚动到顶部 `src/router/index.js`
- 在路由模块注册
```js
const router = createRouter({
   history: createWebHashHistory(),
  routes,
   scrollBehavior () {
   return { left: 0, top: 0 }  //vue3的语法，vue2的语法是x，y
 }
})
```
#### 获取插槽传来的标签/组件
- 可以获取外界插槽传进来的组件或者便签，以数组的形式。
- 注意，如果有注释也会变为数组的数据，所以不要添加注释在插槽里面
```js
const items = this.$slots.default()
```

#### 图片放大器的使用
```js
const currIndex = ref(0)

// 是否显示大图和遮罩层

const show = ref(false)

// 遮罩层的位置

const layerPosition = reactive({

left: 0,

top: 0

})

// 大图背景的位置

const largePosition = reactive({

backgroundPositionX: 0,

backgroundPositionY: 0

})

// 使用usevue来获取鼠标基于元素左上角的坐标

const target = ref(null)

const { elementX, elementY, isOutside } = useMouseInElement(target)

  

watch([elementX, elementY, isOutside], () => {

show.value = !isOutside.value

//定义坐标位置
const position = { x: elementX.value - 100, y: elementY.value - 100 }


//计算X坐标位置

if (elementX.value < 100) {

position.x = 0

} else if (elementX.value > 300) {

position.x = 200

}

  
//计算Y坐标位置

if (elementY.value < 100) {

position.y = 0

} else if (elementY.value > 300) {

position.y = 200

}

//赋值给样式坐标位置
layerPosition.left = position.x + 'px'

layerPosition.top = position.y + 'px'

largePosition.backgroundPositionX = -2 * position.x + 'px' //因为反向，所以为负值，因为放大，所以移动两倍的像素。

largePosition.backgroundPositionY = -2 * position.y + 'px'

```

#### 规格组件-SKU&SPU概念
` spu代表一个商品，拥有很多相同的属性。 sku代表该商品可选规格的任意组合，他是库存单位的唯一标识。

-   SPU（Standard Product Unit）：标准化产品单元。是商品信息聚合的最小单位，是一组可复用、易检索的标准化信息的集合，该集合描述了一个产品的特性。通俗点讲，属性值、特性相同的商品就可以称为一个SPU。
-   SKU（Stock Keeping Unit）库存量单位，即库存进出计量的单位， 可以是以件、盒、托盘等为单位。SKU是物理上不可分割的最小存货单元。在使用时要根据不同业态，不同管理模式来处理。
![[Pasted image 20221104160330.png]]
大致步骤：
1.  根据后台返回的skus数据得到有效sku组合
2.  根据有效的sku组合得到所有的子集集合
3.  根据子集集合组合成一个路径字典，也就是对象。
4.  在组件初始化的时候去判断每个规格是否点击
5.  在点击规格的时候去判断其他规格是否可点击
6.  判断的依据是，拿着说有规格和现在已经选中的规则取搭配，得到可走路径。
    1.  如果可走路径在字典中，可点击
    2.  如果可走路径不在字典中，禁用


#### 分页组件
大致步骤：
-   分页基础布局，依赖数据分析。
-   分页内部逻辑，完成切换效果。
-   接收外部数据，提供分页事件。

落的代码：
-   分页基础布局，依赖数据分析 `src/components/library/xtx-pagination.vue`

```vue
<template>
  <div class="xtx-pagination">
    <a href="javascript:;" class="disabled">上一页</a>
    <span>...</span>
    <a href="javascript:;" class="active">3</a>
    <a href="javascript:;">4</a>
    <a href="javascript:;">5</a>
    <a href="javascript:;">6</a>
    <a href="javascript:;">7</a>
    <span>...</span>
    <a href="javascript:;">下一页</a>
  </div>
</template>
<script>
export default {
  name: 'XtxPagination'
}
</script>
<style scoped lang="less">
.xtx-pagination {
  display: flex;
  justify-content: center;
  padding: 30px;
  > a {
    display: inline-block;
    padding: 5px 10px;
    border: 1px solid #e4e4e4;
    border-radius: 4px;
    margin-right: 10px;
    &:hover {
      color: @xtxColor;
    }
    &.active {
      background: @xtxColor;
      color: #fff;
      border-color: @xtxColor;
    }
    &.disabled {
      cursor: not-allowed;
      opacity: 0.4;
      &:hover {
        color: #333
      }
    }
  }
  > span {
    margin-right: 10px;
  }
}
</style>
```

![1614245759420](http://zhoushugang.gitee.io/erabbit-client-pc-document/assets/img/1614245759420.7fad127d.png)
-   分页内部逻辑，完成切换效果 `src/components/library/xtx-pagination.vue`

1）准备渲染数据

```js
  setup () {
    // 总条数
    const myTotal = ref(100)
    // 每页条数
    const myPageSize = ref(10)
    // 当前第几页
    const myCurrentPage = ref(1)
    // 按钮个数
    const btnCount = 5

    // 重点：根据上述数据得到（总页数，起始页码，结束页码，按钮数组）
    const pager = computed(() => {
      // 计算总页数
      const pageCount = Math.ceil(myTotal.value / myPageSize.value)
      // 计算起始页码和结束页码
      // 1. 理想情况根据当前页码，和按钮个数可得到
      let start = myCurrentPage.value - Math.floor(btnCount / 2)
      let end = start + btnCount - 1
      // 2.1 如果起始页码小于1了，需要重新计算
      if (start < 1) {
        start = 1
        end = (start + btnCount - 1) > pageCount ? pageCount : (start + btnCount - 1)
      }
      // 2.2 如果结束页码大于总页数，需要重新计算
      if (end > pageCount) {
        end = pageCount
        start = (end - btnCount + 1) < 1 ? 1 : (end - btnCount + 1)
      }
      // 处理完毕start和end得到按钮数组
      const btnArr = []
      for (let i = start; i <= end; i++) {
        btnArr.push(i)
      }
      return { pageCount, start, end, btnArr }
    })

    return { pager, myCurrentPage}
  }
```

2）进行渲染

```html
    <a v-if="myCurrentPage<=1" href="javascript:;" class="disabled">上一页</a>
    <a v-else href="javascript:;">上一页</a>
    <span v-if="pager.start>1">...</span>
    <a href="javascript:;" :class="{active:i===myCurrentPage}" v-for="i in pager.btnArr" :key="i">{{i}}</a>
    <span v-if="pager.end<pager.pageCount">...</span>
    <a v-if="myCurrentPage>=pager.pageCount" href="javascript:;" class="disabled">下一页</a>
    <a v-else href="javascript:;">下一页</a>
```

3）切换效果

```html
  <div class="xtx-pagination">
    <a v-if="myCurrentPage<=1" href="javascript:;" class="disabled">上一页</a>
+    <a @click="changePage(myCurrentPage-1)" v-else href="javascript:;">上一页</a>
    <span v-if="pager.start>1">...</span>
+    <a @click="changePage(i)" href="javascript:;" :class="{active:i===myCurrentPage}" v-for="i in pager.btnArr" :key="i">{{i}}</a>
    <span v-if="pager.end<pager.pageCount">...</span>
    <a v-if="myCurrentPage>=pager.pageCount" href="javascript:;" class="disabled">下一页</a>
+    <a @click="changePage(myCurrentPage+1)" v-else href="javascript:;">下一页</a>
  </div>
```

```js
    // 改变页码
    const changePage = (newPage) => {
      myCurrentPage.value = newPage
    }

    return { pager, myCurrentPage, changePage }
```

-   接收外部数据，提供分页事件。

```js
  props: {
    total: {
      type: Number,
      default: 100
    },
    currentPage: {
      type: Number,
      default: 1
    },
    pageSize: {
      type: Number,
      default: 10
    }
  },
```

```js
    // 监听传人的值改变
    watch(props, () => {
      myTotal.value = props.total
      myPageSize.value = props.pageSize
      myCurrentPage.value = props.currentPage
    }, { immediate: true })
```

```js
    // 改变页码
    const changePage = (newPage) => {
      if (myCurrentPage.value !== newPage) {
        myCurrentPage.value = newPage
        // 通知父组件最新页码
        emit('current-change', newPage)
      }
    }
```

最后使用组件：

```js
+   // 记录总条数
	const commentList = ref([])
+   const total = ref(0)
	watch(reqParams, async () => {
      const data = await findCommentListByGoods(props.goods.id, reqParams)
      commentList.value = data.result
+      total.value = data.result.counts
    }, { immediate: true })
```

```js
	// 改变分页函数
    const changePager = (np) => {
      reqParams.page = np
    }
    return { commentInfo, currTagIndex, changeTag, reqParams, changeSort, commentList, total, changePager }
```

```html
    <!-- 分页 -->
    <XtxPagination @current-change="changePager" :total="total" :current-page="reqParams.page"  />
```

筛选和排序改变后页码回到第一页：

```js
    // 改变排序
    const changeSort = (type) => {
      reqParams.sortField = type
+      reqParams.page = 1
    }
```

```js
    const changeTag = (i) => {
      currTagIndex.value = i
      // 设置有图和标签条件
      const currTag = commentInfo.value.tags[i]
      if (currTag.type === 'all') {
        reqParams.hasPicture = false
        reqParams.tag = null
      } else if (currTag.type === 'img') {
        reqParams.hasPicture = true
        reqParams.tag = null
      } else {
        reqParams.hasPicture = false
        reqParams.tag = currTag.title
      }
+      reqParams.page = 1
    }
```

优化：有条数才显示分页

```html
<div class="xtx-pagination" v-if="total>0">
```
#### 登录的vue表单校验

> 文档：https://vee-validate.logaretm.com/v4/ 支持vue3.0
> `有一个方法库，可以进行vue的表单校验

==注意， 核心就是规则对象和表单对象，手动自定义这两个对象。
v-model 绑定表单对象，负责记录数据
规则对象匹配name属性，负责检验规则

- 第一步：安装
-   执行命令 `npm i vee-validate@4.0.3`

第二步：导入
-   修改文件 `src/views/login/index.vue`
- - 注意这是两个组件
```js
import { Form, Field } from 'vee-validate' //导入两个组件， 并且注册两个组件
```

第三步：定义校验规则
-   提取目的 `这些校验规则将来在其他表单验证时候可复用`
-   新建文件 `src/utils/vee-validate-schema.js`
```js
// 定义校验规则提供给vee-validate组件使用
export default {
  // 校验account
  account (value) {
    // value是将来使用该规则的表单元素的值
    // 1. 必填
    // 2. 6-20个字符，需要以字母开头
    // 如何反馈校验成功还是失败，返回true才是成功，其他情况失败，返回失败原因。
    if (!value) return '请输入用户名'
    if (!/^[a-zA-Z]\w{5,19}$/.test(value)) return '字母开头且6-20个字符'
    return true
  },
  password (value) {
    if (!value) return '请输入密码'
    if (!/^\w{6,24}$/.test(value)) return '密码是6-24个字符'
    return true
  },
  mobile (value) {
    if (!value) return '请输入手机号'
    if (!/^1[3-9]\d{9}$/.test(value)) return '手机号格式错误'
    return true
  },
  code (value) {
    if (!value) return '请输入验证码'
    if (!/^\d{6}$/.test(value)) return '验证码是6个数字'
    return true
  },
  isAgree (value) {
    if (!value) return '请勾选同意用户协议'
    return true
  }
}
```

第四步：应用Form， Field 组件

1. 用应用Form， Field 替换form input原生标签， 必须添加name属性，用来匹配校验规则
2. Form组件 的validation-schema属性，用来接收总的校验对象，对象的属性匹配name的值进行校验
3. Field组件必须是v-model绑定的，这样才能获取值进行校验。
4. 自定义组件需要校验也必须要支持v-model, 不然获取不到组件里面的值，然后还是用Field组件名，默认是input组件， 使用as指定自定义组件
```html
<Form ref="formCom" class="form" :validation-schema="schema" v-slot="{errors}" autocomplete="off">

<template v-if="!isMsgLogin">

<div class="form-item">

<div class="input">

<i class="iconfont icon-user"></i>

<Field :class="{ error: errors.account}" v-model="form.account" name="account" type="text" placeholder="请输入用户名或手机号" />

</div>

<div class="error" v-if="errors.account"><i class="iconfont icon-warning" />{{ errors.account }}</div>

</div>

  

<div class="form-item">

<div class="input">

<i class="iconfont icon-lock"></i>

<Field :class="{ error: errors.password }" v-model="form.password" name="password" type="password" placeholder="请输入密码" />

</div>

<div class="error" v-if="errors.password"><i class="iconfont icon-warning" />{{ errors.password }}</div>

  

</div>

</template>

<template v-else>

<div class="form-item">

<div class="input">

<i class="iconfont icon-user"></i>

<Field :class="{ error: errors.mobile}" v-model="form.mobile" name="mobile" type="text" placeholder="请输入手机号" />

</div>

<div class="error" v-if="errors.mobile"><i class="iconfont icon-warning" />{{ errors.mobile }}</div>

  

</div>

<div class="form-item">

<div class="input">

<i class="iconfont icon-code"></i>

<Field :class="{ error: errors.code}" v-model="form.code" name="code" type="password" placeholder="请输入验证码" />

<span class="code">发送验证码</span>

</div>

<div class="error" v-if="errors.code"><i class="iconfont icon-warning" />{{ errors.code }}</div>

  

</div>

</template>

<div class="form-item">

<div class="agree">

<Field as="XtxCheckbox" name="isAgree" v-model="form.isAgree" />

<span>我已同意</span>

<a href="javascript:;">《隐私条款》</a>

<span>和</span>

<a href="javascript:;">《服务条款》</a>

</div>

<div class="error" v-if="errors.isAgree"><i class="iconfont icon-warning" />{{ errors.isAgree }}</div>

  

</div>

<a @click="login()" href="javascript:;" class="btn">登录</a>

</Form>
```


```js
setup (props) {

// 切换短信登录

const isMsgLogin = ref(true)

  

// 双向绑定的数据，用来手动提交表单

const form = reactive({

isAgress: true,

account: null,

password: null,

mobile: null,

code: null

})

// vee-validate表单验证步骤

// 1. 导入组件， 替换form input， 必须添加name属性，用来匹配校验规则

// 2. Field 必须进行双向绑定， 用来获取input组件里面输入的值

// 3. 定义校验规则， form的validate-schema接收schema校验规则对象， 其中定义与name属性对应的校验规则

// 4. 自定义组件需要校验必须要支持v-model, 不然获取不到组件里面的值，然后还是用Field组件名，默认是input组件， 使用as指定自定义组件

// 定义校验规则

const schema = {

account: myschema.account,

password: myschema.password,

mobile: myschema.mobile,

code: myschema.code,

isAgree: myschema.isAgree

}
  
// 监听isMsgLogin重置表单, (1. 需要手动重置数据， 2. 当使用v-if销毁表单时，不需要重置校验结果，如果使用v-show不会销毁表单，需要用reserform方法重置)

const formCom = ref(null)

watch(isMsgLogin, () => {

form.isAgree = true

form.account = null

form.password = null

form.mobile = null

form.code = null

// 如果使用v-show需要手动重置校验结果

formCom.value.resetForm()  

})

// 登录时 对整体表单全局校验

const login = async () => {

const valid = await formCom.value.validate()

}

return {

isMsgLogin,

form,

schema,

formCom,

login

}
}
```

第五步： 手动校验的方法
1. 当需要手动重置校验时， 手动获取组件的实例，然后用重置方法
```js
const formCom = ref(null)
formCom.value.resetForm()
```
2. 当需要手动表单校验时, 使用全部校验的方法
```js
const formCom = ref(null)
const valid = await formCom.value.validate()  //返回布尔值，校验结果
```

#### 封装消息组件
- 消息模块封装为组件，调用时只能组件调用方法。
```vue

<template>

<Transition name="down">

<div class="xtx-message" :style="style[type]" v-show="visible">

<!-- 上面绑定的是样式 -->

<!-- 不同提示图标会变 -->

<i class="iconfont" :class="[style[type].icon]" ></i>

<span class="text">{{ text }}</span>

</div>

</Transition>

</template>

  

<script>

import { onMounted, ref } from 'vue'

  

export default {

name: 'xtxMessage',

props: {

type: {

type: String,

default: 'warn'

},

text: {

type: String,

default: ''

}

},

setup (props) {

// 定义一个对象，包含三种情况的样式，对象key就是类型字符串

const style = {

warn: {

icon: 'icon-warning',

color: '#E6A23C',

backgroundColor: 'rgb(253, 246, 236)',

borderColor: 'rgb(250, 236, 216)'

},

error: {

icon: 'icon-shanchu',

color: '#F56C6C',

backgroundColor: 'rgb(254, 240, 240)',

borderColor: 'rgb(253, 226, 226)'

},

success: {

icon: 'icon-queren2',

color: '#67C23A',

backgroundColor: 'rgb(240, 249, 235)',

borderColor: 'rgb(225, 243, 216)'

}

}

  

// 定义一个数据控制显示隐藏，默认是隐藏，组件挂载完毕显示

const visible = ref(false)

onMounted(() => {

visible.value = true

})

return {

style,

visible

}

}

}

</script>

  

<style lang="less" scoped>

.down {

&-enter {

&-from {

transform: translate3d(0, -75px, 0);

opacity: 0;

  

&-active {

transition: all 0.5s;

  

}

  

&-to {

transform: none;

opacity: 1;

  

}

  

}

  

}

}


</style>
```

##### 封装消息组件为函数调用
- 消息组件的使用为函数调用，这样vue2、3都可以用
1. 首先先封装方法， 然后就可以在各个组件中引入js使用
2. 或者先封装方法， 然后在app原型中挂载方法， 然后就可以在dom实例中使用此方法
```js

// 定义函数式调用组件的方法

// 此文件提供能够显示xtx-message组件的函数

// 使用方法：

// 1. 直接引入js， 然后使用方法 import Message from '/message.js' Message({type:'', text:''})

// 2. 把js方法挂载到vue实例原型上 this.$message({type:'', text:''})

  

import { createVNode, render } from 'vue'

import XtxMessage from './xtx-message.vue' // 导入消息提示组件

let timer = null

// 定义函数式调用组件的方法

export default ({ type, text }) => {

// 渲染组件

// 1. 导入消息提示组件

// 2. 把vue组件编译为虚拟节点（dom节点）

// createVNode(组件， 属性对象（props)）

const vnode = createVNode(XtxMessage, { type, text })

// 3. 准备装载组件虚拟dom节点的dom容器, 可以写在函数外边，减少创建次数

const div = document.createElement('div')

// div.setAttribute('class', 'xtx-message-container')

document.body.appendChild(div)

// 4. 将虚拟节点渲染到dom容器中

// render(虚拟节点，容器)

render(vnode, div)

// 5. 3s后销毁组件

clearTimeout(timer)

timer = setTimeout(() => {

render(null, div)

}, 3000)

}
```

- 在app原型中挂载方法
```js
// 如果你想挂载全局的属性，能够通过组件实例调用的属性 this.$message
import Message from './message'
app.config.globalProperties.$Message = Message// 原型函数
```

#### qq登录跳转流程

-   在登录页面，QQ登录图片处，赋予其打开QQ登录页面功能。
-   回跳的页面得到QQ给的唯一标识openId，根据openId去后台查询是否已经绑定过账户。
    -   如果绑定过，完成登录。
    -   没有绑定过
        -   有账号的，绑定手机号，即为登录。
        -   没账号的，完善账户信息，即为登录。
-   登录成功后，跳转首页，或者来源页面。
    
