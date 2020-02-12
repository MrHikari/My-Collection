### componentDidUpdate 和 componentWillReceiveProps

**componentDidUpdate** 和 **componentWillReceiveProps** 生命周期用法看起来比较相似:

```js
componentWillReceiveProps(nextProps) {
        if(nextProps.count !== this.props.count) {
            // doSomething
        }
    }
```

```js
componentDidUpdate(prevProps) {
        if(prevProps.count !==  this.props.count) {
            this.setState({
                count: this.props.count
            })
        }
    }
```

#### 区别

主要有两点区别

1. 在 React 生命周期调用时机
2. 更新 state 的方式

- 在 React 生命周期调用时机
- - **componentWillReceiveProps** 在组件接受新的 **props** 之前触发
- - **componentDidUpdate** 在组件接受新的 **props** 之后触发

如果想在组件收到 **新 props 之前** 做一些事情，可以使用 **componentWillReceiveProps** ，如果想在收到**新的 props 或状态后**做某事，可以使用 **componentDidUpdate**

- 更新 state 的方式

最主要的区别是

- - **componentWillReceiveProps** 更新状态是`同步`的
- - **componentDidUpdate** 更新状态是`异步`的

这点区别非常重要，也是 **componentWillReceiveProps** 生命周期被废弃的重要原因(可能导致某些问题), 所以推荐使用 **componentDidUpdate**

### React v16.4 后的 getDerivedStateFromProps

`static getDerivedStateFromProps(props, state)` 在组件`创建`时和`更新`时的 **render** 方法之前调用，它应该返回一个对象来更新状态，或者返回 null 来不更新任何内容。

**注意：**

- **getDerivedStateFromProps** 前面要加上 **static** 保留字，声明为`静态`方法，不然会被 **react** 忽略掉
  > getDerivedStateFromProps() is defined as an instance method and will be ignored.Instead, declare it as a static method.
- **getDerivedStateFromProps** 里面的 `this` 为 **undefined**

**static** **静态方法**只能 `Class` (构造函数)来调用(App.staticMethod✅)，而实例是**不能**的( (new App()).staticMethod ❌ )；<br/>
当调用 React Class 组件时，改组件会实例化；

所以<br/>
React Class 组件中，静态方法 **getDerivedStateFromProps** 无权访问 **Class 实例** 的`this`，即 this 为 undefined。

### getSnapshotBeforeUpdate

**getSnapshotBeforeUpdate()** 被调用于 **render** 之**后**，**可以读取**`但`**无法使用**DOM 的时候。它使您的组件可以在**可能更改之前**从 DOM 捕获一些信息（例如滚动位置）。此生命周期返回的**任何值**都将作为**参数**传递给 **componentDidUpdate()**。

**官网例子：**

```js
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    //我们是否要添加新的 items 到列表?
    // 捕捉滚动位置，以便我们可以稍后调整滚动.
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    //如果我们有snapshot值, 我们已经添加了 新的items.
    // 调整滚动以至于这些新的items 不会将旧items推出视图。
    // (这边的snapshot是 getSnapshotBeforeUpdate方法的返回值)
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }

  render() {
    return <div ref={this.listRef}>{/* ...contents... */}</div>;
  }
}
```
