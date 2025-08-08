# Sass 中的 `@extend` 与继承

Sass 的 `@extend` 指令是实现样式复用的核心功能之一，用于让一个选择器**继承另一个选择器的全部样式**，特别适合处理 “大部分样式相同、仅少量细节不同” 的场景。

## 一、`@extend` 的基本用法

### 1. 语法与作用

`@extend` 指令的语法为：

```scss
选择器A {
  @extend 选择器B; // 选择器A 继承 选择器B 的所有样式
  // 选择器A 的特有样式
}
```

**作用**：让 `选择器A` 拥有 `选择器B` 的全部样式，同时可添加自身特有的样式，避免重复代码。

### 2. 实例：按钮样式继承

**场景**：创建基础按钮样式，再基于它扩展出 “报告按钮” 和 “提交按钮”（仅颜色不同）。

**Sass 代码**：

```scss
// 基础按钮样式（被继承的父选择器）
.button-basic {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

// 报告按钮（继承基础样式，添加红色背景）
.button-report {
  @extend .button-basic; // 继承 .button-basic 的所有样式
  background-color: red;
}

// 提交按钮（继承基础样式，添加绿色背景和白色文字）
.button-submit {
  @extend .button-basic; // 继承 .button-basic 的所有样式
  background-color: green;
  color: white;
}
```

**编译后的 CSS 代码**：

```css
/* 基础样式与继承样式合并，避免重复 */
.button-basic, .button-report, .button-submit {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

/* 各选择器的特有样式 */
.button-report {
  background-color: red;
}

.button-submit {
  background-color: green;
  color: white;
}
```

## 二、`@extend` 的核心优势

### 1. 减少代码冗余

继承机制将重复样式（如按钮的边框、内边距、字体大小）合并到共用选择器中，避免在多个选择器中重复书写相同代码。

### 2. 简化 HTML 代码

使用继承后，HTML 元素只需引用最终的继承类，无需同时指定基础类：

```html
<!-- 无需写 class="button-basic button-report" -->
<button class="button-report">报告</button>
<button class="button-submit">提交</button>
```

### 3. 增强样式关联性

通过继承明确体现样式间的 “父子关系”（如 `.button-report` 是 `.button-basic` 的变体），使代码结构更清晰，便于维护。

## 三、`@extend` 的适用场景

- **相似组件的样式复用**：如按钮（基础按钮、成功按钮、警告按钮）、卡片（基础卡片、推荐卡片）等，核心样式一致，仅细节（颜色、边框）不同。
- **主题样式扩展**：基于基础主题样式，扩展出深色主题、浅色主题（仅修改背景色、文字色等）。
- **状态样式管理**：如链接的默认状态、 hover 状态、 active 状态，继承基础链接样式后仅修改状态相关属性。

## 四、注意事项

1. **避免过度继承**：多层级继承（如 A 继承 B，B 继承 C）会导致编译后的 CSS 选择器冗长（如 `.A, .B, .C { ... }`），影响可读性。
2. **谨慎继承通用选择器**：避免继承 `div`、`p` 等通用选择器，可能导致非预期样式被应用。
3. 与混合器（`@mixin`）的区别：
   - `@extend` 适合 “样式完全共享，仅少量差异” 的场景（生成合并选择器）。
   - `@mixin` 适合需要传递参数的动态样式（如不同圆角大小的按钮）。

## 五、总结

| 特性       | 说明                           | 示例                                        |
| ---------- | ------------------------------ | ------------------------------------------- |
| 语法       | `@extend 父选择器;`            | `.button-report { @extend .button-basic; }` |
| 编译后效果 | 父选择器与子选择器共享样式代码 | `.button-basic, .button-report { ... }`     |
| 核心价值   | 减少重复代码，增强样式关联性   | 按钮、卡片等相似组件的样式复用              |

`@extend` 是 Sass 中实现样式继承的高效工具，通过合理使用可显著提升代码复用率和可维护性，尤其适合构建风格统一的组件库或主题系统。