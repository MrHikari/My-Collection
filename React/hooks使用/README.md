## React 的 新增功能 Hook

---

### 简介

**Hook** 是 `React 16.8` 的新增特性。它可以让你在不编写 `class` 的情况下使用 `state` 以及其他的 **React 特性**。

### 简单示例

如下是简单的示例，可以通过普通的 `useState` 来改变类似原来 class 写法中的 **state**。

```js
import React, { useState } from 'react';

function Example() {
  // 声明一个新的叫做 “count” 的 state 变量
  const [count, setCount] = useState(0);
  // 声明一个新的叫做 “value” 的 state 变量
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

在这里，`useState` 就是一个 **Hook** 。通过在函数组件里调用它来给组件添加一些内部 `state`。**React** 会在重复渲染时保留这个 `state`。`useState` 会返回一对值：当前状态和一个让你更新它的函数，你可以在事件处理函数中或其他一些地方调用这个函数。它类似 **class 组件**的 `this.setState`，但是它不会把**新的** `state` 和**旧的** `state` 进行合并。

`useState` 唯一的参数就是**初始** `state`。在上面的例子中，计数器是从零开始的，所以**初始** `state` 就是 *0*。值得注意的是，不同于 `this.state`，这里的 `state` 不一定要是一个对象 —— 如果你有需要，它也可以是。这个初始 `state` 参数只有在**第一次渲染时**会被用到。

声明多个 state 变量
