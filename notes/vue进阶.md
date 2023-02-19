
# vue相关原理进阶

## 1 - 整体目标

- [ ] 了解`Object.defineProperty`实现响应式

- [ ] 了解`指令编译`的基础原理

- [ ] 清楚`observe/watcher/dep`具体指的是什么

- [ ] 了解`发布订阅模式`以及其解决的具体问题

  

## 2 - 数据响应式

  

### 2.1 响应式是什么

  

> 一旦数据发生变化，我们可以立刻知道，并且做一些你想完成的事情，这些事情包括但不限于以下：

  

- 发送一个网络请求

- 打印一段文字

- 操作一个dom

- ....

  

### 2.2 如何实现数据响应式

  

在Javascript里实现数据响应式一般有俩种方案，分别对应着vue2.x 和 vue3.x使用的方式，他们分别是：

  

1. 对象属性拦截 (vue2.x)

  

`Object.defineProperty`

  

2. 对象整体代理 (vue3.x)

  

`Proxy`

  

其中**对象属性拦截**，是我们本次课程关注的重点，不管使用其中的哪种方式，道理都是相通的

  

### 2.3 实现对象属性拦截

  

字面量对象定义

  

```js

let data = {

name:'柴柴老师'

}

```

  

Object.defineProperty对象定义

  

```js

let data = {}

Object.defineProperty(data,'name',{

// 访问name属性就会执行此方法 返回值就是获取到的值

get(){

console.log('name属性被获取了')

return '柴柴老师'

},

// 设置新值就会执行此方法 newVal就是设置的新值

set(newVal){

console.log('name属性被设置新值了')

console.log(newVal)

}

})

```


### 2.4 优化1- get和set联动

> 上一小节，我们的get方法中返回的值始终是`柴柴老师`，是固定的，set中拿到新值之后，我们如何让get中可以得到newVal使我们需要解决的问题


#### 现存问题


```js

let data = {}

Object.defineProperty(data,'name',{

// 访问name属性就会执行此方法 返回值就是获取到的值

get(){

console.log('name属性被获取了')

return '柴柴老师'

},

// 设置新值就会执行此方法 newVal就是设置的新值

set(newVal){

console.log('name属性被设置新值了')

console.log(newVal)

}

})

```

  

#### 错误演示

  

![](asset/oldname.png)

  

#### 解决方案

  

> 我们可以 通过一个中间变量 `_name` 来中转get函数和set函数之间的联动

  

```js

let data = {}

let _name = '柴柴老师'

Object.defineProperty(data,'name',{

// 访问name属性就会执行此方法 返回值就是获取到的值

get(){

console.log('name属性被获取了')

return _name

},

// 设置新值就会执行此方法 newVal就是设置的新值

set(newVal){

console.log('name属性被设置新值了')

console.log(newVal)

_name = newVal

}

})

```

  

#### 结果验证

  

![](asset/_name.png)

  

### 2.5 优化2-更加通用的劫持方案

  

> 大家想想看，如果现在有一份已经声明好了数据的对象，我们如何通过劫持的方法把每一个属性都变成setter和getter的形式

  

一份已经声明好的数据

  

```js

let data = {

name: '柴柴老师',

age: 18,

height:180

}

```

  

我想让里面所有的属性都变成响应式的，并且get和set方法中对于每个属性值的操作是连通的

  

```js

let data = {

name: '柴柴老师',

age: 18,

height:180

}

  

// 遍历每一个属性

Object.keys(data).forEach((key)=>{

// key 属性名

// data[key] 属性值

// data 原对象

defineReactive(data,key,data[key])

})

// 响应式转化方法

function defineReactive(data,key,value){

Object.defineProperty(data,key,{

get(){

return value

},

set(newVal){

value = newVal

}

})

}

```

  

!> 结构说明：这个地方实际上使用了闭包的特性，看下图，在每一次的defineReactive函数执行的时候，都会形成一块独立的函数作用域，传入的`value` 因为闭包的关系会常驻内存，这样一来，每个defineReactive函数中的`value` 会作为各自set和get函数操作的局部变量

  

![](asset/defineReactive.png)

  

### 2.6 响应式总结

  

1. 所谓的响应式其实就是拦截对象属性的访问和设置，插入一些我们自己想要做的事情

2. 在Javascript中能实现响应式拦截的方法有俩种，`Object.defineProperty`方法和`Proxy对象代理`

3. 回归到vue2.x中的data配置项，只要放到了data里的数据，不管层级多深不管你最终会不会用到这个数据都会进行递归响应式处理，所以要求我们如非必要，尽量不要添加太多的冗余数据在data中

4. 需要了解vue3.x中，解决了2中对于数据响应式处理的无端性能消耗，使用的手段是Proxy劫持对象整体 + 惰性处理（用到了才进行响应式转换）

  

## 3 - 数据的变化反应到视图

  

上一章节，我们了解到数据劫持之后，我们可以在数据发生修改之后做任何我们想要做的事情，操作视图当然也是OK的，回归到我们的主角-视图上，我们学习下如何把数据的变化在视图上反应出来，看下面的案例

  

![](asset/01.png)

  

!> 要想把数据反应到视图中，本质上还是离不开dom操作

  

### 3.1 命令式操作视图

  

> 目标：我们通过原始的操作dom的方式让每一次的name的最新值都能显示到p元素内部

  

