## JS字符串String方法

### 常用方法

1. **concat()** 用于连接两个或多个字符串。
　　
```js
var a = "hello",b = "world",c = "!";
const msg = a.concat(b,c) // 功能和 “+” 拼接一样
console.log(msg) // "helloworld!"
```

<br/>

2. **indexOf()** 返回指定字符串在字符串中首次出现的位置。匹配不到则返回-1。

```js
var a = "helloworld!";
const index1 = a.indexOf("world") 
console.log(index1) // 5
const index2 = a.indexOf("xxxxx") 
console.log(index2) // -1
```

<br/>

3. **search()** 用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。无匹配返回-1。<br/>
str.search(regexp/substr)  返回值：str中第一个与正则或字符串相匹配的子串的起始位置。<br/>
说明 search() 方法不执行全局匹配，它将忽略标志 g。它同时忽略 regexp 的 lastIndex 属性，并且总是从字符串的开始进行检索，这意味着它总是返回 stringObject 的第一个匹配的位置。

```js
var str="Hello World!"
const result1 = str.search(/World/); 
console.log(result1); // 6
const result2 = str.search(/world/); 
console.log(result2); // -1
const result3 = str.search(/world/i); 
console.log(result3); // 6
```

<br/>

4. **match()** 在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。匹配不到返回Null。<br/>
match() 将检索字符串 stringObject，以找到一个或多个与 regexp 匹配的文本。这个方法的行为在很大程度上有赖于 regexp 是否具有标志 g。

> str.match(regExp)  

```
var str = "Hello world!";
const result1 = str.match("world");
console.log(result1); // ["world", index: 6, input: "Hello world!", groups: undefined]
const result2 = str.match("xxxxx");
console.log(result2); // null
```

```js
var str="1+2=3";
const result1 = str.match(/\d+/); 
console.log(result1); // ["1", index: 0, input: "1+2=3", groups: undefined]
const result2 = str.match(/\d+/g); 
console.log(result2); // ["1", "2", "3"]
```

<br/>

5. **replace()** 用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。<br/>
字符串 stringObject 的 replace() 方法执行的是查找并替换的操作。它将在 stringObject 中查找与 regexp 相匹配的子字符串，然后用 replacement 来替换这些子串。如果 regexp 具有全局标志 g，那么 replace() 方法将替换所有匹配的字符串。否则，它只替换第一个匹配子串。<br/>
replacement 可以是字符串，也可以是函数。如果它是字符串，那么每个匹配都将由字符串替换。但是 replacement 中的 $ 字符具有特定的含义。

> str.replace(regexp/substrOld,replaceStrNew) 

```js
var str = "Hello, World";
const result1 = str.replace(/(\w+)\s*, \s*(\w+)/, "$1 $2");
console.log(result1); // Hello World
const result2 = str.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1");
console.log(result2); // World Hello
```

```js
var str = "Hello World!";
const result = str.replace(/world/i, "JavaScript");
console.log(result); // Hello JavaScript!
```

<br/>

6. **split()** 用于把一个字符串分割成字符串数组。<br/>
如果把空字符串 ("") 用作 separator，那么 stringObject 中的每个字符之间都会被分割。

> str.split()

```js
"hello".split("") // ["h", "e", "l", "l", "o"]
var txt = "a,b,c,d,e"; // 字符串
txt.split(","); // ["a", "b", "c", "d", "e"]
txt.split(" "); // ["a,b,c,d,e"]
txt.split("|"); // ["a,b,c,d,e"]
```

<br/>

7. **charAt()** 返回指定位置的字符，长度为1。

> str.charAt(index) 
 
index 为必须参数，类型为number（如果参数 index 不在 0 与 string.length 之间，该方法将返回一个空字符串）
另外：str.charAt()即不带参数和str.charAt(NaN)均返回字符串的第一个字符。

```js
var str = "Hello World!";
str.charAt(8); // r
```

<br/>

8. **charCodeAt()**  返回在指定的位置的字符的 Unicode 编码。这个返回值是 0 - 65535 之间的整数。

> str.charCodeAt(index) 

index 为必须参数，类型为number（0到str.length-1之间，否则该方法返回 NaN）。

```js
var str="Hello world!"
str.charCodeAt(1); // 101
```

<br/>

9. **fromCharCode()** 接受一个或多个指定的 Unicode 值，即要创建的字符串中的字符的 Unicode 编码。，然后返回一个字符串。

> String.fromCharCode(numX,numX,...,numX)

