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

---

### 8. 你对 React 的 refs 有什么了解？
**回答：**<br/>
**Refs** 是 **React** 中引用的简写。它是一个有助于存储对特定的 React 元素或组件的引用的属性，它将由组件渲染配置函数返回。用于对 `render()` 返回的特定元素或组件的引用。当需要进行 DOM 测量或向组件添加方法时，它们会派上用场。
```js
class ReferenceDemo extends React.Component{
  display() {
    const name = this.inputDemo.value;
    document.getElementById('displaySpan').innerHTML = name;
  }
  render() {
    return(
      <div>
        Name: <input type="text" ref={input => this.inputDemo = input} />
        <button name="Click" onClick={this.display}>Click</button>            
        <h2>Hello <span id="displaySpan"></span> !!!</h2>
      </div>
    );
  }
}
```

 ---

1. call、 apply 和 bind 的区别?
```
在JS中，这三者都是用来改变函数的this对象的指向的。

相似之处：
1、都是用来改变函数的 this 对象的指向的。
2、第一个参数都是 this 要指向的对象。
3、都可以利用后续参数传参。

区别：
call 和 apply 都是对函数的直接调用。
bind 会创建一个新函数（方法返回的仍然是一个函数），称为绑定函数。当调用这个函数的时候，绑定函数会以创建它时传入 bind() 方法的第一个参数作为this，传入 bind() 方法的 第二个 及 以后的参数 加上 绑定函数运行时本身的参数 按照顺序 作为原函数的参数来调用原函数。因此 bind() 后面还需要 () 来进行调用才可以。
```

2. 深浅克隆
```
涉及 栈和堆 以及 数据类型的概念。

基本类型 --- 名值 都储存在 栈内存 中。复制时，栈内存 会 新 开辟一个内存。

深拷贝 都是针对于较为复杂的 object 类型。
引用类型 --- 名 存栈内存中，值 存堆内存中，但是 栈内存 会提供一个 引用地址 指向 堆内存中的值。
浅拷贝，当复制引用类型的时候，实际上只是复制了指向 堆内存 的地址，即原来的变量与复制的新变量指向了同一个值。
深拷贝，是拷贝对象各个层级的属性，相当于在堆内存中开辟了一个新的空间，存放拷贝的值，并且形成新的地址，供索引。
```

3. 给数组拓展功能，去掉重复项
```js
// 这里默认数组内部的元素暂时为基本类型
const removeSame = (aData) => {
  const result = [];
  aData.forEach((item)=>{
    if (resule.indexOf(item)=== -1) {
      result.push(item)
    }
  })
  return result;
}
```

4. 什么是闭包?用途?

* 变量作用域
```
JavaScript 的特殊的变量作用域。
变量的作用域：全局变量 和 局部变量。
JavaScript 语言的特别之处就在于：函数内部 可以直接读取 全局变量，但是 在函数外部 无法读取 函数内部的局部变量。

补充：var 申明的变量 会变量提升（即变量可以提前声明但是赋值是 understand）。let，const 不会。
```

```
「函数」和「函数内部能访问到的变量」（也叫环境）的总和，就是一个闭包。
通俗：闭包是 需要函数套函数，然后 return 一个函数。
1. 函数嵌套函数，内部函数可以引用外部函数的参数和变量
2. 参数和变量不会被垃圾回收机制所收回

闭包的好处：
1. 希望变量长期驻扎在内存当中（一般函数执行完毕，变量和参数会被销毁）
2. 避免全局变量的污染
```

5. 原型是啥?原型的功能
```
一个需要共享并且通过 实例对象 调用的方法，是在 构造函数 的 原型对象 中的。（实例对象原型__proto__指向构造函数的原型，即prototype）

原型：构造函数有原型，函数有一个属性叫prototype，函数的这个原型指向一个对象，这个对象叫原型对象。这个原型对象有一个constructor属性，指向这个函数本身。
```
```
原型作用
原型作用之一：数据共享,节省内存空间
原型作用之二：为了实现继承
```

6. 如何实现继承?
* 将内部属性指向元构造函数
```js
  function Parent(username){
    this.username = username;
    this.hello = function(){
      alert(this.username);
    }
  }

  function Child(username,password){
    // 通过以下3行实现将Parent的属性和方法追加到Child中，从而实现继承
    // 第一步：this.method是作为一个临时的属性，并且指向Parent所指向的对象，
    // 第二步：执行this.method方法，即执行Parent所指向的对象函数
    // 第三步：销毁this.method属性，即此时Child就已经拥有了Parent的所有属性和方法
    this.method = Parent;
    this.method(username); // 最关键的一行，相当于执行元构造函数运算
    delete this.method;
    this.password = password;
    this.world = function(){
      alert(this.password);
    }
  }
  var parent = new Parent("zhangsan");
  var child = new Child("lisi","123456");
  parent.hello();
  child.hello();
  child.world();
```