```html

<div id="app">

<p></p>

</div>

<script>

let data = {

name: '柴柴老师',

age: 18,

height:180

}

// 遍历每一个属性

Object.keys(data).forEach((key)=>{

// key 属性名

// data[key] 属性值

// data 原对象

defineReactive(data,key,data[key])

})

function defineReactive(data,key,value){

Object.defineProperty(data,key,{

get(){

return value

},

set(newVal){

value = newVal

// 数据发生变化,操作dom进行更新

document.querySelector('#app p').innerHTML = data.name

}

})

}

// 首次渲染

document.querySelector('#app p').innerHTML = data.name

</script>

```

  

### 3.2 声明式操作视图

  

> 目标：我们将data中name属性的值作为文本渲染到标记了v-text的p标签内部，在vue中，我们把这种标记式的声明式渲染叫做`指令`

  

```html

<div id="app">

<p v-text="name"></p>

</div>

<script>

let data = {

name: '柴柴老师',

age: 18,

height: 180

}

// 遍历每一个属性

Object.keys(data).forEach((key) => {

// key 属性名

// data[key] 属性值

// data 原对象

defineReactive(data, key, data[key])

})

function defineReactive(data, key, value) {

Object.defineProperty(data, key, {

get() {

return value

},

set(newVal) {

value = newVal

// 数据发生变化,操作dom进行更新

compile()

}

})

}

//

function compile() {

let app = document.getElementById('app')

// 1.拿到app下所有的子元素

const nodes = app.childNodes //  [text, input, text]

//2.遍历所有的子元素

nodes.forEach(node => {


// nodeType为1为元素节点

if (node.nodeType === 1) {

const attrs = node.attributes

// 遍历所有的attrubites找到 v-model

Array.from(attrs).forEach(attr => {

const dirName = attr.nodeName

const dataProp = attr.nodeValue

if (dirName === 'v-text') {

node.innerText = data[dataProp]

}

})

}

})

}

// 首次渲染

compile()

</script>

```

  

### 3.3 总结

  

1. 不管是指令也好，插值表达式也好，这些都是将数据反应到视图的标记而已，通过标记我们可以把数据的变化响应式的反应到对应的dom位置上去

2. 找标记，把数据绑定到dom的过程，我们称之为`binding`

  

## 4 - 视图的变化反应到数据

  

> 目标：将data中的message属性对应的值渲染到input上面，同时input值发生修改之后，可以反向修改message的值，在vue系统中，v-model指令就是干这个事情的，下面我们就实现一下v-model的功能

  

```html

<div id="app">

<input v-model="name" />

</div>

<script>

let data = {

name: '柴柴老师',

age: 18,

height: 180

}

// 遍历每一个属性

Object.keys(data).forEach((key) => {

// key 属性名

// data[key] 属性值

// data 原对象

defineReactive(data, key, data[key])

})

function defineReactive(data, key, value) {

Object.defineProperty(data, key, {

get() {

return value

},

set(newVal) {

// 数据发生变化,操作dom进行更新

if (newVal === value) {

return

}

value = newVal

compile()

}

})

}

// 编译函数

function compile() {

let app = document.getElementById('app')

// 1.拿到app下所有的子元素

const nodes = app.childNodes //  [text, input, text]

//2.遍历所有的子元素

nodes.forEach(node => {

// nodeType为1为元素节点

if (node.nodeType === 1) {

const attrs = node.attributes

// 遍历所有的attrubites找到 v-model

Array.from(attrs).forEach(attr => {

const dirName = attr.nodeName

const dataProp = attr.nodeValue

if (dirName === 'v-model') {

node.value = data[dataProp]

// 视图变化反应到数据 无非是事件监听反向修改

node.addEventListener('input', (e) => {

data[dataProp] = e.target.value

})

}

})

}

})

}

// 首次渲染

compile()

</script>

```

  

## 5 - 现存架构的问题

  

> 无法做到精准更新

  

```html

<div id="app">

<p v-text="name"></p>

<p v-text="age"></p>

<p v-text="name"></p>

</div>

<script>

let data = {

name: '柴柴老师',

age: 18,

height: 180

}

// 遍历每一个属性

Object.keys(data).forEach((key) => {

// key 属性名

// data[key] 属性值

// data 原对象

defineReactive(data, key, data[key])

})

function defineReactive(data, key, value) {

Object.defineProperty(data, key, {

get() {

return value

},

set(newVal) {

// 数据发生变化,操作dom进行更新

if (newVal === value) {

return

}

value = newVal

compile()

}

})

}

// 编译函数

function compile() {

let app = document.getElementById('app')

// 1.拿到app下所有的子元素

const nodes = app.childNodes //  [text, input, text]

//2.遍历所有的子元素

nodes.forEach(node => {

// nodeType为1为元素节点

if (node.nodeType === 1) {

const attrs = node.attributes

Array.from(attrs).forEach(attr => {

const dirName = attr.nodeName

const dataProp = attr.nodeValue

console.log( dirName,dataProp)

if (dirName === 'v-text') {

console.log(`更新了${dirName}指令,需要更新的属性为${dataProp}`)

node.innerText = data[dataProp]

}

})

}

})

}

// 首次渲染

compile()

</script>

```

  

![](asset/problem.png)

  

