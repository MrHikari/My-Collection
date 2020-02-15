## webpack 入门介绍

转自 https://www.cnblogs.com/JimmyZhang/p/5479030.html _张子阳_

## 简介

&emsp;&emsp;**Webpack**是一个**前端**构建工具，本文将简要介绍它最常用的功能，并创建一个基于**webpack**的前端开发环境。

## 范例项目

&emsp;&emsp;包含两个页面，列表页`list.html`和详情页`detail.html`，仅作为**Webpack**打包的演示，不实际开发功能。范例项目是一个多页面应用，而非 SPA（单页应用）。我们将使用**Webpack**来进行前端资源的编译和打包。

## 技术选型

1. 样式：采用 SCSS，因此需要将 SCSS 转换为 CSS。
2. 脚本：采用 ES6 编写，因此需要使用 Babel 将 ES6 代码，转换为 ES5(目前浏览器所支持的)。
3. UI 框架：React，因此需要将 jsx 转换为 js 代码。

## 基本使用方法

### 建立目录结构

&emsp;&emsp;在 D 盘建一个**空文件夹**，webpack-tutorial，作为示例项目的根目录。结构如下：

- dist
- - js
- - css
- src
- - jsx
- - js
- - css

&emsp;&emsp;dist 目录存放由**webpack**打包后生成的代码，也是`.html页面`所引用的文件；**src**则是我们编写的`源码`，通常是无法直接在页面上引用的，因为当下的浏览器还无法完全支持很多新的技术，例如 ES6。

### 安装 Webpack

&emsp;&emsp;假设你已经安装了**NodeJS**和**npm 包管理工具**，并且版本在`3.8.0`以上。

&emsp;&emsp;打开命令行工具，定位到 D:\webpack-tutorial。首先运行`npm init`，生成`package.json文件`。

&emsp;&emsp;接着安装**webpack**，打开命令行工具，执行：

> npm install webpack --save-dev

&emsp;&emsp;如果是全局安装 webpack，那么在本文后面安装完 extract-text-webpack-plugin 插件后，运行时会报错，所以建议本地安装。

### 运行 webpack

&emsp;&emsp;现在就可以运行 webpack 了，只不过，现在的 Webpack 并没有任何的其他的能力，例如将 scss 转为 css。

&emsp;&emsp;在 src/js 文件夹下，建一个 common.js，公共脚本将写在这个文件里，里面现在只写一行代码：`alert("I'm common");`

&emsp;&emsp;确认命令行的当前目录为：D:\webpack-tutorial，然后执行：

> webpack src/js/common.js dist/js/common.js

&emsp;&emsp;这样将会在 dist/js 文件夹下生成一个 common.js 文件。接着在根目录下创建一个 list.html，然后通过 script 标签引用 dist/js/common.js。用浏览器打开 list.html 可以看到页面弹出提示框。

&emsp;&emsp;打开 dist/js/common.js，可以看到它和 src/js/common.js 是不同的，添加了 webpack 的模块机制，后面引入多个模块时，会看到更详细地看到它的作用：

```js
/******/ (function(modules) { // webpackBootstrap /******/ // The module cache /******/ var installedModules = {}; /******/ // The require function /******/ function __webpack_require__(moduleId) { /******/ // Check if module is in cache /******/ if(installedModules[moduleId]) /******/ return installedModules[moduleId].exports; /******/ // Create a new module (and put it into the cache) /******/ var module = installedModules[moduleId] = { /******/ exports: {}, /******/ id: moduleId, /******/ loaded: false /******/ }; /******/ // Execute the module function /******/ modules[moduleId].call(module.exports, module, module.exports, __webpack_require__); /******/ // Flag the module as loaded /******/ module.loaded = true; /******/ // Return the exports of the module /******/ return module.exports; /******/ } /******/ // expose the modules object (__webpack_modules__) /******/ __webpack_require__.m = modules; /******/ // expose the module cache /******/ __webpack_require__.c = installedModules; /******/ // __webpack_public_path__ /******/ __webpack_require__.p = ""; /******/ // Load entry module and return exports /******/ return __webpack_require__(0); /******/ }) /************************************************************************/ /******/ ([ /* 0 */ /***/ function(module, exports) { alert("I'm common"); /***/ } /******/ ]);
```

