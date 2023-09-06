### JS 异常捕获和抛出

`try...catch` 用来异常捕获（主要适用于**IE5 以上**内核的浏览器，也是最常用的异常捕获方式）

使用`onerror`时间捕获异常，这种捕获方式是比较古老的一中方式，目前一些主流的浏览器暂不支持这种捕获方式。

捕获异常的语法如下：

```js
try {
  //运行代码
} catch (err) {
  //处理错误
}
```

---

### 异步函数的状态捕捉

写异步函数的时候，`promise` 和 `async` 两种方案都非常常见，甚至同一个项目里，不同的开发人员都使用不同的习惯, 如果还在纠结的话：“`async` 是异步编程的终极解决方案”。

##### `try catch` 代码示例：

```js
// 异步获取用户信息
function getUserInfo() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("请求异常");
    }, 1000);
  });
}

// 登录业务方法
async function logined() {
  try {
    // 调用异步方法，获取用户数据
    const userInfo = await getUserInfo();
    // 执行中断
    const pageInfo = await getPageInfo(userInfo?.userId);
  } catch (e) {
    // 捕捉错误，打印错误
    console.warn(e);
  }
}

// 执行登录业务
logined();
```

执行后会在 `catch` 里捕获 请求异常，然后 **getUserInfo** 函数中断执行，这是符合逻辑的，对于有依赖关系的接口，中断执行可以避免程序崩溃，这里唯一的问题是 `try catch` 貌似占据了太多行数，如果每个接口都写的话看起来略显冗余。

##### 直接 `catch`

```js
// 异步获取用户信息
function getUserInfo() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("请求异常");
    }, 1000);
  });
}

// 登录业务方法
async function logined() {
  // 直接使用 catch 捕捉异常
  const userInfo = await getUserInfo().catch((e) => console.warn(e));
  // 执行没有中断，userInfo 为 undefined
  if (!userInfo) return; // 需要做非空校验
  const pageInfo = await getPageInfo(userInfo?.userId);
}

// 执行登录业务
logined();
```

执行后 `catch` 可以正常捕获异常，但是程序没有中断，**控制台不会有报错信息**，返回值 **userInfo** 为 **undefined**, 所以如果这样写的话，就需要对返回值进行非空校验, `if (!userInfo) return;`。

##### 在 `catch` 里 `reject`

```js
// 异步获取用户信息
function getUserInfo() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("请求异常");
    }, 1000);
  });
}

// 登录业务方法
async function logined() {
  // 返回 return Promise.reject(e) 中断 异常时的代码执行
  const userInfo = await getUserInfo().catch((e) => {
    console.warn(e);
    return Promise.reject(e); // 会导致控制台出现 uncaught (in promise) 报错信息
  });
  // 执行中断
  const pageInfo = await getPageInfo(userInfo?.userId);
}

// 执行登录业务
logined();
```

一般在项目里都是用 **axios** 或者 **fetch** 之类发送请求，会对其进行一个封装，也可以在里面进行 `catch` 操作，对错误信息先一步处理，至于是否需要 `reject`，就看是否想要在 `await` 命令异常时候中断了；不使用 `reject` 则不会中断，但是需要每个接口拿到 **response** 后先 非空校验， 使用 `reject` 则会在异常处中断，并且会在控制台暴露 `uncaught (in promise)` 报错信息（控制台报错不明显）。

##### 业务中常用示例 useRequest

```js
// 异步获取用户信息
function getUserInfo() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("请求异常");
    }, 1000);
  });
}

// 请求
const { data, error, loading } = useRequest(async () => {
  try {
    const res = await getUserInfo();
    return res;
  } catch (e) {
    // 捕捉错误，打印错误
    console.warn(e);
  }
});
```
