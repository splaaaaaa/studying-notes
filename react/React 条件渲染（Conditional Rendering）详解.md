# React 条件渲染（Conditional Rendering）详解

条件渲染是 React 中根据状态或属性动态展示不同 UI 的核心机制，其逻辑与 JavaScript 条件判断完全一致。本文详细介绍 React 条件渲染的多种实现方式及使用场景。

## 一、基础条件渲染：通过 `if` 语句判断

根据组件接收的 `props` 或自身 `state`，使用 `if` 语句决定渲染不同内容。

### 1. 基于 `props` 的条件渲染

```jsx
// 定义两个子组件
function UserGreeting(props) {
  return <h1>欢迎回来！</h1>;
}
function GuestGreeting(props) {
  return <h1>请先注册。</h1>;
}

// 父组件根据 isLoggedIn 属性渲染不同子组件
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />; // 已登录状态
  } else {
    return <GuestGreeting />; // 未登录状态
  }
}

// 使用时通过 props 传递状态
root.render(<Greeting isLoggedIn={false} />); // 显示“请先注册”
```

### 2. 基于 `state` 的条件渲染

结合组件内部状态（`state`），实现动态交互的条件渲染。

```jsx
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    // 初始化状态：未登录
    this.state = { isLoggedIn: false };
    // 绑定事件处理函数的 this
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
  }

  // 登录事件：更新状态为已登录
  handleLoginClick() {
    this.setState({ isLoggedIn: true });
  }

  // 登出事件：更新状态为未登录
  handleLogoutClick() {
    this.setState({ isLoggedIn: false });
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button = null;

    // 根据状态动态渲染登录/登出按钮
    if (isLoggedIn) {
      button = <button onClick={this.handleLogoutClick}>登出</button>;
    } else {
      button = <button onClick={this.handleLoginClick}>登录</button>;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} /> {/* 复用 Greeting 组件 */}
        {button} {/* 渲染动态生成的按钮 */}
      </div>
    );
  }
}
```

## 二、JSX 中的条件渲染语法

除了 `if` 语句，还可在 JSX 中直接嵌入条件表达式，简化代码。

### 1. 逻辑与运算符（`&&`）

当条件为 `true` 时渲染右侧元素，为 `false` 时忽略。

```jsx
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {/* 当有未读消息时，渲染提示文本 */}
      {unreadMessages.length > 0 && (
        <h2>您有 {unreadMessages.length} 条未读信息。</h2>
      )}
    </div>
  );
}

// 使用时传递未读消息列表
const messages = ['React', 'Re: React', 'Re:Re: React'];
root.render(<Mailbox unreadMessages={messages} />); // 显示未读提示
```

**原理**：

- 若 `unreadMessages.length > 0` 为 `true`，则表达式结果为 `<h2>...</h2>`，React 会渲染该元素。
- 若为 `false`，表达式结果为 `false`，React 会忽略该部分。

### 2. 三目运算符（`condition ? a : b`）

适用于二选一的条件渲染场景，比 `if` 语句更简洁。

```jsx
// 简单文本渲染
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      用户当前<b>{isLoggedIn ? '已' : '未'}</b>登录。
    </div>
  );
}

// 组件渲染
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <button onClick={this.handleLogoutClick}>登出</button>
      ) : (
        <button onClick={this.handleLoginClick}>登录</button>
      )}
    </div>
  );
}
```

## 三、阻止组件渲染

在极少数情况下，可让组件的 `render` 方法返回 `null`，使组件不渲染任何内容（但生命周期方法仍会执行）。

```jsx
// 警告组件：根据 props.warn 决定是否渲染
function WarningBanner(props) {
  if (!props.warn) {
    return null; // 不渲染任何内容
  }
  return <div className="warning">警告！</div>;
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = { showWarning: true };
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  // 切换警告显示状态
  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? '隐藏警告' : '显示警告'}
        </button>
      </div>
    );
  }
}
```

## 四、总结

React 条件渲染的核心是**利用 JavaScript 条件逻辑控制 UI 输出**，主要方式包括：

- **`if` 语句**：适合复杂逻辑或多分支条件。
- **`&&` 运算符**：适合简单的 “存在即渲染” 场景。
- **三目运算符**：适合二选一的简洁渲染。
- **返回 `null`**：阻止组件渲染。