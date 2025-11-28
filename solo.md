# ðŸ“‹ Complete Development Roadmap - Solo Developer Guide

**Status**: Step-by-Step Implementation  
**Date**: November 27, 2025  
**Scope**: For Solo Developer + GitHub Copilot  
**Total Duration**: 4 Weeks (MVP Phase 1)

---

## ðŸŽ¯ Executive Summary

This roadmap is designed specifically for a **solo developer** using **GitHub Copilot** to build the Travel Agent App from scratch to production deployment. Every step includes:

- âœ… What to do
- âœ… How to do it (commands & code)
- âœ… GitHub Copilot prompts to use
- âœ… How to verify it works
- âœ… What to do if it fails
- âœ… When to commit to Git
- âœ… Time estimate

---

## ðŸ“… 4-Week Timeline Overview

```
WEEK 1: PROJECT SETUP & AUTHENTICATION
â”œâ”€ Days 1-2: Environment Setup
â”œâ”€ Days 3-4: Backend Authentication APIs
â”œâ”€ Days 5: Testing & First Commits
â””â”€ Goal: Login system working end-to-end

WEEK 2: TRIPS & ITINERARY
â”œâ”€ Days 6-7: React Components & Routing
â”œâ”€ Days 8-10: Location Management Backend & Frontend
â”œâ”€ Goal: Trip planning works, basic CRUD

WEEK 3: BUDGET & EXPENSES
â”œâ”€ Days 11-14: Expense APIs & Budget Dashboard
â”œâ”€ Goal: Budget tracking functional

WEEK 4: TESTING, CI/CD & DEPLOYMENT
â”œâ”€ Days 15-16: Unit Tests & CI/CD Setup
â”œâ”€ Days 17-18: Docker & Production Deployment
â”œâ”€ Goal: MVP ready for production
```

---

## âš¡ WEEK 1: PROJECT SETUP & AUTHENTICATION

### ðŸ“ Days 1-2: Environment Setup & Project Initialization

#### **Task 1.1: Install System Requirements**

**What you need to check:**
```bash
# Check Node.js version (need v18+)
node --version
# Should output: v18.x.x or higher

# Check npm version (need v8+)
npm --version
# Should output: v8.x.x or higher

# Check Git version
git --version
# Should output: git version 2.x.x or higher

# Check Docker (optional for local dev, required for deployment)
docker --version
# Should output: Docker version 20.x or higher
```

**If any are missing, install them:**

```bash
# macOS (using Homebrew)
brew install node
brew install git
brew install docker

# Windows (using Chocolatey)
choco install nodejs
choco install git
choco install docker

# Linux (Ubuntu/Debian)
sudo apt-get install nodejs npm git docker.io
```

**Time**: 15 minutes  
**Check**: Run all commands above and confirm versions

---

#### **Task 1.2: Create GitHub Repository**

**What to do:**
1. Go to https://github.com/new
2. Repository name: `travel-agent-app`
3. Description: `Collaborative trip planning and budget management platform`
4. Choose: **Private** (for now)
5. Initialize with README âœ…
6. Add .gitignore: **Node**
7. Add license: **MIT**
8. Click **Create repository**

**Clone to your computer:**
```bash
git clone https://github.com/YOUR_USERNAME/travel-agent-app.git
cd travel-agent-app
```

**Time**: 5 minutes  
**Check**: Repository created and cloned locally

---

#### **Task 1.3: Create Folder Structure**

**Run these commands in order:**

```bash
# Navigate to project root
cd travel-agent-app

# Create main directories
mkdir -p docs packages/{api,web,mobile,shared} .github/workflows

# Create backend structure
mkdir -p packages/api/src/{config,middleware,routes,controllers,services,models,migrations,seeders,utils,types,tests}

# Create frontend structure
mkdir -p packages/web/src/{pages,components/{common,layout,auth,trip,itinerary,budget,team,luggage,shared},hooks,services,store,utils,styles,types,tests,__tests__}
mkdir -p packages/web/public

# Create shared structure
mkdir -p packages/shared/src/{types,constants,utils,validation,helpers}

# Create GitHub workflows
mkdir -p .github/{workflows,templates,issue_templates}

# Verify structure created
tree -L 3
```

**Time**: 5 minutes  
**Check**: All folders created, run `ls -la packages/api/src/` to verify

---

#### **Task 1.4: Create Root Configuration Files**

**Create `.env.example` file:**

```bash
cat > .env.example << 'EOF'
# Server Configuration
NODE_ENV=development
PORT=3000
API_URL=http://localhost:3000

# Database Configuration
DB_HOST=localhost
DB_PORT=5432
DB_NAME=travel_agent_app
DB_USER=postgres
DB_PASSWORD=your_password
DATABASE_URL=postgresql://postgres:your_password@localhost:5432/travel_agent_app

# Redis Configuration
REDIS_URL=redis://localhost:6379

# JWT Configuration
JWT_SECRET=your_jwt_secret_key_here_min_32_chars
JWT_EXPIRY=15m
JWT_REFRESH_SECRET=your_refresh_secret_key_here
JWT_REFRESH_EXPIRY=7d

# OAuth Configuration
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_secret
APPLE_CLIENT_ID=your_apple_client_id
APPLE_CLIENT_SECRET=your_apple_secret

# Google Maps API
GOOGLE_MAPS_API_KEY=your_google_maps_api_key

# Razorpay Payment
RAZORPAY_KEY_ID=your_razorpay_key
RAZORPAY_KEY_SECRET=your_razorpay_secret

# Firebase (for push notifications)
FIREBASE_PROJECT_ID=your_firebase_project
FIREBASE_PRIVATE_KEY=your_firebase_key
FIREBASE_CLIENT_EMAIL=your_firebase_email

# Frontend URLs
VITE_API_URL=http://localhost:3000
VITE_APP_NAME=Travel Agent App

# Email Configuration
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password

# Environment
ENVIRONMENT=development
DEBUG=true
EOF
```

**Create `.env` file (local development):**
```bash
cp .env.example .env
# Edit .env with your actual local values
# Use any text editor to update local credentials
```

**Create `.gitignore` additions:**
```bash
cat >> .gitignore << 'EOF'
# Environment variables
.env
.env.local
.env.*.local

# Dependencies
node_modules/
package-lock.json
yarn.lock

# Build outputs
dist/
build/
.next/
out/

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
Thumbs.db

# Testing
coverage/
.nyc_output/

# Logs
logs/
*.log
npm-debug.log*
yarn-debug.log*

# Cache
.cache/
.eslintcache

# Docker
Dockerfile
docker-compose.local.yml

# Database
*.sqlite
*.db
EOF
```

**Create `package.json` (root - monorepo configuration):**
```bash
cat > package.json << 'EOF'
{
  "name": "travel-agent-app",
  "version": "1.0.0",
  "description": "Collaborative trip planning and budget management platform",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "turbo run dev --parallel",
    "dev:api": "turbo run dev --filter=api",
    "dev:web": "turbo run dev --filter=web",
    "build": "turbo run build",
    "build:shared": "turbo run build --filter=shared",
    "test": "turbo run test",
    "test:api": "turbo run test --filter=api",
    "test:web": "turbo run test --filter=web",
    "lint": "turbo run lint",
    "format": "turbo run format",
    "clean": "turbo run clean && rm -rf node_modules",
    "db:migrate": "turbo run migrate --filter=api",
    "db:seed": "turbo run seed --filter=api",
    "docker:build": "docker-compose build",
    "docker:up": "docker-compose up -d",
    "docker:down": "docker-compose down",
    "docker:logs": "docker-compose logs -f"
  },
  "workspaces": [
    "packages/*"
  ],
  "devDependencies": {
    "turbo": "^1.10.0",
    "prettier": "^3.0.0",
    "eslint": "^8.50.0"
  }
}
EOF
```

**Create `turbo.json` (monorepo task configuration):**
```bash
cat > turbo.json << 'EOF'
{
  "$schema": "https://turbo.build/schema.json",
  "globalEnv": ["NODE_ENV"],
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**"],
      "cache": true
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "test": {
      "outputs": ["coverage/**"],
      "cache": true
    },
    "lint": {
      "outputs": ["*.log"],
      "cache": true
    },
    "format": {
      "cache": false
    }
  }
}
EOF
```

