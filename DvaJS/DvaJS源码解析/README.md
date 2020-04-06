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

