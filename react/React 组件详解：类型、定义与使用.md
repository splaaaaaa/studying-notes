# React 组件详解：类型、定义与使用

React 组件是构建应用的基本单元，可分为**函数组件**和**类组件**两种类型。本文详细介绍组件的定义方式、使用方法及复合组件的构建。

## 一、组件的基本概念

- **定义**：组件是独立可复用的代码块，用于封装 UI 逻辑和展示内容。
- **作用**：将复杂 UI 拆分为小型、可维护的单元，提高代码复用性和可读性。
- **命名规范**：自定义组件名以**大写字母开头**（如 `Welcome`），区分于原生 HTML 标签（小写字母开头）。

## 二、函数组件

函数组件是最简洁的组件定义方式，本质是一个返回 React 元素的 JavaScript 函数，接收 `props` 作为输入参数。

### 1. 定义与使用

#### （1）创建函数组件

```jsx
// 文件路径：src/Welcome.js
import React from 'react';

// 函数组件：接收 props 并返回 JSX
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

export default Welcome; // 导出组件
```

#### （2）渲染组件

在入口文件中导入并渲染组件，通过属性传递数据：

```jsx
// 文件路径：src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import Welcome from './Welcome'; // 导入组件

const root = ReactDOM.createRoot(document.getElementById('root'));
// 传递 name 属性给组件
root.render(<Welcome name="World" />); 
// 页面输出：<h1>Hello, World!</h1>
```

### 2. 特点

- 语法简洁，仅需一个函数。
- 适用于**无状态组件**（仅展示数据，不涉及状态管理）。
- 通过 `props` 接收外部传入的数据（只读，不可修改）。

## 三、类组件

类组件使用 ES6 类语法定义，继承自 `React.Component`，适用于需要**状态管理**或**生命周期方法**的场景。

### 1. 定义与使用

#### （1）创建类组件

```jsx
// 文件路径：src/Welcome.js
import React, { Component } from 'react';

// 类组件：继承 Component 并实现 render 方法
class Welcome extends Component {
  render() {
    // 通过 this.props 访问属性
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

export default Welcome;
```

#### （2）渲染组件

与函数组件的渲染方式一致：

```jsx
// 文件路径：src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import Welcome from './Welcome';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Welcome name="World" />); 
// 页面输出：<h1>Hello, World!</h1>
```

### 2. 特点

- 需继承 `React.Component` 并实现 `render()` 方法（返回 JSX）。
- 通过 `this.props` 接收属性，`this.state` 管理内部状态。
- 支持生命周期方法（如 `componentDidMount`），适合复杂逻辑。

## 四、组件属性（Props）

`props` 是组件的输入参数，用于父组件向子组件传递数据，**只读不可修改**。

### 1. 传递与接收属性

```jsx
// 子组件：接收 name 属性
function HelloMessage(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// 父组件：传递 name 属性
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<HelloMessage name="Runoob" />); 
// 输出：<h1>Hello, Runoob!</h1>
```

### 2. 注意事项

- 属性名避免使用 JavaScript 保留字（如 `class` 需改为 `className`，`for` 需改为 `htmlFor`）。
- 组件只能通过 `props` 接收外部数据，无法直接修改 `props` 的值。

## 五、复合组件

复合组件是将多个组件组合成一个新组件，实现功能拆分与复用。

### 示例：组合多个组件

```jsx
// 子组件 1：展示网站名称
function Name(props) {
  return <h1>网站名称：{props.name}</h1>;
}

// 子组件 2：展示网站地址
function Url(props) {
  return <h1>网站地址：{props.url}</h1>;
}

// 子组件 3：展示网站小名
function Nickname(props) {
  return <h1>网站小名：{props.nickname}</h1>;
}

// 复合组件：组合上述子组件
function App() {
  return (
    <div>
      <Name name="菜鸟教程" />
      <Url url="http://www.runoob.com" />
      <Nickname nickname="Runoob" />
    </div>
  );
}

// 渲染复合组件
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

### 特点

- 拆分复杂 UI 为小型组件，职责单一。
- 父组件通过组合子组件构建完整页面，提高代码可维护性。

## 六、总结

| 组件类型 | 定义方式                 | 适用场景                     | 数据接收方式     |
| -------- | ------------------------ | ---------------------------- | ---------------- |
| 函数组件 | 普通 JavaScript 函数     | 简单展示、无状态逻辑         | `props` 作为参数 |
| 类组件   | ES6 类（继承 Component） | 复杂逻辑、状态管理、生命周期 | `this.props`     |

组件是 React 开发的核心，合理拆分和组合组件可大幅提升应用的可扩展性和复用性。实际开发中，推荐优先使用**函数组件**（结合 Hooks 管理状态），类组件逐渐被替代。