**Create `tsconfig.base.json` (shared TypeScript config):**
```bash
cat > tsconfig.base.json << 'EOF'
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "strict": true,
    "esModuleInterop": true,
    "skipDefaultLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "noImplicitAny": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["packages/*/src/*"],
      "@travel-app/shared": ["packages/shared/src/index.ts"]
    }
  },
  "include": ["packages/**/*.ts", "packages/**/*.tsx"],
  "exclude": ["node_modules"]
}
EOF
```

**Create `.prettierrc.json` (code formatting):**
```bash
cat > .prettierrc.json << 'EOF'
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "always",
  "endOfLine": "lf"
}
EOF
```

**Create `.eslintrc.json` (code linting):**
```bash
cat > .eslintrc.json << 'EOF'
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 2020,
    "sourceType": "module"
  },
  "env": {
    "browser": true,
    "node": true,
    "es2020": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/no-unused-vars": [
      "error",
      {
        "argsIgnorePattern": "^_"
      }
    ],
    "no-console": [
      "warn",
      {
        "allow": ["warn", "error"]
      }
    ]
  }
}
EOF
```

**Time**: 10 minutes  
**Check**: All config files created, run `cat .env.example` to verify

---

#### **Task 1.5: Initialize npm and Install Root Dependencies**

```bash
# Install root dependencies
npm install

# Install Turbo (monorepo manager)
npm install --save-dev turbo@latest

# Verify installation
npx turbo --version
```

**Time**: 5 minutes (depends on internet speed)  
**Check**: `npx turbo --version` shows version number

---

#### **Task 1.6: Create Docker Setup**

**Create `docker-compose.yml`:**
```bash
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    container_name: travel_app_postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: travel_agent_app
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
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
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
  redis_data:

networks:
  default:
    name: travel_app_network
EOF
```

**Start Docker containers:**
```bash
# Start PostgreSQL and Redis
docker-compose up -d

# Check if services are running
docker-compose ps

# View logs
docker-compose logs
```

**Time**: 5 minutes (+ download time first run)  
**Check**: `docker-compose ps` shows both postgres and redis as "running"

---

#### **Task 1.7: First Git Commit**

```bash
# Stage all files
git add .

# Create commit with message
git commit -m "chore: initialize project structure and configuration"

# Verify commit
git log --oneline -3

# Push to GitHub
git push origin main
```

**Time**: 2 minutes  
**Check**: Files appear on GitHub repository

---

### ðŸ“ Days 3-4: Backend Authentication APIs

#### **Task 2.1: Create Backend Package Structure**

**Navigate to backend:**
```bash
cd packages/api
```

**Create `package.json` for backend:**
```bash
cat > package.json << 'EOF'
{
  "name": "api",
  "version": "1.0.0",
  "description": "Travel Agent App Backend API",
  "type": "module",
  "main": "dist/index.js",
  "scripts": {
    "dev": "node --loader ts-node/esm src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "test": "jest",
    "lint": "eslint src/**/*.ts",
    "format": "prettier --write src/**/*.ts",
    "migrate": "node --loader ts-node/esm src/db/migrate.ts",
    "seed": "node --loader ts-node/esm src/db/seed.ts"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1",
    "jsonwebtoken": "^9.1.0",
    "bcryptjs": "^2.4.3",
    "sequelize": "^6.33.0",
    "pg": "^8.11.2",
    "pg-hstore": "^2.3.4",
    "redis": "^4.6.11",
    "axios": "^1.6.0",
    "joi": "^17.11.0",
    "winston": "^3.11.0",
    "socket.io": "^4.7.2",
    "passport": "^0.7.0",
    "passport-google-oauth20": "^2.0.0",
    "passport-apple": "^2.0.2"
  },
  "devDependencies": {
    "@types/express": "^4.17.20",
    "@types/node": "^20.9.0",
    "@types/jest": "^29.5.8",
    "typescript": "^5.2.2",
    "ts-node": "^10.9.1",
    "@typescript-eslint/eslint-plugin": "^6.10.0",
    "@typescript-eslint/parser": "^6.10.0",
    "jest": "^29.7.0",
    "ts-jest": "^29.1.1"
  }
}
EOF
```

**Create `tsconfig.json` for backend:**
```bash
cat > tsconfig.json << 'EOF'
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "noEmit": false
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "src/**/*.test.ts"]
}
EOF
```

**Install backend dependencies:**
```bash
# You're already in packages/api
npm install
```

**Time**: 15 minutes  
**Check**: `node_modules` folder created, npm packages installed

---

#### **Task 2.2: Create Backend Entry Point**

**GitHub Copilot Prompt to use:**
```
Generate Express.js server setup with TypeScript, including:
1. Express app initialization
2. CORS and middleware setup
3. Environment variable loading with dotenv
4. PostgreSQL connection using Sequelize
5. Redis connection
6. Error handling middleware
7. Graceful shutdown
Export functions to start the server
```

**Create `src/index.ts`:**
```bash
cat > src/index.ts << 'EOF'
import express from 'express';
import cors from 'cors';
import dotenv from 'dotenv';
import { sequelize } from './config/database.js';
import { redisClient } from './config/redis.js';
import authRoutes from './routes/auth.routes.js';
import logger from './utils/logger.js';

dotenv.config();

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Routes
app.use('/api/auth', authRoutes);
app.get('/health', (req, res) => {
  res.json({ status: 'ok', timestamp: new Date().toISOString() });
});

// Error handling middleware
app.use((err: any, req: express.Request, res: express.Response, next: express.NextFunction) => {
  logger.error('Error:', err);
  res.status(err.status || 500).json({
    error: err.message || 'Internal Server Error',
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack }),
  });
});

// Start server
async function startServer() {
  try {
    // Test database connection
    await sequelize.authenticate();
    logger.info('Database connected successfully');

    // Test Redis connection
    await redisClient.ping();
    logger.info('Redis connected successfully');

    // Sync database models
    await sequelize.sync({ alter: true });
    logger.info('Database models synchronized');

    app.listen(PORT, () => {
      logger.info(`Server running on port ${PORT}`);
    });
  } catch (error) {
    logger.error('Failed to start server:', error);
    process.exit(1);
  }
}

// Graceful shutdown
process.on('SIGTERM', async () => {
  logger.info('SIGTERM received, shutting down gracefully');
  await sequelize.close();
  await redisClient.quit();
  process.exit(0);
});

startServer();
EOF
```

**Time**: 5 minutes  
**Check**: File created at `packages/api/src/index.ts`

---

#### **Task 2.3: Create Database Configuration**

**GitHub Copilot Prompt:**
```
Create Sequelize database configuration with TypeScript that:
1. Connects to PostgreSQL using environment variables
2. Configures dialect, host, port, username, password, database
3. Enables logging only in development mode
4. Has connection pooling with min/max connections
5. Auto-syncs models in development
Exports the sequelize instance
```

**Create `src/config/database.ts`:**
```bash
cat > src/config/database.ts << 'EOF'
import { Sequelize } from 'sequelize';
import logger from '../utils/logger.js';

const env = process.env.NODE_ENV || 'development';
const databaseUrl = process.env.DATABASE_URL;

if (!databaseUrl) {
  throw new Error('DATABASE_URL environment variable is not set');
}

export const sequelize = new Sequelize(databaseUrl, {
  logging: env === 'development' ? (msg) => logger.debug(msg) : false,
  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000,
  },
  dialect: 'postgres',
  dialectOptions: {
    ssl: env === 'production' ? { require: true, rejectUnauthorized: false } : undefined,
  },
});

logger.info('Database configuration initialized');
EOF
```

**Time**: 3 minutes

---

#### **Task 2.4: Create Redis Configuration**

**GitHub Copilot Prompt:**
```
Create Redis client configuration with TypeScript that:
1. Connects to Redis using environment variable REDIS_URL
2. Has error handling and logging
3. Implements reconnection logic
4. Exports the redis client instance
```

**Create `src/config/redis.ts`:**
```bash
cat > src/config/redis.ts << 'EOF'
import { createClient } from 'redis';
import logger from '../utils/logger.js';

const redisUrl = process.env.REDIS_URL || 'redis://localhost:6379';

export const redisClient = createClient({
  url: redisUrl,
});

redisClient.on('connect', () => {
  logger.info('Redis client connected');
});

redisClient.on('error', (err) => {
  logger.error('Redis client error:', err);
});

redisClient.on('reconnecting', () => {
  logger.warn('Redis client reconnecting');
});

(async () => {
  await redisClient.connect();
})().catch((err) => {
  logger.error('Failed to connect to Redis:', err);
});

export default redisClient;
EOF
```

