# DB Tool Client

A web client application for database management and monitoring, built with Vue 3, Vite, and Element Plus.

## Project Introduction

DB Tool Client is a modern frontend application designed to provide an intuitive user interface for database management and monitoring. It interacts with backend APIs to implement query, display, and monitoring functions for database tables.

### Main Features

- **Employee Data Management**: Query and display employee (EMP) table data
- **Department Data Management**: Query and display department (DEPT) table data
- **Salary Grade Management**: Query and display salary grade (SALGRADE) table data
- **Log Monitoring**: Real-time viewing and monitoring of system logs with WebSocket connection support

## Tech Stack

- **Frontend Framework**: Vue 3 (using Composition API)
- **Build Tool**: Vite
- **UI Component Library**: Element Plus
- **State Management**: Vue's reactive system
- **Network Requests**: Fetch API and Socket.IO
- **Routing**: Vue Router
- **Code Formatting**: ESLint, TypeScript

## Project Structure

```
src/
├── pages/           # Page Components
│   ├── index.vue    # Home Page
│   └── nav/         # Navigation Pages
│       ├── 1/       # Database Management Module
│       │   ├── emp.vue      # Employee Table Management
│       │   ├── dept.vue     # Department Table Management
│       │   └── salgrade.vue # Salary Grade Management
│       └── 2/       # Monitoring Module
│           └── log-monitor.vue # Log Monitoring
```

## Installation and Running

### Requirements

- Node.js (v16 or higher recommended)
- pnpm (recommended) or npm or yarn

### Install Dependencies

```bash
# Use pnpm (recommended)
pnpm install

# Or use npm
npm install

# Or use yarn
yarn install
```

### Run in Development Mode

```bash
npm run dev
```

### Build for Production

```bash
npm run build
```

### Preview Production Build

```bash
npm run preview
```

### Type Check

```bash
npm run typecheck
```

## Backend API

This frontend application needs to connect to backend API services. By default, the backend service should run on:

- Employee Data API: `http://localhost:5001/api/emp`
- Log Monitoring WebSocket: `http://localhost:5001/logs`

## Custom Theme

You can customize the Element Plus theme by modifying the `src/styles/element/index.scss` file.

## License

[MIT License](LICENSE)
