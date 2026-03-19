# Monorepo: Frontend, CMS, and Backend

A modern monorepo setup with three integrated applications:

- **Frontend**: Next.js 14 with App Router and Tailwind CSS
- **CMS**: Payload CMS 2.x with MongoDB
- **Backend**: Medusa.js v2 ecommerce platform

## Project Structure   

```
├── frontend/          # Next.js 14 application
├── cms/              # Payload CMS application
├── backend/          # Medusa.js backend
├── package.json      # Root workspace configuration
└── README.md         # This file
```

## Prerequisites

- **Node.js**: v18.0.0 or higher
- **npm**: v9.0.0 or higher
- **Docker** (optional but recommended): For running databases locally
- **Git**: For version control

## Installation

### 1. Clone and Install Dependencies

```bash
# Install root dependencies and all workspace dependencies
npm install

# Or if using npm workspaces
npm install --workspaces
```

### 2. Environment Variables Setup

Each application has its own `.env.local` file. Update them with your configuration:

#### Frontend (`frontend/.env.local`)
```env
NEXT_PUBLIC_API_URL=http://localhost:3001
NEXT_PUBLIC_CMS_URL=http://localhost:3000
```

#### CMS (`cms/.env.local`)
```env
MONGODB_URI=mongodb://localhost:27017/payload
PAYLOAD_SECRET=your-secret-key-change-this-in-production
PORT=3000
NODE_ENV=development
```

#### Backend (`backend/.env.local`)
```env
DATABASE_URL=postgresql://user:password@localhost:5432/medusa_db
REDIS_URL=redis://localhost:6379
JWT_SECRET=your-secret-key-change-this
NODE_ENV=development
PORT=3001
```

### 3. Database Setup

#### MongoDB (for Payload CMS)

**Option A: Using Docker**
```bash
docker run -d --name mongodb -p 27017:27017 mongo:latest
```

**Option B: Local Installation**
- Download and install from [mongodb.com](https://www.mongodb.com/try/download/community)
- Start MongoDB service

#### PostgreSQL (for Medusa.js)

**Option A: Using Docker**
```bash
docker run -d --name postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  postgres:latest
```

**Option B: Local Installation**
- Download and install from [postgresql.org](https://www.postgresql.org/download/)
- Create database: `createdb medusa_db`

#### Redis (for Medusa.js)

**Option A: Using Docker**
```bash
docker run -d --name redis -p 6379:6379 redis:latest
```

**Option B: Local Installation**
- Download from [redis.io](https://redis.io/download)
- Start Redis service

## Development

### Start All Applications

```bash
# Run all applications in development mode
npm run dev

# Runs:
# - Frontend on http://localhost:3000
# - CMS on http://localhost:3000 (admin at /admin)
# - Backend on http://localhost:3001
```

### Start Individual Applications

```bash
# Frontend only
npm -w frontend run dev

# CMS only
npm -w cms run dev

# Backend only
npm -w backend run dev
```

## Application Details

### Frontend (Next.js 14)

**Location**: `frontend/`

**Port**: 3000 (local dev)

**Features**:
- Next.js 14 with App Router
- Tailwind CSS for styling
- TypeScript support
- Path aliases (`@/*` for `./src/*`)

**Commands**:
```bash
npm -w frontend run dev      # Development server
npm -w frontend run build    # Production build
npm -w frontend start        # Start production server
npm -w frontend run lint     # Run ESLint
```

**Key Files**:
- `src/app/page.tsx` - Home page
- `src/app/layout.tsx` - Root layout
- `src/app/globals.css` - Global styles
- `next.config.js` - Next.js configuration

### CMS (Payload CMS 2.x)

**Location**: `cms/`

**Port**: 3000

**Admin Panel**: `http://localhost:3000/admin`

**Features**:
- Payload CMS v2
- MongoDB integration
- Collections: Users, Pages, Media
- Global Settings
- Role-based access control

**Commands**:
```bash
npm -w cms run dev       # Development server
npm -w cms run build     # Build project
npm -w cms start         # Start production server
npm -w cms run seed      # Seed database
```

**Default Admin Credentials**:
- Email: `admin@example.com`
- Password: Set during first launch

**Collections**:
- **Users**: Manage admin and user accounts
- **Pages**: Create and manage content pages
- **Media**: Upload and manage images/videos

**Key Files**:
- `src/payload.config.ts` - Main CMS configuration
- `src/collections/` - Collection definitions
- `src/globals/Settings.ts` - Site settings

### Backend (Medusa.js v2)

**Location**: `backend/`

**Port**: 3001

**Admin Panel**: `http://localhost:7001/admin` (when fully configured)

**Features**:
- Medusa.js v2 ecommerce
- PostgreSQL database
- Redis caching
- Product and Order management
- Custom API routes

**Commands**:
```bash
npm -w backend run dev       # Development server
npm -w backend run build     # Build project
npm -w backend start         # Start production server
npm -w backend run migrate   # Run migrations
```

**API Routes**:
- `GET /health` - Health check
- `POST /products/featured` - Create featured product
- `GET /products/featured` - Get featured products
- `POST /orders` - Create order
- `PATCH /orders/:orderId/status` - Update order status

**Key Files**:
- `medusa-config.ts` - Main configuration
- `src/services/` - Business logic
- `src/api/routes/` - API endpoints
- `src/models/index.ts` - TypeScript types

## Scripts

### Root Package.json Scripts

```bash
npm run dev      # Start all applications in dev mode
npm run build    # Build all applications
npm run lint     # Lint all workspaces
npm run test     # Run tests across workspaces
npm run clean    # Clean all build artifacts and node_modules
```

## API Communication

### Frontend to Backend
```typescript
// In frontend components
const response = await fetch(
  `${process.env.NEXT_PUBLIC_API_URL}/health`
)
```

### Frontend to CMS
```typescript
// Fetch content from CMS
const pages = await fetch(
  `${process.env.NEXT_PUBLIC_CMS_URL}/api/pages`
)
```

## Deployment

### Frontend (Vercel)
```bash
# Push to GitHub, connect Vercel project
# Automatics deployments on main branch
```

### CMS (Railway, Render, or Heroku)
```bash
# Requires MongoDB and Node.js support
npm run build
npm start
```

### Backend (Railway, Render, or Heroku)
```bash
# Requires PostgreSQL, Redis, and Node.js
npm run build
npm run migrate
npm start
```

## Troubleshooting

### Port Already in Use
```bash
# Find and kill process using port 3000
lsof -i :3000
kill -9 <PID>

# Or use different ports in .env files
```

### Database Connection Issues
- Verify MongoDB/PostgreSQL is running
- Check connection strings in `.env.local`
- Ensure ports are accessible (27017 for MongoDB, 5432 for PostgreSQL)

### npm Workspaces Not Working
```bash
# Clear and reinstall
rm -rf node_modules
npm clean-install
```

### Build Errors
```bash
# Clean everything and reinstall
npm run clean
npm install
npm run build
```

## Contributing

1. Create feature branches from `main`
2. Make changes in relevant workspace
3. Run linting: `npm run lint`
4. Test changes locally
5. Create pull request with clear description

## License

MIT

## Support

For issues specific to each application:
- **Next.js**: [nextjs.org/docs](https://nextjs.org/docs)
- **Payload CMS**: [payloadcms.com/docs](https://payloadcms.com/docs)
- **Medusa.js**: [medusajs.com/docs](https://medusajs.com/docs)

---

**Happy coding!** 🚀
