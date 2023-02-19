## Wepack概念
>它会以一个或多个文件作为打包的入口，将我们整个项目所有文件编译组合成一个或多个文件输出出去。
输出的文件就是编译好的文件，就可以在浏览器段运行了。

### Webpack功能
- 开发模式：仅能编译 JS 中的 `ES Module` 语法
- 生产模式：能编译 JS 中的 `ES Module` 语法，还能压缩 JS 代码
- 默认只支持js打包，其他要安装插件

- 开发模式
```

npx webpack ./src/main.js --mode=development

```

- 生产模式
```

npx webpack ./src/main.js --mode=production

```

`npx webpack`: 是用来运行本地安装 `Webpack` 包的。
`./src/main.js`: 指定 `Webpack` 从 `main.js` 文件开始打包，不但会打包 `main.js`，还会将其依赖也一起打包进来。
`--mode=xxx`：指定模式（环境）。

### Webpack结构
1. entry（入口）
指示 Webpack 从哪个文件开始打包

2. output（输出）
指示 Webpack 打包完的文件输出到哪里去，如何命名等

3. loader（加载器）
webpack 本身只能处理 js、json 等资源，其他资源需要借助 loader，Webpack 才能解析

4. plugins（插件）
扩展 Webpack 的功能

5. mode（模式）

主要由两种模式：
- 开发模式：development
- 生产模式：production

> 开发模式打包的文件， 不会进行压缩， 只会进行输出打包。
> 生产模式打包的文件， js和html会进行压缩，然后输出打包， 但是css需要插件进行压缩。
> 开发服务器打包的文件， 在内存中，不会输出出来。

### 更改入口、出口文件
1. 新建webpack.config.js (webpack默认配置文件名)
- 在项目目录下，创建配置文件
2. 填入配置,设置入口路径，出口路径
- 注意: webpack基于node, 所以导出, 遵守CommonJS规范
```javascript
const path = require('path');

module.exports = {
  entry: './src/main.js', // webpack入口
  output: { // 出口
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  }, 
};
```

## Wepack基础使用
1. 新建src下的资源
- 在项目文件下，创立src文件夹，里面包含入口文件index.js

2. work.js – 业务文件模块
	- 一定要在入口文件引入，才会参与打包

3. index.js – 入口文件引入不同业务模块
- 引入add模块并使用函数输出结果

4. 执行 `yarn build` 自定义命令, 进行打包 (确保终端路径在src的父级文件夹)
- 所有要被打包的资源都要跟入口产生直接/间接的引用关系

5. 打包后默认生成dist和main.js
>当代码更新后，要进行重新发包    1. 确保在src/index.js引入和使用     2. 重新执行yarn build打包命令

6. 把出口的js文件引入到html执行查看效果

### Wepack环境配置

1. 初始化包环境
yarn init   --在项目文件夹下，初始化package.json
2. 安装依赖包（第三方包）
yarn add webpack webpack-cli -D
3. 在package.json 下配置
"scripts": {
"build": "webpack"
},

### 自动生成html插件
- 自动创建并打包html文件，自动导入打包后的js文件。
1. 安装插件(开发模块)
>yarn add html-webpack-plugin -D
2. webpack.config.js添加配置
```javascript
const path = require('path');
const htmlwebpackplugin  = require('html-webpack-plugin ');
module.exports = {
  plugins: [
  new HtmlWebpackPlugin({
  template: '相对路径'   //模板路径，插件根据这个html创建新的经过打包的html,告诉webpack使用插件时, 以我们自己的html文件作为模板去生成dist/html文件
  })
  ]
};
```
3. 终端执行 yarn build 打包文件
### 打包css加载器
>让css引入到js里，然后webpack打包到出口js里，应用插件再让css重新插入到dom里

- css-loader 让webpack能处理css类型文件
- style-loader 把css插入到DOM中

>前提是，先让css引入到js里，也就是要和入口文件发生关系。import "相对路径"
1. 安装依赖css-loader和style-loader
>yarn add css-loader style-loader -D
2. webpack.config.js添加配置
```javascript
module.exports = {
 
  module: { // 加载器配置
    rules: [ // 规则
      { // 一个具体的规则对象
        test: /\.css$/i, // 匹配.css结尾的文件
        use: ["style-loader", "css-loader"], // 让webpack使用者2个loader处理css文件
        // 从右到左的, 所以不能颠倒顺序
        // css-loader: webpack解析css文件-把css代码一起打包进js中
        // style-loader: css代码插入到DOM上 (style标签)
      },
    ],
  },
};
```
3.  终端执行 yarn build 打包js入口文件
### 打包less加载器
- less-loader作用: 识别less文件
- less 作用: 将less编译为css

1. 把index.less引入到入口处
>import "相对路径"
3. 下载加载器来处理less文件
>yarn less less-loader -D
4. webpack.config.js针对less配置
```javascript
module.exports = {
 
 module: { // 加载器配置
    rules: [ // 规则
      { // 一个具体的规则对象
        test: /\.css$/i, // 匹配.css结尾的文件
        use: ["style-loader", "css-loader"], // 让webpack使用者2个loader处理css文件
        // 从右到左的, 所以不能颠倒顺序
        // css-loader: webpack解析css文件-把css代码一起打包进js中
        // style-loader: css代码插入到DOM上 (style标签)
      },
      {
        test: /\.less/,
        use: ['style-loader', 'css-loader', 'less-loader']
      }
    ],
  },
};
```
5. 终端执行 yarn build 打包js入口文件

### 打包图片加载器
>webpack5, 使用asset module技术实现字体文件和图片文件处理, 无需配置额外loader
1. 把图片引入到入口处
- 图片也必须与入口有联系，可以css引入图片，css在导入到入口js里，也可以图片直接导入到入口js里
```javascript
// 7. 手动引入一个图片文件
// webpack眼里万物皆模块
import imgObj from './assets/1.gif'
let theImg = document.createElement("img")
theImg.src = imgObj
document.body.appendChild(theImg)
```
2.  webpack.config.js针对图片配置（asset规则，根据图片大小配置图片）

```javascript
module.exports = {
 module: { // 加载器配置
    rules: [ // 规则
      { // 一个具体的规则对象
        test: /\.css$/i, // 匹配.css结尾的文件
        use: ["style-loader", "css-loader"], // 让webpack使用者2个loader处理css文件
        // 从右到左的, 所以不能颠倒顺序
        // css-loader: webpack解析css文件-把css代码一起打包进js中
        // style-loader: css代码插入到DOM上 (style标签)
      },
      {
        test: /\.less/,
        use: ['style-loader', 'css-loader', 'less-loader']
      },
      { // 图片文件的配置(仅适用于webpack5版本)
        test: /\.(gif|png|jpg|jpeg)/,
        type: 'asset' // 匹配上面的文件后, webpack会把他们当做静态资源处理打包
        // 如果你设置的是asset模式
        // 以8KB大小区分图片文件
        // 小于8KB的, 把图片文件转base64, 打包进js中
        // 大于8KB的, 直接把图片文件输出到dist下
      }
    ],
  },
};
```
- 图片8kb转base64
好处：浏览器不用发请求了，速度快
坏处： 图片转base64，体积会增大30%

### 字体图标加载器
1. src/assets 下 放入fonts字体相关文件夹
- 主要是存放字体文件
2. src/main.js 引入 assets/fonts/iconfont.css
- 让字体图标与入口js发生间接关系（通过css引入）
3. src/main.js 创建一个i标签, 使用字体图标标签添加到body上
- 应用字体图标css
```javascript
// 8. 引入字体图标样式文件
import "./assets/fonts/iconfont.css"
let theI = document.createElement("i")
theI.className = "iconfont icon-qq"
document.body.appendChild(theI)
```
4. 添加针对字体文件的加载器规则, 使用asset/resource(直接输出文件并配置路径)
- 在配置文件中配置字体图标应用的配置
```javascript
module.exports = {
 module: { // 加载器配置
    rules: [ // 规则
      { // 一个具体的规则对象
        test: /\.css$/i, // 匹配.css结尾的文件
        use: ["style-loader", "css-loader"], // 让webpack使用者2个loader处理css文件
        // 从右到左的, 所以不能颠倒顺序
        // css-loader: webpack解析css文件-把css代码一起打包进js中
        // style-loader: css代码插入到DOM上 (style标签)
      },
      {
        test: /\.less/,
        use: ['style-loader', 'css-loader', 'less-loader']
      },
      { // 图片文件的配置(仅适用于webpack5版本)
        test: /\.(gif|png|jpg|jpeg)/,
        type: 'asset' // 匹配上面的文件后, webpack会把他们当做静态资源处理打包
        // 如果你设置的是asset模式
        // 以8KB大小区分图片文件
        // 小于8KB的, 把图片文件转base64, 打包进js中
        // 大于8KB的, 直接把图片文件输出到dist下
      }
      {//字体图标文件配置  
        test: /\.(eot|svg|ttf|woff|woff2)$/,
        type: 'asset/resource', // 所有的字体图标文件, 都输出到dist下
        generator: { // 生成文件名字 - 定义规则  -防止重名
          filename: 'fonts/[name].[hash:6][ext]' // [ext]会替换成.eot/.woff
        }
    ],
  },
};
```
### js降级加载器

babel: 一个javascript编译器, 把高版本js语法降级处理输出兼容的低版本语法
babel-loader: 可以让webpack转译打包的js代码

1. 在入口js里运用高版本js
```javascript
// 9. 书写高版本的js语法
const fn = () => {console.log("我是一个箭头函数")}
console.log(fn);
```
2. 安装依赖babel  babel-loader
> yarn add babel-loader @babel/core @babel/preset-env -D

3.  webpack.config.js添加配置
```javascript
 {
        test: /\.m?js$/,
        exclude: /(node_modules|bower_components)/, // 不去匹配这些文件夹下的文件
        use: {
          loader: 'babel-loader', // 使用这个loader处理js文件
          options: { // 加载器选项
            presets: ['@babel/preset-env'] // 预设: @babel/preset-env 降级规则-按照这里的规则降级我们的js语法
          }
        }
      }
```
4. yarn build 打包

### webpack开发服务器
> 缓存一些已经打包过的内容, 只重新打包修改的文件, 最终运行在内存中给浏览器使用;
   webpack开发服务器, 把代码运行在内存中, 自动更新, 实时返回给浏览器显示。

>本质就是实时监看开发模式下的文件预运行，最后还是要打包成生产模式的文件
1. 下载模块包
> yarn add webpack-dev-server -D
2. 配置webpack开发服务器启动命令在package.json
```json
"scripts": {
"build": "webpack",
"serve": "webpack serve"
},
```
3. 启动工程里面的webpack开发服务器
>yarn serve
>返回一个地址和端口浏览器，可以实时更新打包代码
- 注意开发服务器自动打包的出口在服务器内存中，看不见的