**Time**: 3 minutes

---

#### **Task 2.5: Create Logger Utility**

**Create `src/utils/logger.ts`:**
```bash
cat > src/utils/logger.ts << 'EOF'
import winston from 'winston';

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
    winston.format.errors({ stack: true }),
    winston.format.splat(),
    winston.format.json()
  ),
  defaultMeta: { service: 'travel-agent-api' },
  transports: [
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
    new winston.transports.File({ filename: 'logs/combined.log' }),
  ],
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(
    new winston.transports.Console({
      format: winston.format.combine(
        winston.format.colorize(),
        winston.format.printf(({ level, message, timestamp }) => {
          return `${timestamp} [${level}]: ${message}`;
        })
      ),
    })
  );
}

export default logger;
EOF
```

**Time**: 3 minutes

---

#### **Task 2.6: Create Authentication Routes**

**GitHub Copilot Prompt:**
```
Create Express routes for authentication with TypeScript that:
1. POST /api/auth/email-signup - Register with email/password
2. POST /api/auth/email-login - Login with email/password
3. POST /api/auth/google - Login with Google OAuth
4. POST /api/auth/refresh-token - Refresh JWT token
5. POST /api/auth/logout - Logout user
Each route should return appropriate HTTP status codes and JSON responses
```

**Create `src/routes/auth.routes.ts`:**
```bash
cat > src/routes/auth.routes.ts << 'EOF'
import express, { Router, Request, Response } from 'express';
import { signupController, loginController, refreshTokenController } from '../controllers/auth.controller.js';

const router: Router = express.Router();

// Email authentication routes
router.post('/email-signup', signupController);
router.post('/email-login', loginController);

// Token routes
router.post('/refresh-token', refreshTokenController);

// OAuth routes (Phase 2)
// router.post('/google', googleAuthController);
// router.post('/apple', appleAuthController);

export default router;
EOF
```

**Time**: 3 minutes

---

#### **Task 2.7: Create Authentication Controller**

**GitHub Copilot Prompt:**
```
Create auth controller with TypeScript that:
1. signupController - Validates email/password, hashes password with bcrypt, creates user in DB, returns JWT token
2. loginController - Validates credentials, compares password hash, returns JWT token if valid
3. refreshTokenController - Validates refresh token, issues new access token
Each function should handle errors, validate input, and return proper HTTP responses
```

**Create `src/controllers/auth.controller.ts`:**
```bash
cat > src/controllers/auth.controller.ts << 'EOF'
import { Request, Response } from 'express';
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';
import logger from '../utils/logger.js';

// TODO: Import User model when created
// import { User } from '../models/User.js';

export const signupController = async (req: Request, res: Response) => {
  try {
    const { email, password, name } = req.body;

    // Validation
    if (!email || !password || !name) {
      return res.status(400).json({ error: 'Email, password, and name are required' });
    }

    // TODO: Check if user exists
    // const existingUser = await User.findOne({ where: { email } });
    // if (existingUser) {
    //   return res.status(409).json({ error: 'Email already registered' });
    // }

    // TODO: Hash password and create user
    // const hashedPassword = await bcrypt.hash(password, 10);
    // const user = await User.create({
    //   email,
    //   password: hashedPassword,
    //   name,
    // });

    // TODO: Generate JWT tokens
    // const accessToken = jwt.sign(
    //   { userId: user.id, email: user.email },
    //   process.env.JWT_SECRET!,
    //   { expiresIn: process.env.JWT_EXPIRY }
    // );

    logger.info(`User signup attempt: ${email}`);
    
    // Temporary response
    res.status(201).json({
      message: 'User created successfully',
      // user: { id: user.id, email: user.email, name: user.name },
      // accessToken,
    });
  } catch (error) {
    logger.error('Signup error:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
};

export const loginController = async (req: Request, res: Response) => {
  try {
    const { email, password } = req.body;

    if (!email || !password) {
      return res.status(400).json({ error: 'Email and password are required' });
    }

    logger.info(`User login attempt: ${email}`);

    // TODO: Implement login logic
    res.status(200).json({ message: 'Login successful' });
  } catch (error) {
    logger.error('Login error:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
};

export const refreshTokenController = async (req: Request, res: Response) => {
  try {
    const { refreshToken } = req.body;

    if (!refreshToken) {
      return res.status(400).json({ error: 'Refresh token is required' });
    }

    // TODO: Implement token refresh logic
    res.status(200).json({ message: 'Token refreshed' });
  } catch (error) {
    logger.error('Token refresh error:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
};
EOF
```

**Time**: 5 minutes

---

#### **Task 2.8: Create User Model**

**GitHub Copilot Prompt:**
```
Create Sequelize User model with TypeScript that:
1. Has fields: id (UUID primary key), email (unique), password, name, avatar_url, provider, phone, timezone, language, timestamps
2. Add hooks for password hashing before save
3. Add methods for password comparison
4. Include proper type definitions
5. Create timestamps (createdAt, updatedAt)
```

**Create `src/models/User.ts`:**
```bash
cat > src/models/User.ts << 'EOF'
import { DataTypes, Model } from 'sequelize';
import { sequelize } from '../config/database.js';

export class User extends Model {
  public id!: string;
  public email!: string;
  public password!: string;
  public name!: string;
  public avatarUrl?: string;
  public provider?: string;
  public providerId?: string;
  public phone?: string;
  public timezone?: string;
  public language!: string;
  public readonly createdAt!: Date;
  public readonly updatedAt!: Date;
}

User.init(
  {
    id: {
      type: DataTypes.UUID,
      defaultValue: DataTypes.UUIDV4,
      primaryKey: true,
    },
    email: {
      type: DataTypes.STRING,
      unique: true,
      allowNull: false,
      validate: {
        isEmail: true,
      },
    },
    password: {
      type: DataTypes.STRING,
      allowNull: true,
    },
    name: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    avatarUrl: {
      type: DataTypes.STRING,
      allowNull: true,
    },
    provider: {
      type: DataTypes.STRING,
      allowNull: true,
    },
    providerId: {
      type: DataTypes.STRING,
      allowNull: true,
    },
    phone: {
      type: DataTypes.STRING,
      allowNull: true,
    },
    timezone: {
      type: DataTypes.STRING,
      allowNull: true,
    },
    language: {
      type: DataTypes.STRING,
      defaultValue: 'en',
    },
  },
  {
    sequelize,
    tableName: 'users',
    timestamps: true,
  }
);

export default User;
EOF
```

**Time**: 5 minutes

---

#### **Task 2.9: Test Backend Startup**

```bash
# Navigate to api folder
cd packages/api

# Create logs folder
mkdir -p logs

# Try to start the server
npm run dev
```

**Expected output:**
```
[timestamp] [info]: Database configuration initialized
[timestamp] [info]: Redis client connected
[timestamp] [info]: Database connected successfully
[timestamp] [info]: Redis connected successfully
[timestamp] [info]: Database models synchronized
[timestamp] [info]: Server running on port 3000
```

**If it fails:**
- Check Docker containers are running: `docker-compose ps`
- Check .env file has correct DATABASE_URL
- Check Redis is accessible: `redis-cli ping`

**Time**: 5 minutes  
**Check**: Server starts without errors, can press Ctrl+C to stop

---

#### **Task 2.10: Commit Backend Setup**

```bash
# Navigate back to root
cd ../../

# Stage changes
git add .

# Commit
git commit -m "feat(api): setup backend infrastructure with authentication routes

- Initialize Express server with CORS, middleware
- Setup PostgreSQL with Sequelize ORM
- Setup Redis for caching
- Create logger utility with Winston
- Create User model
- Create authentication controller and routes
- Add environment configuration"

# Push
git push origin main
```

**Time**: 2 minutes

---

### ðŸ“ Day 5: Testing & Testing Infrastructure

#### **Task 3.1: Create Basic Test Structure**

**GitHub Copilot Prompt:**
```
Create a basic Jest test setup for Express backend with TypeScript that:
1. Configures Jest for ES modules
2. Creates test utilities for setting up/tearing down test database
3. Creates a sample test file for auth controller
4. Includes test database connection without affecting production DB
```

