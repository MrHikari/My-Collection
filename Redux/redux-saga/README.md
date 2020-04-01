## redux-saga

[英文原版](https://redux-saga.js.org/)s

**redux-saga** 是一个用于管理应用程序 **Side Effect**（`副作用`，例如 异步获取数据，访问浏览器缓存 等）的 library，它的目标是让副作用管理更容易，执行更高效，测试更简单，在处理故障时更容易。

可以想像为，一个 **saga** 就像是应用程序中一个单独的线程，它独自负责处理副作用。 **redux-saga** 是一个 `redux` **中间件**，意味着这个线程可以通过正常的 redux action 从主应用程序启动，暂停和取消，它能访问完整的 redux state，也可以 dispatch redux action。

**redux-saga** 使用了 ES6 的 `Generator` 功能，让异步的流程更易于读取，写入和测试。通过这样的方式，这些异步的流程看起来就像是标准同步的 Javascript 代码。（有点像 `async/await`，但 `Generator` 还有一些更棒且需要的功能）。

### 安装

> $ npm install --save redux-saga

或者

> $ yarn add redux-saga

### 使用示例

假设在一个 **UI 界面**，最简单的一个单击按钮时从 假定的 **远程服务器** 获取一些 **用户的数据**

假定的页面和假定的点击事件方法
```js
class UserComponent extends React.Component {
  ...
  onSomeButtonClicked() {
    const { userId, dispatch } = this.props; // 获取props中已经指定的 userId数据 和 dispatch 函数
    // dispatching function 是一个用于触发 action 的函数，action 是改变 State 的唯一途径，但是它只描述了一个行为，而 dipatch 可以看作是触发这个行为的方式，而 Reducer 则是描述如何改变数据的。
    dispatch({type: 'USER_FETCH_REQUESTED', payload: {userId}});
    // 这个行为 'USER_FETCH_REQUESTED' 指 获取用户信息，参数是 payload 对象，其中有包含了 userId 数据
  }
  ...
}
```
