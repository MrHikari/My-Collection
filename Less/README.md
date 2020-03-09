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
即使混合有三个参数，你可以只传一个，且可以指定给哪一个形参传值。