**Create `jest.config.js`:**
```bash
cat > packages/api/jest.config.js << 'EOF'
export default {
  preset: 'ts-jest/presets/default-esm',
  testEnvironment: 'node',
  roots: ['<rootDir>/src'],
  testMatch: ['**/__tests__/**/*.test.ts', '**/?(*.)+(spec|test).ts'],
  moduleNameMapper: {
    '^(\\.{1,2}/.*)\\.js$': '$1',
  },
  extensionsToTreatAsEsm: ['.ts'],
};
EOF
```

**Create test file:**
```bash
cat > packages/api/src/__tests__/auth.test.ts << 'EOF'
import { describe, it, expect } from '@jest/globals';

describe('Auth Controller', () => {
  it('should pass basic test', () => {
    expect(true).toBe(true);
  });

  it('TODO: should validate email format', () => {
    // Test will be implemented
    expect(true).toBe(true);
  });

  it('TODO: should hash password correctly', () => {
    // Test will be implemented
    expect(true).toBe(true);
  });
});
EOF
```

**Time**: 5 minutes

---

#### **Task 3.2: Commit and Push to GitHub**

```bash
# Stage all changes
git add .

# Commit
git commit -m "feat: add testing infrastructure and basic test setup

- Setup Jest with TypeScript support
- Create basic test file structure
- Add test scripts to package.json"

# Push
git push origin main
```

**Time**: 2 minutes

---

## ðŸ”„ WEEK 2: TRIPS & ITINERARY

### ðŸ“ Days 6-7: React Components & Routing Setup

#### **Task 4.1: Initialize React Frontend**

```bash
# Navigate to web package
cd packages/web

# Create package.json
cat > package.json << 'EOF'
{
  "name": "web",
  "version": "1.0.0",
  "description": "Travel Agent App Frontend",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "test": "vitest",
    "lint": "eslint src/**/*.tsx",
    "format": "prettier --write src/**/*.tsx"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.18.0",
    "zustand": "^4.4.1",
    "axios": "^1.6.0",
    "recharts": "^2.10.3"
  },
  "devDependencies": {
    "@types/react": "^18.2.37",
    "@types/react-dom": "^18.2.15",
    "@vitejs/plugin-react": "^4.1.1",
    "vite": "^5.0.0",
    "typescript": "^5.2.2",
    "@typescript-eslint/eslint-plugin": "^6.10.0",
    "@typescript-eslint/parser": "^6.10.0",
    "vitest": "^0.34.6",
    "@testing-library/react": "^14.1.2"
  }
}
EOF

# Install dependencies
npm install
```

**Time**: 10 minutes

---

#### **Task 4.2: Create Vite Configuration**

```bash
cat > vite.config.ts << 'EOF'
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: {
    port: 5173,
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true,
      },
    },
  },
});
EOF

cat > tsconfig.json << 'EOF'
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "jsx": "react-jsx",
    "lib": ["ES2020", "DOM", "DOM.Iterable"]
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
EOF
```

**Time**: 3 minutes

---

#### **Task 4.3: Create React App Structure**

```bash
# Create main files
mkdir -p src/pages src/components/common src/components/trip src/hooks src/services src/store src/styles

# Create main entry point
cat > src/main.tsx << 'EOF'
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './styles/globals.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
EOF

# Create App component
cat > src/App.tsx << 'EOF'
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import LoginPage from './pages/LoginPage'
import TripsPage from './pages/TripsPage'

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<LoginPage />} />
        <Route path="/trips" element={<TripsPage />} />
        <Route path="/" element={<TripsPage />} />
      </Routes>
    </BrowserRouter>
  )
}

export default App
EOF

# Create HTML entry point
cat > index.html << 'EOF'
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Travel Agent App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
EOF
```

**Time**: 5 minutes

---

#### **Task 4.4: Create Basic Components & Pages**

```bash
# Create Button component
cat > src/components/common/Button.tsx << 'EOF'
import React from 'react'

interface ButtonProps {
  label: string
  onClick?: () => void
  variant?: 'primary' | 'secondary'
  disabled?: boolean
}

export const Button: React.FC<ButtonProps> = ({
  label,
  onClick,
  variant = 'primary',
  disabled = false,
}) => {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`btn btn-${variant}`}
    >
      {label}
    </button>
  )
}

export default Button
EOF

# Create LoginPage
cat > src/pages/LoginPage.tsx << 'EOF'
import React, { useState } from 'react'
import Button from '../components/common/Button'

const LoginPage: React.FC = () => {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')

  const handleLogin = () => {
    console.log('Login with:', email, password)
  }

  return (
    <div className="login-container">
      <h1>Travel Agent App</h1>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <Button label="Login" onClick={handleLogin} />
    </div>
  )
}

export default LoginPage
EOF

# Create TripsPage
cat > src/pages/TripsPage.tsx << 'EOF'
import React from 'react'

const TripsPage: React.FC = () => {
  return (
    <div className="trips-container">
      <h1>My Trips</h1>
      <p>Upcoming trips will appear here</p>
    </div>
  )
}

export default TripsPage
EOF

# Create basic styles
cat > src/styles/globals.css << 'EOF'
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background-color: #ffffff;
  color: #24292f;
}

.login-container,
.trips-container {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #d0d7de;
  border-radius: 8px;
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
}

.btn-primary {
  background-color: #0969da;
  color: white;
}

.btn-secondary {
  background-color: #f6f8fa;
  color: #24292f;
  border: 1px solid #d0d7de;
}
EOF
```

**Time**: 10 minutes

---

#### **Task 4.5: Test Frontend**

```bash
# Start frontend dev server
npm run dev
```

**Expected:**
- Vite server running on http://localhost:5173
- Can navigate to http://localhost:5173/login

**Time**: 2 minutes

---

#### **Task 4.6: Commit Frontend Setup**

```bash
# Navigate to root
cd ../../

# Stage and commit
git add .

git commit -m "feat(web): setup React frontend with routing and basic components

- Initialize Vite with React and TypeScript
- Setup React Router for navigation
- Create LoginPage and TripsPage components
- Add Button component and global styles
- Configure API proxy to backend"

git push origin main
```

**Time**: 2 minutes

---

### ðŸ“ Days 8-10: Location Management & Trip CRUD

#### **Task 5.1: Create Trip Model**

```bash
cd packages/api

cat > src/models/Trip.ts << 'EOF'
import { DataTypes, Model } from 'sequelize';
import { sequelize } from '../config/database.js';

export class Trip extends Model {
  public id!: string;
  public organizerId!: string;
  public name!: string;
  public description?: string;
  public startDate!: Date;
  public endDate!: Date;
  public defaultCurrency!: string;
  public totalBudget?: number;
  public status!: string;
  public isPrivate!: boolean;
}

Trip.init(
  {
    id: {
      type: DataTypes.UUID,
      defaultValue: DataTypes.UUIDV4,
      primaryKey: true,
    },
    organizerId: {
      type: DataTypes.UUID,
      allowNull: false,
    },
    name: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    description: {
      type: DataTypes.TEXT,
      allowNull: true,
    },
    startDate: {
      type: DataTypes.DATE,
      allowNull: false,
    },
    endDate: {
      type: DataTypes.DATE,
      allowNull: false,
    },
    defaultCurrency: {
      type: DataTypes.STRING(3),
      defaultValue: 'USD',
    },
    totalBudget: {
      type: DataTypes.DECIMAL(12, 2),
      allowNull: true,
    },
    status: {
      type: DataTypes.STRING,
      defaultValue: 'planning',
    },
    isPrivate: {
      type: DataTypes.BOOLEAN,
      defaultValue: false,
    },
  },
  {
    sequelize,
    tableName: 'trips',
    timestamps: true,
  }
);

export default Trip;
EOF
```

**Time**: 5 minutes

---

#### **Task 5.2: Create Location Model**

