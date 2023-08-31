## React 的 新增功能 Hook

---

### 简介

**Hook** 是 `React 16.8` 的新增特性。它可以让你在不编写 `class` 的情况下使用 `state` 以及其他的 **React 特性**。

---

### Hook 规则

**Hook** 本质就是 _JavaScript_ 函数，但是在使用它时需要遵循两条规则。<br/>
官方提供了一个 _linter_ **插件**来强制执行这些规则：

1. 只在最顶层使用 **Hook**<br/>
   **不要**在*循环*，*条件*或*嵌套*函数中调用 **Hook**， 确保总是在 **React** 函数的最顶层调用 **Hook**。为了确保 **Hook** 在每一次渲染中都按照同样的顺序被调用。这让 **React** 能够在多次的 `useState` 和 `useEffect` 调用之间保持 `hook` 状态的正确。

2. 只在 **React** 函数中调用 **Hook**<br/>
   不要在普通的 _JavaScript_ 函数中调用 **Hook**。
   使用场景：

- 在 **React** 的函数组件中调用 **Hook**
- 在**自定义** **Hook** 中调用其他 **Hook**

**ESLint 插件**<br/>
官方发布了一个名为 `eslint-plugin-react-hooks` 的 **ESLint** 插件来强制执行这两条规则。

> npm install eslint-plugin-react-hooks --save-dev

```json
// ESLint 配置
{
  "plugins": [
    // ...
    "react-hooks"
  ],
  "rules": {
    // ...
    "react-hooks/rules-of-hooks": "error", // 检查 Hook 的规则
    "react-hooks/exhaustive-deps": "warn" // 检查 effect 的依赖
  }
}
```

---

### State Hook

如下是简单的示例，可以通过普通的 `useState` 来改变类似原来 class 写法中的 **state**。

```js
import React, { useState } from "react";

function Example() {
  // 声明一个新的叫做 “count” 的 state 变量 int
  const [count, setCount] = useState(0);
  // 声明一个新的叫做 “value” 的 state 变量 string
  const [value, setValue] = useState("default");

  return (
    <div>
      <p>Now num is {count} ！</p>
      <button onClick={() => setCount(count + 1)}>Click me add count</button>
      <p>You set this {value} ！</p>
      <button onClick={() => useState("newValue")}>Change value</button>
    </div>
  );
}
```

在这种函数组件的写法中，`useState` 就是一个 **Hook** 。通过在函数组件里调用它来给组件添加一些内部 `state`。**React** 会在重复渲染时保留这个 `state`。`useState` 会返回一对值：*当前状态*和*一个更新它的函数*，你可以在事件处理函数中或其他一些地方调用这个函数。它类似 **class 组件**的 `this.setState`，但是它不会把**新的** `state` 和**旧的** `state` 进行合并。

`useState` 唯一的参数就是**初始** `state`。在上面的例子中，计数器是从零开始的，所以**初始** `state` 就是 _0_。值得注意的是，不同于 `this.state`，这里的 `state` 不一定要是一个对象 —— 如果你有需要，它也可以是。这个初始 `state` 参数只有在**第一次渲染时**会被用到。

可以在一个组件中*多次*使用 **State Hook**。

---

### Effect Hook

在 **React** 组件中执行过 _数据获取_ 或者对 _DOM 进行操作_。一般将这些操作称为“**副作用**”。

**useEffect** 就是一个 `Effect Hook`，给函数组件增加了操作副作用的能力。它跟 `class` 组件中的 `生命周期` 例如：`componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount` 具有相同的用途，只不过被合并成了一个 **API**。

