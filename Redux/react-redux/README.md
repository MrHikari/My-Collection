## React Redux

**React Redux**是**React**官方用来绑定`redux`的。它使您的 React组件 可以从 Redux store（存储）中读取数据，并向存储分派操作以更新数据。

### 安装

`React Redux 7.1` 需要 `React 16.8.3` 或 **更高版本**。

**注意**：要将 `React Redux` 与 `React` 应用程序一起使用：

> npm install react-redux

或者

> yarn add react-redux

安装 Redux 同时还需要在应用中设置 Redux 存储。

### Provider

**React Redux**提供了 `<Provider />`，这使得 Redux store（存储）可用于您的其他应用程序：

```js
import React from 'react'
import ReactDOM from 'react-dom'

import { Provider } from 'react-redux'
import store from './store'

import App from './App'

const rootElement = document.getElementById('root')
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  rootElement
)
```

### connect()

**React Redux** 提供了一个将组件连接到存储的功能 `connect`。

通常，将 `connect` 通过以下方式调用：

```js
import { connect } from 'react-redux'
import { increment, decrement, reset } from './actionCreators'

// const Counter = ...

const mapStateToProps = (state /*, ownProps*/) => {
  return {
    counter: state.counter
  }
}

const mapDispatchToProps = { increment, decrement, reset }

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter)
```
s