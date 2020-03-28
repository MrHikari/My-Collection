## yield

**yield** 关键字用来`暂停`和`继续`一个生成器函数。可以在需要的时候**控制函数的运行**。

**yield** 关键字 使生成器函数**暂停执行**，并**返回**跟在它后面的表达式的当前值。与 `return` 类似，但是可以使用 `next方法` 让生成器函数**继续**执行函数 **yield** 后面内容，直到遇到 `yield 暂停`或 `return返回` 或 `函数执行结束`。

**yield 关键字**实际返回一个 **IteratorResult（迭代器）对象** ，它有两个属性，`value` 和 `done`，分别代表返回值和是否完成。

**yield**无法单独工作，需要配合**generator(生成器)**的**其他函数**，如 `next`，懒汉式操作，展现强大的主动控制特性。

yield是个关键字，而且它只能用在遍历器函数里面,并且它有一个返回值{value: xxx,done: false},value就是当前遍历器暂停时返回的结果，done为false得时候，表示遍历器没遍历完，为true表示遍历已结束。
```js
function *foo(){
  var x = 1;
  y = yield(x+1);
  return;
}
var f = foo();
```
遍历器函数一个重要的特点就是需要next()方法才能执行，所以上面f = foo()什么都没发生，所以再加一句
```js
f.next();
```
f.next()是遍历器第一次执行，当遍历至关键字yield时，函数暂停，并返回yield后面的值，所以此时返回{value: 2,done: false}

再执行一次f.next(),那么遍历器函数则从上次暂停的yield处开始，直接到return语句，所以结果是{value: undefined,done: true}

next()可以接收参数，就是可以将传入的参数作用于上一次yield
```js
var f = function *(){
  var x = 1;
  var y = yield(x+1);
  var z = yield(x+y);
  return z;
}
var a = f.next();   // 2
var b = f.next(2);  // 3
var c = f.next(4);  // 4
```
第一次执行暂停于yield(x+1),并返回于x+1等于2

第二次执行，next()的参数2,就代替了上面的yield(x+1)，所以y=2,那么暂停于第二个yieldyield(x+y)并返回x+y等于3
同理,第三次执行z=4，return z等于4

最后说一下遍历器函数里面的for..of
```js
function *foo(){
  yield 1;
  yield 2;
  yield 3;
  return;
}
for(let v of foo()){
  console.log(v);
}
// 1
// 2
// 3
```
for..of循环在遍历的时候，就是把每碰到一个yield,把yield后面表达式的值赋给v
