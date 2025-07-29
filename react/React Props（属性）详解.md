# React Props（属性）详解

Props 是 React 中组件间传递数据的核心机制，用于从父组件向子组件传递信息，且具有只读特性（子组件不可直接修改）。本文详细介绍 Props 的使用方法、特性及最佳实践。

## 一、Props 基础：传递与接收数据

### 1. 基本用法

- **父组件**：通过属性名传递数据（支持任意 JavaScript 类型）。
- **子组件**：通过 `props` 参数接收数据（函数组件）或 `this.props`（类组件）。

```jsx
// 子组件（函数组件）
function HelloMessage(props) {
  return <h1>Hello, {props.name}!</h1>; // 接收 name 属性
}

// 父组件传递数据
const element = <HelloMessage name="Runoob" />; // 传递 name="Runoob"

// 渲染
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(element); // 输出：<h1>Hello, Runoob!</h1>
```

## 二、默认 Props（defaultProps）

为组件的 props 设置默认值，当父组件未传递该属性时生效。

### 类组件示例

```jsx
class HelloMessage extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

// 设置默认属性
HelloMessage.defaultProps = {
  name: "Runoob" // 未传递 name 时，默认使用 "Runoob"
};

// 未传递 name 属性
const element = <HelloMessage />;
root.render(element); // 输出：<h1>Hello, Runoob!</h1>
```

## 三、传递多个 Props

可同时传递多个属性给子组件，子组件通过 `props` 统一接收。

```jsx
// 子组件：接收多个属性
const UserCard = (props) => {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>Age: {props.age}</p>
      <p>Location: {props.location}</p>
    </div>
  );
};

// 父组件：传递多个属性
const App = () => {
  return (
    <UserCard 
      name="Alice" 
      age={25} 
      location="New York" 
    />
  );
};

root.render(<App />);
```

## 四、Props 类型验证（PropTypes）

使用 `prop-types` 库对 props 进行类型检查，确保数据类型符合预期（开发阶段有效）。

### 步骤

1. **安装依赖**：

   ```bash
   npm install prop-types
   ```

2. **使用示例**：

   ```jsx
   import PropTypes from 'prop-types';
   
   // 子组件：定义 props 类型规则
   const Greeting = (props) => {
     return <h1>Hello, {props.name}!</h1>;
   };
   
   Greeting.propTypes = {
     name: PropTypes.string.isRequired // name 必须是字符串且必填
   };
   
   // 父组件使用
   const App = () => {
     return <Greeting name="Alice" />; // 正确传递（字符串类型）
     // <Greeting /> 会报错（缺少必填属性 name）
     // <Greeting name={123} /> 会报错（类型应为字符串）
   };
   ```

### 常用验证器

| 验证器                   | 说明                                                       |
| ------------------------ | ---------------------------------------------------------- |
| `PropTypes.string`       | 字符串类型                                                 |
| `PropTypes.number`       | 数字类型                                                   |
| `PropTypes.bool`         | 布尔类型                                                   |
| `PropTypes.func`         | 函数类型                                                   |
| `PropTypes.array`        | 数组类型                                                   |
| `PropTypes.object`       | 对象类型                                                   |
| `PropTypes.node`         | 可渲染内容（数字、字符串、元素等）                         |
| `PropTypes.element`      | React 元素                                                 |
| `PropTypes.oneOf([...])` | 枚举类型（值必须为数组中的选项之一）                       |
| `PropTypes.arrayOf(...)` | 特定类型的数组（如 `PropTypes.arrayOf(PropTypes.number)`） |
| `PropTypes.shape({...})` | 特定结构的对象（如 `{ name: PropTypes.string }`）          |
| `.isRequired`            | 标记为必填属性                                             |

## 五、传递回调函数作为 Props

父组件可将函数作为 props 传递给子组件，实现**子组件向父组件通信**（通过调用函数传递数据）。

```jsx
// 父组件：定义回调函数
class ParentComponent extends React.Component {
  constructor(props)//这是类的构造函数，用于初始化组件的状态（this.state）或绑定方法
    {
    super(props);//在使用ES6的类继承时,super()用于调用父类（React.component）的构造函数。
    this.state = { message: "" };
  }

  // 接收子组件传递的消息并更新状态
  handleMessage = (msg) => {
    this.setState({ message: msg });
  };

  render() {
    return (
      <div>
        <ChildComponent onMessage={this.handleMessage} /> {/* 传递函数 */}
        <p>Message from Child: {this.state.message}</p>
      </div>
    );
  }
}

// 子组件：调用父组件传递的函数
const ChildComponent = (props) => {
  const sendMessage = () => {
    props.onMessage("Hello from Child!"); // 向父组件传递数据
  };

  return <button onClick={sendMessage}>Send Message</button>;
};

root.render(<ParentComponent />);
```

## 六、解构 Props 简化代码

在函数组件中，可通过解构赋值直接提取 props 中的属性，简化代码。

```jsx
// 解构前
const Greeting = (props) => {
  return <h1>Hello, {props.name}!</h1>;
};

// 解构后（更简洁）
const Greeting = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};

// 使用
const App = () => {
  return <Greeting name="Alice" />;
};
```

## 七、Props 与 State 的结合使用

父组件可将自身的 `state` 作为 props 传递给子组件，实现数据的 “自顶向下” 流动。

```jsx
// 父组件：管理状态
class WebSite extends React.Component {
  constructor() {
    super();
    this.state = {
      name: "菜鸟教程",
      site: "https://www.runoob.com"
    };
  }

  render() {
    return (
      <div>
        {/* 将 state 作为 props 传递给子组件 */}
        <Name name={this.state.name} />
        <Link site={this.state.site} />
      </div>
    );
  }
}

// 子组件：接收并显示 props
class Name extends React.Component {
  render() {
    return <h1>{this.props.name}</h1>;
  }
}

class Link extends React.Component {
  render() {
    return <a href={this.props.site}>{this.props.site}</a>;
  }
}

root.render(<WebSite />);
```

## 八、总结

- **核心作用**：Props 是父组件向子组件传递数据的桥梁，实现组件间通信。
- **特性**：只读性（子组件不可修改，需通过父组件更新）、单向数据流（自顶向下）。
- **扩展用法**：支持传递函数（实现反向通信）、类型验证（确保数据合法性）、默认值（增强鲁棒性）。