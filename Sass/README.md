## Sass

### 世界上最成熟、最稳定、最强大的专业级CSS扩展语言！

---

### 安装Sass和Compass

**sass**基于**Ruby语言**开发而成，因此安装**sass**前需要安装**Ruby**。（**注**:`mac`下自带**Ruby**无需在安装Ruby!）

`window`下安装**SASS**首先需要安装**Ruby**，先从官网下载 Ruby 并安装。安装过程中请**注意勾选** `Add Ruby executables to your PATH` 添加到系统环境变量。

**注意**：这里以 **Mac** 为主

// 安装如下(如mac安装遇到权限问题需加 `sudo gem install sass`)
> gem install sass<br/>
> gem install compass

安装完成之后，你应该通过运行下面的命令来确认应用已经正确地安装到了电脑中：

> sass -v<br/>
> Sass 3.x.x (Selective Steve)

> compass -v<br/>
> Compass 1.x.x (Polaris)<br/>
> Copyright (c) 2008-2015 Chris Eppstein<br/>
> Released under the MIT License.<br/>
> Compass is charityware.<br/>
> Please make a tax deductable donation for a worthy cause: http://umdf.org/compass

**更新sass命令**

```
// 更新sass
> gem update sass

// 查看sass版本
> sass -v

// 查看sass帮助
> sass -h
```

---

### 编译sass

**sass**编译有很多种方式，如命令行编译模式、sublime插件SASS-Build、编译软件koala、前端自动化软件codekit、Grunt打造前端自动化工作流grunt-sass、Gulp打造前端自动化工作流gulp-ruby-sass等。

#### 命令行编译;

```
// 单文件转换命令
> sass input.scss output.css

// 单文件监听命令
> sass --watch input.scss:output.css

// 如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
> sass --watch app/sass:public/stylesheets
```

##### 命令行编译配置选项

命令行编译**sass**有配置选项，如编译过后**css**排版、生成调试map、开启debug信息等，可通过使用命令`sass -v`查看详细。一般常用两种 `--style` `--sourcemap`。

```
// 编译格式
> sass --watch input.scss:output.css --style compact

// 编译添加调试map
> sass --watch input.scss:output.css --sourcemap

// 选择编译格式并添加调试map
> sass --watch input.scss:output.css --style expanded --sourcemap

// 开启debug信息
> sass --watch input.scss:output.css --debug-info
```
* `--style` 表示解析后的 css 是什么排版格式;<br/>sass内置有四种编译格式:`nested` `expanded` `compact` `compressed`。（**四种编译只是排版不同，根据各自阅读代码的习惯执行！**）
* `--sourcemap` 表示开启 **sourcemap** 调试。开启sourcemap调试后，会生成一个后缀名为.css.map文件。

---

### 入门使用

#### 变量使用

**sass**为*css*引入了变量`（sass的一个重要特性）`。将反复使用的 `css属性值` **定义成变量**，通过*变量名*来引用它们。或者，对于仅使用过 **一 次** 的属性值，可以赋予其一个**易懂的变量名**，让*review*的开发者一眼就知道这个属性值的用途。

**sass**使用 `$` 符号来标识变量(**注意**：老版本的sass使用 `!` 来标识变量。)，比如 `$highlight-color` 和`$sidebar-width`。选择 `$` ，因为在*CSS*中并无他用，不会导致与现存或未来的css语法冲突。

1. **变量声明**

直接展示示例代码解释：
```scss
// sass变量的声明和css属性的声明很像。
$nav-color: #F90;

nav {
  $width: 100px;
  width: $width;
  color: $nav-color;
}
// 与CSS属性不同，变量可以在css规则块定义之外存在。当变量定义在css规则块内，那么该变量只能在此规则块内使用。如果它们出现在任何形式的 {...} 块中，情况也是如此。
```
编译后：
```css
nav {
  width: 100px;
  color: #F90;
}
```
在这段代码中，`$nav-color`这个变量定义在了**规则块外**，所以在*这个样式表*中都可以像 `nav` 规则块那样引用它。`$width`这个变量定义在了`nav`的 `{ }` **规则块内**，所以它只能在`nav`**规则块** **内**使用。这意味着是你可以在样式表的其他地方**再次定义**和**使用相关规则块内** *$width*变量，不会对这里造成影响。

