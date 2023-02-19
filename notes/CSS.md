  

# PC端

## 选择器

### 选择器三大特性

1. 层叠性

属性选择器相同-就近原则

2. 继承性

继承属性：font/text/lineheight/color；

3. 优先级

元素<类，伪类<ID<行内<!important

4. 权重叠加

元素：0,0,0,1;
  
类/伪类/属性：0,0,1,0;

ID：0,1,0,0;

行内：1,0,0,0

```css

/* 标签选择器 */

p {}

/* 类选择器 */

.class {}

/* id选择器 */

#id {}

/* 通配符选择器 */

* {}

/* 后代选择器 */

div span a {}

/* 嫡长子选择器 */

div>span {}

/* 并行选择器 */

div,span {}

/* 链接伪类选择器 */

class：link {}

class：visited {}

class：hover {}

class：active i {}

input: focus {}

/* 属性选择器 */

input["value"] {}

/* 属性值选择器 */

input[type="text"] {}

/* 多样值选择器 */

1.^代表属性的值以x开头

2.$代表属性的值以x结尾

3.代表属性的值中有x

input[type^ $ *="x"] {}

/* 结构伪类选择器 */

父元素 子元素:first-child（父元素排序，第一不一定是指定子元素）

父元素 子元素:first-of-type(指定子元素排序)

父元素 子元素:last-child
  
父元素 子元素:nth-child()（数字/even/odd/n/2n/2n-1/-n+5)

/* 伪元素选择器 */

1.在子元素内部创建伪行内元素,权重1

子元素::after {}

子元素::before {

content:'内容';

}

```

## 字体

```css

/* 字体声明 */

@font-face {

font-family: ;

src: url();

}

/* 字体图标库 */

1.把下载的font字体文件放在文件根目录下

2.然后css添加字体声明

3.然后复制给定的html里的字体到文本框里

4.给文本框添加font-familty：'icomoon'

font-family: "Arial", "Micosoft yahei";

/* 字体大小 */

font-size: 20px;

/* 字体粗细 */

font-weight: 400;

/* 字体样式 */

font-style: italic; --倾斜

/* 字体复合写法 */

font: italic 700 16px 'Microsoft yahei';

```

## 文本

```css

/* 文本对齐 */

1.给父元素添加

2.行内，行内块元素居中对齐 

text-align: center/right/left;

/* 文本装饰 */

1.文本装饰线

2.li的装饰去除

text-decoration: none/underline/overline/line-through;

li {list-style: none}

/* 文本缩进 */

1.当前元素文字缩进距离

text-indent: 10px/2em;

/* 行间距 */

1.可以被继承

2.等于盒高时，可以让文字垂直居中

line-height: 25px;

/* 文字阴影 */

1.水平，垂直，模糊，颜色

text-shadow: 2px 2px 5px red;

/* 文字效果 */

1.文字过长修剪

text-overflow：clip/ellipsis;

1.不换行

white-space：nowrap;

1.换行，允许长单词被打断并换到下一行

word-wrap ：break-word;

1.换行规则

word-break：keep-all;break-all;

1.文本行水平放置、垂直放置

writing-mode：vertical-rl; vertical-rl;

```

## 背景

```css

/* 颜色 */

background-color: transparent/red;

/* 图片 */

1.规定背景的绘制区域

background-clip:

1.为一个元素指定一幅或多幅背景图像

background-image:

1.规定背景图像的放置位置。

background-origin:

1.规定背景图像的大小

background-size：cover contain；

/* 渐变 */

background-image: linear-gradient(to right, red , yellow); --从右

background-image: linear-gradient(to bottom right, red, yellow); -对角线

background-image: linear-gradient(angle, color-stop1, color-stop2); --角度

background-image: linear-gradient(-90deg, red, yellow); --逆时针90°

/* 径向渐变 */

background-image: radial-gradient(circle, red, yellow, green);

/* 平铺 */

background-repeat: no-repeat;/repeat/repeat-x/repeat-y;

/* 方位 */

1.跟方位词或px移动背景图片

background-position: 0 0;

/* 固定 */

background-attachment: scroll/fixed;

/* 半透明 */

background: rgba(0,0,0,0.5);

/* 复合写法 */

div {background: transparent url(#) repeat fixed top;}

```

  

