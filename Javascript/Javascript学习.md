1. 电脑打开豆包，打开会议纪要，内容复制，生成md-->验证学习的内容对不对，或者让豆包补充
2. es6：let、const、箭头函数、promise、import导入导出、解构赋值、扩展运算符、模版字符串
   let const var区别
   箭头函数和普通函数的区别（改变this有哪些方式、几种方式有什么区别）
   promise和ajax的区别及async和await的区别
3. JS：改变原数组的七个方法、Js的数据类型、闭包、原型、原型链、作用域、作用域链，js事件循环机制、重绘与回流【两种数据类型（基础数据类型、引用数据类型）】
4. 学习方式：B站 + AI（豆包） + 掘金 =>md =>我验证
5. 笔记上传到GitHub（浏览两个git的视频）（两个的区别）
   1. git:https://www.bilibili.com/video/BV1Gv421i7wW/?spm_id_from=333.337.search-card.all.click
   2. git:https://www.bilibili.com/video/BV1HM411377j/?spm_id_from=333.337.search-card.all.click
   3. git rebase和git merge的区别
6. react

# Javascript学习

# 定义

1. Javascript是脚本语言
2. Javascript是一种轻量级编程语言
3. Javascript是可插入Html页面的编程代码
4. Javascript插入Html页面后，可由所有先帝啊浏览器执行

# 用法

## <script>标签

如果在Html页面中插入Javascript，请使用<script>标签

<script>和</script>会告诉Javascript在何处开始和结束

<script>和</script>之间的代码行包含了Javascript

## <body>中的Javascript

Javascript会在页面加载时向Html的<body>写文本：

```html
<!DOCTYPE html>
<html>
  <body>
    .
    .
    <script>
      document.write("<h1>这是一个标题</h1>");
      document.write("<p>这是一个段落</p>");
    </script>
    .
    .
  </body>
</html>
```

## Javascript函数和事件

上面例子中的Javascript语句会在页面加载时执行。

通常，我们需要在某个事件发生时执行代码，比如当用户点击按钮时。

如果我们吧Javascript代码放入函数中，就可以在事件发生时调用该函数。

## 在<head>或者<body>的Javascript

1. 在HTML文档中放入不限数量的脚本
2. 脚本可位于HTML的<body>或<head>部分中，或同时存在于两个部分中
3. 通常做法时**把函数放入<head>部分中**，或者**放在页面底部**。这样就可以把它们安置到同一处位置，不会干扰页面的内容

## 外部的Javascript

也可以把脚本保存到外部文件中。外部文件通常包含被多个网页使用的代码。

外部Javascript文件的文件扩展名是.js。

如需使用外部文件，请在<script>标签的“src”属性中设置该.js文件：

```html
<!DOCTYPE html>
<html>
<body>
<script src="myScript.js"></script>
</body>
</html>
```

# Javascript输出

Javascript没有任何打印或者输出的函数

## Javascript显示数据

Javascript可以通过不同的方式来输出数据

1. 使用window.alert()弹出警告框
2. 使用document.write()方法将内容写道HTML文档中
3. 使用innerHTML写入HTML元素
4. 使用console.log()写入浏览器的控制台

## 使用window.alert()

```html
<!DOCTYPE html>
<html>
<body>
<h1>我的第一个页面</h1><p>我的第一个段落。</p>
	
<script>window.alert(5 + 6);
</script>

</body>
</html>
```

## 操作HTML元素

如从JavaScript访问某个HTML元素，可以使用document.getElementByld(id)方法

请使用“id”属性来标识HTML元素，并innerHTML来获取或插入元素内容：

```html
<!DOCTYPE html><html>
<body>

<h1>我的第一个 Web 页面</h1>

	<p id="demo">我的第一个段落</p>

<script>
	document.getElementById("demo").innerHTML = "段落已修改。";
</script>

</body>
</html>
```

`document.getElementById("demo").innerHTML = "段落已修改。";`的意思是 只要网页中有一个元素，它的 `id="demo"`，那么执行这段 JavaScript 代码时，这个元素的**内容**就会被改成 `段落已修改。`