&emsp;&emsp;可以看到以上代码是很**不紧凑**的，甚至还包含了很多注释。在生产环境下，我们希望代码更加紧凑一些，此时可以使用下面的命令：

> webpack src/js/common.js dist/js/common.min.js -p

这样会生成**紧凑**的 js 代码：

```js
!(function(r) {
  function o(e) {
    if (t[e]) return t[e].exports;
    var n = (t[e] = { exports: {}, id: e, loaded: !1 });
    return r[e].call(n.exports, n, n.exports, o), (n.loaded = !0), n.exports;
  }
  var t = {};
  return (o.m = r), (o.c = t), (o.p = ""), o(0);
})([
  function(r, o) {
    alert("I'm common");
  }
]);
```

### 使用 CommonJS 构建模块

&emsp;&emsp;修改`common.js`：

> var text = "I'm common"; module.exports = text;

&emsp;&emsp;然后在`src/js`下添加另一个文件 list.js：

> var text = require("./common"); alert(text); alert("I'm list");

&emsp;&emsp;可以看到，`list.js`依赖于`common.js`，接着，再次运行**webpack**：

> webpack src/js/list.js dist/js/list.js

&emsp;&emsp;修改`list.html`，现在引用`dist/js/list.js`（之前是 common.js）。打开`list.html`，会看到它依次弹出了两次文本框。现在打开 list.js，会看到它包含了`src/common.js`和`src/list.js`中的代码。

### 添加多个模块

&emsp;&emsp;假设这个项目需要用到**jQuery**，打开命令行工具，安装它：

> npm install jquery -save

&emsp;&emsp;这会在 webpack-tutorial 目录下生成一个 node_modules 文件夹，通过 npm 安装的的模块都会在这个文件夹下。修改 src/js/list.js，让它引用 jquery：

```js
var $ = require("jquery"); // ... 中间无变化 $("body").css("background-color", "#aaaaaa");
```

### 使用 code splitting

&emsp;&emsp;现在创建一个新的 detail.html，并在 src/js/下创建 detail.js：

```js
var $ = require("jquery");
var text = require("./common");
alert(text);
alert("I'm detail");
$("body").css("background-color", "#aaaaff");
```

&emsp;&emsp;它和 list 几乎是一摸一样的，仅仅是为了示范多文件的情况。打开命令行，运行 webpack：

> webpack src/js/detail.js dist/js/detail.js

&emsp;&emsp;打开`dist/js文件夹`，会发现`detail.js`和`list.js`文件体积是一样大的，这是因为它们都包含了`common.js`和`jquery.js`。

&emsp;&emsp;`jquery.js`的体积是比较大的，因此，很有必要将`jquery.js`打包成一个单独的文件，而`list.js`和`detail.js`分开打包。在**webpack**中，可以使用称作**code splitting（代码分拆）的技术**来实现。

&emsp;&emsp;这里 jquery 本来就是一个打包好的文件，不需要打包，只需要引用。但实际项目中，可能会有很多个第三方的类库或者框架，可以将它们打包到一起。

**改写 list.js**：

```js
var text = require("./common");
alert(text);
alert("I'm list");
require.ensure(
  [],
  function() {
    var $ = require("jquery");
    $("body").css("background-color", "#aaaaaa");
  },
  "jquery"
);
```

**执行**：

> webpack src/js/list.js dist/js/list.js --display-modules --display-chunks

注意到后面多了 `--display-modules` 和 `--display-chunks` 选项，用以显示文件所包含的模块。

执行完成后，看到：

```
Hash: 087ab301df8508cffa3e Version: webpack 1.13.0 Time: 295ms Asset Size Chunks Chunk Names list.js 3.95 kB 0 [emitted] main 1.list.js 267 kB 1 [emitted] jquery chunk {0} list.js (main) 308 bytes [rendered] [0] ./src/js/list.js 248 bytes {0} [built] [1] ./src/js/common.js 60 bytes {0} [built] chunk {1} 1.list.js (jquery) 259 kB {0} [rendered] [2] ./~/jquery/dist/jquery.js 259 kB {1} [built]
```