```js
String.fromCharCode(65,66,67); // ABC
```

<br/>

10. **lastIndexOf()**  返回指定字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索。

> str.lastIndexOf(searchStr,startIndex)
  
searchStr 必选，表示需要匹配的字符串值；
startIndex 可选，取值范围 0 到 str.length-1 ，省略则默认尾字符开始检索。

```js
var str="Hello world!";
str.lastIndexOf("Hello"); // O
str.lastIndexOf("World"); // -1
str.lastIndexOf("world"); // 6
```

<br/>

11. **slice()**     提取字符串的某个部分，并以新的字符串返回被提取的部分。

> str.slice(startIndex,endIndex)  // 返回值包含startIndex（必选）不包含endIndex(可选)

```js
var str="Hello World!";
str.slice(6); // World!
```
  
<br/>

12. **substr()** 方法可在字符串中抽取从 start 下标开始的指定数目的字符。

> str.substr(startIndex,length)  //忽略length则返回从startIndex到字符串尾字符

```js
var str="Hello World!";
str.substr(3); // lo World!
```

<br/>

13. **substring()** 方法用于提取字符串中介于两个指定下标之间的字符。

> str.substring(startIndex,endIndex)  // 忽略endIndex则返回从startIndex到字符串尾字符

```js
var str="Hello World!";
str.substring(3, 7); // lo W
```

<br/>

14.  **toLocaleUpperCase()** / **toLocaleLowerCase()**   用于字符串转换大小写（与下面的方法方法仅在某些外国小语种有差别）

```js
var str = "a,b,c,d,e";   // 字符串
console.log(str.toLocaleUpperCase()); // A,B,C,D,E
```

<br/>

15.  **toUpperCase()** / **toLowerCase()**   用于字符串转换大小写

```js
var str = "aBc1".toUpperCase();
console.log(str); // ABC1
```

<br/>


### 扩展方法

**ES2017** 引入了*字符串补全长度*的功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全。

1. **padstart()**

假设希望页面展示的标签彼此正确对齐，以使值在同一位置开始。

例如：
```
        Name: nickName
Phone Number: 130-xxxx-xxxx
```

由于 `Phone Number` 是两个标签中较长的一个，因此需要在 `Name` 标签的开头加上空格。

**注意**：（*临时方法*）为了后续需求的需要，不要将每一项的地府长度专门填充到 `Phone Number` 的长度，把它填充到充足的长度，比如说20个字符。这样一来，如果在后续的需求中需要使用较长的标签，就不必要去修改过多的代码。

原始代码示例：
```js
const label1 = "Name";
const label2 = "Phone Number";
const name = "nickName"
const phoneNumber = "130-xxxx-xxxx";

console.log(label1 + ": " + name);
console.log(label2 + ": " + phoneNumber);

// Name: nickName
// Phone Number: 130-xxxx-xxxx
```

使用 `padStart()` 优化代码：

**注意**：要调用 `padStart()`，需要传递**两个参数**：一个用于填充字符串的目标长度，另一个是希望填充的字符。
```js
const label1 = "Name";
const label2 = "Phone Number";
const name = "nickName"
const phoneNumber = "130-xxxx-xxxx";

console.log(label1.padStart(20, " ") + ": " + name);
console.log(label2.padStart(20, " ") + ": " + phoneNumber);

//               Name: nickName
////     Phone Number: 130-xxxx-xxxx
```

<br/>

2. **padEnd()**

对于相同的标签和值示例，让我们更改填充标签的方式。让我们将标签向左对齐，以便在末尾添加填充。

原始代码示例：
```js
const label1 = "Name";
const label2 = "Phone Number";
const name = "nickName"
const phoneNumber = "130-xxxx-xxxx";

console.log(label1 + ": " + name);
console.log(label2 + ": " + phoneNumber);

// Name: nickName
// Phone Number: 130-xxxx-xxxx
```

使用 `padEnd()` ，并且需要在填充之前将冒号与标签连接起来，这样我们就能确保冒号在正确的位置。

```js
const label1 = "Name";
const label2 = "Phone Number";
const name = "nickName"
const phoneNumber = "130-xxxx-xxxx";

console.log((label1 + ': ').padEnd(20, ' ') + name);
console.log((label2 + ': ').padEnd(20, ' ') + phoneNumber);

// Name:               nickName
// Phone Number:       130-xxxx-xxxx
```

---

**参考资料**
* 博客园
* W3CSCHOOL
* 菜鸟教程
