* 转自 https://www.cnblogs.com/yubingyang/p/11576515.html

> 从**引用**聊到**深浅拷贝**，从**深拷贝**过渡到ES6新数据结构**Map**及**Set**，再到另一个**map**即**Array.map()**和与其类似的**Array.flatMap**()，中间会有其他相关话题，例如**Object.freeze**()与**Object.assign**()等等。

### 前言

> 一边复习一边学习，分清引用与深浅拷贝的区别，并实现浅拷贝与深拷贝，之后通过对深拷贝的了解，拓展到ES6新数据结构Map及Set的介绍，再引入对另一个数组的map方法的使用与类似数组遍历方法的使用。通过一条隐式链将一长串知识点串联介绍，可能会有点杂，但也会有对各知识点不同之处有明显区分，达到更好的记忆与理解。

### 引用、浅拷贝及深拷贝

* #### 引用

通常在介绍深拷贝之前，作为引子我们会看见类似以下例子：

```js
var testObj = {   name: 'currName' };
var secObj = testObj secObj.name = 'changedName';
console.log(testObj); // { name: 'changedName' }
```

这其实就是一种引用，对于复杂数据结构，为了节省存储资源，符号 “=” 其实并不是将值赋给新建的变量，而是做了一个地址引用，使其指向原来存储在堆中的数据的地址，此时**testObj**与**secObj**都指向同一个地址，因此在修改**secObj**的数据内容时，即是对其指向的原有数据进行修改。

对于数组有相似的引用情况，代码如下：

```js
var testArr = [0, [1, 2]];
var secArr = testArr;
secArr[0] = 'x';
console.log(testArr); // [ 'x', [ 1, 2 ] ] 
```

* #### 浅拷贝

对于浅拷贝，其与引用的区别，我们一边实现浅拷贝，之后进行对比再解释，实现如下：

```js
function shallowCopy (obj) {
    var retObj = {};
    for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
            retObj[key] = obj[key];
        }
    }
    return retObj;
};

var testObj = { 'name': 'currName', 'nums': [1, [2, 3]], 'objs': { 'innerobj': 'content'} };
var secObj = shallowCopy(testObj);
secObj.name = 'changedName';
secObj.nums[0] = '一';
secObj.nums[1] = ['二', '三'];
console.log(testObj); // { name: 'currName', nums: ['一', ['二', '三']], objs: { innerObj: 'changedContent' } }
console.log(secObj); // { name: 'changedName', nums: ['一', ['二', '三']], objs: { innerObj: 'changedContent' } }
```

从上例可以看出经过浅拷贝后得到的对象，对于第一层数据其修改后已经不能影响之前的数据，但对于内部还存在迭代器的数据属性，还是有引用情况的存在，所以后者对这些属性的修改，依旧会影响前者中这些属性的内容

引用与浅拷贝的区别就在于： 对第一层数据是否依旧修改后互相影响。

#### 浅拷贝相关方法

* * ##### Object.assign()

**assign**方法效果类似于在数组中的**concat**拼接方法，其可以将源对象中可枚举属性进行复制到目标对象上，并返回目标对象，该方法中第一个参数便就是目标对象，其他参数为源对象。因此该方法我们定义源对象为空对象时便可以在对拷贝的实现中使用，但需要注意的是**Object.assign**()其方法自身实行的便是浅拷贝，而不是深拷贝，因此通过该方法实现的拷贝只能是浅拷贝。

实现浅拷贝代码如下：

```js
var testObj = { 'name': 'currName',   'nums': [1, [2, 3]], 'objs': { 'innerObj': 'content' } };
var secObj = Object.assign({}, testObj);
secObj.name = 'changedName';
secObj.nums[0] = '一';
secObj.nums[1] = ['二', '三'];
secObj.objs['innerObj'] = 'changedContent';
console.log(testObj); // { name: 'currName', nums: [ '一', [ '二', '三' ] ], objs: { innerObj: 'changedContent' } }
console.log(secObj); // { name: 'changedName', nums: [ '一', [ '二', '三' ] ], objs: { innerObj: 'changedContent' } } 
```