```js
import React, { useState, useEffect } from "react";

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
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

当调用 `useEffect` 时，就是在 **React** 在完成对 **DOM** 的更改后运行相应的“_副作用_”函数。由于**副作用函数**是在组件内声明的，所以它们可以访问到组件的 `props` 和 `state`。默认情况下，**React** 会在*每次渲染后*`调用副作用函数` —— 包括**第一次渲染**的时候。

**注意**：`副作用函数`还可以通过**返回一个函数**来指定如何“_清除_”副作用。

跟 useState 一样，可以在组件中多次使用 useEffect ：

通过使用 Hook，你可以把组件内相关的副作用组织在一起（例如创建订阅及取消订阅），而不要把它们拆分到不同的生命周期函数里。

---

### useMemo

**基础概念**

- **useMemo** 的基本概念就是：它在每次 **重新渲染** 的时候能够**缓存**计算结果（它能帮助我们 “记录” 每次渲染之间的计算结果）。

- **React** 所做的主要事情是让我们的 **UI** 与我们的 **状态** 保持同步，而要实现它们的同步，就需要执行一个叫做 “**re-render**” (重新渲染) 的操作。

- 每一次 **重新渲染** 都是一次快照，它基于当前应用程序的状态告诉了应用程序的 **UI** 在某一特定时刻应该展示成什么样子。我们可以把它想象成一叠照片，每张照片都记录了在每个状态变量的在其特定值下展示的样子。

- 每一次 **重新渲染** 都会根据当前的状态产生一个 虚拟 DOM（本质上它是一堆 JS 对象）。

所以从本质上，`useMemo` 和 `useCallback` 都是用来帮助我们优化**重新渲染**的工具 Hook。它们通过以下两种方式实现优化的效果。

- 减少在一次渲染中需要完成的工作量。
- 减少一个组件需要重新渲染的次数。

**假设场景**：当页面有个展示数据（效果）需要经过一个大量的计算后展示，但是页面中又有其他实时的变化造成的数据展示时。

- 普通实现方式：现在代码里定义了需要的数据：`展示数据 1` 和 `展示数据 2`。`展示数据 1` 每次通过计算后展示。`展示数据 2` 每秒钟改变一次，并且在页面中渲染出来。（页面在每秒触发一次重渲，重新展示`展示数据 2`，并且连带了`展示数据 1` 进行了庞大运算，尽管数据 1 在初次计算后没有再发生过变化）

- 解决方案：使用 `useMemo`
  **useMemo** 接受两个参数：

  1. 需要执行的一些计算处理工作，包裹在一个函数中。
  2. 一个依赖数组。

  在页面中组件挂载的过程中，当这个组件第一次被渲染时，React 都会调用这个函数来执行`展示数据 1`的计算逻辑。

  然而，对于每一个后续的渲染，React 都要从以下两种情况中做出选择：

  - 再次调用 useMemo 中的计算函数，重新计算数值。
  - 重复使用上一次已经计算出来的数据。

  React 会判断传入的依赖数组，这个数组中的每个变量是否在两次渲染间 **值是否改变了** ，如果发生了改变，就重新执行计算的逻辑去获取一个新的值，否则不重新计算，直接返回上一次计算的值。

**useMemo** 本质上就像一个小的缓存，而依赖数组就是缓存的失效策略。

在上面的例子中，其实本质上是在说 “只有当`展示数据 1` 的值变化时才重新计算质数列表“。 当组件因为其他情况重新渲染，例如`展示数据 2` 的值改变了，useMemo 就会忽略这个计算函数，直接返回之前缓存的值。

这种缓存的过程通常被称为 **memoization**，这就是为什么这个钩子被称为 “**useMemo**”。

- 解决方案 another
  将场景中的两个`展示数据`抽离为了两个单独的组件 `展示数据1 组件` 和 `展示数据2 组件`，从 页面 组件抽离出来后，这两个组件各自维护自己的状态数据，即使其中一个组件重新渲染了也不会影响另外一个。

还可以使用 `React.memo` 包裹着组件保护它不受到无关状态更新的影响。只有在 **PurePrimeCalculator** 只会在收到新数据或内部状态发生变化时重新渲染。这种组件被称为 **纯组件**。本质上，我们告诉 React 这个组件在 给定相同输入的情况下总是会产生相同的输出 ，并且可以跳过没有 props 和 状态改变 的重渲染。

**注意**：任何在组件内定义的堆数据，在组件重渲的时候，因为是引用类型数据，在**props**传递时，会作为一个新的数据，触发下级组件重渲。所以，在处理时，基本要使用 `useMemo` 缓存。

---

### useCallback

**基础概念**

- **useCallback** 简单概括：**useMemo** 和 **useCallback** 是一个东西，只是将返回值从 `数组/对象` 替换为了 `函数`。

  **useCallback** 接受两个参数：

  - **fn**：想要缓存的函数。此函数可以接受任何参数并且返回任何值。React 将会在初次渲染而非调用时返回该函数。当进行下一次渲染时，如果 dependencies 相比于上一次渲染时没有改变，那么 React 将会返回相同的函数。否则，React 将返回在最新一次渲染中传入的函数，并且将其缓存以便之后使用。React 不会调用此函数，而是返回此函数。
  - **dependencies**：有关是否更新 fn 的所有响应式值的一个列表。响应式值包括 props、state，和所有在组件内部直接声明的变量和函数。

**小结**：将每个`对象/数组/函数`包装在这些 hook 是在浪费时间。在大多数情况下，这些优化的好处几乎可以忽略不计; 因为 React 内部是高度优化的，并且 重新渲染通常并不像通常认为的那样慢或昂贵！
使用这些 hook 的最佳方法是响应问题。如果注意到的 app/页面 变得有些迟钝，可以使用 React Profiler 来寻找慢速渲染。在某些情况下，可以通过重构 app/页面 来提高性能。在其他情况下，useMemo 和 useCallback 可以帮助加快速度。