为了做到精准更新，我们需要借助设计模式来优化我们的架构，下面我们就聊聊发布订阅模式~

  

## 6 - 发布订阅模式优化

  

### 6.1 优化思路思考

  

1.数据更新之后实际上需要执行的代码是什么？

  

```js

node.innerText = data[dataProp]

```

  

为了保存当前的node和dataProp，我们再次设计一个函数执行利用闭包函数将每一次编译函数执行时候的node和dataProp都缓存下来，所以每一次数据变化之后执行的是这样的一个`更新函数`

  

```js

() => {

node.innerText = data[dataProp]

}

```

  

2.一个响应式数据可能会有多个视图部分都需要依赖，也就是响应式数据变化之后，需要执行的更新函数可能不止一个，如下面的代码所示，name属性有俩个div元素都使用了它，所以当name变化之后，俩个div节点都需要得到更新，那属性和更新函数之间应该是一个一对多的关系

  

```html

<div id="app">

<div v-text="name"></div>

<div v-text="name"></div>

<p v-text="age"></p>

<p v-text="age"></p>

</div>

  

<script>

let data = {

name: 'cp',

age: 18

}

</script>

```

  

经过分析我们可以得到下面的存储架构图，每一个响应式属性都绑定了相对应的更新函数，是一个一对多的关系，数据发生变化之后，只会再次执行和自己绑定的更新函数

  

![](asset/03.png)

  

### 6.2 理解发布订阅模式(自定义事件)

  

> 理解发布订阅，关键是理解`一对多`

  

#### 1. 从浏览器事件说起

  

dom绑定事件的方式，我们学过俩种

  

1. dom.onclick = function(){}

2. dom.addEventListener('click',()=>{})

  

这俩种绑定方式的区别是，第二种方案可以实现同一个事件绑定多个回调函数，很明显这是一个一对多的场景，既然浏览器也叫作事件，我们试着分析下浏览器事件绑定实现的思路

  

1. 首先addEventListenr是一个函数方法，接受俩个参数，分别是`事件类型` 和`回调函数`

  

2. 因为是一个事件绑定多个回调函数，那在内存里大概会有这样的一个数据结构

  

```js

{

click: ['cb1','cb2',....],

input: ['cb1','cb2',...]

}

```

  

3. 触发事件执行，浏览器因为有鼠标键盘输入可以触发事件，大概的思路是通过事件名称找到与之关联的回调函数列表，然后遍历执行一遍即可

  

ok，我们分析了浏览器事件的底层实现思路，那我们完全可以自己模仿一个出来，事件的触发，我们也通过设计一个方法来执行

  

#### 2. 实现简单的发布订阅

  

```js

// 增加dep对象 用来收集依赖和触发依赖

const dep = {

map: Object.create(null),

// 收集

collect(dataProp, updateFn) {

if (!this.map[dataProp]) {

this.map[dataProp] = []

}

this.map[dataProp].push(updateFn)

},

// 触发

trigger(dataProp) {

this.map[dataProp] && this.map[dataProp].forEach(updateFn => {

updateFn()

})

}

}

```

  

### 6.3 收集更新函数

  

在编译函数执行的时候，我们把用于更新dom的更新函数收集起来

  

```js

// 编译函数

function compile() {

let app = document.getElementById('app')

// 1.拿到app下所有的子元素

const nodes = app.childNodes //  [text, input, text]

//2.遍历所有的子元素

nodes.forEach(node => {

// nodeType为1为元素节点

if (node.nodeType === 1) {

const attrs = node.attributes

// 遍历所有的attrubites找到 v-model

Array.from(attrs).forEach(attr => {

const dirName = attr.nodeName

const dataProp = attr.nodeValue

console.log(dirName, dataProp)

if (dirName === 'v-text') {

console.log(`更新了${dirName}指令,需要更新的属性为${dataProp}`)

node.innerText = data[dataProp]

// 收集更新函数

dep.collect(dataProp, () => {

node.innerText = data[dataProp]

})

}

})

}

})

}

```

  

### 6.4 触发更新函数

  

> 当属性发生变化的时候，我们通过属性找到对应的更新函数列表，然后依次执行即可

  

```js

function defineReactive(data, key, value) {

Object.defineProperty(data, key, {

get() {

return value

},

set(newValue) {

// 更新视图

if (newValue === value) return

value = newValue

// 再次编译要放到新值已经变化之后只更新当前的key

dep.trigger(key)

}

})

}

```

  

