## webpack 入门学习

### webpack 是什么？

**初次认识（不完整，不正确的）**：**webpack** 是一个 **JS** 文件翻译工具，能够识别 ES Module 语法，例如： 模块引入方式 `import`，模块导出方式 `export`。

**进一步认识**： **webpack** is a module bundler.<br/>
**webpack** 实际上是一个**模块打包工具**。例如： `import 模块 from 相对路径`。`export default 模块`。<br/>
同时也支持 **CommonJS** 的引入方法：`var 模块名 = require(相对路径)`。`module.exports 模块`。

**webpack 的发展**： 单纯的 js 模块打包工具 -> （很多文件）模块打包工具

#### 安装 webpack

**前提安装 node.js**<br/>
创建 node 项目/包文件，为了让文件以 node 的规范，依据设置填写内容，最后选择 yes/no。`-y` 可以生成默认项目。

> npm init 文件名

初始化一个工程文件，主要为了创建出 `package.json` 文件。**package.json** 参考其他笔记。

**然后安装 webpack**

在开发者模式下安装 webpack-cli

> npm install webpack-cli --save-dev

如果是`全局`安装 **webpack**，`尽可能不要使用全局安装`

> npm install webpack-cli -g

删除全局安装的 **webpack**

> npm uninstall webpack-cli -g

如果不放心可以继续执行，确保安装 **webpack**

> npm install webpack --save

运行简单的网页文件,~~让 **webpack** 翻译原来写的 js 文件~~，对原始项目进行打包

> npx webpack 基本文件.js

运行之后，会在项目文件夹中生成 `dist/main.js`， 然后在基础的导入 js 文件的时候，导入`dist/main.js`。

**注意各个模块使用 ES6 语法**

**注意**：全局安装 **webpack** 不利于使用不同版本运行的项目。所以，基本在项目内安装

在`项目`中安装 **webpack**， `--save-dev` 可以简写成 `-D`，可以在包后面 `@版本号`

> npm install webpack webpack-cli --save-dev

在项目中查询当前项目的 `webpack`版本，使用 **npx** 命令

> npx webpack -v

查询 **webpack** 的信息

> npm info webpack

<br/>

---

<br/>

#### webpack 配置

1. 对于不同文件，进行不同的打包

2. webpack 的默认配置，一直在被丰富

3. webpack 的**默认**配置文件（默认必须命名为）： `webpack.config.js`

4. 自定义 webpack 的配置文件名：
> npx webpack --config 自定义的配置文件名

```js
const path = require('path'); // 导入一个核心模块

module.exports = {
  mode: 'production', // 打包模式，如果不配置，会默认设置成production，会造成warning警告。production会压缩代码为一行，development则不会
  entry: './index.js', // 打包的源代码入口文件路径
  output: { // 打包输出
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'bundle') // 组合路径，形成绝对路径（__dirname,指当前项目文件地址）
  }
}
```

##### npm scripts 简化执行命令

```js
{
  "name": "xxxxx",
  "version": "0.0.1",
  "private": true, // 私有
  "scripts": {
    "start": "webpack",
  },
  "dependencies": { // 项目需要的依赖
    "xxxxx": "^1.0.0",
    "webpack": "^4.25.1"
  },
  "devDependencies": { // 开发模式下需要安装的依赖
    "xxxxx": "^1.0.0",
    "webpack-cli": "^3.1.2"
  }
}
```

直接运行，就可以自动打包执行（前提必须有 **webpack** 配置文件配置）
> npm run start

<br/>

---

<br/>

### webpack 参数 Loader

#### 当打包不全是 js 文件的时候，webpack 配置

**Loader**：通俗理解就是 **打包方案**， **webpack** 不能识别非 **js** 模块，通过 **Loader** 提供的方案来识别，协助打包

举例子：

```js
const path = require('path'); // 导入一个核心模块
const avatar = require('图片地址') // 导入一个图片

module.exports = {
  mode: 'production', // 打包模式，如果不配置，会默认设置成production，会造成warning警告。production会压缩代码为一行，development则不会
  entry: './index.js', // 打包的源代码入口文件路径
  module: { // 未知命令会在module中寻找，可在在module中指定规则
    rules: [{
      // 意思是，如果打包中监测到jpg文件，就用file-loader去打包（阅读官方文档）
      test: /\.jpg$/,
      use: {
        loader: 'file-loader' // 比方说使用 file-loader 这个工具，注意这些工具要已安装
      }
    }]
  },
  output: { // 打包输出
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'bundle') // 组合路径，形成绝对路径（__dirname,指当前项目文件地址）
  }
}
```

