# React 组件状态（State）详解

状态（State）是 React 组件管理动态数据的核心机制，用于存储组件的私有数据并驱动 UI 更新。本文详细介绍状态在类组件和函数组件中的使用方式、生命周期钩子的作用及数据流动特性。

## 一、状态的基本概念

- **定义**：状态是组件内部的私有数据，用于管理随用户交互或时间变化的动态信息（如计数器值、当前时间等）。
- **作用**：当状态更新时，React 会自动重新渲染组件，使 UI 与数据保持一致（无需手动操作 DOM）。
- **适用场景**：类组件通过 `this.state` 管理状态；函数组件通过 `useState` Hook 管理状态。

## 二、类组件中的状态管理

类组件通过继承 `React.Component` 实现状态管理，需通过构造函数初始化状态，并使用 `setState` 方法更新状态。

### 1. 基本示例：计数器组件

```jsx
// 文件路径：src/Counter.js
import React, { Component } from 'react';

class Counter extends Component {
  // 构造函数：初始化状态
  constructor(props) {
    super(props); // 必须调用父类构造函数
    this.state = { count: 0 }; // 初始化状态（count 初始值为 0）
  }

  // 定义更新状态的方法
  increment = () => {
    // 使用 setState 更新状态（不可直接修改 this.state）
    this.setState({ count: this.state.count + 1 });
  };

  // 渲染 UI
  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p> {/* 访问状态 */}
        <button onClick={this.increment}>Increment</button> {/* 点击触发更新 */}
      </div>
    );
  }
}

export default Counter;
```

### 2. 渲染组件

```jsx
// 文件路径：src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import Counter from './Counter';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Counter />); // 渲染计数器组件
```

## 三、函数组件中的状态管理（使用 Hook）

函数组件通过 `useState` Hook 管理状态，无需类语法，更简洁。

### 1. 基本示例：计数器组件

```jsx
// 文件路径：src/Counter.js
import React, { useState } from 'react';

function Counter() {
  // 定义状态：count 为当前值，setCount 为更新函数（初始值为 0）
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p> {/* 访问状态 */}
      {/* 点击按钮调用 setCount 更新状态 */}
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

### 2. 特点

- `useState` 接收初始值，返回 `[状态值, 更新函数]` 数组。
- 每次调用 `setCount` 会触发组件重新渲染，UI 同步更新。

## 四、生命周期钩子与状态更新

类组件通过生命周期钩子管理状态的副作用（如定时器、网络请求），常用钩子包括 `componentDidMount`（挂载后）和 `componentWillUnmount`（卸载前）。

### 示例：时钟组件（自动更新时间）

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() }; // 初始化状态为当前时间
  }

  // 组件挂载到 DOM 后执行（设置定时器）
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(), // 每秒调用 tick 方法
      1000
    );
  }

  // 组件从 DOM 卸载前执行（清除定时器）
  componentWillUnmount() {
    clearInterval(this.timerID); // 防止内存泄漏
  }

  // 更新时间的方法
  tick() {
    this.setState({ date: new Date() }); // 更新状态，就会重新执行render重新渲染，从而实现更新时间的兄啊过
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2> {/* 显示当前时间 */}
      </div>
    );
  }
}
```

### 执行流程

1. 组件初始化时，`constructor` 初始化 `date` 状态。
2. 组件挂载后，`componentDidMount` 设置定时器，每秒调用 `tick`。
3. `tick` 方法通过 `setState` 更新 `date`，触发 `render` 重新渲染 UI。
4. 组件卸载时，`componentWillUnmount` 清除定时器，释放资源。

## 五、数据流动：自顶向下单向数据流

React 中状态遵循**单向数据流**原则：

- 状态由特定组件私有，仅该组件可修改。
- 数据通过 `props` 从父组件流向子组件，子组件无法直接修改父组件状态。

### 示例：状态共享与传递

```jsx
// 子组件：仅接收并显示数据（无状态）
function FormattedDate(props) {
  return <h2>现在是 {props.date.toLocaleTimeString()}.</h2>;
}

// 父组件：管理 date 状态并传递给子组件
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() };
  }

  componentDidMount() {
    this.timerID = setInterval(() => this.tick(), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({ date: new Date() });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <FormattedDate date={this.state.date} /> {/* 传递状态给子组件 */}
      </div>
    );
  }
}

// 渲染多个独立的 Clock 组件（各自管理状态）
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}
```

### 特点

- 每个 `Clock` 组件独立管理自己的定时器和 `date` 状态，互不干扰。
- 子组件 `FormattedDate` 仅通过 `props` 接收数据，不关心数据来源（状态或静态值）。

## 六、总结

| 类型     | 状态初始化方式                      | 状态更新方式             | 适用场景                       |
| -------- | ----------------------------------- | ------------------------ | ------------------------------ |
| 类组件   | `constructor` 中初始化 `this.state` | `this.setState({ ... })` | 需要生命周期钩子、复杂状态管理 |
| 函数组件 | `useState(初始值)`                  | 调用 `setXxx(新值)`      | 简单状态管理、无生命周期需求   |



状态是 React 组件动态交互的核心，合理使用状态并遵循单向数据流原则，可使组件逻辑清晰、易于维护。实际开发中，推荐优先使用**函数组件 + Hooks**（如 `useState`）管理状态，更简洁高效。