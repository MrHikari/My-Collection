### classnames

一个简单的**JavaScript**模块，用于有条件地拼接和使用多个 ***className***。

使用npm，Bower或Yarn安装：

```
# npm 安装
npm install classnames

# Bower 安装
bower install classnames

# Yarn 安装 (请注意，它将自动将包保存到 `package.json` 中的 `dependencies`中`)
yarn add classnames
```

可以与 **Node.js**，**Browserify** 或 **webpack** 一起使用：
示例：
```js
var classNames = require('classnames'); 
classNames('foo', 'bar');  // =>'foo bar'
```

或者，可以简单地在 `index.js` 文件上包含一个独立 `<script>` 标签，它将导出全局classNames方法；或者如果使用***RequireJS***，则可以定义模块。

___

#### 用法

`classNames` 接受*任意数量*的参数，这些参数可以是字符串或对象。

示例：
```js
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'

// lots of arguments of various types 许多对应不同类型的参数
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// other falsy values are just ignored 忽略假值
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
```

**数组**将按照上述规则*递归展开*：
```js
var arr = ['b', { c: true, d: false }];
classNames('a', arr); // => 'a b c'
```

**ES2015** 中 动态类名
如果所在的环境支持计算键名（**ES2015**和**Babel**中提供），则可以使用动态类名：
```js
let buttonType = 'primary';
classNames({ [`btn-${buttonType}`]: true });
```
___

#### React.js 中的用法
`classSet`正式替代品，最初是在`React.js` ***Addons***捆绑包中提供的。

使动态和条件`className`更易于使用（尤其是比条件字符串操作更简单）。
示例：
```js
class Button extends React.Component {
  // ...
  render () {
    var btnClass = 'btn';
    if (this.state.isPressed) btnClass += ' btn-pressed';
    else if (this.state.isHovered) btnClass += ' btn-over';
    return <button className={btnClass}>{this.props.label}</button>;
  }
}
```

可以将条件性的类更简洁地表现成一个对象。
示例：
```js
var classNames = require('classnames');

class Button extends React.Component {
  // ...
  render () {
    var btnClass = classNames({
      btn: true,
      'btn-pressed': this.state.isPressed,
      'btn-over': !this.state.isPressed && this.state.isHovered
    });
    return <button className={btnClass}>{this.props.label}</button>;
  }
}
```

可以混合使用对象，数组和字符串参数，支持 `className` *props*参数。
示例：
```js
var btnClass = classNames('btn', this.props.className, {
  'btn-pressed': this.state.isPressed,
  'btn-over': !this.state.isPressed && this.state.isHovered
});
```