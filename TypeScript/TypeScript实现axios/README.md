### 学习ts基础

### 注意 var 的提前声明

```js
function f(param) {
  // var x; 其实在这儿默认帮助开发者优先声明了变量
  if(param) {
    var x = 'yes';
  }
  return x;
}

console(f(true));
console(f(false));
```