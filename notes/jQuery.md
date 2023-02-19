## 认识jQuery

1. JavaScript 类库(js文件)

说的通俗一些就是一个js文件

2. 封装了很多简单易用的方法 (浏览器兼容)

并且考虑了浏览器的兼容问题

3. 绝大多数用来简化 DOM 操作

4. 提升开发效率


### 使用准备

1. 下包:你得先把这个别人写好`js`文件下载到本地

2. 导包:下载完毕之后需要导入到使用的页面中

3. 导入`jQuery`之后页面中会额外增加的一个全局变量

```javascript

console.log($)

```


```javascript

$('body').css('backgroundColor','yellowgreen')

```

  

## 选择器

* jQuery中获取需要操纵的元素

```javascript

// 基本用法

$('选择器')

//从jquery对象再转为dom对象

$('选择器')[0]    --重新变为dom对象，可以用dom方法
```

* 直接调用$方法即可，选择器通过字符串的方式传递进去 其本身是一个函数

* 基本上兼容所有`css`的选择器，比如标签选择器，类选择器，后代选择器等。

* 选择器会选择所有的标签，所以省略了遍历

* 选择器可以添加dom对象,然后转换为jQuery对象

>其实就是由获取dom对象，转变为获取Jquery对象

```javascript

// 标签选择器

$('p').css('background', 'red');

```

  

```javascript

// 类选择器

$('.p').css('background', 'blue');

```

  

```javascript

// id 选择器

$('#p').css('background', 'green');

```

  

```javascript

// 后代选择器

$('body p').css('color', 'white');

```

## 入口函数
* 相当于js里面的window.onload = function(){}
* 用于提前渲染标签元素，这样dom树就会先把元素渲染完毕在渲染js。而且jQuery不断申明，二window.onload 只能用一次
```javascript
//1、jQuery入口函数
$(document).ready(function(){
 
});
 
//2、简写
$(function(){
 
});
```

## jQuery对象
* jQuery 中利用选择器获取到的并非原生的 DOM 对象，而是 jQuery 对象

* jQuery提供的绝大多数方法都需要通过jQuery对象才可以访问

* 可以把dom对象直接丢到$方法中，他返回的也是一个jQ对象

* JQuery对象的原型里存放这css方法
* jQuery对象可以转化为dom对象，$('选择器')[0]  --就是dom对象

```javascript

// jQuery

$('p').css('backgroundColor','pink')

```

```javascript

$('选择器')

$(dom对象)

```

```javascript

let li = document.querySelector('li')

let $li = $('li')

console.log('li:', li)

console.log('$li:', $li)

```

  

## 事件类型

* 和dom基本一样

```javascript

$('选择器').事件名(function (event) {

event.stopPropagation --阻止冒泡

event.preventDefault --阻止默认行为

})

```

## 事件绑定

> 以原生事件类型的名称为依据，封装了相对应的事件处理方法

* 注意this指向dom对象，所以要在选择器中转换为jquery对象

```javascript

$('选择器').事件名(function () {

// 逻辑....

})

```

```javascript

$('li').click(function(){

console.log('click')

console.log(this)

$(this).css('backgroundColor','pink')

})

```

## 高级事件绑定
> 更为强大的事件绑定,还能解绑

```javascript

// 1. 注册事件

.on('事件名', function(){})

// 2. 移除指定事件

.off('事件名')

// 3. 移除所有事件

.off()

// 4. 注册一次性事件

.one('事件名', function(){})

```

* jquery并没有给所有的事件提供绑定方法。所以只能用on方法绑定

```javascript

$('.onoff').on('input', function () {

console.log('input事件')

})

```

## window事件绑定
> 如何为window对象绑定事件呢?
* js里的绑定

```javascript

// 滚动

window.onscroll = function () {}

//  点击

window.onclick = function () {}

```

* jquery里的绑定

```javascript

// 滚动

$(window).scroll(function () {})

//  点击

$(window).click(function () {})

```

  
  
  
  

## 获取未绑定方法

* 因为jquery对象并没有封装全部的方法

