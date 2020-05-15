### axios

基于`promise` ，用于浏览器和 **node.js**的 **http客户端** 。

**特点**:
* 支持从浏览器中创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
* 支持从 **node.js** 创建 [http](https://nodejs.org/api/http.html) 请求
* 支持 [promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
* 能拦截请求和响应
* 能转换请求和响应数据
* 能取消请求
* 自动转换JSON数据
* 客户端支持防御 [XSRF](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0)

#### 安装

1. **npm**安装

> $ npm install axios

2. **bower**安装

> $ bower install axios

3. 通过cdn引入

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

#### 简单示例 参考

1. 执行 GET 请求

```js
// 为 固定 api url 创建请求
axios.get('/user?username=xxx')
  .then(function (response) {
    console.log(response); // 正常返回数据
  })
  .catch(function (error) {
    console.log(error); // 异常时抓取error
  });

// 为一个需要携带参数 api url 创建请求
axios.get('/user', {
    params: {
      username: 'xxx'; // 携带的用户名参数
    }
  })
  .then(function (response) {
    console.log(response); // 正常返回数据
  })
  .catch(function (error) {
    console.log(error); // 异常时抓取error
  });
```

2. 执行 POST 请求

```js
// 携带参数发起 api 请求
axios.post('/user', {
    firstName: 'Steve', // 参数
    lastName: 'Jobs' // 参数
  })
  .then(function (response) {
    console.log(response); // 提交正常后的返回结果
  })
  .catch(function (error) {
    console.log(error); // 提交不成功时的错误抓取
  });
```

3. 执行多个并发请求

```js
// api 请求方法示例1
function getExample1() {
  return axios.get('/user/example1');
}

// api 请求方法示例2
function getExample2() {
  return axios.get('/user/example2');
}

axios.all([getExample1(), getExample2()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```

---

#### axios API

可以通过向 **axios** 传递相关配置来创建请求

**axios(config)**

```js
// 发送 POST 请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
```
```js
// 获取远端图片
axios({
  method:'get',
  url:'http://bit.ly/2mTM3nY',
  responseType:'stream'
})
  .then(function(response) {
  response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
});
```
#### axios(url[, config])
```js
// 发送 GET 请求（默认的方法）
axios('/user/12345');
```

---

#### 请求方法的别名

为方便起见，为所有支持的请求方法提供了别名
```js
axios.request(config)
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
```
**注意**

在使用别名方法时， **url**、**method**、**data** 这些属性都不必在配置中指定。