## 页面布局-盒子模型(标准流)

```css

/* 边框 */

border-width: 5px;

border-style:solid dashed dotted;

border-color:pink;

border:5px solid pink;

border-top:5px;

border-collapse:collapse; --合并边框

/* 内边距 */

1.定盒子的宽高后，用padding会撑开盒子大小，不指定就行

2.padding/-top/-right/-bottom-left

3.行内元素只能设定左右外边距

padding: 5px 10px 10px 20px; --上右下左

/* 外边距 */

1.当两个嵌套外边距重叠，取最大的哪一个

2.行内元素只能设定左右外边距

margin: 5px 10px 10px 20px --上右下左

/* 水平居中 */

1.左右设定auto可以水平居中

2.块元素带宽度，在父元素内水平居中

margin: 0 auto；

1.行内或者行内块，给父元素设定，可以水平居中

text-align：center；

/* 阻止外边距合并 */

1，给父元素制定border

2，给父元素内边距

3，给父元素添加overflow：hidden

/* 圆角边框 */

border-radius: 50%;(高度或者宽度的一半，弧度的最大值)

border-radius: 10px 20px 39px 30px;

border-top-left-radius: 10px; --精确位置

/* 边框图像 */

#borderimg {

border: 10px solid transparent;

border-image: url(border.png) 30 round/stretch;}

/* 盒子阴影 */

1.X Y blur spread color

box-shadow: 10px 10px 10px 10px black inset;

/* 文字阴影 */

1.X Y blur color

text-shadow: 10px 10px 10px bule;

/* 盒子模型 */

box-sizing:content-box;（width=width+padding+border）  //标准盒子模型

box-sizing: border-box; （width=width+padding+border） --不会撑大盒子， 怪异盒子模型（常用）

```

## 页面布局-浮动

1. 脱离标准流 (文字图片不会进位）

2. 一行显示，元素顶部对齐

3. 具有行内块特性（块元素浮动后宽变成内容宽度）

4. 父元素块里一浮全浮--因为不浮动的会独占一行（块级元素特点)

5. 浮动一般在父元素中进行，同级别元素最好都浮动

6. 浮动只影响后面的标准流进位（文字图片不会进位），属于被动型的

```css

/* 清除浮动 */

清除浮动就是为了解决子元素浮动父元素高度为0的问题。

（父无高度，子浮动，影响布局）

1，额外标签法

在子元素（浮动）最后面加一个空标签（div）必须块级，然后添加属性

clear：both；

2.在父元素上添加

overflow：hidden auto scroll；

3.父元素添加伪元素

.clearfix:after {

content:"";

display:block;

height:0;

clear:both;

visibility:hidden;}

4.给父元素添加双伪元素

```

  
  

## 页面布局-定位

1. 行内元素有定位（脱标的）之后也有行内块特性，同时不触发外边距塌陷。

2. 定位不会像浮动那样图片文字不会层叠，定位会层叠。

3. 定位左右冲突时，先左后右，先上后下。

4. 定位优先级高于浮动。

```css

/* 定位模式 */

static relative absolute fixed

/* 边偏移 */

right left top bottom

/* 相对定位 */

1.以自己为标准，不脱标。

position:relative;

/* 绝对定位 */

1.以祖先元素为标准，但是祖先元素，最近一级要有定位（相对，绝对，固定)--子绝父相

2.父元素没有定位依然以浏览器（最终父元素）为标准。

3.脱标（比浮动级别高）

/* 固定定位 */

1.以可视窗口为标准，不跟父元素为标准

2.不滚动，脱标。

position:fixed;

/* 粘性定位（sticky） */

1.相对与固定混合，以窗口为准，占有原来位置。

2.top/bottom/left/right必有一个

position:sticky;

/* 定位的叠放 */

1.只有定位的盒子才有z-index。可以是负数。

z-index：1;

/* 隐藏与显示 */

1.不但隐藏元素，而且位置也消失

display：none；

1.可以显示已隐藏的元素

display：block；

1.隐藏/显示，但是位置还存在

visibility：hidden；

visibility: visible;

/* 滚动条 */

overflow: visible/hidden/scroll/auto（根据需要显示滚动条）

```

  

