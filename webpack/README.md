## webpack 入门学习

### webpack 是什么？

**初次认识（不完整，不正确的）**：**webpack** 是一个 **JS** 文件翻译工具，能够识别 ES Module 语法，例如： 模块引入方式 `import`，模块导出方式 `export`。

**进一步认识**： **webpack** is a module bundler.<br/>
**webpack** 实际上是一个**模块打包工具**。例如： `import 模块 from 相对路径`。`export default 模块`。<br/>
同时也支持 **CommonJS** 的引入方法：`var 模块名 = require(相对路径)`。`module.exports 模块`。

**webpack 的发展**： 单纯的 js 模块打包工具 -> （很多文件）模块打包工具

#### 安装 webpack

**前提安装 node.js**

> npm init 文件名

初始化一个工程文件，主要为了创建出 `package.json` 文件。

**然后安装 webpack**

在开发者模式下安装 webpack-cli

> npm install webpack-cli --save-dev

如果不放心可以继续执行，确保安装 **webpack**

> npm install webpack --save

运行简单的网页文件,~~让 **webpack** 翻译原来写的 js 文件~~，对原始项目进行打包

> npx webpack 基本文件.js

运行之后，会在项目文件夹中生成 `dist/main.js`， 然后在基础的导入 js 文件的时候，导入`dist/main.js`。

**注意各个模块使用 ES6 语法**