2. **变量引用**

变量可以使用在任何css属性的标准值（*比如说 1px 或者 bold*）存在的地方。css生成时，变量会被它们的值所替代。如果你需要一个不同的值，只需要改变这个变量的值，则所有引用此变量的地方生成的值都会随之改变。
```scss
$highlight-color: #F90;
.selected {
  border: 1px solid $highlight-color;
}
```
编译后：
```css
.selected {
  border: 1px solid #F90;
}
```

**注意**：在声明变量时，**变量值**也可以*引用*其他变量。

下例先为颜色值定义了一个变量，然后在另一个更复杂的边框值上也定义了一个变量：
```scss
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}
```
编译后：
```css
.selected {
  border: 1px solid #F90;
}
```

3. **变量名可以使用*中划线*或者*下划线*分隔**

**sass**的变量名可以与*css*中的`属性名`和`选择器名称`相同，包括*中划线*和*下划线*。

```scss
$link-color: blue;
a {
  color: $link_color;
}
```
编译后：
```css
a {
  color: blue;
}
```

在上例中，`$link-color`和`$link_color`其实指向的是**同一个变量**。实际上，在**sass**的 **大多数地方**，*中划线*命名的内容和*下划线*命名的内容是互通的，除了变量，也包括对混合器和Sass函数的命名。

**注意**：在sass中 **纯css**部分**不互通**，比如类名、ID或属性名。

<br/>

#### 嵌套CSS 规则

在传统css中，如果要对页面中某一块指定样式，需要重复写选择器。sass可以只写一遍，sass在输出css时会把这些嵌套规则处理好且使样式可读性更高。
```scss
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
```
编译后：
```css
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```
上边的例子，编译过程中，sass用了两步，每一步都是像打开俄罗斯套娃那样将里边的嵌套规则块一个个打开。首先，把`#content`（父级）这个*id*放到`article`选择器（子级）和`aside`选择器（子级）的前边，然后`#content article`里边还有嵌套的规则，**sass**重复一遍上边的步骤，把新的选择器添加到内嵌的选择器前边。

**注意**：大多数情况下这种简单的嵌套都没问题，但是有些场景下不行，比如想要在嵌套的 选择器 里边立刻应用一个类似于`：hover`的*伪类*。为了解决这种以及其他情况，sass提供了一个**特殊结构** `&`。

1. **父选择器的标识符 `&`**

一般情况下，sass的嵌套规则是**后代选择器**，但在有些情况下却不会希望sass使用这种后代选择器将所有的属性都被翻译成一般的连接方式。

最常见的一种情况是链接之类的元素写`:hover`这种*伪类*时，并不希望以后代选择器的方式连接。

**错误示例**：
```scss
article a {
  color: blue;
  :hover { color: red }
}
```
这意味着`color: red`这条规则将会被应用到选择器 `article a :hover`，**article元素内**链接的**所有子元素**在被`hover` 时都会变成红色。这是**不正确的**！

使用sass父选择器。在使用嵌套规则时，父选择器能对于嵌套规则如何解开提供更好的控制。它就是 `&` 符号，且可以放在任何一个选择器可出现的地方。

**正确示例**：
```scss
article a {
  color: blue;
  &:hover { color: red }
}
```

当包含父选择器标识符的嵌套规则被打开时，它不会像后代选择器那样进行拼接，而是 `&` 被父选择器直接替换：<br/>
编译后：
```css
article a { color: blue }
article a:hover { color: red }
```

在为父级选择器添加`:hover`等伪类时，这种方式非常有用。<br/>
同时父选择器标识符还有另外一种用法，**可以在父选择器之前添加选择器**。

