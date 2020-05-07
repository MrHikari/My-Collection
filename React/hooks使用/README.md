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
