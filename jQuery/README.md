## jQuery

### jQuery是什么

**jQuery**是一个快速，小巧且功能丰富的`JavaScript`库。

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

### 获取 jQuery 文件

浏览 [jQuery官网](https://jquery.com/) 下载对应**需要版本**的 `jQuery` 文件

**版本简介：**

* 1.x 版本 支持IE6 7 8 已停止更新
* 2.x 版本 不支持老版本浏览器 已停止更新
* 3.x 版本 不支持老版本浏览器 当前仍在更新

每一个版本都有压缩版和未压缩版。
* 压缩版`compressed` 去掉了格式，体积小，用于发布
* 未压缩版`uncompressed` 也叫原版，体积较大，方便阅读，用于测试、学习和开发。

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

