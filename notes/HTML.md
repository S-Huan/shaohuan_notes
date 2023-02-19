
## 基础HTML标签

```html

<h1>标题1-6</h1>

<p>段落 </p>

换行<br />

<strong>加粗</strong>

<b>加粗</b>

<em>倾斜</em>

<i>倾斜</i>

<del>删除线</del>

<s>删除线</s>

<ins>下划线</ins>

<u>下划线</u>

<div>盒子独占一行</div>

<span>小盒子占一块</span>

<p>添加图片</p>

&nbsp 空格

&lt 小于

&gt 大于

<header></header>

<nav></nav>

<article></article>

<section></section>

<aside></aside>

<footer></footer>

```

## html快捷键

```css

快速生成3个div;

div*3

生成父子级标签;

ul>li

生成兄弟级标签;

div+p

生成class类名

.demo

类名排序;、

.demo$*3

生成id名（默认div）;

#two

生成p的类名;

p.demo

插入文字;

div {输入文字}

插入文字;

div {输入文字$}*3 x

```

## 链接类标签

### 图片

```html

<img src="图片路径 / ../ 绝对路径不常用 图片网络url" alt="图片未显示时文字" title="鼠标放上时显示文字" width="宽度" height="高度" />

<img src="图片名称" border="边框" />

```

### 超链接

a标签可包含图片img

```html

<a href="http://www.qq.com" target="_blank">腾讯</a> 超链接 _self当前页打开 _blank新页面打开（超链接一般包含在li中）

超链接可以外部链接也可以内部链接 内部通过相对路径打开html文件

<a href="#" target="_blank">腾讯</a> 空超连接 #或者javascript： --不会跳转页面

<a href="文件，或者压缩包" target="_blank">腾讯</a> 下载链接

<a href="地址" target="_blank"><img src="路径" alt=""></a>

```

### 锚点链接

```html

<a href="#锚点名字">源对象</a> --点击的条目设置id 加#号

<h3 id="锚点名字">目标位置</h3> --到想要的位置头部添加id="名字"

```

## 表格类标签

### 表格标签

```html

<table> --表格标签

<thead> --头部标签

<tr> --行标签 第一行

<th></th> --表头单元格 加粗表头

</tr>

</thead> --头部标签

<tbody> --身体标签

<tr> --第二行

<td></td> --单元格 嵌套在行标签中

</tr>

</tbody> --身体标签

</table>

```

### 合并单元格

rowspan="单元格个数" 跨行

colspan="单元格个数" 跨列

找到要跨行还是跨列的单元格 ，把上述代码写入第一个单元格中，删除未写入的单元格

  

### 列表标签

1.无序列表

```html

<ul> --中间只能放li标签 ，定义列表

<li>中间放什么都可以</li>

<li>是无序的</li>

</ul>

```

2.有序列表

```html

<ol> --中间只能放li标签

<li>可以放所有</li>

<li>可以放所有</li>

</ol>

```

3.自定义列表

```html

<dl> --定义列表，只能有dt，dd

<dt>列表头</dt>

<dd>子列表</dd>

<dd>子列表</dd>

</dl>

```

## 表单标签
* 实际应用当中，表单负责采集数据，ajax负责发送数据
* 因为表单提交会跳转action页面，同时页面数据会消失

* ajax负责发送数据，因为表单提交会跳转action页面

### 表单域
* action 表单数据要传送的地址，当提交完成后默认跳转这个地址
* target 跳转的方式
* method 发送数据的方式
* enctype 编码发送的数据

```html

<form action="url地址"method="提交方式"name="表单域名字"></form>

<form action="demo.php" method="post" name="name1">

```

### 表单元素

##### input标签

（type name value给后台表单元素值 checked第一次加载时被选中-按钮可用 maxlengh输入正整数的最大值）

```html

用户名：<input type="text" name="usrname" value="用户名"> --单行字段20字符

密 码：<input type="password" name="pswd" maxlength="6"> --密码可以被掩盖

性 别：男 <input type="radio" name="sex"> 女 <input type="radio" name="sex"> --定义单选，多选一按钮 注意要表单元素名字一样

爱 好：吃饭 <input type="checkbox" checked> 打豆豆 <input type="checkbox"> 睡觉 <input type="checkbox"> --定义复选按钮

<input type="submit" value="免费注册"> --提交给后台

<input type="reset" value="重新填写"> --重置信息

<input type="button" value="普通按钮"> --后期结合js使用

<input type="file" multiple="multiple"> --上传文件

// H5新增

<input type="text/email/url/date/time/month/number/search/tel" required="required" placeholder="提示文本"

autocomplete="on/off" autofocus="autofocus">

```

##### label标签

```html

<input type="button" value="普通按钮" id="label"><label for="label">普通按钮</label> --把普通按钮与文字绑定，可以一起点

```

#### 下拉标签

```html

<label for="label1">下拉标签</label> <br />

<select id="label1"> --至少一个option

<option>选项1</option>

<option>选项2</option>

<option selected="selected">选项3</option> --默认选择

</select>

```

#### textarea文本域标签

```html

<textarea cols="50" rows="5"></textarea > --大量文字，尽量一行显示，不然有空格在里面

resize：none；防止下拉扩大文本域

</form>

```
 
## 音视频标签

1.视频元素

```html

<video src="#" autoplay="autoplay" muted="muted" loop="loop" poster="#" controls="controls"></video>

```

2.音频元素

```html

<audio src="#" autoplay="autoplay" muted="muted" loop="loop" controls="controls"></audio>

input类型

```