&emsp;&emsp;可以看到生成了两个文件：`list.js`和`1.list.js`。其中`list.js`中包含了 **list.js** 和 **common.js** 两个文件，而`1.list.js`包含了**jquery.js**。

&emsp;&emsp;注意上面的一列：`Chunk Names`。**1.list.js**的 chunk 名称是**jquery**，这个是由上面 require.ensure 方法的第三个参数定义的。后面在使用配置文件时会用到这个名称。

&emsp;&emsp;接着使用浏览器打开**list.html**，用调试器查看页面元素，会发现为 head 部分新插入的 script 标签，引用了**1.list.js**。

### 使用 Chrome 查看页面

&emsp;&emsp;这里，我们发现了第一个问题：引用的路径不对。

&emsp;&emsp;暂且先不管这个问题，来看一下`code splitting`的第二个作用：**按需加载**。修改一下 list.js：

```js
var text = require("./common");
alert(text);
alert("I'm list");
if (document.getElementById("container")) {
  require.ensure(
    [],
    function() {
      var $ = require("jquery");
      $("body").css("background-color", "#aaaaaa");
    },
    "jquery"
  );
}
```

&emsp;&emsp;只有当页面存在`container元素`的时候，才去加载**jquery**。因为 list.html 是一个空页面，不包含 id 为 container 的元素。因此，并不会加载 jquery。再次执行**webpack**，打开 list.html 会发现页面没有再插入 script 标签。

&emsp;&emsp;从`list.js`中删除掉 if 判断语句，再按上面相同的方式改写一下`detail.js`，然后对 detail.js 执行**webpack**。那么很快就会发现第二个问题：**1.detail.js**中也包含了 jquery。这样的话，多个文件的共同部分只是拆分，而没有合并。

&emsp;&emsp;当使用`config配置文件`时，则很好解决上面的问题。除此以外，使用配置文件也在运行**webpack**时也省去了很多的输入。

### 使用 config 配置文件

&emsp;&emsp;现在，在项目根目录下创建一个配置文件：`webpack.config.js`。

```js
var src = "./src/js/",
  dist = "./dist/js/";
module.exports = {
  entry: { list: src + "list", detail: src + "detail" },
  output: {
    filename: "[name].js",
    chunkFilename: "[name].chunk[id].js",
    path: dist,
    publicPath: dist
  }
};
```

&emsp;&emsp;这个配置文件是一个纯粹的 js 文件，所以你可以写一些应用逻辑进去。注意到这几个配置项：

- entry：输入，当为对象时，key 为 chunk 的名称，值为模块路径。
- output.filename：输出的 js 文件名。
- output.chunkFilename：输出的 chunk 文件名。
- path：输出的文件路径。
- publicPath：页面应用的路径（即上一小节报错的路径）。

配置完成后，执行一下命令行：

> D:\webpack-tutorial>webpack --display-modules --display-chunks Hash: 2ba5904a636a8ac64101 Version: webpack 1.13.0 Time: 856ms Asset Size Chunks Chunk Names detail.js 3.96 kB 0 [emitted] detail jquery.chunk1.js 267 kB 1 [emitted] jquery list.js 4.01 kB 2 [emitted] list chunk {0} detail.js (detail) 267 bytes [rendered][0] ./src/js/detail.js 204 bytes {0} [built][1] ./src/js/common.js 63 bytes {0} {2} [built] chunk {1} jquery.chunk1.js (jquery) 259 kB {0} {2} [rendered][2] ./~/jquery/dist/jquery.js 259 kB {1} [built] chunk {2} list.js (list) 311 bytes [rendered][0] ./src/js/list.js 248 bytes {2} [built][1] ./src/js/common.js 63 bytes {0} {2} [built]

&emsp;&emsp;可以看到生成了三个文件：`jquery.chunk1.js`，包含了 jquery.js（配置文件中的[name]即为`require.ensure`方法中的第三个参数）；`detail.js`和`list.js`。下面再次打开 list.html，发现上一节的两个问题都已经解决了。

### 使用 CommonsChunkPlugin

&emsp;&emsp;在上面的例子中，我们使用`Code Splitting`将公共的**jquery.js**生成到了 `jquery.chunk1.js` 中，并可以进行按需加载（动态将 script 标签插入到页面 head 中）。

