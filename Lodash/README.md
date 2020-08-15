## Lodash

**Lodash** 是一个`一致性`、`模块化`、`高性能`的 *JavaScript* 实用工具库。

### 对象 “Object” Methods

```js
_.assign(object, [sources])
```

* 参数
* * object (Object): 目标对象。
* * [sources] (...Object): 来源对象。
* 返回
* * (Object): 返回 object.

分配来源对象(*sources*)的可枚举属性到目标对象上。 来源对象的应用规则是从左到右，随后的下一个对象的属性会覆盖上一个对象的属性。

**注意**: 这方法会改变 **object**，参考自 `Object.assign`.

---

### 数组 “Array” Methods

```js
_.forEach(collection, [iteratee=_.identity])
```

* 参数
* * collection (Array|Object): 一个用来迭代的集合。
* * [iteratee=_.identity] (Function): 每次迭代调用的函数。
* 返回
* * (*): 返回集合 collection。

调用 iteratee 遍历 collection(集合) 中的每个元素， iteratee 调用3个参数： (value, index|key, collection)。 如果迭代函数（iteratee）显式的返回 false ，迭代会提前退出。

**注意**: 与其他"集合"方法一样，类似于数组，对象的 "length" 属性也会被遍历。想避免这种情况，可以用 _.forIn 或者 _.forOwn 代替。

---

```js
_.concat(array, [values])
```

* * 参数
* * array (Array): 被连接的数组。
* * [values] (...*): 连接的值。
* 返回值
* * (Array): 返回连接后的新数组。

创建一个新数组，将array与任何数组 或 值连接在一起。

---

```js
_.difference(array, [values])
```

* 参数
* * array (Array): 要检查的数组。
* * [values] (...Array): 排除的值。
* 返回值
* * (Array): 返回一个过滤值后的新数组。

创建一个具有唯一**array值**的*数组*，每个值不包含在其他给定的数组中。（**注**：即创建一个新数组，这个数组中的值，为第一个数字（array 参数）排除了给定数组中的值。）该方法使用 SameValueZero做相等比较。结果值的顺序是由第一个数组中的顺序确定。

**注意**: 不像 `_.pullAll`，这个方法会返回一个新数组。

---

```js
_.differenceBy(array, [values], [iteratee=_.identity])
```

* 参数
* * array (Array): 要检查的数组。
* * [values] (...Array): 排除的值。
* * [iteratee=_.identity] (Array|Function|Object|string): iteratee 调用每个元素。
* 返回值
* * (Array): 返回一个过滤值后的新数组。

这个方法类似 `_.difference` ，除了它接受一个 *iteratee* （**注**：迭代器）， 调用 array 和 values 中的每个元素以产生比较的标准。 结果值是从第一数组中选择。iteratee 会调用一个参数：(value)。（注：首先使用迭代器分别迭代array 和 values中的每个元素，返回的值作为比较值）。

**注意**: 不像 _.pullAllBy，这个方法会返回一个新数组。

---




