# JavaScript 中 `typeof`、`null` 和 `undefined` 总结

## 一、`typeof` 操作符

`typeof` 是 JavaScript 中用于检测变量数据类型的操作符，返回值为表示数据类型的字符串。

### 1. 基本用法与实例

| 变量 / 值                | `typeof` 返回值 | 说明                         |
| ------------------------ | --------------- | ---------------------------- |
| `"John"`                 | `"string"`      | 字符串类型                   |
| `3.14`                   | `"number"`      | 数字类型（包括整数、浮点数） |
| `false`                  | `"boolean"`     | 布尔类型                     |
| `[1, 2, 3, 4]`           | `"object"`      | 数组是特殊的对象类型         |
| `{name: 'John', age:34}` | `"object"`      | 对象类型                     |
| `function(){}`           | `"function"`    | 函数类型                     |
| `Symbol()`               | `"symbol"`      | ES6 新增的符号类型           |
| `BigInt(10)`             | `"bigint"`      | ES2020 新增的大整数类型      |
| `undefined`              | `"undefined"`   | 未定义类型                   |
| `null`                   | `"object"`      | 特殊值（历史原因）           |

### 2. 特殊情况与解决方案

- **数组检测**：
  由于数组属于特殊对象类型，`typeof` 检测数组会返回 `"object"`，需用以下方法正确检测：

  ```javascript
  Array.isArray([1, 2, 3]); // true（推荐）
  [1, 2, 3] instanceof Array; // true
  ```

- **`null` 检测**：
  `typeof null` 返回 `"object"`（设计缺陷），正确检测需用严格相等：

  ```javascript
  myVar === null; // true 或 false
  ```

### 3. 实际应用场景

- **检测未定义变量**：

  ```javascript
  if (typeof variable === "undefined") {
      // 处理变量未定义的逻辑
  }
  ```

- **检测函数是否存在**：

  ```javascript
  if (typeof myFunction === "function") {
      // 函数存在时执行
  }
  ```

## 二、`null` 与 `undefined`

### 1. 基本概念

- **`undefined`**：表示变量未被赋值（如声明后未初始化），类型为 `undefined`。
  示例：

  ```javascript
  var person; // 值为 undefined，类型为 undefined
  person = undefined; // 主动设为 undefined
  ```

- **`null`**：表示 “空值” 或 “无对象引用”，类型为 `object`（历史原因）。
  示例：

  ```javascript
  var person = null; // 值为 null，类型为 object
  ```

### 2. 区别与联系

| 对比维度          | `undefined`                    | `null`                 |
| ----------------- | ------------------------------ | ---------------------- |
| `typeof` 返回值   | `"undefined"`                  | `"object"`             |
| 严格相等（`===`） | `null === undefined` → `false` | 同上                   |
| 松散相等（`==`）  | `null == undefined` → `true`   | 同上                   |
| 含义              | 变量未赋值                     | 变量值为空（主动清空） |

## 总结

- `typeof` 是检测变量类型的基础工具，但存在局限性（如数组、`null` 的误判），需结合其他方法（如 `Array.isArray()`）补充使用。
- `null` 和 `undefined` 都表示 “空”，但含义和类型不同，松散相等时返回 `true`，严格相等时返回 `false`。
- 实际开发中需根据场景选择正确的检测方式，避免因类型误判导致逻辑错误。