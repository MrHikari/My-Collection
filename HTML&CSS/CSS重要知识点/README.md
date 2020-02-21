#### 1.图片向下撑大3像素问题

在一个盒子里面放一张图片，默认情况下，图片会**向下撑大3像素**，有以下几种解决方法：

1. 给图片添加 `display:block`；

2. 给图片添加 `float:left`；

3. 给图片添加 `vertical-align:middle`;

4. 给它的父元素加 `font-size：0`；

#### 2.如何实现一张未知宽高的图片在一个盒子里面做水平垂直居中？

* 给父元素添加宽高，添加行高 添加 `text-align：center`
* 给图片添加 `vertical-align：center`

#### 3.元素的类型分类哪几种？各自都有什么特点？

* **块元素** &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;独占一行，竖着排，能设置宽高
* **行内元素** &emsp;&emsp;&emsp;&emsp;&emsp;默认情况下文本一行显示，不能设置宽高
* **行内块状元素** &emsp;&emsp;&emsp;inline-block,既有行内元素的特点又有块状元素的特点
* **可变元素** &emsp;&emsp;&emsp;&emsp;&emsp;添加float：left 相当于display：block

#### 4.如何实现一个元素消失和出现？

* `display：none`&emsp;&emsp;&emsp;`display：block`
* `opcity:0`&emsp;&emsp;&emsp;&emsp;&emsp; `opcity:1`

#### 5.单行文本溢出显示省略号怎么实现？

```css
width: ***px;
white-space:nowrap;
overflow:hidden;
text-overflow:ellipsis;
```

#### 6.定位的属性值有哪几个？分别有什么特点？

* `position:absolute`&emsp;&emsp;&emsp;绝对定位，脱离文档流    
* 在有父元素或者父元素没有设置定位的情况下,它的参照物是整个浏览器
* 如果父元素设置了相对定位,那么它的参照物就是它的父元素 

* `position:relative`&emsp;&emsp;&emsp;相对定位，不脱离文档流
* 它的参照物是它原来的位置

* `position:fixed`&emsp;&emsp;&emsp;固定定位，脱离文档流
* `position:sticky`&emsp;&emsp;&emsp;粘性定位，脱离文档流
* 它的参照物是整个浏览器

#### 7.样式引入的方式有哪几种

* 外部引入 
```html
<link rel="stylesheet" type="text/css" href=""/>      
    <style>@import url("global.css")</style>
```
* 内部引入&emsp;&emsp;&emsp;`<style></style>`
* 行内样式引入&emsp;&emsp;&emsp;`<div style="">`

#### 8.transition过渡动画使用的过程中要注意一些什么？

1. 必须跟hover一起使用
2. 在hover的时候添加过渡，鼠标滑上去有过渡效果，移开就没有<br/>给他本身加的时候，鼠标滑上去有过渡效果，移开也有

#### 9.用边框写出一个箭头超右的三角形

```css
div{
    border-top:10px solid transparent;
    border-right:10px solid transparent;
    border-bottom:10px solid transparent;
    border-left:10px solid red;
    width:0;
    height:0;
}
```

#### 10.可以取负值的css属性有哪些？

* text-indent
* z-index
* margin-top
* margin-left
* background-position
* left right bottom top
* text-shadow
* box-shadow
等等......

#### 11.一个未知宽高的盒子在另一个盒子里面 水平垂直居中 的3种方法：(不用做计算)

``` css
// 方法1
.box{
  width:500px;
  height:500px;
  position:relative;
}
.box1{
  width:200px;
  height:200px;
  position:absolute;
  left:0;
  right:0;
  bottom:0;
  left:0;
  margin:auto;
}
```
```css
// 方法2
.box{
  width:500px;
  height:500px;
  display:flex;             // 弹性盒
  justify-content:center;   // 水平居中
  align-items:center;       // 垂直居中
}
.box1{
  width:200px;
  height:200px;
}
```
```css
// 方法3
.box{
  width: 500px;
  height: 500px;
  background: red;
  position: relative;
}
.box1{
  width: 200px;
  height: 200px;
  background: green;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
```

#### 12.当子元素使用margin-top，导致父元素整个下移的解决方案：