举例来说，当用户在使用IE浏览器时，可以通过**JavaScript**在`<body>`标签上添加一个ie的类名，为这种情况编写特殊的样式如下：
```scss
#content aside {
  color: red;
  body.ie & { color: green }
}
```
编译后：
```css
#content aside {color: red};
body.ie #content aside { color: green }
```

sass在选择器嵌套上是非常智能的，即使是带有父选择器的情况。当sass遇到群组选择器（由多个逗号分隔开的选择器形成）也能完美地处理这种嵌套。

2. **群组选择器的嵌套**

当需要在一个特定的容器元素内对这样一个群组选择器进行修饰时，传统的css的写法会在群组选择器中的每一个选择器前都重复一遍容器元素的选择器。


sass的嵌套特性在这种场景下，会把每一个内嵌选择器的规则都正确地解出来：

```scss
.container {
  h1, h2, h3 { margin-bottom: .8em }
}
```
编译后：
```css
.container h1, .container h2, .container h3 { margin-bottom: .8em }
```
首先sass将`.container`和`h1` `.container`和`h2` `.container`和`h3`分别组合，然后将三者重新组合成一个群组选择器，生成普通css样式。

对于内嵌在群组选择器内的嵌套规则，处理方式也一样：
```scss
nav, aside {
  a {color: blue}
}
```
编译后：
```css
nav a, aside a {color: blue}
```
sass将`nav`和`a` `aside`和`a`分别组合，然后将二者重新组合成一个群组选择器。

**特别注意**：群组选择器的规则嵌套生成的css。虽然sass让样式表看上去很小，但实际生成的css却可能非常大，这会降低网站的速度。

3. **子组合选择器和同层组合选择器：`>` 、`+` 和 `~`**

`>`、`+`和`~` 这三个**组合选择器**必须和其他选择器配合使用，以指定浏览器仅选择某种特定上下文中的元素。

使用子组合选择器`>`选择一个元素的全部子一级元素。<br/>
示例如下：
* 第一个选择器会选择`article`下的**所有**命中`section`选择器的元素。
* 第二个选择器只会选择`article`下**紧跟着的子元素**中命中`section`选择器的元素。

```scss
article section { margin: 5px }
article > section { border: 1px solid #ccc }
```

使用同层相邻组合选择器`+`选择`header`元素后**紧跟的**`p`元素：
```scss
header + p { font-size: 1.1em }
```

使用同层全体组合选择器`~`，选择所有跟在`article`后的同层`article`元素，不管它们之间隔了多少其他元素：
```scss
article ~ article { border-top: 1px dashed #ccc }
```

这些组合选择器可以应用到sass的规则嵌套中。可以使用在外层选择器后边，或里层选择器前边：
```scss
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
```
sass会将这些嵌套规则一一解开组合在一起：
编译后：
```css
article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```

4. **嵌套属性**

在sass中，除了CSS选择器，属性也可以进行嵌套。

在sass中，嵌套属性的规则是这样的：<br/>
把属性名从**中划线**`-`的地方*断开*，在根属性*后边添加*一个**冒号**`:`，*紧跟*一个`{ }`块，把**子属性部分**写在这个`{ }`块中。<br/>
就像css选择器嵌套一样，sass会把相应的子属性一一解开，把**根属性**和**子属性**部分通过**中划线**`-`*连接*起来，最后生成的效果与传统的css样式一样。