## 写到控制台

```html
<!DOCTYPE html>
<html>
<body>
<h1>我的第一个 Web 页面</h1>
<script>
a = 5;
b = 6;
c = a + b;
console.log(c);
</script>

</body>
</html>
```

按F12来启用调试模式，在调试窗口中点击“Console”菜单

# 语法

## 字面量

1. 在编程语言中，一般固定值称为字面量，如3.14
2. **数字（Number）字面量**可以是整数或者是小数，或者是科学计数(e)
3. **字符串（String）字面量**可以使用单引号或双引号
4. **表达式字面量**用于计算
5. **数组（Array）字面量**定义一个数组
6. **对象（Object）字面量**定义一个对象
7. **函数（Function）字面量**定义一个函数

## 变量

### 声明方式

1. Javascript使用关键字var来定义变量，使用等号来为变量赋值；`var`为ES5引入变量的声明方式，具有**函数作用域。**

```javascript
var x, length
x=5
length=6
```

变量是一个**名称**。字面量是一个**值**。

1. let：ES6引入的变量声明方式，具有块级作用域。
2. const：ES6引入的常量声明方式，具有块级作用域，且值不可变。

### 注意事项

1. 变量必须以**字母**开头
2. 变量也能以$和_符号开头，但不建议
3. 变量名称对大小写敏感

### 声明（创建）Javascript变量

1. 使用var关键词来声明变量，变量声明之后，该变量是空的（它没有值）；如果须向变量赋值，使用等号。

```javascript
var carname;
carname="Volvo";
```

1. 可以在声明变量时对其赋值

```javascript
var carname="Volvo";
```

**var声明特点**

1. 1. 变量可以重复声明（覆盖原变量）
   2. 变量未赋值时，默认值为undefined。
   3. var声明的变量会提升（Hoisting），但不会初始化。

1. 可以在一条语句中声明很多变量。该语句以var开头，并使用逗号分隔变量即可；也可横跨多行；多个变量**不可以**同时赋同一个值。

```javascript
//第一个
var lastname="Doe", age=30, job="carpenter";
//第二个
var lastname="Doe",
age=30,
job="carpenter";
//第三个
var x, y, z = 1;
//x, y 为 undefined， z 为 1。
```

### 重新声明Javascript变量

1. 如果重新声明Javascript变量，该变量的值不会丢失；
2. 在以下两条语句执行后，变量spl的值依然是“shepeilin”

```javascript
var
  spl="shepeilin";
var spl;
```

### 使用let和const（ES6）

1. let是ES6引入的新变量声明方式

1. 1. 语法：`let variableName = value;`

```javascript
let city = "北京";
let age = 30;
console.log(city, age); // 输出: 北京 30
```

1. const：用于定义常量，即一旦赋值后，变量的值不能再被修改。

1. 1. 语法：`const variableName = value;`

```javascript
const z = 10;
// z = 20; // 报错，常量不可重新赋值
if (true) {
    const z = 20; // 不同的常量
    console.log(z); // 输出 20  只在代码块中有效
}
console.log(z); // 输出 10
```

## Javascript操作符

1. Javascript使用**算术运算符**来计算值；使用**赋值运算符**给变量赋值。

| 类型                   | 实例      | 描述                 |
| ---------------------- | --------- | -------------------- |
| 赋值，算数和位运算符   | = + - * / | 在JS运算符中描述     |
| 条件，比较及逻辑运算符 | == != < > | 在JS比较运算符中描述 |

## 语句

在HTML中，Javascript语句用于向浏览器发出命令

1. 语句是用**分号**分隔

```javascript
x = 5 + 6;
y = x * 10;
```

1. 浏览器按照编写顺序依次执行每条语句
2. Javascript可以分批地组合起来；**代码块**以左花括号开始，以右花括号结束；代码块地作用是**一并地执行语句序列**。
3. Javascript语句通常以一个**语句标识符**为开始，并执行该语句。

1. 1. 语句标识符是保留关键字不能作为变量名使用

