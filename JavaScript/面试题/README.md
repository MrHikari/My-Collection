### 1. 去除字符串中最后一个**指定**的字符
**回答：**
```js
function delLast(str,target) {
  const reg = new RegExp(`${target}(?=([^${target}]*)$)`);
  return str.replace(reg, '');
}
```

---

### 2. 使用一个方法把下划线命名转成大驼峰命名
**回答：**
```js
function changeStr(str){
  if(str.split('_').length==1) return;
  str.split('_').reduce((a,b) => {
    return a+b.substr(0,1).toUpperCase() + b.substr(1);
  })
}
```

---

### 3. 一个把字符串大小写切换的方法
**回答：**
```js
function caseConvert(str){
  return str.replace(/([a-z]*)([A-Z]*)/g, (m, s1, s2) => {
	  return `${s1.toUpperCase()}${s2.toLowerCase()}`
  })
}
caseConvert('AsA33322A2aa') //aSa33322a2AA
```

**注意：**<br/>
当 **replace()** 方法的`第二个参数` **replacement** 是函数而不是字符串时，每次匹配都调用该函数，将这个函数的**返回的字符串**将作为替换文本使用。这个函数是自定义的替换规则。

当第二个参数是函数时，不仅自动此函数，还要给这个函数传最少三个参数：

1、当正则没有分组的时候，传进去的第一个实参是正则捕获到的内容，第二个参数是捕获到的内容在原字符串中的索引位置，第三个参数是原字符串（输入字符串）

2、当正则有分组的时候，第一个参数是总正则查找到的内容，后面依次是各个子正则查找到的内容。

3、传完查找到的内容之后，再把总正则查找到的内容在原字符串中的索引传进（就是arguments[0]在str中的索引位置）。最后把输入字符串（就是原字符串）传进去

**示例：**

`要求：`找到字符串中的小写字母，给它在后边加上小括号来注明它在str字符串中的位置。比如str中的第一个a，它出现在str字符串的第一个索引位置中，则a变成a(1)。下面的str最终得到的结果是Xa(1)ZZc(4)Ud(6)Fe(8)
```js
var str="XaZZcUdFe";

var reg=/[a-z]/g;//注意：全文替换必须加g

str=str.replace(reg,function(){

return arguments[0]+"("+arguments[1]+")"+"("+arguments[2]+")";

//arguments.length的值是3，在reg没有分组的情况下length属性肯定是3.

//其中arguments[0]是正则捕获查找到的内容；arguments[1]是正则查找到的内容在str这个字符串中的索引位置；arguments[2]是str字符串本身（叫输入字符串）

//这个匿名函数被自动执行四次，每一次arguments里的值分别是：

//第一次：arguments[0]是a，arguments[1]是1，arguments[2]是原字符str本身

//第二次：arguments[0]是c，arguments[1]是4，arguments[2]是原字符str本身

//第三次：arguments[0]是，arguments[1]是6，arguments[2]是原字符str本身

//第四次：arguments[0]是e，arguments[1]是8，arguments[2]是原字符str本身


})

console.log("str-------->", str); // str--------> Xa(1)(XaZZcUdFe)ZZc(4)(XaZZcUdFe)Ud(6)(XaZZcUdFe)Fe(8)(XaZZcUdFe)
```

---
