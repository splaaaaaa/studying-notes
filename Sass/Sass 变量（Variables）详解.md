# Sass 变量（Variables）详解

Sass（Syntactically Awesome Style Sheets）是 CSS 的预处理器，其变量功能是提升样式代码可维护性的核心特性之一。本文将详细介绍 Sass 变量的定义、使用、作用域及实战技巧。

## 一、Sass 变量的基本用法

### 1. 变量的定义

Sass 变量以 **`$` 符号**开头，语法格式为：

```scss
$变量名: 值;
```

**示例**：

```scss
// 定义变量
$primary-color: #3498db; // 颜色值
$base-font: 'Arial', sans-serif; // 字符串（字体）
$border-radius: 4px; // 数字（尺寸）
$max-width: 1200px; // 数字+单位
$is-dark-mode: false; // 布尔值
```

### 2. 变量的使用

定义后的变量可直接在样式中引用，通过 **`$变量名`** 调用：

```scss
// 使用变量
body {
  font-family: $base-font; // 引用字体变量
  color: $primary-color; // 引用颜色变量
  max-width: $max-width; // 引用尺寸变量
}

.button {
  background-color: $primary-color;
  border-radius: $border-radius; // 引用圆角变量
}
```

**编译为 CSS 后**（变量会被替换为实际值）：

```css
body {
  font-family: 'Arial', sans-serif;
  color: #3498db;
  max-width: 1200px;
}

.button {
  background-color: #3498db;
  border-radius: 4px;
}
```

### 3. 变量支持的数据类型

Sass 变量可存储多种数据类型，满足不同样式需求：

- 字符串（如 `'bold'`、`"12px"`）
- 数字（如 `12`、`3.14`、`2rem`）
- 颜色（如 `#ff0000`、`rgb(255, 0, 0)`、`red`）
- 布尔值（`true` / `false`）
- 列表（如 `10px 20px`、`(a, b, c)`，类似 CSS 中的复合属性值）
- null（空值，用于条件判断）

## 二、变量的作用域（Scope）

Sass 变量的作用域默认遵循 **“局部优先”** 原则，即变量的生效范围取决于定义位置。

### 1. 局部变量

在选择器、嵌套块或混入（mixin）中定义的变量，默认仅在当前块内有效（局部作用域）。

**示例**：

```scss
$global-color: blue; // 全局变量

.container {
  $local-color: red; // 局部变量（仅在 .container 内有效）
  color: $local-color; // 生效：red
  background: $global-color; // 生效：blue
}

p {
  color: $local-color; // 报错：局部变量未定义
}
```

### 2. 全局变量与 `!global` 关键词

若需在局部块内定义全局变量（覆盖外部同名变量），需使用 **`!global` 关键词**：

```scss
$color: green; // 初始全局变量

.box {
  $color: yellow !global; // 重新定义为全局变量
  color: $color; // 黄色
}

.text {
  color: $color; // 黄色（全局变量已被覆盖）
}
```

**编译为 CSS 后**：

```css
.box {
  color: yellow;
}

.text {
  color: yellow;
}
```

### 3. 全局变量的最佳实践

在大型项目中，建议将全局变量集中定义在单独文件（如 `_variables.scss`），通过 `@use` 或 `@import` 引入，实现统一管理。

**示例**：

```scss
// _variables.scss（下划线开头表示局部文件，不单独编译）
$primary: #2c3e50;
$secondary: #e74c3c;
$font-size: 16px;

// main.scss
@use 'variables'; // 引入全局变量

body {
  color: variables.$primary; // 使用命名空间访问
  font-size: variables.$font-size;
}
```

## 三、变量的高级特性

### 1. 变量的计算

Sass 变量支持数学运算（加减乘除），可动态生成样式值。

**示例**：

```scss
$base-width: 100px;
$gap: 20px;

.box {
  width: $base-width; // 100px
}

.container {
  width: $base-width + $gap * 2; // 100px + 40px = 140px
}
```

### 2. 变量的默认值（`!default`）

使用 `!default` 可为变量设置默认值，若变量已被定义则不覆盖，否则使用默认值。

**示例**：

```scss
$theme: 'light'; // 已定义变量

// 若 $theme 未定义，则使用 'dark'
$theme: 'dark' !default; 

body {
  background: if($theme == 'light', #fff, #333); // 白色（因 $theme 已定义为 'light'）
}
```

## 四、Sass 变量 vs CSS 变量

Sass 变量与 CSS 原生变量（`--variable`）的核心区别如下：

| 特性         | Sass 变量                   | CSS 变量                    |
| ------------ | --------------------------- | --------------------------- |
| 生效阶段     | 编译时（编译为 CSS 后消失） | 运行时（保留在 CSS 中）     |
| 作用域控制   | 通过 `!global` 关键词       | 通过 CSS 选择器自然继承     |
| 浏览器兼容性 | 无（需编译为 CSS）          | 现代浏览器支持（IE 不兼容） |
| 动态修改     | 不支持（编译后固定）        | 支持（可通过 JS 动态修改）  |

**适用场景**：

- 静态样式（如主题色、固定尺寸）：优先使用 Sass 变量，兼容性更好。
- 动态样式（如用户自定义主题、响应式实时调整）：使用 CSS 变量。

## 五、总结

- **核心价值**：Sass 变量通过 `$` 定义，可复用样式值，减少重复代码，便于全局修改。
- **作用域**：默认局部有效，`!global` 可提升为全局变量。
- **最佳实践**：全局变量集中管理，结合 `!default` 实现可扩展配置。
- **与 CSS 变量的区别**：Sass 变量是编译时特性，CSS 变量是运行时特性，按需选择。