| 语句         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| break        | 用于跳出循环。                                               |
| catch        | 语句块，在 **try 语句块**执行出错时**执行 catch 语句块**。   |
| continue     | **跳过**循环中的一个迭代。                                   |
| do ... while | 执行一个语句块，在条件语句为 **true** 时继续执行该语句块。   |
| for          | 在条件语句为 true 时，可以将代码块执行**指定的次数**。       |
| for ... in   | 用于遍历数组或者对象的属性（对数组或者对象的属性进行循环操作）。 |
| function     | 定义一个函数                                                 |
| if ... else  | 用于基于不同的条件来执行不同的动作。                         |
| return       | 返回结果，并退出函数                                         |
| switch       | 用于基于不同的条件来执行不同的动作。                         |
| throw        | 抛出（生成）错误 。                                          |
| try          | 实现错误处理，与 catch 一同使用。                            |
| var          | 声明一个变量。                                               |
| while        | 当条件语句为 true 时，执行语句块。                           |

1. Javascript会忽略多余地空格。可以向脚本添加空格，来提高其可读性。
2. 可以在文本字符串中使用反斜杠代码进行换行

```javascript
document.write("你好 \
世界!");
//不能像以下例子执行
document.write \ 
("你好世界!");
```

## 关键字

Javascript关键字用于标识要执行的操作

`var`关键字告诉浏览器创建一个新的变量：

```javascript
var x=5+6;
```

## 注释

1. 单行注释：双斜杠//后的内容会被浏览器忽略
2. 多行注释：以`/*`开始，以`*/`结尾。

## 数据类型

1. **值类型(基本类型)**：字符串（String）、数字(Number)、布尔(Boolean)、空（Null）、未定义（Undefined）、Symbol。
2. **引用数据类型（对象类型）**：对象(Object)、数组(Array)、函数(Function)，还有两个特殊的对象：正则（RegExp）和日期（Date）。
3. **注意**：Symbol是ES6引入了一种新的原始数据类型，表示独一无二的值。

### Javascript拥有动态类型

这意味着相同的变量可用作不同的类型，变量的数据类型可以使用`typeof`操作符来查看。

```javascript
var x;               // x 为 undefined
var x = 5;           // 现在 x 为数字
var x = "John";      // 现在 x 为字符串

typeof "John"                // 返回 string
typeof 3.14                  // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
```

**注意：typeof [1,2,3,4] 返回 "object"**，这是 JavaScript 早期设计的一个"缺陷"，数组本质上是特殊类型的对象。

正确检测数组的方法：

```javascript
Array.isArray([1,2,3]); // true
[1,2,3] instanceof Array; // true
```

### Javascript字符串

1. 字符串是存储字符（比如“Bill Gates”）的变量
2. 字符串可以是引号中的任意文本。可以使用单引号或双引号

```javascript
var carname="Volvo XC60";
var carname='Volvo XC60';
```

1. 在字符串中可以使用引号，只要**不匹配包围字符串的引号**即可。
2. 字符串可以使用索引位置来访问字符串中的每个字符，索引从0开始
3. 字符串长度使用内置属性length来计算字符串长度
4. 如果需要用与周围字符串的引号匹配的引号，那么可以使用**转义字符**来转义，避免Javascript无法解析。
5. 

### Javascript数字

1. Javascript只有一种数字类型。数字可以带小数点，也可以不带：

```javascript
var x1=34.00;      //使用小数点来写
var x2=34;         //不使用小数点来写
```

1. 极大或极小的数字可以通过科学（指数）计数法来书写：

```javascript
var y=123e5;      // 12300000
var z=123e-5;     // 0.00123
```

### Javascript布尔

布尔只能由两个值：true或false

### Javascript数组

```javascript
//第一种方式
var cars=new Array();
cars[0]="Saab";
cars[1]="Volvo";
cars[2]="BMW";
//第二种方式
var cars=new Array("Saab","Volvo","BMW");
```

### 转义字符

