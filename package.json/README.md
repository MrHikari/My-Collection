## package.json文件介绍

&emsp;&emsp;创建一个JS的工程目录,每个项目的根目录下面，一般都有一个**package.json**文件，定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。npm install命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。

下面是一个最简单的package.json文件，只定义两项元数据：项目名称和项目版本。

```json
{
  "name" : "xxx",
  "version" : "0.0.0",
}
```

&emsp;&emsp;**package.json**文件本质上就是一个JSON对象，该对象的每一个元素就是当前项目的一项设置。

详细示例：

```json
{ 
  // 项目名称。
  "name": "demo", 
  // version 是版本（遵守"大版本.次要版本.小版本"的格式）。
  "version": "1.0.0", 
  // description 描述这个模块或者项目。
  "description": "myApp", 
  // main 字段指定了加载的入口文件，这个字段的默认值是模块根目录下面的index.js。 
  "main": "app.js", 
  // scripts 指定了运行脚本命令的npm命令行缩写，比如要运行如下命令node index.js时，可以执行npm run start。 
  "scripts": { 
    "start": "node index.js" 
  }, 
  // repository (仓库)用于指示代码存放地址。
  "repository": {
    "type": "git", 
    "url": "https://github.com/XXXX" 
  }, 
  // author 作者，一位人物信息。
  "author": "XXXX", 
  // contributors 贡献者信息，多人信息。
  "contributors": [
    { "name": "YYYY", "email": "YYYY@example.com", "url": "http://YYYY-website.com" }
    { "name": "ZZZZ", "email": "ZZZZ@example.com", "url": "http://ZZZZ-website.com" }
  ],
  // license 授权方式。所有包都应该指定许可证，以便让用户了解是在什么授权下使用此包，以及此包还有哪些附加限制。
  "license": "ISC", 
  // bugs 问题反馈系统的 URL，或者是 email 地址之类的链接。用户通过该途径向你反馈问题。
  "bugs": { 
    "url": "https://github.com/XXXX" 
  }, 
  // keywords 一个字符串数组，方便其他使用者使用 npm search 搜索时发现该项目。
  "keywords": [ 
    "vue", "react" 
  ],  
  // homepage 项目主页url或者项目的文档首页url。 
  "homepage": "https://github.com/XXXX", 
  // devDependencies 指定项目开发所需要的模块(包), 这些是开发期间需要，但是生产环境不会被安装的包。
  "devDependencies": { 
    "@babel/preset-react": "^7.0.0",
    "@babel/standalone": "^7.3.4",
    "@rematch/core": "^1.1.0", 
    "@rematch/loading": "^1.1.3", 
    "axios": "^0.19.0",
    "classnames": "2.2.6",
    "history": "^4.9.0",
    "react": "^16.8.6",
    "react-dom": "^16.8.6",
    "react-dynamic-loadable": "^1.1.2",
    "react-redux": "^7.1.0",
    "react-router-dom": "^5.0.1", 
    "webpack": "^1.13.2" 
  },
  // dependencies 字段指定了项目开发版和发布版都需要依赖的模块。
  "dependencies": { 
    "underscore": "^1.8.3", 
    "vue": "^2.1.4" 
  }, 
}
```

<br/>

---

### 字段信息具体介绍

#### 必要字段

&emsp;&emsp;**package.json**中最重要的属性是**name**和**version**两个属性，这两个属性是必须要有的，否则模块就无法install，这两个属性一起形成了一个npm模块的唯一标识符。模块中内容变更的同时，模块版本也应该一起变化。

1. `name`

项目名称/模块名称

**规则**

* 必须小于或等于214个字符（包括 @scope/ for 范围包）。
* 不能以点（.）或下划线（_）开头。
* 名称中不得包含大写字母。
* 必须仅使用URL安全字符。

**Tips**

* 不要使用和 Node.js 核心模块相同的名字。
* 不要在名字里包含 js 或者 node 单词。
* 短小精悍，让人看到名字就大概了解包的功能，记住它也会被用在 require() 调用里。
* 保证名字在 npm registry 里是唯一的。

2. `version`
 