#### 图片打包（重点参考官方文档）

设置 **webpack.config.js** 文件

##### file-loader

```js
const path = require('path'); // 导入一个核心模块

module.exports = {
  mode: 'development', // 打包模式，如果不配置，会默认设置成production，会造成warning警告。production会压缩代码为一行，development则不会
  entry: {
    main: '.src/index.js' // 打包的源代码入口文件路径
  },
  module: { // 未知命令会在module中寻找，可在在module中指定规则
    rules: [{
      // 意思是，如果打包中监测到jpg文件，就用file-loader去打包（阅读官方文档）
      test: /\.(jpg|png|gif)$/, // 正则匹配对应的文件
      use: {
        loader: 'file-loader', // 比方说使用 file-loader 这个工具，注意这些工具要已安装
        // 额外配置参数option
        options: {
          name: '[name].[ext]', // 打包出的图片，与原图片一致的名字和后缀，注意 单引号， []中的又叫做placeholder（占位符）
          // 例如，还可以  name: '[name]_[hash].[ext]'
          outputPath: 'images/' // 会把打包生成的文件在 dist 目录下的 images 的文件夹下
        }
      }
    }]
  },
  output: { // 打包输出
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'bundle') // 组合路径，形成绝对路径（__dirname,指当前项目文件地址）
  }
}
```

##### url-loader

注意要安装 url-loader

> npm install url-loader -D

```js
const path = require('path'); // 导入一个核心模块

module.exports = {
  mode: 'development', // 打包模式，如果不配置，会默认设置成production，会造成warning警告。production会压缩代码为一行，development则不会
  entry: {
    main: '.src/index.js' // 打包的源代码入口文件路径
  },
  module: { // 未知命令会在module中寻找，可在在module中指定规则
    rules: [{
      // 意思是，如果打包中监测到jpg文件，就用file-loader去打包（阅读官方文档）
      test: /\.(jpg|png|gif)$/, // 正则匹配对应的文件
      use: {
        // 如果图片大小低于 20kb，那么完全可以 base64 形式打包进 bundle.js文件
        loader: 'url-loader', // 使用 url-loader 这个工具，注意这些工具要已安装
        // 额外配置参数option
        options: {
          name: '[name].[ext]', // 打包出的图片，与原图片一致的名字和后缀，注意 单引号， []中的又叫做placeholder（占位符）
          // 例如，还可以  name: '[name]_[hash].[ext]'
          outputPath: 'images/', // 会把打包生成的文件在 dist 目录下的 images 的文件夹下
          limit: 20480 // 如果文件低于20480个字节（20kb），就打包进入 bundle.js 文件中， 大于limit，就和file-loader 一样，打包到 images 目录下
        }
      }
    }]
  },
  output: { // 打包输出
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'bundle') // 组合路径，形成绝对路径（__dirname,指当前项目文件地址）
  }
}
```

#### 静态资源打包（样式文件）

当引入一个 **css**，**scss** 或者 **less** 文件的时候，**webpack** 默认是不能识别的，所以需要在配置中设置

