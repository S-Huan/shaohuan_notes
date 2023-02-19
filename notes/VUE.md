
## Vue基本概念
>Vue是一个javascript渐进式（逐渐使用，集合更多的功能）框架（拥有自己的语法规则）。主张最少, 自底向上, 增量开发, 组件集合, 便于复用
  
+ 传统开发模式：基于html/css/js文件开发vue
+ 工程化开发方式：在webpack环境中开发vue，这是最推荐, 企业常用的方式

## vue脚手架

>@vue/cli是Vue官方提供的一个node全局模块包(得到vue命令), 此包用于创建脚手架项目
1.  开箱即用
2. 0配置webpack 
3. babel支持
4. css, less支持
5. 开发服务器支持
- 相当于在一个webpack上运行vue，开发的代码直接经过webpack直接打包
- 虽然不用配置，里面的第三方包，还是需要自己下的，如 less-loader 等

### @vue/cli安装

> 把@vue/cli模块包按到全局, 电脑拥有vue命令, 才能创建脚手架工程

+ 全局安装命令

```bash
yarn global add @vue/cli

# OR
npm install -g @vue/cli

# 查看版本
vue -V
```


### @vue/cli 创建项目

> 目标: 使用vue命令, 创建脚手架项目

1. 创建项目
- 在项目目录下创建脚手架目录
```bash

# vue和create是命令, vuecli-demo是文件夹名 注意: 项目名不能带大写字母, 中文和特殊符号

vue create vuecli-demo

```

2. 选择模板
- 选择用什么方式下载脚手架项目需要的依赖包
  
3. 回车等待生成项目文件夹+文件+下载必须的第三方包们

4. 进入脚手架项目下, 启动内置的热更新本地服务器
- 底层是node+webpack热更新服务
==注意要进入刚刚创立的脚手架项目目录
```bash

cd vuecil-demo

npm run serve

# 或

yarn serve

# App running at:
  - Local:   http://localhost:**8080**/ 
  - Network: http://192.168.1.5:**8080**/
  Note that the development build is not optimized.
  To create a production build, run yarn build.
```
  
> 总结: vue命令创建工程目录, 项目内置webpack本地热更新服务器, 帮我们打包项目预览项目


### @vue/cli 文件说明
  
```bash

vuecil-demo # 项目目录

├── node_modules # 项目依赖的第三方包

├── public # 静态文件目录

├── favicon.ico# 浏览器小图标

└── index.html # 单页面的html文件(网页浏览的是它)

├── src # 业务文件夹

├── assets # 静态资源

└── logo.png # vue的logo图片

├── components # 组件目录

└── HelloWorld.vue # 欢迎页面vue代码文件

├── App.vue # 整个应用的根组件

└── main.js # 入口js文件

├── .gitignore # git提交忽略配置

├── babel.config.js # babel配置

├── package.json # 依赖包列表

├── README.md # 项目说明

└── yarn.lock # 项目包版本锁定和缓存地址

```


### @vue/cli 工作原理
- vue组件入口文件，封装进wepack的入口文件 开启本地服务器

![[image-20210117123737917.png]]
1. main.js – 项目打包主入口 – Vue初始化

2. App.vue – Vue页面主入口

3. index.html – 浏览器运行的文件

- 各个组件.vue  => 各个.css => App.vue => main.js => index.html
```javascript
// 项目入口-webpack打包从这开始
//index.js 入口文件

import Vue from 'vue' // 引入vue源码
import App from './App.vue' // 引入App.vue文件模块

Vue.config.productionTip = false // 一个控制台打印的提示

new Vue({ // 实例化Vue(传入配置对象)
  render: h => h(App), // 告诉vue渲染什么
}).$mount('#app') // '渲染到哪里
```

### @vue/cli 配置端口

> 项目中没有webpack.config.js文件，因为@vue/cli用的vue.config.js

- src并列处新建vue.config.js
```jsx

/* 覆盖webpack的配置 */

module.exports = {
devServer: { // 自定义服务配置
open: true, // 自动打开浏览器
port: 3000
}
}

```


### @vue/cli 单vue文件

>业务代码写在这里,作为vue模块存在，相当于node里的模块化

>vue文件分为三个部分
>	标签区域< template >，用于写用到的标签。
>	js区域< script >，用于写业务逻辑。
>	样式区域< style >，页面相关样式。

特点：
- template里只能有一个根标签
- vue文件-独立模块-作用域互不影响
- style配合scoped属性, 保证样式只针对当前template内标签生效
- vue文件配合webpack, 把他们打包起来插入到index.html


```vue

<!-- template必须, 只能有一个根标签, 影响渲染到页面的标签结构 -->
<!--html相关 -->
<template>

<div>欢迎使用vue</div>   --只写影响的ui结构

</template>


<!-- js相关 -->

<script>

export default {

data() {  //定义变量，运用的是简写的形式，相当于data：{}
return {
count： 1
}
},

methods: { //定义函数

addFn(){ // this代表export default后面的组件对象(下属有data里return出来的属性)

this.count++

},

}

</script>


<!-- 当前组件的样式, 设置scoped, 可以保证样式只对当前页面有效 -->

<style scoped>

</style>

  
```

最终: Vue文件配合webpack, 把他们打包起来插入到index.html, 然后在浏览器运行

### eslint了解

>它是一个==代码检查工具==,如果写代码违反了eslint的规则-报错
>默认开启在脚手架里

 -  关闭-进入vue.config.js
```javascript
  module.exports = {
    // ...其他配置
    lintOnSave: false // 关闭eslint检查
  }
//重启服务器
```



### 插入图片
>在单vue文件里，引入图片模块时才可以用相对路径，否则在data定义变量时，就要用链接url地址导入。
- import img from '相对路径'

## Vue指令
**vue指令, 实质上就是特殊的 html 标签属性, 特点: v- 开头**
### vue指令-v-bind
> 给标签属性设置vue变量的值

- 语法：`v-bind:属性名="vue变量" 
- 简写：`:属性名="vue变量"`

```html

<!-- vue指令-v-bind属性动态赋值 -->

<a v-bind:href="url">我是a标签</a>

<img :src="imgSrc">

```

> 总结: 把vue变量的值, 赋予给dom-html属性上, 影响标签显示效果

### vue指令-v-on

> 给标签绑定事件

* 语法
* v-on:事件名="要执行的==少量代码=="
* v-on:事件名="methods中的函数"
* v-on:事件名="methods中的函数(实参)"
* 简写: @事件名="methods中的函数"


```html

<!-- vue指令: v-on事件绑定-->
<div>

<p>你要买商品的数量: {{count}}</p>

<button v-on:click="count = count + 1">增加1</button>

<button v-on:click="addFn">增加1个</button>

<button v-on:click="addCountFn(5)">一次加5件</button>

<button @click="subFn">减少</button>

</div>

<script>

export default {

data() {  //定义变量
return {
count： 1
}
},

methods: { //定义函数

addFn(){ // this代表export default后面的组件对象(下属有data里return出来的属性)

this.count++

},

addCountFn(num){

this.count += num

},

subFn(){

this.count--

}

}

}

</script>

```


### vue指令-v-on事件对象

> vue事件处理函数中, 拿到事件对象

* 语法:
* 无传参, 通过形参（e）直接接收
* 传参, 通过$event指代事件对象传给事件处理函数

  
```vue

<template>

<div>

<a @click="one" href="http://www.baidu.com">阻止百度</a>

<hr>

<a @click="two(10, $event)" href="http://www.baidu.com">阻止去百度</a>

</div>

</template>

  

<script>

export default {

methods: {
//无传参，就可以直接用形参e来直接接收事件对象
one(e){

e.preventDefault()

},

//有传实参，就要在实参最后附加一个$event实参，最后还用形参e接收事件对象
two(num, e){

e.preventDefault()

}

}

}

</script>

```

  
### vue指令-v-on普通修饰符

> 所谓修饰符就是给事件封装的各种功能，功能上相当于事件对象。

* 语法:
* @事件名.修饰符="methods里函数"

- 修饰符：
* .stop - 阻止事件冒泡
* .prevent - 阻止默认行为
* .once - 程序运行期间, 只触发一次事件处理函数（事件还可以触发，函数不触发）
* .native - 当是自定义组件时， 添加native修饰符可以添加事件

```html

<template>

<div @click="fatherFn">

<!-- vue对事件进行了修饰符设置, 在事件后面.修饰符名即可使用更多的功能 -->

<button @click.stop="btn">.stop阻止事件冒泡</button>

<a href="http://www.baidu.com" @click.prevent="btn">.prevent阻止默认行为</a>

<button @click.once="btn">.once程序运行期间, 只触发一次事件处理函数</button>

</div>

</template>

  

<script>

export default {

methods: {

fatherFn(){

console.log("father被触发");

},

btn(){

console.log(1);

}

}

}

</script>

```
  

### vue指令-v-on按键修饰符

> 给键盘事件, 添加修饰符, 封装了键盘事件的事件对象（监听了一些常用的按键）

* 语法:
* @keyup.enter - 监测回车按键
* @keyup.esc - 监测返回按键

```html

<template>

<div>

<input type="text" @keydown.enter="enterFn">

<hr>

<input type="text" @keydown.esc="escFn">

</div>

</template>

  

<script>

export default {

methods: {

enterFn(){

console.log("enter回车按键了");

},

escFn(){

console.log("esc按键了");

}

}

}

</script>

```

> 总结: 多使用事件修饰符, 可以提高开发效率, 少去自己判断过程

### vue指令 v-model

> v-model:是实现vuejs变量和表单标签的value属性, 双向绑定的指令
   所谓双向绑定就是，页面值影响变量，变量值影响页面。
* 语法: v-model="vue数据变量"

* 双向数据绑定
* 数据（变量）变化 -> 视图（value）自动同步
* 视图（value）变化 -> 数据（变量）自动同步

>一般只能用于表单标签，因为标签有value值，才能和变量进行双向绑定。
<如：下拉菜单（给子项添加value），单选框（value），复选框（value），文本框（不用添加value）
```vue

<template>

<div>

<!--

v-model:是实现vuejs变量和表单标签value属性, 双向绑定的指令

-->

<div>

<span>用户名:</span>

<input type="text" v-model="username" />

</div>

<div>

<span>密码:</span>

<input type="password" v-model="pass" />

</div>

<div>

<span>来自于: </span>

<!-- 下拉菜单要绑定在select上 -->
//其实就相当于单选框，三个标签共用过一个变量，但是不会冲突。（但是变量怎么知道赋值哪一个呢？）
f
<select v-model="from">

<option value="北京市">北京</option>

<option value="南京市">南京</option>

<option value="天津市">天津</option>

</select>

</div>

<div>

<!-- 复选框，多个标签共用一个属性，而且可以同时添加-->

遇到复选框, v-model的变量值

非数组 - 关联的是复选框的checked属性（ture/false）

数组 - 关联的是复选框的value属性(真正的value值)



<span>爱好: </span>

<input type="checkbox" v-model="hobby" value="抽烟">抽烟

<input type="checkbox" v-model="hobby" value="喝酒">喝酒

<input type="checkbox" v-model="hobby" value="写代码">写代码

</div>

<div>

<!-- 单选框，两个标签共有一个变量 -->

<span>性别: </span>

<input type="radio" value="男" name="sex" v-model="gender">男

<input type="radio" value="女" name="sex" v-model="gender">女

</div>

<div>

<span>自我介绍</span>

<!-- 文本框，里面的内容自动添加到了value值里，不用额外添加 -->

<textarea v-model="intro"></textarea>

</div>

</div>

</template>

  

<script>

export default {

data() {

return {

username: "",

pass: "",

from: "",

hobby: [],

sex: "",

intro: "",

};

// 总结:

// 特别注意: v-model, 在input[checkbox]的多选框状态

// 变量为非数组, 则绑定的是checked的属性(true/false) - 常用于: 单个绑定使用

// 变量为数组, 则绑定的是他们的value属性里的值 - 常用于: 收集勾选了哪些值

}

};

</script>

```

### vue指令 v-model父传子
>当我们传递给子组件的数据既要使用还要修改。

- 正常做法

1. 传递给子组件数据给他使用  例：:is-followed="article.is_followed"
2. 子组件$emit 传出自定义事件和值，让父组件监听事件并修改那个数据的值 
例： @update-is_followed="article.is_followed = $event"

- model做法

1. v-model会给子组件的props里的value变量传入值  例： value="article.is_followed"
2. 同时自动监听子组件的input事件，并且自动替换变量的值（你在父组件传来的）
例： @input="article.is_followed = $event"（不用写，自动完成）

==总结：==
>其实就是简化了父组件的写法，不用再监听事件，只用传value值就行
子组件依然要设置props属性接收value值，并且$emit(input, value) 传自定义事件，和要修改的值。

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
可以使用属性的[[Q&A#子修改父的值方法 |.sync 修饰符。 ]] 

### vue指令 v-model修饰符

> 给v-model 封装了一些功能

* 语法:
* v-model.修饰符="vue数据变量"

- 修饰符：
* .number 以parseFloat转成数字类型
* .trim 去除首尾空白字符
* .lazy 在change时触发而非input时（输入完毕时触发）

```vue

<template>

<div>

<div>

<span>年龄:</span>

<input type="text" v-model.number="age">

</div>

<div>

<span>人生格言:</span>

<input type="text" v-model.trim="motto">

</div>

<div>

<span>自我介绍:</span>

<textarea v-model.lazy="intro"></textarea>

</div>

</div>

</template>

  

<script>

export default {

data() {

return {

age: "",

motto: "",

intro: ""

}

}

}

</script>

```

### vue指令 v-text和v-html

> innerText/innerHTML的一种封装

* 语法:

* v-text="vue数据变量"   --普通字符串解析
* v-html="vue数据变量"  --当成标签解析
* 
* 注意: 会覆盖插值表达式

```vue