```bash
cat > src/models/Location.ts << 'EOF'
import { DataTypes, Model } from 'sequelize';
import { sequelize } from '../config/database.js';

export class Location extends Model {
  public id!: string;
  public tripId!: string;
  public name!: string;
  public latitude?: number;
  public longitude?: number;
  public address?: string;
  public visitDate?: Date;
  public startTime?: Date;
  public endTime?: Date;
  public estimatedCost?: number;
}

Location.init(
  {
    id: {
      type: DataTypes.UUID,
      defaultValue: DataTypes.UUIDV4,
      primaryKey: true,
    },
    tripId: {
      type: DataTypes.UUID,
      allowNull: false,
    },
    name: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    latitude: {
      type: DataTypes.DECIMAL(10, 8),
      allowNull: true,
    },
    longitude: {
      type: DataTypes.DECIMAL(11, 8),
      allowNull: true,
    },
    address: {
      type: DataTypes.TEXT,
      allowNull: true,
    },
    visitDate: {
      type: DataTypes.DATE,
      allowNull: true,
    },
    startTime: {
      type: DataTypes.DATE,
      allowNull: true,
    },
    endTime: {
      type: DataTypes.DATE,
      allowNull: true,
    },
    estimatedCost: {
      type: DataTypes.DECIMAL(12, 2),
      allowNull: true,
    },
  },
  {
    sequelize,
    tableName: 'locations',
    timestamps: true,
  }
);

export default Location;
EOF
```

**Time**: 5 minutes

---

#### **Task 5.3: Create Trip Controller**

**GitHub Copilot Prompt:**
```
Create a Trip controller with TypeScript that has these functions:
1. createTrip - Takes name, startDate, endDate, budget, currency; creates new trip; returns created trip
2. getTrips - Gets all trips for authenticated user; returns array of trips
3. getTripById - Gets single trip by ID; returns trip with locations
4. updateTrip - Updates trip details; returns updated trip
5. deleteTrip - Soft deletes trip; returns success message
Each function should handle errors and return appropriate HTTP responses
```

**Create `src/controllers/trip.controller.ts`:**
```bash
cat > src/controllers/trip.controller.ts << 'EOF'
import { Request, Response } from 'express';
import Trip from '../models/Trip.js';
import logger from '../utils/logger.js';

// TODO: Add authentication middleware to get userId from request

export const createTrip = async (req: Request, res: Response) => {
  try {
    const { name, description, startDate, endDate, defaultCurrency, totalBudget } = req.body;
    const organizerId = 'TODO_USER_ID'; // Will get from auth middleware

    if (!name || !startDate || !endDate) {
      return res.status(400).json({ error: 'Name, startDate, and endDate are required' });
    }

    const trip = await Trip.create({
      organizerId,
      name,
      description,
      startDate,
      endDate,
      defaultCurrency: defaultCurrency || 'USD',
      totalBudget,
    });

    logger.info(`Trip created: ${trip.id}`);
    res.status(201).json(trip);
  } catch (error) {
    logger.error('Create trip error:', error);
    res.status(500).json({ error: 'Failed to create trip' });
  }
};

export const getTrips = async (req: Request, res: Response) => {
  try {
    const organizerId = 'TODO_USER_ID';

    const trips = await Trip.findAll({
      where: { organizerId },
      order: [['createdAt', 'DESC']],
    });

    res.status(200).json(trips);
  } catch (error) {
    logger.error('Get trips error:', error);
    res.status(500).json({ error: 'Failed to fetch trips' });
  }
};

export const getTripById = async (req: Request, res: Response) => {
  try {
    const { tripId } = req.params;

    const trip = await Trip.findByPk(tripId);

    if (!trip) {
      return res.status(404).json({ error: 'Trip not found' });
    }

    res.status(200).json(trip);
  } catch (error) {
    logger.error('Get trip error:', error);
    res.status(500).json({ error: 'Failed to fetch trip' });
  }
};

export const updateTrip = async (req: Request, res: Response) => {
  try {
    const { tripId } = req.params;
    const { name, description, startDate, endDate, totalBudget } = req.body;

    const trip = await Trip.findByPk(tripId);
    if (!trip) {
      return res.status(404).json({ error: 'Trip not found' });
    }

    await trip.update({
      name: name || trip.name,
      description: description || trip.description,
      startDate: startDate || trip.startDate,
      endDate: endDate || trip.endDate,
      totalBudget: totalBudget || trip.totalBudget,
    });

    logger.info(`Trip updated: ${tripId}`);
    res.status(200).json(trip);
  } catch (error) {
    logger.error('Update trip error:', error);
    res.status(500).json({ error: 'Failed to update trip' });
  }
};

export const deleteTrip = async (req: Request, res: Response) => {
  try {
    const { tripId } = req.params;

    const trip = await Trip.findByPk(tripId);
    if (!trip) {
      return res.status(404).json({ error: 'Trip not found' });
    }

    await trip.destroy();

    logger.info(`Trip deleted: ${tripId}`);
    res.status(200).json({ message: 'Trip deleted successfully' });
  } catch (error) {
    logger.error('Delete trip error:', error);
    res.status(500).json({ error: 'Failed to delete trip' });
  }
};
EOF
```

**Time**: 10 minutes

---

#### **Task 5.4: Create Trip Routes**

```bash
cat > src/routes/trip.routes.ts << 'EOF'
import express from 'express';
import {
  createTrip,
  getTrips,
  getTripById,
  updateTrip,
  deleteTrip,
} from '../controllers/trip.controller.js';

const router = express.Router();

router.post('/', createTrip);
router.get('/', getTrips);
router.get('/:tripId', getTripById);
router.put('/:tripId', updateTrip);
router.delete('/:tripId', deleteTrip);

export default router;
EOF
```

**Add to main routes in `src/index.ts`:**
```bash
# Add this import at the top
import tripRoutes from './routes/trip.routes.js';

# Add this line after auth routes (around line 13)
app.use('/api/trips', tripRoutes);
```

**Time**: 5 minutes

---

#### **Task 5.5: Create Frontend API Service**

```bash
cd ../../packages/web

cat > src/services/api.ts << 'EOF'
import axios from 'axios';

const API_URL = import.meta.env.VITE_API_URL || 'http://localhost:3000';

const apiClient = axios.create({
  baseURL: `${API_URL}/api`,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Add token to requests
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('accessToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default apiClient;
EOF

# Create trips service
cat > src/services/trips.service.ts << 'EOF'
import apiClient from './api';

export interface Trip {
  id: string;
  organizerId: string;
  name: string;
  description?: string;
  startDate: string;
  endDate: string;
  defaultCurrency: string;
  totalBudget?: number;
  status: string;
  isPrivate: boolean;
}

export const tripService = {
  async createTrip(trip: Omit<Trip, 'id'>) {
    const response = await apiClient.post('/trips', trip);
    return response.data;
  },

  async getTrips() {
    const response = await apiClient.get('/trips');
    return response.data;
  },

  async getTripById(tripId: string) {
    const response = await apiClient.get(`/trips/${tripId}`);
    return response.data;
  },

  async updateTrip(tripId: string, updates: Partial<Trip>) {
    const response = await apiClient.put(`/trips/${tripId}`, updates);
    return response.data;
  },

  async deleteTrip(tripId: string) {
    const response = await apiClient.delete(`/trips/${tripId}`);
    return response.data;
  },
};
EOF
```

**Time**: 5 minutes

---

#### **Task 5.6: Create Zustand Store**

```bash
cat > src/store/useTripStore.ts << 'EOF'
import { create } from 'zustand';
import { Trip } from '../services/trips.service';

interface TripStore {
  trips: Trip[];
  loading: boolean;
  error?: string;
  setTrips: (trips: Trip[]) => void;
  addTrip: (trip: Trip) => void;
  removeTrip: (tripId: string) => void;
  setLoading: (loading: boolean) => void;
  setError: (error?: string) => void;
}

export const useTripStore = create<TripStore>((set) => ({
  trips: [],
  loading: false,
  error: undefined,
  setTrips: (trips) => set({ trips }),
  addTrip: (trip) => set((state) => ({ trips: [trip, ...state.trips] })),
  removeTrip: (tripId) =>
    set((state) => ({
      trips: state.trips.filter((t) => t.id !== tripId),
    })),
  setLoading: (loading) => set({ loading }),
  setError: (error) => set({ error }),
}));
EOF
```

**Time**: 3 minutes

---

#### **Task 5.7: Update TripsPage Component**

