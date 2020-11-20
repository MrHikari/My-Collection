### constructor

`constructor()`是**ES6**写法所特有的, 代替了**ES5**的 `getDefaultProps(){ }` , `getInitialState(){ }`

1. 到底要不要添加?
答: 如果需要设置**默认的状态**就要添加。

2. `super()` 要不要传入 `props` ?
答: `constructor()` **必须**配上 `super()`, 如果要在 `constructor` **内部**使用 `this.props` 就要 传入`props` , 否则不需要传入。

3. **绑定事件**到底要不要在构造函数`constructor()`中进行？
答: *js* 的 **bind** 每次都会返回一个新的函数, **为了性能等考虑**, 要在`constructor`中绑定事件。

4. 什么情况下在 `constructor()`中初始化事件 和 初始化状态?
比如: *input* 需要一个默认*value*的时候, 就要 **初始化状态**了。

---

在es5中，如果使用constructor这个函数，实际上是子类的实例（this），父类添加到方法中parent.apply(this)，在es6中，使用继承机制，先创建父类的实例this,再用子类去修改this（super()）.

如果子类没有定义，constructor方法就会被默认添加。也就是说，不管有没有显示定义，任何子类都有一个constructor（）方法。

在es6中的继承规则得知： 不管子类写不写constructor,在new出来的对象里都会默认添加constructor.constructor不可缺少，有些子类的情况可以省去。

语法具体查看：http://es6.ruanyifeng.com/#docs/class-extends

super方法使用：子类在constructor方法中调用super,否则在新建实例时会报错。因为子类自己的this对象，必须通过父类的构造函数完成后调用，得到与父类相同的属性和方法。再进行操作，子类这时也会有自己的属性和方法。如果不调用super,子类就得不到this对象。

---

<br/>

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

---

<br/>

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

---

<br/>

### React在16.4版本中针对 getDerivedStateFromProps 的调整

在旧版本中，**getDerivedStateFromProps** 只会在 `props` 更新是执行而并且不会因组件 `setState` 而触发。*FaceBook* 指出这是最初实施过程中的疏忽，现在已经得到纠正。

所以，在16.4版本中，组件执行 `setState` 时也会触发 **getDerivedStateFromProps** ，每次渲染组件时都会调用 getDerivedStateFromProps。

**注意**： **getDerivedStateFromProps** 中条件的判断和 **return** 的协调。（待验证）

---

<br/>

### 关于 setState 与 触发渲染

在 **React 16** 中为了防止`不必要`的 DOM 更新，允许你决定是否让 `.setState` 更来新状态。在调用 `.setState` 时返回 **null** 将`不再触发更新`。

防止不必要的重新渲染需要遵循的步骤：
1. 检查新的状态值是否与现有值**相同**
2. 如果值相同，将返回 **null**
3. 返回 **null** 将`不会更新状态`和`触发组件重新渲染`

---

<br/>

### React 子父组件生命周期函数执行顺序

父组件在内存中生成页面树的时候先去制作子组件,等到子组件挂在到父组件某个节点时在继续内存渲染父组件直到父组件didmount.

react 方法的作用就是将虚拟DOM渲染成真实DOM

所有的组件和元素都render完成 ===> 执行diff算法 ===> 修改有差异的dom。

虽然执行了很多无用的render，但都是在js里执行的，和真正的dom操作之间还有一层diff算法，他帮助我们节省了大部分dom操作的性能。


**父级组件**内生命周期：
```js
......
    constructor(props){
        super(props);
        console.log("App constructor")
    }
    
    componentWillMount(){
        console.log("App componentWillMount")
    }
    
    componentDidMount(){
        console.log("App componentDidMount")
    }
    render(){
        console.log("App render")
        return (
          <div>App父级组件
            <A></A>
            <B></B>
          </div>);
    }
......
```

**A组件**内生命周期：
```js
......
    constructor(props){
        super(props);
        console.log("A constructor")
    }
    
    componentWillMount(){
        console.log("A componentWillMount")
    }
    
    componentDidMount(){
        console.log("A componentDidMount")
    }
    render(){
        console.log("A render")
        return <div>A组件</div>
    }
......
```

**B组件**内生命周期：
```js
......
    constructor(props){
        super(props);
        console.log("B constructor")
    }
    
    componentWillMount(){
        console.log("B componentWillMount")
    }
    
    componentDidMount(){
        console.log("B componentDidMount")
    }
    render(){
        console.log("B render")
        return <div>B组件</div>
    }
......
```

console打印结果：
> App constructor
> App componentWillMount
> App render
> A constructor
> A componentWillMount
> A render
> B constructor
> B componentWillMount
> B render
> A componentDidMount
> B componentDidMount
> App componentDidMount