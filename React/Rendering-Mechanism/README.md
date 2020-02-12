### React 渲染过程

1. **React**整个的渲染机制就是**React**会调用`render()`函数构建一棵**Dom**树。
2. 在`state`/`props`发生改变的时候，`render()`函数会被再次调用渲染出另外一棵树，重新渲染所有的节点，构造出新的虚拟 **Dom tree** 跟原来的 **Dom tree** 用 **Diff** 算法进行比较，找到需要更新的地方批量改动，再渲染到真实的 **DOM** 上，由于这样做就减少了对 **Dom** 的频繁操作，从而提升的性能。

**_所以在本文会重点介绍下 react render()函数的 react diff 算法_**

### 一、react render()

1、在使用**React**进行构建应用时，我们总会有一个步骤将组建或者 **虚拟 DOM 元素** 渲染到 **真实的 DOM** 上，将任务交给浏览器，进而进行 **layout** 和 **paint** 等步骤，这个函数就是`React.render()`

_ReactComponent render( ReactElement element, DOMElement container, [function callback] )_

![render介绍](https://img-blog.csdnimg.cn/20190318152042239.png)

2、接收 2-3 个参数，并返回 **ReactComponent** 类型的对象，当组件被添加到 **DOM** 中后，执行回调。在这里涉及到了两个 **React** 类型–`ReactComponent`和`ReactElement`

> (JSX 中创建 React 元素最终会被 babel 转译为 createElement(type, config, children),<br/>
> babel 根据 JSX 中标签的首字母来判断是原生 DOM 组件，还是自定义 React 组件)。

##### ReactElement 类型解读

**ReactElement**类型通过函数`React.createElement()`创建，接口定义如下：

```js
ReactElement createElement( string/ReactClass type, [object props], [children …] )
```

1. 第一个参数可以接受字符串（如“p”，“div”等 HTML 的 tag）或 **ReactClass**，
2. 第二个参数为传递的参数，
3. 第三个为子元素，**ReactElement** 有**4**个属性：**type**，**ref**，**key**，**props**，并且轻量，没有状态，是一个**虚拟化的 DOM**元素
4. `createElement()`接收三个参数（**type**，**config**，**children**），做了一些变量初始化，接着调用了`ReactElement()`方法。
5. `ReactElement()`是一个工厂方法，根据传入的参数返回一个**element**对象

[源码流程图参考](https://blog.csdn.net/u012937029/article/details/76696489)

3、进入页面 render()执行了几次

`render`在`componentWillMount`后会执行一次，会在 **props** 及 **state** 改变时执行。

1. 首次加载
2. `setState`改变组件内部**state**。 注意： 此处是说**通过 setState 方法改变**。
3. 接受到新的**props**

### 二、Diff 算法

**react**的一大特点就是**虚拟 DOM**的 **diff 算法** ，下图为**diff**实现流程图。

![diff流程图](https://img-blog.csdnimg.cn/2019031815332182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hlbGxlWmhhaQ==,size_16,color_FFFFFF,t_70)

&emsp;&emsp;**diff 算法** 的特点如下图：**传统 diff 算法**的复杂度为 `O(n^3)`，单纯从 demo 看，复杂度不到 n3，但实际上。 **React** 通过制定大胆的策略，将 `O(n^3)` **复杂度**的问题转换成 `O(n)` **复杂度**的问题

![diff算法特点](https://img-blog.csdnimg.cn/20190318153333528.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hlbGxlWmhhaQ==,size_16,color_FFFFFF,t_70)

1. 在 **React** 中，两棵**DOM**树只会对同一层的节点进行比较，若发现节点已不存在，则该节点及其子节点会被完全删除，不会用于进一步的比较。这样，只需要对树进行一次遍历，就能完成整个**DOM**树的比较
2. 对于同层节点，**React**在数组遍历的增减关键字**Key**,若节点本身完全相同(类型相同，属性相同)，只是位置不同，则**React**只需要考虑同层节点的位置变换，不需要进行节点的销毁和重新创建
   ![DOM树对比](https://img-blog.csdnimg.cn/20190318153756186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hlbGxlWmhhaQ==,size_16,color_FFFFFF,t_70)
3. 对于不同层的节点，只能销毁和重新创建
   ![DOM树对比和重建](https://img-blog.csdnimg.cn/20190318153654344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pY2hlbGxlWmhhaQ==,size_16,color_FFFFFF,t_70)

4. 节点类型不同:<br/>
   当在树中的同一位置前后的节点类型不同，**React**会直接删除原节点，然后创建并插入新的节点。
   注意：删除节点即彻底销毁该节点，也就是说，后续不会查找是否有另外一个节点等同于删除的该节点。如果删除的该节点有子节点，那么子节点也会被删除。这也是 **diff 算法** 复杂度能降到 `O(n)` 的原因。
   同理，当树的同一个位置遇到前后不同的组件时，也是销毁原组件，把新的组件加上去。这应用了第一个假设，不同的组件一般会产生不同的 **DOM** 结构，与其浪费时间去比较不同的 **DOM** 结构，还不如完全创建一个新的组件加上去。
5. 节点类型相同，但是属性不同:<br/>
   React 会对属性进行重设从而实现节点的转换
6. 节点类型相同且属性相同<br/>
   对于同层节点，若节点本身完全相同（类型相同且属性相同），只是位置不同，则**React**只需要考虑同层节点的位置变换，不需要进行节点的销毁和重新创建，这就需要用到下面介绍的 **key** 属性。
   对于不同层的节点，即使节点本身完全相同（类型相同且属性相同），也只能销毁和重新创建。
7. **key**属性<br/>
   为列表节点提供唯一的**key**属性，可以帮助**React**定位到正确的节点进行比较，从而大幅减少**DOM**操作的次数，提高性能

**Diff 总结**

- 通过**diff**策略，将算法从`O(n^3)`简化为`O(n)`
- 分层求异，对 **tree diff** 进行优化
- 分组件求异，相同类生成相似树形结构、不同类生成不同树形结构，对 **component diff** 进行优化
- 设置**key**，对 **element diff** 进行优化
- 尽量保持稳定的 DOM 结构、避免将最后一个节点移动到列表首部、避免节点数量过大或更新过于频繁