**最终代码**
```html
<!DOCTYPE html>

<html lang="en">

  

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

</head>

  

<body>

<div id="app">

<p v-text="name"></p>

<p v-text="name"></p>

<span v-text="age"></span>

<input type="text" v-model="age">

<input type="text" v-model="name">

</div>

  

<script>

// 引入发布订阅模式

const Dep = {

map: {},

// 收集事件的方法

collect(eventName, fn) {

// 如果当前map中已经初始化好了 click:[]

// 就直接往里面push 如果没有初始化首次添加 就先进行初始化

if (!this.map[eventName]) {

this.map[eventName] = []

}

this.map[eventName].push(fn)

},

// 触发事件的方法

trigger(eventName) {

this.map[eventName].forEach(fn => fn())

}

}

  

let data = {

name: '柴柴老师',

age: 29

}

// 把data中的属性变成响应式的

Object.keys(data).forEach((key) => {

defineReactive(data, key, data[key])

})

function defineReactive(data, key, value) {

// 进行转换操作

Object.defineProperty(data, key, {

get() {

  

return value

},

set(newValue) {

// set函数的执行 不会自动判断俩次修改的值是否相等

// 显然如果相等 不应该执行变化的逻辑

if (newValue === value) {

return

}

value = newValue

// 这里我们把最新的值 反映到视图中 这里是关键的位置

// 核心：操作dom 就是通过操作dom api 把最新的值设置上去

// 在这里进行精准更新 -> 通过data中的属性名找到对应的更新函数依次执行

Dep.trigger(key)

}

})

}

// 1.通过标识查找把数据放到对应的dom上显示出来

function compile() {

let app = document.getElementById('app')

// 拿到所有节点

const childNodes = app.childNodes // 所有类型的节点包括文本节点和标签节点

childNodes.forEach(node => {

if (node.nodeType === 1) {

const attrs = node.attributes

Array.from(attrs).forEach(attr => {

const nodeName = attr.nodeName

const nodeValue = attr.nodeValue

// 实现v-text

if (nodeName === 'v-text') {

node.innerText = data[nodeValue]

// 收集更新函数

Dep.collect(nodeValue, () => {

console.log(`当前您修改了属性${nodeValue}`)

node.innerText = data[nodeValue]

})

}

// 实现v-model

if (nodeName === 'v-model') {

// 调用dom操作给input标签绑定数据

node.value = data[nodeValue]

// 收集更新函数

Dep.collect(nodeValue,()=>{

node.value = data[nodeValue]

})

// 监听input事件 在事件回调中 拿到最新的输入值 赋值给绑定的属性

node.addEventListener('input', (e) => {

let newValue = e.target.value

// 反向赋值

data[nodeValue] = newValue

})

}

})

}

})

}

compile()

console.log(Dep)

</script>

<!--

存在的问题：更新太过粗暴

不管你修改了哪个属性,其它属性也会一起跟着进行更新 哪怕你根本就没有动他

只修改了name的时候 正常逻辑 应该只有name相关的更新操作才进行 而不是粗暴的吧所有的都更新一遍

期望：

哪个属性进行了实质性的修改 哪个属性对应的‘编译’部分才得到执行，这个更新优化

我们称之为‘精准更新’

  

发布订阅优化的思路

1. 针对每一个响应式属性 收集与之相关的更新函数

2. 响应式属性更新之后 通过属性名称找到与之绑定在一起的所有更新函数进行处罚执行

-->

  

</body>

  

</html>
```
  

### 6.5 总结

  

1. 了解了发布订阅模式的基础形态

2. 了解发布订阅可以解决什么样的具体问题（精准更新）

  

## 7 - 整体总结

  

1. 数据响应式的实现无非是对象属性拦截，我们使用`Object.defineProperty`来实现，在vue3中使用`Proxy`对象代理方案进行了优化

2. 面试宝典上提到的几个专业名词

`observe`对象指的是把数据处理成响应式的对象
`watcher`指的其实就是数据变化之后的更新函数 (vue中的watcher有两种，一种是用来更新视图的watcher，一种是通过watch配置项声明的watcher)

`dep`指的就是使用发布订阅实现的收集更新函数和触发更新函数的对象

3. 指令实现的核心无非是通过模板编译找到标识然后把数据绑上去，等到数据变化之后再重新放一次

4. 发布订阅模式的本质是解决一对多的问题，在vue中实现数据变化之后的精准更新

# vue组件开发

  

## 1. 整体目标

  

- [x] 了解组件开发的整体流程

- [x] 掌握组件事件和标签事件的区别

- [x] 掌握在组件上使用v-model的方式

  

## 2. Button组件开发

  

### 2.1 确定组件API

  

**属性**

  

| 属性名 | 说明 | 类型 | 默认值 |

| ------ | --------------------------------------------------- | ------ | ------- |

| type | 设置按钮类型，可选值为 `primary` `danger` 或者不设 | String | default |

| size | 设置按钮大小，可选值为 `small` `large` 或者不设 | String | default |

  

**事件**

  

| 事件名称 | 说明 | 回调参数 |

| -------- | ------------ | --------------- |

| click | 按钮点击事件 | (event) => void |

  

### 2.2 编写测试基础Button

  

> 组件有很多的功能，但是这些功能都是由一个最原始的组件逐渐扩展而来的，所以我们先完成一个最基础的button组件，然后逐渐往上添加功能

  

**编写Button组件**

  

目的：完成基础结构 + 基础样式

  

`components/Button/index.vue`

  

```html

<template>

<button class="h-btn">

<slot></slot>

</button>

</template>

<style scoped lang="less">

// 默认背景色 默认大小

.h-btn {

line-height: 1.499;

position: relative;

display: inline-block;

font-weight: 400;

white-space: nowrap;

text-align: center;

background-image: none;

box-shadow: 0 2px 0 rgba(0, 0, 0, 0.015);

cursor: pointer;

transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);

-webkit-user-select: none;

-moz-user-select: none;

-ms-user-select: none;

user-select: none;

touch-action: manipulation;

padding: 5px 10px;

font-size: 14px;

border-radius: 4px;

color: rgba(0, 0, 0, 0.65);

background-color: #fff;

border: 1px solid #d9d9d9;

}

.h-btn:focus {

outline: 0;

}

</style>

```

  

