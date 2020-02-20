### ES6模块主要有两个功能：export和import

#### export与import

**export**用于对外输出本模块（一个文件可以理解为一个模块）变量的接口。

**import**用于在一个模块中加载另一个含有export接口的模块。

也就是说使用export命令定义了模块的对外接口以后，其他JS文件就可以通过import命令加载这个模块（文件）。

#### export与export default

1. export与export default均可用于导出常量、函数、文件、模块等。
2. 你可以在其它文件或模块中通过import+(常量 | 函数 | 文件 | 模块)名的方式，将其导入，以便能够对其进行使用。
3. 在一个文件或模块中，**export**、**import**可以有**多个**，**export default仅有一个**。
4. 通过export方式导出，在导入时要加{ }，export default则不需要。

这样来说其实很多时候export与export default可以实现同样的目的，只是用法有些区别。注意第四条，通过export方式导出，在导入时要加{ }，export default则不需要。使用export default命令，为模块指定默认输出，这样就不需要知道所要加载模块的变量名。