## jQuery

### jQuery是什么

**jQuery**是一个快速，小巧且功能丰富的`JavaScript`库。

<br/>

---

<br/>

### 基本使用

1. 引入`jQuery`文件
2. 写一个入口函数
```html
<script>
  $(document).ready(function() { })
</script>
```
3. 确定需要操作的元素（**jQuery**选择器），操作它（为它添加属性，样式，文本......）
```html
<script>
  $(document).ready(function() {
    $('div').width(100).height(100).css('backgroundColor', 'red').text('文本内容');
  })
</script>
```

<br/>

---

<br/>

### 获取 jQuery 文件

浏览 [jQuery官网](https://jquery.com/) 下载对应**需要版本**的 `jQuery` 文件

**版本简介：**

* 1.x 版本 支持IE6 7 8 已停止更新
* 2.x 版本 不支持老版本浏览器 已停止更新
* 3.x 版本 不支持老版本浏览器 当前仍在更新

每一个版本都有压缩版和未压缩版。
* 压缩版`compressed` 去掉了格式，体积小，用于发布
* 未压缩版`uncompressed` 也叫原版，体积较大，方便阅读，用于测试、学习和开发。

<br/>

---

<br/>

### jQuery 入口函数

**jQuery** 入口函数 和 原生 **JS** 写法的 `window.onload` 入口函数的区别

* **jQuery** 入口函数 在有多个入口函数的时候正常运行
* 执行实际不同：**jQuery**入口函数要快于 `window.onload`
* `window.onload` 要等待页面上所有的资源（dom树/外部css/js连接，图片）都加在完后执行

```html
<script>
  $(function() {
    console.log("入库函数1")
  })
  $(function() {
    console.log("入库函数2")
  })
</script>
```

原生 **JS** 写法
```html
<script>
  window.onload = function () {
    console.log("原生的入口函数") // 后执行
  }
  $(function() {
    console.log("jQuery入库函数") // 先执行
  })
</script>
```

<br/>

---

<br/>

### $ 符号 其实是一个 函数

1. $ （如果遇到报错：$ is not defined，说明没有引入 jQuery 文件）
```js
(function() {

})
```
* `jQuery`文件结构，其实是一个自执行函数，为了在window中添加jQuery元素，$ 其实和 jQuery 是等价的，是一个函数
```js
(function() {
  window.jQuery = window.$ = jQuery;
})
```
* 引入一个 **js** 文件，是会执行这个**js**文件中的代码

2. 传递参数不同，效果也不一样
* * 如果参数传递的是一个匿名函数--->入口函数
* * 如果参数传递的是一个字符串--->选择器/创建一个标签

3. 如果参数是一个dom对象，那他就会把dom对象转换成 jQuery 对象

<br/>

---

<br/>

### dom 对象和 jQuery 对象

**dom 对象**
```html
<script>
  $(function() {
    // 原生js选择器 获取到的对象
    // 特点：只能调用dom方法或者属性，不能调用jQuery的属性或者方法
    var div1 = document.getElementById("one(这里假定一个html中id为one的元素)");
    div1.style.backgroundColor = 'red'; // dom对象是可以调用dom的属性或者方法
    // div1.css('backgroundColor', 'green'); // 报错，因为dom对象不能调用jQuery的属性或者方法
  })
</script>
```

**jQuery 对象**

利用 jQuery 选择器获取到的对象<br/>
特点：只能调用jQuery的方法或者属性，不能调用原生 js dom 对象的属性或者方法
```html
<script>
  $(function() {
    var $div1 = $('#one');
    $div1.css('backgroundColor', 'green'); // jQuery 对象可以调用jQuery的属性或者方法
    // $div1.style.backgroundColor = 'red'; // 报错，jQuery 对象是不可以调用dom的属性或者方法
  })
</script>
```

**jQuery 对象 是什么**

`jQuery 对象`是一个 **伪数组**，其实就是 dom 对象的一个包装集

试验一下：
```html
<script>
  $(function() {
    var div1 = document.getElementById("one(这里假定一个html中id为one的元素)");
    console.log('div1--->', div1);
    var $div1 = $('#one');
    console.log('$div1--->', $div1);
    console.log('checked--->', $div1.__proto__ === Array.prototype); // false, 不是数组，
  })
</script>
```

<br/>

---

<br/>

### dom 对象和 jQuery 对象 相互转换

dom对象 转换成 jQuery对象
```html
<script>
  $(function() {
    var div1 = document.getElementById("one(这里假定一个html中id为one的元素)");
    var $div1 = $(div1);
    console.log('$div1--->', $div1);
  })
</script>
```

jQuery对象 转换成 dom对象
```html
<script>
  $(function() {
    var $divs = $(div); // 获取html中全部的div
    // 使用下标获取 dom对象
    var div1 = $divs[0]
    console.log('div1--->', div1);
    // 使用 jQuery方法 转换
    var div2 = $divs.get(1);
  })
</script>
```

<br/>

---

<br/>

### 设置获取文本 text() 方法

**注意：** 只能单纯的处理文本，不能够解析其他标签内容

* 获取文本： `text()` 不传入参数即可，可以获取到当前指定标签中的全部文本（包括其后代中的全部文本）<br/>包含了多个dom元素的jQuery对象，通过`text()`方法获取文本，会把所有的dom元素的文本获取到

* 设置文本： `text()` 传入参数，参数就是要设置的文本，设置文本会覆盖原先的内容<br/>包含了多个dom元素的jQuery对象，通过`text()`设置文本，会把所有的dom元素的文本都覆盖

<br/>

---

<br/>

### 设置获取样式 css() 方法

**注意：** 在 IE浏览器 中要获取边框这样的样式值，需要给定准确的边框

* 获取样式： `css()` 设置参数为获取值的样式名

* 获取某一类标签的样式时，默认值是获取同类标签的第一个样式值<br/>获取包含了多个dom元素的jQuery对象的样式，只能获取到第一个dom对象的样式

* 设置样式：单样式--->`css(样式名:样式值)`  多样式--->`css({ 样式名1: 样式值, 样式名2: 样式值})`

* 设置的样式是**行内样式**

<br/>

---

<br/>

### jQuery 基本选择器

**jQuery** 选择器是 jQuery 为开发者提供的一组方法，更加方便的获取到页面中的元素。<br/>
**注意：** jQuery 选择器 返回的是 jQuery 对象。

|名称|用法|描述|
|-|-|-|
|ID选择器|$('#myId')|选择id为myId的网页元素|
|类选择器|$('.myClass')|选择class为myClass的元素|
|标签选择器|$('li')|选择所有的li标签元素|
|并集选择器|$('div,p,li')|使用逗号分隔，只要符合条件之一即可|
|交集选择器|$('div.redClass')|选择class为redClass的div标签元素|

<br/>

---

<br/>

### jQuery 层级选择器

|名称|用法|描述|
|-|-|-|
|子代选择器|$('#ul1 > li')|使用>,获取子层级的元素，并不需要继续获取再下一级的元素|
|后代选择器|$('#ul1 li span')|选择id为为ul1元素下的所有li下的所有span元素|

<br/>

---

<br/>

### jQuery 过滤选择器

**注意：** 这类选择器都带有 冒号

|名称|用法|描述|
|-|-|-|
| :eq(index) | $(“li:eq(2)”).css(“color”, ”red”)| 获取到的li元素中，选择索引号为2的元素，索引号index从0开始。|
| :odd | $(“li:odd”).css(“color”, ”red”)| 获取到的li元素中，选择索引号为奇数的元素|
| :even | $(“li:even”).css(“color”, ”red”)| 获取到的li元素中，选择索引号为偶数的元素|

* **eq** 是 equal 等于 的简写
* **odd** 奇数的意思
* **even** 偶数的意思

<br/>

---

<br/>

### jQuery 鼠标的mouse事件方法

##### mousedown() 方法

* 当鼠标指针移动到元素上方，并按下鼠标左键时，会发生 mousedown 事件。
* mousedown() 方法触发 mousedown 事件，或添加当发生 mousedown 事件时运行的函数。
* **提示：**该方法通常与 mouseup() 方法一起使用。

##### mouseenter() 方法

* 当鼠标指针穿过（进入）被选元素时，会发生 mouseenter 事件。
* mouseenter() 方法触发 mouseenter 事件，或添加当发生 mouseenter 事件时运行的函数。
* **注意：**与 mouseover 事件不同，mouseenter 事件只有在鼠标指针进入被选元素时被触发，mouseover 事件在鼠标指针进入任意**子元素**时也会被触发。参见页面底部演示实例。
* **提示：**该事件通常与 mouseleave 事件一起使用。

##### mouseleave() 方法

* 当鼠标指针离开被选元素时，会发生 mouseleave 事件。
* mouseleave() 方法触发 mouseleave 事件，或添加当发生 mouseleave 事件时运行的函数。
* **注意：**与 mouseout 事件不同，mouseleave 事件只有在鼠标指针离开被选元素时被触发，mouseout 事件在鼠标指针离开任意**子元素**时也会被触发。
* **提示：**该事件通常与 mouseenter 事件一起使用。

##### mousemove() 方法

* 当鼠标指针在指定的元素中移动时，就会发生 mousemove 事件。
* mousemove() 方法触发 mousemove 事件，或添加当发生 mousemove 事件时运行的函数。
* **注意：**用户把鼠标移动一个像素，就会发生一次 mousemove 事件。处理所有 mousemove 事件会耗费系统资源。请谨慎使用该事件。

##### mouseout() 方法

* 当鼠标指针离开被选元素时，会发生 mouseout 事件。
* mouseout() 方法触发 mouseout 事件，或添加当发生 mouseout 事件时运行的函数。
* **注意：**与 mouseleave 事件不同，mouseout 事件在鼠标指针离开被选元素或任意**子元素**时都会被触发，mouseleave 事件只有在鼠标指针离开被选元素时被触发。
* **提示：**该事件通常与 mouseover 事件一起使用。

##### mouseover() 方法

* 当鼠标指针位于元素上方时，会发生 mouseover 事件。
* mouseover() 方法触发 mouseover 事件，或添加当发生 mouseover 事件时运行的函数。
* **注意：**与 mouseenter 事件不同，mouseover 事件在鼠标指针进入被选元素或任意**子元素**时都会被触发，mouseenter 事件只有在鼠标指针进入被选元素时被触发。
* **提示：**该事件通常与 mouseout 事件一起使用

##### mouseup() 方法

* 当鼠标指针移动到元素上方，并松开鼠标左键时，会发生 mouseup 事件。
* mouseup() 方法触发 mouseup 事件，或添加当发生 mouseup 事件时运行的函数。
* **提示：**该方法通常与 mousedown() 方法一起使用。

<br/>

---

<br/>