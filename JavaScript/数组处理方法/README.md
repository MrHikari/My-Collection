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