&emsp;&emsp;如果想将公共的 js 打包到同一个 js 文件中，然后手动添加到页面中，则可以使用 CommonsChunkPlugin 插件。

**修改 webpack.config.js 配置文件**：

```js
var webpack = require("webpack");
module.exports = {
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({ name: "lib", minChunks: 2 })
  ]
};
```

为了让文章紧凑一点，配置文件中相同的地方没有再列出来了，下同。

**重新执行 webpack**：

> D:\webpack-tutorial>webpack --display-modules --display-chunks Hash: 881d51e5a0d94bc015bc Version: webpack 1.13.0 Time: 858ms Asset Size Chunks Chunk Names detail.js 265 bytes 0 [emitted] detail list.js 262 bytes 1 [emitted] list lib.js 271 kB 2 [emitted] lib chunk {0} detail.js (detail) 155 bytes {2} [rendered][0] ./src/js/detail.js 155 bytes {0} [built] chunk {1} list.js (list) 152 bytes {2} [rendered][0] ./src/js/list.js 152 bytes {1} [built] chunk {2} lib.js (lib) 259 kB [rendered][1] ./src/js/common.js 63 bytes {2} [built][2] ./~/jquery/dist/jquery.js 259 kB {2} [built]

&emsp;&emsp;可以看到生成了三个文件 `detail.js`、`list.js`和 `lib.js`，其中 `lib.js` 包含了 **common.js** 和 **jquery.js**。

&emsp;&emsp;此时，要记得修改 list.html 和 detail.html，将 lib.js 手动引入进来：

```html
<script src="dist/js/lib.js"></script>
<script src="dist/js/list.js"></script>
```

&emsp;&emsp;如果我们希望只将 jquery 和其他第三方库打包到 lib 中（不包含 common.js），则可以这样配置**webpack.config.js**：

```js
var webpack = require("webpack"); module.exports = { entry: { lib: ["jquery"], // 其他的库，可以包含多个 list: src + "list", detail: src + "detail" }, plugins:[ new webpack.optimize.CommonsChunkPlugin({ name: 'lib', minChunks: Infinity }) ] };
```

运行结果如下：

> D:\webpack-tutorial>webpack --display-modules --display-chunks Hash: c0a9b7c514a8a8ad5585 Version: webpack 1.13.0 Time: 839ms Asset Size Chunks Chunk Names detail.js 385 bytes 0 [emitted] detail lib.js 271 kB 1 [emitted] lib list.js 382 bytes 2 [emitted] list chunk {0} detail.js (detail) 218 bytes {1} [rendered][0] ./src/js/detail.js 155 bytes {0} [built][1] ./src/js/common.js 63 bytes {0} {2} [built] chunk {1} lib.js (lib) 259 kB [rendered][0] multi lib 28 bytes {1} [built][2] ./~/jquery/dist/jquery.js 259 kB {1} [built] chunk {2} list.js (list) 215 bytes {1} [rendered][0] ./src/js/list.js 152 bytes {2} [built][1] ./src/js/common.js 63 bytes {0} {2} [built]

&emsp;&emsp;可以看到 list.js 和 detail.js 中都包含了 common.js，而 lib.js 中包含了第三方的 jquery.js。这样做的**好处**是：可以把第三方库整体打包做 CDN，而不会受到可能频繁变动的 common.js 的影响。

### 使用 Webpack Loader 处理多种资源

&emsp;&emsp;**Webpack Loader**有点类似于一个管道，用来增强**Webpack**的能力。如果我们需要解析 scss 代码，就需要 scss-loader，经过这个管道以后 scss 就转换为了 css；如果需要解析 ES6 代码，就需要`babel-loader`，经过这个管道以后 ES6 就转换为了 ES5。等等，以此类推。

#### 引入样式

&emsp;&emsp;我们先看第一个`loader：css loader`。它用来将样式表引入到当前文件中。

先进行一下准备工作：

- 在`src/css/`文件夹下创建一个**list.css**文件，里面就一行代码：`body{background: #aaa;}`
- 因为现在已经不再演示处理多文件了，所以在`webpack.config.js`中删去`detail: src + "detail"` 这行。
- 删掉 src/js/list.js 中的所有代码

安装 css-loader：

> npm install css-loader --save-dev

修改 list.js

