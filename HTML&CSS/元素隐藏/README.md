### 元素隐藏的不同方式

**dispaly**, **visibility**, **opacity**, **height**&**border**。

#### 简单示例

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>title</title>
    <style>
      div {
        width: 300px;
        height: 200px;
        border: 1px solid red;
      }
    </style>
  </head>
  <body>
    <input type="button" value="显示效果" id="btn" />
    <div id="dv">示例</div>
    <script src="common.js"></script>
    <script>
      document.getElementById("btn").onclick = function() {
        //隐藏div
        //不占位
        //my$("dv").style.display="none";
        //占位
        //my$("dv").style.visibility="hidden";
        //占位
        //my$("dv").style.opacity=0;
        //占位
        //my$("dv").style.height="0px";
        //my$("dv").style.border="0px solid red";
      };
    </script>
  </body>
</html>
```

<br/>

---

<br/>

### 伸缩布局

参考链接：https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex

以前在网页开发过程中，布局一直是不可或缺的，从最早的**表格布局**，到后来的**DIV+CSS 布局**，现在再到**CSS3 的伸缩布局**。

**CSS3**在布局方面做了非常大的改进，使得我们对块级元素的布局排列变得十分灵活，适应性非常强，其强大的伸缩性，在响应式开中可以发挥极大的作用。

- 主轴：Flex 容器的主轴主要用来配置 Flex 项目，默认是水平方向

- 侧轴：与主轴垂直的轴称作侧轴，默认是垂直方向的

- 方向：默认主轴从左向右，侧轴默认从上到下

_主轴和侧轴并不是固定不变的，通过 flex-direction 可以互换。_

#### 使用说明

- 指定一个父盒子为伸缩盒子，即指定：display: flex
- 明确你需要的主侧轴方向，并设置 flex-direction 默认是 row ，纵向是 column
- 设置父盒子的属性来调整子元素的布局，例如 align-items
- 设置子盒子的宽高或者比例，完成具体的子元素在父盒子中的布局

#### 各个属性介绍

- flex-direction 调整主轴方向（默认为水平方向）
- justify-content 调整主轴对齐
- align-items 调整侧轴对齐（子元素可以使用 align-self 覆盖）
- flex-wrap 控制是否换行
- align-content 堆栈（由 flex-wrap 产生的独立行）对齐
- flex-flow 是 flex-direction + flex-wrap 的简写形式
- flex 是子项目在主轴的缩放比例，不指定 flex 属性，则不参与伸缩分配
- order 控制子项目的排列顺序，正序方式排序，从小到大

<br/>

---

<br/>

### 文字溢出省略——CSS 样式

单行省略

```css
display: block;
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;
```

多行省略 （数字即为自定义的行数）/（需要注意溢出隐藏的高度）

```css
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
```

示例

```html
 <style>
     .dan{
         font-size:14px;
         color:#000000;
         line-height:40px;
         height: 40px;
         width:300px;
         background:pink;
         /* 单行省略 */
         display: block;
         overflow: hidden;
         white-space: nowrap;
         text-overflow:ellipsis;
     }
     .duo{
         height:120px;
         width:300px;
         background:pink;
         line-height:40px;
         margin-top:20px;
         /* 多行省略 */
         overflow:hidden;
         text-overflow:ellipsis;
         display:-webkit-box;
         -webkit-box-orient:vertical;
         -webkit-line-clamp:3;
     }
 </style>
 <body>
     <div class="dan">这个是单行省略这个是单行省略这个是单行省略这个是单行省略</div>
     <div class="duo">这个是多行省略这个是多行省略这个是多行省略这个是多行省略这个是多行省略这个是多行省略这个是多行省略这个是多行省略咋回事儿咋回事儿咋回事儿</div>
 </body>
 </html>
```
