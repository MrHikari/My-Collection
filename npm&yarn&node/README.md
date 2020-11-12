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

> \$ npm -v<br/>
> 2.3.0

***

## NPM

**NPM** 是随同 **NodeJS** 一起安装的包管理工具，能解决 **NodeJS** 代码部署上的很多问题。

### npm 基本指令

* 查看所有命令的用例信息
> npm -l

* 查看某个命令的帮助信息
> npm help

* 在帮助文档中查找包含关键词的文档列表
> npm help-search <text>

* 查看某个命令的用法
> npm -h

* 查看本地或者全局**node-module**目录的位置
> npm root [-g]

* 验证*registry*的连通性和身份验证
> npm ping

### npm 常用指令

#### 一、npm install 安装依赖

   1. > npm install <Module Name>
      * 会把*依赖包*安装到**node_modules**目录中
      * 不会修改***package.json***
      * 之后运行`npm install`命令时，不会自动安装依赖包

   2. > npm install <Module Name> --save
      * 会把*依赖包*安装至**node_modules**目录中，
      * 会在***package.json***的**dependencies**属性中添加*依赖包*，
      * 之后运行`npm install`命令会自动安装*依赖包*到**node_module**s中
      * 运行时需要引用的*依赖包*

   3. > npm install <Module Name> --save-dev
      * 会把*依赖包*安装到**node_modules**目录中
      * 会在***package.json***的**devDependencies**属性下添加*依赖包*
      * 之后运行`npm install`命令时，会自动安装*依赖包*到**node_modules**目录中
      * 开发过程需要使用的*依赖包*

   4. > npm install <Module Name> -g
      * 将安装包放在 */usr/local* 下或者 *node* 的安装目录。
      * 可以直接在命令行里使用

#### 二、npm uninstall 卸载依赖

   1. > npm uninstall <Module Name>
   * 删除*依赖包*
   * 并不能自动更新***package.json***

   2. > npm uninstall -g <Module Name>
   * 删除全局*依赖包*
   * 并不能自动更新***package.json***

**注意**：只有加上对应参数才可以更新***package.json***：
   * `-–save`             ---> dependencies
   * `-–save-dev`         ---> devDependencies
   * `-–save-optional`    ---> optionalDependencies

#### 三、npm update 更新依赖

**注意**：更新模块只能往新版本更新，不能往旧的版本回滚。

   1. 检查更新
      > npm install -g npm-check
      > npm-check
**注意**：
   > npm install -g npm-check-updates 
   > npm-check-updates
   将项目或者全局依赖更新到最新版本

   2. 更新依赖
      > npm install -g npm-upgrade
      > npm-upgrade

   3. 更新全局依赖：
      > npm update <Module Name> -g

   4. 更新生产环境依赖：
      > npm update <Module Name> --save

   5. 更新开发环境依赖：
      > npm update <Module Name> --save-dev

#### 四、npm remove 移除依赖

   1. > npm remove <Module Name>
   * 删除*依赖包*
   * 并不能自动更新***package.json***

**注意**：只有加上对应参数才可以更新***package.json***：
   * `-–save`             ---> dependencies
   * `-–save-dev`         ---> devDependencies
   * <Module Name>可以添加版本号 @1.0.0

***

## Yarn

**Yarn** 是一个包管理器。它可以让开发者使用并分享 全世界开发者的（例如 *JavaScript*）代码。

### yarn 基本指令

