## JS 中判断 null、undefined 与 NaN 的方法

### 1.判断 undefined:

```js
var tmp = undefined;
if (typeof tmp == "undefined") {
  console.log("undefined");
}
```

说明：`typeof` 返回的是字符串，有六种可能：**"number"、"string"、"boolean"、"object"、"function"、"undefined"**

### 2.判断 null:

```js
var tmp = null;
if (!tmp && typeof tmp != "undefined" && tmp != 0) {
  console.log("null");
}
```

### 3.判断 NaN:

```js
var tmp = 0 / 0;
if (isNaN(tmp)) {
  console.log("NaN");
}
```

说明：如果把 `NaN` 与任何值（包括其自身）相比得到的结果均是 **false**，所以要判断某个值是否是 `NaN`，`不能`**使用** == 或 === 运算符。

提示：`isNaN()` 函数通常用于检测 `parseFloat()` 和 `parseInt()` 的结果，以判断它们表示的是否是合法的数字。当然也可以用 `isNaN()` 函数来检测算数错误，比如用 **0** 作**除数**的情况。

### 4.判断 undefined 和 null 的关系:

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

### 5.判断 undefined、null 与 NaN:

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
