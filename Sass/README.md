## Sass

### 世界上最成熟、最稳定、最强大的专业级CSS扩展语言！

---

### 安装Sass和Compass

**sass**基于**Ruby语言**开发而成，因此安装**sass**前需要安装**Ruby**。（**注**:`mac`下自带**Ruby**无需在安装Ruby!）

`window`下安装**SASS**首先需要安装**Ruby**，先从官网下载 Ruby 并安装。安装过程中请**注意勾选** `Add Ruby executables to your PATH` 添加到系统环境变量。

**注意**：这里以 **Mac** 为主

//安装如下(如mac安装遇到权限问题需加 `sudo gem install sass`)
> gem install sass<br/>
> gem install compass

安装完成之后，你应该通过运行下面的命令来确认应用已经正确地安装到了电脑中：

> sass -v<br/>
> Sass 3.x.x (Selective Steve)

> compass -v<br/>
> Compass 1.x.x (Polaris)<br/>
> Copyright (c) 2008-2015 Chris Eppstein<br/>
> Released under the MIT License.<br/>
> Compass is charityware.<br/>
> Please make a tax deductable donation for a worthy cause: http://umdf.org/compass

**更新sass命令**

//更新sass
> gem update sass

//查看sass版本
> sass -v

//查看sass帮助
> sass -h

---

### 编译sass

sass编译有很多种方式，如命令行编译模式、sublime插件SASS-Build、编译软件koala、前端自动化软件codekit、Grunt打造前端自动化工作流grunt-sass、Gulp打造前端自动化工作流gulp-ruby-sass等。

1. 命令行编译;
//单文件转换命令
> sass input.scss output.css

//单文件监听命令
> sass --watch input.scss:output.css

//如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
> sass --watch app/sass:public/stylesheets