#### 更改开发服务器端口号

1. 在webpack.config.js中添加如下配置即可
```javascript
devServer: {
    port: 3000,
  },
```
2. 重新启动webpack开发服务器观察效果


##  Webpack使用案例
### 生产环境
```js
//config/webpack.dev.js

const path = require("path"); // nodejs核心模块，专门用来处理路径问题
const ESLintPlugin = require("eslint-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  // 入口
  entry: "./src/main.js", // 相对路径  这里不知道为啥这个相对路径相对哪个文件夹
  // 输出
  output: {
    // 所有文件的输出路径
    // 开发模式没有输出
    path: undefined,    //因为用的是开发服务器， 所以没有输出
    // 入口文件打包输出文件名
    filename: "static/js/main.js", //这是出口文件, 本来应该在path之下的
  },
  // 加载器
  module: {
    rules: [
      // loader的配置
      {
        // 每个文件只能被其中一个loader配置处理
        oneOf: [ //也可以不要这个对象
          {
            test: /\.css$/, // 只检测.css文件
            use: [
              // 执行顺序：从右到左（从下到上）
              "style-loader", // 将js中css通过创建style标签添加html文件中生效
              "css-loader", // 将css资源编译成commonjs的模块到js中
            ],
          },
          {
            test: /\.less$/,
            // loader: 'xxx', // 只能使用1个loader， 如果只有一个loader的话可以用它
            use: [
              // 使用多个loader
              "style-loader",
              "css-loader",
              "less-loader", // 将less编译成css文件
            ],
          },
          {
            test: /\.s[ac]ss$/,
            use: [
              "style-loader",
              "css-loader",
              "sass-loader", // 将sass编译成css文件
            ],
          },
          {
            test: /\.styl$/,
            use: [
              "style-loader",
              "css-loader",
              "stylus-loader", // 将stylus编译成css文件
            ],
          },
          {
            test: /\.(png|jpe?g|gif|webp|svg)$/,
            type: "asset",
            parser: {
              dataUrlCondition: {
                // 小于10kb的图片转base64
                // 优点：减少请求数量  缺点：体积会更大
                maxSize: 10 * 1024, // 10kb
              },
            },
            generator: {
              // 输出图片名称
              // [hash:10] hash值取前10位
              filename: "static/images/[hash:10][ext][query]",
            },
          },
          {
            test: /\.(ttf|woff2?|map3|map4|avi)$/,
            type: "asset/resource",
            generator: {
              // 输出名称
              filename: "static/media/[hash:10][ext][query]",
            },
          },
          {
            test: /\.js$/,
            exclude: /node_modules/, // 排除node_modules下的文件，其他文件都处理
            loader: "babel-loader",    //编译js代码， 进行降级优化
          },
        ],
      },
    ],
  },
  // 插件
  plugins: [
    // plugin的配置
    new ESLintPlugin({   //代码检查
      // 检测哪些文件
      context: path.resolve(__dirname, "../src"),
    }),
    new HtmlWebpackPlugin({    //html自动打包并引入js入口文件
      // 模板：以public/index.html文件创建新的 html文件
      // 新的html文件特点：1. 结构和原来一致 2. 自动引入打包输出的资源
      template: path.resolve(__dirname, "../public/index.html"),
    }),
  ],
  // 开发服务器: 不会输出资源，在内存中编译打包的
  devServer: {
    host: "localhost", // 启动服务器域名
    port: "3000", // 启动服务器端口号
    open: true, // 是否自动打开浏览器
  },
  // 模式
  mode: "development",  //一定要写， 不然不知道输出的是哪一种模式
  devtool: "cheap-module-source-map",
};

```

### 开发环境 
```js
const path = require("path"); // nodejs核心模块，专门用来处理路径问题
const ESLintPlugin = require("eslint-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

// 用来获取处理样式的loader
function getStyleLoader(pre) {
  return [
    MiniCssExtractPlugin.loader, // 提取css成单独文件
    "css-loader", // 将css资源编译成commonjs的模块到js中
    {
      loader: "postcss-loader",
      options: {
        postcssOptions: {
          plugins: [
            "postcss-preset-env", // 能解决大多数样式兼容性问题 ， css的降级插件， 需要在page.js配置  //  "browserslist": [  "last 2 version",  "> 1%", "not dead"

  ]

}
          ],
        },
      },
    },
    pre,
  ].filter(Boolean);  //Boolean函数，返回数组中所有元素的判定， 因为是filter所有返回true的元素
}

module.exports = {
  // 入口
  entry: "./src/main.js", // 相对路径
  // 输出
  output: {
    // 所有文件的输出路径
    // __dirname nodejs的变量，代表当前文件的文件夹目录
    path: path.resolve(__dirname, "../dist"), // 绝对路径
    // 入口文件打包输出文件名
    filename: "static/js/main.js",
    clean: true,   //这里可以自动清除上一次打包的dist文件
  },
  // 加载器
  module: {
    rules: [
      // loader的配置
      {
        oneOf: [
          {
            test: /\.css$/, // 只检测.css文件
            use: getStyleLoader(), // 执行顺序：从右到左（从下到上）
          },
          {
            test: /\.less$/,
            // loader: 'xxx', // 只能使用1个loader
            use: getStyleLoader("less-loader"),
          },
          {
            test: /\.s[ac]ss$/,
            use: getStyleLoader("sass-loader"),
          },
          {
            test: /\.styl$/,
            use: getStyleLoader("stylus-loader"),
          },
          {
            test: /\.(png|jpe?g|gif|webp|svg)$/,
            type: "asset",
            parser: {
              dataUrlCondition: {
                // 小于10kb的图片转base64
                // 优点：减少请求数量  缺点：体积会更大
                maxSize: 10 * 1024, // 10kb
              },
            },
            generator: {
              // 输出图片名称
              // [hash:10] hash值取前10位
              filename: "static/images/[hash:10][ext][query]",
            },
          },
          {
            test: /\.(ttf|woff2?|map3|map4|avi)$/,
            type: "asset/resource",
            generator: {
              // 输出名称
              filename: "static/media/[hash:10][ext][query]",
            },
          },
          {
            test: /\.js$/,
            exclude: /node_modules/, // 排除node_modules下的文件，其他文件都处理
            loader: "babel-loader",
          },
        ],
      },
    ],
  },
  // 插件
  plugins: [
    // plugin的配置
    new ESLintPlugin({
      // 检测哪些文件
      context: path.resolve(__dirname, "../src"),
    }),
    new HtmlWebpackPlugin({
      // 模板：以public/index.html文件创建新的html文件
      // 新的html文件特点：1. 结构和原来一致 2. 自动引入打包输出的资源
      template: path.resolve(__dirname, "../public/index.html"),
    }),
    new MiniCssExtractPlugin({   //进行css的抽取， 因为css默认用style的loader渲染到html上， 性能不好， 所以抽取出来， 用link便签直接添加到html上， 更快。
      filename: "static/css/main.css",
    }),
    new CssMinimizerPlugin(),  //css的压缩插件， 本来js只对js和html在生产模式压缩，添加插件， 页可以压缩css
  ],
  // 模式
  mode: "production",
  devtool: "source-map",
};


```
## Webpack高级优化
### SourceMap
 
 - 原因
>当我们在开发模式开发时， 打包的代码是没有压缩的， 同时也是经过他们处理过的， 所有 css 和 js 合并成了一个文件，并且多了其他代码。此时如果代码运行出错那么提示代码错误位置我们是看不懂的。一旦将来开发代码文件很多，那么很难去发现错误出现在哪里。

- 概念
>SourceMap（源代码映射）是一个用来生成源代码与构建后代码一一映射的文件的方案。
它会生成一个 xxx.map 文件，里面包含源代码和构建后代码每一行、每一列的映射关系。当构建后代码出错了，会通过 xxx.map 文件，从构建后代码出错位置找到映射后源代码出错位置，从而让浏览器提示源代码文件出错位置，帮助我们更快的找到错误根源。

  

