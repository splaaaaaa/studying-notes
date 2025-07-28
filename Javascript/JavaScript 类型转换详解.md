# JavaScript 类型转换详解

## 一、数据类型及检测

### 1. 基本数据类型

JavaScript 包含 6 种基本数据类型：

- 原始类型：`string`（字符串）、`number`（数字）、`boolean`（布尔值）、`symbol`（符号，ES6 新增）
- 特殊类型：`null`（空值）、`undefined`（未定义）
- 对象类型：`object`（对象）、`function`（函数）、`Array`（数组）、`Date`（日期）等

### 2. 类型检测方法

| 检测方式           | 适用场景                              | 局限性                                                       |
| ------------------ | ------------------------------------- | ------------------------------------------------------------ |
| `typeof` 操作符    | 检测基本类型（如 `string`、`number`） | 无法区分数组、日期、对象（均返回 `object`）；`typeof null` 返回 `object` |
| `constructor` 属性 | 区分数组、日期等对象类型              | 若对象原型被修改，可能失效                                   |
| `Array.isArray()`  | 专门检测数组                          | 仅适用于数组                                                 |

#### 示例：

```javascript
// typeof 检测
typeof "Hello"; // "string"
typeof 123; // "number"
typeof [1, 2, 3]; // "object"（局限性）

// constructor 检测
[1, 2, 3].constructor === Array; // true（判断数组）
new Date().constructor === Date; // true（判断日期）
```

## 二、手动类型转换

手动转换指通过特定函数或方法主动将一种类型转换为另一种类型，常见场景包括：

### 1. 转换为字符串（`String()` 或 `toString()`）

- **`String()` 函数**：可转换任意类型（包括 `null` 和 `undefined`）

  ```javascript
  String(123); // "123"（数字 → 字符串）
  String(true); // "true"（布尔值 → 字符串）
  String(null); // "null"（null → 字符串）
  String(undefined); // "undefined"（undefined → 字符串）
  String([1, 2, 3]); // "1,2,3"（数组 → 字符串）
  ```

- **`toString()` 方法**：适用于数字、布尔值、数组等（`null` 和 `undefined` 无此方法）

  ```javascript
  (123).toString(); // "123"
  true.toString(); // "true"
  [1, 2, 3].toString(); // "1,2,3"
  ```

### 2. 转换为数字（`Number()` 及相关方法）

- **`Number()` 函数**：转换任意类型为数字

  ```javascript
  Number("123"); // 123（字符串 → 数字）
  Number(true); // 1（布尔值 → 数字，true→1，false→0）
  Number(null); // 0（null → 0）
  Number(undefined); // NaN（undefined → 非数字）
  Number("123abc"); // NaN（无法转换的字符串 → NaN）
  ```

- **解析字符串为数字**：

  parseInt()：解析为整数（忽略小数部分）

  ```javascript
  parseInt("123.45"); // 123
  parseInt("12abc"); // 12（只解析开头数字）
  ```

  parseFloat()：解析为浮点数

  ```javascript
  parseFloat("123.45"); // 123.45
  parseFloat("12.34.56"); // 12.34（只解析第一个小数点）
  ```

### 3. 转换为布尔值（`Boolean()`）

- 除以下值转换为`false`外，其余均为`true`：

  ```javascript
  Boolean(0); // false
  Boolean(""); // false（空字符串）
  Boolean(null); // false
  Boolean(undefined); // false
  Boolean(NaN); // false
  
  Boolean(1); // true
  Boolean("hello"); // true
  Boolean([]); // true（空数组为 true）
  Boolean({}); // true（空对象为 true）
  ```

### 4. 数字格式化转换

数字对象的方法可将数字转换为特定格式的字符串：`toFixed(n)`：保留`n`位小数

```javascript
(123.456).toFixed(2); // "123.46"
```

- `toExponential(n)`：科学计数法表示（保留`n`位小数）

  ```javascript
  (123.45).toExponential(1); // "1.2e+2"
  ```

- `toPrecision(n)`：保留`n`位有效数字

  ```javascript
  (123.45).toPrecision(3); // "123"
  ```

## 三、自动类型转换

JavaScript 在运算或比较时，会自动将不同类型转换为兼容类型，常见场景包括：

### 1. 算术运算中的转换

- 字符串与数字相加时，字符串会被转换为数字（若无法转换则为`NaN`）

  ```javascript
  "5" + 3; // "53"（字符串拼接，数字 → 字符串）
  "5" - 3; // 2（减法运算，字符串 → 数字）
  "abc" * 2; // NaN（无法转换为数字）
  ```

  - `null`转换为`0`，转换为`NaN`

  ```javascript
  null + 5; // 5（null → 0）
  undefined + 5; // NaN
  ```

### 2. 比较运算中的转换

- 不同类型比较时，会先转换为数字再比较

  ```javascript
  "10" == 10; // true（字符串 → 数字）
  "10" === 10; // false（严格相等，不转换类型）
  true == 1; // true（布尔值 → 数字，true→1）
  ```

### 3. 输出时的转换

- 调用`alert()`或`console.log()`时，自动转换为字符串

  ```javascript
  alert([1, 2, 3]); // 输出 "1,2,3"（数组 → 字符串）
  console.log({name: "John"}); // 输出 "[object Object]"（对象 → 字符串）
  ```

## 四、常见类型转换结果表

| 原始值         | 转换为数字 | 转换为字符串        | 转换为布尔值 |
| -------------- | ---------- | ------------------- | ------------ |
| `123`          | `123`      | `"123"`             | `true`       |
| `"123"`        | `123`      | `"123"`             | `true`       |
| `true`         | `1`        | `"true"`            | `true`       |
| `false`        | `0`        | `"false"`           | `false`      |
| `null`         | `0`        | `"null"`            | `false`      |
| `undefined`    | `NaN`      | `"undefined"`       | `false`      |
| `[]`（空数组） | `0`        | `""`                | `true`       |
| `[1, 2]`       | `NaN`      | `"1,2"`             | `true`       |
| `{}`（空对象） | `NaN`      | `"[object Object]"` | `true`       |

## 总结

- 类型转换是 JavaScript 灵活性的体现，但也需注意潜在问题（如 `==` 与 `===` 的区别）。
- 手动转换推荐使用 `String()`、`Number()` 等函数，确保逻辑清晰。
- 自动转换需留意场景（如字符串拼接与算术运算的差异），避免意外结果。
- 严格相等运算符 `===` 不进行类型转换，推荐优先使用以减少错误。