项目版本号，符合“语义化版本规则”。参考[Semantic Versioning 2.0.0](https://semver.org/lang/zh-CN/)语义化版本规范。

### 可选字段

1. `private`

布尔值。如果 private 为 **true**，npm 会拒绝发布。

这可以防止私有 repositories 不小心被发布出去。

2. `author`

author是单个人信息。contributors是数组形式多人信息。格式设置如下：
 
```json
{
  "author": { 
    "name" : "XXXX" , 
    "email" : "XXXX@example.com",
    "url" : "http://XXXX-website.com"
  },
  "contributors": [
    { "name": "YYYY", "email": "YYYY@example.com", "url": "http://YYYY-website.com" }
    { "name": "ZZZZ", "email": "ZZZZ@example.com", "url": "http://ZZZZ-website.com" }
  ],
}
```

3. `homepage` 

项目主页url或者项目的文档首页url。

4. `repository`
 
用于指示代码存放的位置。

```json
{
  "repository": {
     "type": "git",
     "url": "https://github.com/XXXX/XXXX.github.io"
  }
}
```

5. `bugs`

问题追踪系统的URL或邮箱地址；便于用户通过该途径向作者反馈问题。

```json
{
  "bugs": "https://github.com/XXXX/XXXX.github.io/issues"
}
```

6. `scripts` 
 
&emsp;&emsp;**scripts**指定了运行脚本命令的npm命令行缩写，比如start指定了运行npm run start时，所要执行的命令。 

```json
{
  "scripts": {
    "start": "xxx start",
    "build": "xxx build",
    "test": "xxx test --env=jsdom",
    "test:coverage": "xxx test --env=jsdom --coverage"
  },
}
```

脚本是定义自动化开发相关任务的好方法，比如使用一些简单的构建过程或开发工具。

有一些特殊的脚本名称。 如果定义了 preinstall 脚本，它会在包安装前被调用。 出于兼容性考虑，install、postinstall 和 prepublish 脚本会在包完成安装后被调用。

start 脚本的默认值为 node server.js。


### 重要字段

1. `devDependencies`

指定项目开发所需要的模块。但是生产环境不会被安装的模块。

```json
{
  "devDependencies": {
    "mocker-api": "1.6.6"
  }
}
```

2. `dependencies `

指示当前项目/模块所依赖的其他模块（包）。这些是你的包的开发版和发布版都需要的依赖（包）。

3. `peerDependencies`

当当前项目和所依赖的模块，如果同时依赖另一个模块，但是所依赖的版本不一样，依赖允许说明项目的模块它所依赖的相关的包（模块）的版本等级。

```json
{
  "name": "chai-as-promised",
  "peerDependencies": {
    "chai": "1.x"
  }
}
```

上面代码指定，安装chai-as-promised模块时，主程序chai必须一起安装，而且chai的版本必须是1.x。如果你的项目指定的依赖是chai的2.0版本，就会报错。

4. `bin`

&emsp;&emsp;**bin**用来指定各个内部命令对应的**可执行文件**的位置。
```json
{
  "bin": {
    "someTool": "./bin/someTool.js"
  }
}
```

上面代码指定，someTool 命令对应的可执行文件为 bin 子目录下的 someTool.js。npm会寻找这个文件，在node_modules/.bin/目录下建立符号链接。在上面的例子中，someTool.js会建立符号链接npm_modules/.bin/someTool。由于node_modules/.bin/目录会在运行时加入系统的PATH变量，因此在运行npm时，就可以不带路径，直接通过命令来调用这些脚本。

因此，像下面这样的写法可以采用简写。
```json
{
  "scripts": {
    "start": "./node_modules/someTool/someTool.js build"
  }
}
```

简写为:
```json
{
  "scripts": {
    "start": "someTool build"
  }
}
```

所有node_modules/.bin/目录下的命令，都可以用npm run [命令]的格式运行。在命令行下，键入npm run，然后按tab键，就会显示所有可以使用的命令。

5. `main`

&emsp;&emsp;**main**指定了加载的入口文件，require('moduleName')就会加载这个文件。这个字段的默认值是模块/项目根目录下面的index.js。

6. `config`

&emsp;&emsp;**config**用于添加命令行的环境变量。

下面是一个package.json文件。

```json
{
  "config" : { "port" : "8080" }
}
```

配置项目/模块的脚本的选项或参数。

用户可以改变这个值。

> $ npm config set foo:port 80

### 其他字段

1. `browser`

&emsp;&emsp;**browser**指定该模板供浏览器使用的版本。Browserify这样的浏览器打包工具，通过它就知道该打包那个文件。

```json
{
  "browser": {
    "tipso": "./node_modules/tipso/src/tipso.js"
  },
}
```

2. `engines`

&emsp;&emsp;**engines**字段指明了该模块运行的平台，比如 Node 或者 npm 的某个版本或者浏览器。

```json
{
  "engines": {
      "node": ">=6.9.0",
      "npm": ">=3.10.10"
  }
}
```

3. `man`

&emsp;&emsp;**man**用来指定当前模块/项目的入口文件位置。

```json
{
  "man" :[ "./doc/calc.1" ]
}
```

4. `preferGlobal`

&emsp;&emsp;**preferGlobal**的值是布尔值。
	
```json
{
  "preferGlobal": true
}
```
上述代码表示，该模块更倾向于在全局范围内安装。

当布尔值为false时，当用户不将该模块安装为全局模块时（即不用–global参数）。

未增加该字段，表示该模块的本意就是安装为全局模块。

<br/>

---

**参考资料**
* 开源中国
* CSDN
* 博客园
