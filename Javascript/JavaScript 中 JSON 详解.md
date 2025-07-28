# JavaScript 中 JSON 详解

JSON（JavaScript Object Notation）是一种轻量级的数据交换格式，常用于服务端与网页之间的数据传输。它基于 JavaScript 语法，但作为独立的文本格式，可被多种编程语言解析和生成。

## 一、JSON 基础概念

### 1. 核心特性

- **轻量级**：格式简洁，传输效率高。
- **跨语言**：独立于编程语言，任何语言都能读取和生成。
- **易理解**：语法清晰，结构直观。
- **文本格式**：本质是字符串，便于存储和网络传输。

### 2. 与 JavaScript 的关系

- JSON 语法借鉴了 JavaScript 对象字面量的格式，但存在区别：
  - JSON 是**纯文本**，而 JavaScript 对象是内存中的数据结构。
  - JSON 中的键和字符串值必须用**双引号**包裹（单引号无效）。
  - JSON 不支持函数、注释、undefined 等 JavaScript 特性。

## 二、JSON 语法规则

1. **数据格式**：以 `键/值对` 形式存在，格式为 `{"键": "值"}`。

   - 键必须用双引号包裹（如 `"name"`）。
   - 值可以是字符串、数字、布尔值、null、对象或数组。

2. **分隔符**：多个键 / 值对之间用**逗号**分隔。

3. **对象**：用**大括号 `{}`** 包裹，可包含多个键 / 值对。

   ```json
   {"name": "Runoob", "url": "www.runoob.com", "founded": 2013}
   ```

4. **数组**：用**中括号 `[]`** 包裹，可包含多个对象或值（数组元素用逗号分隔）。

   ```json
   {
     "sites": [
       {"name": "Runoob", "url": "www.runoob.com"},
       {"name": "Google", "url": "www.google.com"},
       {"name": "Taobao", "url": "www.taobao.com"}
     ]
   }
   ```

   - 上述示例中，`sites` 是一个数组，包含 3 个对象，每个对象表示一个网站的信息。

## 三、JSON 与 JavaScript 对象的转换

JavaScript 提供了两个内置函数，用于 JSON 与 JavaScript 对象的相互转换。

### 1. `JSON.parse()`：JSON 字符串 → JavaScript 对象

将 JSON 格式的字符串解析为 JavaScript 对象，便于在代码中操作。

**示例**：

```javascript
// JSON 字符串
const jsonStr = '{ "sites": [' +
  '{"name": "Runoob", "url": "www.runoob.com"},' +
  '{"name": "Google", "url": "www.google.com"} ]}';

// 转换为 JavaScript 对象
const obj = JSON.parse(jsonStr);

// 使用对象数据
console.log(obj.sites[0].name); // 输出："Runoob"
document.getElementById("demo").innerHTML = obj.sites[1].url; // 显示："www.google.com"
```

### 2. `JSON.stringify()`：JavaScript 对象 → JSON 字符串

将 JavaScript 对象转换为 JSON 字符串，便于存储或传输（如发送到服务器）。

**示例**：

```javascript
// JavaScript 对象
const obj = {
  name: "Runoob",
  url: "www.runoob.com",
  sites: ["Google", "Taobao"]
};

// 转换为 JSON 字符串
const jsonStr = JSON.stringify(obj);
console.log(jsonStr); 
// 输出：'{"name":"Runoob","url":"www.runoob.com","sites":["Google","Taobao"]}'
```

## 三、JSON 数据结构示例

### 1. 简单对象

```json
{
  "name": "张三",
  "age": 25,
  "isStudent": false,
  "hobbies": ["读书", "运动"]
}
```

### 2. 嵌套对象与数组

```json
{
  "school": "北京大学",
  "students": [
    {
      "id": 1,
      "name": "李四",
      "scores": {"math": 90, "english": 85}
    },
    {
      "id": 2,
      "name": "王五",
      "scores": {"math": 88, "english": 92}
    }
  ]
}
```

## 四、常见用途

1. **数据传输**：服务端通过 JSON 格式向客户端（网页）发送数据。
2. **数据存储**：作为配置文件或本地存储（如 `localStorage`）的格式。
3. **API 交互**：大多数 RESTful API 采用 JSON 作为请求和响应的数据格式。

## 五、注意事项

1. 语法严格性：
   - 键和字符串值必须用双引号（单引号会导致解析错误）。
   - 末尾不能有多余的逗号（如 `{"a": 1,}` 无效）。
2. **不支持的类型**：JSON 不支持函数、正则表达式、undefined 等，转换时会被忽略或报错。
3. **解析错误**：若 JSON 格式不正确，`JSON.parse()` 会抛出异常，建议使用 `try/catch` 捕获。

## 总结

JSON 是一种高效、通用的数据交换格式，其语法简洁且跨语言兼容。在 JavaScript 中，通过 `JSON.parse()` 和 `JSON.stringify()` 可实现 JSON 与 JavaScript 对象的无缝转换，是前后端数据交互的核心工具之一。掌握 JSON 语法和转换方法，对处理 API 数据、本地存储等场景至关重要。