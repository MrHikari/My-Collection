**ES6**引入了一个新的数据结构类型:`Set`。而**Set**与**Array**的结构是很类似的,且**Set**和**Array**可以相互进行转换。

特点：只有属性值，成员值唯一（不重复）。

Array数组：

使用构造函数创建数组

```js
var arr = new Array();   //创建一个空数组
var arr1 = new Array(10) //创建一个length为10的数组
var arr2 = new Array("red","blue","yellow") //创建了一个包含3个字符串的数组
arr.push(6)  //向arr数组中添加一个数字6，该方法返回数组长度。
console.log(arr.length)  //返回数组长度
```

创建set实例：

创建set的实例同样需要使用Set构造函数，并且传入的参数必须只能是可迭代对象（带Symbol(Symbol.iterator)属性的对象）例如数组，字符串，在ES6...运算符中提及过,返回给你一个去重后的集合。

```js
let oS = new Set();                       //创建一个空set
let oS1 = new Set(['a',1,8,6,1,true]);   //Set(5) {"a", 1, 8, 6, true}
let oS2 = new Set('abcdeab');           //Set(5) {"a", "b", "c", "d", "e"}
```

在Es5中实现去重需要我们手写一个方法。

```js
var o = {name:'zwq'}
var arr = [1,2,3,[1,2],[2,3],4,[1,2],5,o,1,2,o,9,{name:'hehe'},3,{name:'haha'},6];
var obj = {};
var newarr = [];  //去重后的数组。
for(var i = 0; i < arr.length; i++){
    if(!obj[arr[i]]){
        newarr.push(arr[i]);
        obj[arr[i]] = true;
    }
}
consoel.log(newarr);
``` 

利用对象属性不重复我们可以实现数组的去重。但是我们发现手写的数组去重是不能实现对象去重的。

```js
obj[{name:'z'}] = 5
console.log(obj)
{[object Object]: 5}
var o = {name:'zwq',age:18}
obj.ooo = 100
obj
{2,3: 5, [object Object]: 5, ooo: 100}
``` 

当以对象直接作为对象的属性时，由于对象的属性时字符串，他会自己调用toString（）方法。而对象{}[toString()]返回的是这个["[object Undefined]"]，不过是可以处理数组的，因为数组[2,3][toString()]返回的是"2,3",所以是可以去重的。

使用set去重就可以实现对象的去重。并且直接调用就可以返回去重后的数据集合。但是set无数值名，当使用delete方法时，对象要用变量保存起来，否则无法删除。

```js
var oS = new Set(arr);
console.log([...oS]);
```

set的方法

添加

```js
let oS = new Set([1]);
oS.add(2);
oS.add([1,2]);
oS.add(true);

console.log(oS);   //Set(4) {1, 2, Array(2), true}
```

删除

```js
oS.delete(2);
console.log(oS);    //Set(3) {1, Array(2), true}
//删除数组，数组要提前用变量保存起来，否则直接oS.delete([1,2])是删不掉的，因为每一个数组都是不一样的。
```

清空整个set集合

```js
oS.clear();
console.log(oS);  //Set(0) {}
```

检测set集合是否存在某个值

```js
oS.has(true)   //true  
//同样，在检测数组对象是都要用变量保存起来，要不然是找不到他的引用的。
```

由于set集合没有属性名，只有属性值，因此如果我们行操作某个值可以遍set集合。

遍历set集合。

```js
oS.forEach(val=>{
    console.log(val);
})
```

也可以使用ES6新增的for of 遍历。但这个遍历必须是一个可迭代对象，也就是说这个for of可以遍历set map 数组 字符串，但是不可以遍历对象，因为对象不是可迭代对象。

 

```js
for(let prop of oS){
    console.log(prop);  //1 [1,2] true
}
for(let prop of [1,2]){
    console.log(prop);   // 1 2
}
for(let prop of "dsfdf"){
    console.log(prop);  // d s f d f
}  
```

set集合可以转换成数组，方法一使用ES6给Array扩展到的方法form  

Array.form()参数同样必须是可迭代对象。返回一个数组。

```js
let arr = [1,2,4,1,3];
let Ab = new Set(arr);
console.log(Array.from (Ab));     //[1,2,4,3]
console.log(Array.from('1235'));  //["1","2","3","5"]
```

方法二：使用...运算符，因为...运算符可以展开可迭代对象

```js
console.log([...Ab]);     //[1,2,4,3];
```

利用set使用数组的并集，交集，差集

并集

```js
let arr1 = [1, 2, 3, 2, 3];
let arr2 = [3, 2, 4, 4, 5];
// 并集
let newArr = [...arr1,...arr2];
let oS = new Set(newArr);
console.log(oS);   //Set(5) {1, 2, 3, 4, 5}
``` 

交集

```js
let set1 = new Set(arr1);
let set2 = new Set(arr2)
let newArr = [...set1].filter(ele => set2.has(ele))   //使用数组的过滤方法filter
console.log(newArr);     [2,3]
```

差集

```js
let set1 = new Set(arr1);
let set2 = new Set(arr2)
let newArr1 = [...set1].filter(ele => {!set2.has(ele))
let newArr2 = [...set2].filter(ele => {!set1.has(ele))
let newArr = [...newArr1,...newArr2];
console.log(newArr);    //[1,4,5]
```