# ðŸš€ Travel Agent App - Development Start Guide

**Status**: Ready for Implementation  
**Date**: November 26, 2025  
**Audience**: Development Team (Beginners to Intermediate)

---

## ðŸ“– Table of Contents

1. [Development Environment Setup](#1-development-environment-setup)
2. [Project Initialization](#2-project-initialization)
3. [Monorepo Structure](#3-monorepo-structure)
4. [Database Setup](#4-database-setup)
5. [Git Workflow](#5-git-workflow)
6. [Common Commands](#6-common-commands)
7. [Troubleshooting](#7-troubleshooting)

---

## 1. Development Environment Setup

### **Prerequisites**
Before starting, install these globally:

- **Node.js**: v18 LTS or v20 (download from https://nodejs.org)
- **npm**: v8+ (comes with Node.js)
- **Git**: https://git-scm.com/download
- **Docker Desktop**: https://www.docker.com/products/docker-desktop (for PostgreSQL + Redis)
- **PostgreSQL**: v14+ (or use Docker)
- **Redis**: v7+ (or use Docker)

### **Verify Installations**

```bash
# Check Node.js version
node --version  # Should be v18.x.x or v20.x.x

# Check npm version
npm --version   # Should be v8.x.x or higher

# Check Git version
git --version   # Should be v2.x.x or higher

# Check Docker version
docker --version  # Should be Docker 20.x.x or higher
```

### **Global npm Packages** (Install once)

```bash
# Install Turbo (monorepo manager)
npm install -g turbo@latest

# Install TypeScript globally (optional, but helpful)
npm install -g typescript

# Install ESLint CLI globally (optional)
npm install -g eslint
```

### **IDE Setup** (Recommended: VS Code)

**VS Code Extensions** (Install these for development):
```
1. ESLint (Dirk Baeumer)
2. Prettier - Code formatter (Prettier)
3. Thunder Client or REST Client (for API testing)
4. SQLTools (for database management)
5. Docker (Microsoft)
6. GitLens (Eric Amodio)
7. Tabnine (Code completion AI)
8. Postman (API testing)
```

**VS Code Settings** (`.vscode/settings.json`):
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "eslint.validate": ["javascript", "typescript"],
  "files.exclude": {
    "**/node_modules": true,
    "**/.turbo": true
  }
}
```

---

## 2. Project Initialization

### **Step 1: Create Project Directory**

```bash
# Create project folder
mkdir travel-agent-app
cd travel-agent-app

# Initialize Git repository
git init
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

### **Step 2: Create Turbo Monorepo**

```bash
# Generate Turbo monorepo structure
npx create-turbo@latest . --empty

# This creates:
# - packages/ (for all workspaces)
# - turbo.json (configuration)
# - package.json (root)
# - .gitignore
```

### **Step 3: Configure Root package.json**

Replace the root `package.json` with:

```json
{
  "name": "travel-agent-app",
  "version": "0.1.0",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "test": "turbo run test",
    "lint": "turbo run lint",
    "format": "prettier --write \"**/*.{ts,tsx,js,jsx,json,md}\"",
    "format:check": "prettier --check \"**/*.{ts,tsx,js,jsx,json,md}\"",
    "clean": "turbo run clean && rm -rf node_modules"
  },
  "devDependencies": {
    "turbo": "latest",
    "prettier": "^3.1.0",
    "eslint": "^8.54.0",
    "eslint-config-prettier": "^9.1.0"
  },
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=8.0.0"
  }
}
```

### **Step 4: Create .gitignore**

```bash
cat > .gitignore << 'EOF'
# Dependencies
node_modules/
/.pnp
.pnp.js

# Testing
/coverage

# Production
/build
/dist
.next/
out/

# Misc
.DS_Store
*.pem
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*
lerna-debug.log*

# Turbo
.turbo/
dist/

# IDE
.vscode/
.idea/
*.swp
*.swo
EOF
```

### **Step 5: Create Environment Configuration**

```bash
# Create root .env.example
cat > .env.example << 'EOF'
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/travel_app_dev
REDIS_URL=redis://localhost:6379

# JWT
JWT_SECRET=your-secret-key-change-in-production
JWT_EXPIRY=15m
REFRESH_TOKEN_EXPIRY=7d

# Google OAuth
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# Google Maps API
GOOGLE_MAPS_API_KEY=your-google-maps-api-key

# Razorpay (Payment)
RAZORPAY_KEY_ID=your-razorpay-key-id
RAZORPAY_KEY_SECRET=your-razorpay-key-secret

# Firebase (Push Notifications)
FIREBASE_PROJECT_ID=your-firebase-project-id
FIREBASE_PRIVATE_KEY=your-firebase-private-key
FIREBASE_CLIENT_EMAIL=your-firebase-client-email

# App URLs
WEB_URL=http://localhost:3000
API_URL=http://localhost:4000
APP_DEEP_LINK=travelapp://

# Environment
NODE_ENV=development
LOG_LEVEL=debug
EOF

# Copy to local .env (don't commit this!)
cp .env.example .env
echo ".env" >> .gitignore
```

---

## 3. Monorepo Structure

### **Create Workspace Directories**

```bash
# Navigate to packages directory
cd packages

# Create workspaces
mkdir api
mkdir web
mkdir mobile
mkdir shared

cd ..
```

### **Workspace Structure Overview**

```
travel-agent-app/
â”œâ”€â”€ packages/
â”‚   â”œâ”€â”€ api/                 # Backend (Node.js + Express)
â”‚   â”‚   â”œâ”€â”€ src/
â”‚   â”‚   â”‚   â”œâ”€â”€ index.ts
â”‚   â”‚   â”‚   â”œâ”€â”€ routes/
â”‚   â”‚   â”‚   â”œâ”€â”€ controllers/
â”‚   â”‚   â”‚   â”œâ”€â”€ models/
â”‚   â”‚   â”‚   â”œâ”€â”€ middleware/
â”‚   â”‚   â”‚   â”œâ”€â”€ services/
â”‚   â”‚   â”‚   â”œâ”€â”€ utils/
â”‚   â”‚   â”‚   â””â”€â”€ config/
â”‚   â”‚   â”œâ”€â”€ package.json
â”‚   â”‚   â”œâ”€â”€ tsconfig.json
â”‚   â”‚   â””â”€â”€ .env.example
â”‚   â”‚
â”‚   â”œâ”€â”€ web/                 # React Web App
â”‚   â”‚   â”œâ”€â”€ src/
â”‚   â”‚   â”‚   â”œâ”€â”€ pages/
â”‚   â”‚   â”‚   â”œâ”€â”€ components/
â”‚   â”‚   â”‚   â”œâ”€â”€ hooks/
â”‚   â”‚   â”‚   â”œâ”€â”€ services/
â”‚   â”‚   â”‚   â”œâ”€â”€ store/
â”‚   â”‚   â”‚   â”œâ”€â”€ utils/
â”‚   â”‚   â”‚   â””â”€â”€ App.tsx
â”‚   â”‚   â”œâ”€â”€ public/
â”‚   â”‚   â”œâ”€â”€ package.json
â”‚   â”‚   â”œâ”€â”€ tsconfig.json
â”‚   â”‚   â””â”€â”€ vite.config.ts
â”‚   â”‚
â”‚   â”œâ”€â”€ mobile/              # React Native App
â”‚   â”‚   â”œâ”€â”€ src/
â”‚   â”‚   â”‚   â”œâ”€â”€ screens/
â”‚   â”‚   â”‚   â”œâ”€â”€ components/
â”‚   â”‚   â”‚   â”œâ”€â”€ hooks/
â”‚   â”‚   â”‚   â”œâ”€â”€ services/
â”‚   â”‚   â”‚   â”œâ”€â”€ store/
â”‚   â”‚   â”‚   â”œâ”€â”€ utils/
â”‚   â”‚   â”‚   â””â”€â”€ App.tsx
â”‚   â”‚   â”œâ”€â”€ app.json
â”‚   â”‚   â”œâ”€â”€ package.json
â”‚   â”‚   â””â”€â”€ tsconfig.json
â”‚   â”‚
â”‚   â””â”€â”€ shared/              # Shared Types & Utils
â”‚       â”œâ”€â”€ src/
â”‚       â”‚   â”œâ”€â”€ types/
â”‚       â”‚   â”œâ”€â”€ constants/
â”‚       â”‚   â”œâ”€â”€ utils/
â”‚       â”‚   â””â”€â”€ validation/
â”‚       â”œâ”€â”€ package.json
â”‚       â””â”€â”€ tsconfig.json
â”‚
â”œâ”€â”€ .env.example
â”œâ”€â”€ .gitignore
â”œâ”€â”€ turbo.json
â”œâ”€â”€ package.json
â””â”€â”€ README.md
```

### **Initialize Each Workspace**

#### **Shared Package** (`packages/shared/package.json`):
```json
{
  "name": "@travel-app/shared",
  "version": "0.1.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "private": true,
  "scripts": {
    "build": "tsc",
    "clean": "rm -rf dist",
    "dev": "tsc --watch"
  },
  "devDependencies": {
    "typescript": "^5.3.3",
    "@types/node": "^20.10.0"
  }
}
```

#### **API Package** (`packages/api/package.json`):
```json
{
  "name": "@travel-app/api",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "lint": "eslint src --ext .ts",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.2",
    "dotenv": "^16.3.1",
    "socket.io": "^4.7.2",
    "pg": "^8.11.3",
    "redis": "^4.6.12",
    "jsonwebtoken": "^9.1.2",
    "bcryptjs": "^2.4.3",
    "@travel-app/shared": "*"
  },
  "devDependencies": {
    "typescript": "^5.3.3",
    "@types/express": "^4.17.21",
    "@types/node": "^20.10.0",
    "tsx": "^4.7.0",
    "eslint": "^8.54.0",
    "@typescript-eslint/eslint-plugin": "^6.13.2",
    "@typescript-eslint/parser": "^6.13.2"
  }
}
```

#### **Web Package** (`packages/web/package.json`):
```json
{
  "name": "@travel-app/web",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint src --ext .ts,.tsx"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.1",
    "socket.io-client": "^4.7.2",
    "@tanstack/react-query": "^5.20.0",
    "zustand": "^4.4.5",
    "@travel-app/shared": "*"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.2.1",
    "vite": "^5.0.8",
    "typescript": "^5.3.3",
    "@types/react": "^18.2.37",
    "@types/react-dom": "^18.2.15"
  }
}
```

#### **Mobile Package** (`packages/mobile/package.json`):
```json
{
  "name": "@travel-app/mobile",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "expo start",
    "build": "expo build",
    "test": "jest"
  },
  "dependencies": {
    "expo": "^50.0.0",
    "react": "^18.2.0",
    "react-native": "^0.73.0",
    "react-native-gesture-handler": "^2.14.1",
    "@react-navigation/native": "^6.1.9",
    "@react-navigation/bottom-tabs": "^6.5.11",
    "socket.io-client": "^4.7.2",
    "zustand": "^4.4.5",
    "@travel-app/shared": "*"
  },
  "devDependencies": {
    "typescript": "^5.3.3",
    "@types/react": "^18.2.37",
    "expo-cli": "^6.3.0"
  }
}
```

---

## 4. Database Setup

### **Option A: Using Docker** (Recommended for Development)

```bash
# Create docker-compose.yml in root directory
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  postgres:
    image: postgres:16-alpine
    container_name: travel_app_db
    environment:
      POSTGRES_USER: travel_user
      POSTGRES_PASSWORD: travel_password
      POSTGRES_DB: travel_app_dev
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - travel_app_network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U travel_user -d travel_app_dev"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    container_name: travel_app_redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    networks:
      - travel_app_network
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
  redis_data:

networks:
  travel_app_network:
    driver: bridge
EOF

# Start containers
docker-compose up -d

# Verify containers are running
docker-compose ps

# View logs
docker-compose logs -f

# Stop containers
docker-compose down
```

**Update `.env` with Docker credentials**:
```
DATABASE_URL=postgresql://travel_user:travel_password@localhost:5432/travel_app_dev
REDIS_URL=redis://localhost:6379
```

### **Option B: Local PostgreSQL & Redis Installation**

**macOS**:
```bash
# Install via Homebrew
brew install postgresql redis

# Start services
brew services start postgresql
brew services start redis

# Verify
psql --version
redis-cli ping  # Should return "PONG"
```

**Linux (Ubuntu/Debian)**:
```bash
# Install PostgreSQL
sudo apt-get install postgresql postgresql-contrib

# Install Redis
sudo apt-get install redis-server

# Start services
sudo systemctl start postgresql
sudo systemctl start redis-server
```

**Windows**:
- Download PostgreSQL: https://www.postgresql.org/download/windows/
- Download Redis: https://github.com/microsoftarchive/redis/releases

### **Create Database & User**

```bash
# Connect to PostgreSQL
psql -U postgres

# Run these SQL commands:
CREATE USER travel_user WITH PASSWORD 'travel_password';
CREATE DATABASE travel_app_dev OWNER travel_user;
GRANT ALL PRIVILEGES ON DATABASE travel_app_dev TO travel_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO travel_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON SEQUENCES TO travel_user;

# Exit psql
\q
```

### **Verify Database Connection**

```bash
# Test connection
psql -U travel_user -d travel_app_dev -h localhost

# If successful, you'll see the prompt:
travel_app_dev=>

# Exit
\q
```

---

## 5. Git Workflow

### **Initial Commit**

```bash
# Stage all files
git add .

# Create initial commit
git commit -m "chore: initialize monorepo structure"

# Create main branch (if not already on it)
git branch -M main

# Add remote origin (replace with your repo URL)
git remote add origin https://github.com/yourusername/travel-agent-app.git

# Push to remote
git push -u origin main
```

### **Branch Naming Convention**

```
feature/[feature-name]     # New feature: feature/expense-splitting
bugfix/[bug-name]          # Bug fix: bugfix/offline-sync-issue
refactor/[component]       # Refactoring: refactor/api-middleware
docs/[section]             # Documentation: docs/database-schema
chore/[task]               # Maintenance: chore/update-dependencies
```

### **Commit Message Convention**

```
type(scope): description

# Types:
feat     - New feature
fix      - Bug fix
refactor - Code reorganization (no feature/bug change)
test     - Test additions/changes
docs     - Documentation changes
chore    - Maintenance, dependency updates

# Examples:
feat(auth): implement google oauth login
fix(expense): correct split calculation logic
refactor(api): reorganize middleware structure
chore(deps): update express to v4.19.0
```

---

## 6. Common Commands

### **Development**

```bash
# Install dependencies (from root directory)
npm install

# Run all dev servers (web + api)
npm run dev

# Run specific workspace dev
cd packages/api
npm run dev

# Run linting
npm run lint

# Format code
npm run format

# Check formatting (without changes)
npm run format:check
```

### **Database Migrations** (After setting up Sequelize/TypeORM)

```bash
# Create migration
npm run db:migration:create --name=create_users_table

# Run migrations
npm run db:migrate

# Rollback last migration
npm run db:migrate:undo

# Seed database (after setting up seeders)
npm run db:seed
```

### **Testing**

```bash
# Run all tests
npm run test

# Run tests in watch mode
npm run test:watch

# Run with coverage
npm run test:coverage
```

### **Building**

```bash
# Build all packages
npm run build

# Build specific package
cd packages/web
npm run build
```

### **Docker Commands**

```bash
# View running containers
docker-compose ps

# View logs
docker-compose logs -f postgres
docker-compose logs -f redis

# Stop all services
docker-compose down

# Remove all data (WARNING: deletes database!)
docker-compose down -v

# Restart services
docker-compose restart
```

---

## 7. Troubleshooting

### **"Command not found: turbo"**
```bash
# Solution: Install globally
npm install -g turbo@latest

# Or use npx
npx turbo run dev
```

### **"Port 5432 already in use"**
```bash
# Find process using port
lsof -i :5432

# Kill process (replace PID)
kill -9 <PID>

# Or change port in docker-compose.yml
# Change: "5432:5432" â†’ "5433:5432"
```

### **"Cannot find module '@travel-app/shared'"**
```bash
# Solution: Reinstall dependencies
rm -rf node_modules package-lock.json
npm install

# Or rebuild shared package
cd packages/shared
npm run build
```

### **PostgreSQL connection refused**
```bash
# Check if service is running
docker-compose ps

# If not running, start it
docker-compose up -d postgres

# Check logs
docker-compose logs postgres

# Verify credentials in .env
cat .env
```

### **Redis connection failed**
```bash
# Check if Redis is running
docker-compose ps

# Test connection
redis-cli ping  # Should return "PONG"

# If not running, start it
docker-compose up -d redis
```

### **"node_modules" bloat issues**
```bash
# Clean everything
npm run clean

# Reinstall
npm install

# Or selective cleanup
cd packages/api
rm -rf node_modules
npm install
```

---

## âœ… Post-Setup Checklist

Before you start coding:

- [ ] Node.js v18+ installed and verified
- [ ] Docker installed and running
- [ ] PostgreSQL & Redis containers running successfully
- [ ] Database created and connection verified
- [ ] All npm dependencies installed
- [ ] Git repository initialized and first commit done
- [ ] .env file created with correct credentials
- [ ] IDE (VS Code) configured with extensions
- [ ] Can run `npm run dev` without errors
- [ ] Can connect to PostgreSQL (`psql` command works)
- [ ] Can connect to Redis (`redis-cli ping` returns PONG)

---

## ðŸš€ First Development Commands

After completing setup:

```bash
# Terminal 1: Start backend API
cd packages/api
npm run dev

# Terminal 2: Start web frontend (new terminal)
cd packages/web
npm run dev

# Terminal 3: Optionally start mobile app (new terminal)
cd packages/mobile
npm run dev

# Terminal 4: Monitor database (optional, new terminal)
docker-compose logs -f postgres
```

You should now see:
- âœ… API running on `http://localhost:4000`
- âœ… Web running on `http://localhost:3000`
- âœ… PostgreSQL accessible
- âœ… Redis accessible

---

## ðŸ“š Next Steps

1. **Phase 1 Implementation**: Check `PHASE_1_JIRA_BREAKDOWN.md`
2. **TypeScript Types**: Check `SHARED_TYPES.ts`
3. **API Structure**: Check `API_ROUTE_SCAFFOLDING.md`
4. **React Components**: Check `REACT_COMPONENT_STRUCTURE.md`
5. **First Commit**: Check `FIRST_COMMIT_CHECKLIST.md`

---

**Document Version**: 1.0  
**Last Updated**: November 26, 2025  
**Status**: Ready for Use
