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

<br/>

* 查看某个命令的帮助信息
> npm help

<br/>

* 在帮助文档中查找包含关键词的文档列表
> npm help-search <text>

<br/>

* 查看某个命令的用法
> npm -h

<br/>

* 查看本地或者全局**node-module**目录的位置
> npm root [-g]

<br/>

* 验证*registry*的连通性和身份验证
> npm ping

<br/>

### npm 常用指令

#### 一、npm install 安装依赖

   1. > npm install <Module Name>
      * 会把*依赖包*安装到**node_modules**目录中
      * 不会修改***package.json***
      * 之后运行`npm install`命令时，不会自动安装依赖包

<br/>

   2. > npm install <Module Name> --save
      * 会把*依赖包*安装至**node_modules**目录中，
      * 会在***package.json***的**dependencies**属性中添加*依赖包*，
      * 之后运行`npm install`命令会自动安装*依赖包*到**node_module**s中
      * 运行时需要引用的*依赖包*

<br/>

   3. > npm install <Module Name> --save-dev
      * 会把*依赖包*安装到**node_modules**目录中
      * 会在***package.json***的**devDependencies**属性下添加*依赖包*
      * 之后运行`npm install`命令时，会自动安装*依赖包*到**node_modules**目录中
      * 开发过程需要使用的*依赖包*

<br/>

   4. > npm install <Module Name> -g
      * 将安装包放在 */usr/local* 下或者 *node* 的安装目录。
      * 可以直接在命令行里使用

<br/>

#### 二、npm uninstall 卸载依赖

   1. > npm uninstall <Module Name>
   * 删除*依赖包*
   * 并不能自动更新***package.json***

<br/>

   2. > npm uninstall -g <Module Name>
   * 删除全局*依赖包*
   * 并不能自动更新***package.json***

**注意**：只有加上对应参数才可以更新***package.json***：
   * `-–save`             ---> dependencies
   * `-–save-dev`         ---> devDependencies
   * `-–save-optional`    ---> optionalDependencies

<br/>

#### 三、npm update 更新依赖

**注意**：更新模块只能往新版本更新，不能往旧的版本回滚。

   1. 检查更新
      > npm install -g npm-check
      > npm-check
**注意**：
   > npm install -g npm-check-updates 
   > npm-check-updates
   将项目或者全局依赖更新到最新版本

<br/>

   2. 更新依赖
      > npm install -g npm-upgrade
      > npm-upgrade

<br/>

   3. 更新全局依赖：
      > npm update <Module Name> -g

<br/>

   4. 更新生产环境依赖：
      > npm update <Module Name> --save

<br/>

   5. 更新开发环境依赖：
      > npm update <Module Name> --save-dev

<br/>

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

   * 初始化一个新的项目
   > yarn init

<br/>

   * 查看配置文件
   > yarn config

<br/>

   * 默认显示所有依赖包和它们的依赖
   > yarn list
   * * 要限制依赖的深度，可以给 `list` 命令添加一个标志 `--depth` 所需的深度。
   > yarn list [--depth] [--pattern]

<br/>

   * *yarn* 全局缓存
   > yarn cache
   * * 打印出当前的*yarn*全局缓存位置
   > yarn cache dir
   * * 清除*yarn*全局缓存
   > yarn cache clean

<br/>

   * 显示一个依赖包为何安装
   > yarn why <Module Name>

<br/>

### yarn 常用指令

   1. > yarn install
      * 默认安装依赖

<br/>

   2. > yarn add <Module Name>
      * 添加*依赖包*到**node_modules**目录中
      * 修改***package.json***

<br/>

   3. > yarn add <Module Name> --dev
      * 添加*依赖包*到**node_modules**目录中
      * 会在***package.json***的**devDependencies**属性下添加*依赖包*
      * 开发过程需要使用的*依赖包*

<br/>

   4. > yarn global add <Module Name>
      * 全局添加*依赖包*

<br/>

   5. > yarn upgrade <Module Name>
      * 更新某个依赖包
      * 会在***package.json***更新*依赖包*

<br/>

   6. > yarn remove <Module Name>
      * 移除*依赖包*
      * 更新 ***package.json*** 和 ***yarn.lock*** 文件。

***

## Deno

A ***secure*** runtime for *JavaScript* and *TypeScript*.

**Deno**是一个基于 ***V8*** 并且内置于 *Rust* 的 ***JavaScript*** 和 ***TypeScript*** 的简单的，新式的且安全的运行环境。

* Secure by default. No file, network, or environment access, unless explicitly enabled.
* Supports TypeScript out of the box.
* Ships only a single executable file.
* Has built-in utilities like a dependency inspector (deno info) and a code formatter (deno fmt).
* Has a set of reviewed (audited) standard modules that are guaranteed to work with Deno: deno.land/std

[Deno官网](https://deno.land/)

