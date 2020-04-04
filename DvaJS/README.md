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

#### Models

##### State

```js
type State = any
```

**State** 表示 **Model** 的`状态数据`，通常表现为一个 **javascript 对象**（当然它可以是任何值）；操作的时候每次都要当作**不可变数据**（immutable data）来对待，保证每次都是全新对象，没有引用关系，这样才能保证 State 的**独立性**，便于测试和追踪变化。

在 dva 中可以通过 dva 的实例属性 `_store` 看到顶部的 **state 数据**，但是通常很少会用到:

```js
const app = dva();
console.log(app._store); // 实例化dva，然后查看顶部的 state 数据，
```

##### Action

```js
type AsyncAction = any
```

**Action** 是一个普通 **javascript 对象**，它是改变 **State** 的`唯一途径`。无论是从 UI 事件、网络回调，还是 WebSocket 等数据源所获得的数据，最终都会通过 `dispatch` 函数调用一个 **action**，从而改变对应的数据。**action** 必须带有 `type 属性`指明具体的行为，其它字段可以自定义，如果要发起一个 **action** 需要使用 `dispatch 函数`；需要注意的是 `dispatch` 是在组件 `connect Models`以后，通过 `props` 传入的。

```js
dispatch({
  type: 'add',
});
```

##### dispatch 函数

```js
type dispatch = (a: Action) => Action
```

`dispatching function` 是一个用于触发 **action** 的`函数`，**action** 是改变 **State** 的唯一途径，但是它只描述了一个行为，而 `dipatch` 可以看作是**触发这个行为**的`方式`，而 **Reducer** 则是描述如何改变数据的。

在 dva 中，`connect Model` 的组件通过 `props` 可以访问到 `dispatch`，可以调用 **Model** 中的 **Reducer** 或者 **Effects**，常见的形式如：

```js
dispatch({
  type: 'user/add', // 如果在 model 外调用，需要添加 namespace
  payload: {}, // 需要传递的信息
});
```

##### Reducer

```js
type Reducer<S, A> = (state: S, action: A) => S
```

**Reducer**（也称为 `reducing function`）函数接受**两个参数**：之前已经累积运算的结果和当前要被累积的值，返回的是一个新的累积结果。该函数把一个**集合**归并成一个**单值**。

**Reducer** 的概念来自于是函数式编程，很多语言中都有 **reduce API**。如在 javascript 中：

```js
[{x:1},{y:2},{z:3}].reduce(function(prev, next){
    return Object.assign(prev, next);
})
// return {x:1, y:2, z:3}
```

在 dva 中，`reducers` 聚合积累的结果是当前 **model** 的 `state 对象`。通过 `actions` 中传入的值，与当前 `reducers` 中的值进行运算获得新的值（也就是新的 state）。需要注意的是 **Reducer** 必须是纯函数，所以同样的输入必然得到同样的输出，它们不应该产生任何副作用。并且，每一次的计算都应该使用immutable data，这种特性简单理解就是每次操作都是返回一个全新的数据（独立，纯净），所以热重载和时间旅行这些功能才能够使用。

##### Effect

**Effect** 被称为副作用，在 dva 的应用中，最常见的就是异步操作。它来自于函数编程的概念，之所以叫副作用是因为它使得我们的函数变得不纯，同样的输入不一定获得同样的输出。

dva 为了`控制副作用`的操作，底层引入了 **redux-sagas** 做异步流程控制，由于采用了 **generator** 的相关概念，所以`将异步转成同步`写法，从而将 `effects` 转为**纯函数**。

##### Subscription

**Subscriptions** 是一种从 `源` 获取数据的方法，它来自于 *elm*。

**Subscription** 语义是订阅，用于订阅一个数据源，然后根据条件 `dispatch` 需要的 `action`。数据源可以是当前的时间、服务器的 **websocket 连接**、**keyboard 输入**、**geolocation 变化**、**history 路由**变化等等。

```js
import key from 'keymaster';
...
app.model({
  namespace: 'count',
  subscriptions: {
    keyEvent({ dispatch }) {
      key('⌘+up, ctrl+up', () => { dispatch({ type:'add' }) });
    },
  }
});
```

##### Router

这里的路由通常指的是 **前端路由**，由于目前 React 应用现在通常是`单页应用`，所以需要前端代码来控制路由逻辑，通过浏览器提供的 **History API** 可以监听 **浏览器 url** 的变化，从而控制路由相关操作。

dva 实例提供了 `router` 方法来控制路由，使用的是 `react-router`。

```js
import { Router, Route } from 'dva/router';
app.router(({history}) =>
  <Router history={history}>
    <Route path="/" component={HomePage} />
  </Router>
);
```

##### Route Components

在 dva 中，在组件设计方法方面通常将其约束为 Route Components，因为在 dva 中通常以 `页面维度` 来设计 **Container Components**。

所以在 dva 中，通常需要 `connect Model` 的组件都是 `Route Components`，组织在 /routes/ 目录下，而 `/components/` 目录下则是 **纯组件**（**Presentational Components**）。
