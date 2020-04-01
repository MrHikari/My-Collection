## function*

`function*` 这种声明方式(**function** 关键字后 跟一个**星号**）会定义一个生成器函数 (**generator function**)，它返回一个 **Generator** 对象。

### 描述

**生成器函数**在执行时能暂停，后面又能从暂停处继续执行。

调用一个**生成器函数**并不会马上执行它里面的语句，而是返回一个这个生成器的 **迭代器 （ iterator ）对象**。当这个迭代器的 `next()` 方法被首次（后续）调用时，其内的语句会执行到第一个（后续）出现 **yield** 的位置为止，**yield** 后紧跟迭代器要返回的值。或者如果用的是 `yield*`（多了个星号），则表示将执行权移交给另一个生成器函数（当前生成器暂停执行）。

`next()` 方法返回一个对象，这个对象包含两个属性：**value** 和 **done**，**value** 属性表示本次 **yield** 表达式的返回值，**done** 属性为`布尔类型`，表示生成器后续是否还有 **yield 语句**，即生成器函数是否已经执行完毕并返回。

调用 `next()` 方法时，如果传入了参数，那么这个参数会传给上一条执行的 **yield语句** 左边的变量，例如下面例子中的 `x` ：

```js
function *gen(){
    yield 10;
    x = yield 'foo';
    yield x;
}

var gen_obj = gen();
console.log(gen_obj.next());// 执行 yield 10，返回 10
console.log(gen_obj.next());// 执行 yield 'foo'，返回 'foo'
console.log(gen_obj.next(100));// 将 100 赋给上一条 yield 'foo' 的左值，即执行 x=100，返回 100
console.log(gen_obj.next());// 执行完毕，value 为 undefined，done 为 true
```

当在生成器函数中显式 `return` 时，会导致生成器立即变为完成状态，即调用 `next()` 方法返回的对象的 **done** 为 `true`。如果 `return` 后面跟了一个值，那么这个值会作为当前调用 `next()` 方法返回的 **value 值**。
