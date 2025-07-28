# JavaScript 严格模式（Strict Mode）详解

## 一、严格模式的基础

### 1. 定义

严格模式（Strict Mode）是 ECMAScript 5 新增的一种代码运行模式，通过严格的语法规则约束 JavaScript 代码执行，旨在消除语言中的不合理特性、增强安全性，并提高代码的可维护性。

### 2. 启用方式

通过在脚本或函数的**最顶部**添加指令 `'use strict';` 启用，位置决定作用域：

- 全局严格模式：在脚本开头声明，作用于整个脚本（除嵌套在非严格模式函数内的代码）。

  ```javascript
  'use strict'; // 全局启用严格模式
  x = 10; // 报错（x 未声明）
  ```

- 局部严格模式：在函数内声明，仅作用于该函数。

  ```javascript
  function strictFunc() {
    'use strict'; // 函数内启用严格模式
    y = 20; // 报错（y 未声明）
  }
  z = 30; // 不报错（函数外非严格模式）
  ```

## 二、严格模式的核心作用

1. **消除语法不合理性**：禁止 JavaScript 中不严谨的语法（如未声明变量的使用）。
2. **增强代码安全性**：限制对关键资源的误操作（如删除只读属性）。
3. **提高执行效率**：减少编译器的优化障碍，使代码运行更快。
4. **为未来版本铺垫**：禁用已过时或计划移除的特性，兼容未来语言发展。

## 三、严格模式对 `this` 指向的影响（重点）

`this` 的指向在严格模式与非严格模式下有显著差异，这是最容易混淆的点之一：

### 1. 全局作用域中

- 非严格模式：`this`指向全局对象（浏览器中为`window`）。

  ```javascript
  console.log(this === window); // 非严格模式：true
  ```

- 严格模式：`this`指向`undefined`。

  ```javascript
  'use strict';
  console.log(this); // undefined
  ```

### 2. 函数独立调用时

- 非严格模式：函数中的`this`指向全局对象（`window`）。

  ```javascript
  function foo() {
    console.log(this === window); // 非严格模式：true
  }
  foo();
  ```

- 严格模式

  ：函数中的`this`指向`undefined`。

  ```javascript
  'use strict';
  function foo() {
    console.log(this); // undefined
  }
  foo();
  ```

### 3. 对象方法调用时

- 两种模式一致：`this`指向调用方法的对象。

  ```javascript
  const obj = {
    name: 'test',
    func: function() {
      console.log(this.name);
    }
  };
  obj.func(); // 输出 'test'（严格/非严格模式一致）
  ```

### 4. 构造函数中

- 非严格模式：若忘记用`new`调用构造函数，`this`指向全局对象（可能意外修改全局变量）。

  ```javascript
  function Person(name) {
    this.name = name; // 非严格模式：this 指向 window
  }
  Person('Alice'); // 全局变量 name 被修改为 'Alice'
  ```

- 严格模式：若忘记用`new`调用，`this`为`undefined`，赋值时直接报错（避免全局污染）。

  ```javascript
  'use strict';
  function Person(name) {
    this.name = name; // 报错：Cannot set property 'name' of undefined
  }
  Person('Alice'); // 未用 new 调用，this 为 undefined
  ```

### 5. 事件处理函数中

- 两种模式一致：`this`指向触发事件的元素。

  ```javascript
  // HTML: <button id="btn">点击</button>
  const btn = document.getElementById('btn');
  btn.onclick = function() {
    console.log(this === btn); // 严格/非严格模式均为 true
  };
  ```

## 四、严格模式的其他限制

除 `this` 外，严格模式还有诸多语法限制：



| 限制类型              | 具体规则                                                     | 非严格模式行为                                               |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 变量声明              | 禁止使用未声明的变量（必须用 `var`/`let`/`const` 声明）      | 未声明变量会自动成为全局变量（导致全局污染）                 |
| 删除操作              | 禁止删除变量、对象属性或函数（`delete x` 报错）              | 允许删除（但可能意外删除关键资源）                           |
| 变量重名              | 禁止对象属性重名（`{a:1, a:2}`）和函数参数重名（`function(a,a){}`） | 允许重名，后定义的覆盖前定义的（易导致逻辑混乱）             |
| `eval` 与 `arguments` | 禁止将 `eval`/`arguments` 作为变量名或函数名；`eval` 不创建全局变量 | `eval` 可创建全局变量，`arguments` 可被重名覆盖（存在安全隐患） |
| `with` 语句           | 完全禁止使用 `with` 语句（`with` 会混淆作用域）              | 允许使用，但可能导致变量查找效率低且逻辑模糊                 |
| 八进制字面量          | 禁止使用前缀 `0` 表示八进制（如 `012`），需用 `0o` 前缀（ES6 标准） | 允许用 `0` 前缀表示八进制（非标准语法）                      |
| 只读属性赋值          | 对只读属性（`const` 变量、对象的只读属性）赋值会报错         | 赋值失败但不报错（导致隐性 bug）                             |

## 五、使用场景与建议

1. **推荐使用场景**：
   - 大型项目或团队协作：严格模式可提前暴露潜在错误，减少代码冲突。
   - 追求代码质量：强制规范编码习惯，避免 JavaScript 的 “怪异行为”。
   - 工具库 / 框架开发：确保代码在不同环境下的一致性。
2. **注意事项**：
   - 兼容性：支持 IE10+ 及现代浏览器，旧环境（如 IE9 及以下）会忽略严格模式指令。
   - 迁移成本：若旧项目迁移到严格模式，需检查 `this` 指向和语法限制，避免报错。

## 总结

严格模式通过 `'use strict';` 启用，核心作用是规范代码行为。其中，`this` 指向的变化是最关键的差异点（全局 / 独立函数中 `this` 为 `undefined`），需重点掌握。合理使用严格模式可显著提升代码质量，是现代 JavaScript 开发的最佳实践之一。