# DB Tool Client

一个用于数据库管理和监控的Web客户端应用，基于Vue 3、Vite和Element Plus构建。

## 项目介绍

DB Tool Client是一个现代化的前端应用，旨在为数据库管理和监控提供直观的用户界面。它通过与后端API交互，实现对数据库表的查询、展示和监控功能。

### 主要功能

- **员工数据管理**: 查询和展示员工(EMP)表数据
- **部门数据管理**: 查询和展示部门(DEPT)表数据
- **薪资等级管理**: 查询和展示薪资等级(SALGRADE)表数据
- **日志监控**: 实时查看和监控系统日志，支持WebSocket连接

## 技术栈

- **前端框架**: Vue 3 (使用Composition API)
- **构建工具**: Vite
- **UI组件库**: Element Plus
- **状态管理**: Vue的响应式系统
- **网络请求**: Fetch API 和 Socket.IO
- **路由管理**: Vue Router
- **代码格式**: ESLint、TypeScript

## 项目结构

```
src/
├── pages/           # 页面组件
│   ├── index.vue    # 首页
│   └── nav/         # 导航页面
│       ├── 1/       # 数据库管理模块
│       │   ├── emp.vue      # 员工表管理
│       │   ├── dept.vue     # 部门表管理
│       │   └── salgrade.vue # 薪资等级管理
│       └── 2/       # 监控模块
│           └── log-monitor.vue # 日志监控
```

## 安装和运行

### 环境要求

- Node.js (推荐v16或更高版本)
- pnpm (推荐) 或 npm 或 yarn

### 安装依赖

```bash
# 使用pnpm (推荐)
pnpm install

# 或使用npm
npm install

# 或使用yarn
yarn install
```

### 开发模式运行

```bash
npm run dev
```

### 构建生产版本

```bash
npm run build
```

### 预览生产构建

```bash
npm run preview
```

### 类型检查

```bash
npm run typecheck
```

## 后端API

该前端应用需要连接到后端API服务。默认配置下，后端服务应运行在：

- 员工数据API: `http://localhost:5001/api/emp`
- 日志监控WebSocket: `http://localhost:5001/logs`

## 自定义主题

可以通过修改`src/styles/element/index.scss`文件来自定义Element Plus的主题。

## 许可证

[MIT License](LICENSE)
