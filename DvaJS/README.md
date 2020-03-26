## DvaJS

**dva** 首先是一个基于 **redux** 和 **redux-saga** 的`数据流方案`，然后为了简化开发体验，**dva** 还额外内置了 **react-router** 和 **fetch**，所以也可以理解为一个轻量级的应用框架。

### 特性

* 易学易用，仅有 **6** 个 api，对 `redux` 用户尤其友好，配合 `umi` 使用后更是降低为 0 API
* **elm 概念**，通过 `reducers`, `effects` 和 `subscriptions` 组织 `model`
* 插件机制，比如 `dva-loading` 可以自动处理 **loading 状态**，不用一遍遍地写 showLoading 和 hideLoading
* 支持 **HMR**，基于 `babel-plugin-dva-hmr` 实现 `components`、`routes` 和 `models` 的 **HMR**

---

### 安装 dva-cli 和 快速启动项目

通过 **npm** 安装 **dva-cli** 并确保版本是 `0.9.1` 或**以上**。

> $ npm install dva-cli -g<br/>
> $ dva -v<br/>
> dva-cli version 0.9.1

安装完 **dva-cli** 之后，就可以在命令行里访问到 **dva 命令**。现在，你可以通过 `dva new` **创建新应用**。

> $ dva new dva-quickstart

执行命令将创建 **dva-quickstart 目录**，包含 项目初始化目录 和 文件，并提供 开发服务器、构建脚本、数据 mock 服务、代理服务器等功能。

然后 `cd` 进入 **dva-quickstart 目录**，并启动开发服务器：

> $ cd dva-quickstart<br/>
> $ npm start