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

然后根据命令栏的提示，在浏览器里打开 `http://localhost:8000`(或者其他运行的接口) ，可以看到 **dva** 的欢迎界面。

### 定义路由

我们要写个应用来先显示产品列表。首先第一步是创建路由，路由可以想象成是组成应用的不同页面。

在`routes`目录下新建 页面文件，举例内容如下：
```js
import React from 'react';

const Products = (props) => (
  <h2>List of Products</h2>
);

export default Products;
```
添加路由信息到路由表，编辑 `router.js` :
```js
+ import Products from './routes/Products'; // 在文件上方导入页面文件
...
+ <Route path="/products" exact component={Products} /> // 定义相应的 Router 挑战
```
然后在浏览器里打开 `http://localhost:8000/#/products` ，就可以看到新加入的页面。

---

### 概念

#### 数据流向

数据的改变发生**通常是**通过`用户交互行为`或者`浏览器行为`（如路由跳转等）触发的，当此类行为会改变数据的时候可以通过 **dispatch** 发起一个 **action**，如果是`同步行为`会直接通过 **Reducers** 改变 `State` ，如果是**异步行为**（副作用）会**先**触发 `Effects` 然后流向 `Reducers` 最终改变 `State`。

![Dva数据流向示意图]()