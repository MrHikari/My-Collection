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

### 4. React 中 keys 的作用是什么？
**回答：**<br/>
**keys** 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。
```js
render () {
  return (
    <ul>
      {this.state.todoItems.map(({item, index}) => {
        return <li key={index}>{item}</li>
      })}
    </ul>
  )
}
```
在开发过程中，需要保证某个元素的 key 在其同级元素中具有唯一性。在 `React Diff` 算法中 React 会借助元素的 Key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染。此外，React 还需要借助 Key 值来判断元素与本地状态的关联关系，**因此绝不可忽视转换函数中 Key 的重要性**。

---

### 5. shouldComponentUpdate 是做什么的，（react 性能优化是哪个周期函数？）
**回答：**<br/>
`shouldComponentUpdate` 这个方法用来判断是否需要调用 **render 方法**重新描绘 `dom`。因为 `dom` 的描绘非常消耗性能，如果我们能在 `shouldComponentUpdate` 方法中能够写出更优化的 `dom diff` 算法，可以极大的提高性能。

---

### 6. 为什么虚拟 dom 会提高性能?
**回答：**<br/>
`虚拟 dom` 相当于在 **js** 和 **真实 dom** 中间加了`一个缓存`，利用 `dom diff` 算法避免了**没有必要的 dom 操作**，从而提高性能。

用 JavaScript 对象结构表示 DOM 树的结构；<br/>
然后用这个树构建一个真正的 DOM 树，插入页面当中。
当状态变更的时候，重新构造一棵新的对象树（虚拟DOM）。
然后用新的树（虚拟DOM）和旧的树（虚拟DOM）进行比较（DOM DIFF），记录两棵树差异，
把（对比之后）所记录的差异应用到（步骤 1，最开始的）所构建的真正的 DOM 树上，视图就更新了。

---

### 7. React中的事件是什么？
**回答：**<br/>
在 React 中，事件是对`鼠标悬停`、`鼠标单击`、`按键`**等**特定操作的触发反应。处理这些事件类似于处理 DOM 元素中的事件。但是有一些语法差异，如：

* 用驼峰命名法对事件命名而不是仅使用小写字母。
* 事件作为函数而不是字符串传递。

事件参数重包含一组特定于事件的属性。每个事件类型都包含自己的属性和行为，只能通过其事件处理程序访问。