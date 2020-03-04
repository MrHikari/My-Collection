1. 去除字符串中最后一个**指定**的字符<br/>
**回答：**
```js
function delLast(str,target) {
  const reg = new RegExp(`${target}(?=([^${target}]*)$)`);
  return str.replace(reg, '');
}
```

---

2. 使用一个方法把下划线命名转成大驼峰命名<br/>
**回答：**
```js
function changeStr(str){
  if(str.split('_').length==1) return;
  str.split('_').reduce((a,b) => {
    return a+b.substr(0,1).toUpperCase() + b.substr(1);
  })
}
```
