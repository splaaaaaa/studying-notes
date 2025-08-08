# React 路由（`react-router-dom`）详解

## 一、路由基础：安装与核心组件

在 React 中实现路由功能需依赖 `react-router-dom` 库，它是专门为浏览器环境设计的路由解决方案。

### 1. 安装

```bash
npm install react-router-dom  # 或 yarn add react-router-dom
```

### 2. 核心组件说明

| 组件            | 作用                                      | 应用场景                                 |
| --------------- | ----------------------------------------- | ---------------------------------------- |
| `BrowserRouter` | 提供路由上下文（整个路由系统的容器）      | 包裹所有路由相关组件，通常在根组件中使用 |
| `Routes`        | 路由容器，用于包裹多个 `Route`            | 管理路由规则集合                         |
| `Route`         | 定义路径与组件的映射关系                  | `<Route path="/home" element={<Home />}` |
| `Link`          | 路由导航链接（类似 `<a>` 标签，但无刷新） | 替代 `<a>` 实现页面跳转                  |
| `Outlet`        | 嵌套路由的占位符                          | 在父路由组件中显示子路由内容             |
| `Navigate`      | 重定向组件                                | 实现页面跳转（如 404 页面重定向）        |

## 二、基础路由实现（实例代码）

### 1. 创建路由组件

首先定义需要路由的页面组件：

```jsx
// 首页组件
function Home() {
  return <h2>首页</h2>;
}

// 关于页组件
function About() {
  return <h2>关于我们</h2>;
}

// 联系页组件
function Contact() {
  return <h2>联系我们</h2>;
}
```

### 2. 配置路由规则

在根组件中使用核心组件配置路由：

```jsx
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    // 1. 用 Router 包裹整个应用（提供路由上下文）
    <Router>
      <div>
        {/* 2. 导航菜单（用 Link 替代 <a> 标签，避免页面刷新） */}
        <nav>
          <Link to="/">首页 | </Link>
          <Link to="/about">关于我们 | </Link>
          <Link to="/contact">联系我们</Link>
        </nav>

        {/* 3. 路由规则（用 Routes 和 Route 定义路径与组件的映射） */}
        <Routes>
          <Route path="/" element={<Home />} />       {/* 根路径对应 Home 组件 */}
          <Route path="/about" element={<About />} /> {/* /about 对应 About 组件 */}
          <Route path="/contact" element={<Contact />} /> {/* /contact 对应 Contact 组件 */}
        </Routes>
      </div>
    </Router>
  );
}
```

**代码说明**：

- `BrowserRouter` 必须包裹所有路由相关内容，通常别名 `Router` 简化书写。
- `Link` 的 `to` 属性指定跳转路径，点击后 URL 变化且页面无刷新。
- `Routes` 会匹配当前 URL 与 `Route` 的 `path`，渲染对应的 `element` 组件。

## 三、嵌套路由（子路由）

嵌套路由用于在父组件中展示子组件，例如后台管理系统的 “首页> 个人中心” 层级结构。

### 实例：后台仪表盘嵌套路由

```jsx
// 父组件：仪表盘
function Dashboard() {
  return (
    <div>
      <h2>仪表盘</h2>
      {/* 子路由导航 */}
      <nav>
        <Link to="profile">个人资料 | </Link>
        <Link to="settings">设置</Link>
      </nav>
      {/* 子路由占位符：子组件会在这里渲染 */}
      <Outlet />
    </div>
  );
}

// 子组件 1：个人资料
function Profile() {
  return <h3>个人资料内容</h3>;
}

// 子组件 2：设置
function Settings() {
  return <h3>设置内容</h3>;
}

// 路由配置
function App() {
  return (
    <Router>
      <Routes>
        {/* 父路由：path="/dashboard" 对应 Dashboard 组件 */}
        <Route path="/dashboard" element={<Dashboard />}>
          {/* 子路由：路径相对于父路由（完整路径为 /dashboard/profile） */}
          <Route path="profile" element={<Profile />} />
          <Route path="settings" element={<Settings />} />
        </Route>
      </Routes>
    </Router>
  );
}
```

**核心逻辑**：

- 父路由通过 `element={<Dashboard />}` 指定渲染组件。
- 子路由嵌套在父 `Route` 内部，路径为相对路径（无需加 `/`）。
- 父组件中通过 `<Outlet />` 预留位置，子组件会在这里显示。

## 四、动态路由（参数传递）

动态路由用于匹配变化的路径（如 `/user/123`、`/user/456`），通过 `useParams` 钩子获取参数。

### 实例：用户详情页

```jsx
import { useParams } from 'react-router-dom';

// 用户详情组件
function User() {
  // 获取 URL 中的动态参数（path 中定义的 :userId）
  const { userId } = useParams();

  return <h2>用户 ID：{userId}</h2>;
}

// 路由配置
function App() {
  return (
    <Router>
      <Routes>
        {/* 动态路径：:userId 为参数占位符 */}
        <Route path="/user/:userId" element={<User />} />
      </Routes>
    </Router>
  );
}
```

**效果**：

- 访问 `/user/123` → 显示 `用户 ID：123`
- 访问 `/user/456` → 显示 `用户 ID：456`

## 五、404 页面与重定向

通过 `Navigate` 组件实现页面重定向，常用于 404 页面或未登录跳转。

### 实例：404 页面配置

```jsx
import { Navigate } from 'react-router-dom';

// 404 组件
function NotFound() {
  return <h2>页面不存在（404）</h2>;
}

// 路由配置
function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        {/* 匹配所有未定义的路径（* 为通配符） */}
        <Route path="*" element={<NotFound />} />
        {/* 示例：重定向（访问 /old 自动跳转到 /new） */}
        <Route path="/old" element={<Navigate to="/new" />} />
      </Routes>
    </Router>
  );
}
```

## 六、路由跳转与编程式导航

除了 `Link` 组件，还可以通过 `useNavigate` 钩子实现编程式导航（如表单提交后跳转）。

```jsx
import { useNavigate } from 'react-router-dom';

function Login() {
  const navigate = useNavigate(); // 获取导航函数

  const handleLogin = () => {
    // 模拟登录成功后跳转到首页
    navigate('/'); // 跳转到根路径
    // 也可以跳转到上一页：navigate(-1)
  };

  return <button onClick={handleLogin}>登录</button>;
}
```

## 七、总结

| 功能场景     | 实现方式                                  | 核心 API / 组件                   |
| ------------ | ----------------------------------------- | --------------------------------- |
| 基础路由配置 | `Routes` + `Route` 定义路径与组件映射     | `<Route path="x" element={<X />}` |
| 页面导航     | `Link` 组件（声明式）                     | `<Link to="x">`                   |
| 嵌套路由     | 父路由包含子 `Route`，用 `Outlet` 占位    | `<Outlet />`                      |
| 动态参数     | 路径中用 `:参数名` 定义，`useParams` 获取 | `const { 参数名 } = useParams()`  |
| 404 与重定向 | `Navigate` 组件 + 通配符 `*`              | `<Navigate to="x" />`             |
| 编程式导航   | `useNavigate` 钩子                        | `navigate('/path')`               |

`react-router-dom` 是 React 生态中最常用的路由库，通过上述功能可实现单页应用（SPA）的无刷新跳转、复杂路由层级和动态页面内容，是构建多页面 React 应用的核心工具。