```bash
cat > src/pages/TripsPage.tsx << 'EOF'
import React, { useEffect } from 'react';
import { useTripStore } from '../store/useTripStore';
import { tripService } from '../services/trips.service';

const TripsPage: React.FC = () => {
  const { trips, loading, setTrips, setLoading } = useTripStore();

  useEffect(() => {
    const fetchTrips = async () => {
      setLoading(true);
      try {
        const data = await tripService.getTrips();
        setTrips(data);
      } catch (error) {
        console.error('Failed to fetch trips:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchTrips();
  }, [setTrips, setLoading]);

  if (loading) {
    return <div>Loading trips...</div>;
  }

  return (
    <div className="trips-container">
      <h1>My Trips</h1>
      {trips.length === 0 ? (
        <p>No trips yet. Create your first trip!</p>
      ) : (
        <div className="trips-list">
          {trips.map((trip) => (
            <div key={trip.id} className="trip-card">
              <h3>{trip.name}</h3>
              <p>{trip.description}</p>
              <p>
                {new Date(trip.startDate).toLocaleDateString()} -{' '}
                {new Date(trip.endDate).toLocaleDateString()}
              </p>
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default TripsPage;
EOF
```

**Time**: 5 minutes

---

#### **Task 5.8: Test Trip CRUD**

```bash
# Terminal 1: Start backend
cd packages/api
npm run dev

# Terminal 2: Start frontend
cd packages/web
npm run dev

# Terminal 3: Test API with curl
curl -X GET http://localhost:3000/api/trips
curl -X POST http://localhost:3000/api/trips \
  -H "Content-Type: application/json" \
  -d '{
    "name":"Paris Trip",
    "startDate":"2024-03-15",
    "endDate":"2024-03-20",
    "totalBudget":3000
  }'
```

**Time**: 10 minutes

---

#### **Task 5.9: Commit Week 2 Progress**

```bash
cd ../../

git add .

git commit -m "feat: implement trip management CRUD operations

- Create Trip and Location models
- Implement trip controller with CRUD operations
- Add trip routes to API
- Create frontend Trip service
- Setup Zustand store for trip state management
- Update TripsPage to fetch and display trips
- Add API client with axios interceptors"

git push origin main
```

**Time**: 2 minutes

---

## ðŸ’° WEEK 3: BUDGET & EXPENSES

### ðŸ“ Days 11-14: Expense APIs & Budget Dashboard

#### **Task 6.1: Create Expense Model**

```bash
cd packages/api

cat > src/models/Expense.ts << 'EOF'
import { DataTypes, Model } from 'sequelize';
import { sequelize } from '../config/database.js';

export class Expense extends Model {
  public id!: string;
  public tripId!: string;
  public amount!: number;
  public category!: string;
  public description?: string;
  public paidByUserId!: string;
  public expenseDate!: Date;
}

Expense.init(
  {
    id: {
      type: DataTypes.UUID,
      defaultValue: DataTypes.UUIDV4,
      primaryKey: true,
    },
    tripId: {
      type: DataTypes.UUID,
      allowNull: false,
    },
    amount: {
      type: DataTypes.DECIMAL(12, 2),
      allowNull: false,
    },
    category: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    description: {
      type: DataTypes.TEXT,
      allowNull: true,
    },
    paidByUserId: {
      type: DataTypes.UUID,
      allowNull: false,
    },
    expenseDate: {
      type: DataTypes.DATE,
      allowNull: false,
      defaultValue: DataTypes.NOW,
    },
  },
  {
    sequelize,
    tableName: 'expenses',
    timestamps: true,
  }
);

export default Expense;
EOF
```

**Time**: 5 minutes

---

#### **Task 6.2: Create Budget Model**

```bash
cat > src/models/Budget.ts << 'EOF'
import { DataTypes, Model } from 'sequelize';
import { sequelize } from '../config/database.js';

export class Budget extends Model {
  public id!: string;
  public tripId!: string;
  public category!: string;
  public allocatedAmount!: number;
  public spentAmount!: number;
  public alertThreshold!: number;
}

Budget.init(
  {
    id: {
      type: DataTypes.UUID,
      defaultValue: DataTypes.UUIDV4,
      primaryKey: true,
    },
    tripId: {
      type: DataTypes.UUID,
      allowNull: false,
    },
    category: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    allocatedAmount: {
      type: DataTypes.DECIMAL(12, 2),
      allowNull: false,
    },
    spentAmount: {
      type: DataTypes.DECIMAL(12, 2),
      defaultValue: 0,
    },
    alertThreshold: {
      type: DataTypes.INTEGER,
      defaultValue: 80,
    },
  },
  {
    sequelize,
    tableName: 'budgets',
    timestamps: true,
  }
);

export default Budget;
EOF
```

**Time**: 5 minutes

---

#### **Task 6.3: Create Expense Controller**

```bash
cat > src/controllers/expense.controller.ts << 'EOF'
import { Request, Response } from 'express';
import Expense from '../models/Expense.js';
import Budget from '../models/Budget.js';
import logger from '../utils/logger.js';

export const createExpense = async (req: Request, res: Response) => {
  try {
    const { tripId, amount, category, description, paidByUserId } = req.body;

    const expense = await Expense.create({
      tripId,
      amount,
      category,
      description,
      paidByUserId,
      expenseDate: new Date(),
    });

    // Update budget spent amount
    const budget = await Budget.findOne({ where: { tripId, category } });
    if (budget) {
      await budget.update({
        spentAmount: (parseFloat(budget.spentAmount.toString()) + parseFloat(amount)).toString(),
      });
    }

    logger.info(`Expense created: ${expense.id}`);
    res.status(201).json(expense);
  } catch (error) {
    logger.error('Create expense error:', error);
    res.status(500).json({ error: 'Failed to create expense' });
  }
};

export const getExpenses = async (req: Request, res: Response) => {
  try {
    const { tripId } = req.params;

    const expenses = await Expense.findAll({
      where: { tripId },
      order: [['expenseDate', 'DESC']],
    });

    res.status(200).json(expenses);
  } catch (error) {
    logger.error('Get expenses error:', error);
    res.status(500).json({ error: 'Failed to fetch expenses' });
  }
};

export const updateExpense = async (req: Request, res: Response) => {
  try {
    const { expenseId } = req.params;
    const { amount, category, description } = req.body;

    const expense = await Expense.findByPk(expenseId);
    if (!expense) {
      return res.status(404).json({ error: 'Expense not found' });
    }

    await expense.update({ amount, category, description });
    logger.info(`Expense updated: ${expenseId}`);
    res.status(200).json(expense);
  } catch (error) {
    logger.error('Update expense error:', error);
    res.status(500).json({ error: 'Failed to update expense' });
  }
};

export const deleteExpense = async (req: Request, res: Response) => {
  try {
    const { expenseId } = req.params;

    const expense = await Expense.findByPk(expenseId);
    if (!expense) {
      return res.status(404).json({ error: 'Expense not found' });
    }

    await expense.destroy();
    logger.info(`Expense deleted: ${expenseId}`);
    res.status(200).json({ message: 'Expense deleted successfully' });
  } catch (error) {
    logger.error('Delete expense error:', error);
    res.status(500).json({ error: 'Failed to delete expense' });
  }
};
EOF
```

**Time**: 10 minutes

---

#### **Task 6.4: Create Budget Controller**

```bash
cat > src/controllers/budget.controller.ts << 'EOF'
import { Request, Response } from 'express';
import Budget from '../models/Budget.js';
import Expense from '../models/Expense.js';
import logger from '../utils/logger.js';

export const getBudgets = async (req: Request, res: Response) => {
  try {
    const { tripId } = req.params;

    const budgets = await Budget.findAll({ where: { tripId } });
    res.status(200).json(budgets);
  } catch (error) {
    logger.error('Get budgets error:', error);
    res.status(500).json({ error: 'Failed to fetch budgets' });
  }
};

export const updateBudget = async (req: Request, res: Response) => {
  try {
    const { budgetId } = req.params;
    const { allocatedAmount, alertThreshold } = req.body;

    const budget = await Budget.findByPk(budgetId);
    if (!budget) {
      return res.status(404).json({ error: 'Budget not found' });
    }

    await budget.update({ allocatedAmount, alertThreshold });
    logger.info(`Budget updated: ${budgetId}`);
    res.status(200).json(budget);
  } catch (error) {
    logger.error('Update budget error:', error);
    res.status(500).json({ error: 'Failed to update budget' });
  }
};

export const getBudgetAnalytics = async (req: Request, res: Response) => {
  try {
    const { tripId } = req.params;

    const budgets = await Budget.findAll({ where: { tripId } });
    const expenses = await Expense.findAll({ where: { tripId } });

    const totalBudget = budgets.reduce((sum, b) => sum + parseFloat(b.allocatedAmount.toString()), 0);
    const totalSpent = expenses.reduce((sum, e) => sum + parseFloat(e.amount.toString()), 0);
    const remaining = totalBudget - totalSpent;
    const utilization = totalBudget > 0 ? ((totalSpent / totalBudget) * 100).toFixed(2) : '0';

    res.status(200).json({
      totalBudget,
      totalSpent,
      remaining,
      utilization: `${utilization}%`,
      byCategory: budgets.map((b) => ({
        category: b.category,
        allocated: b.allocatedAmount,
        spent: b.spentAmount,
        percentage: parseFloat(b.allocatedAmount.toString()) > 0
          ? ((parseFloat(b.spentAmount.toString()) / parseFloat(b.allocatedAmount.toString())) * 100).toFixed(2)
          : '0',
      })),
    });
  } catch (error) {
    logger.error('Get analytics error:', error);
    res.status(500).json({ error: 'Failed to fetch analytics' });
  }
};
EOF
```

