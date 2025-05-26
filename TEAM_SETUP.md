# Team Setup Guide

This guide will help team members set up the DB Tool Client project on their local machines.

## Prerequisites

1. **Git**
   - Download and install Git from [https://git-scm.com/downloads](https://git-scm.com/downloads)
   - Verify installation: `git --version`

2. **Node.js**
   - Install Node.js v16 or higher from [https://nodejs.org/](https://nodejs.org/)
   - Verify installation: `node --version`

3. **Package Manager**
   - We recommend using pnpm (faster and more efficient)
   - Install pnpm globally: `npm install -g pnpm`
   - Verify installation: `pnpm --version`

## Getting Started

### First Time Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/041103621/db-tool-client.git
   cd db-tool-client
   ```

2. **Switch to Development Branch**
   ```bash
   git checkout demo-dev
   ```

3. **Install Dependencies**
   ```bash
   # Using pnpm (recommended)
   pnpm install

   # Or using npm
   npm install

   # Or using yarn
   yarn install
   ```

4. **Start Development Server**
   ```bash
   npm run dev
   ```

### For Existing Repository

If you already have the repository cloned:

1. **Update Your Local Repository**
   ```bash
   git fetch origin
   ```

2. **Switch to Development Branch**
   ```bash
   git checkout demo-dev
   ```

3. **Update Dependencies**
   ```bash
   pnpm install
   ```

## Development Workflow

1. **Before Starting Work**
   ```bash
   git checkout demo-dev
   git pull origin demo-dev
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Making Changes**
   - Make your changes
   - Test your changes locally
   - Commit your changes:
     ```bash
     git add .
     git commit -m "descriptive message about your changes"
     ```

4. **Pushing Changes**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Creating Pull Request**
   - Go to GitHub repository
   - Create a new pull request from your feature branch to `demo-dev`
   - Wait for code review and approval

## Common Issues and Solutions

### Port Already in Use
If you see "Port already in use" error when running the development server:
1. Find the process using the port: `netstat -ano | findstr :5173`
2. Kill the process: `taskkill /PID <process_id> /F`

### Node Modules Issues
If you encounter dependency-related issues:
1. Delete node_modules folder and package-lock.json/pnpm-lock.yaml
2. Run `pnpm install` again

## Project Structure Reference

```
src/
├── pages/           # Page Components
│   ├── index.vue    # Home Page
│   └── nav/         # Navigation Pages
│       ├── 1/       # Database Management Module
│       └── 2/       # Monitoring Module
```

## Backend Connection

The application connects to:
- API Endpoint: `http://localhost:5001/api/emp`
- WebSocket: `http://localhost:5001/logs`

Make sure the backend server is running before starting development.

## Need Help?

If you encounter any issues:
1. Check the existing documentation
2. Search for similar issues in the project's issue tracker
3. Ask team members for help
4. Create a new issue with detailed description of your problem

## Code Style Guidelines

1. Use TypeScript for type safety
2. Follow Vue 3 Composition API patterns
3. Use Element Plus components when possible
4. Write meaningful commit messages
5. Document any new features or changes 