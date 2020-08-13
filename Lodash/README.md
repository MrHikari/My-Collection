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

