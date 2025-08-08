# React 组件 API 详解

## 一、生命周期方法

组件的生命周期分为挂载、更新、卸载三个阶段，每个阶段有对应的方法用于处理特定逻辑。

### 1. 挂载阶段

- **`constructor(props)`**：初始化状态（`this.state`）和绑定方法的 `this`。
- **`static getDerivedStateFromProps(nextProps, prevState)`**：根据新属性更新状态，返回新状态对象（`null` 表示不更新）。
- **`componentDidMount()`**：组件挂载到 DOM 后调用，适合执行 DOM 操作或数据请求。



**实例**：



jsx







```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 }; // 初始化状态
  }

  static getDerivedStateFromProps(nextProps, prevState) {
    if (nextProps.reset) {
      return { count: 0 }; // 当 props.reset 为 true 时重置 count
    }
    return null;
  }

  componentDidMount() {
    console.log('组件已挂载'); // 可在此处发起 API 请求
  }

  render() {
    return <div>{this.state.count}</div>;
  }
}
```

### 2. 更新阶段

- **`static getDerivedStateFromProps(props, state)`**：同挂载阶段，用于根据属性更新状态。
- **`shouldComponentUpdate(nextProps, nextState)`**：返回布尔值，决定组件是否重绘（优化性能）。
- **`render()`**：渲染组件 UI，是唯一必须实现的方法。
- **`getSnapshotBeforeUpdate(prevProps, prevState)`**：DOM 更新前调用，返回需要保存的快照（如滚动位置）。
- **`componentDidUpdate(prevProps, prevState, snapshot)`**：组件更新后调用，可处理 DOM 操作或数据请求。



**实例**：



jsx







```jsx
class MyComponent extends React.Component {
  shouldComponentUpdate(nextProps, nextState) {
    // 仅当 count 变化时重绘
    return nextState.count !== this.state.count;
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    return { scrollPosition: window.scrollY }; // 保存滚动位置
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    if (snapshot) {
      window.scrollTo(0, snapshot.scrollPosition); // 恢复滚动位置
    }
  }

  render() {
    return <div>{this.state.count}</div>;
  }
}
```

### 3. 卸载阶段

- **`componentWillUnmount()`**：组件卸载前调用，用于清理资源（如定时器、事件监听）。



**实例**：



jsx







```jsx
class MyComponent extends React.Component {
  componentWillUnmount() {
    clearInterval(this.timer); // 清理定时器
    console.log('组件即将卸载');
  }

  render() {
    return <div>{this.state.count}</div>;
  }
}
```

## 二、状态管理（`setState`）

状态（`state`）是组件内部的动态数据，通过 `setState` 方法更新，直接影响组件 UI 渲染。

### 1. `setState` 语法

jsx







```jsx
setState(object nextState[, function callback])
```



- **`nextState`**：包含要更新的状态键值对，React 会合并到当前状态。
- **`callback`**：可选回调函数，在状态更新并完成渲染后执行。

### 2. `setState` 核心特性

- **不能直接修改 `this.state`**：直接修改不会触发重绘，必须通过 `setState`。

  jsx

  

  

  

  ```jsx
  // 错误
  this.state.count = 1;
  
  // 正确
  this.setState({ count: 1 });
  ```

- **状态更新是异步的**：React 会批量处理 `setState` 调用以优化性能，因此不能依赖 `this.state` 或 `this.props` 的当前值计算下一个状态。

  jsx

  

  

  

  ```jsx
  // 错误（可能无法正确更新）
  this.setState({
    count: this.state.count + this.props.increment
  });
  
  // 正确（使用函数形式，确保基于前一个状态计算）
  this.setState((prevState, props) => ({
    count: prevState.count + props.increment
  }));
  ```

- **状态更新会合并**：`setState` 仅更新指定的状态键，未指定的键保持不变。

  jsx

  

  

  

  ```jsx
  // 初始状态：{ name: 'Alice', age: 20 }
  this.setState({ age: 21 }); // 更新后：{ name: 'Alice', age: 21 }
  ```

