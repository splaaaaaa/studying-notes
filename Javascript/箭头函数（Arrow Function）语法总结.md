# 箭头函数（Arrow Function）语法总结

## 一、箭头函数基础定义

箭头函数是 ES6 新增的函数简写形式，语法更简洁，适用于函数式编程场景，基本结构：

```javascript
const 函数名 = (参数) => { 函数体 };
```

## 二、常见语法写法

### 1. 无参数

```javascript
const sayHello = () => {
  console.log("Hello!");
};
```

### 2. 单个参数（可省略括号）

```javascript
const greet = name => {  // 省略参数括号
  console.log(`Hello, ${name}`);
};
```

### 3. 多个参数（必须带括号）

```javascript
const add = (a, b) => {  // 多参数需括号
  return a + b;
};
```

### 4. 简写返回值（省略 `{}` 和 `return`）

```javascript
const multiply = (a, b) => a * b;  // 单行表达式直接返回
```

### 5. 返回对象（需用 `()` 包裹）

```javascript
const getUser = () => ({ name: "Alice", age: 25 });  // 避免被解析为函数体
```

### 6. 结合数组方法（`map`/`filter` 等）

```javascript
const numbers = [1, 2, 3, 4];
const squared = numbers.map(num => num * num);  // 简洁处理数组
```

## 三、与普通函数的核心区别

| 特性             | 箭头函数                     | 普通函数                    |
| ---------------- | ---------------------------- | --------------------------- |
| `this` 绑定      | 固定为定义时的上下文         | 动态绑定（取决于调用者）    |
| `arguments` 对象 | 无（可用 `...args` 替代）    | 有（类数组参数集合）        |
| 构造函数能力     | 不可用 `new` 实例化          | 可用 `new` 实例化           |
| 简写语法         | 支持省略 `function`/`return` | 无简写，需完整声明          |
| 适合作为对象方法 | 不推荐（`this` 指向异常）    | 推荐（`this` 指向当前对象） |

## 四、典型应用场景

### ✅ 推荐使用场景

- 数组方法回调（`map`/`filter`/`reduce`）
- 定时器回调（`setTimeout`/`setInterval`）
- 简单逻辑函数（计算、数据转换）
- React 函数组件（无生命周期逻辑）

### ❌ 不推荐使用场景

- 需要动态 `this` 的对象方法
- 构造函数（无法用 `new`）
- 依赖 `arguments` 的函数
- 复杂逻辑函数（可读性下降）

## 五、关键示例对比

### 1. `this` 指向差异

```javascript
// 普通函数：this 指向调用者
const obj = {
  name: "Tom",
  sayHi: function () {
    console.log(this.name); // 输出 "Tom"
  }
};

// 箭头函数：this 指向定义时的上下文（如 window）
const obj2 = {
  name: "Tom",
  sayHi: () => {
    console.log(this.name); // 输出 undefined
  }
};
```

### 2. 数组处理示例

```javascript
// 过滤偶数
const numbers = [1, 2, 3, 4, 5];
const evenNumbers = numbers.filter(n => n % 2 === 0); // 输出 [2, 4]
```

## 六、总结口诀

```kotlin
箭头函数更简洁，this不变要记得；
return 对象别忘括，构造函数它不能干；
回调映射最擅长，复杂逻辑用普通函数。
```



箭头函数的核心优势是简化代码和固定 `this`，但需根据场景合理选择，避免因特性限制导致逻辑错误。