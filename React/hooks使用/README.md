## React 的 新增功能 Hook

---

### 简介

**Hook** 是 `React 16.8` 的新增特性。它可以让你在不编写 `class` 的情况下使用 `state` 以及其他的 **React 特性**。

---

### State Hook

如下是简单的示例，可以通过普通的 `useState` 来改变类似原来 class 写法中的 **state**。

```js
import React, { useState } from 'react';

function Example() {
  // 声明一个新的叫做 “count” 的 state 变量 int
  const [count, setCount] = useState(0);
  // 声明一个新的叫做 “value” 的 state 变量 string
  const [value, setValue] = useState('default');

  return (
    <div>
      <p>Now num is {count} ！</p>
      <button onClick={() => setCount(count + 1)}>
        Click me add count
      </button>
      <p>You set this {value} ！</p>
      <button onClick={() => useState('newValue')}>
        Change value
      </button>
    </div>
  );
}
```

在这种函数组件的写法中，`useState` 就是一个 **Hook** 。通过在函数组件里调用它来给组件添加一些内部 `state`。**React** 会在重复渲染时保留这个 `state`。`useState` 会返回一对值：*当前状态*和*一个更新它的函数*，你可以在事件处理函数中或其他一些地方调用这个函数。它类似 **class 组件**的 `this.setState`，但是它不会把**新的** `state` 和**旧的** `state` 进行合并。

`useState` 唯一的参数就是**初始** `state`。在上面的例子中，计数器是从零开始的，所以**初始** `state` 就是 *0*。值得注意的是，不同于 `this.state`，这里的 `state` 不一定要是一个对象 —— 如果你有需要，它也可以是。这个初始 `state` 参数只有在**第一次渲染时**会被用到。

可以在一个组件中*多次*使用 **State Hook**。

---

### Effect Hook

在 **React** 组件中执行过 *数据获取* 或者对 *DOM进行操作*。一般将这些操作称为“**副作用**”。

**useEffect** 就是一个 `Effect Hook`，给函数组件增加了操作副作用的能力。它跟 `class` 组件中的 `生命周期` 例如：`componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount` 具有相同的用途，只不过被合并成了一个 **API**。

```js
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // 相当于 componentDidMount 和 componentDidUpdate:
  useEffect(() => {
    // 使用浏览器的 API 更新页面标题
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

当调用 `useEffect` 时，就是在 **React** 在完成对 **DOM** 的更改后运行相应的“*副作用*”函数。由于**副作用函数**是在组件内声明的，所以它们可以访问到组件的 `props` 和 `state`。默认情况下，**React** 会在*每次渲染后*`调用副作用函数` —— 包括**第一次渲染**的时候。

**注意**：`副作用函数`还可以通过**返回一个函数**来指定如何“*清除*”副作用。

跟 useState 一样，可以在组件中多次使用 useEffect ：

通过使用 Hook，你可以把组件内相关的副作用组织在一起（例如创建订阅及取消订阅），而不要把它们拆分到不同的生命周期函数里。
