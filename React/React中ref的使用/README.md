## React ref的用法

### React的ref有3种用法：

1. 字符串 (**已废弃**)
2. 回调函数
3. `React.createRef()` （React16.3提供）

#### 字符串

**最早**的ref用法。(了解，**已弃用**)

1.dom节点上使用，通过this.refs[refName]来引用真实的dom节点
```js
<input ref="inputRef" /> //this.refs['inputRef']来访问
```
2.类组件上使用，通过this.refs[refName]来引用组件的实例
```js
<CustomInput ref="comRef" /> //this.refs['comRef']来访问
```

#### 回调函数

回调函数就是在dom节点或组件上挂载函数，函数的传入参数是 **dom节点**或 **组件实例**，达到的效果与字符串形式是一样的，都是获取其引用。

回调函数的触发时机：

1. 组件渲染后，即 **componentDidMount** 后
2. 组件卸载后，即 **componentWillMount** 后，此时，传入参数为 **null**
3. ref改变后

##### 1.dom节点上使用回调函数
```js
<input ref={(input) => { this.textInput = input; }} type="text" />
```

##### 2.类组件上使用
```js
<CustomInput ref={(input) => { this.textInput = input; }} />
```

##### 3.可用通过props跨级传递的方式来获取子孙级dom节点或组件实例

下面是在跨两级获取到孙级别的组件内部的dom节点

```js
function CustomTextInput(props) { // 接受调用 CustomTextInput 时传入的全部参数 
  return (
    <div>
        <input ref={props.inputRef} />
    </div>
  );
}
function Parent(props) { // 接受调用 Parent 时传入的全部参数
  return (
    <div>
      My input: <CustomTextInput inputRef={props.inputRef} />
    </div>
  );
}
class Grandparent extends React.Component {
  render() {
    return (
      <Parent
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

#### React.createRef()

在 **React 16.3** 版本后，使用此方法来创建 **ref**。将其赋值给一个变量，通过 **ref** 挂载在 **dom节点** 或组件上，该 **ref**的 **current属性**将能拿到 `dom节点`或`组件`的**实例**。

例如：
```js
class Child extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  componentDidMount() {
    console.log(this.myRef.current);
  }
  render() {
    return <input ref={this.myRef}/>
  }
}
```

#### React.forwardRef

同样是 **React 16.3** 版本后提供的，可以用来创建**子组件**，以传递 **ref**。

例如：
```js
// 子组件（通过forwardRef方法创建）
const Child=React.forwardRef((props,ref)=>(
  <input ref={ref} />
));

// 父组件
class Father extends React.Component{
  constructor(props){
    super(props);
    this.myRef=React.createRef();
  }
  componentDidMount(){
    console.log(this.myRef.current);
  }
  render(){
    return <Child ref={this.myRef}/>
  }
}
```
子组件通过 `React.forwardRef` 来创建，可以将 **ref** 传递到内部的节点或组件，进而实现跨层级的引用。

`forwardRef` 在高阶组件中可以获取到原始组件的实例。

例如：

```js
// 生成高阶组件
const logProps = logProps(Child);

// 调用高阶组件
class Father extends React.Component{
  constructor(props){
    super(props);
    this.myRef=React.createRef();
  }
  componentDidMount(){
    console.log(this.myRef.current);
  }
  render(){
    return <LogProps ref={this.myRef}/>
  }
}

// HOC
function logProps(Component) {
  class LogProps extends React.Component {
    componentDidUpdate(prevProps) {
      console.log('old props:', prevProps);
      console.log('new props:', this.props);
    }

    render() {
      const {forwardedRef, ...rest} = this.props;

      // Assign the custom prop "forwardedRef" as a ref
      return <Component ref={forwardedRef} {...rest} />;
    }
  }

  // Note the second param "ref" provided by React.forwardRef.
  // We can pass it along to LogProps as a regular prop, e.g. "forwardedRef"
  // And it can then be attached to the Component.
  return React.forwardRef((props, ref) => {
    return <LogProps {...props} forwardedRef={ref} />;
  });
}
```
```js
// 生成高阶组件
const logProps=logProps(Child);

// 调用高阶组件
class Father extends React.Component{
  constructor(props){
    super(props);
    this.myRef=React.createRef();
  }
  componentDidMount(){
    console.log(this.myRef.current);
  }
  render(){
    return <LogProps ref={this.myRef}/>
  }
}

// HOC
function logProps(Component) {
  class LogProps extends React.Component {
    componentDidUpdate(prevProps) {
      console.log('old props:', prevProps);
      console.log('new props:', this.props);
    }

    render() {
      const {forwardedRef, ...rest} = this.props;

      // Assign the custom prop "forwardedRef" as a ref
      return <Component ref={forwardedRef} {...rest} />;
    }
  }

  // Note the second param "ref" provided by React.forwardRef.
  // We can pass it along to LogProps as a regular prop, e.g. "forwardedRef"
  // And it can then be attached to the Component.
  return React.forwardRef((props, ref) => {
    return <LogProps {...props} forwardedRef={ref} />;
  });
}
``` 

注意：
1. ref在函数式组件上不可使用，函数式组件无实例，但是其内部的dom节点和类组件可以使用
2. 可以通过ReactDOM.findDOMNode()，入参是一个组件或dom节点，返回值的组件对应的dom根节点或dom节点本身
   通过refs获取到组件实例后，可以通过此方法来获取其对应的dom节点
3. React的render函数返回的是vDom(虚拟dom)



使用ref需要注意的地方。
通常情况我们在项目中尽量不要去使用ref，但是有些时候做动画效果难免需要用。
常常出现的一个坑就是，在你使用setState函数改变数据，页面重新渲染，你在setState函数后面操作dom，往往会出错。
为什么呢？前面我们有讲过，setState函数是一个异步函数。所以你直接在它后面操作dom，往往dom并不是渲染后的dom。
如何避免呢？setState提供了一个回调函数，我们可以在回调函数中进行数据操作。
