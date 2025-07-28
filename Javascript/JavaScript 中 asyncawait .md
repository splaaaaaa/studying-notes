# JavaScript 中 `async/await` 详解

`async/await` 是 ES2017 引入的异步编程语法糖，基于 Promise 实现，旨在简化异步代码的编写，使其更接近同步代码的直观性，解决回调地狱和 Promise 链式调用的复杂性问题。

## 一、异步编程的基础与演进

### 1. 同步与异步的核心差异

- **同步编程**：代码按顺序执行，前一个操作完成后才执行下一个（单线程阻塞）。
  示例：

  ```javascript
  console.log('1');
  console.log('2'); // 输出：1, 2（严格按顺序执行）
  ```

- **异步编程**：耗时操作被放入 “任务队列”，主线程继续执行后续代码，空闲时再处理队列任务（非阻塞）。
  示例：

  ```javascript
  console.log('1');
  setTimeout(() => console.log('2'), 0); // 异步任务放入队列
  console.log('3'); // 输出：1, 3, 2（异步任务后执行）
  ```

## 二、`async/await` 核心语法

### 1. `async` 函数

- **定义**：在函数声明前添加 `async` 关键字，标记该函数为异步函数。

- **返回值**：始终返回一个 Promise 对象：

  - 若返回非 Promise 值，自动包装为 `resolved` 状态的 Promise（如 `return 123` 等价于 `return Promise.resolve(123)`）；
  - 若抛出错误，返回 `rejected` 状态的 Promise（如 `throw new Error('错了')` 等价于 `return Promise.reject(...)`）。

  示例：

  ```javascript
  // 返回非 Promise 值，自动包装为 Promise
  async function getNum() {
    return 42;
  }
  getNum().then(num => console.log(num)); // 输出：42
  
  // 抛出错误，返回 rejected Promise
  async function throwErr() {
    throw new Error('出错了');
  }
  throwErr().catch(err => console.error(err)); // 输出：Error: 出错了
  ```

### 2. `await` 表达式

- **作用**：暂停 `async` 函数的执行，等待 Promise 完成后再继续。

- **限制**：只能在 `async` 函数内部使用。

- **行为**：

  - 若等待的 Promise 为 `resolved`，返回其结果值；
  - 若等待的 Promise 为 `rejected`，抛出错误（需用 `try/catch` 捕获）。

  示例：

  ```javascript
  // 定义一个返回 Promise 的延迟函数
  function delay(ms) {
    return new Promise(resolve => setTimeout(() => resolve(ms), ms));
  }
  
  // 使用 async/await 执行异步操作
  async function run() {
    console.log('开始');
    const time1 = await delay(1000); // 等待 1 秒，返回 1000
    console.log(`等待了 ${time1}ms`);
    const time2 = await delay(2000); // 再等待 2 秒，返回 2000
    console.log(`总共等待了 ${time1 + time2}ms`);
  }
  
  run();
  // 输出：
  // 开始
  // 等待了 1000ms（1秒后）
  // 总共等待了 3000ms（再等2秒后）
  ```

## 三、错误处理

`await` 等待的 Promise 若被拒绝（`rejected`），会抛出错误，需通过 `try/catch` 捕获（与同步代码的错误处理逻辑一致）。

示例：

```javascript
async function fetchUser() {
  try {
    const response = await fetch('https://api.example.com/user');
    if (!response.ok) {
      throw new Error(`请求失败：${response.status}`); // 主动抛错
    }
    const user = await response.json(); // 解析响应数据
    console.log('用户数据：', user);
  } catch (error) {
    // 捕获所有错误（网络错误、主动抛错等）
    console.error('获取失败：', error);
  }
}
```

## 四、并行执行异步操作

若多个异步操作无依赖关系，可通过 `Promise.all()` 并行执行，避免顺序等待导致的性能浪费。

示例：

```javascript
async function fetchAllData() {
  // 并行发起两个请求（同时执行，而非顺序等待）
  const [userRes, postRes] = await Promise.all([
    fetch('/api/user'),
    fetch('/api/posts')
  ]);

  // 分别解析结果
  const user = await userRes.json();
  const posts = await postRes.json();

  return { user, posts };
}
```

## 五、常见问题与最佳实践

1. **不可遗漏 `await`**
   若忘记添加 `await`，会直接返回 Promise 对象而非结果，导致逻辑错误。

   ```javascript
   // 错误：未加 await，data 是 Promise 对象
   async function bad() { const data = fetch('/api'); }
   
   // 正确：加 await 等待结果
   async function good() { const data = await fetch('/api'); }
   ```

2. **避免不必要的 `async`**
   若函数内无 `await`，无需声明为 `async`（会多余包装为 Promise）。

   ```javascript
   // 不必要的 async
   async function useless() { return 'hello'; }
   
   // 更简洁：直接返回值
   function better() { return 'hello'; }
   ```

3. **顶层 `await`（ES2022 特性）**
   在模块（`*.mjs`）顶层可直接使用 `await`，无需包裹在 `async` 函数中。

   ```javascript
   // 模块文件中
   const config = await fetch('/config');
   console.log(config);
   ```

4. **性能优化**

   - 无依赖的异步操作优先用 `Promise.all()` 并行执行；
   - 避免在循环中逐个 `await`（会串行执行，可用 `Promise.all()` 改写）。

## 六、`async/await` 与 Promise 对比

| 特性     | `async/await`          | Promise（`then`/`catch`）    |
| -------- | ---------------------- | ---------------------------- |
| 可读性   | 高，语法接近同步代码   | 中，链式调用可能冗长         |
| 错误处理 | 用 `try/catch`（直观） | 用 `.catch()`（链式）        |
| 调试体验 | 好，调用栈清晰         | 较差，链式调用可能丢失栈信息 |
| 代码结构 | 扁平，无嵌套           | 链式或嵌套（复杂场景繁琐）   |

## 总结

`async/await` 是 JavaScript 异步编程的里程碑改进，其核心价值在于：

- 使异步代码的逻辑和写法接近同步代码，大幅提升可读性；
- 基于 Promise 实现，与现有异步生态完全兼容；
- 通过 `try/catch` 统一处理错误，比 Promise 的链式 `catch` 更直观。

虽然 `async/await` 不能完全替代 Promise（如并行操作仍需 `Promise.all()`），但在绝大多数异步场景中，它是更优雅、更高效的解决方案，是现代 JavaScript 开发的必备技能。