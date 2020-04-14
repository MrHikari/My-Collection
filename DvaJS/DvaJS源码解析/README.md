## Dva 源码解析

### package.json

在学习看任何项目的源码之前，先去浏览 `package.json` 文件。看看项目的入口文件，了解该用了哪些依赖，对项目便有了大致的概念。

`package.json` 里是这么写的：
```js
···
  "scripts": {
    "start": "roadhog server",
    "build": "roadhog build",
    "lint": "eslint --ext .js src test",
    "precommit": "npm run lint"
  },
···
```

一般对于大规模使用和公开的框架，基本上它们的依赖都可以在 **npm** 中找到，翻翻依赖，"roadhog": "^0.5.2"。

比如，`start` 所对应的 `roadhog server`，原来 **roadhog** 是个和 **webpack** 相似的库，起的是 **webpack** `自动打包`和`热更替`的作用。

在 roadhog 的默认配置里有这么一条信息：


`entry` 指定 **webpack** 入口文件，支持 glob 格式。

如果当前的项目是多页类型，并且希望把 `src/pages` 的文件作为入口。可以这样配置：

```js
{
  "entry": "src/index.js",
}
```

启动的入口回到了 src/index.

#### src/index.js

在 `src/index.js` 里，**dva** 一共做了这么几件事：

1. 从 **dva** 依赖中引入 `dva` ：
```js
import dva from 'dva'`;
```

2. 通过函数生成一个 **app 对象** d d：
```js
const app = dva();
```

3. 加载插件：
```js
app.use({});
```

4. 注入 model：
```js
app.model(require('./models/example'));
```

5. 添加路由：
```js
app.router(require('./routes/indexAnother'));
```

6. 启动：
```js
app.start('#root');
```

在这 6 步当中，dva 完成了<br/>
使用 React 解决 view 层、<br/>
使用 redux 管理 model、<br/>
使用 redux-saga 解决异步的主要功能。<br/>
总结目前大部分的脚手架工具，发现目前 前端框架 之所以被称为“框架”也就是解决了 数据和页面调用等文件管理问题。前端工程师至今所做的事情都是在 分离动态的 data 和静态的 view ，只不过侧重点和实现方式也不同。

至今为止出了这么多框架，但是前端 **MVX** 的思想一直都没有改变。