1. 用$(‘选择器’).trigger('方法')

2. $(‘选择器’).get(索引)

3. $(‘选择器’)[索引]

  

## 触发事件

> 用代码的方式触发注册的事件

* 为了初始化代码

* 直接调用对应的事件方法即可，不需要传入任何参数就是触发，比如点击事件，调用click方法即可

* 但并不是所有事件都有对应的方法，比如input事件，这个时候就可以通过trigger的方式来触发，直接传入希望触发的事件名就好啦

* trigger方法可以触发任意的事件，包括自定义的事件，什么叫做自定义事件呢？顾名思义就是事件名是咱们自己想的，写什么都可以

* 自定义事件必须通过on的方式来注册，把第一个参数换成自定义的事件名就好啦，同时他也只能通过trigger的方式来触发


```javascript

// 1. 直接触发

.事件名()

// 2. trigger触发

.trigger('事件名')

// 3. 触发自定义事件

.trigger('自定义事件') --可以触发jquery没有封装的方法

// 4. 注册自定义事件

.on('自定义事件',function(){})

```

  

## 事件委托

* 就是通过祖先元素选择冒泡范围，后代选择器是真正执行的事件，this也是点击谁指向谁

```javascript

// on 方法内置支持事件委托

$('祖先元素').on('事件名', '后代选择器', function () {

})

```

1. 事件委托需要为某个在 DOM 中已经存在的祖先元素添加事件监听

2. `delegate` 方法是 jQuery 中专门的事件委托的方法

3. `on` 方法中也内置支持事件委托，推荐使用 `on` 方法

## 链式编程

​通过`jQuery`对象调用的大部分方法返回的还是同一个`jQuery`对象,所以就可以给同一个对象绑定不同的事件，估计也可以添加不同的属性

  

```javascript

$('.text').focus(回调函数).blur(回调函数)

```

```javascript

$('.text')

.focus(function () {

console.log('获取焦点')

})

.blur(function () {

console.log('失去焦点')

})

```

  

## 内容操纵

> 如何通过jQuery操纵元素的内容

  

1. ​先通过旋转器获取到希望操纵的元素

2. 然后调用对应的方法

3. 传递参数就是添加内容,不传递参数就是取值

  

```javascript

// 设置

$('选择器').html('内容')

$('选择器').text('内容')

// 取值

$('选择器').html()

$('选择器').text()

```

  

```javascript

// 2. 设置标签

$('.box1').html(' <a href="#">黑马程序员</a>')

$('.box2').text(' <a href="#">黑马程序员</a>')

```

  

* html赋值的时候可以解析标签，text不可以

* html取值时，连标签都获得了，而text只获得了文本

* 赋值操作可以进行链式编程，因为还是同一个对象

  
  

## 样式操纵

> 使用jQuery提供的方法操纵元素样式

  

* 用jquery对象里的css方法操作元素样式有两种方式：一种是赋值、一种是取值

* css方法设置的样式在行内

  
  
  

1. 赋值的方式一

* 直接设置键值对,第一个参数是样式的名字,第二个参数是样的值

  

```javascript

.css('样式名','值')

.css('backgroundColor','pink')

.css('color','red')

.css('width','200px')

.css('height',200)

```

2. 赋值的方式二

  

传递对象,把这些个键值对设置到同一个对象中即可

  

```javascript

.css(对象)

.css({

backgroundColor:'pink',

color:'red',

width:'200px',

height:200

})

```

3. 取值的方法

```javascript

.css('样式名')

.css('backgroundColor')

```

  

## 属性操纵

  

> 使用jQuery提供的方法操纵元素的属性

* href属性、img的src属性、自定义的info属性

* 无论是内置的，还是自定义的，这些都是元素的属性

  

### attr方法

  

1. 设置属性是通过attr方法传入两个参数，分别是属性名和设置的值

2. 传入属性名就好，和css方法是一致的

  

```javascript

// 赋值

.attr('属性名','值')

// 取值

.attr('属性名')

```

