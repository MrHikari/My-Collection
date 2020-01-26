## React-router 和 React-router-dom

对于初步使用 React 的程序猿们在接触到 React-router 的时候很纠结，什么是 React-router？什么是 React-router-dom？

<br/>

### React-router

**React-router** 是一个基于 React 之上的强大路由库，它可以让你向应用中快速地添加视图和数据流，同时保持页面与 URL 间的同步，实现了路由的核心功能。

React-router 提供了一些 router 的核心 api，包括 Router, Route, Switch 等，但是它**没有**提供 dom 操作进行跳转的 api。

<br/>

---

### React-router-dom

**React-router-dom** 基于**React Router**，加入了在浏览器运行环境下的一些功能; 提供了 BrowserRouter, Route, Link 等 api,我们可以*通过 dom 的事件控制路由*。例如点击一个按钮进行跳转。

**React-router-native** 基于 React-router，类似 React-router-dom，加入了 React-native 运行环境下的一些功能。

从源码层面来说明：

**React-router-dom** 的部分组件只是从 **React Router** 导入，然后导出而已。

```
import {Swtich, Route, Router, HashHistory, Link} from 'react-router-dom';
```

```
import {Switch, Route, Router} from 'react-router';
import {HashHistory, Link} from 'react-router-dom';
```

上面两种写法其实没什么区别。

**React-router-dom** 依赖 **React-router**， 所以当你 npm install 的时候，即使没有下载 React-router ，其实 React-router-dom 已经为你下载完成 React-router 了。

<br/>

---

### 选择

简单的解释：其实上这两个包只要引用一个就行了，不同之处就是 **React-router-dom** 比 **React-router** 多出了 <Link> <BrowserRouter> 这样的 DOM 类组件。
因此只需引用 react-router-dom 这个包就行了。当然，如果搭配 redux ，你还需要使用 react-router-redux。

### **React-router**与**React-router-dom**的路由跳转对比

**React-router**：router4.0 以上版本用 this.props.history.push('/path')实现跳转；

router3.0 以上版本用 this.props.router.push('/path')实现跳转

**React-router-dom**：直接用 this.props.history.push('/path')就可以实现跳转

<br/>