* #### 深拷贝

接上面对浅拷贝的介绍，很容易就可以想到深拷贝便是在浅拷贝的基础上，让内部存在更深层数据的对象，不止第一层不能改变原有数据，内部更深层次数据修改时也不能使原有数据改变，即消除了数据中所有存在引用的情况。通过对浅拷贝的实现，我们很容易就想到通过递归的方法对深拷贝进行实现。

以下就是通过递归实现深拷贝的过程：

* * **Version 1**： 对于深拷贝，因为存在数组与对象互相嵌套的问题，第一个版本先简单统一处理对象的深拷贝，不深究数组对象的存在。

```js
function deepCopy(content) {
    var retObj = {};
    for (const key in content) {
        if (content.hasOwnProperty(key)) {
            retObj[key] = typeof;
            content[key] === 'object' ? deepCopy(content[key]) : content[key];
        }
    }
    return retObj;
}
var testObj = { 'name': 'currName', 'nums': [1, [2, 3]], 'objs': { 'innerObj': 'content' } };
var secObj = deepCopy(testObj);
secObj.name = 'changedName';
secObj.nums[0] = '一';
secObj.nums[1] = ['二', '三'];
secObj.objs['innerObj'] = 'changedContent';
secObj.age = 18;
console.log(testObj); // { name: 'currName', nums: [ 1, [ 2, 3 ] ], objs: { innerObj: 'content' } }
console.log(secObj); // { name: 'changedName', nums: { '0': '一', '1': [ '二', '三' ] }, objs: { innerObj: 'changedContent' }, age: 18 }
```

* * **Version 2**: 完善数组与对象组合嵌套的情况

此时对于内部存在的数组来说，会被转化为对象，键为数组的下标，值为数组的值，被存储在新的对象中，因此有了我们完善的第二版。

```js
function deepCopy (obj) {
    var tempTool = Array.isArray(obj) ? [] : {};
    for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
            tempTool[key] = typeof obj[key] === 'object' ? deepCopy(obj[key]) : Array.isArray(obj) ? Array.prototype.concat(obj[key]) : obj[key];
        }
    }
    return tempTool;
}
var testObj = { 'name': 'currName', 'nums': [1, [2, 3]], 'objs': { 'innerObj': 'content' } };
var secObj = deepCopy(testObj);
secObj.name = 'changedName';
secObj.nums[0] = '一';
secObj.nums[1] = ['二', '三'];
secObj.objs['innerObj'] = 'changedContent';
secObj.age = 18;
console.log(testObj); // { name: 'currName', nums: [ 1, [ 2, 3 ] ], objs: { innerObj: 'content' } }
console.log(secObj); // { name: 'changedName', nums: [ '一', [ '二', '三' ] ], objs: { innerObj: 'changedContent' }, age: 18 } 
```

### ES6中 Map、Set

#### Map

对于Hash结构 即 键值对的集合，Object对象只能用字符串作为key值，在使用上有很大的限制，ES6提供的新的数据结构Map相对于Object对象，其“键”的范围不限于字符串类型，实现了“值-值”的对应，使用上可以有更广泛的运用。但Map在赋值时，只能接受如数组一般有lterator接口且每个成员都是双元素的数组的数据结构作为参数，该数组成员是一个个表示键值对的数组，之外就只能通过Map自身set方法添加成员。

所以以下我们先介绍将对象转为Map的方法，再对Map自身方法做一个简单介绍，本节最后介绍一个Map的运用场景

* ##### Object转为Map方法：

