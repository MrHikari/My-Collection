### JS 中判断 null、undefined 与 NaN 的方法

#### 1.判断 undefined:

```js
var tmp = undefined;
if (typeof tmp == "undefined") {
  console.log("undefined");
}
```

说明：`typeof` 返回的是字符串，有六种可能：**"number"、"string"、"boolean"、"object"、"function"、"undefined"**

#### 2.判断 null:

```js
var tmp = null;
if (!tmp && typeof tmp != "undefined" && tmp != 0) {
  console.log("null");
}
```

#### 3.判断 NaN:

```js
var tmp = 0 / 0;
if (isNaN(tmp)) {
  console.log("NaN");
}
```

说明：如果把 `NaN` 与任何值（包括其自身）相比得到的结果均是 **false**，所以要判断某个值是否是 `NaN`，`不能`**使用** == 或 === 运算符。

提示：`isNaN()` 函数通常用于检测 `parseFloat()` 和 `parseInt()` 的结果，以判断它们表示的是否是合法的数字。当然也可以用 `isNaN()` 函数来检测算数错误，比如用 **0** 作**除数**的情况。

#### 4.判断 undefined 和 null 的关系:

```js
var tmp = undefined;
if (tmp == undefined) {
  console.log("undefined == undefined");
}
```

```js
var tmp = undefined;
if (tmp == null) {
  console.log("null == undefined");
}
```

结论：`null == undefined`

#### 5.判断 undefined、null 与 NaN:

```js
var tmp = null;
var tmp1 = undefined;
var tmp2 = NaN;
if (!tmp) {
  console.log("null or undefined or NaN");
}
if (!tmp1) {
  console.log("null or undefined or NaN");
}
if (!tmp2) {
  console.log("null or undefined or NaN");
}
```

结果：console 都打印。

<br/>

---

<br/>

### 13 个 JS 数组精简技巧

_来自 https://juejin.im/post/5db62f1bf265da4d560906ab 如有侵权，联系删除。_

数组是 JS 最常见的一种数据结构，咱们在开发中也经常用到,在这篇文章中,提供一些小技巧,帮助咱们提高开发效率。

#### 1. 删除数组的重复项

![数组精简图示1](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa852a1d1bf6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 2. 替换数组中的特定值

有时在创建代码时需要替换数组中的特定值，有一种很好的简短方法可以做到这一点，咱们可以使用.splice(start、value to remove、valueToAdd)，这些参数指定咱们希望从哪里开始修改、修改多少个值和替换新值。

![数组精简图示2](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa870084b618?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 3. Array.from 达到 .map 的效果

咱们都知道 .map() 方法，.from() 方法也可以用来获得类似的效果且代码也很简洁。

![数组精简图示3](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa883dfc8695?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 4. 置空数组

有时候我们需要清空数组，一个快捷的方法就是直接让数组的 length 属性为 0，就可以清空数组了。

![数组精简图示4](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa89a5b762dd?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 5. 将数组转换为对象

有时候，出于某种目的，需要将数组转化成对象，一个简单快速的方法是就使用展开运算符号(...):

![数组精简图示5](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa8ac3dc7095?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 6. 用数据填充数组

在某些情况下，当咱们创建一个数组并希望用一些数据来填充它，这时 .fill()方法可以帮助咱们。

![数组精简图示6](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa8c58885093?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 7. 数组合并

使用展开操作符，也可以将多个数组合并起来。

![数组精简图示7](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa8d766b8ec9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 8. 求两个数组的交集

求两个数组的交集在面试中也是有一定难度的正点，为了找到两个数组的交集，首先使用上面的方法确保所检查数组中的值不重复，接着使用.filter 方法和.includes 方法。如下所示：

![数组精简图示8](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa8ec0988182?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 9. 从数组中删除虚值

在 JS 中，虚值有 false, 0，''， null, NaN, undefined。咱们可以 .filter() 方法来过滤这些虚值。

![数组精简图示9](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa8fdc8fe989?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 10. 从数组中获取随机值

有时我们需要从数组中随机选择一个值。一种方便的方法是可以根据数组长度获得一个随机索引，如下所示：

![数组精简图示10](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa9146b84abb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 11. 反转数组

现在，咱们需要反转数组时，没有必要通过复杂的循环和函数来创建它，数组的 reverse 方法就可以做了：

![数组精简图示2](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa925550abc1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 12. lastIndexOf() 方法

![数组精简图示12](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa93994c0055?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 13. 对数组中的所有值求和

JS 面试中也经常用 reduce 方法来巧妙的解决问题

![数组精简图示13](https://user-gold-cdn.xitu.io/2019/10/28/16e0fa94e42554ee?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
