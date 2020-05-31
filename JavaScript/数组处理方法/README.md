## JS 数组处理方法

### Array 对象属性

|属性|描述|
|-|-|
|constructor|返回对创建此对象的数组函数的引用。|
|length|设置或返回数组中元素的数目。|
|prototype|使您有能力向对象添加属性和方法。|

### 数组的原型方法

数组的方法有数组原型方法，也有从object对象继承来的方法。<br/>
这里只介绍数组的原型方法，数组原型方法主要有以下这些：

* **基础方法**
```js
join()
push() 和 pop()
shift() 和 unshift()
sort()
reverse()
concat()
slice()
splice()
```

* **新增方法**
```js
indexOf() 和 lastIndexOf() （ES5新增）
forEach() （ES5新增）
map() （ES5新增）
filter() （ES5新增）
every() （ES5新增）
some() （ES5新增）
reduce()和 reduceRight() （ES5新增）
```

### 详细介绍

#### 基础方法

* **join()**

**join()** 方法用于把数组中的所有元素放入一个字符串。元素是通过**指定的分隔符**进行分隔的。
```js
arrayObject.join(separator)
```
**separator**	可选。指定要使用的分隔符。如果省略该参数，便使用逗号（`,`）作为分隔符。

---

* **push()**

**push()** 方法可向数组的末尾添加一个或多个元素，并返回新的长度。
```js
arrayObject.push(element1,element2,....,elementN)
```
**element** 要添加到数组的元素。必须有一个参数。否则原数组没有变化，空耗性能。

---

* **pop()**

**pop()** 方法用于删除并返回数组的最后一个元素。

```js
arrayObject.pop()
```
**pop()** 方法将删除 **arrayObject** 的*最后一个元素*，把数组*长度减 1*，并且**返回**它删除的元素的值。<br/>
**注意**：如果数组已经为空，则 **pop()** 不改变数组，并返回 `undefined` 值。

---

* **shift()**

**shift()** 方法用于把数组的**第一个元素**从其中删除，并返回第一个元素的值。
```js
arrayObject.shift()
```
如果数组是空的，那么 **shift()** 方法将不进行任何操作，返回 `undefined` 值。<br/>
**注意**：该方法**不创建新数组**，而是直接修改**原有**的 **arrayObject**。

---

* **unshift()**

**unshift()** 方法可向数组的开头添加一个或更多元素，并返回新的长度。
```js
arrayObject.unshift(element1,element2,....,elementN)
```
**element**	必须有一个要添加到数组的元素。否则原数组没有变化，空耗性能。

返回 **arrayObject** 的*新长度*。<br/>
**unshift()** 方法将把它的参数插入 **arrayObject** 的头部，并将已经存在的元素*顺次地移到较高的下标处*，以便留出空间。该方法的**第一个参数**将成为数组的新元素 (**0**号下标位置)，如果还有**第二个参数**，它将成为新的元素 (**1**号下标位置)，以此顺位类推。

**注意**：**unshift()** 方法**不创建**新的创建，而是**直接修改**原有的数组。

---

