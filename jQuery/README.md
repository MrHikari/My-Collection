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