<template>

<div>

<p v-text="str"></p>

<p v-html="str">{{22 + 11}}</p>

</div>

</template>

  

<script>

export default {

data() {

return {

str: "<span>我是一个span标签</span>"

}

}

}

</script>

```

### vue指令 v-show和v-if

> 都能控制标签的隐藏或出现

* 语法:
* v-show="vue变量"   --变量的值为布尔值
* v-if="vue变量"          --变量的值为布尔值 v-if采用惰性加载

* 原理
* v-show 用的display:none隐藏 (频繁切换使用)
* v-if 直接从DOM树上移除  （直接删除时使用）

* 高级
* v-if v-else 和v-else-if 使用 （用法相同，就是if判断，然后else直接返回否的结果，或者else-if接着判断）

```html

<template>

<div>

<h1 v-show="isOk">v-show的盒子</h1>

<h1 v-if="isOk">v-if的盒子</h1>


<div>

<p v-if="age > 18">我成年了</p>

<p v-else>还得多吃饭</p>

</div>

</div>

</template>

  

<script>

export default {

data() {

return {

isOk: true,

age: 15

}

}

}

</script>

```

### vue指令-v-for

> 可以根据数组 / 对象 / 数字 / 字符串 (可遍历结构)遍历标签

>v-for就是根据数组或者对象里面值的数量，创建相应结构的组件目录。各个组件与各个值相互对应。组件之间，各个值之间相互独立。

>所谓遍历标签，就是在逻辑上创建了好几个相同的组件（组件之间是独立的），传给各个组件的值之间也是独立的。

>==也即是说，每个组件对应是独立的数据层。==
* 语法
* v-for="(值变量, 索引变量 / key变量) in 目标结构"  --循环数组或者对象
* v-for="值变量 in 目标结构"         --循环数字

* 注意:
- v-for的临时变量名不能用到v-for范围外
- 让谁循环生成, v-for就写谁身上 


```vue

<template>

<div id="app">

<div id="app">

<!-- v-for 把一组数据, 渲染成一组DOM -->

<!-- 口诀: 让谁循环生成, v-for就写谁身上 -->

<p>学生姓名</p>

<ul>

<li v-for="(item, index) in arr" :key="item">

{{ index }} - {{ item }}

</li>

</ul>

  

<p>学生详细信息</p>

<ul>

<li v-for="obj in stuArr" :key="obj.id">

<span>{{ obj.name }}</span>

<span>{{ obj.sex }}</span>

<span>{{ obj.hobby }}</span>

</li>

</ul>

  

<!-- v-for遍历对象(了解) -->

<p>老师信息</p>

<div v-for="(value, key) in tObj" :key="value">

{{ key }} -- {{ value }}

</div>

  

<!-- v-for遍历整数(了解) - 从1开始 -->

<p>序号</p>

<div v-for="i in count" :key="i">{{ i }}</div>

</div>

</div>

</template>

  

<script>

export default {

data() {

return {

arr: ["小明", "小欢欢", "大黄"],

stuArr: [

{

id: 1001,

name: "孙悟空",

sex: "男",

hobby: "吃桃子",

},

{

id: 1002,

name: "猪八戒",

sex: "男",

hobby: "背媳妇",

},

],

tObj: {

name: "小黑",

age: 18,

class: "1期",

},

count: 10,

};

},

};

</script>

```

> 总结: vue最常用指令, 铺设页面利器, 快速把数据赋予到相同的dom结构上循环生成
 Vue 处理指令时，v-for 比 v-if 具有更高的优先级, 虽然用起来也没报错好使, 但是性能不高, 如果你有5个元素被v-for循环, v-if也会分别执行5次.

### vue指令-v-for更新监测

> 就是数据的双向绑定是否触发? 后台的数据变了，前端是否实时变化。
 
 ==改变原数组的方法才能让v-for更新
- 情况1: 改变数组结构的方法     --运用js方法，改变数组的结构，可以更新

```js
//这些方法会触发数组改变, v-for会监测到并更新页面
`push()`

`pop()`

`shift()`

`unshift()`

`splice()`

`sort()`

`reverse()`

```

- 情况2: 不改变数组结构的方法    --所以不可以更新

```js

* `slice()`

* `filter()`

* `concat()`

// 数组上述方法不会造成v-for更新，不会改变原始数组

export default {

data(){

return {

arr: [5, 3, 9, 2, 1]

}

}

}

// 解决v-for更新 - 覆盖原始数组

let newArr = this.arr.slice(0, 3)

this.arr = newArr

},

```

- 情况3: 更新值        --直接数组赋值改变数组的值，不改变数组结构，所以不更新

```js

this.arr[0] = 1000;// 更新某个值的时候, v-for是监测不到的

//用-this.$set()或者Vue.set()方法解决

this.$set(this.arr, 0, 1000)

// 参数1: 更新目标结构

// 参数2: 更新位置

// 参数3: 更新值

```

### vue指令-v-for就地更新
>`v-for` 循环出新的虚拟DOM结构, 和旧的虚拟DOM 结构对比, 尝试复用标签就地更新内容
- 这样有问题，比如插入，就是会把后面的标签结构也更新，因为把后边的挤掉了。
![[image-20210414215044425.png]]


### 自定义指令-注册

  [自定义指令文档](https://www.vue3js.cn/docs/zh/guide/custom-directive.html)
- 自定义指令的功能和内置指令一样，就是自己封装里面的功能。
- 可以获取组件里的标签dom, 给自定义指令封装函数对象，已达到封装功能的目的。

#### 局部注册和使用
- 在当前的组件里封装，注册，使用，指令也只能在当前组件里注册使用。
语法：
```js
export default {

directives: {
指令名：{
inserted(el, options){        //指令所在标签, 被插入到网页上触发(一次)
     //el 获得使用指令的标签dom
     //指令的默认值，也就是传给指令的变量，但是要想获取变量的值，要 options.value获取

},
update(el, options){      //指令对应数据/标签更新时, 此方法执行
     
bind(绑定事件触发){

}
}
}
}

} 

```

- 案例-在当前组件.vue文件中使用

```vue

<template>

<div>


<input type="text" v-focus>

</div>

</template>

  

<script>

// 目标: 创建 "自定义指令",

// 1. 创建自定义指令

// 全局 / 局部

// 2. 在标签上使用自定义指令 v-指令名

// 注意:

// inserted方法 - 指令所在标签, 被插入到网页上触发(一次)

// update方法 - 指令对应数据/标签更新时, 此方法执行

export default {

data(){

return {

colorStr: 'red'

}

},

directives: {

focus: {

inserted(el){

el.focus()

}

}

}

}

</script>

  

<style>

</style>

```

#### 全局注册
- 在入口main.js里面封装，注册，使用。可以全部组件都可以注册使用。（注意，封装还是在入口js里面封装）

语法：
```vue
vue.directive("指令名"， {

inserted(el){              //指令所在标签, 被插入到网页上触发(一次)
                           //el获得的是真实dom

},

update(el){      //指令对应数据/标签更新时, 此方法执行
     

}

})
```

- 案例-在入口js里面写
```js

// 全局指令 - 到处"直接"使用

Vue.directive("gfocus", {

inserted(el) {

el.focus() // 触发标签的事件方法

}

})

```


### 自定义指令-传值

> 像使用内置的指令一样，自定义指令也能传入值。
- 因为传值意味这改变数据，inserted方法不会随时更新页面，所以要用update方法
  语法：
```vue
<!--这里以全局指令为例-->

Vue.directive('color', {

<!--binding对象接受标签传入的值，注意，里面有多个属性，value属性对应的才是真正的值-->

update(el, binding) {  <!--el接收的是真实dom-->

el.style.color = binding.value

}

})
  
```


- 案例-全局注册color指令

```js

// 目标: 自定义指令传值

Vue.directive('color', {

inserted(el, binding) {

el.style.color = binding.value

},

update(el, binding) {

el.style.color = binding.value

}

})

```
- 注意：标签传值给自定义指令时，传入的默认是变量
```vue
<input type="text" v-focus="变量">  <!--如果传入固定的值要"'值'"-->
```

## vue基础

### vue基础_MVVM设计模式

>设计模式: 是一套被反复使用的、多数人知晓的、经过分类编目的、代码设计经验的总结。

>MVVM通过`数据双向绑定`让数据自动地双向同步 **不再需要操作DOM**
+ MVVM，一种软件架构模式，决定了写代码的思想和层次
+ M： model数据模型 (data里定义)
+ V： view视图 （html页面）
+ VM： ViewModel视图模型 (vue.js源码)
![[双向数据绑定.png]]

- V（修改视图） -> M（数据自动同步）
- M（修改数据） -> V（视图自动同步）

>就是通过vue实现内部数据与视图数据实时同步。

### vue基础_插值表达式

>  给dom标签中, 插入内容（变量的值），又叫声明式渲染/文本插值

- 语法: {{ 表达式 }}  --不是语句，只能是表达式（常量加运算符，能直接有结果）

```jsx

<template>

<div>

<h1>{{ msg }}</h1>

<h2>{{ obj.name }}</h2>

<h3>{{ obj.age > 18 ? '成年' : '未成年' }}</h3>

</div>

</template>

  

<script>

export default {

data() { // 格式固定, 定义vue数据变量

return { // key相当于变量名

msg: "hello, vue",

obj: {

name: "小vue",

age: 5

}

}

}

}

</script>

  

<style>

</style>

```

> 总结: dom中插值表达式赋值, vue的变量必须在data里声明

  

### vue基础_虚拟dom

>.vue文件中的template里写的标签, 都是模板, 都要被vue处理成虚拟DOM对象, 才会渲染显示到真实DOM页面上

>虚拟DOM保存在内存中, 只记录dom关键信息, 配合diff算法提高DOM更新的性能

1. 内存中生成一样的虚拟DOM结构(==本质是个JS对象==)

- template里标签结构

```vue

<template>

<div id="box">

<p class="my_p">123</p>

</div>

</template>

```

- 对应的虚拟DOM结构

```js

const dom = {

type: 'div',

attributes: [{id: 'box'}],

children: {

type: 'p',

attributes: [{class: 'my_p'}],

text: '123'

}

}

```

  
2. 通过虚拟dom提高更新效率

* 生成新的虚拟DOM结构
* 和旧的虚拟DOM结构对比
![[image-20210414215426783.png]]

* 利用diff算法, 找不不同, 只更新变化的部分(重绘/回流)到页面 - 也叫打补丁

==好处1: 提高了更新DOM的性能(不用把页面全删除重新渲染)==

==好处2: 虚拟DOM只包含必要的属性(没有真实DOM上百个属性)==

### vue基础_diff算法

>vue用diff算法, 新虚拟dom, 和旧的虚拟dom比较

1.  根元素变了, 子元素全部删除重建
2. 各个元素没变, 属性改变, ==元素复用==, 更新属性
3. 子元素改变, 元素内容改变（插入或者删除）
	-  无key - 就地更新。v-for不会移动DOM, 而是尝试复用, 就地更新，这就导致后边的标签变化，造成不必要更新。
	
	>因为新旧dom对比，发现标签结构变化就直接把变化的全部就地更新
	
![[image-20210414215502653.png]]


- 有key - 值为索引，还是就地更新

>key的作用就是以key为标准精确判断dom结构（新旧dom的key值应该是一一对应不重复的）, 但是这里新旧dom-key值索引不会随着结构变化（索引是数组的排序，是固定的）所有还是就地更新

![[image-20210414215525492.png]]
  
- 有key - 值为id,精确更新
- key的值只能是唯一不重复的, 字符串或数值
- 新DOM里数据的key存在, 去旧的虚拟DOM结构里找到key标记的标签, 复用标签
- 新DOM里数据的key存在, 去旧的虚拟DOM结构里没有找到key标签的标签, 创建

>这样就能精确判断哪一个变化了，哪一个没有变化。就可以直接移动dom，不需要再次更新后面的结构了。
![[image-20210414215546869.png]]

  

### vue基础_动态class

>用v-bind给标签属性class设置动态的值,就是class属性是否赋值的布尔判断

* 语法:  :class="{类名: 布尔值}"
- 使用场景: vue变量控制标签是否应该有类名
- 语法二： :class = "[变量， 变量]"   有 变量 = 类名 就加， 没有就不加
- 使用场景： 变量控制应该添加什么样的类名
```vue

<template>

<div>

<p :class="{red_str: bool}">动态class</p>

</div>

</template>

  

<script>

export default {

data(){

return {

bool: true

}

}

}

</script>

  

<style scoped>

.red_str{

color: red;

}

</style>

```

  

### vue基础_动态style

> 用v-bind给标签属性style设置动态的值,就是用赋值的方式设置css样式

* 语法
* :style="{css属性: '值'}"

```vue

<template>

<div>

<!-- 动态style语法

:style="{css属性名: 值}"

-->

<p :style="{backgroundColor: colorStr}">动态style</p>

</div>

</template>

  

<script>

export default {

data(){

return {

colorStr: 'red'

}

}

}

</script>

<style>

</style>

```

## vue过滤器
### vue过滤器-定义使用

>为了转换格式, 过滤器就是一个**函数**, 传入值返回处理后的值；

过滤器场景：
* 字母转大写, 输入"hello", 输出"HELLO"
* 字符串翻转, "输入hello, world", 输出"dlrow ,olleh"

方法一：
* Vue.filter("过滤器名", (值) => {return "返回处理后的值"})  
	 --是一个方法，全局过滤器写在main.js（只能定义一个）
	```vue
	vue.filter("filter", () => {})
	```


方法二：
* filters: {
   过滤器名字 (值) => {return "返回处理后的值"} 
   }
	 --是一个函数体赋值的变量，局部过滤器写在某vue文件里（可以写多个）
```js
export  default{

data(){

return {

a: 10,

b: 20

}

},
}