```js
function objToMap (object) {
    let map = new Map();
    for (const key in object) {
        if (object.hasOwnProperty(key)) {
            map.set(key, object[key]);
        }
    }
    return map;
}
var testObj = { 'name': 'currName', 'nums': [1, [2, 3]], 'objs': { 'innerObj': 'content' } };
let map = objToMap(testObj);
map.set('name', 'changedName');
console.log(testObj); // { name: 'currName', nums: [ 1, [ 2, 3 ] ], objs: { innerObj: 'content' } };
console.log(map); // Map { 'name' => 'changedName', 'nums' => [ 1, [ 2, 3 ] ], 'objs' => { innerObj: 'content' } } 
```

* ##### Map自身方法介绍

含增删改查方法：set、get、has、delete；

遍历方法：keys、values、entries、forEach；

其他方法：size、clear。

需要注意的是forEach方法还可以接受第二个参数，改变第一个参数即回调函数的内部this指向。

```js
let map = new Map([['name', 'currName'], ['nums', [1, [2, 3]]], ['objs', {'innerObj': 'content'}] ]) // 增 删 改 查
map.set('test', 'testContent');
map.delete('objs');
map.set('name', 'changedName');
console.log(map.get('nums'));// [ 1, [ 2, 3 ] ]
console.log(map.has('nums'));// true
console.log(map);// Map { 'name' => 'changedName', 'nums' => [ 1, [ 2, 3 ] ], 'test' => 'testContent' }  // 遍历方法
console.log(map.keys());// [Map Iterator] { 'name', 'nums', 'test' };
console.log(map.values());// [Map Iterator] { 'changedName', [ 1, [ 2, 3 ] ], 'testContent' } console.log(map.entries());// [Map Iterator] { [ 'name', 'changedName' ], [ 'nums', [ 1, [ 2, 3 ] ] ], [ 'test', 'testContent' ] };
const testObj = { objName: 'objName' };
map.forEach(function (value, key) {
    console.log(key, value, this.objName)
    // name changedName objName
    // nums [ 1, [ 2, 3 ] ] objName
    // test testContent objName
    }, 
    testObj
);
// 其他方法
console.log(map.size); // 3
console.log(map); // Map { 'name' => 'changedName', 'nums' => [ 1, [ 2, 3 ] ], 'test' => 'testContent' } 
map.clear();
console.log(map); // Map {}
```

* ##### Map应用场景

对于经典算法问题中 上楼梯问题：共n层楼梯，一次仅能跨1或2步，总共有多少种走法？

这一类问题都有一个递归过程中内存溢出的bug存在，此时就可以运用Map减少递归过程中重复运算的部分，解决内存溢出的问题。

```js
let n = 100;
let map = new Map();
function upStairs (n) {
    if (n === 1) return 1;
    if (n === 2) return 2;
    if (map.has(n)) return map.get(n);
    let ret = upStairs(n - 1) + upStairs(n - 2);
    map.set(n, ret);
    return ret
}
console.log(upStairs(n)); // 573147844013817200000 
```

#### WeakMap

本节介绍在ES6中，与Map相关且一同发布的WeakMap数据结构。

* ##### WeakMap与Map区别

WeakMap与Map主要有下图三个区别

|区别|Map|WeakMap|
|-|-|-|
|“键”类型：|任何类型|Object对象|
|自身方法：|基本方法：set、get、has、delete；遍历方法：keys、values、entries、forEach；其他方法：size、clear。|基本方法：set、get、has、delete。|
|键引用类型：|强引用|弱引用|

此处我们对强弱引用进行简单介绍：弱引用在回收机制上比强引用好，在“适当”的情况将会被回收，减少内存资源浪费，但由于不是强引用，WeakMap不能进行遍历与size方法取得内部值数量。

* ##### WeakMap自身方法

含增删改查方法：set、get、has、delete。

```js
let wMap = new WeakMap();
let key = {};
let obj = {name: 'objName'};
wMap.set(key, obj);
console.log(wMap.get(key)); // { name: 'objName' }
console.log(wMap.has(key)); // true
wMap.delete(key);
console.log(wMap.has(key)); // false
```

* ##### WeakMap应用场景