```scss
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
```
编译后：
```css
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

对于属性的缩写形式，甚至可以像下边这样来嵌套，指明例外规则：
```scss
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
```
这比下边这种同等样式的写法要好：
```css
nav {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}
```

<br/>

#### 导入SASS文件

*css*`@import`规则（一个特别不常用的特性），它允许在**一个***css*文件中导入**其他***css*文件。然而，只有在执行到`@import`时，浏览器**才**会去下载其他*css*文件，这导致页面加载起来特别慢。

*sass*`@import`规则，**区别**在于：*sass*的`@import`规则在生成*css*文件时就把相关文件导入进来。这意味着所有相关的样式被归纳到了同一个*css*文件中，而*无*需发起额外的下载请求。另外，所有在**被导入文件**中定义的*变量*和*混合器*均可在导入文件中使用。

1. **使用*SASS*部分文件**

当通过`@import`把*sass*样式分散到多个文件时，通常只想生成少数几个*css*文件。那些专门为`@import`命令而编写的*sass*文件，并不需要生成对应的独立*css*文件，这样的*sass*文件称为局部文件。

*sass*使用一个特殊的约定来命名局部文件。<br/>
此约定即：*sass***局部文件**的文件名以*下划线*`_`开头。这样，*sass*就**不会**在编译时单独编译这个文件输出css，而只把这个文件用作**导入**。当`@import`一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线。<br/>
**举例**：导入`themes/_night-sky.scss`这个局部文件里的变量，只需在样式表中写`@import "themes/night-sky"`。

2. **默认变量值**

代码运行是从上往下，反复声明一个变量时，只有最后一处声明有效且它会覆盖前边的值。

举例说明：
```scss
$link-color: blue;
$link-color: red;
a {
color: $link-color;
}
```
在上边的例子中，超链接的`color`会被设置为*red*。

当一个可被他人通过`@import`导入的*sass*库文件，导入者可能希望定制修改*sass*库文件中的某些值。

使用*sass*的`!default`标签可以实现这个目的。类似于*css*属性中`!important`标签，但是功能却是相反的。不同的是`!default`用于**变量**，含义是：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。
```scss
$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
```
在上例中，如果在导入*sass*局部文件之前声明了一个`$fancybo-width`变量，那么局部文件中对`$fancybox-width`赋值*400px*的操作就**无效**。如果没有做这样的声明，则`$fancybox-width`将**默认**为*400px*。

3. **嵌套导入**

跟原生的css不同，*sass*允许`@import`命令写在css规则内。<br/>
这种导入方式下，生成对应的css文件时，局部文件会被直接插入到css规则内导入它的地方。举例说明，有一个名为*_blue-theme.scss*的局部文件，内容如下：
```scss
aside {
  background: blue;
  color: white;
}
```
然后把它导入到一个CSS规则内，如下所示：
```scss
.blue-theme {@import "blue-theme"}
```
生成的结果跟你直接在`.blue-theme`选择器内写*_blue-theme.scss*文件的内容完全一样。
```scss
.blue-theme {
  aside {
    background: blue;
    color: #fff;
  }
}
```
被导入的局部文件中定义的所有变量和混合器，也会在这个规则范围内生效。这些变量和混合器不会全局有效，这样就可以通过嵌套导入只对站点中某一特定区域运用某种颜色主题或其他通过变量配置的样式。

4. **原生的CSS导入**

**sass**兼容原生的*css*，也支持原生的*CSS*`@import`。尽管通常在**sass**中使用`@import`时，**sass**会尝试找到对应的*sass*文件并导入进来，但在下列三种情况下会生成原生的*CSS*`@import`（这会造成浏览器解析*css*时的额外下载）

* 被导入文件的名字以 `.css` 结尾；
* 被导入文件的名字是一个*URL地址*（比如http://www.sass.hk/css/css.css）（比如使用谷歌字体API提供的相应服务）；
* 被导入文件的名字是*CSS*的 `url()` 值。

**注意**：不能用**sass**的`@import`直接导入一个原始的*css*文件，因为**sass**会执行为使用*css*原生的`@import`。因为**sass**的语法完全兼容*css*，所以可以把原始的`.css后缀文件`改名为`.scss后缀文件`，即可直接导入了。

文件导入是保证*sass*的代码**可维护性**和**可读性**的重要一环。其次非常重要的是注释。

<br/>

#### 静默注释

**sass**提供了一种不同于*css*标准注释格式 `/* ... */` 的注释语法，即**静默注释**，其内容不会出现在生成的css文件中。静默注释的语法跟**JavaScript**、 **Java** 等 类C的语言中单行注释的语法相同，注释以 `//` 开头，注释内容直到行末。

注释示例：
```scss
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```