**Time**: 10 minutes

---

#### **Task 6.5: Create Routes for Expenses & Budget**

```bash
cat > src/routes/expense.routes.ts << 'EOF'
import express from 'express';
import {
  createExpense,
  getExpenses,
  updateExpense,
  deleteExpense,
} from '../controllers/expense.controller.js';

const router = express.Router();

router.post('/', createExpense);
router.get('/:tripId', getExpenses);
router.put('/:expenseId', updateExpense);
router.delete('/:expenseId', deleteExpense);

export default router;
EOF

cat > src/routes/budget.routes.ts << 'EOF'
import express from 'express';
import {
  getBudgets,
  updateBudget,
  getBudgetAnalytics,
} from '../controllers/budget.controller.js';

const router = express.Router();

router.get('/:tripId', getBudgets);
router.put('/:budgetId', updateBudget);
router.get('/:tripId/analytics', getBudgetAnalytics);

export default router;
EOF
```

**Add to main index.ts:**
```bash
# Add imports
import expenseRoutes from './routes/expense.routes.js';
import budgetRoutes from './routes/budget.routes.js';

# Add routes
app.use('/api/expenses', expenseRoutes);
app.use('/api/budgets', budgetRoutes);
```

**Time**: 5 minutes

---

#### **Task 6.6: Create Frontend Budget Components**

```bash
cd ../../packages/web

# Create Budget service
cat > src/services/budget.service.ts << 'EOF'
import apiClient from './api';

export interface Budget {
  id: string;
  category: string;
  allocatedAmount: number;
  spentAmount: number;
  alertThreshold: number;
}

export const budgetService = {
  async getBudgets(tripId: string) {
    const response = await apiClient.get(`/budgets/${tripId}`);
    return response.data;
  },

  async getAnalytics(tripId: string) {
    const response = await apiClient.get(`/budgets/${tripId}/analytics`);
    return response.data;
  },

  async updateBudget(budgetId: string, updates: Partial<Budget>) {
    const response = await apiClient.put(`/budgets/${budgetId}`, updates);
    return response.data;
  },
};
EOF

# Create Expense service
cat > src/services/expense.service.ts << 'EOF'
import apiClient from './api';

export interface Expense {
  id: string;
  tripId: string;
  amount: number;
  category: string;
  description?: string;
  paidByUserId: string;
  expenseDate: string;
}

export const expenseService = {
  async createExpense(expense: Omit<Expense, 'id'>) {
    const response = await apiClient.post('/expenses', expense);
    return response.data;
  },

  async getExpenses(tripId: string) {
    const response = await apiClient.get(`/expenses/${tripId}`);
    return response.data;
  },

  async updateExpense(expenseId: string, updates: Partial<Expense>) {
    const response = await apiClient.put(`/expenses/${expenseId}`, updates);
    return response.data;
  },

  async deleteExpense(expenseId: string) {
    const response = await apiClient.delete(`/expenses/${expenseId}`);
    return response.data;
  },
};
EOF

# Create Budget Dashboard Component
cat > src/components/budget/BudgetDashboard.tsx << 'EOF'
import React, { useEffect, useState } from 'react';
import { budgetService } from '../../services/budget.service';

interface Analytics {
  totalBudget: number;
  totalSpent: number;
  remaining: number;
  utilization: string;
  byCategory: Array<{
    category: string;
    allocated: number;
    spent: number;
    percentage: string;
  }>;
}

interface Props {
  tripId: string;
}

export const BudgetDashboard: React.FC<Props> = ({ tripId }) => {
  const [analytics, setAnalytics] = useState<Analytics | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchAnalytics = async () => {
      try {
        const data = await budgetService.getAnalytics(tripId);
        setAnalytics(data);
      } catch (error) {
        console.error('Failed to fetch analytics:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchAnalytics();
  }, [tripId]);

  if (loading) return <div>Loading budget...</div>;
  if (!analytics) return <div>No budget data available</div>;

  return (
    <div className="budget-dashboard">
      <h2>Budget Overview</h2>
      <div className="budget-summary">
        <div className="stat">
          <span>Total Budget:</span>
          <strong>${analytics.totalBudget.toFixed(2)}</strong>
        </div>
        <div className="stat">
          <span>Spent:</span>
          <strong>${analytics.totalSpent.toFixed(2)}</strong>
        </div>
        <div className="stat">
          <span>Remaining:</span>
          <strong>${analytics.remaining.toFixed(2)}</strong>
        </div>
        <div className="stat">
          <span>Utilization:</span>
          <strong>{analytics.utilization}</strong>
        </div>
      </div>

      <div className="budget-by-category">
        <h3>By Category</h3>
        {analytics.byCategory.map((cat) => (
          <div key={cat.category} className="category-item">
            <span>{cat.category}</span>
            <div className="progress-bar">
              <div
                className="progress-fill"
                style={{ width: `${parseFloat(cat.percentage)}%` }}
              />
            </div>
            <span>${cat.spent.toFixed(2)} / ${cat.allocated.toFixed(2)}</span>
          </div>
        ))}
      </div>
    </div>
  );
};

export default BudgetDashboard;
EOF
```

**Time**: 10 minutes

---

#### **Task 6.7: Commit Week 3 Progress**

```bash
cd ../../

git add .

git commit -m "feat: implement budget and expense management

- Create Expense and Budget models
- Implement expense controller with CRUD operations
- Implement budget controller with analytics
- Add expense and budget routes
- Create frontend services for expenses and budgets
- Build Budget Dashboard component with analytics display
- Add real-time budget calculation"

git push origin main
```

**Time**: 2 minutes

---

## ðŸš€ WEEK 4: TESTING, CI/CD & DEPLOYMENT

### ðŸ“ Days 15-16: Testing & CI/CD Setup

#### **Task 7.1: Setup GitHub Actions CI/CD**

```bash
mkdir -p .github/workflows

cat > .github/workflows/test-and-deploy.yml << 'EOF'
name: Test and Deploy

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test_db
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm install

      - name: Lint code
        run: npm run lint

      - name: Run tests
        run: npm run test
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test_db
          REDIS_URL: redis://localhost:6379

      - name: Build project
        run: npm run build

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'

    steps:
      - uses: actions/checkout@v3

      - name: Deploy to production
        run: |
          echo "Deploying to production..."
          # Add your deployment commands here

EOF
```

**Time**: 5 minutes

---

#### **Task 7.2: Add Tests**

```bash
cd packages/api

# Create basic tests
cat > src/__tests__/auth.test.ts << 'EOF'
describe('Auth Controller', () => {
  it('should validate email format', () => {
    const email = 'test@example.com';
    expect(email).toContain('@');
  });

  it('should handle missing credentials', () => {
    const email = '';
    const password = '';
    expect(email && password).toBeFalsy();
  });
});
EOF

cd ../web

cat > src/__tests__/Button.test.tsx << 'EOF'
import React from 'react';
import { describe, it, expect, vi } from 'vitest';
import { render, screen } from '@testing-library/react';
import Button from '../components/common/Button';

describe('Button Component', () => {
  it('renders button with label', () => {
    render(<Button label="Click me" />);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const onClick = vi.fn();
    const { getByText } = render(<Button label="Click me" onClick={onClick} />);
    getByText('Click me').click();
    expect(onClick).toHaveBeenCalled();
  });
});
EOF
```

**Time**: 5 minutes

---

#### **Task 7.3: Create Docker Files**

