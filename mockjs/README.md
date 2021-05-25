### mockjs

#### 安装mockjs

> npm install mockjs --save-dev
mockjs几乎只会在开发过程中使用。

#### 拦截数据的方法 Mock.mock()

记录数据模板。当拦截到匹配 rurl 的 Ajax 请求时，将根据数据模板 template 生成模拟数据，并作为响应数据返回。

```js
// Mock.mock( rurl, template )
Mock.mock('/\/api\/msdk\/proxy\/query_common_credit/', {
    "ret":0,
    "data":
      {
        "mtime": "@datetime", // 随机生成日期时间
        "score|1-800": 800, // 随机生成1-800的数字
        "rank|1-100":  100, // 随机生成1-100的数字
        "stars|1-5": 5, // 随机生成1-5的数字
        "nickname": "@cname", // 随机生成中文名字
      }
});
```
