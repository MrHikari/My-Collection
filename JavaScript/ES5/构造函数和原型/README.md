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

---

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

<br/>

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
    console.log('方法1');
  }
}
// 实例成员只能通过实例化的对象来访问，例如 param1， param2， method1
const example1 = new Example('xxx', 1);

// 静态成员 在构造函数本身上添加的成员
Example.newParam = '静态成员';

example1.method1();
```

<br/>


**构造函数的问题**

构造函数方法虽然好用，但是**存在浪费内存的问题**。每次实例化一个新的对象，便在内存中开辟了新的内存空间。

<br/>

**构造函数原型 prototype**

构造函数通过原型分配的函数是所有对象**共享的**。

JavaScript 规定，每一个构造函数都有一个 `prototype` 属性，指向另一个对象。`prototype`是一个对象，这个对象的所有属性和方法，都归构造函数所拥有。**可以将不会改变的方法，直接定义在 portotype 对象上，这样所有实例化的对象，便可以共享这些方法**。

示例：
```js
function Example(param1, param2) {
  this.param1 = param1;
  this.param2 = param2;
  this.method1 = function() {
    console.log('方法1');
  }
  Example.prototype.method2 = function() {
    console.log('公共属性和方法');
  }
}
// 实例成员只能通过实例化的对象来访问，例如 param1， param2， method1
const example1 = new Example('xxx', 1);

// 静态成员 在构造函数本身上添加的成员
Example.newParam = '静态成员';

example1.method1();
```

<br/>

**对象原型 __proto__**

对象都会有一个属性 `__proto__` 指向构造函数的 `prototype` 原型对象，之所以对象可以使用构造函数 `prototype` 原型对象的属性和方法，是因为对象有 `__proto__` 原型的存在。

* `__proto__`对象原型和原型对象 `prototype` 是等价的
* `__proto__`对象原型的意义就在于为对象的查找机制提供一个方向，或者说是一条路线，但是它是一个非标准属性，在实际开发中，不可以使用这个属性，`__proto__`内部指向原型对象 `prototype`

示例：
```js
function Example(param1, param2) {
  this.param1 = param1;
  this.param2 = param2;
  this.method1 = function() {
    console.log('方法1');
  }
  Example.prototype.method2 = function() {
    console.log('公共属性和方法');
  }
}
// 实例成员只能通过实例化的对象来访问，例如 param1， param2， method1
const example1 = new Example('xxx', 1);

// 静态成员 在构造函数本身上添加的成员
Example.newParam = '静态成员';

example1.method1();

console.log(example1.__proto === Example.prototype); // 结果是true
```

<br/>

**constructor 构造函数**

`对象原型`（ __proto__ ）和`构造函数`（ prototype ）`原型对象`内部都有一个属性 `constructor` 属性，constructor 被称为构造函数，因为它重新指向回了构造函数本身。

**constructor** 主要用于 当前对象 引用于哪个**构造函数**，可以让原型对象重新指向原来的构造函数。

<br/>

**构造函数、实例和原型对象的三者关系**

![三者关系示意图]()

