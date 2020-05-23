#### render 次数优化

##### 为什么要减少 render 次数？

&emsp;&emsp;代码运行了 render 并不一定会有真实 dom 的操作，但是一定执行一次 dom diff。react 大名鼎鼎的 diff 算法确实高效，参考 vitual-dom 原理与简单实现，是 O(n)的算法。

&emsp;&emsp;需要更新 dom 时运算 diff 无可厚非，但是 diff 之后发现无差别，就是白白耗费了性能，O(n)虽好，做不必要的 O(n)也是一件浪费资源的事，因此需要尽可能减少 diff 次数，也就是减少 render 次数。

##### 如何减少 render 的次数？

1. 减少 setState 的次数

&emsp;&emsp;需要说明的是 setState 可以是批量的，可能多次 setState 只会有一次 render; 也可以是 setState 一次就 render 一次。

- **批量**：在合成事件, **生命周期函数**中的 setState 是异步批量更新的, 多次 setState 只会走一次 render
- **非批量**：在 setTimeOut, setInterval, 原生事件, **Promise**中的 setState 是同步逐个更新的, 而且每次 setState 都会走一次 render

因此减少 setState 次数**需要减少**的是**非批量**的情形，在合成事件，比如 onClick 里边去多次 setState 是没有关系的。

&emsp;&emsp; **举个例子**：

&emsp;&emsp;通常写一个上拉加载的列表会像下边这样,每次请求开始时将状态置为加载中。但是可能进页面其实就是加载中的状态了。

```js
 getList(pageNum) {
    this.setState({
      status: 1, // 1 loading, 2 success, 3 error
    });
    fetchList(pageNum)
      .then(res => {
        this.setState({
          data: [...this.statedata, ...res],
        });
      })
      .catch(e => {
        console.log(e);
      });
  }
```

&emsp;&emsp;这样存在的一个问题是第一页会多一次不必要的 setState, 因为初始状态一般就是加载中，当你使用 Component 而且没有重写 shouldComponentUpdate 时，这会导致一次不必要的 render 的。改成下边这样就 ok 了。

```js
  getList(pageNum) {
    // 加上这样一个判断
    if (this.state.status !== 1) {
      this.setState({
        status: 1,
      });
    }
    fetchList(pageNum)
      .then(res => {
        this.setState({
          data: [...this.state.data, ...res],
        });
      })
      .catch(e => {
        console.log(e);
      });
  }
```

&emsp;&emsp;第一段代码首屏会有 3 次 render, 第二段代码只有两次。

2. 使用**PureComponent**

&emsp;&emsp;**Component**是每次 setState 都会去 render, 即使你 setState 的值和之前相同，也会 render。

&emsp;&emsp;**PureComponent**重写了**shouldComponentUpdate**，在里边做了一次浅比较，如果 setState 后新 state 和旧 state 相同是不会走 render 的。

&emsp;&emsp;潜在的问题是当你把 state 里一个对象的某个属性值改了，由于浅比较是相等的，所以不会走 render, 造成显示异常，这个注意下就行。

&emsp;&emsp;**举个例子**：

&emsp;&emsp;还是参照上方的示例，使用**Component**会 render 3 次的方案，但是使用 PureComponent。

```js
class App extends React.PureComponent {}
```

&emsp;&emsp;可以发现也只 render 了两次。这是因为我们的那次多余的 setState set 的是和原来相同的值，**浅比较相等**，所以没有 render。

3. 重写 shouldComponentUpdate

&emsp;&emsp;这里有一个 react 生命周期的图, 从图中可以看出更新的时候每次 render 之前都会走 shouldComponentUpdate, 当 shouldComponentUpdate 返回 false 的时候就不会 render 了，因此我们可以在 shouldComponentUpdate 中合理控制是否 render.

&emsp;&emsp;继续参考上方的示例，使用会 render 3 次的方案。

&emsp;&emsp;重写一下 shouldComponentUpdate, 也来做一个浅比较。

```js
  shouldComponentUpdate(nextProps, nextState) {
    for (let key in nextState) {
      if (nextState[key] !== this.state[key]) {
        return true;
      }
    }
    for (let key in nextProps) {
      if (nextProps[key] !== this.props[key]) {
        return true;
      }
    }
    return false;
  }
```

&emsp;&emsp;发现这样做后也只 render 了 2 次。

4. 减少 diff 代价（这个不是减少 render 次数的方法）

&emsp;&emsp;将 state 的尽量放在更小的组件中，这样 render 时计算 diff 的成本更小一些.

&emsp;&emsp;**举个例子**：

```js
import React from "react";
import { Text, Button, View } from "react-native";

class SubComponent extends React.Component {
  constructor(props, context) {
    super(props, context);
  }

  state = {
    text: "A"
  };

  onPress = () => {
    this.setState({
      text: "B"
    });
  };

  render() {
    console.log("SubComponent render");
    return <Button title={this.state.text} onPress={this.onPress} />;
  }
}

class SubComponent2 extends React.Component {
  render() {
    console.log("SubComponent2 render");
    return <Text>F</Text>;
  }
}

function FunctionComponent() {
  console.log("FunctionComponent excuted");
  return <Text>G</Text>;
}

class App extends React.Component {
  constructor(props, context) {
    super(props, context);
  }

  state = {
    text: "D"
  };

  onPress = () => {
    this.setState({
      text: "E"
    });
  };

  render() {
    console.log("App render");
    return (
      <View>
        <Button title={this.state.text} onPress={this.onPress} color="red" />
        <SubComponent />
        <SubComponent2 />
        <FunctionComponent />
      </View>
    );
  }
}

export default App;
```

&emsp;&emsp;点红色按钮执行结果如下

```js
App render
App.js:20 SubComponent render
App.js:27 SubComponent2 render
App.js:36 FunctionComponent excuted
```

&emsp;&emsp;蓝色按钮执行结果如下

```js
SubComponent render
```

&emsp;&emsp;若父组件内状态变更，则**它和它所有子组件**都会**render**, 而**子组件的变更**则不会影响到父组件和兄弟组件。因此在实际开发中应该尽量减小 state 的作用范围，如果一个状态能收敛到组件内部，就不应该放在外边。这样可以**降低 render 操作的代价**。

---

#### 报错解决
```
warning: React does not recognize the xxx prop on a DOM element......
```
这是 **React**不能识别 dom元素 上的非标准 attribute 报出的警告，最终的渲染结果中React会移除这些非标准的 **attribute**。

通常 `{...this.props}` 和 `cloneElement(element, this.props)` 这两种写法，会将父级别无用的 **attribute** 传递到子级的dom元素上。