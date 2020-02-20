```js
for (var i = 0, type;type = ['String', 'Array', 'Number'][i++]) {
    // 代码块
}
```
比较疑惑，因为从平时接触的来看基本上都是

``js
for(语句1，语句2，语句3) {

}
// 语句1：起始
// 语句2：循环终止条件
// 语句3：在循环后被执行的语句
```
现在的疑惑如下

```js
for(var i =10,i--;) {

}
```

实际上上面的语句等同于

```js
for(var i =0, i<10 i++;) {

}
```
这是为什么?

原来这里等同于把循环终止条件和循环被执行后执行的语句相结合了即把判断和赋值放到一起了，一边循环一边赋值，

i--是什么判断条件，当i--为fasle即，循环终止，在js中0, null, undefined, false, ‘’，

根据Boolean的隐形转化，其结果为false，即i=0时条件终止

再回到我们之前的问题

```js
for (var i = 0, type; type = ['String', 'Array', 'Number'][i++]) {
    // 代码块
}
```
 

```js
var i =0, type; //语句1
type = ['String', 'Array', 'Number'][i++] // 语句2
```
即这里的判断+赋值调件为type = ['String', 'Array', 'Number'][i++]，终止条件为type=‘undefined’