```js
require("css!../css/list.css");
```

运行一遍 webpack，可以看到 dist/js/list.js 将 list.css 包含了进去：

> D:\webpack-tutorial>webpack --display-modules --display-chunks Hash: a669b2f88986bbfd0da0 Version: webpack 1.13.0 Time: 585ms Asset Size Chunks Chunk Names list.js 3.29 kB 0 [emitted] list chunk {0} list.js (list) 1.73 kB [rendered][0] ./src/js/list.js 35 bytes {0} [built][1] ./~/css-loader!./src/css/list.css 186 bytes {0} [built][2] ./~/css-loader/lib/css-base.js 1.51 kB {0} [built]

&emsp;&emsp;这时候，如果打开 list.html，会发现什么变化都没有，这是因为 css-loader 的作用仅仅是将 css 文件链入到脚本中，却不对它做任何处理。

&emsp;&emsp;此时，如果希望将样式的内容渲染到页面上，则需要安装另一个 loader：style-loader。

显然，设计原则是：一个 loader 只干一件事。

> npm install style-loader --save-dev

接着修改 list.js

```js
require("style!css!../css/list.css");
```

&emsp;&emsp;这是一个链式语法，意思是：先用 css-loader 加载 list.css 文件，然后再用`style-loader`渲染它。

