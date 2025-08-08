# React 表单事件详解

## 一、React 表单处理的核心逻辑

React 表单处理的核心是通过 **受控组件** 实现状态与 UI 的同步：将表单元素的值绑定到组件的 `state`，并通过 `onChange` 事件监听输入变化，实时更新 `state`。

```jsx
class FormExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = { username: '' };
  }

  handleChange = (e) => {
    // 通过 e.target.value 获取输入值，并更新 state
    this.setState({ username: e.target.value });
  };

  render() {
    return (
      <input
        type="text"
        value={this.state.username} // 绑定 state
        onChange={this.handleChange} // 监听变化
      />
    );
  }
}
```

## 二、`event.target.value` 的原理：基于原生 JavaScript 机制

`event.target.value`（或简写为 `e.target.value`）能准确获取表单元素的值，并非 React 单独设计的特性，而是基于 **JavaScript 原生事件机制**：

### 1. `event.target` 是 JavaScript 事件的原生属性

在浏览器的 JavaScript 事件模型中：
当一个事件（如 `onChange`、`onClick`）被触发时，浏览器会自动创建一个 **事件对象（Event Object）**，其中包含了与事件相关的所有信息。
`event.target` 是这个事件对象的原生属性，它 **指向触发当前事件的 DOM 元素本身（即事件的 “源头”）**。

**示例（原生 HTML/JS）**：

```html
<!-- 普通 HTML 中 -->
<input type="text" onchange="handleChange(event)">

<script>
function handleChange(event) {
  // event.target 就是触发事件的 <input> 元素
  console.log(event.target); // 输出 <input type="text"> 元素
  console.log(event.target.value); // 输出输入框的当前值
}
</script>
```

### 2. `value` 是表单元素的原生属性

对于表单元素（如 `<input>`、`<select>`、`<textarea>`）：
浏览器原生就为它们定义了 `value` 属性，用于存储或表示元素的当前值：

- `<input type="text">` 的 `value` 是输入的文本；
- `<select>` 的 `value` 是选中选项的 `value` 属性值；
- `<textarea>` 的 `value` 是输入的多行文本。

因此，当 `event.target` 指向一个表单元素时，`event.target.value` 自然就能获取到该元素的原生 `value` 属性值。

### 3. React 只是复用了原生机制

React 中的事件处理（如 `onChange`）虽然对浏览器原生事件做了一层封装（称为 **合成事件 SyntheticEvent**），但 **保留了与原生事件一致的核心属性和行为**，包括 `event.target` 和 `value`。

React 不会额外 “设置” 指向关系，而是直接复用了浏览器的原生逻辑：

- 当用户在输入框中输入时，触发 `onChange` 事件；
- React 将原生事件封装后传递给处理函数，`event.target` 依然指向触发事件的输入框 DOM 元素；
- 因此通过 `event.target.value` 就能直接获取该输入框的原生 `value` 值。

## 三、多表单元素的统一处理：利用 `name` 属性

对于包含多个表单元素的场景，可通过 `event.target.name` 区分不同元素，结合 `event.target.value` 实现统一处理：

```jsx
class MultiFormExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: '',
      age: 0
    };
  }

  handleChange = (e) => {
    // 通过 e.target.name 区分不同元素
    const { name, value } = e.target;
    this.setState({ [name]: value });
  };

  render() {
    return (
      <div>
        <input
          type="text"
          name="username" // 定义 name 属性
          value={this.state.username}
          onChange={this.handleChange}
        />
        <input
          type="number"
          name="age" // 定义 name 属性
          value={this.state.age}
          onChange={this.handleChange}
        />
      </div>
    );
  }
}
```

**原理**：
`event.target.name` 同样基于原生机制 ——`name` 是表单元素的原生属性，`event.target` 指向元素后，可直接获取其 `name` 值，从而区分不同字段。

## 四、总结

- **核心逻辑**：React 表单处理通过绑定 `state` 和监听 `onChange` 事件实现，而获取输入值的 `e.target.value` 依赖原生 JavaScript 机制。
- **`event.target.value` 的本质**：
  浏览器原生机制保证了 `event.target` 指向事件源头元素，表单元素的原生 `value` 属性存储当前值，React 仅复用这一机制。
- **扩展**：多元素处理可通过 `event.target.name` 区分，原理与 `value` 一致，均基于原生属性。