```javascript

// 设置

$('a').attr('href', 'http://www.itheima.com/')

$('img').attr('src', 'http://www.itheima.com/images/logo.png')

$('img').attr('info','itheima')

```

```javascript

// 取值

console.log($('a').attr('href'))

console.log($('img').attr('info'))

```

### removeAttr方法

  

```javascript

// 删除属性

.removeAttr('属性名')

```

```javascript

// 删除

$('a').removeAttr('href')

$('img').removeAttr('info')

```

  

## 类名操纵

  

> 使用jQuery提供的方法操纵元素的类名

  

* `addClass`就是添加类名

* `removeClass`就是移除类名

* `hasClass`是判断类名是否存在，返回的是`布尔值`

* `toggleClass`可以对类名进行切换，有就移除，没有就添加。

  

参数都一样，直接传入需要操纵的类名即可

  
  

```javascript

// 1. 添加类名

$('.add').click(function () {

$('.test').addClass('active')

})

```

  

```javascript

// 2. 移除类名

$('.remove').click(function () {

$('.test').removeClass('active')

})

```

  

```javascript

// 3. 判断类名

$('.has').click(function () {

let res = $('.test').hasClass('active')

console.log('res:', res)

})

```

  

```javascript

// 4. 切换类名

$('.toggle').click(function () {

$('.test').toggleClass('active')

})

```

  
  

## value操纵

  

> 使用jQuery提供的方法操纵表单元素的value值

  

* .val()方法

* 添加参数就是赋值，不添加就是取值

  

```javascript

// 取值

$('选择器').val()

// 赋值

$('选择器').val('值')

```

```javascript

$('input').val('黑马程序员')

```

  

## 元素位置

  

> `jQuery`中获取元素位置

  

* `position`和`offset`都可以获取位置

* 两者返回的是一个对象

* 两者获取位置的参照物不同:

* `offset`始终参照的是`html`

* `position`参照的有定位属性的最近祖先元素

* 对于`margin`两者的处理也不相同

* `offset`不会计算margin

* `position`会算上margin的长度

* `offset`方法可以设置位置,但是没有动画效果

```javascript

$('选择器').offset()

$('选择器').position()

```

  

## 滚动距离

  

> `jQuery`中如何获取滚动距离呢?

  

* 获取元素滚动距离

```javascript

$('选择器').scrollTop()

$('选择器').scrollLeft()

```

* 获取网页的滚动距离

```javascript

$('html').scrollTop()

$('html').scrollLeft()

```

* 设置滚动距离

```javascript

$('html').scrollTop(值)

$('html').scrollLeft(值)

```

  
  

## 获取尺寸

```javascript

$(’选择器‘).width() --内容宽度

$(’选择器‘).height() -- 内容高度

$(’选择器‘).innerWidth() -- 内容宽度+内边距

$(’选择器‘).innerHeight() -- 内容高度+内边距

$(’选择器‘).outerHeight() --内容宽度+内边距+边框

$(’选择器‘).outerWidth() --内容高度+内边距+边框

$(’选择器‘).outerHeight(true) --内容高度+内边距+边框+外边距

$(’选择器‘).outerWidth(true) --内容宽度+内边距+边框+外边距

```

## 动画 - 显示&隐藏

* 底层就是display和缩放

```javascript

// 显示方法

$('html').show(持续时间)

// 隐藏方法

$('html').hide(持续时间)

// 切换方法

$('html').toggle(持续时间)

  

```

  

> 如果为元素叠加了多种动画会依次按顺序执行,如果不要这么做可以手动停止他们

  

## 动画 - 淡入&淡出

* 底层就是透明度和display

```javascript

// 淡入

$('html').fadeIn(持续时间)

// 淡出

$('html').fadeOut(持续时间)

// 淡入&淡出

$('html').fadeToggle(持续时间)

```

  

## 动画 - 展开&收起

* 底层就是改变display，宽度，高度、边距

```javascript

// 展开

$('html').slideDown(持续时间)

// 收起

$('html').slideUp(持续时间)

// 展开&收起

$('html').silideToggle(持续时间)

```