* call()方法方式

call 方法是Function类中的方法
call 方法的第一个参数的值赋值给类(即方法)中出现的this
call 方法的第二个参数开始依次赋值给类(即方法)所接受的参数

```js
  function test(str){
    alert(this.name + " " + str);
  }
  var object = new Object();
  object.name = "zhangsan";
  test.call(object,"langsin");// 此时，第一个参数值object传递给了test类(即方法)中出现的this，而第二个参数"langsin"则赋值给了test类(即方法)的str

  function Parent(username){
    this.username = username;
    this.hello = function(){
      alert(this.username);
    }
  }
  function Child(username,password){
    Parent.call(this, username);
    this.password = password;
    this.world = function(){
      alert(this.password);
    }
  }
  var parent = new Parent("zhangsan");
  var child = new Child("lisi","123456");
  parent.hello();
  child.hello();
  child.world();
```

* apply()方法方式

apply 方法接受2个参数
A、第一个参数与call方法的第一个参数一样，即赋值给类(即方法)中出现的this
B、第二个参数为数组类型，这个数组中的每个元素依次赋值给类(即方法)所接受的参数
```js
  function Parent(username){
    this.username = username;
    this.hello = function(){
      alert(this.username);
    }
  }
  function Child(username, password){
    Parent.apply(this, new Array(username));
    this.password = password;
    this.world = function(){
      alert(this.password);
    }
  }
  var parent = new Parent("zhangsan");
  var child = new Child("lisi","123456");
  parent.hello();
  child.hello();
  child.world();
```

* 原型链方式，即子类通过prototype将所有在父类中通过prototype追加的属性和方法都追加到Child，从而实现了继承
```js
  function Person(){
  }
  Person.prototype.hello = "hello";
  Person.prototype.sayHello = function(){
    alert(this.hello);
  }
  
  function Child(){
  }
  Child.prototype = new Person();// 这行的作用是：将Parent中将所有通过prototype追加的属性和方法都追加到Child，从而实现了继承
  Child.prototype.world = "world";
  Child.prototype.sayWorld = function(){
    alert(this.world);
  }
  
  var c = new Child();
  c.sayHello();
  c.sayWorld();
```

* 混合方式

混合了call方式、原型链方式 （实际不推荐，混乱，可读性和维护成本高）
```js
  function Parent(hello){
    this.hello = hello;
  }
  Parent.prototype.sayHello = function(){
    alert(this.hello);
  }

  function Child(hello,world){
    Parent.call(this,hello);//将父类的属性继承过来
    this.world = world;//新增一些属性
  }

  Child.prototype = new Parent();//将父类的方法继承过来

  Child.prototype.sayWorld = function(){//新增一些方法
    alert(this.world);
  }

  var c = new Child("zhangsan","lisi");
  c.sayHello();
  c.sayWorld(); 
```

7. 页面上十个li添加监听，点谁谁变红

```
个人思路，如果有ul标签，我会把点击事件加在ul上，这样li的点击事件会冒泡到ul，然后在React根据监控li的key或者其他唯一性标识，修改对应的li的样式。
```

8. 用ES6的方法进行数组求和

Array 的 reduce 方法
```js
const aData = [1,2,3,4,5,6,7,8,9,10];

const result = aData.reduce((prev, cur, index, arr)=>{ return prev + cur });
// prev 初始值，或者上一次运算的返回值
// cur 当前值
// index 当前值的索引
// arr 当前值所在的数组
```

9. 不借助第三方变量，实现变量交换

* 算术运算法 (处理number)
```js
var a = 10;
var b = 12;
a = b - a; 
b = b - a; 
a = b + a; 
```

* 借助数组
```js
var a = 1, b = 2;
a = [b, b = a][0];
```

* 位运算
```js
var a = 1, b = 2;
a ^= b; // a = a ^ b = 1 ^ 2 = 3
b ^= a; // b = b ^ (a ^ b) = 2 ^ (1 ^ 2) = 1
a ^= b; 
```

