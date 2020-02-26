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

**new 的作用**

1. 在内存中创建一个新的空对象
2. 让 `this` 指向这个新的对象
3. 执行构造函数内部的代码，为新对象添加属性和方法
4. 返回这个新对象（构造函数内部不需要 return）

**静态成员 实例成员**

JavaScript的构造函数中可以添加一些成员，可以在构造函数本身上添加，也可以在构造函数内部的 `this` 上添加。

* **静态成员**：在构造函数本身上添加的成员，称为 静态成员。只能由构造函数本身来访问。
* **实例成员**：在构造函数内部创建的对象成员，称为 实例成员。只能由实例化的对象来访问。

示例：
```js
function Example(param1, param2) {
  this.param1 = param1;
  this.param2 = param2;
  this.method1 = function() {
    console.log('方法1')；
  }
}
// 实例成员只能通过实例化的对象来访问，例如 param1， param2， method1
const example1 = new Example('xxx', 1);

// 静态成员 在构造函数本身上添加的成员
Example.newParam = '静态成员'

example1.method1();
```
