转自 https://www.cnblogs.com/fecommunity/p/11922109.html *Web前端社区*

### 1. 说一下使用jQuery和使用框架的区别？
* 数据和视图的分离，（jQuery数据和视图混在一起，代码耦合）-------开放封闭原则
* 以数据驱动视图（只关注数据变化，DOM操作被封装）

### 2.说一下对MVVM的理解？
* MVC：Model, View, Controller（主要用于后端）
* MVVM: Model, View, ViewModel[中间连接者，连接view和和model]

#### 关于ViewModel

* ##### 2.2.1 MVVM在React中对应关系

1. M(odel)：对应组件的方法或生命周期函数中实现的业务逻辑和this.state中保存的本地数据，如果React集成了redux +react-redux，那么组件中的业务逻辑和本地数据可以完全被解耦出来单独存放当做M层，如业务逻辑放在Reducer和Action中。
2. V(iew)-M(odel)：对应组件中的JSX，它实质上是Virtual DOM的语法糖。React负责维护 Virtual DOM以及对其进行diff运算，而React-dom 会把Virtual DOM渲染成浏览器中的真实DOM
3. View：对应框架在浏览器中基于虚拟DOM生成的真实DOM（并不需要我们自己书写）以及我们书写的CSS
4. 绑定器：对应JSX中的命令以及绑定的数据，如`className={ this.props.xxx }`、`{this.props.xxx}`等等

* ##### 2.2.2 MVVM的双绑和单绑区别

1. 一般，只有UI表单控件才存在双向数据绑定，非UI表单控件只有单向数据绑定。
2. 单向数据绑定是指：M的变化可以自动更新到ViewModel，但ViewModel的变化需要手动更新到M（通过给表单控件设置事件监听）
3. 双向数据绑定是指念：M的变化可以自动更新到ViewModel，ViewModel的变化也可以自动更新到M
4. 双向绑定 = 单向绑定 + UI事件监听。双向和单向只不过是框架封装程度上的差异，本质上两者是可以相互转换的。
5. 优缺点：在表单交互较多的情况下，单向数据绑定的优点是数据更易于跟踪管理和维护，缺点是代码量较多比较啰嗦，双向数据绑定的优缺点和单向绑定正好相反。

### 3.说一下对组件化的理解？

#### 组件的封装
* a. 视图的封装
* b. 数据的封装
* c. 变化逻辑（数据驱动视图变化，setState）

#### 组件的复用
* a. 使用props来传递数据（同一个组件传递不同飞数据）
* b. 组件的复用（同一个组件使用不同的数据）

### 4.JSX的本质是什么？

* JSX语法（标签、JS表达式，判断，循环，事件绑定）
* JSX是语法糖, 需要被解析成JS才能运行（h函数的使用）
* JSX是独立的标准，可以被其他项目使用
```js
   // 下面的代码实际执行流程：
    // JSX 代码
    const user = {
        firstName : 'xiugang',
        lastName : 'zhang'
    }
    var profile = <div>
        <img src="a.jpg" className='profile'/>
        <h3>{[user.firstName, user.lastName].join(' ')}</h3>
    </div>

    // 解析结果(重点掌握)，关键点：是使用了一个React.createElement来创建节点的
    var profile = React.createElement('div', null, [
        React.createElement('img', {src : 'a.jpg', className : 'profile'}),
        React.createElement('h3', null, [[user.firstName, user.lastName].join(' ')])
])

/* @jsx h*/
// 1. 使用插件：cnpm install babel-plugin-transform-react-jsx
// 2. 开始编译JSX： babel --plugins transform-react-jsx demo.js
// 3. 可以自定义React.createElement变为一个h函数： /* @jsx h*/
```

### 5.JSX和VDOM的关系？
#### 5.1 分析为何需要VDOM
* VDOM是React初次推广开来的，结合JSX
* JSX就是模板渲染成HTML
* 初次渲染 + 修改state之后的re-render
* 正好符合VDOM的应用场景

#### 5.2 React.createElement和h函数
#### 5.3 何时patch？
* 初次渲染----`ReactDOM.render(<App/>, container)`
* 会触发patch(container, vnode)函数
* `re-render-- setState`
* 会触发`patch(vNode, newVNode)`

#### 5.4 自定义组件的解析？

##### 5.4.1 自定义组件的解析（TODOInput和TODOList组件的解析）

* ‘div’可以直接渲染即可，vdom可以实现
* TodoInput和TodoList是自定义组件（class），vdom不认识
* 因此Input和List定义的时候必须声明render函数
* 根据props属性初始化实例，然后执行实例的render函数
* render函数返回的还是vnode的对象

```js
React.createElement(TodoInput, { addTitle: this.addTitle.bind(this) }),
    React.createElement(TodoList, { data: this.state.list })

    // 上面的代码相当于是先去创建一个TodoList实例对象
    var list = new TodoList({ data: this.state.list });
    // 然后再去调用这个函数的render方法（返回的是一个JSX，然后对当前的这个JSX渲染为VDOM）
    var vnode = list.render();
```

### 6.说一下setState的过程？

```js
    // 1. 每个组件实例，都有renderComponent方法
    class Component {
        constructor(){

        }

        // 每个组件都有这个函数
        renderComponent(){
            // 获取上一次的vNode
            const prevVnode = this._vnode;

            // render函数只需之后，得到的还是一个新的node
            const newVnode = this.render();

            // 开始对比，找出差异
            patch(prevVnode, newVnode);

            // 更新node为最新的node
            this._vnode = newVnode;
        }
    }

    // 2. 执行renderComponent会重新执行实例的render方法
    // 3. render函数返回newVnode，然后拿到prevNode（也就是上次的vnode）----多次执行setState视图最终也只会渲染一次
    // 4. 执行patch（preVnode, newVNode）
```

* setState通过一个队列机制实现state更新，当执行setState时，会将需要更新的state很后放入状态队列，而不会立即更新this.state，队列机制可以高效地批量更新state。如果不通过setState而直接修改this.state的值
* 那么该state将不会被放入状态队列中。当下次调用setState并对状态队列进行合并时，就会忽略之前修改的state，造成不可预知的错误
* 同时，也利用了队列机制实现了setState的异步更新，避免了频繁的重复更新state

### 7.阐述自己对React和Vue的认识？
#### 7.1 两者的本质区别
* vue本质是MVVM框架，由MVC发展而来
* React本质是前端组件化框架，由后端组件化发展而来
* 并不妨碍两者都能实现相同的功能

#### 7.2 模板的区别
* vue-使用模板（最初由angular提出）
* React-使用JSX
* 模板语法上，更倾向于JSX
* 模板分离上，更倾向于Vue
* 模板应该和JS逻辑分离
* “开放封闭原则”

#### 7.3 组件化的区别
* React本身就是组件化，没有组件化就不是React
* vue也支持组件化，不过是在MVVM上的扩展
* 查阅vue组件化的文档
* 组件化更适合选择React

#### 7.4 两者的共同点
* 都支持组件化
* 都是数据驱动视图

#### 7.5. 如何选择
* 国内使用，首推vue。文档更易读，易学，社区够大
* 如果团队水平较高，推荐使用React，组件化和JSX