10. display:.none;和visibility:hidden;
```
很多前端的同学认为 visibility: hidden 和 display: none 的区别仅仅在于 display: none 隐藏后的元素不占据任何空间，而 visibility: hidden 隐藏后的元素空间依旧保留，实际上没那么简单，visibility是一个非常有故事性的属性。

1、visibility 具有继承性，给父元素设置 visibility:hidden; 子元素也会继承这个属性。但是如果重新给子元素设置visibility: visible; 则子元素又会显示出来。这个和 display: none 有着质的区别

2、visibility: hidden 不会影响计数器的计数，如图所示，visibility: hidden 虽然让一个元素不见了，但是其计数器仍在运行。这和 display: none 完全不一样

3、CSS3的 transition 支持 visibility 属性，但是并不支持 display，由于 transition 可以延迟执行，因此可以配合visibility 使用 纯css实现hover延时显示效果。提高用户体验。
```

11. cookie、session 和 localStorage

```
共同点：都是保存在浏览器端、且同源的
区别：
1、cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下 
2、存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie、所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大 
3、数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭之前有效；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie：只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭 
4、作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localstorage在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的 
5、web Storage支持事件通知机制，可以将数据更新的通知发送给监听者 
6、web Storage的api接口使用更方便
```

12. Ajax跨域

```
为什么会出现跨域？
跨域问题来源于 JavaScript 的同源策略，即只有 协议+主机名+端口号 (如存在)相同，则允许相互访问。也就是说 JavaScript 只能访问和操作自己域下的资源，不能访问和操作其他域下的资源。
跨域问题是针对 JS 和 ajax 的，html本身没有跨域问题，比如a标签、script标签、甚至form标签（可以直接跨域发送数据并接收数据）等。

如何解决跨域问题？
JSONP （只支持get请求不支持post请求）
　　JSONP 是 JSON with Padding 的略称。它是一个非官方的协议，它允许在服务器端集成 Script tags 返回至客户端，通过javascript callback 的形式实现跨域访问（这仅仅是JSONP简单的实现形式）。
JSONP原理
JSONP的最基本的原理是：动态添加一个<script>标签，而script标签的src属性是没有跨域的限制的。这样说来，这种跨域方式其实与ajax XmlHttpRequest协议无关了。

添加响应头，允许跨域
　　addHeader(‘Access-Control-Allow-Origin:*’);//允许所有来源访问
　　addHeader(‘Access-Control-Allow-Method:POST,GET’);//允许访问的方式
代理的方式
服务器A的test01.html页面想访问服务器B的后台action，返回“test”字符串，此时就出现跨域请求，浏览器控制台会出现报错提示，由于跨域是浏览器的同源策略造成的，对于服务器后台不存在该问题，可以在服务器A中添加一个代理action，在该action中完成对服务器B中action数据的请求，然后在返回到test01.html页面。
```

13. 字符串去掉空格

```js
str.trim(); // 去掉首尾空格
str.replace(" ",""); // 去除所有空格，包括首尾、中间
str.replaceAll(" ", ""); //去掉所有空格，包括首尾、中间
str.replaceAll(" +","");  //去掉所有空格，包括首尾、中间
str.replaceAll("\\s*", ""); // 可以替换大部分空白字符，不限于空格
```

14. React兄弟传值
```
* 传参
1. 1号子组件与父组件 先实现 **子--父** 间通信
2. 父组件与 2号子组件 实现 **父--子** 间通信

* redux
使用redux处理数据和状态
```

15. React双向绑定的实现原理
```
什么是双向数据绑定
数据模型和视图之间的双向绑定。

当数据发生变化的时候，视图也就发生变化，当视图发生变化的时候，数据也会跟着同步变化；可以这样说用户在视图上的修改会自动同步到数据模型中去，数据模型也是同样的变化。

双向数据绑定的优点：无需和单向数据绑定那样进行CRUD（Create，Retrieve，Update，Delete）操作，双向数据绑定最常应用在就表单上，这样当用户在前端页面完成输入后，不用任何操作，我们就已经拿到了用户输入好的数据，并放到数据模型中了。

实现双向数据绑定
但是，在 React中是不存在双向数据绑定的机制的，需要我们自己对其进行实现。

数据影响视图
这种功能实际上，React 使用 setState({ }) 方法修改数据。（React内部提供的修改方法），不允许通过 this.state.属性名 = 数据 的方法进行数据修改。

视图影响数据
通过 React 提供的 onChange 监听事件 实现数据的动态录入。
同时，使用 value 或者 defaultValue 在 input 框中呈现内容。
```

16. React的动态路由

