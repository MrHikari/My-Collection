## Node.js

**Node.js** 是一个基于 ***Chrome V8*** 引擎的 ***JavaScript*** 运行环境。

### node.js 安装配置

#### Mac OS 上安装

可以通过以下两种方式在 **Mac OS** 上来安装 **node.js**：

1. 在官方下载网站下载 *pkg* 安装包，直接点击安装即可。
2. 使用 **brew** 命令来安装: `$ brew install node`

#### **Node.js** 和 **npm** 的安装

&emsp;&emsp;既然是 **JavaScript 项目**必然会使用到 ***JavaScript*** 的依赖，那么就需要使用 **npm** 包管理工具。

**NPM** 常见的使用场景有以下几种：

- 允许用户从 NPM 服务器下载别人编写的第三方包到本地使用。
- 允许用户从 NPM 服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到 NPM 服务器供别人使用。

&emsp;&emsp;由于新版的 **Node.js** 已经集成了 **npm** ，所以可以在[Node.js 官网](https://nodejs.org/en/)下载**最新的稳定版本**，并且 **npm** 也一并安装好了。可以通过输入 `npm -v` 来测试是否成功安装。命令如下，出现版本提示表示安装成功:

> \$ npm -v
> 2.3.0

***

## NPM

**NPM** 是随同 **NodeJS** 一起安装的包管理工具，能解决 **NodeJS** 代码部署上的很多问题。

### npm 指令

* 查看所有命令的用例信息
> npm -l

* 查看某个命令的帮助信息
> npm help

* 在帮助文档中查找包含关键词的文档列表
> npm help-search

* 查看某个命令的用法
> npm -h

* 查看本地或者全局node-module目录的位置
> npm root [-g]

* 验证registry的连通性和身份验证
> npm ping


### 📚准备阶段


#### 一、npm install 安装依赖






