### withRouter

**作用**：在不是通过路由切换过来的组件，将`react-router` 的 `history`、`location`、`match` **三个对象**传入**props对象**上。
 
默认情况下**必须**是经过*路由匹配渲染*的组件才存在`this.props`，才拥有路由参数，才能使用编程式导航的写法。（例如：执行 `this.props.history.push('/xxxx')` 跳转到对应路由的页面。<br/>
然而不是所有组件都直接与路由相连（通过路由跳转到此组件）的，当这些组件**需要路由参数**时，使用`withRouter`就可以给此组件传入路由参数，此时就可以使用`this.props`。
 
**注意**：**高阶组件**中的`withRouter`, 作用是将一个组件包裹进*Route*里面, 然后`react-router`的三个对象`history`, `location`, `match`就会被放进这个组件的`props`*属性*中.

例如：*app.js*这个组件，一般是首页，不是通过路由跳转过来的，而是直接从浏览器中输入地址打开的，如果不使用`withRouter`，此组件的`this.props`为空，没法执行`props`中的`history`、`location`、`match`等方法。

```js
// withRouter实现原理: 
// 将组件包裹进 Route, 然后返回
// const withRouter = () => {
//     return () => {
//         return <Route component={Nav} />
//     }
// }

// 这里是简化版
const withRouter = ( Component ) => () => <Route component={ Component }/>
```
上面是实现的原理, ｀react-router-dom｀ 里面是有这个组件的, 直接引入使用就可以了

```js
import React from 'react'
import './nav.css'
import {
    NavLink,
    withRouter
} from "react-router-dom"

class Nav extends React.Component{
    handleClick = () => {
        // Route 的 三个对象将会被放进来, 对象里面的方法可以被调用
        console.log(this.props);
    }
    render() {
        return (
            <div className={'nav'}>
                <span className={'logo'} onClick={this.handleClick}>掘土社区</span>
                <li><NavLink to="/" exact>首页</NavLink></li>
                <li><NavLink to="/activities">动态</NavLink></li>
                <li><NavLink to="/topic">话题</NavLink></li>
                <li><NavLink to="/login">登录</NavLink></li>
            </div>
        );
    }
}

// 导出的是 withRouter(Nav) 函数执行
export default withRouter(Nav)
```

**withRouter**的作用：如果某个组件不是一个**Router**, 但是需要操作它跳转一个页面, 就可以使用`withRouter`来实现。
