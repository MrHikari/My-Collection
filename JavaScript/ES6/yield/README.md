## yield

**yield** 关键字用来`暂停`和`继续`一个生成器函数。可以在需要的时候**控制函数的运行**。

**yield**无法单独工作，需要配合**generator(生成器)**的**其他函数**，如 `next`，懒汉式操作，展现强大的主动控制特性。

**yield** 关键字 使生成器函数**暂停执行**，并**返回**跟在它后面的表达式的当前值。与 `return` 类似，但是可以使用 `next方法` 让生成器函数**继续**执行函数 **yield** 后面内容，直到遇到 `yield 暂停` 或 `return返回` 或 `函数执行结束`。

**yield 关键字**实际返回一个 **IteratorResult（迭代器）对象** ，它有两个属性，`value` 和 `done`(返回值 `{value: xxx,done: false}`)，分别代表返回值和是否完成(`value`就是当前遍历器暂停时返回的结果，`done`为**false**的时候，表示遍历器没遍历完，为**true**表示遍历已结束。)。

示例：
```js
function foo(){
  var x = 1;
  y = yield(x+1);
  return;
}
var f = foo();
```
遍历器函数一个重要的特点就是需要 `next()` 方法才能执行，所以上面 `f = foo()` 什么都没发生，所以再加一句
```js
f.next();
```
`f.next()` 是遍历器**第一次执行**，当遍历至关键字 **yield** 时，函数**暂停**，并**返回** yield 后面的值，所以此时返回 `{value: 2, done: false}`

**再**执行一次 `f.next()`,那么遍历器函数则从**上次暂停**的 yield 处开始，直接到 `return` 语句，所以结果是`{value: undefined, done: true}`

`next()` 可以接收参数，就是可以将**传入的参数**作用于 **上一次yield**
```js
var f = function *(){
  var x = 1;
  var y = yield(x+1);
  var z = yield(x+y);
  return z;
}
var a = f.next();   // x+1 ---> 1+1 ---> 2
var b = f.next(2);  // x+y ---> 1+2 ---> 3
var c = f.next(4);  // 传入参数是4，直接代替yield(x+y)，z = 4，return 4 ---> 4
```
第一次执行暂停于 `yield(x+1)`，并返回于 **x+1** 等于 2

第二次执行，`next()`的参数 2，就代替了上面的 `yield(x+1)`，所以 **y = 2**,那么暂停于第二个`yield(x+y)`并返回 **x+y** 等于 3

同理，`next()`的参数 4，就代替了上面的 `yield(x+y)`，第三次执行 **z = 4**，`return z` 等于 4

**注意**：<br/>
遍历器函数里面的 `for...of`
```js
function foo(){
  yield 1;
  yield 2;
  yield 3;
  return;
}
for(let item of foo()){
  console.log(item);
}
// 1
// 2
// 3
```
`for...of` 循环在遍历的时候，就是把每碰到一个 **yield**,把 **yield** 后面表达式的值 赋值给 `item`

### yield 使用注意点

1. **yield** 并不能直接生产值，而是产生一个**等待输出**的函数
2. **除IE外**，其他所有浏览器均可兼容（包括 win10 的Edge）
3. 某个函数包含了 **yield**，意味着这个函数**已经是**一个 `Generator`
4. 如果 **yield** 在其他表达式中，需要用 `()` 单独括起来
5. **yield** 表达式本身没有返回值，或者说总是返回 **undefined**(由 `next` 返回)
6. `next()` 可无限调用，但既定循环完成之后总是返回 **undeinded**
