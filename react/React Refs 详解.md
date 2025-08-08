# React Refs 详解

## 一、什么是 Refs？

Refs（引用）是 React 提供的一种特殊属性，用于直接访问 DOM 元素或组件实例。它打破了 React 单向数据流的常规模式，允许开发者直接操作 DOM 或获取子组件的实例方法，适用于以下场景：

- 处理焦点、文本选择或媒体播放
- 触发强制动画
- 集成第三方 DOM 库

## 二、创建与使用 Refs 的方式

### 1. 类组件中使用 `React.createRef`（推荐）

#### 步骤：

1. **创建 Ref 对象**：在构造函数中通过 `React.createRef()` 初始化。
2. **绑定 Ref**：在 `render()` 中通过 `ref` 属性将 Ref 绑定到 DOM 元素或组件。
3. **访问 Ref**：通过 `ref.current` 访问绑定的 DOM 元素或组件实例。

#### 实例（输入框获取焦点）：

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    // 1. 创建 Ref 对象
    this.myInputRef = React.createRef();
  }

  handleClick = () => {
    // 3. 访问 Ref 并调用 DOM 方法（让输入框获取焦点）
    this.myInputRef.current.focus();
  };

  render() {
    return (
      <div>
        {/* 2. 绑定 Ref 到输入框 */}
        <input type="text" ref={this.myInputRef} />
        <button onClick={this.handleClick}>点我输入框获取焦点</button>
      </div>
    );
  }
}
```

### 2. 回调 Refs（旧方式，了解即可）

通过回调函数创建 Ref，在 React 16.3 前常用，现在推荐 `React.createRef`。

#### 实例：

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = null; // 存储 DOM 元素引用
    // 定义回调函数，接收 DOM 元素作为参数
    this.setMyRef = (element) => {
      this.myRef = element;
    };
  }

  componentDidMount() {
    // 访问 Ref（组件挂载后确保元素存在）
    if (this.myRef) {
      this.myRef.focus();
    }
  }

  render() {
    // 绑定回调函数到 ref 属性
    return <input type="text" ref={this.setMyRef} />;
  }
}
```

### 3. 函数组件中使用 `useRef` Hook

在函数组件中，通过 `useRef` Hook 创建 Ref，用法与类组件的 `React.createRef` 类似。

#### 实例：

```jsx
import React, { useRef } from 'react';

const MyComponent = () => {
  // 创建 Ref 对象（初始值为 null）
  const inputRef = useRef(null);

  const handleClick = () => {
    // 访问 Ref 并调用 DOM 方法
    inputRef.current.focus();
  };

  return (
    <div>
      {/* 绑定 Ref 到输入框 */}
      <input type="text" ref={inputRef} />
      <button onClick={handleClick}>Focus Input</button>
    </div>
  );
};
```

## 三、Refs 的核心应用场景

### 1. 直接操作 DOM 元素

通过 Ref 访问 DOM 元素的属性和方法，例如修改样式、获取焦点等。

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }

  handleClick = () => {
    // 改变输入框背景颜色
    this.myRef.current.style.backgroundColor = 'yellow';
  };

  render() {
    return (
      <div>
        <input type="text" ref={this.myRef} />
        <button onClick={this.handleClick}>Change Background</button>
      </div>
    );
  }
}
```

### 2. 访问子组件实例

通过 Ref 调用子组件的方法或访问其属性（仅适用于类组件）。

```jsx
// 子组件（类组件）
class ChildComponent extends React.Component {
  constructor(props) {
    super(props);
    this.inputRef = React.createRef();
  }

  // 子组件的实例方法：让输入框获取焦点
  focusInput = () => {
    this.inputRef.current.focus();
  };

  render() {
    return <input type="text" ref={this.inputRef} />;
  }
}

// 父组件
class ParentComponent extends React.Component {
  constructor(props) {
    super(props);
    this.childRef = React.createRef(); // 绑定子组件
  }

  handleClick = () => {
    // 调用子组件的实例方法
    this.childRef.current.focusInput();
  };

  render() {
    return (
      <div>
        <ChildComponent ref={this.childRef} />
        <button onClick={this.handleClick}>Focus Child Input</button>
      </div>
    );
  }
}
```

## 四、小结

| 场景         | 类组件                           | 函数组件                     |
| ------------ | -------------------------------- | ---------------------------- |
| **创建 Ref** | `this.myRef = React.createRef()` | `const myRef = useRef(null)` |
| **绑定 Ref** | `<input ref={this.myRef} />`     | `<input ref={myRef} />`      |
| **访问 Ref** | `this.myRef.current`             | `myRef.current`              |
| **核心作用** | 直接操作 DOM 或子组件实例        | 同上                         |

- Refs 适用于需要直接操作 DOM 或组件实例的场景，但应避免过度使用（优先通过 `state` 和 `props` 管理状态）。
- 回调 Refs 是历史用法，现在推荐 `React.createRef`（类组件）和 `useRef`（函数组件）。
- 在并发模式中，Refs 依然可以正常工作，可与 `useTransition` 等 Hook 配合使用。