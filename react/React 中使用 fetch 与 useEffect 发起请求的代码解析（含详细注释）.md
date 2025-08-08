# React 中使用 `fetch` 与 `useEffect` 发起请求的代码解析（含详细注释）

## 完整代码（带注释）

```jsx
// 使用 useEffect 处理副作用（网络请求属于副作用）
useEffect(() => {
  // 定义异步请求函数（因为 useEffect 回调不能直接是 async 函数，所以封装一层）
  const fetchData = async () => {
    try {
      // 1. 发起 GET 请求到目标 API
      // await 等待请求完成，返回一个 Response 对象
      const response = await fetch('https://api.example.com/data');
      
      // 2. 将响应数据解析为 JSON 格式
      // 注意：response.json() 也是异步操作，需要 await
      const result = await response.json();
      
      // 3. 成功获取数据后，更新状态（存储数据 + 标记加载完成）
      setData(result); // 将解析后的数据存入 state
      setLoading(false); // 加载状态设为 false，隐藏加载提示
      
    } catch (error) {
      // 4. 捕获请求过程中的错误（如网络故障、解析失败等）
      console.error('Error fetching data:', error);
      // 可在此处添加错误处理逻辑，如 setError(error.message) 展示错误提示
    }
  };

  // 调用异步请求函数，触发数据获取
  fetchData();

}, []); // 依赖数组为空：表示该副作用仅在组件挂载时执行一次（类似 componentDidMount）
```

## 代码思路解析

1. **为什么用 `useEffect`？**

   - `useEffect` 是 React 函数组件中处理**副作用**的钩子，网络请求属于典型的副作用（操作外部资源）。
   - 依赖数组 `[]` 确保请求只在组件**首次挂载**时执行一次，避免重复请求（类似类组件的 `componentDidMount`）。

2. **为什么封装 `fetchData` 异步函数？**

   - `useEffect` 的回调函数**不能直接是 `async` 函数**（会返回 Promise，导致 React 无法正确清理副作用）。
   - 因此将请求逻辑封装到 `fetchData` 内部，再在 `useEffect` 中调用，解决语法限制。

3. **请求流程拆解**

   - **发起请求**：`fetch('https://api.example.com/data')` 发起 GET 请求，返回 Promise 对象。
   - **处理响应**：`response.json()` 将响应体解析为 JSON 格式（异步操作，需 `await`）。
   - **更新状态**：用 `setData` 存储数据，`setLoading(false)` 结束加载状态，驱动组件重新渲染。
   - **错误处理**：`try/catch` 捕获网络错误、解析失败等异常，避免程序崩溃。

4. **状态管理配合**

   - 通常需要定义两个状态：`const [data, setData] = useState(null);`（存储请求结果）和 `const [loading, setLoading] = useState(true);`（标记加载状态）。

   - 组件渲染时可根据`loading`

     状态展示不同内容：

     ```jsx
     if (loading) return <div>加载中...</div>;
     if (!data) return <div>暂无数据</div>;
     return <div>{/* 渲染 data 内容 */}</div>;
     ```

## 扩展说明

- **带参数的请求**：若需动态传参（如 `id`），可在依赖数组中添加参数，实现参数变化时重新请求：

  ```jsx
  useEffect(() => {
    const fetchData = async () => {
      const response = await fetch(`https://api.example.com/data/${id}`); // 使用 id 参数
      // ... 其余逻辑不变
    };
    fetchData();
  }, [id]); // 依赖 id：id 变化时重新执行请求
  ```

- **POST 请求**：如需提交数据，可在 `fetch` 中添加配置：

  ```jsx
  fetch('https://api.example.com/data', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: 'test' }) // 发送的数据
  });
  ```



通过以上逻辑，该代码片段可作为 React 函数组件中发起网络请求的通用模板，适配大多数 GET 请求场景。