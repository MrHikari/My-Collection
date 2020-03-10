## Less

[Less官网](http://lesscss.org/)<br/>
[Less中文网](http://lesscss.cn/)

### CSS 预处理 Less

`Less` 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。

实际上 less 需要 node.js 编译

Less 既可以在 客户端运行，也可以借助 node.js 在服务端运行。

#### 安装 Less

全局安装 Less
> npm install -g less

#### 浏览器端用法

首先**必须**在项目中放入 `less.min.js` 文件。

其次，在页面文件中引入该 `less.min.js` 文件，并且所写的less样式要指定好**type**，`type="text/less"`

```html
<!-- set options before less.js script -->
<!-- 或者 <script type="text/less"> 直接书写样式 </script> -->
<script>
  less = {
    env: "development",
    async: false,
    fileAsync: false,
    poll: 1000,
    functions: {},
    dumpLineNumbers: "comments",
    relativeUrls: false,
    rootpath: ":/a.com/"
  };
</script>
<script src="less.min.js"></script>
```

这种方法属于 **运行时编译**

---

### Less 中的注释

以 `//` 开头的注释，不会被编译到 css 文件中<br/>
以 `/**/` 包裹的注释会被编译到 css 文件中

---

### Less 中的变量

* 使用 `@` 来生命一个变量，例如 `@backgroundColor: pink`
* 变量 **延迟加载**，在当前变量所在的作用域全部解析结束后，读取当前作用域处理后的`最后结果`
* 如果 **变量** 作为普通的 属性值 使用，直接在对应的样式属性名后赋值，例如`backgroundColor: @backgroundColor`
* 如果 **变量** 作为 选择器 或者 属性名 使用
* 如果 **变量** 作为 URL， `@{url}`

**变量** 作为 `选择器` 或者 `属性名` 示例：
```less
@diyBackground: backgroundColor;
@selector: #wrap;

@{selector}{
  @{diyBackground}: red;
}
```

---

### Less 中的嵌套规则

* 基本的嵌套规则，类似 html 的 层级关系，将下一级的样式写入父级的对象中
* 内层选择器前面的 `&` 符号就表示对父选择器的引用。在一个内层选择器的前面，如果没有 `&` 符号，则它被解析为父选择器的后代；如果有 `&` 符号，它就被解析为父元素自身或父元素的伪类。同时用在选择器中的 `&` 还可以反转嵌套的顺序并且可以应用到多个类名上。
[less 的 & 详解](https://www.jianshu.com/p/127b0974cfc3)

示例：
```less
a {
    color: red;
 
    &:hover {
        color: blue;
    }
 
}
```
转换后的CSS
```css
a {
    color: red;
}
a:hover {
    color: blue;
```

---

### Less 的混合（Mixin）

将需要重复使用的代码封装到一个类中,在需要使用的地方调用封装好的类即可，在预处理的时候 **Less** 会自动将用到的封装好的类中的代码拷贝过来，本质就是 `ctrl+c` --> `ctrl + v`

##### 一般混合 和 不带输出的混合

示例：
```less
.center{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.father{
  width: 300px;
  height: 300px;
  background: red;
  .center();
  .son{
    width: 200px;
    height: 200px;
    background: blue;
    .center();
  }
}
```

**Less** 中混合的**注意点**:
* 如果混合名称的后面**没有**`()`, 那么在预处理的时候,会保留混合的代码（意思是在输出的css中，会含有.center这部分代码）
* 如果混合名称的后面**加上**`()`,那么在预处理的时候,不会保留混合的代码，（意思是在输出的css中，不会含有.center这部分代码，但是调用它的其他类还是复制这部分代码过来了）

示例：
```less
.center{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.center(){
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```

##### 带参数的混合

在 **Less** 中，还可以像函数一样定义一个带参数的属性集合


**带**参数的混合
```less
.whc(@w, @h, @c){
  width: @w;
  height: @h;
  background: @c;
}

#wrap{
  .sample{
    .whc(200px, 100px, red);
  }
}
```
**带**参数的混合, 并且带**默认值**
```less
.whc(@w:100px, @h:100px, @c:pink){
  width: @w;
  height: @h;
  background: @c;
}
.box1{
  //width: 200px;
  //height: 200px;
  //background: red;
  //.whc(200px, 200px, red);

  // 这里是给混合的 指定形参 传递数据
  .whc(@c:red);
}
.box2{
  //width: 300px;
  //height: 300px;
  //background: blue;
  .whc(300px, 300px, blue);
}
```
赋值通过 `:` 赋值，参数也是，不是 `=` 。
即使混合有三个参数，你可以只传一个，且可以**指定**给哪一个形参传值。

##### 匹配模式

当编写一个组件，或者需要展示一些动态样式的时候，我们需要尽可能地节省代码，复用大部分公共代码

示例：比如某个控件的 less文件 比如我们希望三角形能够选择朝向样色等
```less
.triagle(@_) { // 注意：1. 样式名要一致  2. @_ 标识符 每次调用下面的混合时会带上这个混合，作为公共的样式
  width: 0px;
  height: 0px;
  overflow: hidden;
}

// 尖角朝右
.triagle(L, @w, @c) {
  border-width: @w; // 自定义宽度
  border-style: dashed solid dashed dashed; // 为了兼容 IE7 以下浏览器
  border-color: transparent @c transparent transparent; // 自定义颜色
}
// 尖角朝左
.triagle(R, @w, @c) {
  border-width: @w; // 自定义宽度
  border-style: dashed dashed dashed solid; // 为了兼容 IE7 以下浏览器
  border-color: transparent transparent transparent @c; // 自定义颜色
}
// 尖角朝下
.triagle(T, @w, @c) {
  border-width: @w; // 自定义宽度
  border-style: dashed dashed solid dashed; // 为了兼容 IE7 以下浏览器
  border-color: transparent transparent @c transparent; // 自定义颜色
}
// 尖角朝上
.triagle(B, @w, @c) {
  border-width: @w; // 自定义宽度
  border-style: solid dashed dashed dashed; // 为了兼容 IE7 以下浏览器
  border-color: @c transparent transparent transparent; // 自定义颜色
}
```

然后在业务逻辑文件中调用的时候
```css
@import "less文件地址";

#wrap .sjx{
  .triagle(R, 40px, pink)
}
```

##### 变量 @arguments

在 Mixin 中，**@arguments** 具有特殊的意思，当 Mixin 被调用的时候，它包含了所有传入的参数，如果你不想单独处理参数的话，这个将会很好用。
```less
.box-shadow(@x: 0; @y: 0; @blur: 1px; @color: #000){
    -webkit-box-shadow: @arguements;
       -moz-box-shadow: @arguements;
            box-shadow: @arguements;
}
.test{
    .box-shadow(2px; 5px);
}
```
编译为：
```css
.test{
    -webkit-box-shadow: 2px 5px 1px #000;
       -moz-box-shadow: 2px 5px 1px #000;
            box-shadow: 2px 5px 1px #000;
}
```

##### 可变参数和变量 @rest

如果想使用数量可变的参数的 Mixin 的时候，你可以使用 `...` 这个参数。在一个变量名后面使用这个参数，将会给变量分配参数。
```less
.mixin(...){} // 匹配0-N个参数
.mixin(){} // 只匹配0个参数
.mixin(@a: 1){} // 匹配0-1个参数
.mixin(@a: 1; ...){} // 匹配0-N个参数
.mixin(@a;...){} // 匹配1-N个参数

// 此外：

.mixin(@a; @rest...){
// @rest 代表 @a参数 后面的所有参数
// @arguement 代表所有参数
}
```

---

### Less 运算

示例：
```less
@width: 100px;
@height: 30px;

.box_demo {
  width: @width +20;
  height: (@height + 10) * 5;
}
```

* **Less** 中可以直接给变量进行一些运算操作的，比如进行 `+ - * /` 操作等，你也可以使用 `()` 小括号来添加一些更复杂的运算操作；
* **Less** 中进行运算操作的时候，**不强制**进行运算操作的值必须带单位，只要有**一个**有单位就可以了

