# React 组件生命周期详解

## 一、生命周期的三个核心状态

React 组件的生命周期可划分为三个主要阶段，每个阶段对应特定的方法调用，用于控制组件在不同阶段的行为：

| 状态           | 描述                                 |
| -------------- | ------------------------------------ |
| **Mounting**   | 组件实例被创建并插入到 DOM 中        |
| **Updating**   | 组件因 props 或 state 变化而重新渲染 |
| **Unmounting** | 组件从 DOM 中移除（卸载）            |

## 二、各阶段的生命周期方法

### 1. 挂载阶段（Mounting）

当组件首次被创建并插入 DOM 时，按以下顺序调用方法：

- **`constructor(props)`**
  - 组件初始化时调用，用于设置初始 state 或绑定方法的 `this`。
  - 必须调用 `super(props)` 以继承父组件的属性。
- **`static getDerivedStateFromProps(props, state)`**
  - 在 `render` 方法前调用（初始挂载和后续更新时均会触发）。
  - 用于根据 props 动态更新 state，返回新的状态对象（返回 `null` 表示不更新）。
- **`render()`**
  - class 组件中**唯一必须实现**的方法，用于渲染组件的 UI 结构。
  - 返回 React 元素（如 JSX），描述组件的外观。
- **`componentDidMount()`**
  - 组件挂载到 DOM 后立即调用。
  - 适合执行 DOM 操作（如获取元素尺寸）、数据请求（如 API 调用）或设置定时器。

### 2. 更新阶段（Updating）

当组件的 `props` 或 `state` 发生变化时，触发更新流程，方法调用顺序如下：

- **`static getDerivedStateFromProps(props, state)`**
  - 与挂载阶段作用一致，用于根据新 props 更新 state。
- **`shouldComponentUpdate(nextProps, nextState)`**
  - 返回布尔值，决定组件是否需要重新渲染（优化性能）。
  - 若返回 `false`，则跳过后续更新流程（`render` 及之后的方法均不执行）。
- **`render()`**
  - 重新渲染组件 UI（与挂载阶段逻辑一致）。
- **`getSnapshotBeforeUpdate(prevProps, prevState)`**
  - 在 DOM 更新前调用，用于捕获 DOM 信息（如滚动位置）。
  - 返回的值会作为参数传递给 `componentDidUpdate`。
- **`componentDidUpdate(prevProps, prevState, snapshot)`**
  - 组件更新后调用，可处理 DOM 操作或根据前后状态差异执行逻辑（如数据请求）。

### 3. 卸载阶段（Unmounting）

当组件从 DOM 中移除时，仅调用以下方法：

- `componentWillUnmount()`
  - 组件卸载前调用，用于清理资源（如定时器、事件监听、订阅等），避免内存泄漏。

## 三、实例解析生命周期流程

### 1. 时钟组件（`Clock`）

展示挂载、更新、卸载阶段的方法配合，实现每秒更新时间：

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() }; // 初始化状态
  }

  componentDidMount() {
    // 挂载后设置定时器，每秒更新时间
    this.timerID = setInterval(() => this.tick(), 1000);
  }

  componentWillUnmount() {
    // 卸载前清理定时器
    clearInterval(this.timerID);
  }

  tick() {
    // 更新状态，触发重新渲染
    this.setState({ date: new Date() });
  }

  render() {
    return (
      <div>
        <h1>Hello, Runoob!</h1>
        <h2>现在时间是：{this.state.date.toLocaleTimeString()}。</h2>
      </div>
    );
  }
}
```

**流程说明**：

- `constructor` 初始化时间状态。
- `componentDidMount` 启动定时器，每秒调用 `tick` 方法。
- `tick` 通过 `setState` 更新时间，触发 `render` 重新渲染。
- 组件卸载时，`componentWillUnmount` 清除定时器，避免资源泄漏。

### 2. 透明度动画组件（`Hello`）

展示 `componentDidMount` 在挂载后持续更新组件状态的场景：

```jsx
class Hello extends React.Component {
  constructor(props) {
    super(props);
    this.state = { opacity: 1.0 }; // 初始透明度
  }

  componentDidMount() {
    // 挂载后设置定时器，每100ms更新透明度
    this.timer = setInterval(
      function() {
        let opacity = this.state.opacity;
        opacity -= 0.05;
        if (opacity < 0.1) opacity = 1.0;
        this.setState({ opacity: opacity });
      }.bind(this),
      100
    );
  }

  render() {
    return (
      <div style={{ opacity: this.state.opacity }}>
        Hello {this.props.name}
      </div>
    );
  }
}
```

**核心逻辑**：

- `componentDidMount` 中启动定时器，动态修改 `opacity` 状态。
- `render` 根据 `opacity` 状态更新元素样式，实现渐变动画。

### 3. 数据更新组件（`Button` 与 `Content`）

展示父子组件交互时的生命周期调用：

```jsx
class Button extends React.Component {
  constructor(props) {
    super(props);
    this.state = { data: 0 };
    this.setNewNumber = this.setNewNumber.bind(this);
  }

  setNewNumber() {
    this.setState({ data: this.state.data + 1 }); // 更新数据，触发子组件更新
  }

  render() {
    return (
      <div>
        <button onClick={this.setNewNumber}>INCREMENT</button>
        <Content myNumber={this.state.data} />
      </div>
    );
  }
}

class Content extends React.Component {
  componentDidMount() {
    console.log("Component DID MOUNT!"); // 挂载时触发
  }

  shouldComponentUpdate(newProps) {
    return true; // 允许更新（默认返回true）
  }

  componentDidUpdate(prevProps) {
    console.log("Component DID UPDATE!"); // 更新后触发
  }

  componentWillUnmount() {
    console.log("Component WILL UNMOUNT!"); // 卸载时触发
  }

  render() {
    return <h3>{this.props.myNumber}</h3>;
  }
}
```

**调用顺序**：

1. 初始渲染：`Content` 触发 `componentDidMount`。
2. 点击按钮：`Button` 状态更新，`Content` 因 `props` 变化触发 `shouldComponentUpdate` → `render` → `componentDidUpdate`。

## 四、总结

- **核心阶段**：挂载（初始化）、更新（响应状态 / 属性变化）、卸载（资源清理）。
- 关键方法：
  - 挂载：`constructor`（初始化）、`componentDidMount`（DOM 操作 / 数据请求）。
  - 更新：`shouldComponentUpdate`（性能优化）、`componentDidUpdate`（更新后处理）。
  - 卸载：`componentWillUnmount`（清理资源）。
- **`render` 方法**：唯一必须实现的方法，负责渲染 UI，不允许执行副作用操作（如定时器、请求）。