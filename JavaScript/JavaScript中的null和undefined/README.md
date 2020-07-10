### 概述

&emsp;&emsp;`null`和**undefined**属于js中两种不同的基本数据类型，都可以表示“没有”，含义非常相似。将一个变量赋值为undefined或null，老实说，语法效果几乎没区别。并且在if语句的判断条件中，它们都会自动转为false，相等运算符（`==`）甚至直接报告两者相等。

```js
var a = null;
var b = undefined;
if (!a) {
  console.log('a is false');
} //a is false
if (!b) {
  console.log('b is false');
} //b is false
if (null == undefined) {
  console.log('null == undefined is true')
} //null == undefined is true
```

&emsp;&emsp;`null`是一个表示“空”的对象，转为**数值**时为**0**；**undefined**是一个表示"此处无定义"的原始值，转为数值时为`NaN`。

```js
Number(null); // 0
null + 9; // 9
Number(undefined); // NaN
undefined + 9; // NaN
```

### 用法和含义

&emsp;&emsp;对于`null`和**undefined**，大致可以入下理解。null表示空值，即该处的值现在为空。调用函数时，某个**参数未设置任何值**，这时就可以传入null，表示该参数为空。比如，某个函数接受引擎抛出的错误作为参数，如果运行过程中未出错，那么这个参数就会传入null，表示未发生错误。undefined表示“**未定义**”，下面是返回undefined的典型场景。

1. 变量声明且没有赋值；

2. 获取对象中不存在的属性时；

3. 函数需要实参，但是调用时没有传值，形参是undefined；

4. 函数调用没有返回值或者return后没有数据，接收函数返回的变量是undefined。

```js
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于 undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();

o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
```

#### 获取数据中的数字数据，包括 0

```js
typeof(value) === 'number' && !Number.isNaN(value)
```