### 构造函数和原型

#### 创建对象

1. 利用 `new Object()` 创建对象
```js
const obj1 = new Object();
```

2. 利用 对象字面量创建对象
```js
const obj2 = {};
```

3. 利用构造函数创建对象
```js
function Example(param1, param2) {
  this.param1 = param1;
  this.param2 = param2;
  this.method1 = function() {
    console.log('方法1')；
  }
}

const example1 = new Example('xxx', 1);
const example2 = new Example('yyy', 2);

example1.method1();
```

#### 构造函数

**构造函数**是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋予初始值，与 `new` 一块使用。

**注意：**
1. 构造函数用于创建某一类对象，其**首字母要大写**
2. 构造函数要和 `new` 一起使用