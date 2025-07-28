# JavaScript for循环

## 一、循环的作用

循环可将代码块执行指定次数，在需要重复运行相同代码且每次值不同时（如输出数组元素）非常方便，能避免大量重复代码的书写。

## 二、JavaScript 支持的循环类型

- `for`：循环代码块一定的次数
- `for/in`：循环遍历对象的属性
- `while`：当指定的条件为 `true` 时循环指定的代码块
- `do/while`：同样当指定的条件为 `true` 时循环指定的代码块

## 三、for 循环

### 1. 基本语法

```javascript
for (语句1; 语句2; 语句3) {
    被执行的代码块
}
```

- **语句 1**：在循环（代码块）开始前执行，通常用于初始化循环中所用的变量（如 `var i = 0`）
- **语句 2**：定义运行循环（代码块）的条件，若返回 `true` 则循环继续，返回 `false` 则循环结束（如 `i < 5`）
- **语句 3**：在循环（代码块）已被执行之后执行，通常用于增加初始变量的值（如 `i++`）

### 2. 各语句的灵活性

- **语句 1**：
  - 可选，可初始化多个值（如 `for (var i = 0, len = cars.length; i < len; i++)`）
  - 可省略（若循环开始前已设置好变量，如 `var i = 2, len = cars.length; for (; i < len; i++)`）
- **语句 2**：
  - 可选，若省略，必须在循环内提供 `break`，否则循环无法停止可能导致浏览器崩溃
- **语句 3**：
  - 可选，增量方式多样（可为负数 `i--` 或更大值 `i = i + 15`）
  - 可省略（若循环内部有相应的变量操作代码，如 `var i = 0, len = cars.length; for (; i < len;) { ... i++; }`）

## 四、for/in 循环

用于循环遍历对象的属性，语法示例：

```javascript
var person = { fname: "Bill", lname: "Gates", age: 56 };
for (x in person) { // x 为属性名
    txt = txt + person[x];
} //输出：BillGates56
```

# JavaScript 循环之 while 与 do/while 循环详解

## 全文总结

本文重点介绍了 JavaScript 中的两种循环类型：`while` 循环和 `do/while` 循环。两种循环均用于在指定条件为 `true` 时重复执行代码块，但执行逻辑存在差异。文章通过语法说明、实例演示和注意事项，清晰阐述了两种循环的使用方式，并提及与 `for` 循环的功能相似性。

## 一、while 循环

### 1. 基本语法

```javascript
while (条件) {
    需要执行的代码块
}
```

### 2. 执行逻辑

- 先判断条件是否为`ture`
  - 若为 `true`，执行代码块，执行完毕后再次检查条件；
  - 若为 `false`，退出循环，不再执行代码块。

### 3. 关键注意事项

- 必须在循环内部 **修改条件中变量的值**（如 `i++`），否则可能导致循环永不结束（死循环），进而使浏览器崩溃。

- 示例：

  ```javascript
  var i = 0;
  while (i < 5) {
      x += "数字为 " + i + "\n";
      i++; // 若省略此句，将陷入死循环
  }
  ```

## 二、do/while 循环

### 1. 基本语法

```javascript
do {
    需要执行的代码块
} while (条件);
```

### 2. 执行逻辑

- 先执行一次代码块，再判断条件是否为`true`：
  - 若为 `true`，再次执行代码块，重复检查条件；
  - 若为 `false`，退出循环。
- 特点：**至少执行一次代码块**，无论条件初始是否为 `true`。

### 3. 关键注意事项

- 与 `while` 循环相同，需在循环内部修改条件变量的值，避免死循环。

- 示例：

  ```javascript
  var i = 0;
  do {
      x += "数字为 " + i + "\n";
      i++;
  } while (i < 5);
  ```

## 三、while 与 do/while 的核心区别

| 循环类型   | 执行顺序               | 最少执行次数                |
| ---------- | ---------------------- | --------------------------- |
| `while`    | 先判断条件，再执行代码 | 0 次（条件初始为 false 时） |
| `do/while` | 先执行代码，再判断条件 | 1 次                        |

## 四、与 for 循环的对比

- 功能相似性：while循环可实现与for循环相同的功能（如遍历数组）。

  - 示例（遍历数组）：

    ```javascript
    // for 循环
    for (var i = 0; cars[i]; i++) {
        document.write(cars[i] + "<br>");
    }
    
    // while 循环（等效功能）
    var i = 0;
    while (cars[i]) {
        document.write(cars[i] + "<br>");
        i++;
    }
    ```

- 适用场景：`while` 循环更适合 **循环次数不确定** 的场景（条件依赖动态变化），而 `for` 循环更适合 **循环次数已知** 的场景。

## 五、总结

- `while` 和 `do/while` 循环的核心是通过条件控制代码块的重复执行；
- 区分两者的执行顺序是使用的关键；
- 务必避免死循环（需在循环内更新条件变量）。

# JavaScript 中 break、continue 语句及标签的用法总结

## 一、break 语句

### 1. 功能与适用场景

- 可用于跳出 `switch` 语句（此前章节已涉及）。
- 可用于跳出循环，跳出后会继续执行循环之后的代码。

### 2. 实例

