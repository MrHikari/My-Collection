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