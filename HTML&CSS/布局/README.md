### CSS 布局笔记

#### vertical-align 属性

名称|描述
-|-
baseline|默认。元素放置在父元素的基线上。|
sub|垂直对齐文本的下标。|
super|垂直对齐文本的上标|
top|把元素的顶端与行中最高元素的顶端对齐|
text-top|把元素的顶端与父元素字体的顶端对齐|
middle|把此元素放置在父元素的中部。|
bottom|把元素的顶端与行中最低的元素的顶端对齐。|
text-bottom|把元素的底端与父元素字体的底端对齐。|
length|-|
%|使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。|
inherit|规定应该从父元素继承 vertical-align 属性的值。|


### webkit

#### ::-webkit-scrollbar 滚动条的设置

 ::-webkit-scrollbar         滚动条整体部分
 ::-webkit-scrollbar-thumb             滚动条里面的小方块，能上下左右移动（取决于是垂直滚动条还是水平滚动条）
 ::-webkit-scrollbar-track      滚动条的轨道（里面装有thumb）
 ::-webkit-scrollbar-button      滚动条轨道两端的按钮，允许通过点击微调小方块的位置
 ::-webkit-scrollbar-track-piece    内层轨道，滚动条中间部分（除去）
 ::-webkit-scrollbar-corner     边角，及两个滚动条的交汇处
 ::-webkit-resizer       两个滚动条的交汇处上用于通过拖动调整元素大小的小控件


自定义滚动条简单版：

/*定义滚动条宽高及背景，宽高分别对应横竖滚动条的尺寸*/
.scrollbar::-webkit-scrollbar{
    width: 16px;
    height: 16px;
    background-color: #f5f5f5;
}
/*定义滚动条的轨道，内阴影及圆角*/
.scrollbar::-webkit-scrollbar-track{
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
    border-radius: 10px;
    background-color: #f5f5f5;
}
/*定义滑块，内阴影及圆角*/
.scrollbar::-webkit-scrollbar-thumb{
    /*width: 10px;*/
    height: 20px;
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
    background-color: #555;
}

### CSS calc() 函数
css
```
#div1 {
    position: absolute;
    left: 50px;
    width: calc(100% - 100px);
    border: 1px solid black;
    background-color: yellow;
    padding: 5px;
    text-align: center;
}
```
**定义与用法**

`calc()` 函数用于动态计算长度值。

* 需要注意的是，运算符前后都需要保留一个空格，例如：width: calc(100% - 10px)；
* 任何长度值都可以使用calc()函数进行计算；
* calc()函数支持 "+", "-", "*", "/" 运算；
* calc()函数使用标准的数学运算优先级规则；

`calc(expression)`<br/>
expression 必须，一个数学表达式，结果将采用运算后的返回值。