!> 由于button中的文字是动态的，完全是由用户使用时决定，所以我们需要设计一个插槽，用来渲染传入的自定义文字

  
  
  

**测试基础Button**

  

`app.vue`

  

```html

<template>

<div>

<h-button>Default</h-button>

<h-button>Danger</h-button>

<h-button>Primary</h-button>

</div>

</template>

  

<script>

import HButton from '@/components/Button'

export default {

components:{

HButton

}

}

</script>

```

  

![](asset/default-btn.png)

  

进过测试，我们编写的button组件可以进行正常使用，并且`插槽功能`是生效的

  

### 2.3 完成type配置

  

> 核心思路：通过prop传入的值的不同切换需要渲染的类名，达到显示不一样背景色的目的

  

**1. 准备对应class类**

  

```html

<style scoped lang="less">

// primary类

.h-btn-primary {

color: #fff;

background-color: #1890ff;

border-color: #1890ff;

text-shadow: 0 -1px 0 rgba(0, 0, 0, 0.12);

box-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);

}

// danger类

.h-btn-danger {

color: #fff;

background-color: #ff4d4f;

border-color: #ff4d4f;

text-shadow: 0 -1px 0 rgba(0, 0, 0, 0.12);

box-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);

}

</style>

```

  

**2. 编写props**

  

```html

<script>

export default {

props: {

type: {

type: String

}

}

}

</script>

```

  

**3. 根据不同的prop切换class**

  

> 因为要添加的类名是根据prop的不同计算得来的，所以我们可以使用计算属性来完成匹配计算，然后我们找到匹配规则，类名为 `h-btn-danger`，prop值为`danger`，所以计算公式为：`h-btn-prop`

  

```html

<template>

<button class="h-btn" :class="[typeClass]">

<slot></slot>

</button>

</template>

  

<script>

export default {

props: {

type: {

type: String,

// 默认值

default() {

return 'default'

}

}

},

computed: {

typeClass() {

return `h-btn-${this.type}`

}

}

}

</script>

```

  

**4. 测试type属性**

  

`app.vue`

  

```html

<template>

<div>

测试type属性:

<h-button>Default</h-button>

<h-button type="danger">Danger</h-button>

<h-button type="primary">Primary</h-button>

</div>

</template>

  

<script>

import HButton from '@/components/Button'

export default {

components:{

HButton

}

}

</script>

```

  

![](asset/button-type.png)

  
  
  

### 2.4 完成size配置

  

**1. 准备对应class类**

  

```html

<style scoped lang="less">

// size:small

.h-btn-small {

padding: 4px 8px;

font-size: 12px;

}

// size:large

.h-btn-large {

padding: 6px 12px;

font-size: 16px;

}

</style>

```

  

**2. 编写props**

  

```html

<script>

export default {

props: {

size: {

type: String,

default(){

return 'default'

},

validator: function (value) {

return ['small','large'].includes(value)

}

}

}

}

</script>

```

  

**3. 根据不同的prop切换class**

  

```html

<template>

<button class="h-btn" :class="[typeClass]">

<slot></slot>

</button>

</template>

  

<script>

export default {

props: {

size: {

type: String,

default(){

return 'default'

},

// 校验

validator: function (value) {

return ['small','large'].includes(value)

}

}

},

computed: {

sizeClass() {

return `h-btn-${this.size}`

}

}

}

</script>

```

  

**4. 测试size属性**

  

`app.vue`

  

```html

<template>

<div>

测试size属性:

<h-button>Default</h-button>

<h-button size="small">Small</h-button>

<h-button size="large">Large</h-button>

</div>

</template>

  

<script>

import HButton from '@/components/Button'

export default {

components:{

HButton

}

}

</script>

```

  

![](asset/button-size.png)

  
  
  
  
  

### 2.5 完成事件绑定

  

**1.组件直接绑定click事件**

  

```html

<template>

<div>

<h-button size="large" @click="clickHandler">Large</h-button>

</div>

</template>

  

<script>

import HButton from '@/components/Button'

export default {

components:{

HButton

},

methods:{

clickHandler(){

console.log('按钮点击了')

}

}

}

</script>

```

  

测试发现，点击事件并没有绑定成功，接下来我们说一下，vue系统中的事件系统

  

1. 浏览器原生事件 （在浏览器支持的原生标签上绑定的事件）

  

```html

<button @click="handler"></button>

```

  

2. 组件事件 （在组件身上绑定的事件）

  

```html

<h-button @click="handler"></h-button>

```

  

!> 组件绑定的事件默认是不会被浏览器识别的，我们需要做额外的处理让事件生效，有俩种方案

  

1. 添加`.native`修饰符

  

添加修饰符之后，事件会被绑定到组件的根元素身上

  

2. 把click当成自定义事件通过`$emit`执行（推荐）

  

**2. 使用$emit方法触发事件**

  

> 用户的本意是想在点击button按钮的时候，触发组件身上绑定的click回调函数

  

```html

<template>

<button @click="clickHandler">

<slot></slot>

</button>

</template>

  

<script>

export default {

methods: {

clickHandler(e) {

// 触发自定义事件click,并传递事件对象e

this.$emit('click', e)

}

}

}

</script>

```

  

