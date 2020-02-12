目前如果要进行**移动端开发**，就必须掌握一门移动端开发语言。

最基础的便是在各自领域的 **Android**、**iOS**。

其次便是其他的跨平台开发语言，这里主要推荐 **Google** 的 **Flutter 框架**（**Dart 语言**），以及 **Facebook** 的 **React Native 框架** (**JavaScript 语言**)。

这里主要使用 **Mac** 开发，所以主要记录 **macOS** 环境下的坑。

### 基础软件安装

1. 新版本的**Visual Studio Code**（简称**VS code**）
2. 建议安装升级最新版本的 **Xcode**，**Xcode** 是运行在操作系统 **Mac OS X**上的集成开发工具（IDE），由 **Apple Inc** 开发。**Xcode** 是开发 **macOS** 和 **iOS** 应用程序的最快捷的方式。

&emsp;&emsp;在安装 **Xcode** 的时候，要注意 **Mac 闪存** 的容量，因为 **Xcode** 有 **11GB** 左右的大小，强烈建议直接从 **App Store** 中安装，请优先确保网络的通畅。**Xcode** 安装完毕后，**需要打开运行一下**，这个时候会提示需要安装一些组件，同意即可。

### 环境搭建

开发移动端，不能只凭基本的软件，还需要对应的开发环境，不然只是单纯的 **JavaScript 项目**。

1. **Node.js** 和 **npm** 的安装

&emsp;&emsp;既然是 **JavaScript 项目**必然会使用到 JavaScript 的依赖，那么就需要使用 **npm** 包管理工具。
npm 常见的使用场景有以下几种：

- 允许用户从 NPM 服务器下载别人编写的第三方包到本地使用。
- 允许用户从 NPM 服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到 NPM 服务器供别人使用。

&emsp;&emsp;由于新版的 **Node.js** 已经集成了 **npm** ，所以可以在[Node.js 官网](https://nodejs.org/en/)下载**最新的稳定版本**，并且 **npm** 也一并安装好了。可以通过输入 `npm -v` 来测试是否成功安装。命令如下，出现版本提示表示安装成功:

> \$ npm -v<br/>
> 2.3.0

2. **CocoaPods** 安装

##### CocoaPods 是什么？

&emsp;&emsp;**CocoaPods** 是一个负责管理 **iOS** 项目中第三方开源库的工具。**CocoaPods** 的项目源码在 **Github** 上管理。开发 **iOS** 项目不可避免地要使用第三方开源库，**CocoaPods** 的出现可以节省设置和更新第三方开源库的时间，在 **iOS** 开发中经常会用到第三方库如 **AFNetworking**, **ASIHttpRequest** 等，在使用第三方库时，不光要导数源码外，集成这些依赖库还需要手动去配置，而且当这些第三方库发生了更新，需要手动去更新项目。这就显得非常麻烦。**CocoaPods** 就是为了解决这个问题而生的。通过 **CocoaPods**，我们可以将第三方的依赖库统一管理起来，配置和更新只需要通过简单的几行命令即可完成。

&emsp;&emsp;**mac** 系统已经默认安装好 **Ruby** 环境，如果不确定系统中是否有 **Ruby**，可以在终端中输入命令行：`ruby -v`查看当前**Ruby**版本。

> ruby 2.6.3p62 (2019-04-16 revision 67580) [universal.x86_64-darwin19]

&emsp;&emsp;如果是新版的 **macOS 系统**，要确认 **Ruby 版本** 是不是最新的。不是最新的需要更新，不然之后的 `pod install` 操作可能会失败。

通常有以下操作，可以解决大部分 **gem** 下载更新 **CocoaPods** 报错状况。但是不建议使用大陆地区的镜像，因为部分镜像可能停止了同步更新。

1. gem sources -l // 查看所有 gem 源，检查是否是最新的
2. gem sources --remove https://gems.ruby-china.org/ // 将当前的 gem 源删除
3. gem sources -a https://gems.ruby-china.com // 添加新的 gem 源

`2,3步骤参考官网`[RubyGems 镜像服务](https://gems.ruby-china.org/)

4. sudo gem update --system // 更新 gem
5. sudo gem install -n /usr/local/bin cocoapods --pre // 升级 CocoaPods
6. pod repo update // 更新本地仓库
7. pod install // 最后再执行 pod install 基本不会报错了

如果 `pod install` 出现 **glog** 脚本报错

请确认是不是第一次在 **Mac** 上安装 **Xcode**，是不是第一次安装环境创建工程。<br/>
执行 `sudo xcode-select --switch /Applications/Xcode.app` 即可。