```js
const path = require('path'); // 导入一个核心模块

module.exports = {
  mode: 'development', // 打包模式，如果不配置，会默认设置成production，会造成warning警告。production会压缩代码为一行，development则不会
  entry: {
    main: '.src/index.js' // 打包的源代码入口文件路径
  },
  module: { // 未知命令会在module中寻找，可在在module中指定规则
    rules: [{
      // 意思是，如果打包中监测到jpg文件，就用file-loader去打包（阅读官方文档）
      test: /\.(jpg|png|gif)$/, // 正则匹配对应的文件
      use: {
        // 如果图片大小低于 20kb，那么完全可以 base64 形式打包进 bundle.js文件
        loader: 'url-loader', // 使用 url-loader 这个工具，注意这些工具要已安装
        // 额外配置参数option
        options: {
          name: '[name].[ext]', // 打包出的图片，与原图片一致的名字和后缀，注意 单引号， []中的又叫做placeholder（占位符）
          // 例如，还可以  name: '[name]_[hash].[ext]'
          outputPath: 'images/', // 会把打包生成的文件在 dist 目录下的 images 的文件夹下
          limit: 20480 // 如果文件低于20480个字节（20kb），就打包进入 bundle.js 文件中， 大于limit，就和file-loader 一样，打包到 images 目录下
        }
      }
    },{ // 增加新的规则，识别对应的样式文件
      test: /\.css$/,
      // 这里需要对应的loader，需要 优先安装
      // css-loader 帮助分析几个css文件中关系，然后合并为一个css
      // style-loader，在得到css-loader生成css内容后，将这些css内容挂载到页面的header部分中
      use: ['style-loader', 'css-loader']
    }]
  },
  output: { // 打包输出
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'bundle') // 组合路径，形成绝对路径（__dirname,指当前项目文件地址）
  }
}
```

如果使用一些css预处理工具，比如Sass和Less，那么就要重新设置`webpack.config.js`

`webpack.config.js` 中节选部分rules的配置
```js
rules: [{ // 增加新的规则，识别对应的样式文件
      test: /\.scss$/,
      // 这里需要对应的loader，需要 优先安装
      // css-loader 帮助分析几个css文件中关系，然后合并为一个css
      // style-loader，在得到css-loader生成css内容后，将这些css内容挂载到页面的header部分中
      // loader的执行是从下往上的，从右到左的
      use: [
        'style-loader',
        'css-loader',
        'sass-loader'
      ]
    }]
```

**避免厂商前缀**

节选部分rules的配置
```js
rules: [{ // 增加新的规则，识别对应的样式文件
      test: /\.scss$/,
      // 这里需要对应的loader，需要 优先安装
      // css-loader 帮助分析几个css文件中关系，然后合并为一个css
      // style-loader，在得到css-loader生成css内容后，将这些css内容挂载到页面的header部分中
      // loader的执行是从下往上的，从右到左的
      use: [
        'style-loader',
        'css-loader',
        'sass-loader',
        'postcss-loader'
      ]
    }]
```

**注意：** 在项目目录项配置一个 `postcss.config.js` 文件

`postcss.config.js` 中配置

```js
module.exports = {
  plugins: [
    // 配置需要使用的插件
    require('autoprefixer')
  ]
}
```

**css-loader** 常用的配置项

`webpack.config.js` 中节选部分rules的配置

```js
rules: [{ // 增加新的规则，识别对应的样式文件
      test: /\.scss$/,
      // 这里需要对应的loader，需要 优先安装
      // css-loader 帮助分析几个css文件中关系，然后合并为一个css
      // style-loader，在得到css-loader生成css内容后，将这些css内容挂载到页面的header部分中
      // loader的执行是从下往上的，从右到左的
      use: [
        'style-loader',
        // 可以使用对象，增加一些配置
        {
          loader: 'css-loader',
          options: {
            importLoaders: 2 // 通过import引入的其他scss文件，在引入之前也要执行之前的2个loader
          }
        },
        'sass-loader',
        'postcss-loader'
      ]
    }]
```

**css打包的模块化**

**css module** 概念，css 样式只在当前的模块里生效

`webpack.config.js` 中节选部分rules的配置

```js
rules: [{ // 增加新的规则，识别对应的样式文件
      test: /\.scss$/,
      // 这里需要对应的loader，需要 优先安装
      // css-loader 帮助分析几个css文件中关系，然后合并为一个css
      // style-loader，在得到css-loader生成css内容后，将这些css内容挂载到页面的header部分中
      // loader的执行是从下往上的，从右到左的
      use: [
        'style-loader',
        // 可以使用对象，增加一些配置
        {
          loader: 'css-loader',
          options: {
            importLoaders: 2, // 通过import引入的其他scss文件，在引入之前也要执行之前的2个loader
            modules: true // 开启css的模块化打包
          }
        },
        'sass-loader',
        'postcss-loader'
      ]
    }]
```

在对应模块中引入css样式时，需要`import style from './对应的css样式文件地址'`，使用的时候`style.对应的样式名`。