&emsp;&emsp;再次运行 webpack，然后打开 list.html，发现页面已经加载了 list.css 样式，页面背景变成了灰色(#aaa)。

&emsp;&emsp;用**Chrome 浏览器**查看页面源码，会发现样式是嵌入到 list.js 中去的，而不是通过 link 标签引入到页面中。这样就会带来一个问题：页面闪烁。当你频繁刷新页面时，会发现在脚本加载完成前，页面是默认的白色，等到脚本加载完成后才变成灰色。

&emsp;&emsp;对一个 Web 组件而言，它应当包含 HTML 结构，CSS 样式表，JS 脚本控制行为，还可能包含字体和图片。这些统称为**资源 assets**，它们统统可以通过 Webpack 打包成一个 js 文件。如果是开发一个 Web 组件，这么做通常没有太大问题，因为 Web 组件只是页面的一小块区域。但如果是全局的样式，资源加载前带来的闪烁问题则会影响用户体验。
将图片和 HTML 打包到 js 中，也需要相应的 loader，这里就不在演示了。

&emsp;&emsp;如此一来，我们希望将 css 文件生成到 dist/css 文件夹中，再通过传统的方式用 link 标签进行引用，而不是生成到 list.js 脚本中。现在，重新编辑一下 webpack.config.js：

```js
var src = "./src/js/",
  dist = "./dist/js/";
module.exports = {
  entry: { list: src + "list" },
  output: {
    filename: "[name].js",
    chunkFilename: "[name].chunk[id].js",
    path: dist,
    publicPath: dist
  },
  module: { loaders: [{ test: /\.css$/, loaders: ["style", "css"] }] }
};
```

这里添加了 loaders 的配置。然后修改 list.js：

```js
require("../css/list.css");
```

&emsp;&emsp;然后运行一遍 webpack，会发现没有任何改变，list.css 依然是打包进了 list.js 中。上面修改配置，不过是将 loader 的使用规则放到了配置中，并没有其他变化。

&emsp;&emsp;为了将 css 打包到 dist/css 文件夹，需要另一个 webpack 插件：extract-text-webpack-plugin。打开命令行，进行安装：

> npm install extract-text-webpack-plugin --save-dev

然后修改`webpack.config.js`配置文件：

```js
var src = "./src/js/",
  dist = "./dist/js/";
var ExtractTextPlugin = require("extract-text-webpack-plugin");
module.exports = {
  entry: { list: src + "list" },
  output: {
    filename: "[name].js",
    chunkFilename: "[name].chunk[id].js",
    path: dist,
    publicPath: dist
  },
  module: {
    loaders: [
      { test: /\.css$/, loader: ExtractTextPlugin.extract("style", "css") }
    ]
  },
  plugins: [new ExtractTextPlugin("../css/[name].css", { allChunks: true })]
};
```

&emsp;&emsp;再次运行 webpack，可以看到在 dist 文件夹下生成了 list.css 文件。注意 plugins 中 new ExtractTextPlugin 的参数，第一个参数指定了生成的 css 文件的位置，这个位置是相对于配置中的 output.path（./dist/js/），而不是相当于项目的根目录(D:\webpack-tutorial)。

修改 list.html，在 head 中引用刚才生成的 css 文件，可以看到一切正常：

```html
<link href="dist/css/list.css" rel="stylesheet" />
```

&emsp;&emsp;上面做了这么多的工作，相当于将 list.css 从 src/css 文件夹拷贝一份到 dist/css 文件夹，好像并没有太大意义。正常情况下，是在 src/css 下编写 scss 文件，然后生成到 dist/css 中。可能你已经猜到了，我们又需要安装一个 loader 了：sass-loader。

> npm install sass-loader node-sass --save-dev

接着再次修改配置文件：

```js
module: {
  loaders: [
    { test: /\.css$/, loader: ExtractTextPlugin.extract("style", "css") },
    { test: /\.scss$/, loader: ExtractTextPlugin.extract("style", "css!sass") }
  ];
}
```

在上面多加了一个 sass-loader 用于处理.scss 文件。接着删除 src/css/list.css，然后创建 list.scss 文件：

> $bg-main:#aaa; body{ background:$bg-main; }

&emsp;&emsp;修改 list.js `require("../css/list.scss");`。然后再次运行 webpack，成功后可以看到 dist/css 下生成的 css。如此一来，就可以在项目中编写 scss 文件了。

#### 使用 Babel 处理 ES6

&emsp;&emsp;2015 年推出了 ES6（ES2015），可惜现在浏览器的支持很有限。但好在有 Babel 这样的神器，可以将 ES6 转为现在浏览器所支持的 ES5。有了上面的经验，后面就容易很多了，因为都是大同小异的。

先安装 babel-loader 和 babel-preset-2015：

> npm install babel-loader babel-core babel-preset-es2015 --save-dev

&emsp;&emsp;可能很多人会问：babel-preset-es2015 是做什么的？简单说一下，就是 babel 本身也是啥都做不了，要借助 preset 来完成，目的同样是为了架构更简洁吧。

修改 webpack.config.js，加入 babel-loader：

```js
module: {
  loaders: [
    {
      test: /\.jsx?$/,
      exclude: /node_modules/,
      loader: "babel",
      query: { presets: ["es2015"] }
    }
  ];
}
```

现在，修改 list.js，编写一段 ES6 代码：

```js
var arr = [1, 2, 3].map(x => x + 1);
alert(arr);
```

运行**webpack**，然后打开`dist/js/list.js`，会看到 lambda 表达式 x=>x+1 已经转为了匿名函数：

```js
function(module, exports) { "use strict"; var arr = [1, 2, 3].map(function (x) { return x + 1; }); alert(arr); }
```

#### 引入 React

&emsp;&emsp;因为我们选用**React**作为前端 UI 框架，那么就需要让**Webapck**支持它。不同的是：React 是 Babel 的一个预设，而非 Webpack 的一个 Loader。现在先安装它：

> npm install react react-dom babel-preset-react --save-dev

这里 react 和 react-dom 是 React 的核心模块；babel-preset-react 则是用于 babel 的 preset。

接着修改 webpack.config.js：

```js
{ test: /\.jsx?$/, loader: 'babel-loader', exclude:/node_modules/, query:{ presets: ['es2015', "react"] } }
```

在 src/jsx 文件夹下创建一个`hello.jsx`，用于测试**React**：

```js
var React = require("react");
var ReactDOM = require("react-dom");
var HelloMessage = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});
ReactDOM.render(
  <HelloMessage name="张子阳" />,
  document.getElementById("container")
);
```

&emsp;&emsp;修改`src/js/list.js`：

```js
import Hello from "../jsx/hello.jsx";
```

&emsp;&emsp;这里**不可以**require，只能**import**。

&emsp;&emsp;修改 list.html，在 body 中加入一行代码：`<div id="container"></div>`。打开 list.html，会看到 “Hello 张子阳”的字样。

&emsp;&emsp;生成的 dist/js/list.js 会变得非常庞大，因为包含了很多 react 的代码，可以使用前面提到的功能生成多个文件，这里就不再演示了。
