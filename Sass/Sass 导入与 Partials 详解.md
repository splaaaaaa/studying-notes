# Sass 导入与 Partials 详解

在 Sass 中，`@import` 指令和 **Partials（局部文件）** 是实现样式模块化管理的核心机制，能够将代码拆分为多个文件，提升可维护性。以下是详细说明：

## 一、Sass Partials：不单独编译的局部文件

### 1. 什么是 Partials？

Partials 是 Sass 中用于拆分代码的 “局部文件”，其文件名以 **下划线 `_` 开头**（如 `_variables.scss`）。
**核心特性**：

- 不会被单独编译为 CSS 文件（仅作为 “模块” 被其他文件导入）。
- 用于存储可复用的代码（如变量、混合器、工具类样式等）。

**示例**：
创建 `_colors.scss`（局部文件，定义颜色变量）：

```scss
// _colors.scss（不会生成单独的 colors.css）
$primary: #3498db;
$secondary: #e74c3c;
```

### 2. 为什么需要 Partials？

- **避免冗余 CSS**：局部文件仅用于代码复用，不生成独立输出，减少最终 CSS 文件数量。
- **模块化管理**：按功能拆分文件（如 `_variables.scss` 存变量、`_mixins.scss` 存混合器），结构更清晰。

## 二、`@import` 指令：导入局部文件

### 1. 基本用法

Sass 的 `@import` 指令用于将 Partials 或其他 Sass/CSS 文件导入到主文件中，语法：

```scss
@import "文件名"; // 无需写下划线和 .scss 后缀
```

**示例**：
主文件 `style.scss` 导入 `_colors.scss` 和 `_variables.scss`：

```scss
// style.scss（主文件，会被编译为 style.css）
@import "colors"; // 导入 _colors.scss（省略下划线和后缀）
@import "variables"; // 导入 _variables.scss

body {
  color: $primary; // 使用 _colors.scss 中的变量
  font-size: $base-font-size; // 使用 _variables.scss 中的变量
}
```

**编译后**：`style.css` 会包含 `_colors.scss`、`_variables.scss` 和主文件的所有样式，无额外 HTTP 请求。

### 2. 与 CSS `@import` 的区别

| 特性          | Sass `@import`                            | CSS `@import`                      |
| ------------- | ----------------------------------------- | ---------------------------------- |
| 处理时机      | 编译时（将内容合并到主 CSS 文件）         | 运行时（浏览器加载时额外请求文件） |
| HTTP 请求数量 | 无额外请求（合并为一个文件）              | 每次导入产生一个请求（影响性能）   |
| 支持文件类型  | 支持 `.scss`、`.sass`、`.css` 文件        | 仅支持 `.css` 文件                 |
| 变量复用      | 导入后可直接使用被导入文件的变量 / 混合器 | 不支持（无法共享变量）             |

### 3. 导入规则

- **省略文件名前缀和后缀**：导入 `_colors.scss` 时，直接写 `@import "colors";`（无需 `_` 和 `.scss`）。

- 支持嵌套导入

  ：可在选择器内导入文件，使导入的样式仅作用于该选择器范围：

  ```scss
  .sidebar {
    @import "sidebar-styles"; // 导入的样式仅作用于 .sidebar 内部
  }
  ```

- **导入 CSS 文件**：若导入 `.css` 文件，行为与 CSS `@import` 一致（会生成额外请求），建议优先用 Sass 文件。

## 三、实战：模块化项目结构

一个典型的 Sass 项目可按功能拆分 Partials，通过 `@import` 合并到主文件：

```plaintext
sass/
├── _variables.scss   // 变量（颜色、字体、尺寸等）
├── _mixins.scss      // 混合器（可复用的样式片段）
├── _reset.scss       // 重置样式（如清除默认边距）
├── _header.scss      // 头部样式
├── _footer.scss      // 底部样式
└── style.scss        // 主文件（导入所有局部文件）
```

主文件 `style.scss` 导入示例：

```scss
// 导入基础模块
@import "variables";
@import "mixins";
@import "reset";

// 导入页面组件样式
@import "header";
@import "footer";

// 主样式
body {
  margin: 0;
  font-family: $base-font; // 使用 _variables.scss 中的变量
}
```

**编译后**：所有局部文件的内容会被合并到 `style.css` 中，输出一个完整的 CSS 文件。

## 四、注意事项

1. **避免同名冲突**：若同一目录下存在 `_file.scss` 和 `file.scss`，导入 `@import "file";` 会优先选择 `file.scss`，忽略 `_file.scss`（建议文件名唯一）。
2. **控制导入层级**：过度嵌套导入可能导致样式优先级混乱，建议保持扁平的导入结构。
3. **现代替代方案**：Sass 3.0+ 推荐使用 `@use` 和 `@forward` 替代 `@import`（功能更强大，支持命名空间），但 `@import` 仍广泛兼容。

## 五、总结

- **Partials**：以 `_` 开头的局部文件，不单独编译，用于拆分模块化代码。
- **`@import`**：在主文件中导入 Partials，合并代码为一个 CSS 文件，避免额外 HTTP 请求。
- **核心价值**：实现样式模块化管理，减少重复代码，提升大型项目的可维护性。