//filters是配置项，toUp才是定义的变量
   
filters: {
toUp (val) {
return val.toUpperCase()
}
}
```

用法：
  {{ 值 | 过滤器名字(实参) }}
- 过滤器只能用在, ==插值表达式和v-bind表达式==。

### vue过滤器-传参和多过滤器

> 可同时使用多个过滤器, 或者给过滤器传参

* 用法:
* 过滤器传参:   vue变量 | 过滤器(实参) 

```vue
// 例：{{ msg | toUp('|') }}


filters: {
// str：第一个参数是固定的，是数据本身msg
// 2.从第二个开始接受的就是具体的参数，这里用 s 接收
toUp (str, s) {

return str.toUpperCase().join(s)

}

}
```

* 多个过滤器:    vue变量 | 过滤器1 | 过滤器2
```html
<p :title="msg | toUp | reverse('|')">鼠标长停</p>
```

## vue计算属性

### vue计算属性-computed

> 定义：要用的属性不存在，要通过已有的属性计算得来
> 原理：底层借助了Object.defineproperty方法提供的getter和setter

- 注意: ==计算属性==和data属性都是==变量==-不能重名
- 注意2: 函数内变量变化, 会自动重新计算结果返回
- 在页面端赋值计算属性要用完整写法-自定义添加set（setter）
- 计算属性默认getter，在数据端赋值给变量，一定要返回值赋值，（getter）

语法：   
computed: {
==属性名== (){}      --基本语法里默认getter，里面写计算（返回值）赋值给属性
}
```js
//<p>{{ num }}</p>

