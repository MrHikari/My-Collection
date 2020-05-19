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

参考资料 [XMLHttpRequest.responseType](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseType)

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

为了方便，为所有**支持**的**请求方法**提供了*别名*<br/>
（在**HTTP连接**中消息报文分为*Request*请求和*Response*响应两种，每种报文在HTTP首部会有不同的字段来标识不同的用途。）

```js
axios.request(config)
// HTTP定义了很多于服务器交互的方法
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
```
**注意**

在使用别名方法时， **url**、**method**、**data** 这些属性都**不必**在配置中指定。

---

#### 并发

处理并发请求的助手函数
```js
axios.all(iterable)
// - 参数： 请求数组
```
```js
axios.spread(callback)
// - 参数： 对应请求返回值
```

---

#### 创建实例

可以使用自定义配置新建一个 **axios** 实例

```js
axios.create([config])
const instance = axios.create({
  baseURL: 'http://localhost:3000', // 建立请求的url
  timeout: 1000, // 请求超时时间设置，单位毫秒
  headers: {'X-Custom-Header': 'foobar'} // 请求头
});
```

---

**实例方法**

以下是可用的实例方法。指定的配置将与实例的配置合并。

```js
axios#request(config)
axios#get(url[, config])
axios#delete(url[, config])
axios#head(url[, config])
axios#options(url[, config])
axios#post(url[, data[, config]])
axios#put(url[, data[, config]])
axios#patch(url[, data[, config]])
```

---

#### 请求配置

这些是创建请求时可以用的配置选项。只有 **url** 是*必需*的。如果*没有*指定 **method**，请求将默认使用 `get` 方法。
[ArrayBuffer：类型化数组](https://javascript.ruanyifeng.com/stdlib/arraybuffer.html#toc0)

```js
{
  // `url` 是用于请求的服务器 URL(api路由)
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // default 没有填写时默认 get

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://sample.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data, headers) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

   // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

   // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // default

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

 // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

   // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // default

  // `responseEncoding` indicates encoding to use for decoding responses
  // Note: Ignored for `responseType` of 'stream' or client-side requests
  responseEncoding: 'utf8', // default

   // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

   // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

   // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // default

  // `socketPath` defines a UNIX Socket to be used in node.js.
  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  // Only either `socketPath` or `proxy` can be specified.
  // If both are specified, `socketPath` is used.
  socketPath: null, // default

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```