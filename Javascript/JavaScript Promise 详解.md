# JavaScript Promise 详解

Promise 是 ES6 引入的用于处理异步操作的对象，旨在更优雅地管理复杂的异步任务，解决传统回调函数嵌套（“回调地狱”）的问题。它代表一个异步操作的最终完成（或失败）及其结果值。

## 一、Promise 核心概念

### 1. 本质与作用

- **本质**：一个 “承诺”—— 异步操作在未来某个时间点会返回结果（成功或失败）。
- **作用**：替代嵌套回调，使异步代码更清晰、易维护，支持链式调用和集中错误处理。

### 2. 三种状态

- **pending**：初始状态，异步操作未完成（既非成功也非失败）。
- **fulfilled**：异步操作成功完成，状态不可逆，会返回结果值。
- **rejected**：异步操作失败，状态不可逆，会返回失败原因（如错误对象）。

**状态转换**：
`pending → fulfilled`（通过 `resolve` 触发）或 `pending → rejected`（通过 `reject` 触发），一旦转换则无法再改变。

### 3. 基本语法（创建 Promise）

```javascript
const myPromise = new Promise((resolve, reject) => {
  // 异步操作（如 AJAX 请求、定时器等）
  if (/* 操作成功 */) {
    resolve("成功的结果"); // 状态转为 fulfilled，传递结果
  } else {
    reject("失败的原因"); // 状态转为 rejected，传递错误
  }
});
```

## 二、Promise 常用方法

### 1. `then()`：处理成功或失败的结果

- 用于指定 `fulfilled` 或 `rejected` 状态的回调函数。
- 支持链式调用（返回一个新的 Promise）。

**语法**：

```javascript
myPromise
  .then(
    (result) => { /* 处理成功：fulfilled 状态触发 */ },
    (error) => { /* 处理失败：rejected 状态触发（可选） */ }
  );
```

**示例**：

```javascript
myPromise
  .then(
    (res) => console.log("成功：", res),
    (err) => console.error("失败：", err)
  );
```

### 2. `catch()`：专门处理失败

- 简化错误处理，仅捕获 `rejected` 状态的错误（相当于 `then(null, errorHandler)`）。

**语法**：

```javascript
myPromise
  .then((result) => { /* 处理成功 */ })
  .catch((error) => { /* 处理失败（包括 then 中的错误） */ });
```

**示例**：

```javascript
myPromise
  .then(res => console.log("成功：", res))
  .catch(err => console.error("失败：", err));
```

### 3. `finally()`：无论结果如何都执行

- 无论 Promise 状态是 `fulfilled` 还是 `rejected`，都会执行（如清理资源、关闭加载动画）。

**语法**：

```javascript
myPromise
  .then(result => { /* 处理成功 */ })
  .catch(error => { /* 处理失败 */ })
  .finally(() => { /* 最终执行操作 */ });
```

**示例**：

```javascript
fetchData()
  .then(data => console.log("数据：", data))
  .catch(err => console.error("错误：", err))
  .finally(() => console.log("操作完成（无论成功/失败）"));
```

## 三、Promise 高级特性

### 1. 链式调用

通过 `then()` 返回新的 Promise，实现多个异步操作按顺序执行，避免嵌套。

**示例**：

```javascript
// 第一步：获取用户 ID
getUserId()
  .then(userId => {
    // 第二步：通过 ID 获取用户信息
    return getUserInfo(userId);
  })
  .then(userInfo => {
    // 第三步：通过用户信息获取订单
    return getOrders(userInfo.id);
  })
  .then(orders => {
    console.log("最终订单：", orders);
  })
  .catch(error => {
    // 捕获链中任意步骤的错误
    console.error("流程出错：", error);
  });
```

### 2. 静态方法

| 方法                 | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| `Promise.all(arr)`   | 等待数组中**所有 Promise 都成功**，返回结果数组；若任一失败则立即返回该错误。 |
| `Promise.race(arr)`  | 返回数组中**最先完成的 Promise 结果**（无论成功或失败）。    |
| `Promise.resolve(v)` | 快速创建一个已成功的 Promise，结果为 `v`。                   |
| `Promise.reject(e)`  | 快速创建一个已失败的 Promise，错误为 `e`。                   |

**示例**：

```javascript
// Promise.all：等待所有请求完成
Promise.all([fetchUser(), fetchPosts()])
  .then(results => {
    const [user, posts] = results;
    console.log("用户：", user, "帖子：", posts);
  })
  .catch(err => console.error("任一请求失败：", err));

// Promise.race：取最快完成的结果
Promise.race([fetchFromServerA(), fetchFromServerB()])
  .then(result => console.log("最快响应：", result))
  .catch(err => console.error("最快响应为失败：", err));
```

## 四、实际应用场景

### 1. 处理 AJAX 请求

用 Promise 封装 XMLHttpRequest，使异步请求更简洁。

**示例**：

```javascript
function fetchData(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(xhr.responseText); // 成功：返回响应内容
      } else {
        reject(new Error(xhr.statusText)); // 失败：返回错误
      }
    };
    xhr.onerror = () => reject(new Error("网络错误")); // 网络错误
    xhr.send();
  });
}

// 使用
fetchData("https://api.example.com/data")
  .then(data => console.log("获取成功：", data))
  .catch(err => console.error("获取失败：", err));
```

### 2. 多步骤异步操作

通过链式调用按顺序执行依赖于前一步结果的异步任务。

**示例**：

```javascript
// 1. 获取用户 ID → 2. 获取用户信息 → 3. 获取用户帖子
getUserId()
  .then(id => getUserInfo(id))
  .then(user => getPosts(user.id))
  .then(posts => console.log("用户帖子：", posts))
  .catch(err => console.error("步骤出错：", err));
```

## 五、最佳实践与注意事项

1. **避免嵌套**：用链式调用替代 `then` 内部嵌套 `then`，防止 “Promise 地狱”。

   ```javascript
   // 错误：嵌套
   doA().then(a => {
     doB(a).then(b => { /* ... */ });
   });
   
   // 正确：链式
   doA().then(a => doB(a)).then(b => { /* ... */ });
   ```

2. **必须处理错误**：始终用 `catch()` 捕获错误，避免 “未捕获的 Promise 拒绝” 报错。

3. **不可取消**：Promise 一旦创建无法取消，若需取消可结合 `AbortController`。

4. **与 async/await 配合**：async/await 是基于 Promise 的语法糖，更接近同步代码的可读性（需理解 Promise 基础）。

   ```javascript
   async function fetchData() {
     try {
       const user = await getUser(123);
       const posts = await getPosts(user.id);
       console.log(posts);
     } catch (err) {
       console.error("错误：", err);
     }
   }
   ```

## 六、浏览器支持

- 支持 ES6 的现代浏览器（Chrome 58+、Edge 14+、Firefox 54+、Safari 10+）均支持 Promise。
- 旧浏览器（如 IE）需通过 polyfill 兼容。

## 总结

Promise 是 JavaScript 异步编程的核心工具，通过状态管理、链式调用和集中错误处理，解决了传统回调的痛点。其核心优势在于：

- 使异步代码线性化，易读易维护；
- 支持多个异步操作的串行（链式）和并行（`Promise.all`）执行；
- 为 async/await 语法提供基础，是现代 JavaScript 异步编程的基石。