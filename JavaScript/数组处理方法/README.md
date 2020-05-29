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