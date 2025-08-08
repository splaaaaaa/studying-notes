# Sass（Syntactically Awesome Stylesheets）函数详解

Sass 是一种 CSS 预处理器，提供了强大的函数功能，可实现颜色处理、数值计算、字符串操作等高级样式逻辑。以下是 Sass 函数的详细总结：

## 一、Sass 内置函数分类及示例

### 1. 颜色函数（Color Functions）

用于调整颜色值（加深、变浅、透明化等）。

| 函数名称                         | 用途         | 示例                           |
| -------------------------------- | ------------ | ------------------------------ |
| `lighten($color, $amount)`       | 变浅颜色     | `lighten(#333, 20%) → #666666` |
| `darken($color, $amount)`        | 变深颜色     | `darken(#ccc, 10%) → #b3b3b3`  |
| `adjust-hue($color, $degrees)`   | 改变色相     | `adjust-hue(#6b717f, 20deg)`   |
| `rgba($color, $alpha)`           | 设置透明度   | `rgba(#000, 0.5)`              |
| `mix($color1, $color2, $weight)` | 混合两种颜色 | `mix(red, blue, 50%)`          |

### 2. 字符串函数（String Functions）

用于操作字符串。

| 函数名称                 | 用途     | 示例                         |
| ------------------------ | -------- | ---------------------------- |
| `quote($string)`         | 添加引号 | `quote(hello) → "hello"`     |
| `unquote($string)`       | 移除引号 | `unquote("hello") → hello`   |
| `str-length($string)`    | 获取长度 | `str-length("hello") → 5`    |
| `to-upper-case($string)` | 转为大写 | `to-upper-case("abc") → ABC` |
| `to-lower-case($string)` | 转为小写 | `to-lower-case("ABC") → abc` |

### 3. 数值函数（Number Functions）

用于处理数值（单位、舍入、比较等）。

| 函数名称             | 用途       | 示例                    |
| -------------------- | ---------- | ----------------------- |
| `percentage($value)` | 转为百分比 | `percentage(0.5) → 50%` |
| `round($value)`      | 四舍五入   | `round(2.5) → 3`        |
| `ceil($value)`       | 向上取整   | `ceil(4.2) → 5`         |
| `floor($value)`      | 向下取整   | `floor(4.8) → 4`        |
| `abs($value)`        | 取绝对值   | `abs(-10) → 10`         |

### 4. 列表函数（List Functions）

用于操作 Sass 中的列表类型（类似数组）。

| 函数名称                           | 用途         | 示例                            |
| ---------------------------------- | ------------ | ------------------------------- |
| `length($list)`                    | 获取列表长度 | `length(1px solid red) → 3`     |
| `nth($list, $n)`                   | 获取第 n 项  | `nth(10px 20px 30px, 2) → 20px` |
| `join($list1, $list2, $separator)` | 合并两个列表 | `join(10px, 20px) → 10px 20px`  |

### 5. 映射函数（Map Functions）

Sass 中的 map 类似 JavaScript 的对象（键值对结构）。

| 函数名称                  | 用途         | 示例                         |
| ------------------------- | ------------ | ---------------------------- |
| `map-get($map, $key)`     | 取值         | `map-get($colors, primary)`  |
| `map-keys($map)`          | 获取所有键   | `map-keys($colors)`          |
| `map-values($map)`        | 获取所有值   | `map-values($colors)`        |
| `map-has-key($map, $key)` | 是否包含某键 | `map-has-key($map, warning)` |

### 6. 其他常用函数

| 函数名称                              | 用途           | 示例                        |
| ------------------------------------- | -------------- | --------------------------- |
| `if($condition, $if-true, $if-false)` | 条件判断       | `if(true, red, blue) → red` |
| `unit($number)`                       | 返回单位       | `unit(10px) → px`           |
| `unitless($number)`                   | 判断是否无单位 | `unitless(10px) → false`    |

## 二、自定义函数（@function）

Sass 允许使用 `@function` 自定义函数，类似 JavaScript 的函数。

**示例**：

```scss
@function double($n) {
  @return $n * 2;
}

.container {
  width: double(10px); // 输出: width: 20px;
}
```

**注意**：需使用 `@return` 返回结果。

## 三、使用场景举例

### 1. 颜色主题管理

```scss
$primary-color: #3498db;
.btn {
  background-color: lighten($primary-color, 10%);
}
```

### 2. 响应式计算

```scss
@function rem($px) {
  @return $px / 16 * 1rem;
}

body {
  font-size: rem(14px); // 输出: font-size: 0.875rem;
}
```

## 四、补充建议

- 建议多使用函数减少重复计算。
- 与 mixin 配合使用可增强组件样式的灵活性。
- 常用于主题系统、字体尺寸缩放、单位转换等领域。