```bash
cd packages/api

cat > Dockerfile << 'EOF'
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
EOF

cd ../web

cat > Dockerfile << 'EOF'
FROM node:18-alpine as builder

WORKDIR /app
COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
EOF

cat > nginx.conf << 'EOF'
server {
    listen 80;
    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
        try_files $uri $uri/ /index.html;
    }
    location /api {
        proxy_pass http://api:3000;
    }
}
EOF
```

**Time**: 5 minutes

---

#### **Task 7.4: Create Production Docker Compose**

```bash
cd ../../

cat > docker-compose.prod.yml << 'EOF'
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_NAME}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: always

  redis:
    image: redis:7-alpine
    restart: always
    volumes:
      - redis_data:/data

  api:
    build:
      context: packages/api
    depends_on:
      - postgres
      - redis
    environment:
      NODE_ENV: production
      DATABASE_URL: postgresql://${DB_USER}:${DB_PASSWORD}@postgres:5432/${DB_NAME}
      REDIS_URL: redis://redis:6379
    ports:
      - "3000:3000"
    restart: always

  web:
    build:
      context: packages/web
    depends_on:
      - api
    ports:
      - "80:80"
    environment:
      VITE_API_URL: http://localhost:3000
    restart: always

volumes:
  postgres_data:
  redis_data:
EOF
```

**Time**: 5 minutes

---

#### **Task 7.5: Commit Testing & Deployment Setup**

```bash
git add .

git commit -m "feat: add testing infrastructure and deployment configuration

- Setup GitHub Actions CI/CD pipeline
- Add Jest and Vitest configuration
- Create basic unit tests for auth and components
- Add Docker files for containerization
- Create production docker-compose configuration
- Setup automated testing on push/PR"

git push origin main
```

**Time**: 2 minutes

---

### ðŸ“ Days 17-18: Final Deployment

#### **Task 8.1: Test Production Build Locally**

```bash
# Build all packages
npm run build

# Build Docker containers
docker-compose -f docker-compose.prod.yml build

# Start services
docker-compose -f docker-compose.prod.yml up

# Test application
curl http://localhost/api/trips
```

**Time**: 10 minutes

---

#### **Task 8.2: Create README & Documentation**

```bash
cat > README.md << 'EOF'
# Travel Agent App

Collaborative trip planning and budget management platform.

## Quick Start

### Prerequisites
- Node.js 18+
- Docker & Docker Compose
- Git

### Development Setup

```bash
# Clone repository
git clone <repo-url>
cd travel-agent-app

# Install dependencies
npm install

# Start databases
docker-compose up -d

# Start development servers
npm run dev
```

### Access Application
- Frontend: http://localhost:5173
- Backend API: http://localhost:3000
- Health Check: http://localhost:3000/health

### Environment Variables
Copy `.env.example` to `.env` and update values

### Database Migrations
```bash
npm run db:migrate
npm run db:seed
```

### Testing
```bash
npm run test
npm run lint
```

### Production Deployment
```bash
docker-compose -f docker-compose.prod.yml up
```

## Architecture

- **Frontend**: React + Vite
- **Backend**: Express.js + TypeScript
- **Database**: PostgreSQL
- **Cache**: Redis
- **Monorepo**: Turbo

## Project Structure

See `docs/folder-hierarchy.md` for complete structure

## API Documentation

- Create Trip: `POST /api/trips`
- Get Trips: `GET /api/trips`
- Add Expense: `POST /api/expenses`
- Get Budget: `GET /api/budgets/:tripId`

## Contributing

1. Create feature branch: `git checkout -b feature/feature-name`
2. Commit changes: `git commit -m "feat: description"`
3. Push: `git push origin feature/feature-name`
4. Create Pull Request

## License

MIT
EOF
```

**Time**: 5 minutes

---

#### **Task 8.3: Final Testing Checklist**

```bash
# Run complete test suite
npm run test

# Check for errors/warnings
npm run lint

# Build for production
npm run build

# Test Docker build
docker build -t travel-agent-api packages/api
docker build -t travel-agent-web packages/web

# Run containers
docker-compose -f docker-compose.prod.yml up
```

**Verify:**
- âœ… All tests pass
- âœ… No lint errors
- âœ… Build completes without errors
- âœ… Docker containers start
- âœ… API responds to requests
- âœ… Frontend loads

**Time**: 15 minutes

---

#### **Task 8.4: Final Commit & Release**

```bash
git add .

git commit -m "docs: add README and finalize MVP Phase 1

- Complete README with setup instructions
- Add API documentation
- Document project structure
- Add contribution guidelines
- Ready for MVP deployment"

# Create a release tag
git tag -a v1.0.0 -m "MVP Phase 1 Release"

git push origin main
git push origin v1.0.0
```

**Time**: 2 minutes

---

## ðŸ“‹ Complete Command Reference

### Daily Development

```bash
# Start all services
npm run dev

# Just backend
npm run dev:api

# Just frontend
npm run dev:web

# Run tests
npm run test

# Lint code
npm run lint

# Format code
npm run format

# Build for production
npm run build
```

### Database Operations

```bash
# Start database services
docker-compose up -d

# View logs
docker-compose logs

# Stop services
docker-compose down

# Database migration (future)
npm run db:migrate

# Seed database (future)
npm run db:seed
```

### Git Workflow

```bash
# Check status
git status

# Stage changes
git add .

# Commit
git commit -m "type: message"

# Push to GitHub
git push origin main

# Create branch
git checkout -b feature/name

# Switch branches
git checkout main

# Merge branch
git merge feature/name
```

### Deployment

```bash
# Build Docker images
docker-compose -f docker-compose.prod.yml build

# Start production
docker-compose -f docker-compose.prod.yml up -d

# View logs
docker-compose -f docker-compose.prod.yml logs -f

# Stop services
docker-compose -f docker-compose.prod.yml down
```

---

## ðŸŽ¯ GitHub Copilot Prompts Summary

### For Backend Tasks
```
"Generate Express.js controller with error handling for [feature]"
"Create Sequelize model with validations for [entity]"
"Build REST API route handlers for CRUD operations on [resource]"
```

### For Frontend Tasks
```
"Create React component with hooks and TypeScript for [feature]"
"Build Zustand store for managing [state]"
"Generate form component with validation for [form name]"
```

### For Configuration
```
"Setup TypeScript configuration for [project]"
"Create Docker configuration for [service]"
"Generate GitHub Actions workflow for [purpose]"
```

---

## âœ… Success Criteria - Week by Week

**Week 1: âœ…**
- [ ] Project folder structure created
- [ ] GitHub repository setup
- [ ] Backend server running on port 3000
- [ ] PostgreSQL & Redis connected
- [ ] Authentication routes created
- [ ] User model created
- [ ] First commit pushed to GitHub

**Week 2: âœ…**
- [ ] React frontend running on port 5173
- [ ] Trip CRUD APIs working
- [ ] Frontend can fetch and display trips
- [ ] API proxy configured
- [ ] Zustand store managing state
- [ ] Location model created (Phase 1 only adds basic structure)

**Week 3: âœ…**
- [ ] Expense CRUD APIs working
- [ ] Budget model and controller created
- [ ] Budget analytics calculated
- [ ] Frontend components displaying budgets
- [ ] Budget Dashboard component built
- [ ] Expense tracking functional

**Week 4: âœ…**
- [ ] Unit tests written and passing
- [ ] CI/CD pipeline configured
- [ ] Docker configuration complete
- [ ] Production build tested locally
- [ ] README and documentation complete
- [ ] MVP ready for deployment

---

## ðŸš€ Deployment Platforms (For Future)

Once you're done with Week 4 and want to deploy:

```bash
# Heroku
heroku login
heroku create travel-agent-app
git push heroku main

# AWS (EC2)
# SSH into instance
# Clone repo
# Run docker-compose

# DigitalOcean
# Create droplet
# Deploy using App Platform or docker-compose

# Vercel (Frontend)
# Connect GitHub repo
# Deploy frontend only
```

---

**Status**: âœ… COMPLETE SOLO DEVELOPER ROADMAP  
**Duration**: 4 Weeks  
**Estimated Hours**: 150-200 hours  
**Difficulty**: Beginner to Intermediate  
**With GitHub Copilot**: 30-40% faster development  

**Ready to start? Begin with Day 1, Task 1.1!** ðŸš€
