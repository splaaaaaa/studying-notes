# JavaScript 中 `javascript:void(0)` 详解

`javascript:void(0)` 是 JavaScript 中用于控制链接行为的常见语法，核心在于 `void` 关键字的特性。本文详细解释其含义、用法及与相关语法的区别。

## 一、`void` 关键字的核心作用

`void` 是 JavaScript 的运算符，**用于执行一个表达式，但不返回任何值（返回 `undefined`）**。

- 语法格式：
  - `void 表达式`（如 `void 0`、`void (x + 1)`）
  - `void(表达式)`（如 `void(0)`、`void(alert('hello'))`）
  - 也可用于函数：`void func()` 或 `void(func())`

## 二、`javascript:void(0)` 的含义与应用

`javascript:void(0)` 是 `void` 运算符的典型用法，**表示执行表达式 `0` 但不返回任何结果**，常用来禁用链接的默认行为。

### 1. 禁用链接跳转

当 `<a>` 标签的 `href` 属性设为 `javascript:void(0)` 时，点击链接不会发生跳转（无任何默认动作），仅作为 “死链接” 存在。
**示例**：

```html
<a href="javascript:void(0)">单击此处什么也不会发生</a>
```

### 2. 执行表达式但不跳转

`void` 后的表达式会被执行，但链接不会跳转。可用于在点击时触发函数或代码，同时阻止默认跳转。
**示例**：

```html
<a href="javascript:void(alert('点击触发警告'))">点我显示警告</a>
```

- 点击链接时，`alert('点击触发警告')` 会执行（弹出提示），但页面不会跳转。

### 3. 对变量赋值的影响

`void` 执行表达式后返回 `undefined`，因此可用于给变量赋值 `undefined`。
**示例**：

```javascript
function getValue() {
  var a, b, c;
  a = void (b = 5, c = 7); // 执行 b=5 和 c=7，但 a 被赋值为 undefined
  document.write(`a = ${a}, b = ${b}, c = ${c}`); // 输出：a = undefined, b = 5, c = 7
}
getValue();
```

## 三、`href="#"` 与 `href="javascript:void(0)"` 的区别

| 语法                        | 特性与用途                                                   |
| --------------------------- | ------------------------------------------------------------ |
| `href="#"`                  | - 包含位置信息，默认锚点为页面顶部（`#top`）； - 可通过 `#id` 定位到页面中 `id` 匹配的元素（如 `#pos` 定位到 `<p id="pos">`）； - 点击时会使页面滚动到指定位置（若无指定 `id` 则滚动到顶部）。 |
| `href="javascript:void(0)"` | - 无位置信息，仅作为 “死链接”； - 点击时不会滚动页面，也不会跳转； - 适合纯粹需要禁用跳转的场景（仅触发自定义事件）。 |

### 示例对比

```html
<!-- 死链接：点击无反应 -->
<a href="javascript:void(0);">点我没有反应</a>

<!-- 定位链接：点击滚动到 id="pos" 的元素 -->
<a href="#pos">点我定位到尾部</a>

<br><br><br><br><br> <!-- 模拟长页面 -->
<p id="pos">尾部定位点</p>
```

## 四、总结

1. **`void` 运算符**：执行表达式但返回 `undefined`，用于控制代码执行而不产生返回值。
2. **`javascript:void(0)`**：常用作 `<a>` 标签的 `href` 属性，禁用链接默认跳转，适合需要点击触发事件但不跳转的场景。
3. 与 `href="#"` 的区别：`#` 用于页面内定位，`javascript:void(0)` 用于纯死链接，避免不必要的页面滚动。

根据需求选择合适的语法：需定位时用 `#id`，需禁用跳转时用 `javascript:void(0)`。