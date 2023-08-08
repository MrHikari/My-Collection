## Node.js

**Node.js** 是一个基于 **_Chrome V8_** 引擎的 **_JavaScript_** 运行环境。

### node.js 安装配置

#### Mac OS 上安装

可以通过以下两种方式在 **Mac OS** 上来安装 **node.js**：

1. 在官方下载网站下载 _pkg_ 安装包，直接点击安装即可 [Node.js 官网下载地址](https://nodejs.org/zh-cn/download)。
2. 使用 **brew** 命令来安装: `$ brew install node`。
3. 安装 **nvm** 命令指定对应的安装版本: `$ nvm install 版本号`。

- 实践分析

1. nodejs 官网下载

   - 可以获取 当前稳定大版本 最新大版本
   - 可以获取 所有历史大版本版本的 最终版本

2. brew install

   - 可以获取 当前最新版本 或者指定（大版本号为双数的版本最新）

3. nvm install
   - 可以指定任何版本的 node 下载

- 意外处理

1. 使用 `brew install node`，有提示错误，**node** 指令不生效
   例如：

```
brew link node
# 报错
Linking /usr/local/Cellar/node/12.5.0...
Error: Could not symlink bin/node
Target /usr/local/bin/node
already exists. You may want to remove it:
  rm '/usr/local/bin/node'

To force the link and overwrite all conflicting files:
  brew link --overwrite node

To list all files that would be deleted:
  brew link --overwrite --dry-run node
```

根据提示

```
[~]$ rm '/usr/local/bin/node'
[~]$ brew link --overwrite node
Linking /usr/local/Cellar/node/20.5.0...
Error: Could not symlink share/doc/node/gdbinit
/usr/local/share/doc/node is not writable.
[~]$ brew link --overwrite node
Linking /usr/local/Cellar/node/20.5.0...
Error: Could not symlink share/doc/node/gdbinit
/usr/local/share/doc/node is not writable.
[~]$ sudo chown -R $(whoami) /usr/local/share
[~]$ brew link --overwrite node
Linking /usr/local/Cellar/node/20.5.0... 202 symlinks created
[~]$ node -v
v20.5.0
[~]$ npm -v
8.19.3
```

2. 安装 **nvm** 提示下载成功，但是指令无效

例如：

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion


[~]$ nvm --v
[~]$ nvm: command not found
```

解决 `command not found` 的问题
此时并不是表示没有安装成功，只是需要自己手写一个配置文件，即在.nvm 中新建一个.bash_profile 的文件，将下面这两句话写入文档。

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

保存并关闭文件，然后输入下列语句，运行此文件，命令为：

```
source .bash_profile
```

关闭终端并重新打开，重新输入 nvm 检查是否可以正常显示。
如果结果依然为，`nvm: command not found`, 原因应该为：系统是最新更新的 macOS Catalina 系统，默认的 shell 是 zsh，所以找不到配置文件。

解决方案

```
# 1.新建一个 .zshrc 文件（如果没有的话）
touch ~/.zshrc
# 2.在 ~/.zshrc文件最后，增加一行
source ~/.bash_profile
```

#### **Node.js** 和 **npm** 的安装

&emsp;&emsp;既然是 **JavaScript 项目**必然会使用到 **_JavaScript_** 的依赖，那么就需要使用 **npm** 包管理工具。

**NPM** 常见的使用场景有以下几种：

- 允许用户从 NPM 服务器下载别人编写的第三方包到本地使用。
- 允许用户从 NPM 服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到 NPM 服务器供别人使用。

&emsp;&emsp;由于新版的 **Node.js** 已经集成了 **npm** ，所以可以在[Node.js 官网](https://nodejs.org/zh-cn/)下载**最新的稳定版本**，并且 **npm** 也一并安装好了。可以通过输入 `npm -v` 来测试是否成功安装。命令如下，出现版本提示表示安装成功:

> \$ npm -v<br/>
> 8.19.3

---

## NPM

**NPM** 是随同 **NodeJS** 一起安装的包管理工具，能解决 **NodeJS** 代码部署上的很多问题。

### npm 基本指令

- 查看所有命令的用例信息
  > npm -l

<br/>

- 查看某个命令的帮助信息
  > npm help

<br/>

- 在帮助文档中查找包含关键词的文档列表
  > npm help-search <text>

<br/>

- 查看某个命令的用法
  > npm -h

<br/>

- 查看本地或者全局**node-module**目录的位置
  > npm root [-g]

<br/>

- 验证*registry*的连通性和身份验证
  > npm ping

<br/>

### npm 常用指令

#### 一、npm install 安装依赖

1.  > npm install <Module Name>
    - 会把*依赖包*安装到**node_modules**目录中
    - 不会修改**_package.json_**
    - 之后运行`npm install`命令时，不会自动安装依赖包

<br/>

2.  > npm install <Module Name> --save
    - 会把*依赖包*安装至**node_modules**目录中，
    - 会在**_package.json_**的**dependencies**属性中添加*依赖包*，
    - 之后运行`npm install`命令会自动安装*依赖包*到**node_modules** 中
    - 运行时需要引用的*依赖包*

<br/>

3.  > npm install <Module Name> --save-dev
    - 会把*依赖包*安装到**node_modules**目录中
    - 会在**_package.json_**的**devDependencies**属性下添加*依赖包*
    - 之后运行`npm install`命令时，会自动安装*依赖包*到**node_modules**目录中
    - 开发过程需要使用的*依赖包*

<br/>

4.  > npm install <Module Name> -g
    - 将安装包放在 _/usr/local_ 下或者 _node_ 的安装目录。
    - 可以直接在命令行里使用

<br/>

#### 二、npm uninstall 卸载依赖

1.  > npm uninstall <Module Name>

- 删除*依赖包*
- 并不能自动更新**_package.json_**

<br/>

2.  > npm uninstall -g <Module Name>

- 删除全局*依赖包*
- 并不能自动更新**_package.json_**

**注意**：只有加上对应参数才可以更新**_package.json_**：

- `-–save` ---> dependencies
- `-–save-dev` ---> devDependencies
- `-–save-optional` ---> optionalDependencies

<br/>

#### 三、npm update 更新依赖

**注意**：更新模块只能往新版本更新，不能往旧的版本回滚。

1.  检查更新 > npm install -g npm-check > npm-check
    **注意**：
    > npm install -g npm-check-updates
    > npm-check-updates
    > 将项目或者全局依赖更新到最新版本

<br/>

2.  更新依赖
    > npm install -g npm-upgrade
    > npm-upgrade

<br/>

3.  更新全局依赖：
    > npm update <Module Name> -g

<br/>

4.  更新生产环境依赖：
    > npm update <Module Name> --save

<br/>

5.  更新开发环境依赖：
    > npm update <Module Name> --save-dev

<br/>

#### 四、npm remove 移除依赖

1.  > npm remove <Module Name>

- 删除*依赖包*
- 并不能自动更新**_package.json_**

**注意**：只有加上对应参数才可以更新**_package.json_**：

- `-–save` ---> dependencies
- `-–save-dev` ---> devDependencies
- <Module Name>可以添加版本号 @1.0.0

---

## Yarn

**Yarn** 是一个包管理器。它可以让开发者使用并分享 全世界开发者的（例如 _JavaScript_）代码。

### yarn 基本指令

- 初始化一个新的项目
  > yarn init

<br/>

- 查看配置文件
  > yarn config

<br/>

- 默认显示所有依赖包和它们的依赖
  > yarn list
- - 要限制依赖的深度，可以给 `list` 命令添加一个标志 `--depth` 所需的深度。
    > yarn list [--depth] [--pattern]

<br/>

- _yarn_ 全局缓存
  > yarn cache
- - 打印出当前的*yarn*全局缓存位置
    > yarn cache dir
- - 清除*yarn*全局缓存
    > yarn cache clean

<br/>

- 显示一个依赖包为何安装
  > yarn why <Module Name>

<br/>

### yarn 常用指令

1.  > yarn install
    - 默认安装依赖

<br/>

2.  > yarn add <Module Name>
    - 添加*依赖包*到**node_modules**目录中
    - 修改**_package.json_**

<br/>

3.  > yarn add <Module Name> --dev
    - 添加*依赖包*到**node_modules**目录中
    - 会在**_package.json_**的**devDependencies**属性下添加*依赖包*
    - 开发过程需要使用的*依赖包*

<br/>

4.  > yarn global add <Module Name>
    - 全局添加*依赖包*

<br/>

5.  > yarn upgrade <Module Name>
    - 更新某个依赖包
    - 会在**_package.json_**更新*依赖包*

<br/>

6.  > yarn remove <Module Name>
    - 移除*依赖包*
    - 更新 **_package.json_** 和 **_yarn.lock_** 文件。

---

## Deno

A **_secure_** runtime for _JavaScript_ and _TypeScript_.

**Deno**是一个基于 **_V8_** 并且内置于 _Rust_ 的 **_JavaScript_** 和 **_TypeScript_** 的简单的，新式的且安全的运行环境。

- Secure by default. No file, network, or environment access, unless explicitly enabled.
- Supports TypeScript out of the box.
- Ships only a single executable file.
- Has built-in utilities like a dependency inspector (deno info) and a code formatter (deno fmt).
- Has a set of reviewed (audited) standard modules that are guaranteed to work with Deno: deno.land/std

[Deno 官网](https://deno.land/)
