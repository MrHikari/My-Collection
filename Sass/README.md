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
