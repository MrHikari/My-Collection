## Less

[Less官网](http://lesscss.org/)<br/>
[Less中文网](http://lesscss.cn/)

### CSS 预处理 Less

`Less` 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。

实际上 less 需要 node.js 编译

#### 安装 Less

全局安装 Less
> npm install -g less

#### 浏览器端用法

首先**必须**在项目中放入 `less.min.js` 。

其次，在页面文件中引入该 `less.min.js`，并且所写的less样式要指定好**type**，`type="text/less"`

```html
<!-- set options before less.js script -->
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
<script src="less.js"></script>
```

这种方法属于 **运行时编译**