**注意**：实际上，*css*的标准注释格式 `/* ... */` 内的注释内容亦可在生成的*css*文件中抹去。当注释出现在**原生**css*不允许*的地方（比如 在css属性或选择器中），**sass**将不知如何将其生成到对应css文件中的相应位置，于是这些注释被抹掉。
```
body {
  color /* 这块注释内容不会出现在生成的css中 */: #333;
  padding: 1; /* 这块注释内容也不会出现在生成的css中 */ 0;
}
```

<br/>

#### 混合器

使用变量来统一处理这种情况是非常不错的选择。但是当样式变得越来越复杂，就需要大段的重用样式的代码，独立的变量无法应付这种情况了。这时可以通过**sass**的**混合器**实现大段样式的重用。

**混合器**使用`@mixin`标识符定义。看上去很像其他的*CSS* `@标识符`。这个标识符给一大段样式赋予一个名字，这样你就可以轻易地通过引用这个名字重用这段样式。<br/>
示例*sass*代码，定义了一个混合器，目的是添加跨浏览器的圆角边框。
```scss
@mixin rounded-corners {
  -moz-border-radius: 5px
  ;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```

然后可以在样式表中通过`@include`来使用这个混合器，放在需要的任何地方。<br/>
`@include`调用会把混合器中的**所有样式**提取出来放在`@include`被调用的地方。<br/>
**示例**：
```scss
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
```
*sass*最终生成：
```scss
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```

在`.notice`中的属性`border-radius-moz-border-radius`和`-webkit-border-radius`全部来自`rounded-corners`这个**混合器**。

**注意**：大量的重用可能会导致生成的样式表过大，导致加载缓慢。所以，必须考虑混合器的使用场景，避免滥用。

1. **使用混合器的场景**

利用混合器，可以很容易地在样式表（项目的样式文件，*scss*文件）的不同地方**共享样式**。如果重复一段样式，那就应该把这段样式构造成优良的混合器，尤其是这段样式本身就是一个逻辑单元（比如说是一组放在一起有意义的属性）。

*经验*：判断一组属性是否应该组合成一个混合器，就是能否为这个混合器做出一个的名称。如果一时纠结，这时候构造一个混合器可能并不合适。

混合器是展示性的描述，用来描述一条css规则应用之后会产生怎样的效果。

混合器和类配合使用写出整洁的html和css，因为使用语义化的类名亦可以避免重复使用混合器。为了保持html和css的**易读性**和**可维护性**。

2. **混合器中的*CSS*规则**

混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性，如下代码:
```scss
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
```
当一个包含css规则的混合器通过`@include`包含在一个*父规则*中时，在混合器中的规则最终会生成父规则中的嵌套规则。<br/>
举个例子，这个例子中使用了`no-bullets`这个混合器：
```scss
ul.plain {
  color: #444;
  @include no-bullets;
}
```
*sass*的`@include`指令会将引入混合器的那行代码替换成混合器里边的内容。<br/>
最终，上边的例子如下代码:
```scss
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```
混合器中的规则甚至可以使用*sass*的父选择器标识符`&`。使用起来跟不用混合器时一样，*sass*解开嵌套规则时，用父规则中的选择器替代`&`。

3. **给混合器传参**

混合器并不一定总得生成相同的样式。可以通过在@include混合器时给混合器传参，来定制混合器生成的精确样式。当@include混合器时，参数其实就是可以赋值给css属性值的变量。如果你写过JavaScript，这种方式跟JavaScript的function很像：
```scss
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```
当混合器被@include时，你可以把它当作一个css函数来传参。如果你像下边这样写：
```scss
a {
  @include link-colors(blue, red, green);
}
```
*Sass*最终生成的是：
```scss
a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```
当你@include混合器时，有时候可能会很难区分每个参数是什么意思，参数之间是一个什么样的顺序。为了解决这个问题，sass允许通过语法$name: value的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可：
```scss
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
```
尽管给混合器加参数来实现定制很好，但是有时有些参数我们没有定制的需要，这时候也需要赋值一个变量就变成很痛苦的事情了。所以sass允许混合器声明时给参数赋默认值。