## 用户界面样式

```css

/* 鼠标样式 */

cursor: default;/pointer/move/text/not-allowed

/* input的蓝色边框去除 */

outline：none；

/* textarea的拖拽按钮去除 */

resize：none

/* 用户可调整范围 */

resize: both/vertical/horizontal;

/* outline空间 */

1.属性在轮廓与元素的边缘边框之间添加空间。

2.轮廓线是在元素边框之外绘制的，并且可能与其他内容重叠。

outline-offset

/* 图文居中 */

1.给图片元素添加

2.让图片和文字居中

vertical-align:bottom middle top；

/* 文字基线baseline */

1.行内块元素天然下面有缝隙

baseline：bottom/top/middle

/* 单行省略号 */

1.white-spare：nowrap;不自动换行，

2.overflow：hidden;溢出隐藏，

3.text-overflow: ellipsis;多余部分省略号。

/* 多行省略号 */

overflow:hidden;

text-overflow:ellipsis;

display;-webkit-box;

-web-line-clamp:2;

-webkit-box-orient: :vertical;

/* 滤镜 */

filter：blur（3px）;

```

## 计数器

```css

counter-reset name - 创建或重置计数器

counter-increment name - 递增计数器值 --谁计数给谁添加

content - 插入生成的内容 --输入计数参数

counter(name) 或 counters() 函数 - 将计数器的值添加到元素 --输出name的计数值

```

## 2D转换

### transform属性

1. 注意层叠性

```css

/* 锚点 */

transform-origin:right bottom;

/* 移动 --先移动后旋转 */

1.不影响别的位置，不脱标

2.transform 不脱标而且占着位置随便动。

3.对行内没有效果

4.其中%是针对自身的百分比

transform：translate(x,y);

transform：translateX(n);

transform：translateY(n);

transform：translate(-50%，-50%);

/* 旋转 */

rotate(-20deg); --逆时针20deg。

rotateY(150deg);

rotateX(150deg);

/* 缩放 */

scale(2,3); --宽高放大几倍

/* 倾斜 */

skewX()；

skewY()；

```

## 3D转换

1. z轴表示画面到人眼的距离

2. 透视效果，人眼距离屏幕的距离

3. 有z轴就有透视

```css

/* 3d效果 */

transform：translate3d(x,y,z);

perspective: 1000PX; --透视

transform-style: preserve-3d; --子元素变成3d状态（父元素添加）

/* 缩放 */

缩放scale3d(x,y，z);宽高z轴放大几倍

```

### 动画

1. 可以多组动画

```css

/* 动画声明 */

@keyframes example {

from {background-color: red;}

to {background-color: yellow;}

}

@keyframes example {

20% {background-color: red;}

30% {background-color: yellow;}

50% {background-color: yellow;}

}

/* 动画属性添加 */

animation-name: example;

animation-duration: 4s;

animation-delay： 2s； --延迟

animation-iteration-count：2/infinite;；--循环次数

animation-timing-function：； --速度曲线

animation-fill-mode：forwards; --保持最后一帧

/* 过渡 */

1.谁起始变化写谁里面

2.当宽高都要变时，可以逗号隔开。或者用all属性

transition: width 0.5 ease 1s; --属性，时间，快慢，何时开始

transition-timing-function: linear; --过渡的速度变化

transition-delay: 1s; --过渡的延迟

/* 函数 */

width：calc(100% - 30px) 表示宽度经过计算，动态变化，跟着父元素（100%）变化

```

# 移动端

1. 视口以逻辑分辨率为主，一般自动视口代码，保证视口统一。

2. 一般设计出二倍图，测量时要选2x，才能测量

3. 古老移动布局为流式布局或者百分比布局，即width：100%；高度确定。

## Flex布局