1. overflow:hidden        // 溢出隐藏、清除浮动、解决外边距塌陷等等
2. 把margin改成padding
3. border-top:1px solid rgba(0,0,0,0)
4. 给父元素或者子元素浮动

#### 13.子元素都设置float，父元素没有设置高度，出现高度塌陷的问题，解决方案：

1. 给父元素设置height 遇到自适应用不了，
2. 添加`overflow：hidden/auto`
3. 给浮动的元素下面添加一个空盒子，给空盒子添加`clear：both;`
4. 万能清除法如下：
```css
.clear:after{
  content:"";   // content 属性与 :before 及 :after 伪元素配合使用，来插入生成内容。
  clear:both;
  display:block；
  height:0;
  overflow:hidden;
  visibility:hidden;
}
.clear{
  zoom:1;
}
```
5. 给父元素也添加**float**
6. 给父元素添加`display:table`

#### 14.透明度的属性是什么？请也写上它的兼容写法？

```css
  // IE下是filter:alpha(opacite=30)；火狐和谷歌是opacite:0.3
  opcity:0.3;
  filter:alpha(opcity=30) 
```

#### 15.什么是BFC？BFC的触发条件有哪些？

&emsp;&emsp;**BFC** 全称为 **块格式化上下文** (Block Formatting Context) 直译为块级格式化上下文，是一个独立的渲染区域。具有BFC特性的元素可以看作是一个隔离了的独立容器，容器里面的内容不会影响到外面的元素。<br/>
&emsp;&emsp;使用了`float:left/right` <br/>`position:absolute/fixed`  <br/>`display:inline-block/table-cell/table-caption/flex/inline-flex` <br/>`overflow:hidden/auto`等等 都是**BFC**。

#### 16.如何解决margin上下值发生重叠的问题？

&emsp;&emsp;给任何一个子元素添加一个父元素，并让这个父元素成为**BFC**区域里面的元素，所以就需要给父元素添加`overflow:hidden/auto/scroll;` `display:inline-block/flex;`等。

#### 17.怪异盒是怎么产生的？它有什么特点？如何由W3C标准盒模型变成怪异盒模型？

* 产生原因：DOCTYPE的缺失在IE8以下会触发怪异盒模式
* 特点：padding值不会计算在元素原有的宽高之上
* border值不会计算在元素原有宽高之上
* 变成怪异盒模型：添加属性box-sizing:border-box;
* box-sizing:content-box;默认值

#### 18.哪些属性可以被继承？

1. 字体系列属性
```css
  font-family：字体样式

  font-weight：字体的粗细

  font-size：字体的大小

  font-style：字体的类型
```

2. 文本系列属性
```css
  text-indent：文本缩进

  text-align：文本水平对齐

  line-height：行高

  letter-spacing：单词之间的间距

  text-transform：控制文本小：uppercase、lowercase、capitalize

  color：文本颜色

  // 列表
  list-style: list-style 简写属性在一个声明中设置所有的列表属性。
```

 #### 19.图片整合是用什么技术实现的？

* css Sprites
* 用background-position 来进行背景图片定位技术

&emsp;&emsp;CSS Sprites其实就是把网页中一些背景图片整合到一张图片文件中，再利用CSS的"background-image","background-repeat","background-position"的组合进行背景定位，background-position可以用数字精确地定位出背景图片的位置。

 #### 20.移动端布局的方式有哪些？

* 流式布局、等比缩放布局或混合布局

**知识点**：《CSS3-rem》相对于根元素字体大小的单位<br/>
  《CSS3-media》在Html内容不变的情况，根据媒体设备不同，浏览器窗口尺寸不同，使用不同的CSS样式<br/>
  《CSS3-flex》弹性布局

 #### 21.transition和animation之间有什么共同点和不同点？

* 相同点：都是随着时间改变元素的属性值
* 不同点：1.transition需要跟hover一起使用<br/>2.animation不需要触发任何事件

#### 22.em和rem是什么？移动端为什么要用rem这个单位？

* em是相对于最近的父元素的字号大小，1em=16px
* rem 是 root em是相对于根元素字号的大小， 1rem=16px

#### 23.响应式网页设计有哪些特点？
 
1. 网站必须建立灵活的网格基础；  
2. 引用到网站的图片必须是可伸缩的
3. 不同的显示风格，需要在Media Query上设置不同的样式 
4. meta标签