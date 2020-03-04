1. html的元素有哪些（包含H5）?<br/>
**回答：**
```js
// 表单类
form（块级）
input、textarea、button、select、label

// 布局结构类（以下基本属于块级元素）
div、p、h1、h2、h3、h4、h5、h6、ul、li、ol、table、tr、td、thead、tbody。
span（内联元素）

// 媒体相关（以下基本属于内联元素）
audio、vedio、img、canvas、a

// h5语义化标签
header、footer、section、article、aside

// 功能类标签
link、meta、title、html、body、script

// 其他不常用的标签
br、hr
```

---

2. CSS3有哪些新增的特性?<br/>
**回答：**
### 边框(borders):
* border-radius 圆角
* box-shadow  盒阴影
* border-image 边框图像

### 背景:
* background-size 背景图片的尺寸
* background_origin 背景图片的定位区域
* background-clip 背景图片的绘制区域

### 渐变：
* linear-gradient 线性渐变
* radial-gradient 径向渐变

### 文本效果;
* word-break
* word-wrap
* text-overflow
* text-shadow
* text-wrap
* text-outline
* text-justify

### 转换：
* **2D转换属性**
* transform
* transform-origin
* **2D转换方法**
* translate(x,y)
* translateX(n)
* translateY(n)
* rotate(angle)
* scale(n)
* scaleX(n)
* scaleY(n)
* rotate(angle)
* matrix(n,n,n,n,n,n)

### 3D转换：
*3D转换属性：

* transform
* transform-origin
* transform-style
* 3D转换方法
* translate3d(x,y,z)
* translateX(x)
* translateY(y)
* translateZ(z)
* scale3d(x,y,z)
* scaleX(x)
* scaleY(y)
* scaleZ(z)
* rotate3d(x,y,z,angle)
* rotateX(x)
* rotateY(y)
* rotateZ(z)
* perspective(n)

### 过渡
* transition

### 动画
* @Keyframes规则
* animation

### 弹性盒子(flexbox)
### 多媒体查询@media

---

3. CSS选择器有哪些<br/>
**回答：**

选择器	例子	例子描述	CSS
.class	.intro	选择 class="intro" 的所有元素。	1
#id	#firstname	选择 id="firstname" 的所有元素。	1
*	*	选择所有元素。	2
element	p	选择所有
元素。

1
element,element	div,p	选择所有
元素和所有
元素。

1
element element	div p	选择
元素内部的所有
元素。

1
element>element	div>p	选择父元素为
元素的所有
元素。

2
element+element	div+p	选择紧接在
元素之后的所有
元素。

2
[attribute]	[target]	选择带有 target 属性所有元素。	2
[attribute=value]	[target=_blank]	选择 target="_blank" 的所有元素。	2
[attribute~=value]	[title~=flower]	选择 title 属性包含单词 "flower" 的所有元素。	2
[attribute|=value]	[lang|=en]	选择 lang 属性值以 "en" 开头的所有元素。	2
:link	a:link	选择所有未被访问的链接。	1
:visited	a:visited	选择所有已被访问的链接。	1
:active	a:active	选择活动链接。	1
:hover	a:hover	选择鼠标指针位于其上的链接。	1
:focus	input:focus	选择获得焦点的 input 元素。	2
:first-letter	p:first-letter	选择每个
元素的首字母。

1
:first-line	p:first-line	选择每个
元素的首行。

1
:first-child	p:first-child	选择属于父元素的第一个子元素的每个
元素。

2
:before	p:before	在每个
元素的内容之前插入内容。

2
:after	p:after	在每个
元素的内容之后插入内容。

2
:lang(language)	p:lang(it)	选择带有以 "it" 开头的 lang 属性值的每个
元素。

2
element1~element2	p~ul	选择前面有
元素的每个

元素。

3
[attribute^=value]	a[src^="https"]	选择其 src 属性值以 "https" 开头的每个 元素。	3
[attribute$=value]	a[src$=".pdf"]	选择其 src 属性以 ".pdf" 结尾的所有 元素。	3
[attribute*=value]	a[src*="abc"]	选择其 src 属性中包含 "abc" 子串的每个 元素。	3
:first-of-type	p:first-of-type	选择属于其父元素的首个
元素的每个

元素。

3
:last-of-type	p:last-of-type	选择属于其父元素的最后
元素的每个

元素。

3
:only-of-type	p:only-of-type	选择属于其父元素唯一的
元素的每个

元素。

3
:only-child	p:only-child	选择属于其父元素的唯一子元素的每个
元素。

3
:nth-child(n)	p:nth-child(2)	选择属于其父元素的第二个子元素的每个
元素。

3
:nth-last-child(n)	p:nth-last-child(2)	同上，从最后一个子元素开始计数。	3
:nth-of-type(n)	p:nth-of-type(2)	选择属于其父元素第二个
元素的每个

元素。

3
:nth-last-of-type(n)	p:nth-last-of-type(2)	同上，但是从最后一个子元素开始计数。	3
:last-child	p:last-child	选择属于其父元素最后一个子元素每个
元素。

3
:root	:root	选择文档的根元素。	3
:empty	p:empty	选择没有子元素的每个
元素（包括文本节点）。

3
:target	#news:target	选择当前活动的 #news 元素。	3
:enabled	input:enabled	选择每个启用的 元素。	3
:disabled	input:disabled	选择每个禁用的 元素	3
:checked	input:checked	选择每个被选中的 元素。	3
:not(selector)	:not(p)	选择非
元素的每个元素。

3
::selection	::selection	选择被用户选取的元素部分。	3
[css有哪些属性可以继承？](https://www.jianshu.com/p/fbfc6c751e34)

---
