为了大型项目需要，只凭借单纯的 React，是无法满足 **快速高效** 开发。
**React** 轻量级的`视图层框架`。**Redux** 是目前 React 生态圈中，最好的`数据层框架`。

**注意：** **Redux** 是一个 **JavaScript** 应用工具。

#### Redux 示意

![Redux 示意图](https://github.com/MrHikari/My-Collection/blob/master/%E5%9B%BE%E7%89%87%E6%9A%82%E5%AD%98/Redux%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)



#### 安装 Redux

在项目目录下安装
> npm isntall --save redux

#### 生成 store

**基础示例：**<br/>
项目路径下创建一个文件，或者在选择一个路径下创建一个文件
```js
import { createStore } from 'redux'; // 从redux引入创建store方法
import reducer from 'reducer的相对路径'; // 在创建完reducer.js后引入
const store = createStore();
export default store; // 将生成的store暴露出去
```

新建文件 `reducer.js`
```js
const defaultState={}; // 模拟文件中预存的数据
export default (state = defaultState, action) => {
  return state;
}
```