| 代码 | 输出        |
| ---- | ----------- |
| \'   | 单引号      |
| \"   | 双引号      |
| \\   | 反斜杠      |
| \n   | 换行        |
| \r   | 回车        |
| \t   | tab(制表符) |
| \b   | 退格符      |
| \f   | 换页符      |

### Javascript对象

1. 对象由**花括号**分隔。在括号内部，对象的属性以名称和值对的形式（name:value)来定义。属性由逗号分隔

```javascript
var person={firstname:"John", lastname:"Doe", id:5566};

//等同于

var person={
firstname : "John",
lastname  : "Doe",
id        :  5566
};
```

1. 对象属性由两种寻址方式

```javascript
name=person.lastname;
name=person["lastname"];
```

### 声明变量类型

声明新变量时，可以使用关键词“new”来声明其类型：

```javascript
var carname=new String;
var x=      new Number;
var y=      new Boolean;
var cars=   new Array;
var person= new Object;
```

**注意**：Javascript变量均为对象。当声明一个变量时，就创建了一个新的对象。

## Javascript函数

1. 函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块。

### 语法

1. 函数就是包裹在花括号中的代码块，前面使用了关键词function

```javascript
function functionname()
{
    // 执行代码
}
```

1. 当调用函数时，会执行函数内的代码
2. 可以在某事件发生时直接调用函数（比如当用户点击按钮时），并且可由Javascript在任何位置进行调用

注意：Javascript对大小写敏感。关键词**function必须是小写的**，并且必须以**与函数名称相同**的大小写来调用函数。

### 调用带参数的函数

1. 调用函数时，可以向其传递值，这些值被称为参数
2. 参数可以在函数中使用，可以发送任意多的参数，由逗号分隔

```javascript
myFunction(argument1,argument2)

//声明函数时，需要把参数作为变量来声明
function myFunction(var1,var2)
{
代码
}

//变量和参数必须以一致的顺序出现。第一个变量就是第一个被传递的参数的给定的值，以此类推。
```

### 带有返回值的函数

1. 使用return语句可以将值返回调用它的地方，在使用return语句时，函数会停止运行，并返回特定的值。
2. **注意**：整个Javascript并不会停止执行，仅仅是函数。Javascript将继续执行代码，从调用函数的地方。

### 变量

#### 局部Javascript变量

1. 在Javascript函数内部声明的变量（使用var）是局部变量，所以只能在函数内部访问它。（该变量的作用域是局部的）
2. 在不同函数中可以使用名称相同的局部变量，因为只有声明过该变量的函数才能识别出该变量。
3. 只要函数运行完毕，本地变量就会删除。

#### 全局Javascript变量

在函数外声明的变量是全局变量，网页上的所有脚本和函数都能访问它

####  Javascript变量的生存期

1. Javascript变量的生命期从它们被声明的事件开始
2. 局部变量会在函数运行以后被删除
3. 全局变量会在页面关闭后被删除

#### 向为声明的Javascript变量分配值

1. 如果您把值赋给尚未声明的变量，该变量将被自动作为 window 的一个属性。
2. 在非严格模式下给未声明变量赋值创建的全局变量，是全局对象的可配置属性，可以删除。

```javascript
var var1 = 1; // 不可配置全局属性
var2 = 2; // 没有使用 var 声明，可配置全局属性

console.log(this.var1); // 1
console.log(window.var1); // 1
console.log(window.var2); // 2

delete var1; // false 无法删除
console.log(var1); //1

delete var2; 
console.log(delete var2); // true
console.log(var2); // 已经删除 报错变量未定义
```

| 方式                            | 是否自动挂到 `window` | 可否删除   | 严格模式下行为 |
| ------------------------------- | --------------------- | ---------- | -------------- |
| `carname = "Volvo"`（未声明）   | ✅ 是                  | ✅ 可以删除 | ❌ 报错         |
| `var carname = "Volvo"`         | ✅ 是                  | ❌ 不可删除 | ✅ 正常         |
| `let / const carname = "Volvo"` | ❌ 否                  | ❌ 不可删除 | ✅ 正常         |

## Javascript作用域