export default {

data(){

return {

a: 10,

b: 20

}

},

//computed是一个配置项，num才是定义的变量又叫计算属性
computed: {

num(){
//一定要返回一个值
return this.a + this.b

}


```

用法：
- 直接调用{{ num }}

### vue计算属性-缓存

>  计算属性是基于它们的依赖项的值结果进行缓存的，只要依赖的变量不变, 都直接从缓存取结果


计算属性优势:
- 计算属性对应函数执行后, 会把return值缓存起来

- 依赖项不变, 多次调用都是从缓存取值

- 依赖项值-变化, 函数会"自动"重新执行-并缓存新的值
```js

export default {

data(){

return {

msg: "Hello, Vue"

}

},

//和data并列的变量
computed: {

reverseMessage(){

console.log("计算属性执行了");

return this.msg.split("").reverse().join("")

}

},

methods: {

getMessage(){

console.log("函数执行了");

return this.msg.split("").reverse().join("")

}

}

}


```

> 总结: 计算属性根据依赖变量结果缓存, 依赖变化重新计算结果存入缓存, 比普通方法性能更高

### vue计算属性-完整写法

> 计算属性也是变量, 如果想要在页面端直接赋值, 需要使用完整写法
- 计算属性本意是数据层赋值，默认getter方法，return数据赋值给计算属性
- 当其他地方也想赋值给计算属性时，就要用setter方法，得到赋值的实参
简单写法:
```vue
computed: {

'属性名'(){      --给计算属性赋值一个函数体，内置getter
return ""
}
}

```

完整写法：
```js

computed: {

"属性名": {
// 给计算属性赋值触发set方法
set(值){

},
// 使用计算属性的值触发get方法
get() {

return "值"

}

}

}

```

## vue侦听器

### vue侦听器-watch

> 可以侦听data/computed属性值改变

语法:

```js

//watch是配置项
watch: {

"被侦听的属性名" (newVal, oldVal){

}

}

```

> 总结: 想要侦听一个属性变化, 可使用侦听属性watch


方法二：
- 与watch的核心原理是一致的，但是不需要组件初始化的时候就必须监听， 而是可以在任何位置开启监听。

```js
this.$watch('value', () => {

editor.txt.html(this.value)

})
```
### vue侦听器-深度侦听和立即执行

> 侦听复杂类型（对象或者数组）, 或者立即执行侦听函数
- 其实就是监听对象里面属性的值，对象的属性变才需要监听，所以要加deep属性深度监听
- handler里回收的是监听的对象，里面有这个对象所有属性。

* 语法:
```js

watch: {

"要侦听的属性名": {

immediate: true, // 立即执行

deep: true, // 深度侦听复杂类型内变化

handler (newVal, oldVal) {
//val是监听的对象
}

}

}

```

完整例子代码:
```vue

<template>

<div>

<input type="text" v-model="user.name">

<input type="text" v-model="user.age">

</div>

</template>

<script>

export default {

data(){

return {

user: {

name: "",

age: 0

}

}

},

watch: {

user: {

handler(newVal, oldVal){

// newVal里是user里的对象，在这里又打印了一次

console.log(newVal, oldVal);

},

deep: true,

immediate: true

}

}

}

</script>

  

<style>

  

</style>

```




## vue组件
### vue组件_概念

> 组件是可复用的 Vue 实例, 封装标签, 样式和JS代码

>**一个页面， 可以拆分成一个个组件，一个组件就是一个整体, 每个组件可以有自己独立的 结构 样式 和 行为(html, css和js)**

**组件化** ：封装的思想，把页面上 `可重用的部分` 封装为 `组件`，从而方便项目的 开发 和 维护

- 独立的个体的意义在于，他们接收的数据是独立的，不受其他组件影响。（但是UI结构可能相互影响比如label标签）
### 函数式组件
functional为true，表示该组件为一个函数式组件
函数式组件： 没有data状态，没有响应式数据，只会接收props属性， 没有this， 他就是一个函数
```js
<script>

export default {

name: 'MenuItem',

functional: true,

props: {

icon: {

type: String,

default: ''

},

title: {

type: String,

default: ''

}

},

render(h, context) {

const { icon, title } = context.props

const vnodes = []

  

if (icon) {

if (icon.includes('el-icon')) {

vnodes.push(<i class={[icon, 'sub-el-icon']} />)

} else {

vnodes.push(<svg-icon icon-class={icon}/>)

}

}

  

if (title) {

vnodes.push(<span slot='title'>{(title)}</span>)

}

return vnodes

}

}

</script>

  

<style scoped>

.sub-el-icon {

color: currentColor;

width: 1em;

height: 1em;

}

</style>
```
  
### vue组件_基础使用

> 每个组件都是一个独立的个体, 代码里体现为一个独立的.vue文件

==(重要): 组件内data必须是一个函数, 这样引入组件时才可以独立作用域,==
- 给组件命名有两种方式，一种是使用链式命名"my-component"，一种是使用大驼峰命名"MyComponent"

步骤:
1. 创建组件 components/Pannel.vue（各个单vue文件就是独立的个体）
2. 注册组件: 创建后需要注册后再使用

#### 全局 - 注册使用

-  在全局入口main.js, 上注册

	语法:
```js

import Vue from 'vue'

import 组件对象 from 'vue文件路径'

Vue.component("组件名", 组件对象)

```

- 全局注册PannelG组件名后, 就可以当做标签在==任意Vue==文件中template里用
- 使用时会把这个自定义标签当做组件解析, 使用==组件里封装的标签替换到这个位置==

```vue
<PannelG></PannelG>

<PannelG/>
/*单双标签都可以或者小写加-形式, 运行后*/

<pannel-g></pannel-g>

```

#### 局部- 注册使用
- 在单个vue文件上注册

	语法：
	```js

import 组件对象 from 'vue文件路径'

export default {

components: {

"组件名": 组件对象

}

}

```

  当前页面当做标签使用

```vue

<template>

<div id="app">

<h3>案例：折叠面板</h3>

<!-- 4. 组件名当做标签使用 -->

<!-- <组件名></组件名> -->

<PannelG></PannelG>

<PannelL></PannelL>

</div>

</template>

  

<script>

import Pannel from './components/Pannel_1'

export default {

components: {
 //当key值与value值一样可以简写成 pannel
Pannel: Pannel

}

}

</script>

```

组件使用总结:
1. (创建)封装html+css+vue到独立的.vue文件中

2. (引入注册)组件文件 => 得到组件配置对象

3. (使用)当前页面当做标签使用


### vue组件-scoped作用

> 解决多个组件样式名相同, 冲突问题

给.vue组件里style标签上加scoped属性即可

```vue

<style scoped>

```

>在style上加入scoped属性, 就会在此组件的标签上加上一个随机生成的data-v开头的属性
而且必须是当前组件的元素, 才会有这个自定义属性, 才会被这个样式作用到

![[image-20210216122749906.png]]

### vue组件-name属性

> 可以用组件的name属性值, 来注册组件名字

```vue

<template>

<div>

<p>我是一个Com组件</p>

</div>

</template>

  

<script>

export default {

name: "ComNameHaHa" // 注册时可以定义自己的名字

}

</script>

```

App.vue - 注册和使用
```vue

<script>
//引入，注册，使用组件
export default {

components: {

[Com.name]: Com // 对象里的key是变量的话[]属性名表达式

// "ComNameHaHa": Com

}

}

</script>

```


## vue组件通信

- 因为每个组件的变量和值都是独立的，
- 所以每个组件引入到父组件后，是独立的。每次子组件标签运用也是独立的，传的参数也互不影响。
	- 父: 使用其他组件的vue文件
	- 子: 被引入的组件(嵌入)
  
### vue组件通信_父向子-props

>父组件给子组件的变量传参数，父组件用子组件的业务逻辑。
  
  - 父组件来传参，才有利于子组件的复用。一般是App.vue充当总的父组件
步骤:
1. 创建组件MyProduct.vue 
2. 组件内在props定义变量, 用于接收外部传入的值
	- props: [] - 只声明变量, 不能类型校验  
	- props: {
			变量：{
			                default："默认值"  () => {}  （数组和对象的默认值效验要写全）
					type: 类型，
					required：true，  --必传属性
					validator(value){     --自定义检验，检验传来的变量， 要返回布尔值
					}				
	} - 声明变量和校验类型规则 

	
```vue

<template>

<div class="my-product">

<h3>标题: {{ title }}</h3>

<p>价格: {{ price }}元</p>

<p>{{ intro }}</p>

</div>

</template>

  

<script>
// 在子组件内，定义变量，等待传参（子设置接受）
export default {
//props有哪2种定义方式,
//props: [] - 只声明变量, 不能类型校验  
//props: {} - 声明变量和校验类型规则 - 不对则报错
props: ['title', 'price', 'intro']

}

</script>

  

<style>

</style>

```

3. App.vue中引入注册组件, 使用时, 传入具体数据给组件显示

```vue

<template>

<div>

<!--
可以使用多次子组件标签，而且每次都是独立的，可以传不同的参数。（父设置传参）
-->

<Product :title="好吃的口水鸡" :price="50" :intro="开业大酬宾, 全场8折"></Product>

<Product :title="好可爱的可爱多" :price="20" :intro="老板不在家, 全场1折"></Product>

<Product :title="好贵的北京烤鸭" :price="290" :intro="str"></Product>

</div>

</template>

  

<script>

// 引入子组件

import Product from './components/MyProduct'

export default {

data(){

return {

str: "好贵啊, 快来啊, 好吃"

}

},

// 注册组件

components: {

// Product: Product // key和value变量名同名 - 简写

Product

}

}

</script>

  

<style>

  

</style>

```



### vue组件通信_父向子-配合循环

>给子组件标签使用v-for指令

- 每次循环，变量和组件都是独立的

```vue

<template>

<div>

<MyProduct v-for="obj in list" :key="obj.id"
 
:title="obj.proname"  --给子组件传参

:price="obj.proprice"

:intro="obj.info"

></MyProduct>

</div>

</template>

  

<script>
//引入，注册子组件
import MyProduct from './components/MyProduct'

export default {

data() {

return {

list: [

{

id: 1,

proname: "超级好吃的棒棒糖",

proprice: 18.8,

info: "开业大酬宾, 全场8折",

},

{

id: 2,

proname: "超级好吃的大鸡腿",

proprice: 34.2,

info: "好吃不腻, 快来买啊",

},

{

id: 3,

proname: "超级无敌的冰激凌",

proprice: 14.2,

info: "炎热的夏天, 来个冰激凌了",

},

],

};

},

// 3. 注册组件

components: {

// MyProduct: MyProduct

MyProduct

}

};

</script>

  

<style>

</style>

```


### vue组件通信_单向数据流

  - 在vue中需要遵循单向数据流原则

1. 父组件的数据发生了改变，子组件会自动跟着变
2. 子组件不能直接修改父组件传递过来的props，因为props是只读的

![[image-20210423161646951.png]]

==父组件传给子组件的是一个对象，子组件修改对象的属性，是不会报错的，对象是引用类型, 互相更新==


### vue组件通信_依赖注入
>当一堆后代组件共用某一个祖先组件的变量时，一个一个传值太麻烦了。
>所以就在祖先组件里面设置provide 属性。用inject接收。

- 不是响应式的传值
1. 祖先组件里设置传值
```js
export default {

provide () {

return {

articleId: this.articleId

}
  
}
}

```

2. 后代组件里设置接受
```js

export default {

inject: {

	articleId: {

		type: [Number, String, Object],

		default: null

}

}
	}
```

### vue组件通信_子向父

> 从子组件把值传出来给外面使用
1. 父组件传值子组件的逻辑就是父给变量赋值，子设置接受变量
2. 子传父也是，父设置接受变量，子给变量赋值

语法:
* 父: @自定义事件名="父methods函数"（父设置接受变量）
* 子: this.$emit("自定义事件名", 传值) - 执行父methods里函数代码（子设置传值）（这里立即执行，相当于子立即执行父的事件方法，并且传值给父的事件方法）
*  父自定义事件和方法, 等待子组件触发事件后给方法传值
==注意，父組件可以用$event接收事件參數，也就是說子組件的传值父组件可以 $event接收==

流程：
子触发传值语句，传值给父方法，父自定义事件调用父方法，处理传来的值。
![[image-20210217102551882.png]]

子组件：
```vue

<template>

<div class="my-product">

<h3>标题: {{ title }}</h3>

<p>价格: {{ price }}元</p>

<p>{{ intro }}</p>

<button @click="subFn">宝刀-砍1元</button>

</div>

</template>

  

<script>

import eventBus from '../EventBus'

export default {

props: ['index', 'title', 'price', 'intro'],

methods: {

subFn(){

this.$emit('subprice', this.index, 1) // 子向父

eventBus.$emit("send", this.index, 1) // 跨组件

}

}

}

</script>

  

<style>

.my-product {

width: 400px;

padding: 20px;

border: 2px solid #000;

border-radius: 5px;

margin: 10px;

}

</style>

```

父组件：

```vue

<template>

<div>

<!-- 目标: 子传父 -->

<!-- 1. 父组件, @自定义事件名="父methods函数" -->

<MyProduct v-for="(obj, ind) in list" :key="obj.id"

:title="obj.proname"

:price="obj.proprice"

:intro="obj.info"

:index="ind"

@subprice="fn"

></MyProduct>

</div>

</template>

  

<script>

  

import MyProduct from './components/MyProduct_sub'

export default {

data() {

return {

list: [

{

id: 1,

proname: "超级好吃的棒棒糖",

proprice: 18.8,

info: "开业大酬宾, 全场8折",

},

{

id: 2,

proname: "超级好吃的大鸡腿",

proprice: 34.2,

info: "好吃不腻, 快来买啊",

},

{

id: 3,

proname: "超级无敌的冰激凌",

proprice: 14.2,

info: "炎热的夏天, 来个冰激凌了",

},

],

};

},

components: {

MyProduct

},

methods: {

fn(inde, price){

// 逻辑代码

this.list[inde].proprice > 1 && (this.list[inde].proprice = (this.list[inde].proprice - price).toFixed(2))

}

}

};

</script>

  

<style>

</style>

```


### vue组件通信-EventBus

> 两个组件不用父子引用时，可以用eventbus通信，通讯方案：事件总线（event-bus)

核心语法：

- EventBus/index.js- 定义事件总线bus对象（创建空白对象）
- 就是在一个模块化的js文件里，导出一个vue空白对象
- 然后双方组件都引入这个带有空vue对象的模块化js
```js

import Vue from 'vue'

// 导出空白vue对象

export default new Vue()

```

List.vue注册事件 - 等待接收值的组件 
- 创建on监听事件，等待接受值

```vue
 

<script>
data () {

return {

arr [
1, 2, 3, 4

]

}
}

// 目标: 跨组件传值

// 1. 引入空白vue对象(EventBus)

// 2. 接收方 - $on监听事件

import eventBus from "../EventBus";

export default {


// 3. 组件创建完毕, 监听send事件

created() {
// 和子传父一样，设置等待接受值的形参，等待另一个组件传值
eventBus.$on("send", (index, price) => {

this.arr[index].proprice > 1 &&

(this.arr[index].proprice = (this.arr[index].proprice - price).toFixed(2));

});

},

};

</script>


<style>


</style>

```

- components/MyProduct_sub.vue在另一个组件中设置传值的实参

```vue


<script>
// 1. 引入空白vue对象(EventBus)
import eventBus from '../EventBus'

export default {


methods: {

//和子向父一样，设置方法传输实参给接受数据组件的自定义事件

subFn(){

this.$emit('subprice', this.index, 1) // 子向父

eventBus.$emit("send", this.index, 1) // 跨组件

}

}

}

</script>

  

<style>


</style>

```


> 总结: 空的Vue对象, 只负责$on注册事件, $emit触发事件, 一定要确保 $ on先执行

![[image-20210309122823432 1.png]]
### vue爷孙通信-$attrs $listeners

在 Vue 2.4 版本中加⼊的 `$attrs` 和 `$listeners` 可以用来作为跨级组件之间的通信机制。 (父传孙)

**父组件**

```jsx
<template>
  <div>
    <my-child1 :money="100" desc='你好哇' @test1="fn1" @test2="fn2"></my-child1>
  </div>
</template>
```



**子组件**

```vue
<template>
  <div class="my-child1">
    <!-- $attrs => { "money": 100, "desc": "你好哇" } -->
    <div>{{ $attrs }}</div>
    <my-child2 v-bind="$attrs" v-on="$listeners" ></my-child2>
  </div>
</template>

<script>
import MyChild2 from './my-child2'
export default {
  created () {
    console.log(this.$listeners)
  },
  components: {
    MyChild2
  }
}
</script>
```

**孙组件**

```jsx
<template>
  <div>
    我是child2 - {{ money }} - {{ desc }}
    <button @click="clickFn">按钮</button>
  </div>
</template>

<script>
export default {
  props: ['money', 'desc'],
  methods: {
    clickFn () {
      this.$emit('test1', '嘎嘎')
      this.$emit('test2', '嘿嘿')
    }
  }
}
</script>
```




## vue组件进阶

### 组件进阶 - 动态组件

> 多个组件使用同一个挂载点，并动态切换，这就是动态切换不同的组件

>其实就是让引入的组件变成值赋值给变量，这样就可以来回变换不同的值，也就是不同的组件，实现动态变化。
>应该是vue内置component组件, 配合is属性, 接收引入的组件


步骤：
1. 准备被切换的 - UserName.vue / UserInfo.vue 2个组件

2. 引入到UseDynamic.vue注册

3. 准备变量来承载要显示的"组件名"

4. 设置挂载点< component >, 使用is属性来设置要显示哪个组件

5. 点击按钮 – 修改comName变量里的"组件名"

```vue

<template>

<div>

<button @click="comName = 'UserName'">账号密码填写</button>

<button @click="comName = 'UserInfo'">个人信息填写</button>

  

<p>下面显示注册组件-动态切换:</p>

<div style="border: 1px solid red;">

<component :is="comName"></component>

</div>

</div>

</template>


<script>


import UserName from '../components/01/UserName'

import UserInfo from '../components/01/UserInfo'

export default {

data(){

return {

comName: "UserName"

}

},

components: {

UserName,

UserInfo

}

}

</script>

```


### 组件进阶 - 组件缓存

> 组件切换会导致组件被频繁（destroy）销毁和重新创建（created）生命周期转换，性能不高

- 使用Vue内置的keep-alive组件, 可以让包裹的组件不执行销毁、重建的生命周期，而是==激活==，==未激活== 的声明周期

语法:

​ Vue内置的keep-alive组件 包起来要频繁切换的组件

```vue

<div style="border: 1px solid red;">

<!-- Vue内置keep-alive组件, 把包起来的组件缓存起来 -->

<keep-alive>

<component :is="comName"></component>

</keep-alive>

</div>

```

[[VUE#vue生命周期|补充生命周期]]：
>在组件内部可以调用这些钩子函数

* activated - 激活时触发 
>在在组件第一次渲染时会被调用，之后在每次缓存组件被激活时调用。

>第一次进入缓存路由/组件，在mounted后面，beforeRouteEnter守卫传给 next 的回调函数之前调用，并且给因为组件被缓存了，再次进入缓存路由、组件时，不会触发这些钩子函数，beforeCreate created beforeMount mounted 都不会触发

* deactivated - 失去激活状态触发
>组件被停用（离开路由）时调用。

>使用keep-alive就不会调用beforeDestroy(组件销毁前钩子)和destroyed(组件销毁)，因为组件没被销毁，被缓存起来了，这个钩子可以看作beforeDestroy的替代，如果你缓存了组件，要在组件销毁的的时候做一些事情，可以放在这个钩子里，组件内的离开当前路由钩子beforeRouteLeave => 路由前置守卫 beforeEach =>全局后置钩子afterEach => deactivated 离开缓存组件 => activated 进入缓存组件(如果你进入的也是缓存路由)


> 总结: keep-alive可以提高组件的性能, 内部包裹的标签不会被销毁和重新创建, 触发激活和非激活的生命周期方法


### 组件进阶 - 组件插槽

> 用于实现组件的内容分发, 通过 slot 标签, 可以接收到写在组件标签内的内容

> 组件内容分发技术, slot占位, 使用组件时传入替换slot位置的标签

- 其实就是组件的模板，组件本身就是模板，靠被人引入使用。组件的插槽就是这个模板的模板，靠别人给他传入标签结构
- 流程就是，父组件（引用的组件） ==> 传标签给子组件（被引用的组件）  ==> 形成新的模板组件   ==>   再被父组件（引用的组件）使用 


步骤：

1. 组件内用< slot >< /slot >占位
```vue
<template>

<div>

<!-- 按钮标题 -->

<div class="title">

<h4>芙蓉楼送辛渐</h4>

<span class="btn" @click="isShow = !isShow">

{{ isShow ? "收起" : "展开" }}

</span>

</div>

<!-- 下拉内容 -->

<div class="container" v-show="isShow">

<!-- 用< slot >< /slot >占位 -->

<slot></slot>

</div>

</div>

</template>

  

```
2. 使用组件时< Pannel >< /Pannel >夹着的地方, 传入标签替换slot
  
```vue

views/03_UseSlot.vue - 组件插槽使用


<template>

<div id="container">

<div id="app">

<h3>案例：折叠面板</h3>

<Pannel>

<img src="../assets/mm.gif" alt="">

<span>我是内容</span>

</Pannel>

<Pannel>

<p>寒雨连江夜入吴,</p>

<p>平明送客楚山孤。</p>

<p>洛阳亲友如相问，</p>

<p>一片冰心在玉壶。</p>

</Pannel>

<Pannel></Pannel>

</div>

</div>

</template>

  

<script>

import Pannel from "../components/03/Pannel";

export default {

components: {

Pannel,

},

};

</script>

```

  

### 组件进阶 - 插槽默认内容


> 如果外面不给传, 想给个默认显示内容


步骤：
- < slot >夹着内容默认显示内容
	- 如果不给插槽slot传东西, 显示默认内容
	- 如果传入标签，就显示传入的标签

```vue

<slot>默认内容</slot>

```

  
### 组件进阶 - 具名插槽

> 当一个组件内有2处以上需要外部传入标签的地方，也就是有两个slot占位标签

  
步骤：
1. 被使用组件内有两个slot占位时，就需要给slot标签命名：name="插槽"
2. 使用组件时, 插槽内传入的标签要用template标签包裹，并且要用 v-slot:插槽名（或者#插槽）。
3. 使用组件时，插槽传入的便签也可以直接添加 slot = ”name“，代替v-slot那种写法（这种写法要在template里写）

被引用的组件内：
```vue

<template>

<div>

<!-- 按钮标题 -->

<div class="title">

<slot name="title"></slot>

<span class="btn" @click="isShow = !isShow">

{{ isShow ? "收起" : "展开" }}

</span>

</div>

<!-- 下拉内容 -->

<div class="container" v-show="isShow">

<slot name="content"></slot>

</div>

</div>

</template>

```

  应用的组件：
```vue

<template>

<div id="container">

<div id="app">

<h3>案例：折叠面板</h3>

<Pannel> 

<template v-slot:title> //或者#title

<h4>芙蓉楼送辛渐</h4>

</template>

<template v-slot:content>  //或者#content

<img src="../assets/mm.gif" alt="">

<span>我是内容</span>

</template>

</Pannel>



<script>

import Pannel from "../components/04/Pannel";

export default {

components: {

Pannel,

},

};

</script>

```


### 组件进阶 - 作用域插槽

>在父组件要给子组件插槽传值，并且要用子组件里面的变量时，可以用作用域插槽

使用场景：插槽可以自定义标签, 作用域插槽可以把组件内的值取出来自定义内容，可以让组件更加灵活的适用于不同的场景和项目。

步骤：

1. 子组件, 在slot上绑定属性和子组件内的值
```vue

<template>

<div>
<!-- 下拉内容 -->

<div class="container" v-show="isShow">

给占位符添加自定义属性，把变量赋值（v-bind）给属性上

<slot :row="defaultObj">{{ defaultObj.defaultOne }}</slot>

</div>

</div>

</template>

```

2. 父（使用）组件, 传入自定义标签, 用template和v-slot="自定义变量名"（和具名插槽很像）

```vue
<template>

<Pannel>

<!-- 需求: 插槽时, 使用组件内变量 -->

<!-- scope变量: {row: defaultObj} -->

<template v-slot="scope">

<p>{{ scope.row.defaultTwo }}</p>

</template>

</Pannel>

</template>
```
3.   然后使用(父)组件v-slot="变量" ,变量名会收集子组件的slot身上属性和值形成对象

```vue
<template v-slot="scope">

<p>{{ scope.row.defaultTwo }}</p>  <!-- scope变量: {row: defaultObj} 把子组件里面的属性和值，变为对象的-->

</template>
```

## Vuex基础

>Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用**`集中式`**存储管理应用的所有组件的状态，并以相应的规则保证状态以一种**`可预测`**的方式发生变化。

- state是放置所有公共状态的属性
- mutations可以修改state数据，并且mutations必须是同步更新
- actions则负责进行异步操作
![[image-20200902235150562.png]]

- vuex是采用集中式管理组件依赖的共享数据的一个工具，可以解决不同组件数据共享问题。


![[Pasted image 20220724000315.png]]


**结论**

1. 修改state状态必须通过**`mutations`**

2. **`mutations`**只能执行同步代码，类似ajax，定时器之类的代码不能在mutations中执行

3. 执行异步代码，要通过actions，然后将数据提交给mutations才可以完成

4. state的状态即共享数据可以在组件中引用

5. 组件中可以调用action

  

### vuex基础-初始化功能


> 建立一个新的脚手架项目, 在项目中应用vuex

```bash

$ vue create demo

```
  
> 开始vuex的初始化建立，选择模式时，选择默认模式

初始化：

- 第一步：`npm i vuex --save` => 安装到**`运行时依赖`** => 项目上线之后依然使用的依赖 ,开发时依赖 => 开发调试时使用

> 开发时依赖 就是开开发的时候，需要的依赖，运行时依赖，项目上线运行时依然需要的

- 第二步： **在main.js中** `import Vuex from 'vuex'`

- 第三步：**在main.js中** `Vue.use(Vuex)` => 调用了 vuex中的 一个install方法

- 第四步：`const store = new Vuex.Store({...配置项})`  --实例化一个vuex

- 第五步：在根实例配置 store 选项指向 store 实例对象 （和注册路由一样）
  
```js

import Vue from 'vue'

import Vuex from 'vuex'

Vue.use(vuex)

const store = new Vuex.Store({})

new Vue({

el: '#app',

store

})

```

  
### vuex基础-state

>state是放置所有公共状态的属性，如果你有一个公共状态数据 ， 你只需要定义在 state对象中
  
第一步：**定义state**
  
```js

// 初始化vuex对象

const store = new Vuex.Store({       //实例化一个vuex那一步

state: {              //放入一个state对象

// 管理数据

count: 0          //添加状态属性

}

})

```


 第二步： **组件中使用count**

方法一：**原始形式**- 插值表达式
组件中可以使用 **this.$store** 获取到vuex中的store对象实例，查找实例里面的**state**属性获取**count**状态属性， 但是当多次获取这个属性时，效率就会变低因为多次调用。

```vue

<div> state的数据：{{ $store.state.count }}</div>

```

方法二： **计算属性** - 将state属性定义在计算属性中  --其实就是用计算属性中转一下

```js

// 把state中数据，定义在组件内的计算属性中

computed: {

count () {

return this.$store.state.count

}

}

```


```vue

<div> state的数据：{{ count }}</div>       //这里引用时其实引用的是计算属性，所以直接count

```

方法三： **辅助函数** - mapState       --其实就是一个转换计算属性的封装

>mapState是辅助函数，帮助我们把store中的数据映射到 组件的计算属性中, 它属于一种方便用法

  
	 第一步：导入mapState

```js

import { mapState } from 'vuex'

```

	第二步：采用数组形式引入state属性

```js
computed: {

...mapState(['count', 'age'])       //利用延展运算符将导出的状态映射给计算属性，因为数组里面有两个属性

}
```


> 上面代码的最终得到的是 **类似** 其实展开还是计算属性


```js

count () {

return this.$store.state.count

},

age () {

return this.$store.state.age

}

```

	第三步:  UI中应用还是引用计算属性的方法引用

```vue

<div> state的数据：{{ count }}</div>

```

### vuex基础-getters

> 除了state之外，有时我们还需要从state中派生出一些状态，这些状态是依赖state的，此时会用到getters  其实就是getters里面的计算属性

例如，state中定义了list，为1-10的数组，组件中，需要显示所有大于5的数据

```js

state: {

list: [1,2,3,4,5,6,7,8,9,10]

}

```

第一步： **定义getters**

- 其实虽然是方法，但是返回的还是一个属性值，所以就是当属性用

```js

getters: {

// getters函数的第一个参数是 state

// 必须要有返回值

filterList: state => state.list.filter(item => item > 5)

}

```

  
第二步：使用getters

方法一： **原始方式** -$store

```vue

<div>{{ $store.getters.filterList }}</div>  --其实虽然是方法，但是返回的还是一个属性值

```

方法二：**辅助函数** - mapGetters

```js

import { mapGetters } from 'vuex'

computed: {

...mapGetters(['filterList'])

}

```

```vue

<div>{{ filterList }}</div>  --这里其实值已经卸在了计算属性里，计算逻辑都在定义的方法里

```

  

### vuex基础-mutations
  
> state数据的修改只能通过mutations，并且mutations必须是同步更新，目的是形成**`数据快照`**
  
数据快照：一次mutation的执行，**立刻**得到一种视图状态，因为是立刻，所以必须是同步

第一步：**定义mutations**    
>在vuex中的store对象实例中定义mutations，它是一个对象，对象中存放修改state的方法


```js

const store = new Vuex.Store({

state: {

count: 0

},

// 定义mutations

// 方法里参数 第一个参数是当前store的state属性

// payload 载荷 运输参数 调用mutaiions的时候 可以传递参数 传递载荷,就是外界调用这个方法修改state时，可以传递参数进来

mutations: {

addCount (state, payload) {

state.count += 1 

}

}

})

```

第二步： 在组件中调用mutations

方法一：**原始形式**-$store

> 在组件里的方法对象里定义方法（名字随意），$store获取实例对象，提交给这个实例两个实参，一个是要调用的方法（mutations里的），一个是传给这个方法的payload载荷实参

在组件里面
``` vue

<template>

<button @click="addCount">+1</button>

</template>


<script>

export default {

methods: {

// 调用方法

addCount () {

// 调用store中的mutations 提交给muations

// commit(方法名, 载荷参数)

this.$store.commit('addCount', 10) // 直接调用mutations

}

}

}

</script>

```

在mutations里面

```js
const store = new Vuex.Store({

state: {

count: 0

},

mutations: {

addCount (state, payload) {

state.count += payload
}

}

})

}

```

方法二：**辅助函数** - mapMutations

> mapMutations和mapState很像，它把位于mutations中的方法提取了出来，我们可以将它导入

在组件中引入
```js

import { mapMutations } from 'vuex'

methods: {

...mapMutations(['addCount'])

}

```

在组件中使用
```vue

<button @click="addCount(100)">+100</button>  //在方法中传入载荷参数

注意：这里必须传入载荷参数，不然就会默认传入事件参数对象$event
vue中的事件方法默认第一个实参参数，就是事件参数对象 

```

> 上面代码的含义是将mutations的方法导入了methods中，等同于

```js

methods: {

// commit(方法名, 载荷参数)

addCount () {

this.$store.commit('addCount')

}

}

```

==请注意： Vuex中mutations中要求不能写异步代码，如果有异步的ajax请求，应该放置在actions中==

### vuex基础-actions

> state是存放数据的，mutations是同步更新数据，actions则负责进行异步操作

> actions在vuex中的store对象实例中定义mutations，是一个对象，对象中存放异步的方法

第一步：在实例中**定义actions**
  
```js

actions: {

//  context表示当前的store的实例 等同于this.$store, 所有可以获取前面的各种方法属性
//可以通过 context.state 获取状态,
//也可以通过context.commit 来调用mutations方法
//也可以 context.dispatch调用其他的action

getAsyncCount (context, payload) {

setTimeout(function(){

// 一秒钟之后 要给一个数 去修改state

context.commit('addCount', 123)   //这里就是调用了mutations的方法，异步传进去一个数据，再让mutations同步的修改state里面是属性，从而修改组件里的state的引用

}, 1000)

}

}

```

第二步：使用action

方法一：在组件中actions的**原始调用** - $store

```js

addAsyncCount () {

this.$store.dispatch('getAsyncCount'). 

}

```

**传参调用**

```js

addAsyncCount () {

this.$store.dispatch('getAsyncCount', 123)

}

```

  

方法二： 在组件中actions的**辅助函数** -mapActions

  
> actions也有辅助函数，可以将action导入到组件中，和mutations一样，也是卸在methods里

```js

import { mapActions } from 'vuex'

methods: {

...mapActions(['getAsyncCount']) 

}

```

在点击事件中传参

```vue

<button @click="getAsyncCount(111)">+异步</button>

<!--当函数方法里用到了载荷参数，这里也必须传参。vue中的事件方法默认第一个实参参数，是事件参数对象$event -->

```

### vuex总结
vuex其实就是一个数据中介。为了解决数据在组件中通讯的问题，把各个组件里的数据再次封装在了vuex里（子模块里），各个vuex分为三个部分服务于数据：
1. state其中保存数据，只能读不能改。
2. mutations保存更改数据的方法。拥有state对象的实参，从而更改数据。但是异步的方法不能使用。比如axios。
3. actions保存 更改数据，传参mutations方法的方法。拥有this.$.store的实参，从而更改一切。可以异步。
4. vuex使用的过程就是组件调用的过程，一切在vuex里面写好逻辑之后，就等组件调用，
调用state，表示得到数值；
调用mutations，表示更改数据；
调用actions，表示异步过程，它可以调用一切。
调用getters，表示封装数据，或者子模块的快速访问。

==vuex里面保存各个组件的数据逻辑，组件里保存了各个组件的UI逻辑，组件调用vuex里的数据，供页面使用。==
>其实就是大鱼吃小鱼的过程，一切都是为了state中的数据服务。组件作为一个应用方，1.可以直接获取state里的数据，2.可以调用mutations方法，给mutations传参数，让mutations更改state里的数据。3. 可以调用actions方法，给actions传参，让actions给mutations传参，从而更改state的数据。

## Vuex的模块化

>如果把所有的状态都放在state中，当项目变得越来越大的时候，Vuex会变得越来越难以维护，所以把vuex模块化，分成多个子模块。最终目的就是封装各个组件的数据逻辑。组件不在处理数据。
  
![[image-20200904155846709.png]]

### 子模块的命名空间

>默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。
>但是state的调用要记得加上模块名

> 这句话的意思是 刚才的user模块还是setting模块，它的 action、mutation 和 getter 其实并没有区分，都可以直接通过全局的方式$store直接调用.

  ![[image-20200904164007116.png]]
>想保证内部模块的高封闭性，我们可以采用**命名空间** `namespaced`来进行设置,保证了子模块的 `action/mutations`状态的独立性

-  定义命名空间
```js

user: {

namespaced: true,  //在子模块里面添加命名空间

state: {

token: '12345'

},

mutations: {

// 这里的state表示的是user的state

updateToken (state) {

state.token = 678910

}

}

},

```

### 子模块的调用
1. 子模块之间如果没有命名空间，直接用根模块$store直接调用就好，他们之间是相通的。详见命名空间章节。
2. 当子模块之间有命名空间时，模块的state，mutations，action调用是不同的，注意getters一直都属于根模块。像个管理者，管理着各个模块的state（因为getters能获取根模块的state）。以下都默认模块之间有命名空间。


#### 子模块state的调用

 - user模块state中，设置 token属性
- setting模块state中， 设置name属性

```js

const store = new Vuex.Store({

modules: {   //在实例中的modules对象中定义子模块

user: {      //子模块user对象，里面存放vuex不同状态的对象

state: {        //user子模块的state状态

token: '12345'

}

},

setting: {

state: {

name: 'Vuex实例'

}

}

})

```

==方法一：原始形式使用子模块的值
- 此时要获取子模块的状态 需要通过 $store.**`state`**.**`子模块名称`**.**`属性名`** 来获取
- 实际上子模块的state状态的上级是根模块的state状态，所以可以用根模块的state直接调用到子模块的state状态

```vue

<template>

<div>

<div>用户token {{ $store.state.user.token }}</div>  //这里省略了子模块的状态名state，以后引用子模块就不需要写子模块的状态名

<div>网站名称 {{ $store.state.setting.name }}</div>

</div>

</template>

```

==方法二：运用根模块的getters，获取根模块的state，从而获取里面的state.子模块名称.属性名，

```js

getters: {

token: state => state.user.token,

name: state => state.setting.name

}

```

注意：这个getters是根级别的getters==

然后再**通过mapGetters快速访问**
- 相当于就是把子模块里的state属性里的值，通过根的getters，传递给了其他组件使用，从而达到子模块的值可以传出

```js

computed: {

...mapGetters(['token', 'name']) //这里就是getters封装了两个子模块的state状态值

}

``` 

#### 子模块mutations的调用

- ==方法一：**辅助函数-带上模块的属性名路径**

- 因为辅助函数还是根模块的辅助函数，所以还是要带路径
```vue

<script>

import { mapMutations } from 'vuex'

methods: {

...mapMutations(['user/updateToken']),   //这里用根模块的辅助函数引入，但是点击事件引用不了这个方法，还需要再封装一层，

test () {

this['user/updateToken']()   //创建一个方法，调用带路径的user/updateTolken这个方法，为了语法通过，只能用【】来使用这个方法   //其实就是this.user/updateToken() 

}
}
</script>



<template>

<button @click="test">修改token</button>

</template>
  

```

==方法二： **createNamespacedHelpers** 创建基于某个命名空间辅助函数
- 这样就可以像全局模块那样引用了

```vue
<script>
import { mapGetters, createNamespacedHelpers } from 'vuex'

const { mapMutations } = createNamespacedHelpers('user')//这里是创建了基于user这个子模块的命名空间的辅助函数，所以可以methods直接引用
export default {

methods：{
...mapMutations(['updateToken'])        //直接引用子模块的mutations状态里的方法
}

}

</script>

<button @click="updateToken">修改token2</button>

```

==方法三：  当子模块调用另一个子模块时

```js
resetRouter()

// vuex中 user子模块 permission子模块

// 子模块调用子模块的action 默认情况下 子模块的context是子模块的

// 父模块 调用 子模块的action

context.commit('permission/setRoutes', [], { root: true })

// 子模块调用子模块的action 可以 将 commit的第三个参数 设置成 { root: true } 就表示当前的context不是子模块了 而是父模块

}
```


#### 子模块action的调用
==方法一：**直接调用-带上模块的属性名路径**
  
```js

test () {

this.$store.dispatch('user/updateToken') // 直接调用方法

}

```


==方法二： **createNamespacedHelpers** 创建基于某个命名空间辅助函数
- 这样就可以像全局模块那样引用了

```vue
<script>
import { mapGetters, createNamespacedHelpers } from 'vuex'

const { mapActions } = createNamespacedHelpers('user')//这里是创建了基于user这个子模块的命名空间的辅助函数，所以可以methods直接引用
export default {

methods：{
...mapActions(['updateToken'])        //直接引用子模块的action状态里的方法
}

}

</script>

<button @click="updateToken">修改token2</button>

```

### 子模块的总结
1. 其实命名空间与不是命名空间他们的调用差别不大
	-  $store根模块直接引入调用，命名空间下，需要添加子模块的名字 例如：`this. $store.dispatch('user/updateToken')`
	- 辅助函数调用，各个属性都有辅助函数。命名空间下需要添加子模块的名字 例如： `...mapMutations(['user/updateToken']), `
	
## vue路由
### vue路由 - 基础使用
#### vue-router介绍

> vue-router本质是一个第三方包

1. vue-router模块包

2. 它和 Vue.js 深度集成

3. 可以定义 - 视图表(映射规则)

4. 提供2个内置全局组件（router-link，< router-view >）

5. 声明式导航自动激活的 CSS class 的链接


#### vue-router使用

> 下载路由模块, 编写对应规则注入到vue实例上, 使用router-view挂载点显示切换的路由

[vue-router文档](https://router.vuejs.org/zh/)

1. 安装
```bash

yarn add vue-router

```

2. 导入路由
 ==注意这里是单独的路由模块==  router/index
```js
// 在router/index.js导入路由
import Vue from 'vue'
import VueRouter from 'vue-router'
//这里到导入之后要用到的组件
import Find from '@/views/Find' // @是src的绝对地址
import My from '@/views/My'
import Part from '@/views/Part'
import NotFound from '@/views/NotFound'
import Recommend from '@/views/Second/Recommend'
import Ranking from '@/views/Second/Ranking'
import SongList from '@/views/Second/SongList'
```

3. 注册路由
```js
// 在router/index.js注册路由
// 在vue中，使用使用vue的插件，都需要调用Vue.use()

Vue.use(VueRouter)

```

4. 创建路由规则数组
```js
// 在router/index.js创建规则路由
//这里规则数组定义的变量要和后面传入规则的变量名一样，都是routes
const routes = [

{

path: "/find",

component: Find   //这里的component里面就是导入的组件，也是要路由的组件

},

{

path: "/my",

component: My

},

{

path: "/part",

component: Part

}

]

//路由配置参数：

​ path : 跳转路径

​ component : 路径相对于的组件

​ name:命名路由

​ children:子路由的配置参数(路由嵌套)

​ props:路由解耦

​ redirect : 重定向路由

​ meta : {
      //meta保存路由对象额外信息的
}
```

  
5. 创建路由对象 - 传入规则
```js

// 在router/index.js创建路由对象
const router = new VueRouter({

routes  //这里就是路由规则的传入，因为名变量名一样，所以简写

})

export default router  //导出路由模块

```

  
6. 关联到vue实例
```js
// 在main.js关联到vue实例

//接收router模块
import router from '@/router' 
new Vue({

router //把创建好的路由对象关联到vue实例中，也是变量名一样，简写

})

```

7. 页面使用< router-view></router-view >承载路由
```vue
<!-- 7. 设置挂载点-当url的hash值路径切换, 显示规则里对应的组件到这 -->
切换url上hash值, 开始匹配规则, 对应组件展示到这里
<router-view></router-view>

```
  

#### 组件分类

> .vue文件分2类, 一个是页面组件, 一个是复用组件，本质无区别

src/views(或pages) 文件夹 和 src/components文件夹

* 页面组件 - 页面展示 - 配合路由用
* 复用组件 - 展示数据/常用于复用


> 总结: views下的页面组件, 配合路由切换, components下的一般引入到views下的vue中复用展示数据

  

### vue路由 - 声明式导航
> 可用全局组件router-link来替代a标签
1. vue-router提供了一个全局组件 router-link
2. router-link实质上最终会渲染成a链接 to属性等价于提供 href属性(to无需#)

#### 声明式导航 - 高亮显示
- router-link提供了声明式导航高亮的功能(自带类名，然后给类名写css属性)
- router-link实质上最终会渲染成a链接，同时会给a链接带上默认类名router-link-active，router-link-exact-active（精确匹配，两个类名都有）
  
```vue

<template>

<div>

<div class="footer_wrap">

<router-link to="/find">发现音乐</router-link>

<router-link to="/my">我的音乐</router-link>

<router-link to="/part">朋友</router-link>

</div>

<div class="top">

<router-view></router-view> //这里是挂载点，声明式导航转到的地址，在这里显示

</div>

</div>

</template>

  

<script>

export default {};

</script>

  

<style scoped>

/* 省略了 其他样式 */

.footer_wrap .router-link-active{

color: white;

background: black;

}

</style>

```

> 总结: 链接导航, 用router-link配合to, 实现点击切换路由


#### 声明式导航 - 类名区别

> 声明式导航可以用全局组件router-link来替代a标签，同时给a标签添加类名（2种不同的类名）

**当路由嵌套导航时：**

*  声明式导航中的url中hash值路径, 与href属性值完全相同, 设置router-link-exact-active (精确匹配)类名，还有模糊匹配类名router-link-active。

* 声明式导航中的url中hash值路径, 是href属性值这个路径的子属性（不一定真是children属性）, 只设置router-link-active (模糊匹配) 这个类名

>其实就是当有嵌套路由时，url刚好和某个路由相同时，同时存在精确类名和模糊类名。
>当url和路由不一样， url只是路由的子路由时， 就只添加模糊类名


==但是当嵌套三层路由时，怎么分配类名呢，两个父路由一个类名？==

![[image-20210512102829250.png]]

  

#### 声明式导航 - 动态路由/跳转传参

> 在跳转路由时传参, 可以给路由表中的路径传参（动态路由），也可以给要跳转的组件传值（跳转传值）

语法：
```html
<!-- 字符串 -->
<router-link to="/home">Home</router-link>
<!-- 渲染结果 -->
<a href="/home">Home</a>

<!-- 使用 v-bind 的 JS 表达式 -->
<router-link :to="'/home'">Home</router-link>

<!-- 同上 -->
<router-link :to="{ path: '/home' }">Home</router-link>

<!-- 命名的路由 -->
<router-link :to="{ path: '/home/123'}">User</router-link>
或
<router-link :to="{ name: 'user', params: { userId: '123' }}">User</router-link>


<!-- 带查询参数，下面的结果为 `/register?plan=private` -->
<router-link :to="{ path: '/register', query: { plan: 'private' }}">
或
<router-link :to="{ path: '/register?plan=private'>
  Register
</router-link>
```

##### 动态路由
- to属性的params属性可以传值给路由表中的规定的动态路由值 ==:name==,达到动态路由的目的
- 其实就是当接口是动态取值时（靠外来的值来动态取数据），需要用动态路由，这样每个取值都有单独的路由了。
- 但是当路由表中:name?  带问号的话，就是可以动态也可以不动态。不加？ 必须动态路由

```html
<!-- 命名的路由 -->
<router-link :to="{ path: '/home/123'}">User</router-link>  //前提是路由表里已经命名  :userId
或
<router-link :to="{ name: 'user', params: { userId: '123' }}">User</router-link>//这里userId要和路由表中一样 :userId
```

##### 跳转传参
- 当跳转传参给路由表时，跳转到的组件内部也能拿到传的参数
- $route是路由信息对象，包括‘path，hash，query，fullPath，matched，name’等路由信息参数；
- $router是路由实例对象，包括了路由的跳转方法，实例对象等   
	- this.$router.options.routes 可以拿到初始化时配置的路由规则数组
 
方法一：
* $route.query.参数名 {{ $route.query.name }}

> ?key=value 用$route.query.key 取值

方法二：
* $route.params.参数名{{ $route.params.username }}
  
>  /值 提前在路由规则/path/:key 用$route.params.key 取值

方法三：
- props传参-就是把路由参数映射到组件的props数据中
>推荐这个方法, 因为跳转传的参，给路由表，路由表映射数据到props，然后请求数据axios里的参数就可以用props里的参数了

1. 跳转到的组件的props属性可以设置
```js

props: {

articleId: {

type: [Number, String],

required: true

}

},

```
2. 路由表中也要设置
```
{

path: '/article/:articleId',

name: 'article',

component: () => import('@/views/article'),

props: true

},
```
				 
### vue路由 - 重定向和模式

#### 路由 - 重定向

> 匹配path后, 强制切换到目标path上

* 网页打开url默认hash值是/路径
* redirect是设置要重定向到哪个路由路径

```js

//在定义路由规则这个步骤,给默认的路径添加重定向
const routes = [

{

path: "/", // 默认hash值路径

redirect: "/find" // 重定向到/find

// 浏览器url中#后的路径被改变成/find-重新匹配数组规则

}

]

```

> 总结: 强制重定向后, 还会重新来数组里匹配一次规则

#### 路由 - 404页面

> 如果路由hash值, 没有和数组里规则匹配，默认给一个404页面

语法: 

在定义路由规则==最后==, path匹配*(任意路径) 

前面不匹配就命中最后这个, 显示对应组件页面

```js

import NotFound from '@/views/NotFound'

const routes = [

// ...省略了其他配置

// 404在最后(规则是从前往后逐个比较path)

{

path: "*",

component: NotFound

}

]

```

#### 路由 - 模式设置

> 在SPA单页应用中，有两种路由模式

**hash模式** ： # 后面是路由路径，特点是前端访问，#后面的变化不会经过服务器

**history模式**：正常的/访问模式，特点是后端访问，任意地址的变化都会访问服务器

[模式文档](https://router.vuejs.org/zh/api/#mode)

> 开发到现在，我们一直都在用hash模式，打包我们尝试用history模式

改成history模式非常简单，只需要将路由的mode类型改成history即可

```js

const createRouter = () => new Router({

mode: 'history', // require service support

scrollBehavior: () => ({ y: 0 }), // 管理滚动行为 如果出现滚动 切换就让 让页面回到顶部

routes: [...constantRoutes] // 改成只有静态路由

})

```


> 假设我们的地址是这样的 **`www.xxxx/com/hr`**/a **`www.xxxx/com/hr`**/b


我们会发现，其实域名是**`www.xxxx/com`**，hr是特定的前缀地址，此时我们可以配置一个base属性，配置为hr

```js

const createRouter = () => new Router({

mode: 'history', // require service support

base: '/hr/', // 配置项目的基础地址

scrollBehavior: () => ({ y: 0 }), // 管理滚动行为 如果出现滚动 切换就让 让页面回到顶部

routes: [...constantRoutes] // 改成只有静态路由

})

```


### vue路由 - 编程式导航
> 编程式导航用JS代码跳转, 声明式导航用a标签

#### 编程式导航 - 基础使用

> 用JS代码来进行跳转

语法:
- 里面可以直接传入路径或者路由名，
```js

this.$router.push()//跳转到指定的url，并在history中添加记录，点击回退返回到上一个页面


this.$router.replace()//跳转到指定的url，但是history中不会添加记录，点击回退到上上个页面


this.$router.go(n)//向前或者后跳转n个页面，n可以是正数也可以是负数

​ 路由跳转 ： this.$router.push()

​ 路由替换 : this.$router.replace()

​ 后退： this.$router.back()      //不传值直接哪里来回哪里去

​ 前进 ：this.$router.forward()

```

1. main.js - 路由数组里, 给路由起名字
```json

{

path: "/find",

name: "Find",

component: Find

},

{

path: "/my",

name: "My",

component: My

},

{

path: "/part",

name: "Part",

component: Part

},

```

2. App.vue - 给导航标签添加点击事件， 配合js的编程式导航跳转
```vue

<template>

<div>

<div class="footer_wrap">

<span @click="btn('/find', 'Find')">发现音乐</span>
 
<span @click="btn('/my', 'My')">我的音乐</span>

<span @click="btn('/part', 'Part')">朋友</span>

</div>

<div class="top">

<router-view></router-view>

</div>

</div>

</template>

  

<script>

// 目标: 编程式导航 - js方式跳转路由

// 语法:

// this.$router.push({path: "路由路径"})      或者直接传入路径或者路由名

// this.$router.push({name: "路由名"})          

// 注意:

// 虽然用name跳转, 但是url的hash值还是切换path路径值

// 场景:

// 方便修改: name路由名(在页面上看不见随便定义)

// path可以在url的hash值看到(尽量符合组内规范)

export default {

methods: {

btn(targetPath, targetName){

// 方式1: path跳转

this.$router.push({

// path: targetPath,

name: targetName

})

}

}

};

</script>

```

  

#### 编程式导航 - 跳转传参

> JS跳转路由, 传参


语法： query / params 任选 一个
- 当用path跳转时，不能用params传参
```js

this.$router.push({

path: "路由路径"

name: "路由名",

query: {

"参数名": 值

}

params: {

"参数名": 值

}

})

  

// 对应路由接收 $route.params.参数名 取值

// 对应路由接收 $route.query.参数名 取值

```


案例：
```vue

<template>

<div>

<div class="footer_wrap">

<span @click="oneBtn">朋友-小传</span>

<span @click="twoBtn">朋友-小智</span>

</div>

<div class="top">

<router-view></router-view>

</div>

</div>

</template>

  

<script>

// 目标: 编程式导航 - 跳转路由传参

// 方式1:

// params => $route.params.参数名

// 方式2:

// query => $route.query.参数名

// 重要: path会自动忽略params

// 推荐: name+query方式传参

// 注意: 如果当前url上"hash值和?参数"与你要跳转到的"hash值和?参数"一致, 爆出冗余导航的问题, 不会跳转路由

export default {

methods: {

oneBtn(){

this.$router.push({

name: 'Part',

params: {

username: '小传'

}

})

},

twoBtn(){

this.$router.push({

name: 'Part',

query: {

name: '小智'

}

})

}

}

};

</script>

// 对应路由接收 $route.params.参数名 取值

// 对应路由接收 $route.query.参数名 取值

```

> 总结: 传参2种方式，query方式，params方式

### vue路由 - 嵌套和守卫

#### vue路由 - 路由嵌套
> 在现有的一级路由下, 再嵌套二级路由

[二级路由示例-网易云音乐-发现音乐下](https://music.163.com/)

1. 创建需要用的所有组件

src/views/Find.vue -- 发现音乐页

src/views/My.vue -- 我的音乐页

src/views/Second/Recommend.vue -- 发现音乐页 / 推荐页面

src/views/Second/Ranking.vue -- 发现音乐页 / 排行榜页面

src/views/Second/SongList.vue -- 发现音乐页 / 歌单页面
```js
import Vue from 'vue'
import App from './App.vue'
import Find from '@/views/Find' // @是src的绝对地址
import My from '@/views/My'
import Recommend from '@/views/Second/Recommend'
import Ranking from '@/views/Second/Ranking'
import SongList from '@/views/Second/SongList'
```
  
2. main.js– 在创建路由规则时，继续配置2级路由

- 一级路由path从/开始定义

- 二级路由往后path直接写名字, 无需/开头

- 嵌套路由在上级路由的children数组里编写路由信息对象

- 某一个子路由path如果为"",表示默认子路由，当跳转父路由时，默认显示此子路由
```js
const routes = [
  {
    path: "/", // 默认hash值路径
    redirect: "/find" // 重定向到/find
    // 浏览器url中#后的路径被改变成/find-重新匹配规则
  },
  {
    path: "/find",
    name: "Find",
    component: Find,
    children: [
      {
        path: "recommend",
        component: Recommend
      },
      {
        path: "ranking",
        component: Ranking
      },
      {
        path: "songlist",
        component: SongList
      }
    ]
  },
  {
    path: "/my",
    name: "My",
    component: My
  },
]
```

3.  在Find.vue中，配置子路由导航

```vue

<template>

<div>

//子路由里面嵌套了二级路由导航
<div class="nav_main">
//这里的路径要写全
<router-link to="/find/recommend">推荐</router-link>

<router-link to="/find/ranking">排行榜</router-link>

<router-link to="/find/songlist">歌单</router-link>

</div>

  
<div style="1px solid red;">
//这里接受二级路由导航的组件
<router-view></router-view>

</div>

</div>

</template>

  

<script>

export default {};

</script>

  

<style scoped>

</style>

```

> 总结: 嵌套路由, 找准在哪个页面里写router-view和对应规则里写children

#### vue路由 - 前置守卫

> 路由跳转之前, 先执行一次前置守卫函数, 判断是否可以正常跳转


方法：
- 在路由对象上使用固定方法beforeEach

```js

// 目标: 路由守卫

// 场景: 当你要对路由权限判断时

// 语法: router.beforeEach((to, from, next)=>{//路由跳转"之前"先执行这里, 决定是否跳转})

// 参数1: 要跳转到的路由 (路由对象信息) 目标

// 参数2: 从哪里跳转的路由 (路由对象信息) 来源

// 参数3: 函数体 - next()才会让路由正常的跳转切换, next(false)在原地停留, next("强制修改到另一个路由路径上")

// 注意: 如果不调用next, 页面留在原地

  

// 例子: 判断用户是否登录, 是否决定去"我的音乐"/my

const isLogin = true; // 登录状态(未登录)

router.beforeEach((to, from, next) => {

if (to.path === "/my" && isLogin === false) {

alert("请登录")

next(false) // 阻止路由跳转

} else {

next() // 正常放行

}

})

```

  
> 总结: next()放行, next(false)留在原地不跳转路由, next(path路径)强制换成对应path路径跳转

  
 
#### vue路由 - 钩子函数

>关于vue-router中的钩子函数主要分为3类

1. 全局钩子函数

beforeEach函数有三个参数,分别是:

​ to:router即将进入的路由对象

​ from:当前导航即将离开的路由

​ next:function,进行管道中的一个钩子，如果执行完了,则导航的状态就是 confirmed （确认的）否则为false,终止导航。

  
2. 单独路由独享组件

​ beforeEnter


3. 组件内钩子

​ beforeRouterEnter，

​ beforeRouterUpdate,

​ beforeRouterLeave

### vant组件库

#### 基础介绍

> vant是一个轻量、可靠的移动端 Vue 组件库, 开箱即用，
> 组件库还是模块化的第三方包。需要下载第三方包，然后导入第三方包，然后注册第三方包，推荐自动全局注册

[vant官网](https://vant-contrib.gitee.io/vant/#/zh-CN/)

特点:
* 提供 60 多个高质量组件，覆盖移动端各类场景

* 性能极佳，组件平均体积不到 1kb

* 完善的中英文文档和示例

* 支持 Vue 2 & Vue 3

* 支持按需引入和主题定制

  ==注意：组件才需要注册，如果引入的是函数的方法，直接引用，不用注册==
#### 默认全部引入

> [[JS#ES6的模块化|默认全部引入]]就是模块化的默认导入第三方包的方式。


步骤：
1.全部引入, 快速开始: [https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart](https://vant-contrib.gitee.io/vant/)

2.下载Vant组件库到当前项目中
```bash
yarn add vant -D
```

3.在main.js中默认导入第三方包，全局注册所有组件
```js

import Vue from 'vue';

import Vant from 'vant';

import 'vant/lib/index.css';

Vue.use(Vant);

```

4. 使用组件

https://vant-contrib.gitee.io/vant/#/zh-CN/button

```vue

<van-button type="primary">主要按钮</van-button>

<van-button type="info">信息按钮</van-button>

<van-button type="default">默认按钮</van-button>

<van-button type="warning">警告按钮</van-button>

<van-button type="danger">危险按钮</van-button>

```

  
#### 手动按需引入

> [[JS#ES6的模块化|按需引入]]就是模块化的按需导入第三方包的方式。

1. 手动单独引入, 快速开始: [https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart](https://vant-contrib.gitee.io/vant/)

```vue
<script>

// 手动 按需引入

import Button from 'vant/lib/button'; // button组件

import 'vant/lib/button/style'; // button样式
</script>
```

2. [[Node.js#全局中间件|注册]](可以在main.js里全局注册，也可以在页面里局部注册)

```vue
<script>

import Button from 'vant/lib/button'; // button组件

import 'vant/lib/button/style'; // button样式

export default{

components: { // 手动局部注册组件

VanButton: Button

// 等价的

[Button.name]: Button

}
};


</script>
```

3. 使用

```vue

<van-button type="primary">主要按钮</van-button>

<van-button type="info">信息按钮</van-button>

<van-button type="default">默认按钮</van-button>

<van-button type="warning">警告按钮</van-button>

<van-button type="danger">危险按钮</van-button>

```

#### 自动按需引入

> 封装了按需加载组件的第三方包，就是不用引入组件的样式了。（有些还是需要详见文档）

[babel-plugin-import](https://github.com/ant-design/babel-plugin-import) 是一款 babel 插件，它会在编译过程中将 import 的写法自动转换为按需引入的方式。

1. 安装插件

```bash

yarn add babel-plugin-import -D

```

2. 在babel配置文件里 (babel.config.js)

```js

module.exports = {

plugins: [    

['import', {

libraryName: 'vant',

libraryDirectory: 'es',

style: true

}, 'vant']

]

};

```

3. 全局注册（推荐） - 会自动按需引入

```js

// 方式: 全局 - 自动按需引入vant组件

// (1): 下载 babel-plugin-import

// (2): babel.config.js - 添加官网说的配置 (一定要重启服务器)

// (3): main.js 按需引入某个组件, Vue.use全局注册 - 某个.vue文件中直接使用vant组件

import { Button } from 'vant';

// 方式一. 通过 app.use 注册
// 注册完成后，在模板中通过 <van-button> 或 <VanButton> 标签来使用按钮组件
app.use(Button);

// 方式二. 通过 app.component 注册
// 注册完成后，在模板中通过 <van-button> 标签来使用按钮组件
app.component(Button.name, Button);


```

>总结：其实就是封装了按需引入的插件

####  弹出框使用

> 使用弹出框组件

```vue

<template>

<div>

<van-button type="primary" @click="btn">主要按钮</van-button>

<van-button type="info">信息按钮</van-button>

<van-button type="default">默认按钮</van-button>

<van-button type="warning">警告按钮</van-button>

<van-button type="danger">危险按钮</van-button>

</div>

</template>

  

<script>


  

// 目标: 使用弹出框

// 1. 找到vant文档

// 2. 引入

// 3. 在恰当时机, 调用此函数的方法 (还可以用组件的用法，注册使用)

import { Dialog } from "vant";

export default {


methods: {

btn() {

Dialog({ message: "提示", showCancelButton: true }); // 调用执行时, 页面就会出弹出框

},

},

};

</script>

```



## vue生命周期
>从Vue实例, 创建到销毁的过程
>就是vue实例创建之后在不同的阶段，内置不同的方法

  ![[Day03.png]]

### 钩子函数

> **Vue** 框架内置函数，随着组件的生命周期阶段，自动执行，像钩子钩在了不同声明周期上

作用: 特定的时间点，执行特定的操作

![[image-20210111193357762.png]]


分类: 4大阶段8个方法（钩子函数）

- 初始化 | 初始化 | beforeCreate | created |

- 挂载     | 挂载 | beforeMount | mounted |

- 更新     | 更新 | beforeUpdate | updated |

- 销毁     | 销毁 | beforeDestroy | destroyed |

其他钩子函数：
- 像路由的钩子函数。
- 指令的钩子函数： inserted(dom, options){} ;  componentUpdated(dom, options){}
[官网文档](https://cn.vuejs.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA)

### 初始化阶段

> 初始化阶段有2个钩子函数，代表阶段的两个时间节点，每个节点有不同的对象、属性和方法    如：vm.$forceUpdate  在这个阶段获取data和methods等

场景: 组件创建完毕后，可以在created 生命周期函数中发起Ajax 请求，从而初始化 data 数据

![[image-20210511153050932 1.png]]

初始化阶段执行流程：

1.new Vue() – Vue实例化(组件也是一个小的Vue实例)

2.Init Events & Lifecycle – 内置初始化事件和生命周期函数

3.beforeCreate – 生命周期钩子函数被执行

4.Init injections&reactivity – Vue内部添加data和methods等

5.created – 生命周期钩子函数被执行, 实例创建

6.编译模板阶段，确定template模板挂载到HTML哪个地方(挂载阶段)

7.Has el option? – 检查入口函数main.js是否有el选项 
	- 没有. 调用$mount()方法
	- 有, 继续检查有没有template选项
```js
new Vue({
	el: "#App",//挂载前准备，检查模板对应的标签（靠它使真实与虚拟dom对应）
  render: h => h(App),
}).$mount('#app')  //或者调用$mount()方法
```

components/Life.vue - 初始化阶段文件
```html


<script>

export default {

data(){

return {

msg: "hello, Vue"

}

},

// 一. 初始化

// new Vue()以后, vue内部给实例对象添加了一些属性和方法, data和methods初始化"之前"

beforeCreate(){

console.log("beforeCreate -- 执行");

console.log(this.msg); // undefined

},

// data和methods初始化以后

// 场景: 网络请求, 注册全局事件

created(){

console.log("created -- 执行");

console.log(this.msg); // hello, Vue


}

}

</script>

```


### 挂载阶段

> 挂载阶段有2个钩子函数，代表阶段的两个时间节点。在这个阶段，渲染template模板，挂载真实dom

场景: 预处理data, 不会触发updated钩子函数，因为真实dom还没挂载

![[image-20210511153649298.png]]
挂载阶段执行流程：

1. template选项检查
​ 有 - 编译template返回render渲染函数      --这里还是虚拟dom还不能获取真实dom
​ 无 – 编译el选项对应标签作为template(要渲染的模板)

2. 虚拟DOM挂载成真实DOM之前

3. beforeMount – 生命周期钩子函数被执行

4. Create … – 把虚拟DOM和渲染的数据一并挂到真实DOM上

5. 真实DOM挂载完毕

6. mounted – 生命周期钩子函数被执行


components/Life.vue -演示

```html

<template>

<div>

<p>学习生命周期 - 看控制台打印</p>

<p id="myP">{{ msg }}</p>

</div>

</template>

  

<script>

export default {

// ...省略其他代码

// 二. 挂载

// 真实DOM挂载之前

// 场景: 预处理data, 不会触发updated钩子函数

beforeMount(){

console.log("beforeMount -- 执行");

console.log(document.getElementById("myP")); // null


this.msg = "重新值"

},

// 真实DOM挂载以后

// 场景: 挂载后真实DOM

mounted(){

console.log("mounted -- 执行");

console.log(document.getElementById("myP")); // p

}

}

</script>

```


### 更新阶段

> 更新阶段有2个钩子函数，代表阶段的两个时间节点。，在这个阶段获得真实dom

场景: 获取更新后的真实DOM

![[image-20210511154016777.png]]
  

更新阶段执行流程：

0. 挂载真实dom完成，当更新data数据后，会改变真实dom

1. 当data里数据改变, 更新DOM之前

2. beforeUpdate – 生命周期钩子函数被执行

3. Virtual DOM…… – 虚拟DOM重新渲染, 打补丁到真实DOM（diff算法）

4. updated – 生命周期钩子函数被执行

5. 当有data数据改变 – 重复这个循环


- components/Life.vue -演示

准备ul+li循环, 按钮添加元素, 触发data改变->导致更新周期开始

```html


<script>

export default {

data(){

return {

msg: "hello, Vue",

arr: [5, 8, 2, 1]

}

},

// ...省略其他代码

  

// 三. 更新

// 前提: data数据改变才执行

// 更新之前

beforeUpdate(){

console.log("beforeUpdate -- 执行");

console.log(document.querySelectorAll("#myUL>li")[4]); // undefined

},

// 更新之后

// 场景: 获取更新后的真实DOM

updated(){

console.log("updated -- 执行");

console.log(document.querySelectorAll("#myUL>li")[4]); // li

}

}

</script>

```

  
### 销毁阶段

>销毁阶段有2个钩子函数，代表阶段的两个时间节点。这个阶段，整个组件被v-if销毁

场景: 移除全局事件, 移除当前组件, 计时器, 定时器, eventBus移除事件$off方法

![[image-20210511154330252.png]]

销毁阶段执行流程：

1.当$destroy()被调用 – 比如组件DOM被移除(例v-if)

2.beforeDestroy – 生命周期钩子函数被执行

3.拆卸数据监视器、子组件和事件侦听器

4.实例销毁后, 最后触发一个钩子函数

5.destroyed – 生命周期钩子函数被执行

  
- components/Life.vue -演示

App.vue - 在主vue上用 v-if 移除组件标签-> 导致Life组件进入销毁阶段

```vue

<Life v-if="show"></Life>

<button @click="show = false">销毁组件</button>

  

<script>

data(){

return {

show: true

}

},

</script>

```

在组件内部执行销毁阶段
```html

<script>

export default {

// ...省略其他代码

// 四. 销毁

// 前提: v-if="false" 销毁Vue实例

// 场景: 移除全局事件, 移除当前组件, 计时器, 定时器, eventBus移除事件$off方法

beforeDestroy(){

// console.log('beforeDestroy -- 执行');

clearInterval(this.timer)

},

destroyed(){

// console.log("destroyed -- 执行");

}

}

</script>

```



## axios
>axios 是一个专门用于发送ajax请求的库,可以代替[[Ajax#XMLHttpRequest |jquery的ajax ]]，都是封装的xhr

==特点==

* 支持客户端发送Ajax请求

* 支持服务端Node.js发送请求

* 支持[[JS#Promise的用法|Promise相关用法]]

* 支持请求和响应的拦截器功能

* 自动转换JSON数据

* axios 底层还是原生js实现, 内部通过Promise封装的,返回promise对象

### axios基本使用

语法：
```js

axios({

method: '请求方式', // get post

url: '请求地址',

data: { // 拼接到请求体的参数, post请求的参数

xxx: xxx,

},

params: { // 拼接到请求行的参数, get请求的参数

xxx: xxx

}

}).then(res => {

console.log(res.data) // 后台返回的结果，默认封装一层data，也就是说，最外层是axios包的data对象，里面才是返回的数据

}).catch(err => {

console.log(err) // 后台报错返回

})

```

[axios文档](http://www.axios-js.com/)

==步骤：

1. 下载安装第三方模块
>yarn add axios

2. 在业务子组件中，引入axios模块
```vue
import axios from "axios";
```
3. 在需要的地方发起axios请求
```js
methods: {

getAllFn() {

axios({

url: "/api/getbooks",

method: "GET", // 默认就是GET方式请求, 可以省略不写

}).then((res) => {

console.log(res);

});

// axios()-原地得到Promise对象
```

### axios基本使用-获取数据

```vue

<template>

<div>

<p>1. 获取所有图书信息</p>

<button @click="getAllFn">点击-查看控制台</button>

</div>

</template>

  

<script>

// 目标1: 获取所有图书信息

// 1. 下载axios

// 2. 引入axios

// 3. 发起axios请求

import axios from "axios";

export default {

methods: {

getAllFn() {

axios({

url: "http://123.57.109.30:3006/api/getbooks",

method: "GET", // 默认就是GET方式请求, 可以省略不写

}).then((res) => {

console.log(res);

});

// axios()-原地得到Promise对象

},

}

};

</script>

```

  

### axios基本使用-get传参
- 在url?拼接 – 查询字符串
```vue

<template>

<div>

<p>2. 查询某本书籍信息</p>

<input type="text" placeholder="请输入要查询 的书名" v-model="bName" />

<button @click="findFn">查询</button>

</div>

</template>

  

<script>

import axios from "axios";

export default {

data() {

return {

bName: ""

};

},

methods: {

// ...省略了查询所有的代码

findFn() {

axios({

url: "/api/getbooks",

method: "GET",

params: { // 都会axios最终拼接到url?后面

bookname: this.bName

}

}).then(res => {

console.log(res);

})

}

},

};

</script>

```


### axios基本使用-post传参
- 在请求体 传参（自动转换成json）给后台
```vue

<template>

<div>

<p>3. 新增图书信息</p>

<div>

<input type="text" placeholder="书名" v-model="bookObj.bookname">

</div>

<div>

<input type="text" placeholder="作者" v-model="bookObj.author">

</div>

<div>

<input type="text" placeholder="出版社" v-model="bookObj.publisher">

</div>

<button @click="sendFn">发布</button>

</div>

</template>

  

<script>

import axios from "axios";

export default {

data() {

return {

bName: "",

bookObj: { // 参数名提前和后台的参数名对上-发送请求就不用再次对接了

bookname: "",

author: "",

publisher: ""

}

};

},

methods: {

// ...省略了其他代码

sendFn(){

axios({

url: "/api/addbook",

method: "POST",

data: {

appkey: "7250d3eb-18e1-41bc-8bb2-11483665535a",

...this.bookObj//扩展运算符，把对象展开bookname: "",author: "",publisher: ""


// 等同于下面

// bookname: this.bookObj.bookname,

// author: this.bookObj.author,

// publisher: this.bookObj.publisher

}

})

}

},

};

</script>

```


### axios基本使用-全局配置

1. 把根url变为全局统一设置
2. 把引入的axios变成全局祖册

- 在入口js里
```js
import axios from 'axios'

axios.defaults.baseURL = "https://www.escook.com"

Vue.prototype.$axios = axios//在vue的原型链上挂载方法
```

```js

axios.defaults.baseURL = "http://123.57.109.30:3006"

  
export default {

// 所有请求的url前置可以去掉, 请求时, axios会自动拼接baseURL的地址在前面

getAllFn() {

axios({

url: "/api/getbooks",

method: "GET", // 默认就是GET方式请求, 可以省略不写

}).then((res) => {

console.log(res);

});

// axios()-原地得到Promise对象

},
}
```

  

### Axios的拦截器介绍
axios的拦截器原理如下
  
![[image-20200811012945409.png]]

**axios拦截器**
- axios作为网络请求的第三方工具, 可以进行请求和响应的拦截
- 模板

```js

// 导出一个axios的实例 而且这个实例要有请求拦截器 响应拦截器

import axios from 'axios'

const service = axios.create() // 创建一个axios的实例

service.interceptors.request.use(config => {}, error => {}) // 请求拦截器

service.interceptors.response.use(respose => {}, error => {}) // 响应拦截器

export default service // 导出axios实例

  

```

1. **通过create创建了一个新的axios实例**

```js

// 创建了一个新的axios实例
//提前引入axios store

const service = axios.create({

baseURL: process.env.VUE_APP_BASE_API, // url = base url + request url

// withCredentials: true, // send cookies when cross-domain requests

timeout: 5000 // 超时时间

})

```



2. **请求拦截器**

请求拦截器主要处理 token的**`统一注入问题`**


```js

// axios的请求拦截器

service.interceptors.request.use(

config => {

// do something before request is sent

  

if (store.getters.token) {

// let each request carry token

// ['X-Token'] is a custom headers key

// please modify it according to the actual situation

config.headers['X-Token'] = getToken()

}

return config

}else{

return Promise.reject(new Error())  //reject 一定要返回错误对象，这里的没有错误对象，因为错误是自己发现的逻辑错误，所以自己new一个对象

}



error => {

// do something with request error

console.log(error) // for debug

return Promise.reject(error)

}

)

```

  

3. **响应拦截器**

响应拦截器主要处理 返回的**`数据异常`** 和**`数据结构`**问题

- 错误error的回调函数的实参有一个respose属性，存放的是服务端回复的错误对象
- 还有一个message属性，是错误的介绍

```js

// 响应拦截器

service.interceptors.response.use(

response => {

const res = response.data //解构一层data，因为返回的数据会自动封装一层data


// if the custom code is not 20000, it is judged as an error.

if (res.code !== 20000) {

Message({

message: res.message || 'Error',

type: 'error',

duration: 5 * 1000

})

if (res.code === 50008 || res.code === 50012 || res.code === 50014) {

// to re-login

MessageBox.confirm('You have been logged out, you can cancel to stay on this page, or log in again', 'Confirm logout', {

confirmButtonText: 'Re-Login',

cancelButtonText: 'Cancel',

type: 'warning'

}).then(() => {

store.dispatch('user/resetToken').then(() => {

location.reload()

})

})

}

return Promise.reject(new Error(res.message || 'Error'))

} else {

return res

}

},

error => {

console.log('err' + error) // for debug

Message({

message: error.message,

type: 'error',

duration: 5 * 1000

})

return Promise.reject(error)

}

)

```



## $nextTick和 $refs

### $refs-获取原生DOM

> 利用 ref 和 $refs 可以用于获取 原生dom 元素

- 在vue的挂载的生命周期中，可以获取单个标签原生dom元素。
- 方法一：元素标签设置id，然后原生js方法document.getElementById("id")
- 方法二：元素标签设置ref，然后this.$refs.myH

```vue

<template>

<div>

<p>1. 获取原生DOM元素</p>

<h1 id="h" ref="myH">我是一个孤独可怜又能吃的h1</h1>

</div>

</template>

  
<script>

// 目标: 获取组件对象

// 1. 创建组件/引入组件/注册组件/使用组件

// 2. 组件起别名ref

// 3. 恰当时机, 获取组件对象

export default {

mounted(){

console.log(document.getElementById("h")); // h1

console.log(this.$refs.myH); // h1

}

}

</script>

  

<style>

  

</style>

```

- 可以在for遍历的标签中，获取所有标签的DOM
```js
    <!-- 2. 通过this.$refs.li 获取所有遍历元素，返回数组中  -->
      <li v-for="i in 4" :key="i" ref="li">{{i}}</li>
```

  

### $refs-获取组件对象

>当$refs获取的对象是组件时，获取的是组件的实例，也就是组件的this，可以调用组件的方法和属性。
>就是：父组件直接调用子组件方法和属性。（不常用）

方法：

1.  创建组件/引入组件/注册组件/使用组件
2. 在父组件里给组件起别名ref，< Demo ref="de"></Demo >
3. let demoObj = this.$refs.de; --获取子组件对象export default{}
  
```vue

<template>

<div>

<p>1. 获取原生DOM元素</p>

<h1 id="h" ref="myH">我是一个孤独可怜又能吃的h1</h1>

<p>2. 获取组件对象 - 可调用组件内一切</p>

<Demo ref="de"></Demo>

</div>

</template>

  

<script>

// 目标: 获取组件对象

// 1. 创建组件/引入组件/注册组件/使用组件

// 2. 组件起别名ref

// 3. 恰当时机, 获取组件对象

import Demo from './Child/Demo'

export default {

mounted(){

console.log(document.getElementById("h")); // h1

console.log(this.$refs.myH); // h1

  

let demoObj = this.$refs.de;

demoObj.fn()//但是这是取的export default{}里methods对象里的方法，可以直接这样跨域取方法吗？

},

components: {

Demo

}

}

</script>

```

  
> 总结: ref定义值, 通过$refs.值 来获取组件对象, 就能继续调用组件内的变量

  

### $nextTick-基础使用

> Vue更新DOM-异步的，当vue数据层更新视图层时，不会第一时间按照单线程任务顺序执行。

```vue
<script>
  

data(){

return {

count: 0

}

},

methods: {

btn(){

this.count++; // vue监测数据更新, 开启一个DOM更新队列(异步任务)

console.log(this.$refs.myP.innerHTML); // 0

  
// 原因: Vue更新DOM异步

// 解决: this.$nextTick()

// 过程: DOM更新完会挨个触发$nextTick里的函数体

this.$nextTick(() => {

console.log(this.$refs.myP.innerHTML); // 1

})

}

}

}

</script>

```
- 解决办法
	1. 可以在update生命周期中获得更新后的原生dom
	2. 在this.$nextTick里的函数体里获得
		- 因为this.$nexTick()函数返回promise对象，也可以用async await方法让异步变同步，以达到获得原生dom的目的。
```js
methods: {

btn(){

this.count++; // vue监测数据更新, 开启一个DOM更新队列(异步任务)

console.log(this.$refs.myP.innerHTML); // 0

  
// 原因: Vue更新DOM异步

// 解决: this.$nextTick()

// 过程: DOM更新完会挨个触发$nextTick里的函数体

this.$nextTick(() => {

console.log(this.$refs.myP.innerHTML); // 1

})

}

}

}

```

###  $nextTick-使用场景
  >当想要获取真实dom方法或者属性，就要想一想，原生dom有没有更新

```vue

<template>

<div>

<input ref="myInp" type="text" placeholder="这是一个输入框" v-if="isShow">

<button v-else @click="btn">点击我进行搜索</button>

</div>

</template>


<script>

// 目标: 点按钮(消失) - 输入框出现并聚焦

// 1. 获取到输入框

// 2. 输入框调用事件方法focus()达到聚焦行为

export default {

data(){

return {

isShow: false

}

},

methods: {

async btn(){

this.isShow = true;

// this.$refs.myInp.focus()

// 原因: data变化更新DOM是异步的

// 输入框还没有挂载到真实DOM上

// 解决:

// this.$nextTick(() => {

// this.$refs.myInp.focus()

// })

// 扩展: await取代回调函数

// $nextTick()原地返回Promise对象

await this.$nextTick()//这里这个的作用就是让await上方的代码，异步变同步，没有什么实际作用。这样下面的refs才能获取。

this.$refs.myInp.focus()

}

}

}

</script>

```


