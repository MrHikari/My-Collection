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


