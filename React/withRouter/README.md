### withRouter

作用：把不是通过路由切换过来的组件中，将react-router 的 history、location、match 三个对象传入props对象上
 
默认情况下必须是经过路由匹配渲染的组件才存在this.props，才拥有路由参数，才能使用编程式导航的写法，执行this.props.history.push('/detail')跳转到对应路由的页面
然而不是所有组件都直接与路由相连（通过路由跳转到此组件）的，当这些组件需要路由参数时，使用withRouter就可以给此组件传入路由参数，此时就可以使用this.props
 
高阶组件中的withRouter, 作用是将一个组件包裹进Route里面, 然后react-router的三个对象history, location, match就会被放进这个组件的props属性中.

如何使用withRouter：
比如app.js这个组件，一般是首页，不是通过路由跳转过来的，而是直接从浏览器中输入地址打开的，如果不使用withRouter此组件的this.props为空，没法执行props中的history、location、match等方法。

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
上面是实现的原理, react-router-dom 里面是有这个组件的, 直接引入使用就可以了

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
所以withRouter的作用就是, 如果我们某个东西不是一个Router, 但是我们要依靠它去跳转一个页面, 比如点击页面的logo, 返回首页, 这时候就可以使用withRouter来做.
在这个例子中, 我将span使用withRouter作为一个可点击跳转的Link