WeakMap因为键必须为对象，且在回收机制上的优越性，其可以用在以下两个场景：

1. 对特定DOM节点添加状态时。当DOM节点被删除，将DOM节点作为“键”的WeakMap也会自动被回收。

2. 对类或构造函数中私有属性绑定定义。当实例被删除，被作为“键”的this消失，WeakMap自动回收。

示例代码如下：
```js
let element = document.getElementById('box');
let wMap = new WeakMap();
wMap.set(element, {clickCount: 0});
element.addEventListener('click', () => {
    let countObj = wMap.get(element);
    countObj.clickCount++;
    console.log(wMap.get(element).clickCount); // click -> n+=1
});
const _age = new WeakMap();
const _fn = new WeakMap();
class Girl {
    constructor (age, fn) {
        _age.set(this, age);
        _fn.set(this, fn);
    }
    changeAge () {
        let age = _age.get(this)
        age = age >= 18 ? 18 : null;
        _age.set(this, age);
        _age.get(this) === 18 ? _fn.get(this)() : console.log('error');
    }
}
const girl = new Girl(25, () => console.log('forever 18 !'));
girl.changeAge(); // forever 18 ! 
```

#### Set

介绍完ES6新增的Map与WeakMap数据结构，我们继续介绍一同新增的Set数据结构。

Set之于Array，其实有点像Map之于Object，Set是在数组的数据结构基础上做了一些改变，新出的一种类似于数组的数据结构，Set的成员的值唯一，不存在重复的值。以下将对Set数据结构作一些简单的介绍。

* ##### Set与Array之间的相互转换

Set可以将具有Iterable接口的其他数据结构作为参数用于初始化，此处不止有数组，但仅以数组作为例子，单独讲述一下。

```js
// Set -> Array
let arr = [1, 2, 3, 3];
let set = new Set(arr);
console.log(set); // Set { 1, 2, 3 } 
// Array -> Set
const arrFromSet1 = Array.from(set);
const arrFromSet2 = [...set];
console.log(arrFromSet1); // [ 1, 2, 3 ]
console.log(arrFromSet2); // [ 1, 2, 3 ]
```

* ##### Set自身方法

Set内置的方法与Map类似

含增删查方法：add、has、delete；

遍历方法：keys、values、entries、forEach；

其他方法：size、clear。

```js
let arr = [1, 2, 3, 3];
let set = new Set(arr);
// 增删改查
set.add(4);
console.log(set); // Set { 1, 2, 3, 4 };
set.delete(3);
console.log(set); // Set { 1, 2, 4 };
console.log(set.has(4)); // true
// 遍历方法 因为在Set结构中没有键名只有健值，所以keys方法和values方法完全一致
console.log(set.keys()); // [Set Iterator] { 1, 2, 4 }
console.log(set.values()); // [Set Iterator] { 1, 2, 4 }
for (const item of set.entries()) {
    console.log(item);
    // [ 1, 1 ]
    // [ 2, 2 ]
    // [ 4, 4 ]
}
const obj = {   name: 'objName' };
set.forEach(function (key, value) {
     console.log(key, value, this.name);
     // 1 1 'objName'
     // 2 2 'objName'
     // 4 4 'objName' 
     }, obj
);
// 其他方法
console.log(set.size); // 3
set.clear();
console.log(set) // Set {} 
```

* ##### Set应用场景

因为扩展运算符...对Set作用，再通过Array遍历方法，很容易求得并集、交集及差集，也可以通过间接使用Array方法，构造新的数据赋给Set结构变量。

```js
let a = new Set([1, 2, 3]);
let b = new Set([2, 3, 4]);
// 并集
let union = new Set([...a, ...b]);
console.log(union); // Set { 1, 2, 3, 4 }
// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
console.log(intersect); // Set { 2, 3 }
// 差集
let difference = new Set([...[...a].filter(x => !b.has(x)), ...[...b].filter(x => !a.has(x))]);
console.log(difference) // Set { 1, 4 }
// 赋新值
let aDouble = new Set([...a].map(x => x * 2));
console.log(aDouble); // Set { 2, 4, 6 }
let bDouble = new Set(Array.from(b, x => x * 2));
console.log(bDouble); // Set { 4, 6, 8 } 
```

