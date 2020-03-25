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
全局安装 `TypeScript`，如果报错或者没有反应，请翻墙或者切换移动热点，并且加上`sudo`
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

#### 变量作用域

**变量作用域**指 定了变量定义的位置。

程序中 变量的可用性 由 变量作用域 决定。

**TypeScript** 有以下几种作用域：

* **全局作用域** − 全局变量定义在程序结构的外部，它可以在你代码的任何位置使用。
* **类作用域** − 这个变量也可以称为 字段。类变量声明在一个类里头，但在类的方法外面。 该变量可以通过类的对象来访问。类变量也可以是静态的，静态的变量可以通过类名直接访问。
* **局部作用域** − 局部变量，局部变量只能在声明它的一个代码块（如：方法）中使用。

以下实例说明了三种作用域的使用：
```ts
var global_num = 12 // 全局变量
class Numbers {
   num_val = 13; // 实例变量
   static sval = 10; // 静态变量
   
   storeNum():void {
      var local_num = 14; // 局部变量
   } 
} 
console.log("全局变量为: " + global_num)
console.log(Numbers.sval) // 静态变量
var obj = new Numbers();
console.log("实例变量: " + obj.num_val)
```
以上代码使用 tsc 命令编译为 JavaScript 代码为：
```js
var global_num = 12; // 全局变量
var Numbers = /** @class */ (function () {
    function Numbers() {
        this.num_val = 13; // 类变量
    }
    Numbers.prototype.storeNum = function () {
        var local_num = 14; // 局部变量
    };
    Numbers.sval = 10; // 静态变量
    return Numbers;
}());
console.log("全局变量为: " + global_num);
console.log(Numbers.sval); // 静态变量
var obj = new Numbers();
console.log("类变量: " + obj.num_val);
```
执行以上 JavaScript 代码，输出结果为：

> 全局变量为: 12<br/>10<br/>类变量: 13

如果我们在方法外部调用局部变量 local_num，会报错：

> error TS2322: Could not find symbol 'local_num'.

---

### TypeScript 运算符

参考 [菜鸟教程](https://www.runoob.com/typescript/ts-operators.html)

#### ~ 按位取反

`~`是按位取反运算符

二进制在内存的存储：二进制数在内存中以补码的形式存储

另外，正数的原码、补码和反码都相同

负数的反码与原码符号位相同，数值为取反；补码是在反码的基础上加1

比如：

`~9`的计算步骤：

1. 转二进制：0 1001
2. 计算补码：0 1001
3. 按位取反：1 0110
4. 转为原码：1 0110
5. 按位取反：1 1001 // 反码
6. 末位加一：1 1010 // 补码

* 符号位为1是负数，即-10

**规律**：`~x=-（x+1）`；

因此，`t=~9`（1001）并不能输出 6（0110），而是 -10。

---

### TypeScript 条件语句

条件语句用于**基于不同的条件**来**执行不同的动作**。

`TypeScript` 条件语句是通过一条或多条语句的执行结果（**True** 或 **False**）来决定执行的代码块。

#### 条件语句

通常在项目代码中，需要通过不同的条件来执行不同的操作。在代码中使用条件语句来完成该任务。

在 **TypeScript** 中，可以使用以下条件语句：

* `if 语句` - 只有当指定条件为 **true** 时，使用该语句来执行代码
* `if...else 语句` - 当条件为 **true** 时执行代码，当条件为 **false** 时执行其他代码
* `if...else if....else 语句` - 使用该语句来选择多个代码块之一来执行
* `switch 语句` - 使用该语句来选择多个代码块之一来执行

**switch 语句示例**
```ts
switch(expression){
    case expression1 :
       statement(s);
       break; /* 可选的 */
    case expression2:
       statement(s);
       break; /* 可选的 */

    /* 您可以有任意数量的 case 语句 */
    default : /* 可选的 */
       statement(s);
}
```

**switch** 语句**必须**遵循下面的规则：

* `switch 语句`中的 `expression` 是一个常量表达式，必须是一个整型或枚举类型。
* 一个 switch 中可以有**任意数量**的 `case 语句`。每个 case 后跟一个要比较的值和一个冒号。
* case 的 expression1, expression2 等等 必须与 switch 中的变量具有相同的数据类型，且必须是一个常量或字面量。
* 当被测试的变量等于 case 中的常量时，case 后跟的语句将被执行，直到遇到 `break 语句` 为止。
* 当遇到 break 语句时，switch **终止**，控制流将跳转到 switch 语句后的下一行。
* 不是每一个 case 都需要包含 break。如果 case 语句不包含 break，控制流将会 继续 后续的 case，直到遇到 break 为止。
* 一个 switch 语句可以有一个可选的 `default case`，出现在 switch 的**结尾**。`default case` 可用于在上面所有 case 都不为真时执行一个任务。`default case` 中的 `break 语句` **不是必需**的。

---

### TypeScript 循环

#### for 循环

`TypeScript` **for** 循环用于多次执行一个语句序列，简化管理循环变量的代码。

语法格式如下所示：
```TS
for ( init; condition; increment ){
    statement(s);
}
```

下面是 for 循环的控制流程解析：

* `init` 会首先被执行，且只会执行一次。这一步允许声明并初始化任何循环控制变量。也可以不在这里写任何语句，只要有一个分号出现即可。
* 接下来，会判断 `condition`。如果为 **true**，则执行循环主体。如果为 **false**，则不执行循环主体，**且控制流会跳转到紧接着 for 循环的下一条语句**。
* 在执行完 for 循环主体后，控制流会跳回上面的 `increment` 语句。该语句允许更新循环控制变量。该语句可以留空，只要在条件后有一个分号出现即可。
* 条件再次被判断。如果为 **true**，则执行循环，这个过程会不断重复（循环主体，然后增加步值，再然后重新判断条件）。在条件变为 **false** 时，for 循环终止。
在这里，statement(s) 可以是一个单独的语句，也可以是几个语句组成的代码块。

`condition` 可以是任意的表达式，当条件为 **true** 时执行循环，当条件为 **false** 时，退出循环。

for 循环 参考流程图：<br/>
![]()

#### for...in 循环

`for...in` 语句用于**一组值的集合**或**列表**进行迭代输出。

语法格式如下所示：
```ts
for (var val in list) { 
    // 语句
}
```

`val` 需要为 **string** 或 **any** 类型。

#### for...of 、forEach、every 和 some 循环

**TypeScript** 还支持 `for...of` 、`forEach`、`every` 和 `some` 循环。

`for...of` 语句创建一个循环来迭代可迭代的对象。在 **ES6** 中引入的 `for...of` 循环，以替代 `for...in` 和 `forEach()` ，并支持新的迭代协议。`for...of` 允许你遍历 Arrays（数组）, Strings（字符串）, Maps（映射）, Sets（集合）等可迭代的数据结构等。

1. **TypeScript** `for...of` 循环
```ts
let someArray = [1, "string", false];

for (let entry of someArray) {
  console.log(entry); // 1, "string", false
}
```

`forEach`、`every` 和 `some` 是 **JavaScript** 的循环语法，**TypeScript** 作为 **JavaScript** 的语法超集，当然默认也是支持的。

**注意**：因为 `forEach` 在 **iteration** 中是无法返回的，所以可以使用 `every` 和 `some` 来取代 `forEach`。

2. **TypeScript** `forEach` 循环
```ts
let list = [4, 5, 6];
list.forEach((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
});
```

3. **TypeScript** `every` 循环
```ts
let list = [4, 5, 6];
list.every((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
    return true; // Continues
    // Return false will quit the iteration
});
```
