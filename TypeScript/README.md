## TypeScript

**TypeScript** 是 `JavaScript` 的一个**超集**，支持 `ECMAScript 6` 标准。

**TypeScript** 是由微软开发的自由和开源的编程语言。

**TypeScript** 设计目标是开发大型应用，它可以**编译**成`纯 JavaScript`，编译出来的 `JavaScript` 可以运行在任何浏览器上。

---

### JavaScript 与 TypeScript 的区别

**TypeScript** 是 `JavaScript` 的超集，扩展了 `JavaScript` 的语法，因此现有的 `JavaScript` 代码可与 **TypeScript** 一起工作无需任何修改，**TypeScript** 通过**类型注解**提供编译时的**静态类型检查**。

**TypeScript** 可处理已有的 `JavaScript` 代码，并**只**对其中的 **TypeScript** 代码进行**编译**。

---

### TypeScript 安装

#### NPM 安装 TypeScript

如果本地环境已经安装了 **npm** 工具，可以使用以下命令来安装：<br/>
全局安装 `TypeScript`
> npm install -g typescript

安装完成后可以使用 **tsc** 命令来执行 `TypeScript` 的相关代码，以下是查看版本号：
> $ tsc -v<br/>
> Version 3.2.2

假设新建一个 test.ts 的文件，代码如下：
```ts
var message:string = "Hello World" 
console.log(message)
```

通常使用 `.ts` 作为 `TypeScript` 代码文件的扩展名。

然后执行以下命令将 `TypeScript` 转换为 `JavaScript` 代码：

> tsc test.ts

这时候再当前目录下（与 `test.ts` **同一目录**）就会生成一个 `test.js` 文件，代码如下：
```js
var message = "Hello World";
console.log(message);
```
使用 **node** 命令来执行 `test.js` 文件：

> $ node test.js<br/>
> Hello World