#### WeakSet

* ##### WeakSet与Set对比

WeakSet之于Set，依旧相当于WeakMap之于Map。

WeakSet与Set之间不同之处，依然是：

1. WeakSet内的值只能为对象；

2. WeakSet依旧是弱引用。

* ##### WeakSet自身方法

因为弱引用的关系，WeakSet只有简单的增删查方法：add、delete、has

```js
let obj1 = {'name': 1};
let obj2 = {'name': 2};
let wSet = new WeakSet();
wSet.add(obj1).add(obj2);
console.log(wSet.has(obj2)); // true
wSet.delete(obj2);
console.log(wSet.has(obj2)) // false
```

* ##### WeakSet应用场景

对于WeakSet的应用场景，其与WeakMap类似，因为弱引用的优良回收机制，WeakSet依旧可以存放DOM节点，避免删除这些节点后引发的内存泄漏的情况；也可以在构造函数和类中存放实例this，同样避免删除实例的时候产生的内存泄漏的情况。

```js
// 1
let wSet = new WeakSet();
wSet.add(document.getElementById('box'));
const _boy = new WeakSet();
// 2
class Boy {
    constructor () {
        _boy.add(this);
    }
    method () {
        if (!_boy.has(this)) {
            throw new TypeError('Boy.prototype.method 只能在Boy的实例上调用！');
        }
    }
} 
```

### 数组中map方法及遍历相关方法

讲完大Map，此时我们继续了解完小map，map即为Array.map()，是数组中一个遍历方法。并将map作为一个引子，我们对比多介绍几个Array中遍历相关的方法。

#### Array.map()、Array.flatMap()
Array.map() —— 可以有三个参数，item、index、arr，此时当做forEach使用；常用方法是通过第一个参数遍历修改后返回一个新数组。

Array.flatMap() —— 前置知识：Array方法中有一个ES6中新加入的数组展开嵌套的方法Array.flat()，其中可以有一个参数表示展开层数，默认只展开一层。而Array.flatMap() 为 Array.map()与Array.flat()方法的叠加。

例子如下：

```js
// flat
const testArr = [1, 2, [3, [4]]];
const flatArr = testArr.flat();
console.log(flatArr);
// [1, 2, 3, Array(1)] -> 0: 1
// 1: 2 
// 2: 3
// 3: [4]
const arr = [1, 2, 3]
// map
const mapArr = arr.map(x => x * 2);
console.log(mapArr); // [2, 4, 6]
arr.map((item, index, arr) => {   console.log(item, index, arr) // 1 0 [1, 2, 3]                                 // 2 1 [1, 2, 3]                                 // 3 2 [1, 2, 3] })  // flatMap // arr.flatMap(x => [x * 2]) === arr.map(x => x * 2) const flatMapArr = arr.flatMap(x => [x * 2]) console.log(flatMapArr) // [2, 4, 6]
```

#### Array.reduce()

Array.reduce() —— reduce方法与map最大的不同是不返回新的数组，其返回的是一个计算值，参数为回调函数与回调函数参数pre初始值，回调函数中参数为pre与next，当在默认情况时，pre为数组中第一个值，next为数组中第二个值，回调函数返回值可以滚雪球般更改pre值；而当index设置数值后，pre初始值为参数值，next从数组中第一个值一直取到数组最后一位。

例子如下：

```js
const arr = [1, 2, 3, 4, 5];
const result = arr.reduce((pre, next) => {
    console.log(pre, next); 
    // 1 2
    // 3 3
    // 6 4
    // 10 5
    return pre + next
});
console.log(result); // 15
arr.reduce((pre, next) => {
    console.log(pre, next);
    // 9 1
    // 9bala 2
    // 9balabala 3
    // 9balabalabala 4
    // 9balabalabalabala 5
    return pre += 'bala'
    }, 9
);
```

