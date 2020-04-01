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

这个组件 `dispatch` 一个 `plain Object` 的 **action** 到 **Store**。`redux-saga` 会创建一个 **Saga** 来**监听**所有的 `USER_FETCH_REQUESTED` **action**，并**触发**一个 **API** 调用获取用户数据。

在 sagas.js 中
```js
import { call, put, takeEvery, takeLatest } from 'redux-saga/effects';
import Api from '...'; // 从对应的文件获得Api请求路径

// worker Saga : 将在 USER_FETCH_REQUESTED action 被 dispatch 时调用
function* fetchUser(action) {
   try {
      const user = yield call(Api.fetchUser, action.payload.userId);
      yield put({type: "USER_FETCH_SUCCEEDED", user: user});
   } catch (e) {
      yield put({type: "USER_FETCH_FAILED", message: e.message});
   }
}

/*
  在每个 `USER_FETCH_REQUESTED` action 被 dispatch 时调用 fetchUser
  允许并发（译注：即同时处理多个相同的 action）
*/
function* mySaga() {
  yield takeEvery("USER_FETCH_REQUESTED", fetchUser);
}

/*
  也可以使用 takeLatest

  不允许并发，dispatch 一个 `USER_FETCH_REQUESTED` action 时，
  如果在这之前已经有一个 `USER_FETCH_REQUESTED` action 在处理中，
  那么处理中的 action 会被取消，只会执行当前的
*/
function* mySaga() {
  yield takeLatest("USER_FETCH_REQUESTED", fetchUser);
}

export default mySaga;
```

为了能跑起 Saga，我们需要使用 redux-saga 中间件将 Saga 与 Redux Store 建立连接。

在 main.js 文件中
```js
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'

import reducer from './reducers'
import mySaga from './sagas'

// create the saga middleware
const sagaMiddleware = createSagaMiddleware()
// mount it on the Store
const store = createStore(
  reducer,
  applyMiddleware(sagaMiddleware)
)

// then run the saga
sagaMiddleware.run(mySaga)

// render the application
```