1. 网页端有兼容性问题，移动端没有

2. 没有浮动的问题

3. 设置方式：给父元素（亲爹）添加display:flex;

4. 组成部分：弹性容器，弹性盒子，主轴，侧轴

5. 弹性盒子（子级）沿主轴排列

6. 子级没有宽，就以内容的宽，没有高，就默认拉伸至父级

7. 子级有宽高，但是父级容不下子级，就挤压盒子宽高。

### 主轴对齐方式

1. 添加在父级当中

```css

justify-content: flex-start; --默认起点排列

justify-content: flex-end; --终点排列

justify-content: center; --主轴中心排列

justify-content: spare-around; --空白均分盒子两侧，子级空白是两侧的二倍

justify-content: spare-between; --空白均分相邻盒子之间，两测没有

justify-content: spare-evenly; --子级与父级盒子间距相等

```

### 测轴对齐方式

1. 添加在父级当中，控制父级中的所有盒子。

```css

align-items:center; --子级沿侧轴居中

align-items:stretch; --默认属性，子级高度拉伸至父级。（当子级没有高度时）

align-content: center; --侧轴中心排列

align-content: spare-around; --空白均分盒子两侧，子级空白是两侧的二倍

align-content: spare-between; --空白均分相邻盒子之间，两测没有

align-self：center; --添加在子级中，控制单独的盒子

```

### 主轴方向转换

1. 因为子级默认沿主轴方向排列，所以转换主轴方向可以调整子级排列。

2. 也是给父级添加。

3. 同时侧轴也会变为主轴相反的方向

4. 注意侧轴变水平，盒子居中，要用侧轴对齐的方式。

```css

flex-direction: column; --主轴由水平变垂直

```

### 伸缩比

1. 给子级添加flax：1；表示父级盒子剩余的宽分成一份，子级占用一份。

2. 所谓剩余部分就是标准流剩余的宽

3. 其他子级可可以添加flax：2；表示份数叠加，父级盒子宽剩余分成3分，各占不同的份数。

### 盒子换行

1. 给父级添加flex-wrap：wrap； --子级换行显示，默认不换行。

2. 换行的同时，不会挤压盒子宽高。

## 移动适配-rem

1. 根据rem单位，设置网页尺寸，可以相对变化盒子大小。

2. rem是相对单位，相当于html标签字号计算结果，所以要先设置html字号

3. 1rem = 1html字号

### 移动适配-媒体查询

1. 使用媒体查询设置差异化的css属性

2. 能够检测视口的宽度。差异化编写css

3. 一般html字号是检测到的视口宽度的10分之一

```css

/* 语法 */

根据媒体查询到的宽度，设置html字号，相当于if

@media (width: 375px) {

html {

font-size:37.5px;

}

}

@media (width: 1920px) {

html {

font-size:192px;

}

}

宽高根据设定的基本字号大小变为rem单位，因为字号根据视口变化，盒子宽高也会根于字号变化

div {

width: 1.813rem;

height: 0.773rem;

```

### 移动适配-flexible

1. 使用flexible.js配合rem实现不同宽度的设备中，网页尺寸等比缩放的效果

2. 核心原理就是根据不同视口宽度给html根节点设置不同的font-size

3. 就是上面媒体查询的js实现。

## 移动适配-vw/vh

1. 相对单位，相对视口的尺寸计算结果

2. 1vw = 1/100视口宽度 1vh = 1/100视口高度

3. 相对于rem省略了根据字号变化，不用窗口查询了

4. 相当于vw直接根据视口变化，而rem根据ht ml字号变化。字号根据视口变化

# less

1. less是css预编译器，可以快速生成css

2. 使css有逻辑性和计算性

3. less文件不可引入，只能用easy-less插件生成对于css文件

## less计算

```less

.box {

width: 100 + 20vw;

width: 100 - 20vw;

width: 100 * 20vw;

width: (100 / 20vw);

}

```

## less嵌套

1. less嵌套生成后代选择器

```less

.father {

width:100px;

.son { --css里生成后代选择器

width: 60px;

&:hover { --生成当前选择器

color: pink;

}

}

&:hover {

color: black;

}

}

```

