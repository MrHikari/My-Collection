### 关于类组件事件绑定的 this 问题

在类组件中，绑定事件（**onClick**、**onKeyPress**等）的方式有以下几种，其中**比较保险和推荐的**还是使用`箭头函数` `()=> { }` 的方式。如果你觉的箭头函数的写法太长，可以看一下下面这几种改进的方式：

```html
<button onClick = { () => this.addN() } >n+1</button> <!--传一个函数给onClick即可，注意C大写-->
 
<button onClick = { this.addN } >n+1</button><!--这种写法会有问题! 使得this.addN里的this变成window-->
 
<button onClick = { this.addN.bind(this) }>n+1</button> <!--对上述问题，可通过 bind 绑定 this，还不如第一种方式-->
 
<!--但第一种写法代码太长，可在类组件的constructor 中用 this._addN = () => this.addN() 给箭头函数取个名字，然后写成:-->
<button onClick = { this._addN } >n+1</button>
 
<!--上面这种方法还要写一个 _addN，不如直接将 addN() 方法写到constructor 中，解决了上述的所有问题：-->
constructor(){
    this.addN = () => this.setstate({ n: this.state.n+1 })
}
render(){
    return <button onClick = { this.addN }>n+1</button>
}
 
<!--但上面这种方式需将 this.addN 写到 constructor 里，于是 React 组委会推荐下述的语法糖，它和上述的代码实际上没有区别：-->
class Son extends React.Component{
    addN = () => this. setState({ n: this.state.n+1 });
    render(){
        return <button onClick = { this. addN }>n+1</button>
    }
}
```

**注意**：在类组件中，绑定事件不能在 `onClick = { }` 花括号内通过 `this.addN` 的方式直接调用类组件中的方法，这会使得 **React** 在执行事件时将 `this` 关键字指向 `window`。

具体原因是，比如 `<button onClick = { this.addN } >n+1</button>` 代码，当用户点击按钮时，**React** 实际上会去调用 `button.onClick.call(null，event)` 函数，在执行的过程中浏览器会将值为 **null** 的 **this** 指向 `window`，所以会出现上述问题。