- **触发重绘**：`setState` 会默认触发组件重绘，除非 `shouldComponentUpdate` 返回 `false`。

### 3. 实例：计数器

jsx







```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { clickCount: 0 };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // 使用函数形式确保状态正确更新
    this.setState((prevState) => ({
      clickCount: prevState.clickCount + 1
    }));
  }

  render() {
    return (
      <h2 onClick={this.handleClick}>
        点我！点击次数为: {this.state.clickCount}
      </h2>
    );
  }
}
```

## 三、事件处理

事件处理用于响应用户交互（如点击、输入），需确保 `this` 指向组件实例。

### 正确实例：按钮点击事件

jsx







```jsx
class EventExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = { message: "未点击" };
    // 方式1：构造函数中绑定 this（推荐）
    this.handleClick = this.handleClick.bind(this);
  }

  // 事件处理函数
  handleClick() {
    this.setState({ message: "已点击" });
  }

  // 方式2：使用箭头函数（自动绑定 this）
  handleArrowClick = () => {
    this.setState({ message: "箭头函数点击" });
  };

  render() {
    return (
      <div>
        <p>{this.state.message}</p>
        {/* 使用绑定后的函数 */}
        <button onClick={this.handleClick}>点击我（bind绑定）</button>
        {/* 使用箭头函数定义的方法 */}
        <button onClick={this.handleArrowClick}>点击我（箭头函数）</button>
      </div>
    );
  }
}
```



**说明**：



- 类方法默认不绑定 `this`，需通过 `bind` 或箭头函数确保 `this` 指向组件实例。
- 箭头函数定义的方法会自动绑定 `this`，无需额外处理。

## 四、属性传递与类型检查

- **属性传递**：通过 `this.props` 接收父组件传递的数据。
- **类型检查**：使用 `PropTypes` 确保属性类型正确（需安装 `prop-types` 库）。



**实例**：



jsx







```jsx
import PropTypes from 'prop-types';

class PropExample extends React.Component {
  render() {
    return <h3>{this.props.title}</h3>;
  }
}

// 类型检查
PropExample.propTypes = {
  title: PropTypes.string.isRequired // title 必须是字符串且必填
};

// 使用组件
<PropExample title="Hello React" />
```

## 五、条件渲染与列表渲染

### 1. 条件渲染

根据状态或属性决定渲染内容：



jsx







```jsx
class ConditionalRender extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isVisible: true };
  }

  toggle = () => {
    this.setState(prevState => ({ isVisible: !prevState.isVisible }));
  };

  render() {
    return (
      <div>
        {this.state.isVisible && <p>显示内容</p>}
        <button onClick={this.toggle}>切换显示</button>
      </div>
    );
  }
}
```

### 2. 列表渲染

通过 `map` 遍历数组生成列表，需为每个元素添加 `key`：

```jsx
class ListRender extends React.Component {
  render() {
    const items = ["苹果", "香蕉", "橙子"];
    return (
      <ul>
        {items.map((item, index) => (
          <li key={index}>{item}</li> // key 确保列表高效更新
        ))}
      </ul>
    );
  }
}
```

## 六、受控组件

表单元素的值由状态控制，通过 `onChange` 事件更新状态：

```jsx
class ControlledForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { inputValue: "" };
  }

  handleChange = (e) => {
    this.setState({ inputValue: e.target.value });
  };

  handleSubmit = (e) => {
    e.preventDefault();
    alert("提交的值：" + this.state.inputValue);
  };

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          type="text"
          value={this.state.inputValue}
          onChange={this.handleChange}
        />
        <button type="submit">提交</button>
      </form>
    );
  }
}
```

## 总结

React 组件 API 涵盖生命周期、状态管理、事件处理等核心功能：

- 生命周期方法控制组件在不同阶段的行为，便于资源管理和 DOM 操作。
- `setState` 是状态更新的核心，需注意其异步性和合并特性。
- 事件处理需确保 `this` 指向正确，常用 `bind` 或箭头函数。
- 属性传递和类型检查增强组件的可靠性，条件渲染与列表渲染实现动态 UI。