## less变量

```less

@colora：pink； --变量存储数据

.box {

color: @colora;

}

```

## less引入

1. less之间合并引用，@import '路径';插入主less里

2. 一般写在主less开头

3. 生成的css会自动导入所有less里的代码

## 所有css导出路径

1. less生成的css文件路径一般储存在css文件夹中

2. 在设置中找easy-less插件的json配置文件

3. 添加"out"： "../路径/"

## 单独css导出路径

1. 在要导出的less文件中添加// out: ../路径/

2. 一定第一行

## 禁止css导出路径

1. 在要禁止的less文件中添加// out: false

2. 一定第一行

## Less的函数
- less的所有css都是函数，所以函数都可以复用，当其他组件复用这段代码的时候，这就是less的混入。
- less的函数的写法有两种，一种是普通less， 一种是函数的less
	- 普通less当函数
```less
.class {
width: 100px,
height: 10px
}
.box {
.class()    //直接把普通less当函数复用， 但是因为是普通less，所以它会先编译一次，然后调用的时候又编译一次。
}
```

- 函数型less

```less
.class (@width) {  //直接定义了less的函数
width: @width,
height: 10px
}
.box {
.class(100px)    //只会在调用的时候编译，并且函数是可以传入参数的。
}
```


# SCSS

[官方文档](https://www.sass.hk/)


> 首先注意,这里的sass和我们的scss是什么关系


sass和scss其实是`一样的`css预处理语言，SCSS 是 Sass 3 引入新的语法，其后缀名是分别为 .sass和.scss两种。

SASS版本3.0之前的后缀名为.sass，而版本3.0之后的后缀名.scss。

两者是有不同的，继sass之后scss的编写规范基本和css一致，sass时代是有严格的缩进规范并且没有‘{}’和‘；’。

而scss则和css的规范是一致的。


## 搭建小型测试环境


> 为了方便应用scss，我们可以在vscode中安装一个名为**`easy sass`** 的插件，但是我们只在该项目中工作区中应用该插件，因为在项目中，不需要该插件的辅助，有webpack来做这件事

> 首先我们新建一个文件夹test，然后我们在test下新建一个index.html，并新建一个test.scss

页面结构如下

```html

<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

<link rel="stylesheet" href="./test.css">

</head>

<body>

<nav> Scss样式 </nav>

<div id="app">

Hello World

</div>

<div id="content">

<article>

<h1>文章标题</h1>

<p>文章内容 <a href="">百度</a> </p>

</article>

<aside>

侧边栏

</aside>

</div>

</body>

</html>

```


我们使用的**`easy sass`**插件会自动的帮助我们把**`test.scss => test.css`**


## 变量


`sass`使用`$`符号来标识变量

  

```bash

$highlight-color: #f90

```


上面我们声明了一个 名为**`$highlight-color`**的变量, 我们可以把该变量用在任何位置

  

```bash

#app {

background-color: $highlight-color;

}

```

  

以空格分割的多属性值也可以标识变量

  

```bash

$basic-border: 1px solid black;

```

  

```bash

#app {

background-color: $highlight-color;

border: $basic-border

}

```

  

**变量范围**

  

与`CSS`属性不同，变量可以在`css`规则块定义之外存在。当变量定义在`css`规则块内，那么该变量只能在此规则块内使用。如果它们出现在任何形式的`{...}`块中（如`@media`或者`@font-face`块），情况也是如此：

  

```bash

$nav-color: #F90;

nav {

$width: 100px;

width: $width;

color: $nav-color;

background-color: black

}

  

# 编译后

  

nav {

width: 100px;

color: #F90;

background-color: black;

}

  

```

  

在这段代码中，`$nav-color`这个变量定义在了规则块外边，所以在这个样式表中都可以像 `nav`规则块那样引用它。`$width`这个变量定义在了`nav`的`{ }`规则块内，所以它只能在`nav`规则块 内使用。这意味着是你可以在样式表的其他地方定义和使用`$width`变量，不会对这里造成影响。

  

**嵌套语法**

  

和less一样,scss同样支持**`嵌套型`**的语法

  

```scss

#content {

article {

h1 { color: #1dc08a }

p { font-style: italic; }

}

aside { background-color: #f90 }

}

```

  

转化后

  

```scss

#content article h1 {

color: #1dc08a;

}

  

#content article p {

font-style: italic;

}

  

#content aside {

background-color: #f90;

}

  

```

  

**&父选择器**

  

假如你想针对某个特定子元素 进行设置

  

比如

  

```scss

#content {

article {

h1 { color: #1dc08a }

p { font-style: italic; }

a {

color: blue;

&:hover { color: red }

}

}

aside { background-color: #f90 }

}

```


>学到这里,我们会发现scss和less有很多相似之处,最大的区别就在于声明变量的方式,less采用的是**`@变量名`**, 而scss采用的**`$变量名`**


  

# 响应布局

## 媒体查询

1. @media (媒体特性) {css code}

2. 用max-width/min-width属性，注意书写层叠性

3. min-width属性，从小到大书写，保证最下面的大的属性能盖到其他上面小属性

3. max-width属性，从大到小书写，保证最下面的小的属性能盖到其他上面大属性

```less

@media (min-width: 768px) {

}

@media (min-width: 992px) {

}

@media (min-width: 1200px) {

}

```

### 完整写法

1. @media 关键词 媒体类型 and (媒体特性) { css code }

2. 关键词：and only not

3. 媒体类型：screen print apeech all

4. 媒体特性：orientation：portrait/landscape;max-width;min-width

### 引入形式

1. 媒体查询可以在css里引入，也可以在html通过link引入

2. 注意link引入也有层叠性

```html

<link rel="stylesheet" href="url" media="(媒体特性)">

```

## bootstrap

1. Twitter开发的UI框架，大量css样式

2. 下载开发环境

3. 加载css样式，然后引入类名

### 全局样式

1. 封装好的css预设

2. 查找bootstrap文档，给样式添加类名即可。

3. 全局样式是给HTML标签添加的类名

#### 栅格系统

1. 将整个页面的宽度等分成若干份（bs3分成12份）

2. 类名对应效果：col-xs-3( 当宽度<768px 时 占3/12份)；col-sm-*(>=768); col-md-*(>=992); col-lg-*（>=1200）

3. container类名--指定宽度且居中（版心）；container-fluid类名--所有盒子宽度为100%

4. container类自带内间距15px， row类带内间距-15px，所以可以相互抵消（分开写）。

#### 表格

1. table-*类名

2. 给 < table > 添加的样式

#### 按钮

1. 给< a > < button > < input >添加

2. btn，btn-*类名

### 组件样式

1. 封装好的html+css或js预设

2. 比如下拉菜单，导航条等

3. 因为其中可能包括js预设，所以要配合js插件使用

#### 字体图标

1. 和iconfont用法一样

2. 引入css，然后在< i >中添加类名

#### js插件

1. 先引入css样式， 再引入jquery库，再引入js

2. js插件保证组件样式可以交互

### 定制样式

1. 就是所有模板预设的修改

CSS object-fit 属性用于指定应如何调整 < img > 或 < video > 的大小以适合其容器。--它会剪切图像的侧面，保留长宽比，并填充空间，

object-fit:

cover;

fill - 默认值。调整替换后的内容大小，以填充元素的内容框。如有必要，将拉伸或挤压物体以适应该对象。

contain - 缩放替换后的内容以保持其纵横比，同时将其放入元素的内容框。

cover - 调整替换内容的大小，以在填充元素的整个内容框时保持其长宽比。该对象将被裁剪以适应。

none - 不对替换的内容调整大小。

scale-down - 调整内容大小就像没有指定内容或包含内容一样（将导致较小的具体对象尺寸）

var()函数

var(name, value) --变量名称必须以两个破折号（--）开头，且区分大小写！

全局变量
```css
:root {

--blue: #1e90ff;

--white: #ffffff;

}

```

局部变量

直接选择器中声明它。