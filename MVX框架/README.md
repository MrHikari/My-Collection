### MVX框架模式：MVC，MVP，MVVM

1. **MVC**：**Model**(`模型`) + **View**(`视图`) + **controller**(`控制器`)，主要是基于分层的目的，让彼此的职责分开。

**View**通过**Controller**来和**Model**联系，**Controller**是**View**和**Model**的协调者，**View**和**Model**不直接联系，基本联系都是单向的。

`用户User`通过`控制器Controller`来操作`模板Model`从而达到`视图View`的变化。

MVC模式最早是由Trygve Reenskaug在1978年提出的，是 帕罗奥多研究中心（Xerox PARC）在20世纪80年代为程序语言Smalltalk 发明的一种软件架构。MVC模式的目的是实现一种动态的程序设计，使后续对程序的修改和扩展简化，并且使程序某一部分的重复利用成为可能。除此之外，此模式通过对复杂度的简化，使程序结构更加直观

MVC最重要的是将原本杂乱的代码层级化，将原本杂乱的代码分为 展示层（View,Controller）和领域层(Model),在这时，领域层中的领域对象由于其自身特点不需要对展示层有任何了解，可以同时为不同的展示层工作。

2. **MVP**：是从`MVC模式`演变而来的，都是通过 **Controller**/**Presenter**负责逻辑的处理 + **Model**提供数据 + **View**负责显示。

在**MVP**中，**Presenter**完全把**View**和**Model**进行了分离，主要的程序逻辑在**Presenter**里实现。

并且，**Presenter**和**View**是没有直接关联的，是通过定义好的接口进行交互，从而使得在变更**View**的时候可以保持**Presenter**不变。

MVP模式的框架：Riot,js。

3. MVVM：MVVM是把MVC里的Controller和MVP里的Presenter改成了ViewModel。Model+View+ViewModel。

View的变化会自动更新到ViewModel,ViewModel的变化也会自动同步到View上显示。

这种自动同步是因为ViewModel中的属性实现了Observer，当属性变更时都能触发对应的操作。

MVVM模式的框架有：AngularJS+Vue.js和Knockout+Ember.js后两种知名度较低以及是早起的框架模式。

其实Vue.js不是一个框架，因为它只聚焦视图层，是一个构建数据驱动的Web界面的库。

Vue.js通过简单的API（应用程序编程接口）提供高效的数据绑定和灵活的组件系统。

[扩展地址](https://www.dazhuanlan.com/2019/10/27/5db4a6703dc01/)