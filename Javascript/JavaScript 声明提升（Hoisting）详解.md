# JavaScript 声明提升（Hoisting）详解

## 一、声明提升的概念

声明提升（Hoisting）是 JavaScript 解析器的一种特性：**函数声明和变量声明会被 “提升” 到其所在作用域的顶部**，但变量的**初始化（赋值）不会被提升**。

这意味着，在代码实际执行前，解析器会先处理所有声明，使开发者可以在声明之前使用变量或函数（尽管不推荐这种写法）。

## 二、变量声明提升

### 1. 核心规则

- **变量声明（`var`）会被提升**，但**初始化（赋值）不会被提升**。
- 使用 `let` 或 `const` 声明的变量也会提升，但存在 “暂时性死区”（TDZ），在声明前使用会报错（详见后续说明）。

### 2. 示例解析

#### 例 1：变量声明提升的表现

```javascript
// 先使用变量，后声明
x = 5;
console.log(x); // 输出 5
var x;
```

- 实际执行顺序（解析器处理后）：

  ```javascript
  var x; // 声明被提升到作用域顶部
  x = 5; // 赋值留在原地
  console.log(x); // 5
  ```

#### 例 2：变量初始化不提升

```javascript
var x = 5;
console.log(x + " " + y); // 输出 "5 undefined"
var y = 7;
```

- 实际执行顺序：

  ```javascript
  var x; // x 声明提升
  var y; // y 声明提升（但未赋值）
  x = 5; // x 初始化留在原地
  console.log(x + " " + y); // y 此时未赋值，为 undefined
  y = 7; // y 初始化留在原地
  ```

### 3. `let`/`const` 与暂时性死区（TDZ）

- `let`和`const`声明的变量也会提升，但在声明语句执行前，变量处于 “暂时性死区”，无法访问（访问会报错），避免了`var`

   声明的不合理行为。

  ```javascript
  console.log(z); // 报错：Cannot access 'z' before initialization
  let z = 10;
  ```

## 三、函数声明提升

### 1. 核心规则

- **函数声明（`function` 关键字定义）会被整体提升**，包括函数体，因此可以在声明前调用函数。
- 函数表达式（如 `var fn = function() {}`）的提升规则同变量声明（仅声明提升，赋值不提升）。

### 2. 示例解析

#### 例 1：函数声明提升

```javascript
// 先调用函数，后声明
myFunction(); // 输出 "Hello World"

function myFunction() {
  console.log("Hello World");
}
```

- 实际执行顺序：

  ```javascript
  // 函数声明被整体提升到作用域顶部
  function myFunction() {
    console.log("Hello World");
  }
  myFunction(); // 调用
  ```

#### 例 2：函数表达式不整体提升

```javascript
// 函数表达式：仅变量声明提升，函数体不提升
myFunction(); // 报错：myFunction is not a function
var myFunction = function() {
  console.log("Hello World");
};
```

- 实际执行顺序：

  ```javascript
  var myFunction; // 变量声明提升
  myFunction(); // 此时 myFunction 是 undefined，调用会报错
  myFunction = function() { // 赋值留在原地
    console.log("Hello World");
  };
  ```

## 四、声明提升的注意事项
1. **避免依赖提升特性**：  
   尽管解析器允许“先使用后声明”，但这种写法会降低代码可读性，易导致逻辑混乱。**建议在作用域顶部集中声明所有变量和函数**，符合常规编程逻辑。

2. **`let`/`const` 与 `var` 的区别**：  
   - `var` 声明的变量提升后默认值为 `undefined`，在声明前使用不会报错（但值为 `undefined`）。  
   - `let`/`const` 声明的变量存在“暂时性死区”，在声明前使用会直接报错，更严格地规范了变量使用。

3. **严格模式（`"use strict"`）**：  
   严格模式下，不允许使用未声明的变量，进一步避免了因提升导致的意外错误。


## 五、总结
- 声明提升是 JavaScript 解析器的特性，表现为**函数声明和变量声明被提升到作用域顶部**，但**初始化不提升**。
- 变量声明中，`var` 存在提升后默认值为 `undefined` 的特性，`let`/`const` 则因“暂时性死区”更严格。
- 最佳实践：**在作用域顶部集中声明所有变量和函数**，避免依赖提升特性，提升代码可读性和可维护性。