```javascript
// for 循环中使用 break
for (i = 0; i < 10; i++) {
    if (i == 3) {
        break; // 当 i=3 时跳出循环
    }
    x = x + "The number is " + i + "<br>";
}
// 简化写法（if 语句只有一行代码时可省略花括号）
for (i = 0; i < 10; i++) {
    if (i == 3) break;
    x = x + "The number is " + i + "<br>";
}
```

## 二、continue 语句

### 1. 功能与适用场景

- 用于循环中，作用是中断当前迭代，跳过本次迭代中 `continue` 之后的代码，直接进入下一次迭代。

### 2. 实例

```javascript
// for 循环中使用 continue
for (i = 0; i <= 10; i++) {
    if (i == 3) continue; // 当 i=3 时跳过本次迭代
    x = x + "The number is " + i + "<br>";
}

// while 循环中使用 continue（需注意更新循环变量，避免死循环）
while (i < 10) {
    if (i == 3) {
        i++; // 此处必须更新 i，否则会陷入死循环
        continue;
    }
    x = x + "该数字为 " + i + "<br>";
    i++;
}
```

## 三、JavaScript 标签

### 1. 定义

通过在语句前加冒号（`label:`）对 JavaScript 语句进行标记，用于配合 `break` 和 `continue` 实现更灵活的代码控制。

### 2. 与 break、continue 结合使用的规则

- `continue` 语句（无论是否带标签）**仅能用于循环**。
- 无标签的 `break` 语句**可用于循环或 switch 语句**。
- 带标签的 `break` 语句**可跳出任何代码块**（不限于循环或 switch）。

### 3. 实例

```javascript
// 带标签的 break 跳出代码块
cars = ["BMW", "Volvo", "Saab", "Ford"];
list: {
    document.write(cars[0] + "<br>");
    document.write(cars[1] + "<br>");
    document.write(cars[2] + "<br>");
    break list; // 跳出 list 标记的代码块，后续代码不执行
    document.write(cars[3] + "<br>");
}

// 多层循环中使用标签控制外层循环
outerloop: for (var i = 0; i < 10; i++) {
    innerloop: for (var j = 0; j < 10; j++) {
        if (j > 3) {
            break; // 跳出 innerloop 循环
        }
        if (i == 2) {
            break innerloop; // 跳出 innerloop 循环
        }
        if (i == 4) {
            break outerloop; // 跳出 outerloop 循环
        }
        document.write("i=" + i + " j=" + j + " ");
    }
}
```

## 四、总结

- `break` 用于**完全终止循环或跳出代码块 /switch**，`continue` 用于**跳过循环的当前迭代**。
- 标签扩展了两者的功能，尤其在多层循环中，能精准控制需要跳出或继续的循环层级。
- 使用时需注意语法规则（如 `continue` 仅用于循环），避免逻辑错误。

# JavaScript 中标签的作用

在 JavaScript 中，标签（`label`）是通过在语句前加冒号（`label:`）定义的标记，主要用于配合 `break` 和 `continue` 语句，实现对代码执行流程的精准控制，尤其在复杂代码结构（如多层循环、代码块）中作用显著。其核心作用如下：

## 1. 增强 `break` 语句的跳转能力

- **默认情况下**，`break` 只能跳出当前所在的循环或 `switch` 语句。

- 结合标签后，`break`可以跳出任何被标记的代码块（不限于循环或`switch`），实现跨代码块的跳转。

  示例：

  ```javascript
  list: { // 标记一个代码块
      document.write("步骤1<br>");
      document.write("步骤2<br>");
      break list; // 跳出整个 list 代码块
      document.write("步骤3<br>"); // 此句不会执行
  }
  ```

## 2. 增强 `continue` 语句的循环控制能力

- **默认情况下**，`continue` 只能跳过当前循环的一次迭代，作用于最内层循环。

- 结合标签后，continue可以指定跳过外层循环的当前迭代，直接进入外层循环的下一次迭代（仅适用于循环代码块）。

  示例：

  ```javascript
  outer: for (let i = 0; i < 3; i++) { // 外层循环标签
      inner: for (let j = 0; j < 3; j++) {
          if (j === 1) {
              continue outer; // 跳过外层循环的当前迭代，直接进入 i 的下一次循环
          }
          console.log(`i=${i}, j=${j}`);
      }
  }
  // 输出：i=0, j=0；i=1, j=0；i=2, j=0（跳过了 j=1 时的外层循环迭代）
  ```

## 3. 解决多层循环的控制难题

在多层嵌套循环中，默认的 `break` 和 `continue` 只能作用于最内层循环。通过标签，可明确指定要控制的循环层级，避免逻辑混乱。
示例：

```javascript
outerloop: for (let i = 0; i < 5; i++) { // 外层循环标签
    innerloop: for (let j = 0; j < 5; j++) { // 内层循环标签
        if (i === 3) break outerloop; // 直接跳出外层循环
        if (j === 2) break innerloop; // 跳出内层循环
        console.log(`i=${i}, j=${j}`);
    }
}
```

## 4. 语法规则限制

- 标签仅能与 `break` 或 `continue` 配合使用，单独使用无意义。
- `continue` 结合标签时，标签**必须标记循环语句**（不能是普通代码块），否则会报错。
- `break` 结合标签时，标签可标记任何代码块（循环、普通代码块等）。

## 总结

标签的核心价值是为 `break` 和 `continue` 提供 “目标定位” 功能，使其在复杂代码结构中突破默认作用范围，实现更灵活、精准的流程控制，尤其适合多层循环或需要跨代码块跳转的场景。