#### Array.filter()、Array.find()、Array.findIndex()

Array.filter() —— 返回值是一个数组，第一个参数为回调函数，第二个参数为回调函数中this指向。回调函数的参数有value，index及arr。满足回调函数的中过滤条件的，会被push到返回值中新的数组中。

Array.find() —— 返回值是数组内的一个值，该方法返回数组内满足条件的第一个值，第一个参数为回调函数，第二个参数为回调函数中this指向。回调函数的参数有查找到的符合条件前的value，index及arr。当查找的是数组中不可重复的值时，建议使用find方法，会比filter更优越。

Array.findIndex() —— 返回值为Number，该方法返回数组内满足条件的第一个值在数组中的index，第一个参数为回调函数，第二个参数为回调函数中this指向。回调函数中的参数与find方法类似。

例子如下：
```
const arr = [1, 2, 3, 4, 5];
const obj = {num: 3};
// filter;
const filterArr = arr.filter(function (value, index, arr) {
    console.log(index, arr);
    // 0 [1, 2, 3, 4, 5]
    // 1 [1, 2, 3, 4, 5]
    // 2 [1, 2, 3, 4, 5]
    // 3 [1, 2, 3, 4, 5]
    // 4 [1, 2, 3, 4, 5]
    return value > this.num;
    }, obj
);
console.log(filterArr); // [4, 5]
// find
const findResult = arr.find(function (value, index, arr) {
    console.log(index, arr);
    // 0 [1, 2, 3, 4, 5]
    // 1 [1, 2, 3, 4, 5]
    // 2 [1, 2, 3, 4, 5]
    // 3 [1, 2, 3, 4, 5]
    return value > this.num;
    }, obj
);
console.log(findResult); // 4
// findIndex
const findIndexResult = arr.findIndex(function (value) { return value > this.num }, obj);
console.log(findIndexResult) // 3
```

#### Array.includes()

Array.includes() —— 返回值为Boolean值，其可以简单快捷的判断数组中是否含有某个值。其第一个参数为需要查找的值，第二个参数为开始遍历的位置，遍历位置起始点默认为0。相比于indexOf、filter、find及findIndex方法，includes方法更简单快捷返回Boolean值进行判断，其二对于数组中NaN值，includes可以识别到NaN。

```js
const arr = [1, 2, 3, NaN];
console.log(arr.includes(NaN)); // true
console.log(arr.includes(2, 2)); // false
```

#### Array.every()、Array.some()

Array.every() —— 返回值为Boolean类型，类似于if判断中的 && 条件符，当数组中每个值都满足条件时返回true。其第一个参数为回调函数，第二个参数为回调函数的this指向。回调函数的参数为对比结果为true的value，index及arr，到碰到false停止。

Array.some() —— 返回值为Boolean类型，类似于if判断中的 || 条件符，当数组中存在任意一个值满足条件时返回true。其参数与every方法相同，但回调函数的参数，some方法为对比结果为false的value，index及arr，到碰到true停止。

例子如下：
```js
// every
const arr = [1, 2, 3, 4, 5];
const obj = { num: 3 };
const everyResult = arr.every(function(value, index, arr) {
    console.log(index, arr);
    // 0 [1, 2, 3, 4, 5]
    // 1 [1, 2, 3, 4, 5]
    // 2 [1, 2, 3, 4, 5]
    return value < this.num;
    }, obj
);
console.log(everyResult) // false
// some
const someResult = arr.some(function(value, index, arr) {
    console.log(index, arr);
    // 0 [1, 2, 3, 4, 5]
    // 1 [1, 2, 3, 4, 5]
    // 2 [1, 2, 3, 4, 5]
    // 3 [1, 2, 3, 4, 5]
    return value > this.num;
    }, obj
);
console.log(someResult) // true 
```