- 通过查看[Webpack DevTool 文档](https://webpack.docschina.org/configuration/devtool/)可知，SourceMap 的值有很多种情况.

但实际开发时我们只需要关注两种情况即可：

- 开发模式：`cheap-module-source-map`
- 优点：打包编译速度快，只包含行映射
- 缺点：没有列映射

```js

module.exports = {

// 其他省略

mode: "development",

devtool: "cheap-module-source-map",

};

```

- 生产模式：`source-map`
- 优点：包含行/列映射
- 缺点：打包编译速度更慢

```js

module.exports = {

// 其他省略

mode: "production",
devtool: "source-map",

};

```

### HotModuleReplacement

- 原因
>开发时我们修改了其中一个模块代码，Webpack 默认会将所有模块全部重新打包编译，速度很慢。
所以我们需要做到修改某个模块代码，就只有这个模块代码需要重新打包编译，其他模块不变，这样打包速度就能很快。

- 概念
>HotModuleReplacement（HMR/热模块替换）：在程序运行中，替换、添加或删除模块，而无需重新加载整个页面。

  
1. 基本配置
```js

module.exports = {

// 其他省略

devServer: {

host: "localhost", // 启动服务器域名

port: "3000", // 启动服务器端口号

open: true, // 是否自动打开浏览器

hot: true, // 开启HMR功能（只能用于开发环境，生产环境不需要了）

},

};

```

此时 css 样式经过 style-loader 处理，已经具备 HMR 功能了。
但是 js 还不行。


2. JS 配置
```js{17-28}

// main.js

import count from "./js/count";

import sum from "./js/sum";

// 引入资源，Webpack才会对其打包

import "./css/iconfont.css";

import "./css/index.css";

import "./less/index.less";

import "./sass/index.sass";

import "./sass/index.scss";

import "./styl/index.styl";

  

const result1 = count(2, 1);

console.log(result1);

const result2 = sum(1, 2, 3, 4);

console.log(result2);

  

// 判断是否支持HMR功能

if (module.hot) {

module.hot.accept("./js/count.js", function (count) {

const result1 = count(2, 1);

console.log(result1);

});

  

module.hot.accept("./js/sum.js", function (sum) {

const result2 = sum(1, 2, 3, 4);

console.log(result2);

});

}

```

上面这样写会很麻烦，所以实际开发我们会使用其他 loader 来解决
  
比如：[vue-loader](https://github.com/vuejs/vue-loader), [react-hot-loader](https://github.com/gaearon/react-hot-loader)。， 也就是说其实框架自带的loader已经自带这个热更新了。

### OneOf

- 原因
> 打包时每个文件都会经过所有 loader 处理，虽然因为 `test` 正则原因实际没有处理上，但是都要过一遍。比较慢。

- 概念
>顾名思义就是只能匹配上一个 loader, 剩下的就不匹配了。
  

```js

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

  

module.exports = {

entry: "./src/main.js",

output: {

path: undefined, // 开发模式没有输出，不需要指定输出目录

filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中

// clean: true, // 开发模式没有输出，不需要清空输出结果

},

module: {

rules: [

{

oneOf: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: ["style-loader", "css-loader"],

},

{

test: /\.less$/,

use: ["style-loader", "css-loader", "less-loader"],

},

{

test: /\.s[ac]ss$/,

use: ["style-loader", "css-loader", "sass-loader"],

},

{

test: /\.styl$/,

use: ["style-loader", "css-loader", "stylus-loader"],

},

{

test: /\.(png|jpe?g|gif|webp)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

generator: {

// 将图片文件输出到 static/imgs 目录中

// 将图片文件命名 [hash:8][ext][query]

// [hash:8]: hash值取8位

// [ext]: 使用之前的文件扩展名

// [query]: 添加之前的query参数

filename: "static/imgs/[hash:8][ext][query]",

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

generator: {

filename: "static/media/[hash:8][ext][query]",

},

},

{

test: /\.js$/,

exclude: /node_modules/, // 排除node_modules代码不编译

loader: "babel-loader",

},

],

},

],

},

plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

],

// 开发服务器

devServer: {

host: "localhost", // 启动服务器域名

port: "3000", // 启动服务器端口号

open: true, // 是否自动打开浏览器

hot: true, // 开启HMR功能

},

mode: "development",

devtool: "cheap-module-source-map",

};

```

  

生产模式也是如此配置。

### Include/Exclude

- 原因
> 开发时我们需要使用第三方的库或插件，所有文件都下载到 node_modules 中了。而这些文件是不需要编译可以直接使用的。所以我们在对 js 文件处理时，要排除 node_modules 下面的文件。

- 概念
 - include
包含，只处理 xxx 文件

- exclude
排除，除了 xxx 文件以外其他文件都处理

- 但是只能用一个

```js{60-61,72}

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

  

module.exports = {

entry: "./src/main.js",

output: {

path: undefined, // 开发模式没有输出，不需要指定输出目录

filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中

// clean: true, // 开发模式没有输出，不需要清空输出结果

},

module: {

rules: [

{

oneOf: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: ["style-loader", "css-loader"],

},

{

test: /\.less$/,

use: ["style-loader", "css-loader", "less-loader"],

},

{

test: /\.s[ac]ss$/,

use: ["style-loader", "css-loader", "sass-loader"],

},

{

test: /\.styl$/,

use: ["style-loader", "css-loader", "stylus-loader"],

},

{

test: /\.(png|jpe?g|gif|webp)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

generator: {

// 将图片文件输出到 static/imgs 目录中

// 将图片文件命名 [hash:8][ext][query]

// [hash:8]: hash值取8位

// [ext]: 使用之前的文件扩展名

// [query]: 添加之前的query参数

filename: "static/imgs/[hash:8][ext][query]",

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

generator: {

filename: "static/media/[hash:8][ext][query]",

},

},

{

test: /\.js$/,

// exclude: /node_modules/, // 排除node_modules代码不编译

include: path.resolve(__dirname, "../src"), // 也可以用包含

loader: "babel-loader",

},

],

},

],

},

plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

exclude: "node_modules", // 默认值

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

],

// 开发服务器

devServer: {

host: "localhost", // 启动服务器域名

port: "3000", // 启动服务器端口号

open: true, // 是否自动打开浏览器

hot: true, // 开启HMR功能

},

mode: "development",

devtool: "cheap-module-source-map",

};


```

### Cache

- 原因
> 每次打包时 js 文件都要经过 Eslint 检查 和 Babel 编译，速度比较慢。
我们可以缓存之前的 Eslint 检查 和 Babel 编译结果，这样第二次打包时速度就会更快了。

- 概念
> 对 Eslint 检查 和 Babel 编译结果进行缓存。

```js{63-66,77-82}

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

  

module.exports = {

entry: "./src/main.js",

output: {

path: undefined, // 开发模式没有输出，不需要指定输出目录

filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中

// clean: true, // 开发模式没有输出，不需要清空输出结果

},

module: {

rules: [

{

oneOf: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: ["style-loader", "css-loader"],

},

{

test: /\.less$/,

use: ["style-loader", "css-loader", "less-loader"],

},

{

test: /\.s[ac]ss$/,

use: ["style-loader", "css-loader", "sass-loader"],

},

{

test: /\.styl$/,

use: ["style-loader", "css-loader", "stylus-loader"],

},

{

test: /\.(png|jpe?g|gif|webp)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

generator: {

// 将图片文件输出到 static/imgs 目录中

// 将图片文件命名 [hash:8][ext][query]

// [hash:8]: hash值取8位

// [ext]: 使用之前的文件扩展名

// [query]: 添加之前的query参数

filename: "static/imgs/[hash:8][ext][query]",

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

generator: {

filename: "static/media/[hash:8][ext][query]",

},

},

{

test: /\.js$/,

// exclude: /node_modules/, // 排除node_modules代码不编译

include: path.resolve(__dirname, "../src"), // 也可以用包含

loader: "babel-loader",

options: {

cacheDirectory: true, // 开启babel编译缓存

cacheCompression: false, // 缓存文件不要压缩

},

},

],

},

],

},

plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

exclude: "node_modules", // 默认值

cache: true, // 开启缓存

// 缓存目录

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

],

// 开发服务器

devServer: {

host: "localhost", // 启动服务器域名

port: "3000", // 启动服务器端口号

open: true, // 是否自动打开浏览器

hot: true, // 开启HMR功能

},

mode: "development",

devtool: "cheap-module-source-map",

};

```

### Thead

- 原因
>  当项目越来越庞大时，打包速度越来越慢，甚至于需要一个下午才能打包出来代码。这个速度是比较慢的。我们想要继续提升打包速度，其实就是要提升 js 的打包速度，因为其他文件都比较少。而对 js 文件处理主要就是 eslint 、babel、Terser 三个工具，所以我们要提升它们的运行速度。我们可以开启多进程同时处理 js 文件，这样速度就比之前的单进程打包更快了。

- 概念
>多进程打包：开启电脑的多个进程同时干一件事，速度更快。

- **需要注意：请仅在特别耗时的操作中使用，因为每个进程启动就有大约为 600ms 左右开销。**

  

我们启动进程的数量就是我们 CPU 的核数。
1. 如何获取 CPU 的核数，因为每个电脑都不一样。
```js

// nodejs核心模块，直接使用

const os = require("os");

// cpu核数

const threads = os.cpus().length;

```

2. 下载包
```

npm i thread-loader -D

```

3. 使用
```js{1,7,9-10,88-101,118,131,133-143}

const os = require("os");

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

const TerserPlugin = require("terser-webpack-plugin");

  

// cpu核数

const threads = os.cpus().length;

  

// 获取处理样式的Loaders

const getStyleLoaders = (preProcessor) => {

return [

MiniCssExtractPlugin.loader,

"css-loader",

{

loader: "postcss-loader",

options: {

postcssOptions: {

plugins: [

"postcss-preset-env", // 能解决大多数样式兼容性问题

],

},

},

},

preProcessor,

].filter(Boolean);

};

  

module.exports = {

entry: "./src/main.js",

output: {

path: path.resolve(__dirname, "../dist"), // 生产模式需要输出

filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中

clean: true,

},

module: {

rules: [

{

oneOf: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: getStyleLoaders(),

},

{

test: /\.less$/,

use: getStyleLoaders("less-loader"),

},

{

test: /\.s[ac]ss$/,

use: getStyleLoaders("sass-loader"),

},

{

test: /\.styl$/,

use: getStyleLoaders("stylus-loader"),

},

{

test: /\.(png|jpe?g|gif|webp)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

generator: {

// 将图片文件输出到 static/imgs 目录中

// 将图片文件命名 [hash:8][ext][query]

// [hash:8]: hash值取8位

// [ext]: 使用之前的文件扩展名

// [query]: 添加之前的query参数

filename: "static/imgs/[hash:8][ext][query]",

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

generator: {

filename: "static/media/[hash:8][ext][query]",

},

},

{

test: /\.js$/,

// exclude: /node_modules/, // 排除node_modules代码不编译

include: path.resolve(__dirname, "../src"), // 也可以用包含

use: [

{

loader: "thread-loader", // 开启多进程

options: {

workers: threads, // 数量

},

},

{

loader: "babel-loader",

options: {

cacheDirectory: true, // 开启babel编译缓存

},

},

],

},

],

},

],

},

plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

exclude: "node_modules", // 默认值

cache: true, // 开启缓存

// 缓存目录

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

threads, // 开启多进程

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

// 提取css成单独文件

new MiniCssExtractPlugin({

// 定义输出文件名和目录

filename: "static/css/main.css",

}),

// css压缩

// new CssMinimizerPlugin(),

],

optimization: {

minimize: true,

minimizer: [

// css压缩也可以写到optimization.minimizer里面，效果一样的

new CssMinimizerPlugin(),

// 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了

new TerserPlugin({

parallel: threads // 开启多进程, 这个本来内置了， 但是要开启多进程就要再配置一下多进程的配置。

})

],

},

// devServer: {

// host: "localhost", // 启动服务器域名

// port: "3000", // 启动服务器端口号

// open: true, // 是否自动打开浏览器

// },

mode: "production",

devtool: "source-map",

};

```

我们目前打包的内容都很少，所以因为启动进程开销原因，使用多进程打包实际上会显著的让我们打包时间变得很长。

### Tree Shaking

- 原因
>开发时我们定义了一些工具函数库，或者引用第三方工具函数库或组件库。
如果没有特殊处理的话我们打包时会引入整个库，但是实际上可能我们可能只用上极小部分的功能。
这样将整个库都打包进来，体积就太大了。

- 概念
>  `Tree Shaking` 是一个术语，通常用于描述移除 JavaScript 中的没有使用上的代码。
**注意：它依赖 `ES Module`。**

- Webpack 已经默认开启了这个功能，无需其他配置。


### Babel 插件

- 原因 
>Babel 为编译的每个文件都插入了辅助代码，使代码体积过大！
Babel 对一些公共方法使用了非常小的辅助代码，比如 `_extend`。默认情况下会被添加到每一个需要它的文件中。
你可以将这些辅助代码作为一个独立模块，来避免重复引入。

- 概念
> `@babel/plugin-transform-runtime`: 禁用了 Babel 自动对每个文件的 runtime 注入，而是引入 `@babel/plugin-transform-runtime` 并且使所有辅助代码从这里引用。

  

1. 下载包
```

npm i @babel/plugin-transform-runtime -D

```

2. 配置
```js{100}

const os = require("os");

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

const TerserPlugin = require("terser-webpack-plugin");

  

// cpu核数

const threads = os.cpus().length;

  

// 获取处理样式的Loaders

const getStyleLoaders = (preProcessor) => {

return [

MiniCssExtractPlugin.loader,

"css-loader",

{

loader: "postcss-loader",

options: {

postcssOptions: {

plugins: [

"postcss-preset-env", // 能解决大多数样式兼容性问题

],

},

},

},

preProcessor,

].filter(Boolean);

};

  

module.exports = {

entry: "./src/main.js",

output: {

path: path.resolve(__dirname, "../dist"), // 生产模式需要输出

filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中

clean: true,

},

module: {

rules: [

{

oneOf: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: getStyleLoaders(),

},

{

test: /\.less$/,

use: getStyleLoaders("less-loader"),

},

{

test: /\.s[ac]ss$/,

use: getStyleLoaders("sass-loader"),

},

{

test: /\.styl$/,

use: getStyleLoaders("stylus-loader"),

},

{

test: /\.(png|jpe?g|gif|webp)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

generator: {

// 将图片文件输出到 static/imgs 目录中

// 将图片文件命名 [hash:8][ext][query]

// [hash:8]: hash值取8位

// [ext]: 使用之前的文件扩展名

// [query]: 添加之前的query参数

filename: "static/imgs/[hash:8][ext][query]",

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

generator: {

filename: "static/media/[hash:8][ext][query]",

},

},

{

test: /\.js$/,

// exclude: /node_modules/, // 排除node_modules代码不编译

include: path.resolve(__dirname, "../src"), // 也可以用包含

use: [

{

loader: "thread-loader", // 开启多进程

options: {

workers: threads, // 数量

},

},

{

loader: "babel-loader",

options: {

cacheDirectory: true, // 开启babel编译缓存

cacheCompression: false, // 缓存文件不要压缩

plugins: ["@babel/plugin-transform-runtime"], // 减少代码体积

},

},

],

},

],

},

],

},

plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

exclude: "node_modules", // 默认值

cache: true, // 开启缓存

// 缓存目录

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

threads, // 开启多进程

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

// 提取css成单独文件

new MiniCssExtractPlugin({

// 定义输出文件名和目录

filename: "static/css/main.css",

}),

// css压缩

// new CssMinimizerPlugin(),

],

optimization: {

minimizer: [

// css压缩也可以写到optimization.minimizer里面，效果一样的

new CssMinimizerPlugin(),

// 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了

new TerserPlugin({

parallel: threads, // 开启多进程

}),

]

// devServer: {

// host: "localhost", // 启动服务器域名

// port: "3000", // 启动服务器端口号

// open: true, // 是否自动打开浏览器

// },

mode: "production",

devtool: "source-map",

};

}

```
### Image Minimizer

- 原因
> 开发如果项目中引用了较多图片，那么图片体积会比较大，将来请求速度比较慢。
我们可以对图片进行压缩，减少图片体积。

**注意：如果项目中图片都是在线链接，那么就不需要了。本地项目静态图片才需要进行压缩。**

- 概念
> `image-minimizer-webpack-plugin`: 用来压缩图片的插件

1. 下载包

```

npm i image-minimizer-webpack-plugin imagemin -D

```

还有剩下包需要下载，有两种模式：

- 无损压缩
```

npm install imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemin-svgo -D

```

- 有损压缩
```

npm install imagemin-gifsicle imagemin-mozjpeg imagemin-pngquant imagemin-svgo -D

```

- 无损压缩配置
```js

optimization: {   //压缩插件放在这个对象里

minimizer: [

// css压缩也可以写到optimization.minimizer里面，效果一样的

new CssMinimizerPlugin(),

// 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了

new TerserPlugin({

parallel: threads, // 开启多进程

}),

// 压缩图片

new ImageMinimizerPlugin({

minimizer: {

implementation: ImageMinimizerPlugin.imageminGenerate,

options: {

plugins: [

["gifsicle", { interlaced: true }],

["jpegtran", { progressive: true }],

["optipng", { optimizationLevel: 5 }],

[

"svgo",

{

plugins: [

"preset-default",

"prefixIds",

{

name: "sortAttrs",

params: {

xmlnsOrder: "alphabetical",

},

},

],

},

],

],

},

},

}),

],

},
```

### Code Split

- 原因
>打包代码时会将所有 js 文件打包到一个文件中，体积太大了。我们如果只要渲染首页，就应该只加载首页的 js 文件，其他文件不应该加载。


>所以我们需要将打包生成的文件进行代码分割，生成多个 js 文件，渲染哪个页面就只加载某个 js 文件，这样加载的资源就少，速度就更快。

- 概念
>   现在都是单页面应用， 而且是框架应用， 所以基本不用多入口打包。只需要切割单页面的js。

代码分割（Code Split）主要做了两件事：
1. 分割文件：将打包生成的文件进行分割，生成多个 js 文件。（按需引入的js会被分割打包，node_module的js会被分割打包，==在多入口打包中， 共用的方法， 也会被分割打包==）
2. 按需加载：需要哪个文件就加载哪个文件。


#### 多入口打包

- 当多入口打包时， 打包出来也是多个js文件, 但是多个js文件有共用的代码，所以要用split切割js。
```js

// webpack.config.js

const path = require("path");

const HtmlWebpackPlugin = require("html-webpack-plugin");

  

module.exports = {

// 单入口

// entry: './src/main.js',

// 多入口

entry: {  //多入口， 几个入口输出几个js， 但是还是一个html
 
main: "./src/main.js",

app: "./src/app.js",

},

output: {

path: path.resolve(__dirname, "./dist"),

// [name]是webpack命名规则，使用chunk的name作为输出的文件名。

// 什么是chunk？打包的资源就是chunk，输出出去叫bundle。

// chunk的name是啥呢？ 比如： entry中xxx: "./src/xxx.js", name就是xxx。注意是前面的xxx，和文件名无关。

// 为什么需要这样命名呢？如果还是之前写法main.js，那么打包生成两个js文件都会叫做main.js会发生覆盖。(实际上会直接报错的)

filename: "js/[name].js",  

clear: true,

},

plugins: [

new HtmlWebpackPlugin({

template: "./public/index.html",

}),

],

mode: "production",

};

```


>如果多入口文件中都引用了同一份代码，我们不希望这份代码被打包到两个文件中，导致代码重复，体积更大。我们需要提取多入口的重复代码，只打包生成一个 js 文件，其他文件引用它就好。


```js{28-67}

// webpack.config.js

const path = require("path");

const HtmlWebpackPlugin = require("html-webpack-plugin");

  

module.exports = {

// 单入口

// entry: './src/main.js',

// 多入口

entry: {

main: "./src/main.js",

app: "./src/app.js",

},

output: {

path: path.resolve(__dirname, "./dist"),

// [name]是webpack命名规则，使用chunk的name作为输出的文件名。

// 什么是chunk？打包的资源就是chunk，输出出去叫bundle。

// chunk的name是啥呢？ 比如： entry中xxx: "./src/xxx.js", name就是xxx。注意是前面的xxx，和文件名无关。

// 为什么需要这样命名呢？如果还是之前写法main.js，那么打包生成两个js文件都会叫做main.js会发生覆盖。(实际上会直接报错的)

filename: "js/[name].js",

clean: true,

},

plugins: [

new HtmlWebpackPlugin({

template: "./public/index.html",

}),

],

mode: "production",

optimization: {

// 代码分割配置

splitChunks: {

chunks: "all", // 对所有模块都进行分割

// 以下是默认值

// minSize: 20000, // 分割代码最小的大小

// minRemainingSize: 0, // 类似于minSize，最后确保提取的文件大小不能为0

// minChunks: 1, // 至少被引用的次数，满足条件才会代码分割

// maxAsyncRequests: 30, // 按需加载时并行加载的文件的最大数量

// maxInitialRequests: 30, // 入口js文件最大并行请求数量

// enforceSizeThreshold: 50000, // 超过50kb一定会单独打包（此时会忽略minRemainingSize、maxAsyncRequests、maxInitialRequests）

// cacheGroups: { // 组，哪些模块要打包到一个组

// defaultVendors: { // 组名

// test: /[\\/]node_modules[\\/]/, // 需要打包到一起的模块

// priority: -10, // 权重（越大越高）

// reuseExistingChunk: true, // 如果当前 chunk 包含已从主 bundle 中拆分出的模块，则它将被重用，而不是生成新的模块

// },

// default: { // 其他没有写的配置会使用上面的默认值

// minChunks: 2, // 这里的minChunks权重更大

// priority: -20,

// reuseExistingChunk: true,

// },

// },

// 修改配置

cacheGroups: {

// 组，哪些模块要打包到一个组

// defaultVendors: { // 组名

// test: /[\\/]node_modules[\\/]/, // 需要打包到一起的模块

// priority: -10, // 权重（越大越高）

// reuseExistingChunk: true, // 如果当前 chunk 包含已从主 bundle 中拆分出的模块，则它将被重用，而不是生成新的模块

// },

default: {

// 其他没有写的配置会使用上面的默认值

minSize: 0, // 我们定义的文件体积太小了，所以要改打包的最小文件体积

minChunks: 2,

priority: -20,

reuseExistingChunk: true,

},

},

},

},

};

```

> 虽然切割出来了公用的js， 但是我想让某些js按需导入， 不要上来就导入我引用的js文件。

想要实现按需加载，动态导入模块。还需要额外配置：

1. 修改文件
- main.js
```js

console.log("hello main");

document.getElementById("btn").onclick = function () {

// 动态导入 --> 实现按需加载

// 即使只被引用了一次，也会代码分割

import("./math.js").then(({ sum }) => {

alert(sum(1, 2, 3, 4, 5));

});

};

```

- public/index.html
```html

<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8" />

<meta http-equiv="X-UA-Compatible" content="IE=edge" />

<meta name="viewport" content="width=device-width, initial-scale=1.0" />

<title>Code Split</title>

</head>

<body>

<h1>hello webpack</h1>

<button id="btn">计算</button>

</body>

</html>

```


#### 单入口打包

>开发时我们可能是单页面应用（SPA），只有一个入口（单入口）。那么我们需要这样配置切割：

```js

const path = require("path");

const HtmlWebpackPlugin = require("html-webpack-plugin");

  

module.exports = {

// 单入口

entry: "./src/main.js",

// 多入口

// entry: {

// main: "./src/main.js",

// app: "./src/app.js",

// },

output: {

path: path.resolve(__dirname, "./dist"),

// [name]是webpack命名规则，使用chunk的name作为输出的文件名。

// 什么是chunk？打包的资源就是chunk，输出出去叫bundle。

// chunk的name是啥呢？ 比如： entry中xxx: "./src/xxx.js", name就是xxx。注意是前面的xxx，和文件名无关。

// 为什么需要这样命名呢？如果还是之前写法main.js，那么打包生成两个js文件都会叫做main.js会发生覆盖。(实际上会直接报错的)

filename: "js/[name].js",

clean: true,

},

plugins: [

new HtmlWebpackPlugin({

template: "./public/index.html",

}),

],

mode: "production",

optimization: {

// 代码分割配置

splitChunks: {

chunks: "all", // 对所有模块都进行分割

// 以下是默认值

// minSize: 20000, // 分割代码最小的大小

// minRemainingSize: 0, // 类似于minSize，最后确保提取的文件大小不能为0

// minChunks: 1, // 至少被引用的次数，满足条件才会代码分割

// maxAsyncRequests: 30, // 按需加载时并行加载的文件的最大数量

// maxInitialRequests: 30, // 入口js文件最大并行请求数量

// enforceSizeThreshold: 50000, // 超过50kb一定会单独打包（此时会忽略minRemainingSize、maxAsyncRequests、maxInitialRequests）

// cacheGroups: { // 组，哪些模块要打包到一个组

// defaultVendors: { // 组名

// test: /[\\/]node_modules[\\/]/, // 需要打包到一起的模块

// priority: -10, // 权重（越大越高）

// reuseExistingChunk: true, // 如果当前 chunk 包含已从主 bundle 中拆分出的模块，则它将被重用，而不是生成新的模块

// },

// default: { // 其他没有写的配置会使用上面的默认值

// minChunks: 2, // 这里的minChunks权重更大

// priority: -20,

// reuseExistingChunk: true,

// },

// },

},

}
}
```

==注意==：eslint不支持动态导入语法import()的检验， 所以要在加个配置在.eslist.js 
  

- 下载包
```

npm i eslint-plugin-import -D

```


- 配置
```js{9}

// .eslintrc.js

module.exports = {

// 继承 Eslint 规则

extends: ["eslint:recommended"],

env: {

node: true, // 启用node中全局变量

browser: true, // 启用浏览器中全局变量

},

plugins: ["import"], // 解决动态导入import语法报错问题 --> 实际使用eslint-plugin-import的规则解决的

parserOptions: {

ecmaVersion: 6,

sourceType: "module",

},

rules: {

"no-var": 2, // 不能使用 var 定义变量

},

};

```




#### 给动态导入文件重命名

1. 先给import注释名字
```js
  
document.getElementById("btn").onClick = function () {

// eslint会对动态导入语法报错，需要修改eslint配置文件

// webpackChunkName: "math"：这是webpack动态导入模块命名的方式

// "math"将来就会作为[name]的值显示。

import(/* webpackChunkName: "math" */ "./js/math.js").then(({ count }) => {

console.log(count(2, 1));

});

};

```

2. 再给配置的output对象配置chunkfilename
```js

entry: "./src/main.js",

output: {

path: path.resolve(__dirname, "../dist"), // 生产模式需要输出

filename: "static/js/[name].js", // 入口文件打包输出资源命名方式

chunkFilename: "static/js/[name].chunk.js", // 动态导入输出资源命名方式

assetModuleFilename: "static/media/[name].[hash][ext]", // 图片、字体等资源命名方式（注意用hash）

clean: true,

},
```

#### 统一命名配置
1. 出口js打包的名字 （不然当多入口打包的时候， 容易冲突）
2. 动态导入的js打包的名字 （不然split切割后的名字太乱）
3. 图片、字体等资源命名方式  （本身已经用hash重命名过， 只是加个本来的名字好辨识）
4. 独立抽取的css打包的名字 （css本来应该和js同名， 但是当多个js出口打包时， 或者有切割js时， 就要重命名）
```js{36-38,71-78,83-85,132-134}

const os = require("os");

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

const TerserPlugin = require("terser-webpack-plugin");

const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

  

// cpu核数

const threads = os.cpus().length;

  

// 获取处理样式的Loaders

const getStyleLoaders = (preProcessor) => {

return [

MiniCssExtractPlugin.loader,

"css-loader",

{

loader: "postcss-loader",

options: {

postcssOptions: {

plugins: [

"postcss-preset-env", // 能解决大多数样式兼容性问题

],

},

},

},

preProcessor,

].filter(Boolean);

};

  

module.exports = {

entry: "./src/main.js",

output: {

path: path.resolve(__dirname, "../dist"), // 生产模式需要输出

filename: "static/js/[name].js", // 入口文件打包输出资源命名方式

chunkFilename: "static/js/[name].chunk.js", // 动态导入输出资源命名方式

assetModuleFilename: "static/media/[name].[hash][ext]", // 图片、字体等资源命名方式（注意用hash）

clean: true,

},

module: {

rules: [

{

oneOf: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: getStyleLoaders(),

},

{

test: /\.less$/,

use: getStyleLoaders("less-loader"),

},

{

test: /\.s[ac]ss$/,

use: getStyleLoaders("sass-loader"),

},

{

test: /\.styl$/,

use: getStyleLoaders("stylus-loader"),

},

{

test: /\.(png|jpe?g|gif|svg)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

// generator: {

// // 将图片文件输出到 static/imgs 目录中

// // 将图片文件命名 [hash:8][ext][query]

// // [hash:8]: hash值取8位

// // [ext]: 使用之前的文件扩展名

// // [query]: 添加之前的query参数

// filename: "static/imgs/[hash:8][ext][query]",

// },

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

// generator: {

// filename: "static/media/[hash:8][ext][query]",

// },

},

{

test: /\.js$/,

// exclude: /node_modules/, // 排除node_modules代码不编译

include: path.resolve(__dirname, "../src"), // 也可以用包含

use: [

{

loader: "thread-loader", // 开启多进程

options: {

workers: threads, // 数量

},

},

{

loader: "babel-loader",

options: {

cacheDirectory: true, // 开启babel编译缓存

cacheCompression: false, // 缓存文件不要压缩

plugins: ["@babel/plugin-transform-runtime"], // 减少代码体积

},

},

],

},

],

},

],

},

plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

exclude: "node_modules", // 默认值

cache: true, // 开启缓存

// 缓存目录

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

threads, // 开启多进程

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

// 提取css成单独文件

new MiniCssExtractPlugin({

// 定义输出文件名和目录

filename: "static/css/[name].css",

chunkFilename: "static/css/[name].chunk.css",

}),

// css压缩

// new CssMinimizerPlugin(),

],

optimization: {

minimizer: [

// css压缩也可以写到optimization.minimizer里面，效果一样的

new CssMinimizerPlugin(),

// 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了

new TerserPlugin({

parallel: threads, // 开启多进程

}),

// 压缩图片

new ImageMinimizerPlugin({

minimizer: {

implementation: ImageMinimizerPlugin.imageminGenerate,

options: {

plugins: [

["gifsicle", { interlaced: true }],

["jpegtran", { progressive: true }],

["optipng", { optimizationLevel: 5 }],

[

"svgo",

{

plugins: [

"preset-default",

"prefixIds",

{

name: "sortAttrs",

params: {

xmlnsOrder: "alphabetical",

},

},

],

},

],

],

},

},

}),

],

// 代码分割配置

splitChunks: {

chunks: "all", // 对所有模块都进行分割

// 其他内容用默认配置即可

},

},

// devServer: {

// host: "localhost", // 启动服务器域名

// port: "3000", // 启动服务器端口号

// open: true, // 是否自动打开浏览器

// },

mode: "production",

devtool: "source-map",

};

```

### Preload / Prefetch

- 原因
> 我们前面已经做了代码分割，同时会使用 import 动态导入语法来进行代码按需加载（我们也叫懒加载，比如路由懒加载就是这样实现的）。

>但是加载速度还不够好，比如：是用户点击按钮时才加载这个资源的，如果资源体积很大，那么用户会感觉到明显卡顿效果。

>我们想在浏览器空闲时间，加载后续需要使用的资源。我们就需要用上 `Preload` 或 `Prefetch` 技术。

- `Preload`：告诉浏览器立即加载资源。
- `Prefetch`：告诉浏览器在空闲时才开始加载资源。

- 共同点
- 都只会加载资源，并不执行。
- 都有缓存。

- 区别
- `Preload`加载优先级高，`Prefetch`加载优先级低。
- `Preload`只能加载当前页面需要使用的资源，`Prefetch`可以加载当前页面资源，也可以加载下一个页面需要使用的资源。

```js
plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

exclude: "node_modules", // 默认值

cache: true, // 开启缓存

// 缓存目录

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

threads, // 开启多进程

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

// 提取css成单独文件

new MiniCssExtractPlugin({

// 定义输出文件名和目录

filename: "static/css/[name].css",

chunkFilename: "static/css/[name].chunk.css",

}),

// css压缩

// new CssMinimizerPlugin(),

new PreloadWebpackPlugin({

rel: "preload", // preload兼容性更好

as: "script",

// rel: 'prefetch' // prefetch兼容性更差

}),

],
```
### Network Cache

- 原因
> 将来开发时我们对静态资源会使用缓存来优化，这样浏览器第二次请求资源就能读取缓存了，速度很快。

>但是这样的话就会有一个问题, 因为前后输出的文件名是一样的，都叫 main.js，一旦将来发布新版本，因为文件名没有变化导致浏览器会直接读取缓存，不会加载新资源，项目也就没法更新了。

>所以我们从文件名入手，确保更新前后文件名不一样，这样就可以做缓存了。

- 概念

>它们都会生成一个唯一的 hash 值。

- fullhash（webpack4 是 hash）
每次修改任何一个文件，所有文件名的 hash 至都将改变。所以一旦修改了任何一个文件，整个项目的文件缓存都将失效。

- chunkhash
根据不同的入口文件(Entry)进行依赖文件解析、构建对应的 chunk，生成对应的哈希值。我们 js 和 css 是同一个引入，会共享一个 hash 值。

- contenthash
根据文件内容生成 hash 值，只有文件内容变化了，hash 值才会变化。所有文件 hash 值是独享且不同的。

  - 给文件名都添加contenthash， 这样浏览器缓存更方便。
```js{37-39,135-136}

const os = require("os");

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

const TerserPlugin = require("terser-webpack-plugin");

const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

const PreloadWebpackPlugin = require("@vue/preload-webpack-plugin");

  

// cpu核数

const threads = os.cpus().length;

  

// 获取处理样式的Loaders

const getStyleLoaders = (preProcessor) => {

return [

MiniCssExtractPlugin.loader,

"css-loader",

{

loader: "postcss-loader",

options: {

postcssOptions: {

plugins: [

"postcss-preset-env", // 能解决大多数样式兼容性问题

],

},

},

},

preProcessor,

].filter(Boolean);

};

  

module.exports = {

entry: "./src/main.js",

output: {

path: path.resolve(__dirname, "../dist"), // 生产模式需要输出

// [contenthash:8]使用contenthash，取8位长度

filename: "static/js/[name].[contenthash:8].js", // 入口文件打包输出资源命名方式

chunkFilename: "static/js/[name].[contenthash:8].chunk.js", // 动态导入输出资源命名方式

assetModuleFilename: "static/media/[name].[hash][ext]", // 图片、字体等资源命名方式（注意用hash）

clean: true,

},

module: {

rules: [

{

oneOf: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: getStyleLoaders(),

},

{

test: /\.less$/,

use: getStyleLoaders("less-loader"),

},

{

test: /\.s[ac]ss$/,

use: getStyleLoaders("sass-loader"),

},

{

test: /\.styl$/,

use: getStyleLoaders("stylus-loader"),

},

{

test: /\.(png|jpe?g|gif|svg)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

// generator: {

// // 将图片文件输出到 static/imgs 目录中

// // 将图片文件命名 [hash:8][ext][query]

// // [hash:8]: hash值取8位

// // [ext]: 使用之前的文件扩展名

// // [query]: 添加之前的query参数

// filename: "static/imgs/[hash:8][ext][query]",

// },

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

// generator: {

// filename: "static/media/[hash:8][ext][query]",

// },

},

{

test: /\.js$/,

// exclude: /node_modules/, // 排除node_modules代码不编译

include: path.resolve(__dirname, "../src"), // 也可以用包含

use: [

{

loader: "thread-loader", // 开启多进程

options: {

workers: threads, // 数量

},

},

{

loader: "babel-loader",

options: {

cacheDirectory: true, // 开启babel编译缓存

cacheCompression: false, // 缓存文件不要压缩

plugins: ["@babel/plugin-transform-runtime"], // 减少代码体积

},

},

],

},

],

},

],

},

plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

exclude: "node_modules", // 默认值

cache: true, // 开启缓存

// 缓存目录

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

threads, // 开启多进程

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

// 提取css成单独文件

new MiniCssExtractPlugin({

// 定义输出文件名和目录

filename: "static/css/[name].[contenthash:8].css",

chunkFilename: "static/css/[name].[contenthash:8].chunk.css",

}),

// css压缩

// new CssMinimizerPlugin(),

new PreloadWebpackPlugin({

rel: "preload", // preload兼容性更好

as: "script",

// rel: 'prefetch' // prefetch兼容性更差

}),

],

optimization: {

minimizer: [

// css压缩也可以写到optimization.minimizer里面，效果一样的

new CssMinimizerPlugin(),

// 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了

new TerserPlugin({

parallel: threads, // 开启多进程

}),

// 压缩图片

new ImageMinimizerPlugin({

minimizer: {

implementation: ImageMinimizerPlugin.imageminGenerate,

options: {

plugins: [

["gifsicle", { interlaced: true }],

["jpegtran", { progressive: true }],

["optipng", { optimizationLevel: 5 }],

[

"svgo",

{

plugins: [

"preset-default",

"prefixIds",

{

name: "sortAttrs",

params: {

xmlnsOrder: "alphabetical",

},

},

],

},

],

],

},

},

}),

],

// 代码分割配置

splitChunks: {

chunks: "all", // 对所有模块都进行分割

// 其他内容用默认配置即可

},

},

// devServer: {

// host: "localhost", // 启动服务器域名

// port: "3000", // 启动服务器端口号

// open: true, // 是否自动打开浏览器

// },

mode: "production",

devtool: "source-map",

};

```

  

- 问题：
>当我们修改 math.js 文件再重新打包的时候，因为 contenthash 原因，math.js 文件 hash 值发生了变化（这是正常的）

>但是 main.js 文件的 hash 值也发生了变化，这会导致 main.js 的缓存失效。明明我们只修改 math.js, 为什么 main.js 也会变身变化呢？

  
- 原因：
- 更新前：math.xxx.js, main.js 引用的 math.xxx.js
- 更新后：math.yyy.js, main.js 引用的 math.yyy.js, 文件名发生了变化，间接导致 main.js 也发生了变化

  
- 解决：
>将 hash 值单独保管在一个 runtime 文件中。
我们最终输出三个文件：main、math、runtime。当 math 文件发送变化，变化的是 math 和 runtime 文件，main 不变。

>runtime 文件只保存文件的 hash 值和它们与文件关系，整个文件体积就比较小，所以变化重新请求的代价也小。

```js
optimization: {

minimizer: [

// css压缩也可以写到optimization.minimizer里面，效果一样的

new CssMinimizerPlugin(),

// 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了

new TerserPlugin({

parallel: threads, // 开启多进程

}),

// 压缩图片

new ImageMinimizerPlugin({

minimizer: {

implementation: ImageMinimizerPlugin.imageminGenerate,

options: {

plugins: [

["gifsicle", { interlaced: true }],

["jpegtran", { progressive: true }],

["optipng", { optimizationLevel: 5 }],

[

"svgo",

{

plugins: [

"preset-default",

"prefixIds",

{

name: "sortAttrs",

params: {

xmlnsOrder: "alphabetical",

},

},

],

},

],

],

},

},

}),

],

// 代码分割配置

splitChunks: {

chunks: "all", // 对所有模块都进行分割

// 其他内容用默认配置即可

},

// 提取runtime文件

runtimeChunk: {     //让缓存更加持久

name: (entrypoint) => `runtime~${entrypoint.name}`, // runtime文件命名规则

},

},
```
### Core-js

- 原因
> 过去我们使用 babel 对 js 代码进行了兼容性处理，其中使用@babel/preset-env 智能预设来处理兼容性问题。

>它能将 ES6 的一些语法进行编译转换，比如箭头函数、点点点运算符等。但是如果是 async 函数、promise 对象、数组的一些方法（includes）等，它没办法处理。

>所以此时我们 js 代码仍然存在兼容性问题，一旦遇到低版本浏览器会直接报错。所以我们想要将 js 兼容性问题彻底解决

- 概念
>`core-js` 是专门用来做 ES6 以及以上 API 的 `polyfill`。

>`polyfill`翻译过来叫做垫片/补丁。就是用社区上提供的一段代码，让我们在不兼容某些新特性的浏览器上，使用该新特性。


1. 首先， 因为高级的js eslint不识别， 需要先搞定eslint的验证。

- 下载包
```

npm i @babel/eslint-parser -D

```

- .eslintrc.js
```js{4}

module.exports = {

// 继承 Eslint 规则

extends: ["eslint:recommended"],

parser: "@babel/eslint-parser", // 支持最新的最终 ECMAScript 标准

env: {

node: true, // 启用node中全局变量

browser: true, // 启用浏览器中全局变量

},

plugins: ["import"], // 解决动态导入import语法报错问题 --> 实际使用eslint-plugin-import的规则解决的

parserOptions: {

ecmaVersion: 6, // es6

sourceType: "module", // es module

},

rules: {

"no-var": 2, // 不能使用 var 定义变量

},

};

```

2. 使用`core-js`

- 下载包
```

npm i core-js

```

- 手动全部引入
- 在js入口文件里引入`import "core-js";`  但是我们只是对某一个方法做补丁。

- 手动按需引入
- 在js入口文件里引入`import "core-js/es/promise";` 但是一个一个的太麻烦

- 自动按需引入
- babel.config.js
```js

module.exports = {

// 智能预设：能够编译ES6语法

presets: [

[

"@babel/preset-env",

// 按需加载core-js的polyfill

{ useBuiltIns: "usage", corejs: { version: "3", proposals: true } },

],

],

};

```

此时就会自动根据我们代码中使用的语法，来按需加载相应的 `polyfill` 了。


### PWA

- 为什么
>开发 Web App 项目，项目一旦处于网络离线情况，就没法访问了。
我们希望给项目提供离线体验。

- 是什么
>渐进式网络应用程序(progressive web application - PWA)：是一种可以提供类似于 native app(原生应用程序) 体验的 Web App 的技术。

>其中最重要的是，在 **离线(offline)** 时应用程序能够继续运行功能。内部通过 Service Workers 技术实现的
>。

  

1. 下载包
```

npm i workbox-webpack-plugin -D

```

- 在配置文件 的插件选项配置
```js
plugins: [

new ESLintWebpackPlugin({

// 指定检查文件的根目录

context: path.resolve(__dirname, "../src"),

exclude: "node_modules", // 默认值

cache: true, // 开启缓存

// 缓存目录

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

threads, // 开启多进程

}),

new HtmlWebpackPlugin({

// 以 public/index.html 为模板创建文件

// 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源

template: path.resolve(__dirname, "../public/index.html"),

}),

// 提取css成单独文件

new MiniCssExtractPlugin({

// 定义输出文件名和目录

filename: "static/css/[name].[contenthash:8].css",

chunkFilename: "static/css/[name].[contenthash:8].chunk.css",

}),

// css压缩

// new CssMinimizerPlugin(),

new PreloadWebpackPlugin({

rel: "preload", // preload兼容性更好

as: "script",

// rel: 'prefetch' // prefetch兼容性更差

}),

new WorkboxPlugin.GenerateSW({

// 这些选项帮助快速启用 ServiceWorkers

// 不允许遗留任何“旧的” ServiceWorkers

clientsClaim: true,

skipWaiting: true,

}),
]
```

- 在mainjs的入口文件配置
```js

if ("serviceWorker" in navigator) {

window.addEventListener("load", () => {

navigator.serviceWorker

.register("/service-worker.js")

.then((registration) => {

console.log("SW registered: ", registration);

})

.catch((registrationError) => {

console.log("SW registration failed: ", registrationError);

});

});

}
```

  
此时如果直接通过 VSCode 访问打包后页面，在浏览器控制台会发现 `SW registration failed`。

因为我们打开的访问路径是：`http://127.0.0.1:5500/dist/index.html`。此时页面会去请求 `service-worker.js` 文件，请求路径是：`http://127.0.0.1:5500/service-worker.js`，这样找不到会 404。

实际 `service-worker.js` 文件路径是：`http://127.0.0.1:5500/dist/service-worker.js`。

  
- 解决路径问题

- 下载包
```

npm i serve -g

```

serve 也是用来启动开发服务器来部署代码查看效果的。

- 运行指令
```

serve dist

```

此时通过 serve 启动的服务器我们 service-worker 就能注册成功了。

## 总结

我们从 4 个角度对 webpack 和代码进行了优化：

1. 提升开发体验

- 使用 `Source Map` 让开发或上线时代码报错能有更加准确的错误提示。


2. 提升 webpack 提升打包构建速度

- 使用 `HotModuleReplacement` 让开发时只重新编译打包更新变化了的代码，不变的代码使用缓存，从而使更新速度更快。

- 使用 `OneOf` 让资源文件一旦被某个 loader 处理了，就不会继续遍历了，打包速度更快。

- 使用 `Include/Exclude` 排除或只检测某些文件，处理的文件更少，速度更快。

- 使用 `Cache` 对 eslint 和 babel 处理的结果进行缓存，让第二次打包速度更快。

- 使用 `Thead` 多进程处理 eslint 和 babel 任务，速度更快。（需要注意的是，进程启动通信都有开销的，要在比较多代码处理时使用才有效果）

  
3. 减少代码体积

- 使用 `Tree Shaking` 剔除了没有使用的多余代码，让代码体积更小。

- 使用 `@babel/plugin-transform-runtime` 插件对 babel 进行处理，让辅助代码从中引入，而不是每个文件都生成辅助代码，从而体积更小。

- 使用 `Image Minimizer` 对项目中图片进行压缩，体积更小，请求速度更快。（需要注意的是，如果项目中图片都是在线链接，那么就不需要了。本地项目静态图片才需要进行压缩。）

  

4. 优化代码运行性能

- 使用 `Code Split` 对代码进行分割成多个 js 文件，从而使单个文件体积更小，并行加载 js 速度更快。并通过 import 动态导入语法进行按需加载，从而达到需要使用时才加载该资源，不用时不加载资源。

- 使用 `Preload / Prefetch` 对代码进行提前加载，等未来需要使用时就能直接使用，从而用户体验更好。

- 使用 `Network Cache` 能对输出资源文件进行更好的命名，将来好做缓存，从而用户体验更好。

- 使用 `Core-js` 对 js 进行兼容性处理，让我们代码能运行在低版本浏览器。

- 使用 `PWA` 能让代码离线也能访问，从而提升用户体验。

## Vue 脚手架配置

  

### 开发模式配置

  

```js

// webpack.dev.js

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

const { VueLoaderPlugin } = require("vue-loader");

const { DefinePlugin } = require("webpack");

const CopyPlugin = require("copy-webpack-plugin");

  

const getStyleLoaders = (preProcessor) => {

return [

"vue-style-loader",

"css-loader",

{

loader: "postcss-loader",

options: {

postcssOptions: {

plugins: [

"postcss-preset-env", // 能解决大多数样式兼容性问题

],

},

},

},

preProcessor,

].filter(Boolean);

};

  

module.exports = {

entry: "./src/main.js",

output: {

path: undefined,

filename: "static/js/[name].js",

chunkFilename: "static/js/[name].chunk.js",

assetModuleFilename: "static/js/[hash:10][ext][query]",

},

module: {

rules: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: getStyleLoaders(),

},

{

test: /\.less$/,

use: getStyleLoaders("less-loader"),

},

{

test: /\.s[ac]ss$/,

use: getStyleLoaders("sass-loader"),

},

{

test: /\.styl$/,

use: getStyleLoaders("stylus-loader"),

},

{

test: /\.(png|jpe?g|gif|svg)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

},

{

test: /\.(jsx|js)$/,

include: path.resolve(__dirname, "../src"),

loader: "babel-loader",

options: {

cacheDirectory: true,

cacheCompression: false,

plugins: [

// "@babel/plugin-transform-runtime" // presets中包含了

],

},

},

// vue-loader不支持oneOf

{

test: /\.vue$/,

loader: "vue-loader", // 内部会给vue文件注入HMR功能代码

options: {

// 开启缓存

cacheDirectory: path.resolve(

__dirname,

"node_modules/.cache/vue-loader"

),

},

},

],

},

plugins: [

new ESLintWebpackPlugin({

context: path.resolve(__dirname, "../src"),

exclude: "node_modules",

cache: true,

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

}),

new HtmlWebpackPlugin({

template: path.resolve(__dirname, "../public/index.html"),

}),

new CopyPlugin({

patterns: [

{

from: path.resolve(__dirname, "../public"),

to: path.resolve(__dirname, "../dist"),

toType: "dir",

noErrorOnMissing: true,

globOptions: {

ignore: ["**/index.html"],

},

info: {

minimized: true,

},

},

],

}),

new VueLoaderPlugin(),

// 解决页面警告

new DefinePlugin({

__VUE_OPTIONS_API__: "true",

__VUE_PROD_DEVTOOLS__: "false",

}),

],

optimization: {

splitChunks: {

chunks: "all",

},

runtimeChunk: {

name: (entrypoint) => `runtime~${entrypoint.name}`,

},

},

resolve: {

extensions: [".vue", ".js", ".json"], // 自动补全文件扩展名，让vue可以使用

},

devServer: {

open: true,

host: "localhost",

port: 3000,

hot: true,

compress: true,

historyApiFallback: true, // 解决vue-router刷新404问题

},

mode: "development",

devtool: "cheap-module-source-map",

};

```

  

### 生产模式配置

  

```js

// webpack.prod.js

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

const TerserWebpackPlugin = require("terser-webpack-plugin");

const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

const { VueLoaderPlugin } = require("vue-loader");

const { DefinePlugin } = require("webpack");

  

const getStyleLoaders = (preProcessor) => {

return [

MiniCssExtractPlugin.loader,

"css-loader",

{

loader: "postcss-loader",

options: {

postcssOptions: {

plugins: [

"postcss-preset-env", // 能解决大多数样式兼容性问题

],

},

},

},

preProcessor,

].filter(Boolean);

};

  

module.exports = {

entry: "./src/main.js",

output: {

path: undefined,

filename: "static/js/[name].[contenthash:10].js",

chunkFilename: "static/js/[name].[contenthash:10].chunk.js",

assetModuleFilename: "static/js/[hash:10][ext][query]",

clean: true,

},

module: {

rules: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: getStyleLoaders(),

},

{

test: /\.less$/,

use: getStyleLoaders("less-loader"),

},

{

test: /\.s[ac]ss$/,

use: getStyleLoaders("sass-loader"),

},

{

test: /\.styl$/,

use: getStyleLoaders("stylus-loader"),

},

{

test: /\.(png|jpe?g|gif|svg)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

},

{

test: /\.(jsx|js)$/,

include: path.resolve(__dirname, "../src"),

loader: "babel-loader",

options: {

cacheDirectory: true,

cacheCompression: false,

plugins: [

// "@babel/plugin-transform-runtime" // presets中包含了

],

},

},

// vue-loader不支持oneOf

{

test: /\.vue$/,

loader: "vue-loader", // 内部会给vue文件注入HMR功能代码

options: {

// 开启缓存

cacheDirectory: path.resolve(

__dirname,

"node_modules/.cache/vue-loader"

),

},

},

],

},

plugins: [

new ESLintWebpackPlugin({

context: path.resolve(__dirname, "../src"),

exclude: "node_modules",

cache: true,

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

}),

new HtmlWebpackPlugin({

template: path.resolve(__dirname, "../public/index.html"),

}),

new CopyPlugin({

patterns: [

{

from: path.resolve(__dirname, "../public"),

to: path.resolve(__dirname, "../dist"),

toType: "dir",

noErrorOnMissing: true,

globOptions: {

ignore: ["**/index.html"],

},

info: {

minimized: true,

},

},

],

}),

new MiniCssExtractPlugin({

filename: "static/css/[name].[contenthash:10].css",

chunkFilename: "static/css/[name].[contenthash:10].chunk.css",

}),

new VueLoaderPlugin(),

new DefinePlugin({

__VUE_OPTIONS_API__: "true",

__VUE_PROD_DEVTOOLS__: "false",

}),

],

optimization: {

// 压缩的操作

minimizer: [

new CssMinimizerPlugin(),

new TerserWebpackPlugin(),

new ImageMinimizerPlugin({

minimizer: {

implementation: ImageMinimizerPlugin.imageminGenerate,

options: {

plugins: [

["gifsicle", { interlaced: true }],

["jpegtran", { progressive: true }],

["optipng", { optimizationLevel: 5 }],

[

"svgo",

{

plugins: [

"preset-default",

"prefixIds",

{

name: "sortAttrs",

params: {

xmlnsOrder: "alphabetical",

},

},

],

},

],

],

},

},

}),

],

splitChunks: {

chunks: "all",

},

runtimeChunk: {

name: (entrypoint) => `runtime~${entrypoint.name}`,

},

},

resolve: {

extensions: [".vue", ".js", ".json"],

},

mode: "production",

devtool: "source-map",

};

```

  

### 其他配置

  

- package.json

  

```json

{

"name": "vue-cli",

"version": "1.0.0",

"description": "",

"main": "main.js",

"scripts": {

"start": "npm run dev",

"dev": "cross-env NODE_ENV=development webpack serve --config ./config/webpack.dev.js",

"build": "cross-env NODE_ENV=production webpack --config ./config/webpack.prod.js"

},

"keywords": [],

"author": "",

"license": "ISC",

"devDependencies": {

"@babel/core": "^7.17.10",

"@babel/eslint-parser": "^7.17.0",

"@vue/cli-plugin-babel": "^5.0.4",

"babel-loader": "^8.2.5",

"copy-webpack-plugin": "^10.2.4",

"cross-env": "^7.0.3",

"css-loader": "^6.7.1",

"css-minimizer-webpack-plugin": "^3.4.1",

"eslint-plugin-vue": "^8.7.1",

"eslint-webpack-plugin": "^3.1.1",

"html-webpack-plugin": "^5.5.0",

"image-minimizer-webpack-plugin": "^3.2.3",

"imagemin": "^8.0.1",

"imagemin-gifsicle": "^7.0.0",

"imagemin-jpegtran": "^7.0.0",

"imagemin-optipng": "^8.0.0",

"imagemin-svgo": "^10.0.1",

"less-loader": "^10.2.0",

"mini-css-extract-plugin": "^2.6.0",

"postcss-preset-env": "^7.5.0",

"sass-loader": "^12.6.0",

"stylus-loader": "^6.2.0",

"vue-loader": "^17.0.0",

"vue-style-loader": "^4.1.3",

"vue-template-compiler": "^2.6.14",

"webpack": "^5.72.0",

"webpack-cli": "^4.9.2",

"webpack-dev-server": "^4.9.0"

},

"dependencies": {

"vue": "^3.2.33",

"vue-router": "^4.0.15"

},

"browserslist": ["last 2 version", "> 1%", "not dead"]

}

```

  

- .eslintrc.js

  

```js

module.exports = {

root: true,

env: {

node: true,

},

extends: ["plugin:vue/vue3-essential", "eslint:recommended"],

parserOptions: {

parser: "@babel/eslint-parser",

},

};

```

  

- babel.config.js

  

```js

module.exports = {

presets: ["@vue/cli-plugin-babel/preset"],

};

```

  

### 合并开发和生产配置

  

```js

// webpack.config.js

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

const TerserWebpackPlugin = require("terser-webpack-plugin");

const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

const { VueLoaderPlugin } = require("vue-loader");

const { DefinePlugin } = require("webpack");

const CopyPlugin = require("copy-webpack-plugin");

  

// 需要通过 cross-env 定义环境变量

const isProduction = process.env.NODE_ENV === "production";

  

const getStyleLoaders = (preProcessor) => {

return [

isProduction ? MiniCssExtractPlugin.loader : "vue-style-loader",

"css-loader",

{

loader: "postcss-loader",

options: {

postcssOptions: {

plugins: ["postcss-preset-env"],

},

},

},

preProcessor,

].filter(Boolean);

};

  

module.exports = {

entry: "./src/main.js",

output: {

path: isProduction ? path.resolve(__dirname, "../dist") : undefined,

filename: isProduction

? "static/js/[name].[contenthash:10].js"

: "static/js/[name].js",

chunkFilename: isProduction

? "static/js/[name].[contenthash:10].chunk.js"

: "static/js/[name].chunk.js",

assetModuleFilename: "static/js/[hash:10][ext][query]",

clean: true,

},

module: {

rules: [

{

// 用来匹配 .css 结尾的文件

test: /\.css$/,

// use 数组里面 Loader 执行顺序是从右到左

use: getStyleLoaders(),

},

{

test: /\.less$/,

use: getStyleLoaders("less-loader"),

},

{

test: /\.s[ac]ss$/,

use: getStyleLoaders("sass-loader"),

},

{

test: /\.styl$/,

use: getStyleLoaders("stylus-loader"),

},

{

test: /\.(png|jpe?g|gif|svg)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024, // 小于10kb的图片会被base64处理

},

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

},

{

test: /\.(jsx|js)$/,

include: path.resolve(__dirname, "../src"),

loader: "babel-loader",

options: {

cacheDirectory: true,

cacheCompression: false,

plugins: [

// "@babel/plugin-transform-runtime" // presets中包含了

],

},

},

// vue-loader不支持oneOf

{

test: /\.vue$/,

loader: "vue-loader", // 内部会给vue文件注入HMR功能代码

options: {

// 开启缓存

cacheDirectory: path.resolve(

__dirname,

"node_modules/.cache/vue-loader"

),

},

},

],

},

plugins: [

new ESLintWebpackPlugin({

context: path.resolve(__dirname, "../src"),

exclude: "node_modules",

cache: true,

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

}),

new HtmlWebpackPlugin({

template: path.resolve(__dirname, "../public/index.html"),

}),

new CopyPlugin({

patterns: [

{

from: path.resolve(__dirname, "../public"),

to: path.resolve(__dirname, "../dist"),

toType: "dir",

noErrorOnMissing: true,

globOptions: {

ignore: ["**/index.html"],

},

info: {

minimized: true,

},

},

],

}),

isProduction &&

new MiniCssExtractPlugin({

filename: "static/css/[name].[contenthash:10].css",

chunkFilename: "static/css/[name].[contenthash:10].chunk.css",

}),

new VueLoaderPlugin(),

new DefinePlugin({

__VUE_OPTIONS_API__: "true",

__VUE_PROD_DEVTOOLS__: "false",

}),

].filter(Boolean),

optimization: {

minimize: isProduction,

// 压缩的操作

minimizer: [

new CssMinimizerPlugin(),

new TerserWebpackPlugin(),

new ImageMinimizerPlugin({

minimizer: {

implementation: ImageMinimizerPlugin.imageminGenerate,

options: {

plugins: [

["gifsicle", { interlaced: true }],

["jpegtran", { progressive: true }],

["optipng", { optimizationLevel: 5 }],

[

"svgo",

{

plugins: [

"preset-default",

"prefixIds",

{

name: "sortAttrs",

params: {

xmlnsOrder: "alphabetical",

},

},

],

},

],

],

},

},

}),

],

splitChunks: {

chunks: "all",

},

runtimeChunk: {

name: (entrypoint) => `runtime~${entrypoint.name}`,

},

},

resolve: {

extensions: [".vue", ".js", ".json"],

},

devServer: {

open: true,

host: "localhost",

port: 3000,

hot: true,

compress: true,

historyApiFallback: true, // 解决vue-router刷新404问题

},

mode: isProduction ? "production" : "development",

devtool: isProduction ? "source-map" : "cheap-module-source-map",

};

```

  

## 优化配置

  

```js{11-13,29-38,151-161,199-229,237-240,252}

const path = require("path");

const ESLintWebpackPlugin = require("eslint-webpack-plugin");

const HtmlWebpackPlugin = require("html-webpack-plugin");

const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

const TerserWebpackPlugin = require("terser-webpack-plugin");

const CopyPlugin = require("copy-webpack-plugin");

const { VueLoaderPlugin } = require("vue-loader");

const { DefinePlugin } = require("webpack");

const AutoImport = require("unplugin-auto-import/webpack");

const Components = require("unplugin-vue-components/webpack");

const { ElementPlusResolver } = require("unplugin-vue-components/resolvers");

// 需要通过 cross-env 定义环境变量

const isProduction = process.env.NODE_ENV === "production";

  

const getStyleLoaders = (preProcessor) => {

return [

isProduction ? MiniCssExtractPlugin.loader : "vue-style-loader",

"css-loader",

{

loader: "postcss-loader",

options: {

postcssOptions: {

plugins: ["postcss-preset-env"],

},

},

},

preProcessor && {

loader: preProcessor,

options:

preProcessor === "sass-loader"

? {

// 自定义主题：自动引入我们定义的scss文件

additionalData: `@use "@/styles/element/index.scss" as *;`,

}

: {},

},

].filter(Boolean);

};

  

module.exports = {

entry: "./src/main.js",

output: {

path: isProduction ? path.resolve(__dirname, "../dist") : undefined,

filename: isProduction

? "static/js/[name].[contenthash:10].js"

: "static/js/[name].js",

chunkFilename: isProduction

? "static/js/[name].[contenthash:10].chunk.js"

: "static/js/[name].chunk.js",

assetModuleFilename: "static/js/[hash:10][ext][query]",

clean: true,

},

module: {

rules: [

{

test: /\.css$/,

use: getStyleLoaders(),

},

{

test: /\.less$/,

use: getStyleLoaders("less-loader"),

},

{

test: /\.s[ac]ss$/,

use: getStyleLoaders("sass-loader"),

},

{

test: /\.styl$/,

use: getStyleLoaders("stylus-loader"),

},

{

test: /\.(png|jpe?g|gif|svg)$/,

type: "asset",

parser: {

dataUrlCondition: {

maxSize: 10 * 1024,

},

},

},

{

test: /\.(ttf|woff2?)$/,

type: "asset/resource",

},

{

test: /\.(jsx|js)$/,

include: path.resolve(__dirname, "../src"),

loader: "babel-loader",

options: {

cacheDirectory: true,

cacheCompression: false,

plugins: [

// "@babel/plugin-transform-runtime" // presets中包含了

],

},

},

// vue-loader不支持oneOf

{

test: /\.vue$/,

loader: "vue-loader", // 内部会给vue文件注入HMR功能代码

options: {

// 开启缓存

cacheDirectory: path.resolve(

__dirname,

"node_modules/.cache/vue-loader"

),

},

},

],

},

plugins: [

new ESLintWebpackPlugin({

context: path.resolve(__dirname, "../src"),

exclude: "node_modules",

cache: true,

cacheLocation: path.resolve(

__dirname,

"../node_modules/.cache/.eslintcache"

),

}),

new HtmlWebpackPlugin({

template: path.resolve(__dirname, "../public/index.html"),

}),

new CopyPlugin({

patterns: [

{

from: path.resolve(__dirname, "../public"),

to: path.resolve(__dirname, "../dist"),

toType: "dir",

noErrorOnMissing: true,

globOptions: {

ignore: ["**/index.html"],

},

info: {

minimized: true,

},

},

],

}),

isProduction &&

new MiniCssExtractPlugin({

filename: "static/css/[name].[contenthash:10].css",

chunkFilename: "static/css/[name].[contenthash:10].chunk.css",

}),

new VueLoaderPlugin(),

new DefinePlugin({

__VUE_OPTIONS_API__: "true",

__VUE_PROD_DEVTOOLS__: "false",

}),

// 按需加载element-plus组件样式

AutoImport({

resolvers: [ElementPlusResolver()],

}),

Components({

resolvers: [

ElementPlusResolver({

importStyle: "sass", // 自定义主题

}),

],

}),

].filter(Boolean),

optimization: {

minimize: isProduction,

// 压缩的操作

minimizer: [

new CssMinimizerPlugin(),

new TerserWebpackPlugin(),

new ImageMinimizerPlugin({

minimizer: {

implementation: ImageMinimizerPlugin.imageminGenerate,

options: {

plugins: [

["gifsicle", { interlaced: true }],

["jpegtran", { progressive: true }],

["optipng", { optimizationLevel: 5 }],

[

"svgo",

{

plugins: [

"preset-default",

"prefixIds",

{

name: "sortAttrs",

params: {

xmlnsOrder: "alphabetical",

},

},

],

},

],

],

},

},

}),

],

splitChunks: {

chunks: "all",

cacheGroups: {

// layouts通常是admin项目的主体布局组件，所有路由组件都要使用的

// 可以单独打包，从而复用

// 如果项目中没有，请删除

layouts: {

name: "layouts",

test: path.resolve(__dirname, "../src/layouts"),

priority: 40,

},

// 如果项目中使用element-plus，此时将所有node_modules打包在一起，那么打包输出文件会比较大。

// 所以我们将node_modules中比较大的模块单独打包，从而并行加载速度更好

// 如果项目中没有，请删除

elementUI: {

name: "chunk-elementPlus",

test: /[\\/]node_modules[\\/]_?element-plus(.*)/,

priority: 30,

},

// 将vue相关的库单独打包，减少node_modules的chunk体积。

vue: {

name: "vue",

test: /[\\/]node_modules[\\/]vue(.*)[\\/]/,

chunks: "initial",

priority: 20,

},

libs: {

name: "chunk-libs",

test: /[\\/]node_modules[\\/]/,

priority: 10, // 权重最低，优先考虑前面内容

chunks: "initial",

},

},

},

runtimeChunk: {

name: (entrypoint) => `runtime~${entrypoint.name}`,

},

},

resolve: {

extensions: [".vue", ".js", ".json"],

alias: {

// 路径别名

"@": path.resolve(__dirname, "../src"),

},

},

devServer: {

open: true,

host: "localhost",

port: 3000,

hot: true,

compress: true,

historyApiFallback: true, // 解决vue-router刷新404问题

},

mode: isProduction ? "production" : "development",

devtool: isProduction ? "source-map" : "cheap-module-source-map",

performance: false,

};

```