### 2.6 总结

  

1. 编写组件时应该API先行，先确定组件该如何给用户用，再根据API编写逻辑

2. props的名称应该具备语义化，类型应该符合规范，并且可以添加自定义校验

3. 组件上绑定的类似于原生的事件，默认是不会被识别的，需要额外处理

4. 组件有一些设计需要整体把控，比如props与对应类名的匹配，这是我们故意设计的

  

## 3. Editor编辑器组件开发

  

> Button组件的编写，我们是从零开始的，接下来我们借助一些开源的三方基础插件，完成我们自己编辑器组件的编写

  

组件依赖：[wangEditor](https://www.wangeditor.com/)

  

安装依赖： `npm i wangeditor --save`

  

### 3.1 确定基础API

  

**指令**

  

| 指令名 | 说明 | 类型 | 默认值 |

| ------- | ------------------------ | ------ | ------ |

| v-model | 提供编辑器数据的双向绑定 | String | 无 |

  

### 3.2 编写测试基础Editor

  

**编写Editor组件**

  

`components/Editor/index.vue`

  

```html

<template>

<div class="editorContainer" ref="editor"></div>

</template>

  

<script>

import E from 'wangeditor'

export default {

methods: {

initEditor(){

const editor = new E(this.$refs.editor)

// 或者 const editor = new E( document.getElementById('div1') )

editor.create()

}

},

mounted(){

this.initEditor()

}

}

</script>

```

  

**测试组件**

  

`app.vue`

  

```html

<template>

<div>

<Editor/>

</div>

</template>

  

<script>

import Editor from '@/components/Editor'

export default {

components:{

Editor

}

}

</script>

```

  

![](asset/editor-base.png)

  

### 3.3 完成v-model双向绑定

  

**前置知识**

  

!> 当我们在一个组件身上通过v-model绑定一个响应式数据时，记住，他是一个语法糖，实际上相当于完成了俩件事情

  

1. 组件上绑定了一个名为`value` 的自定义属性

2. 组件身上绑定了一个名为`input`的自定义事件

  

**1. 接受数据传入**

  

```html

<template>

<div>

<Editor v-model="content"/>

</div>

</template>

  

<script>

import Editor from '@/components/Editor'

export default {

components:{

Editor

},

data(){

return {

content:'我是编辑器,我真牛逼！'

}

}

}

</script>

```

  

`Editor/index.vue`

  

```html

<template>

<div class="editorContainer" ref="editor"></div>

</template>

  

<script>

import E from 'wangeditor'

export default {

props: {

// 接收数据传入

value: {

type: String

}

},

methods: {

initEditor() {

const editor = new E(this.$refs.editor)

editor.create()

  

}

},

mounted() {

console.log('传过来的数据为', this.value)

this.initEditor()

}

}

</script>

```

  

**2. 实现数据回显**

  

> 数据我们拿到了，然后就可以把它渲染到编辑器内部了，编辑器有一个方法是专门用来设置内容的，我们找到它，`editor.txt.html('富文本内容') `

  

```js

initEditor() {

const editor = new E(this.$refs.editor)

// 监听编辑器改动事件,把最新内容传出去

editor.config.onchange = (newHtml) => {

console.log('change 之后最新的 html', newHtml)

this.$emit('input', newHtml)

}

editor.create()

editor.txt.html(this.value)

}

```

  

![](asset/edit-emit.png)

  

**4. Bug修复**

  

> 看起来我们实现了数据的传入回显和修改时的内容传出，接下来我们在app.vue中动态的修改一下传入的`content`，看看编辑器有没有实时响应得到显示

  

我们通过调试工具，修改content属性的值，发现编辑器并没有得到显示，然后再查看props数据，发现最新的数据已经传进去了，之所以没有显示到编辑器中，是因为编辑器类似一个独立的个体，它并不知道props已经变成新内容了，所以我们的思路是: `监听props的变化，然后把props的值设置到编辑器里`

  

如何监听 - `watch`

  

如何设置 - `editor.txt.html()`

  

```js

initEditor() {

const editor = new E(this.$refs.editor)

editor.config.onchange = (newHtml) => {

console.log('change 之后最新的 html', newHtml)

this.$emit('input', newHtml)

}

editor.create()

editor.txt.html(this.value)

// 为了能使用editor对象,我们采取一个更加灵活的命令式监听写法

this.$watch('value', () => {

editor.txt.html(this.value)

})

}

```

  

再次测试，发现双向绑定已经完全正常，nice~

  

### 3.4 总结

  

通过这一节的学习，我们应该掌握以下知识点

  

1. 组件上绑定`v-model` 等同于做了什么

2. watch监听的另外一种调用方法命令式的监听方法，功能一样

3. 使用三方现成的开源插件编写自己组件的流程（基础使用 、三方方法调用）


# 服务端渲染

## 1 - 什么是服务器端渲染？

  

> server side render 前端页面的产生是由服务器端生成的，我们就称之为服务端渲染

  

### 1.1 新建server文件夹

  

```

server

```

  

### 1.2 生成一个node项目

  

```bash

npm init -y

```

  

### 1.3 安装express

  

[express](https://www.expressjs.com.cn/) 官方文档

  

```bash

npm install express --save

```

  

### 1.4 服务端渲染小案例

  

`app.js`

  

```js

const express = require('express')

const app = express()

const port = 3000

// 当路径为跟路径,返回完整的html片段

app.get('/', (req, res) => res.send(`

<html>

<body>

<h1>hi,hello</h1>

</body>

</html>

`))

  

app.listen(port, () => console.log(`Example app listening on port ${port}!`))

```

  

### 1.5 运行查看效果

  

```bash

node app.js

```

  

### 1.6 打开浏览器

  

`http://localhost:3000`

  

![](asset/ssr.png)

  

### 1.7 右键查看源代码

  

![](asset/source.png)

  

总结：所谓的服务端渲染值得是页面的内容完全是由服务端侧决定到底要展示出什么内容

  

## 2 - 什么是客户端渲染？

  

> client side render 服务端只提供json格式的数据，渲染成什么样子由客户端通过js控制

  

通过vite快速创建一个基于vue框架的客户端渲染样例

  

### 2.1 新建client文件夹

  

```

client

```

  

### 2.2 生成一个vue项目

  

> 我们使用vite工具快速生成一个vue项目，https://vitejs.dev/

  

```bash

npm init @vitejs/app client-vue-app --template vue

```

  

### 2.3 安装依赖并启动

  

```bash

cd vue-app

npm install (or `yarn`)

npm run dev (or `yarn dev`)

```

  

### 2.4 浏览器查看效果

  

`http://localhost:8080`

  

![](asset/vue-client.png)

  

### 2.5 查看源代码

  

![](asset/vue-source.png)

  

结论：通过查看源代码我们发现，源代码并没有显示我们页面中实际渲染的内容，我们只看到一个main.js文件，和一个id为app的根元素，所以我们知道网页内容是通过js来动态的进行渲染的，js运行在浏览器，浏览器也就是客户端，这种由浏览器端的js做主导渲染网页内容的方式，我们就称之为**客户端渲染**

  
  
  

思考题：如何得知一个网站是哪种方式的渲染？

  

## 3 - 客户端渲染vs服务端渲染

  

> 客户端渲染我们叫做CSR渲染方式，服务端渲染我们叫做SSR渲染

  

### 3.1 运行架构对比

  

![](asset/csrssr.png)

  

**说明**

  

CSR执行流程：浏览器加载html文件 -> 浏览器下载js文件 -> 浏览器运行vue代码 -> 渲染页面

  

SSR执行流程：浏览器加载html文件 -> 服务端装填好内容 -> 返回浏览器渲染

  

### 3.2 开发模式对比

  

CSR：前后端通过接口JSON数据进行通信，各自开发互不影响

  

SSR：前后端分工搭配复杂，前端需要写好html模板交给后端，后端装填模板内容返给浏览器

  

### 3.3 特点优势总结

  

| | 客户端渲染（CSR） | 服务端渲染（SSR） |

| ------------------ | ----------------- | ----------------- |

| 首次渲染时间 | 长 | 很短 |

| seo支持 | 差 | 良好 |

| 前后端分工开发效率 | 快 | 慢 |

  

思考：如果我们的项目既想要使用vue高效率的开发项目，同时还想要首屏渲染时间很短，那该怎么办？

  

## 4 - vue框架中的服务端渲染

  

为了解决第3章节提出的问题，目前我们的vue组件都是在浏览器侧通过js渲染出来的，所以首次加载时间很慢，那么我们把vue组件交给服务端负责渲染，渲染为完整内容之后直接返给客户端，是不是就可以可以解决既想渲染快，还想继续使用vue进行开发的问题了？

  

[vue ssr基础使用](https://ssr.vuejs.org/zh/guide/#%E5%AE%89%E8%A3%85)

  

### 4.1 新建vue-ssr文件夹

  

```

vue-ssr

```

  

### 4.2 把server文件夹中的文件拷贝进来

  

### 4.3 安装必要依赖

  

```bash

npm install vue vue-server-renderer --save

```

  

### 4.4 vue服务端渲染最小demo

  

`app.js`

  

```js

const Vue = require('vue')

const server = require('express')()

  

const renderer = require('vue-server-renderer').createRenderer()

  

server.get('*', (req, res) => {

const app = new Vue({

data: {

url: req.url

},

template: `<div>访问的 URL 是：{{ url }}</div>`,

})

renderer.renderToString(app, (err, html) => {

if (err) throw err

res.send(html)

})

})

  

server.listen(8888,() => console.log(`Example app listening on port 8888!`))

```

  

### 4.5 浏览器访问

  

`http://localhost:8888`

  

![](asset/vue-ssr.png)

  

### 4.6 查看源代码

  

![](asset/vue-ssr-source.png)

  
  
  

结论：我们通过在服务器端渲染vue组件的方式，让网页中又有了完整的内容，这样我们就可以既使用了vue开发又节省了首次渲染时间

  

### 4.7 遗留问题

  

修改app.js，添加一个button元素并使用vue的方式绑定click事件

  

```js

const Vue = require('vue')

const server = require('express')()

  

const renderer = require('vue-server-renderer').createRenderer()

  

server.get('*', (req, res) => {

const app = new Vue({

data: {

url: req.url

},

template:

`<div>

访问的 URL 是：{{ url }}

<button @click="alert('123')">click me!</button>

</div>`,

})

renderer.renderToString(app, (err, html) => {

if (err) throw err

res.send(html)

})

})

  

server.listen(8888,() => console.log(`Example app listening on port 8888!`))

```

  

运行发现，页面成功显示了button按钮，但是可惜的是，没有成功绑定事件，点击无效，事实上除了事件没有绑定之外，服务器端虽然完成了vue的渲染，但是给到客户端的时候已经成了字符串了，一系列我们熟悉的vue应用的特性，我们都无法使用，比如数据响应式更新，那该怎么办呢？

  

为了解决以上问题，我们需要引入一个新的概念，称作 `同构`

  

## 5 - 理解同构理念

  

> 一套vue（react）代码，在服务端执行一次，在客户端再执行一次，就做同构

  

```js

const app = new Vue({

data: {

url: req.url

},

template:

`<div>

访问的 URL 是：{{ url }}

<button @click="alert('123')">click me!</button>

</div>`

})

```

  

上面所示的vue代码，我们在服务端的执行保持不变，只要我们把这段代码在客户端再重新执行一遍，不就可以拥有原本vue应用的所有特性了么，确实如此，不过这个过程的难度太大，我们现在只需要理解，所谓的同构是指：**同一套vue代码在服务端执行一次在客户端再执行一次**

  

1. 服务端执行完成渲染解决了首次加载速度慢的问题

2. 浏览器执行解决了绑定事件及恢复vue本身特性的问题

  

## 6 - Nuxt.js框架使用

  

> nuxt.js是一套使用vue框架开发应用的服务端渲染框架，提供了开箱即用的功能

  

### 1. 使用nuxt.js创建一个ssr项目

  

```bash

npm create nuxt-app <项目名>

```

  

按照提示选择项目之后完成创建，需要注意，这一步要选择ssr

  

![](asset/ssr-chose.png)

  

### 2. 启动项目

  

```bash

cd vue-ssr-app

npm run dev

```

  

`http://localhost:3000`

  

![](asset/my-blog.png)

  

### 3. 查看源代码

  

![](asset/nuxt-source.png)

  

显然，我们看到了网页上有实际渲染的内容，这是服务端负责的渲染

  

### 4. 搭建首页

  

`pages/index.vue`

  

在nuxt.js生成的项目中我们依旧像之前一样写单文件组件.vue代码，ElementUI组件也可以正常使用

  

```html

<template>

<div class="container">

<Logo />

<div class="articleList">

<el-collapse>

<el-collapse-item title="一致性 Consistency" name="1">

<div>与现实生活一致：与现实生活的流程、逻辑保持一致，遵循用户习惯的语言和概念；</div>

</el-collapse-item>

</el-collapse>

</div>

</div>

</template>

  

<script>

export default {}

</script>

  

<style>

.container{

padding:0 200px;

}

.articleList{

margin-top:30px;

}

</style>

```

  

### 5. 异步数据获取

  

https://axios.nuxtjs.org/

  

#### 1. 认识asyncData方法

  

`asyncData`方法会在组件（**限于页面组件**）每次加载之前被调用。它可以在服务端或路由更新之前被调用，你可以利用 `asyncData`方法来获取数据，Nuxt.js 会将 `asyncData` 返回的数据融合组件 `data` 方法返回的数据一并返回给当前组件

  

官网推荐使用的请求方式 https://axios.nuxtjs.org/

  

```js

async asyncData({ $axios }) {

const ip = await $axios.$get('http://icanhazip.com')

return { ip }

},

data(){

return {

name:'cp'

}

}

  

----合并完之后的data数据----

{

name:'cp',

ip

}

```

  

#### 2. 获取文章列表（移动端项目）

  

```js

async asyncData ({ $axios }) {

const url = 'http://ttapi.research.itcast.cn/app/v1_1/articles?channel_id=0&timestamp=1606309443970&with_top=1'

const res = await $axios.$get(url)

// eslint-disable-next-line no-console

console.log('文章数据列表：', res)

return {

list: res.data.results

}

}

```

  

#### 3. 渲染接口数据

  

```html

<el-collapse>

<el-collapse-item v-for="item in list" :key="item.id" :title="item.title.slice(0,40)">

<div>评论数：{{ item.comm_count }} 点赞数：{{ item.like_count }}</div>

</el-collapse-item>

</el-collapse>

```

  

#### 4. 预览效果并查看源代码

  

结论：我们完成了既使用vue开发模式，又实现了服务端渲染模式，nice~

  

## 7- 总结

  

### 7.1 服务端渲染和客户端渲染各自指什么？有什么特点？

  

```

SSR 服务端渲染 网页内容由服务端生成 首屏时间短 有利于seo

CSR 客户端渲染 vue、react框架渲染方式 spa都是客户端渲染 首屏渲染时间长不利于seo

```

  

### 7.2 同构的本质是什么？

  

```

一份vue代码在服务端渲染一遍 然后在客户端再渲染一遍

服务端渲染解决了首屏显示快 客户端渲染是需要把事件、响应式特性等vue经典的特性都绑回去

  

我们既可以使用vue的开发模式 又可以享受俩种渲染方式的优势

```

  

### 7.3 Nuxt.js中如何实现异步数据获取（asyncData方法）？

  

```

asyncData函数时Nuxtjs框架内置的一个函数

执行结果和和data进行融合 一起返给当前组件

```