1. 在Javascript中，对象和函数同样也是变量
2. 在Javascript中，作用域未可访问变量、对象、函数的集合
3. Javascript函数作用域：作用域在函数内修改

### Javascript局部作用域

1. 变量在函数内声明，变量为局部变量，具有局部作用域；局部变量，只能在函数内部访问
2. 局部变量在函数开始执行时创建，函数执行完后局部变量会自动销毁

### Javascript全局变量

1. 变量在函数外定义，即为全局变量
2. 全局变量有**全局作用域**：网页中所有脚本和函数均可使用

```javascript
var carName = " Volvo";
 
// 此处可调用 carName 变量
function myFunction() {
    // 函数内可调用 carName 变量
}
```

1. 如果变量在函数内没有声明（**没有使用var关键字**），该变量为全局变量。

```javascript
// 此处可调用 carName 变量
 
function myFunction() {
    carName = "Volvo";
    // 此处可调用 carName 变量
}
```

### HTML中的全局变量

1. 在HTML中，全局变量是window对象，所以window对象可以调用函数内的未声明（未加var）的局部变量。

1. 1. **注意**：所有全局变量都属于window对象

1. 在Javascript中，函数内部的局部变量通常不可以直接被外部作用域访问，但有几种方式可以将函数内的局部变量暴露给外部作用域，具体如下：

- **通过全局对象：**在函数内部，可以通过将局部变量赋值给 window 对象的属性来使其成为全局可访问的。例如，使用 **window.a = a;** 语句，可以在函数外部通过 **window.a** 访问到这个局部变量的值。
- **定义全局变量：**在函数内部不使用 **var、let** 或 **const** 等关键字声明变量时，该变量会被视为全局变量，从而可以在函数外部访问。但这种做法通常不推荐，因为它可能导致意外的副作用和代码难以维护。
- **返回值：**可以通过在函数内部使用 **return** 语句返回局部变量的值，然后在函数外部接收这个返回值。这样，虽然局部变量本身不会被暴露，但其值可以通过函数调用传递到外部。
- **闭包：**JavaScript 中的闭包特性允许内部函数访问外部函数的局部变量。即使外部函数执行完毕后，其局部变量仍然可以被内部函数引用。
- **属性和方法：**定义在全局作用域中的变量和函数都会变成 window 对象的属性和方法，因此可以在调用时省略 window，直接使用变量名或函数名。

# Javascript事件

## HTML事件

1. HTML事件可以是浏览器行为，也可以是用户行为。以下是HTML事件的实例：

1. 1. HTML页面完成加载
   2. HTML input字段改变时
   3. HTML按钮被点击

1. 当事件发生时，Javascript可以执行一些代码；HTML元素中可以添加事件属性，使用Javascript代码来添加HTML元素。

1. 1. 单引号：`<some-HTML-element some-event='JavaScript 代码'>`
   2. 双引号：`<some-HTML-element some-event="JavaScript 代码">`

1. 在以下实例中，按钮元素中添加了onclick属性（并加上代码），Javascript代码将修改id=“demo”元素的内容：

```javascript
<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
```

### 常见的HTML事件

| 事件        | 描述                                 |
| ----------- | ------------------------------------ |
| onchange    | HTML 元素改变                        |
| onclick     | 用户点击 HTML 元素                   |
| onmouseover | 鼠标指针移动到指定的元素上时发生     |
| onmouseout  | 用户从一个 HTML 元素上移开鼠标时发生 |
| onkeydown   | 用户按下键盘按键                     |
| onload      | 浏览器已完成页面的加载               |

## Javascript可以做什么？

1. 事件可以用于处理表单验证，用户输入，用户行为及浏览器动作：

- 页面加载时触发事件
- 页面关闭时触发事件
- 用户点击按钮执行动作
- 验证用户输入内容的合法性
- 等等 ...

1. 可以使用多种方式来执行Javascript事件代码：

- HTML 事件属性可以直接执行 JavaScript 代码
- HTML 事件属性可以调用 JavaScript 函数
- 你可以为 HTML 元素指定自己的事件处理程序
- 你可以阻止事件的发生。
- 等等 ...