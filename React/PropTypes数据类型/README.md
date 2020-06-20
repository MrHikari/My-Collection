## facebook/prop-types

在开发中，程序运行时类型检查 **React**`props` 和 类似的 对象数据。

开发者可以使用 **prop-types** 来记录传递给组件的属性参数（`props`）的**预期类型**。**React**（以及可能的其他库)，将根据这些定义*检查*传递给组件的 `prop`，并在开发不匹配时发出警告。

**总结**：**prop-types**的主要作用：对`props`中数据类型进行*检测*及*限制*

### 安装

> npm install --save prop-types

### 引入使用

在对应文件开头引入
```js
var PropTypes = require('prop-types'); // ES5 with npm
import PropTypes from 'prop-types'; // ES6
```

### 官方示例

`PropTypes`提供了许多验证工具，用来帮助开发者确定 `props` 数据的有效性。
**注意**：出于性能原因，类型检查*仅在*开发模式下进行。

`PropTypes`的很多验证器
```js
import React from 'react';
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // 利用props渲染
   }
}

MyComponent.propTypes = {
// 可以声明一个prop是特定的JS类型（Array,Boolean,Function,Number,Object,String,Symbol）。默认情况下，这些都是可选的。

optionalArray: PropTypes.array,   // 数组
optionalBool: PropTypes.bool,     // 布尔值
optionalFunc: PropTypes.func,     // 函数
optionalNumber: PropTypes.number, // 数字
optionalObject: PropTypes.object, // 对象
optionalString: PropTypes.string, // 字符串
optionalSymbol: PropTypes.symbol, // ES6引入原始类型——Symbol

// 指定类型为：任何可以被渲染的元素，包括数字，字符串，数组，react 元素，fragment。
optionalNode: PropTypes.node,

// 指定类型为：一个 react 元素（只接收一个）
optionalElement: PropTypes.element,

// 可以类型为某个类的实例，这里使用 JS 的 instanceOf 操作符实现
optionalMessage: PropTypes.instanceOf(Message),


// 指定枚举类型：你可以把属性限制在某些 特定值 之内
optionalEnum: PropTypes.oneOf(['News', 'Photos']),

// 指定多个类型：你也可以把属性类型限制在某些 指定的类型范围内
optionalUnion: PropTypes.oneOfType([
  PropTypes.string,
  PropTypes.number,
  PropTypes.instanceOf(Message)
]),

// 指定某个类型的数组
optionalArrayOf: PropTypes.arrayOf(PropTypes.number), // 这里指定 纯数字数组

// 指定类型为对象，且对象属性值是特定的类型
optionalObjectOf: PropTypes.objectOf(PropTypes.number), // 这里指定 属性值为数字 的对象


// 指定类型为对象，且可以规定哪些属性必须传入，哪些属性可以不用（为了避免不必要的错误，非必传的属性请设置默认值）
optionalObjectWithShape: PropTypes.shape({
  optionalProperty: PropTypes.string,
  requiredProperty: PropTypes.number.isRequired
}),

// 指定类型为对象，且可以指定对象的哪些属性必须有，哪些属性没有。如果出现没有定义的属性，会出现警告。
// 下面的代码 optionalObjectWithStrictShape 的属性值为对象，但是对象的属性最多有两个，optionalProperty 和 requiredProperty。
// 出现第三个属性，控制台出现警告。
optionalObjectWithStrictShape: PropTypes.exact({
  optionalProperty: PropTypes.string,
  requiredProperty: PropTypes.number.isRequired
}),

// 加上 isReqired 限制，可以指定某个属性必须提供，没有会出现警告。
requiredFunc: PropTypes.func.isRequired,
requiredAny: PropTypes.any.isRequired,

// 可以指定一个自定义的验证器。如果验证不通过，它应该返回 Error 对象，而不是 `console.warn ` 或 抛出错误。`oneOfType` 中不起作用。
customProp: function(props, propName, componentName) {
  if (!/matchme/.test(props[propName])) {
    return new Error( // 返回一个自定义 error
      'Invalid prop `' + propName + '` supplied to' +
      ' `' + componentName + '`. Validation failed.'
    );
  }
},

// 可以提供一个自定义的验证器 arrayOf 和 objectOf。如果验证失败，它应该返回一个 Error 对象。
// 验证器用来验证数组或对象的每个值。验证器的前两个参数是数组或对象本身，还有对应的key。
customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
  if (!/matchme/.test(propValue[key])) {
    return new Error(
      'Invalid prop `' + propFullName + '` supplied to' +
      ' `' + componentName + '`. Validation failed.'
    );
  }
})
```