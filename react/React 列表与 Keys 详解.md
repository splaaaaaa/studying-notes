# React 列表与 Keys 详解

## 一、列表的创建（使用 `map()` 方法）

在 React 中，可通过 JavaScript 的 `map()` 方法遍历数组生成列表，这是创建动态列表的核心方式。

### 基础示例

```jsx
// 直接生成列表
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) => <li>{number}</li>);

// 封装为组件
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) => <li>{number}</li>);
  return <ul>{listItems}</ul>;
}
```

## 二、`key` 的作用与必要性

`key` 是 React 列表渲染中的特殊属性，用于帮助 React 识别列表中元素的变化（新增、删除、排序等），从而优化渲染性能。

- **缺失 `key` 的问题**：若未给列表元素分配 `key`，React 会抛出警告 `a key should be provided for list items`。
- **核心作用**：当列表元素发生变化时，React 通过 `key` 快速定位变化的元素，避免不必要的 DOM 重新渲染。

## 三、`key` 的取值规则

`key` 的值需满足**在兄弟节点间唯一**，具体取值优先级如下：

1. **优先使用数据中的唯一 `id`**
   若列表数据来自数据库等有唯一标识的来源，直接使用 `id` 作为 `key`：

   ```jsx
   const todoItems = todos.map((todo) => <li key={todo.id}>{todo.text}</li>);
   ```

2. **无 `id` 时使用索引（`index`）**
   仅当数据无确定 `id` 时使用索引，且列表**不会重新排序**（排序会导致 `key` 随位置变化，降低性能）：

   ```jsx
   const todoItems = todos.map((todo, index) => (
     // 仅在无唯一 id 时使用
     <li key={index}>{todo.text}</li>
   ));
   ```

## 四、`key` 的作用范围：兄弟节点

在 React 中，key 的 “兄弟节点” 指的是同一列表中由同一个循环（如 `map` 生成）的相邻元素。简单来说：key 只需要在 “同层级、同父元素下的同级元素” 之间保持唯一，超出这个范围则无需唯一。

- 示例说明：

  ```jsx
  // 两个独立列表，key 可重复（非兄弟节点）
  function Blog(props) {
    // 侧边栏列表（兄弟节点组A）
    const sidebar = props.posts.map((post) => (
      <li key={post.id}>{post.title}</li>
    ));
    // 内容列表（兄弟节点组B）
    const content = props.posts.map((post) => (
      <div key={post.id}> {/* 与 sidebar 中的 key 重复不影响 */}
        <h3>{post.title}</h3>
        <p>{post.content}</p>
      </div>
    ));
    return (
      <div>
        <ul>{sidebar}</ul>
        <div>{content}</div>
      </div>
    );
  }
  ```

  上述代码中，`sidebar`和`content`是两个独立列表，即使`key`相同（均为`post.id`），也不会产生冲突，因为它们属于不同的兄弟节点组。

## 五、`key` 在组件提取中的正确用法

当从列表中提取组件时，`key` 应保留在数组中的组件引用上，而非组件内部的元素上。

### 错误示例

```jsx
// 错误：key 不应放在组件内部
function ListItem(props) {
  return <li key={props.value.toString()}>{props.value}</li>; // 错误位置
}

function NumberList(props) {
  return (
    <ul>
      {props.numbers.map((number) => (
        <ListItem value={number} /> // 缺少 key
      ))}
    </ul>
  );
}
```

### 正确示例

```jsx
// 正确：key 放在数组中的组件上
function ListItem(props) {
  return <li>{props.value}</li>; // 无需 key
}

function NumberList(props) {
  return (
    <ul>
      {props.numbers.map((number) => (
        <ListItem key={number.toString()} value={number} /> // 正确位置
      ))}
    </ul>
  );
}
```

## 六、`key` 与组件属性的区别

- `key` 是 React 内部使用的标识，**不会传递给组件**，组件无法通过 `props.key` 获取其值。

- 若组件需要使用与`key`

  相同的值，需单独作为属性传递：

  ```jsx
  const content = posts.map((post) => (
    <Post
      key={post.id} // React 内部使用
      id={post.id}  // 组件可通过 props.id 获取
      title={post.title}
    />
  ));
  ```

## 七、JSX 中嵌入 `map()` 表达式

JSX 允许在大括号中直接嵌入 `map()` 方法，简化代码结构：

```jsx
function NumberList(props) {
  return (
    <ul>
      {props.numbers.map((number) => (
        <ListItem key={number.toString()} value={number} />
      ))}
    </ul>
  );
}
```

> 注意：若 `map()` 嵌套层级过深，建议提取为独立组件，提升代码可读性。

## 八、总结

- **列表渲染**：通过 `map()` 方法遍历数组生成列表，是 React 中创建动态列表的基础。
- **`key` 的核心**：帮助 React 识别列表元素的变化，优化渲染性能，需在兄弟节点间保持唯一。
- **`key` 的取值**：优先使用数据的唯一 `id`，无 `id` 时可临时使用索引（避免在可排序列表中使用）。
- **正确用法**：`key` 应放在数组生成的组件上，而非组件内部；且不会传递给组件，需单独作为属性传递。