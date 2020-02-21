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

