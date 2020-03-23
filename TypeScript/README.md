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

---

### 注意点

* 空白和换行
* * TypeScript 会忽略程序中出现的空格、制表符和换行符。
* * 空格、制表符通常用来缩进代码，使代码易于阅读和理解。

* TypeScript 区分大小写
* * TypeScript 区分大写和小写字符。

* 分号是可选的
* * 每行指令都是一段语句，你可以使用分号或不使用， 分号在 TypeScript 中是可选的，建议使用。

---

### TypeScript 变量声明

#### TypeScript
 变量的命名规则：

1. 变量名称可以包含数字和字母。
2. 除了下划线 _ 和美元 $ 符号外，不能包含其他特殊字符，包括空格。
3. 变量名不能以数字开头。
4. 变量使用前必须先声明，可以使用 **var**，**let**，**const** 来声明变量。

可以使用以下四种方式来声明变量：

1. 声明变量的类型及初始值：
```ts
var [变量名] : [类型] = 值;
```
* 例如：
```ts
var uname:string = "Runoob";
```

2. 声明变量的类型，但没有初始值，变量值会设置为 undefined：
```ts
var [变量名] : [类型];
```
* 例如：
```ts
var uname:string;
```

3. 声明变量并初始值，但不设置类型类型，该变量可以是任意类型：
```ts
var [变量名] = 值;
```
* 例如：
```ts
var uname = "Runoob";
```

4. 声明变量没有设置类型和初始值，类型可以是任意类型，默认初始值为 undefined：
```ts
var [变量名];
```
* 例如：
```ts
var uname;
```

#### 类型断言（Type Assertion）

**类型断言**可以用来**手动指定**一个值的类型，即`允许变量从一种类型更改为另一种类型`。

语法格式：
```ts
<类型>值
```
或:
```ts
值 as 类型
```

**实例**：
```ts
var str = '1'
var str2:number = <number> <any> str // str、str2 是 string 类型，按照最右边最后的类型决定
console.log(str2)
```

编译后，以上代码会生成如下 **JavaScript** 代码：
```js
var str = '1';
var str2 = str; // str、str2 是 string 类型
console.log(str2);
```
执行输出结果为：
> 1
字符串 `'1'`

#### 类型推断

当类型没有给出时，**TypeScript** 编译器利用`类型推断`来推断类型。

如果由于缺乏声明而不能推断出类型，那么它的类型被视作默认的动态 **any** 类型。

```ts
var num = 2; // 类型推断为 number
console.log("num 变量的值为 " + num);
num = "12"; // 编译错误，因为在上方代码 num 优先被推断为了 number 类型
console.log(num);
```

第一行代码声明了变量 num 并=设置初始值为 2。 注意变量声明没有指定类型。因此，程序使用类型推断来确定变量的数据类型，第一次赋值为 2，num 设置为 number 类型。

第三行代码，当我们再次为变量设置字符串类型的值时，这时编译会错误。因为变量已经设置为了 number 类型。

> error TS2322: Type '"12"' is not assignable to type 'number'.