```
import {
  HashRouter as Router,
  Route,
  Link,
  Switch
} from 'react-router-dom'
// 使用 react-router

......
  <Route path = "/main/:value">XXXXX</Route>
......

```

17. GET请求和POST请求

```
HTTP协议中的两种发送请求的方法。
HTTP是基于TCP/IP的关于数据如何在万维网中如何通信的协议。

* GET在浏览器回退时是无害的，而POST会再次提交请求。
* GET产生的URL地址可以被Bookmark，而POST不可以。
* GET请求会被浏览器主动cache，而POST不会，除非手动设置。
* GET请求只能进行url编码，而POST支持多种编码方式。
* GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
* GET请求在URL中传送的参数是有长度限制的，而POST么有。
* 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
* GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
* GET参数通过URL传递，POST放在Request body中。

GET产生一个TCP数据包；POST产生两个TCP数据包。
对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。
```

18. 常见的HTTP状态码

```js
const codeMessage = {
  200: '服务器成功返回请求的数据。',
  201: '新建或修改数据成功。',
  202: '一个请求已经进入后台排队（异步任务）。',
  204: '删除数据成功。',
  400: '发出的请求有错误，服务器没有进行新建或修改数据的操作。',
  401: '用户没有权限（令牌、用户名、密码错误）。',
  403: '用户得到授权，但是访问是被禁止的。',
  404: '发出的请求针对的是不存在的记录，服务器没有进行操作。',
  406: '请求的格式不可得。',
  410: '请求的资源被永久删除，且不会再得到的。',
  422: '当创建一个对象时，发生一个验证错误。',
  500: '服务器发生错误，请检查服务器。',
  502: '网关错误。',
  503: '服务不可用，服务器暂时过载或维护。',
  504: '网关超时。',
};
```

19. 判断数组

```js
let arr = [];

// instanceof运算符
// 这个运算符可以判断一个对象是否是在其原型链上原型构造函数中的属性。
console.log(arr instanceof Array); // true

// constructor
// 这个属性是返回对象相对应的构造函数。
console.log(arr.constructor === Array); // true
// __proto__
console.log(arr.__proto__  === ArArray.prototyperay); // true

// 写一个函数方法
var isType = function (obj) {
    // Object.prototype.toString.call(arr) === '[object Array]'
    return Object.prototype.toString.call(obj).slice(8,-1);
}
console.log(isType(arr) === 'Array'); // true

// 数组自带的isArray方法
console.log(Array.isArray(arr)); // true
```

20. 冒泡和捕获

22. 从0~99中随机10个数字，不能重复

23. 数组排序三种方法

24. 延时器相关的题目

25. 一次完整的HTTP请求的7个步骤

26. 变量声明提升、暂时性死区(TDZ)

27. 关于Promise

28. 箭头函数和普通函数的区别

29. v-if. v-show 的区别

30. :key

31. 元素浮动的时候display属性是啥?

32. CSS3的钟表

33. 开发过程中遇到的内存泄露情况，如何解决的?

34. 统计字符串中出现次数最多的字

35. 怎样添加、移除、移动、复制、创建和查找节点?

36. 谈谈垃圾回收机制方式及内存管理

37. 行内元素有哪些?块级元素呢?

38. 哪些CSS属性可以继承?

39. 实现一个函数

40. 你做的页面在哪些浏览器内核中测试过?

51. 什么是函数柯里化?编写函数，将函数柯里化

52. 你知道多少种Doctype 文档类型?

53. 如何形成BFC?

54. 定位有哪几种?分别什么功能

55. rgba0和opacity的透明效果有什么不同

56. 简述CSS的盒模型和box-sizing:border-box 盒模型

57. 什么是函数式编程?

58. 前端页面有哪三层构成，分别是什么?作用是什么?

59. 身上有的，就遮蔽原型了

60. 找的神题、关于闭包的

61. XSS和CSRF

62. 请说出Vue几种常用的指令

63. Vue中如何让CSS只在当前组件中起作用?

64. Vue和React开发中如何使用全局状态常量?你都用这个状态常量做什么事情?

65. 你会用什么工程化工具?它们和webpack有什么异同?

66. 你会不会用Express 或者koa, 简单介绍一-下如何使用?

67. 一次完整HTTP事务是怎样的过程?

68. Vue和React中，什么是路由的懒加载

69. Vue和React中，路由的History 有什么用?

70. Vue-Router中的<router-link>标签和<a>标签有什么区别

71. CSS3中如何实现动画?