## 动画 - 动画对列以及停止方法

* 就是你的行为太快，动画加载的太慢，导致你行为停止，动画还在依次进行

* 动画方法和stop方法返回的是同一个对象，可以链式编程

```javascript

// 停止当前动画-进行下一个动画

$(选择器).stop()

// 停止所有在对列动画 -暂停当前位置

$(选择器).stop(true)

// 停止所有在对列动画 -暂停当前动画结束位置

$(选择器).stop(true，true)

```

  
  

## 动画 - 自定义动画

  

```javascript

// 只有数值类属性支持动画，默认px

$('选择器').animate({

width: 100，

height: 100，

margin: 100，

},时间)

// 支持非样式的属性

$('选择器').animate({

scrollTop: 100，

scrollLeft: 100，

},时间)

```

## 动画 - 动画的回调函数

* 动画执行完毕立即执行

* 回调函数中的this是执行动画的dom元素

```javascript

$(选择器).基础动画方法(持续时间，回调函数)

$(选择器).animate(属性，持续时间，回调函数)

```

## 动画 - 动画延迟

* 可以一直添加动画方法，先添加延迟，再添加方法

```javascript

$(’选择器‘).delay().动画方法().delay().动画方法()

```

## 节点 - 过滤

  

> 使用jQuery的过滤方法对找到的元素再次筛选

  

1. first()方法 last()方法 eq()方法

2. 对获取的jquery对象进行过滤,last最后一个,first第一个 eq(索引)可以选择

> $('选择器').first().html()

3. 这三个方法的返回值是jQ对象

  

## 节点 - 查找


> 使用jQuery提供的查找方法对元素再次检索


* parent()--用来获取父元素 不能添加css选择器，因为父元素只有一个

* children()--获取子元素 （）可以添加css选择器

* siblings--获取兄弟元素不包括自己 （） 可以添加css选择器

* find--获取后代元素 （） 必须添加css选择器

  
  

```javascript

$('.course')

.children()

.css('backgroundColor', 'skyblue')

$('.campus')

.children('.bj')

.css('backgroundColor', 'pink')

```

```javascript

$('.sz')

.siblings()

.css('color', 'red')

$('.sz')

.siblings('.gz')

.css('backgroundColor', 'pink')

```

  
## 节点 - 插入

* 插入节点 ：传入创建的dom节点或者html元素

* 改变节点位置： 传入现有的dom元素或者jquery对象

* 当身有html元素时，传入选择器可以移动元素位置（可以传入dom节点但jquery节点更方便）

* 当没html元素时，传入’<元素>‘ 可以创建元素节点

```javascript

// 父元素结尾

$(父元素选择器).append($(选择器)) --传入选择器，移动元素位置

$(父元素选择器).append(‘<img>’) --传入元素，直接创建元素节点

// 父元素开头

$(父元素选择器).prepend()

// 兄弟元素前面

$(兄弟元素选择器).before()

// 兄弟元素后面

$(兄弟元素选择器).after()

```

* `append`、`prepend`、`after`、`before` 均支持直接将 html 字符串做为节点插入

## 节点 - 删除
* `remove` 方法删除的是当前调用方法的元素节点

```javascript

// 删除自己

$('选择器').remove();

```

## 节点 - 克隆

* `clone` 方法复制得到的元素节点仍是 jQuery 对象

* 待复制的节点中如果有事件监听，需要为 `clone` 方法传入参数 `true`

```html

<script>

// 通过复制获得新的节点 拷贝事件

$('选择器').clone(true);

// 通过复制获得新的节点 不拷贝事件

$('选择器').clone(false);

</script>

```

## jQuery方法
```javascript
//遍历方法，
$.each(要遍历的数组， function (i, item) {

})
```

## 插件机制
* 通过官方文档确认插件写法:https://api.jquery.com/jQuery.fn.extend/

* 直接添加到了jquery的原型上了

* jQuery=$

```javascript

jQuery.fn.extend({

插件名 (参数) {

// 逻辑

}

})

```

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  

​