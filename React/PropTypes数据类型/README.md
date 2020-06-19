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

### 组件特殊属性——propTypes

对Component设置propTypes属性，可以为Component的props属性进行类型检查。


引用方法：import PropTypes from 'prop-types'


PropTypes提供了许多验证工具，用来帮助你确定props数据的有效性。
处于性能原因，类型检查仅在开发模式下进行。

PropTypes的更多验证器

```js
import React from 'react';
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // 利用属性做更多得事
   }
}

MyComponent.propTypes = {
//你可以定义一个属性是特定的JS类型（Array,Boolean,Function,Number,Object,String,Symbol）。默认情况下，这些都是可选的。

optionalArray: PropTypes.array,
optionalBool: PropTypes.bool,
optionalFunc: PropTypes.func,
optionalNumber: PropTypes.number,
optionalObject: PropTypes.object,
optionalString: PropTypes.string,
optionalSymbol: PropTypes.symbol,

//指定类型为：任何可以被渲染的元素，包括数字，字符串，react 元素，数组，fragment。
optionalNode: PropTypes.node,

// 指定类型为：一个react 元素
optionalElement: PropTypes.element,

//你可以类型为某个类的实例，这里使用JS的instanceOf操作符实现
optionalMessage: PropTypes.instanceOf(Message),


//指定枚举类型：你可以把属性限制在某些特定值之内
optionalEnum: PropTypes.oneOf(['News', 'Photos']),

// 指定多个类型：你也可以把属性类型限制在某些指定的类型范围内
optionalUnion: PropTypes.oneOfType([
  PropTypes.string,
  PropTypes.number,
  PropTypes.instanceOf(Message)
]),

// 指定某个类型的数组
optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

// 指定类型为对象，且对象属性值是特定的类型
optionalObjectOf: PropTypes.objectOf(PropTypes.number),


//指定类型为对象，且可以规定哪些属性必须有，哪些属性可以没有
optionalObjectWithShape: PropTypes.shape({
  optionalProperty: PropTypes.string,
  requiredProperty: PropTypes.number.isRequired
}),

// 指定类型为对象，且可以指定对象的哪些属性必须有，哪些属性可以没有。如果出现没有定义的属性，会出现警告。
//下面的代码optionalObjectWithStrictShape的属性值为对象，但是对象的属性最多有两个，optionalProperty 和 requiredProperty。
//出现第三个属性，控制台出现警告。
optionalObjectWithStrictShape: PropTypes.exact({
  optionalProperty: PropTypes.string,
  requiredProperty: PropTypes.number.isRequired
}),

//加上isReqired限制，可以指定某个属性必须提供，如果没有出现警告。
requiredFunc: PropTypes.func.isRequired,
requiredAny: PropTypes.any.isRequired,

// 你也可以指定一个自定义的验证器。如果验证不通过，它应该返回Error对象，而不是`console.warn `或抛出错误。`oneOfType`中不起作用。
customProp: function(props, propName, componentName) {
  if (!/matchme/.test(props[propName])) {
    return new Error(
      'Invalid prop `' + propName + '` supplied to' +
      ' `' + componentName + '`. Validation failed.'
    );
  }
},

//你也可以提供一个自定义的验证器 arrayOf和objectOf。如果验证失败，它应该返回一个Error对象。
//验证器用来验证数组或对象的每个值。验证器的前两个参数是数组或对象本身，还有对应的key。
customArrayProp: PropTypes.arrayOf(function(propValue, key,     componentName, location, propFullName) {
  if (!/matchme/.test(propValue[key])) {
    return new Error(
      'Invalid prop `' + propFullName + '` supplied to' +
      ' `' + componentName + '`. Validation failed.'
    );
  }
})
```