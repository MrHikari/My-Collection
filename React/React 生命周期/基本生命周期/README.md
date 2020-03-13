### React组件生命周期的阶段是什么？

**React** 组件的`生命周期`有三个不同的阶段：

* **初始渲染阶段**：这是组件即将开始其生命之旅并进入 DOM 的阶段。
* **更新阶段**：一旦组件被添加到 DOM，它只有在 prop 或状态发生变化时才可能更新和重新渲染。这些只发生在这个阶段。
* **卸载阶段**：这是组件生命周期的最后阶段，组件被销毁并从 DOM 中删除。

---

### React 生命周期函数

初始化阶段：

* getDefaultProps: 获取实例的默认属性
* getInitialState: 获取每个实例的初始化状态
* componentWillMount: 组件即将被装载、渲染到页面上
* render: 组件在这里生成虚拟的 DOM 节点
* componentDidMount: 组件真正在被装载之后

运行中状态：

* componentWillReceiveProps: 组件将要接收到属性的时候调用
* shouldComponentUpdate: 组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）
* componentWillUpdate: 组件即将更新不能修改属性和状态
* render: 组件重新描绘
* componentDidUpdate: 组件已经更新

销毁阶段：

* componentWillUnmount: 组件即将销毁