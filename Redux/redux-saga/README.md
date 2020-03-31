## redux-saga

[英文原版](https://redux-saga.js.org/)s

**redux-saga** 是一个用于管理应用程序 **Side Effect**（`副作用`，例如 异步获取数据，访问浏览器缓存 等）的 library，它的目标是让副作用管理更容易，执行更高效，测试更简单，在处理故障时更容易。

可以想像为，一个 **saga** 就像是应用程序中一个单独的线程，它独自负责处理副作用。 **redux-saga** 是一个 `redux` **中间件**，意味着这个线程可以通过正常的 redux action 从主应用程序启动，暂停和取消，它能访问完整的 redux state，也可以 dispatch redux action。

**redux-saga** 使用了 ES6 的 `Generator` 功能，让异步的流程更易于读取，写入和测试。通过这样的方式，这些异步的流程看起来就像是标准同步的 Javascript 代码。（有点像 `async/await`，但 `Generator` 还有一些更棒且需要的功能）。
