### 受控组件

##### 什么是受控组件？

其值由React控制的输入表单元素称为“受控组件”。

受控组件有两个特点：
1. 设置**value**值，**value**由**state**控制
2. **value**值一般在`onChange`事件中通过`setState`进行修改，通过绑定onChange实现了类似双向绑定

示例：
```js
  ...... // 省略代码
  handleChange(event) {
    this.setState({value: event.target.value});
  }
  ...... // 省略代码
  render() {
    return (
      ...... // 省略代码
      <input type="text" value={this.state.value} onChange={this.handleChange} />
      ...... // 省略代码
    );
  }
```
理解：首先 **input** 的显示的值受 `value={this.state.value}`控制，输入时通过 **onChange** 函数回调，将新接受的值设置回 **this.state.value**，从而再作用到组件的显示，形成了双向绑定的感觉。

---

### 非受控组件

##### 什么是非受控组件？

表单数据由 DOM 处理的组件称为“非受控组件”。

非受控组件有两个特点：
1. 不设置value值
2. 通过**ref**获取 dom节点 然后再取 value 值

在大多数情况下，推荐使用 受控组件 来实现表单。<br/>
在受控组件中，表单数据由 React 组件处理。如果让表单数据由 DOM 处理时，替代方案为使用非受控组件。

示例:
```js
  ...... // 省略代码
  render() {
    return (
      ...... // 省略代码
      <input type="file" />
      ...... // 省略代码
    );
  }
```
在React中，`<input type="file" />` 始终是一个不受控制的组件，因为它的值只能由用户设置，而不是以编程方式设置。