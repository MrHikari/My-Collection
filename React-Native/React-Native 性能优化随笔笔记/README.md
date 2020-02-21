#### render次数优化

##### 为什么要减少render次数？

&emsp;&emsp;代码运行了render并不一定会有真实dom的操作，但是一定执行一次dom diff。react大名鼎鼎的diff算法确实高效，参考vitual-dom原理与简单实现，是O(n)的算法。

&emsp;&emsp;需要更新dom时运算diff无可厚非，但是diff之后发现无差别，就是白白耗费了性能，O(n)虽好，做不必要的O(n)也是一件浪费资源的事，因此需要尽可能减少diff次数，也就是减少render次数。

##### 如何减少render的次数？

1. 减少setState的次数

&emsp;&emsp;需要说明的是setState可以是批量的，可能多次setState只会有一次render; 也可以是setState一次就render一次。

* **批量**：在合成事件, **生命周期函数**中的setState是异步批量更新的, 多次setState只会走一次render
* **非批量**：在setTimeOut, setInterval, 原生事件, **Promise**中的setState是同步逐个更新的, 而且每次setState都会走一次render

因此减少setState次数**需要减少**的是**非批量**的情形，在合成事件，比如onClick里边去多次setState是没有关系的。

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

&emsp;&emsp;这样存在的一个问题是第一页会多一次不必要的setState, 因为初始状态一般就是加载中，当你使用Component而且没有重写shouldComponentUpdate时，这会导致一次不必要的render的。改成下边这样就ok了。

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

&emsp;&emsp;第一段代码首屏会有3次render, 第二段代码只有两次。

2. 使用**PureComponent**

&emsp;&emsp;**Component**是每次setState都会去render, 即使你setState的值和之前相同，也会render。

&emsp;&emsp;**PureComponent**重写了**shouldComponentUpdate**，在里边做了一次浅比较，如果setState后新state和旧state相同是不会走render的。

&emsp;&emsp;潜在的问题是当你把state里一个对象的某个属性值改了，由于浅比较是相等的，所以不会走render, 造成显示异常，这个注意下就行。

&emsp;&emsp;**举个例子**：

&emsp;&emsp;还是参照上方的示例，使用**Component**会 render 3次的方案，但是使用PureComponent。

```js
class App extends React.PureComponent{}
```

&emsp;&emsp;可以发现也只render了两次。这是因为我们的那次多余的setState set的是和原来相同的值，**浅比较相等**，所以没有render。

3. 重写shouldComponentUpdate

&emsp;&emsp;这里有一个react生命周期的图, 从图中可以看出更新的时候每次render之前都会走shouldComponentUpdate, 当shouldComponentUpdate返回false的时候就不会render了，因此我们可以在shouldComponentUpdate中合理控制是否render.

&emsp;&emsp;继续参考上方的示例，使用会render 3次的方案。

&emsp;&emsp;重写一下shouldComponentUpdate, 也来做一个浅比较。

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
&emsp;&emsp;发现这样做后也只render了2次。

4. 减少diff代价（这个不是减少render次数的方法）

&emsp;&emsp;将state的尽量放在更小的组件中，这样render时计算diff的成本更小一些.

&emsp;&emsp;**举个例子**：

```js
import React from 'react';
import { Text, Button, View } from 'react-native';

class SubComponent extends React.Component {
  constructor(props, context) {
    super(props, context);
  }

  state = {
    text: 'A',
  };

  onPress = () => {
    this.setState({
      text: 'B',
    });
  };

  render() {
    console.log('SubComponent render');
    return <Button title={this.state.text} onPress={this.onPress} />;
  }
}

class SubComponent2 extends React.Component {
  render() {
    console.log('SubComponent2 render');
    return (
      <Text>F</Text>
    )
  }
}


function FunctionComponent() {
  console.log('FunctionComponent excuted')
  return <Text>G</Text>
}

class App extends React.Component {
  constructor(props, context) {
    super(props, context);
  }

  state = {
    text: 'D',
  }

  onPress = () => {
    this.setState({
      text: 'E'
    })
  }

  render() {
    console.log('App render');
    return (
      <View>
        <Button title={this.state.text} onPress={this.onPress} color="red"/>
        <SubComponent />
        <SubComponent2 />
        <FunctionComponent />
      </View>
    )
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

&emsp;&emsp;若父组件内状态变更，则**它和它所有子组件**都会**render**, 而**子组件的变更**则不会影响到父组件和兄弟组件。因此在实际开发中应该尽量减小state的作用范围，如果一个状态能收敛到组件内部，就不应该放在外边。这样可以**降低render操作的代价**。