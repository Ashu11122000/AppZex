# AppZex SaaS --- Full Stack Developer Technical Assignment

## Project Overview

AppZex SaaS is a production-minded, multi-tenant agency project
management SaaS application being developed for the AppZex Solutions
Full Stack Developer Technical Assignment.

The application is designed around an agency/project-management workflow
where multiple agencies can use the same SaaS platform while keeping
their data isolated.

### Required technology stack

-   Frontend: Next.js
-   Backend: Node.js
-   Database: MySQL
-   ORM: Prisma
-   Backend framework: Express.js
-   Language: TypeScript

The assignment also requires role-based access, multi-tenant security,
project workflow management, protected files, and at least one
meaningful AI-powered workflow.

---

# Assignment Requirements

The application has four main roles:

1.  Super Admin
2.  Agency Admin
3.  Agency Team
4.  Agency Client

## Super Admin

The Super Admin operates at the platform level and should be able to:

-   Manage agencies
-   Activate/suspend agencies
-   View platform-level metrics
-   Monitor platform activity
-   Support agencies through an appropriate support mode

The Super Admin area is separate from the normal agency workspace.

## Agency Workspace

The agency workspace should support:

-   Agency/team management
-   Client management
-   Project management
-   Milestones
-   Tasks
-   Meetings
-   Activity tracking
-   Feedback/change requests
-   Files

## Client Portal

Agency clients should have a secure portal where they can access only
their authorized company/project information.

The portal should support:

-   Project progress
-   Deadlines
-   Updates
-   Pending actions
-   Feedback/change requests
-   Meeting notes
-   Shared files

## AI Requirement

The assignment requires at least one meaningful AI-powered workflow.

Potential workflows include:

-   AI Project Health
-   AI Meeting Summary
-   AI Client Update
-   AI Feedback Assistant

The AI implementation must be connected to a real application workflow
and should document:

-   Input data
-   Output
-   AI model/provider
-   API key handling
-   Error handling
-   Tenant/project data isolation

---

# Security and Multi-Tenancy Requirements

Security is a major part of this assignment.

The backend must enforce authorization rather than relying only on
frontend restrictions.

Important requirements include:

-   Agency A must not access Agency B data.
-   Client users must only access their authorized data.
-   Direct API/ID manipulation must not bypass authorization.
-   Suspended agencies must be blocked from normal operations.
-   Passwords must be securely hashed.
-   Files must have protected access.
-   Super Admin access must remain separate from normal agency access.
-   Operational records must be scoped to the correct agency/tenant.

The backend will therefore use authentication middleware, role
middleware, tenant middleware, client-access checks, and agency-status
checks.

---

# Initial Project Setup

## Created project directories

The initial project structure was created with:

``` text
AppZex-Saas/
├── backend/
├── frontend/
├── database/
└── docs/
```

The backend is currently the first major implementation area.

---

# Docker MySQL Setup

The assignment requires MySQL, so MySQL was configured through Docker
rather than installing MySQL directly on Windows.

The Docker image used is:

``` text
mysql:8.4
```

The container is:

``` text
appzex-mysql
```

The database is:

``` text
appzex_saas
```

The application database user is:

``` text
appzex_user
```

The host port is:

``` text
3307
```

Docker maps:

``` text
localhost:3307
        ↓
MySQL container:3306
```

---

# Docker Verification

The MySQL container was started and verified.

Important verification:

``` powershell
docker ps
```

The container was confirmed as:

``` text
appzex-mysql
```

with port:

``` text
3307 -> 3306
```

The health status was verified as:

``` text
healthy
```

MySQL version inside the container was verified as:

``` text
8.4.11
```

The application database and user were also verified.

The database list confirmed:

``` text
appzex_saas
information_schema
performance_schema
```

----

# MySQL Git Commit

The Docker MySQL setup was committed:

``` text
6b00f72
Add MySQL docker setup
```

The commit was pushed:

``` powershell
git push
```

---

# Backend Initialization

Entered the backend directory:

``` powershell
cd backend
```

Initialized the Node.js package:

``` powershell
npm init -y
```

This created:

``` text
backend/package.json
```

---

# Backend Production Dependencies

The following packages were installed:

``` powershell
npm install express @prisma/client jsonwebtoken bcryptjs zod helmet cors express-rate-limit dotenv multer morgan
```

Purpose of the main dependencies:

  Package              Purpose
  -------------------- ----------------------------
  express              HTTP/API server
  @prisma/client       Prisma database client
  jsonwebtoken         JWT authentication
  bcryptjs             Password hashing
  zod                  Request/data validation
  helmet               HTTP security headers
  cors                 Cross-origin configuration
  express-rate-limit   API rate limiting
  dotenv               Environment variables
  multer               File uploads
  morgan               HTTP request logging

---

# Prisma MySQL Adapter

Because the project uses Prisma 7 with MySQL, the required adapter and
driver were installed:

``` powershell
npm install @prisma/adapter-mariadb mariadb
```

Installed versions:

``` text
@prisma/adapter-mariadb 7.10.0
mariadb                  3.5.4
```

---

# Prisma Client Generation

Prisma Client was generated using:

``` powershell
npx prisma generate
```

Successful output confirmed:

``` text
Generated Prisma Client (7.10.0) to .\generated\prisma
```

Therefore:

``` text
backend/
└── generated/
    └── prisma/
```

is now generated automatically by Prisma.

---

# Prisma Formatting and Validation

The schema was formatted:

``` powershell
npx prisma format
```

Result:

``` text
Formatted prisma\schema.prisma
```

The schema was validated:

``` powershell
npx prisma validate
```

Result:

``` text
The schema at prisma\schema.prisma is valid
```

At this stage the Prisma foundation is working.

---

# Current Backend Folder Structure

The planned complete backend structure is:

``` text
backend/
│
├── .agents/
├── .claude/
├── .windsurf/
│
├── node_modules/
│
├── prisma/
│   ├── schema.prisma
│   ├── seed.ts
│   └── migrations/
│       └── ...
│
├── generated/
│   └── prisma/
│       └── ...generated Prisma Client
│
├── src/
│   │
│   ├── config/
│   │   ├── env.ts
│   │   ├── database.ts
│   │   ├── cors.ts
│   │   └── ai.ts
│   │
│   ├── controllers/
│   │   ├── auth.controller.ts
│   │   ├── user.controller.ts
│   │   ├── super-admin.controller.ts
│   │   ├── agency.controller.ts
│   │   ├── agency-member.controller.ts
│   │   ├── client.controller.ts
│   │   ├── project.controller.ts
│   │   ├── milestone.controller.ts
│   │   ├── task.controller.ts
│   │   ├── meeting.controller.ts
│   │   ├── feedback.controller.ts
│   │   ├── file.controller.ts
│   │   ├── activity.controller.ts
│   │   ├── dashboard.controller.ts
│   │   └── ai.controller.ts
│   │
│   ├── middlewares/
│   │   ├── auth.middleware.ts
│   │   ├── role.middleware.ts
│   │   ├── tenant.middleware.ts
│   │   ├── client-access.middleware.ts
│   │   ├── agency-status.middleware.ts
│   │   ├── validation.middleware.ts
│   │   ├── upload.middleware.ts
│   │   ├── rate-limit.middleware.ts
│   │   ├── error.middleware.ts
│   │   └── not-found.middleware.ts
│   │
│   ├── routes/
│   │   ├── index.ts
│   │   ├── auth.routes.ts
│   │   ├── user.routes.ts
│   │   ├── super-admin.routes.ts
│   │   ├── agency.routes.ts
│   │   ├── agency-member.routes.ts
│   │   ├── client.routes.ts
│   │   ├── project.routes.ts
│   │   ├── milestone.routes.ts
│   │   ├── task.routes.ts
│   │   ├── meeting.routes.ts
│   │   ├── feedback.routes.ts
│   │   ├── file.routes.ts
│   │   ├── activity.routes.ts
│   │   ├── dashboard.routes.ts
│   │   └── ai.routes.ts
│   │
│   ├── services/
│   │   ├── auth.service.ts
│   │   ├── user.service.ts
│   │   ├── super-admin.service.ts
│   │   ├── agency.service.ts
│   │   ├── agency-member.service.ts
│   │   ├── client.service.ts
│   │   ├── project.service.ts
│   │   ├── milestone.service.ts
│   │   ├── task.service.ts
│   │   ├── meeting.service.ts
│   │   ├── feedback.service.ts
│   │   ├── file.service.ts
│   │   ├── activity.service.ts
│   │   ├── dashboard.service.ts
│   │   └── ai/
│   │       ├── ai.service.ts
│   │       ├── project-health.service.ts
│   │       ├── meeting-summary.service.ts
│   │       ├── client-update.service.ts
│   │       └── feedback-assistant.service.ts
│   │
│   ├── validators/
│   │   ├── auth.validator.ts
│   │   ├── user.validator.ts
│   │   ├── agency.validator.ts
│   │   ├── agency-member.validator.ts
│   │   ├── client.validator.ts
│   │   ├── project.validator.ts
│   │   ├── milestone.validator.ts
│   │   ├── task.validator.ts
│   │   ├── meeting.validator.ts
│   │   ├── feedback.validator.ts
│   │   ├── file.validator.ts
│   │   └── ai.validator.ts
│   │
│   ├── types/
│   │   ├── auth.types.ts
│   │   ├── user.types.ts
│   │   ├── tenant.types.ts
│   │   ├── agency.types.ts
│   │   ├── client.types.ts
│   │   ├── project.types.ts
│   │   ├── task.types.ts
│   │   ├── meeting.types.ts
│   │   ├── feedback.types.ts
│   │   ├── file.types.ts
│   │   ├── ai.types.ts
│   │   └── express.d.ts
│   │
│   ├── utils/
│   │   ├── jwt.ts
│   │   ├── password.ts
│   │   ├── api-response.ts
│   │   ├── pagination.ts
│   │   ├── logger.ts
│   │   ├── file.ts
│   │   └── tenant.ts
│   │
│   ├── app.ts
│   └── server.ts
│
├── uploads/
│   └── .gitkeep
│
├── .env
├── .env.example
├── .gitignore
├── package.json
├── package-lock.json
├── prisma7.config.ts
├── tsconfig.json
└── README.md
```

---

# Purpose of the Main Backend Layers

## Config

Stores environment, database, CORS, and AI configuration.

``` text
src/config/
```

## Controllers

Receives HTTP requests and returns API responses.

``` text
src/controllers/
```

## Middlewares

Handles cross-cutting security and request processing:

-   Authentication
-   Role authorization
-   Tenant isolation
-   Client access
-   Agency status
-   Validation
-   File upload
-   Rate limiting
-   Error handling

``` text
src/middlewares/
```

## Routes

Defines REST API endpoints.

``` text
src/routes/
```

## Services

Contains business logic and database operations.

``` text
src/services/
```

## Validators

Uses Zod to validate incoming API data.

``` text
src/validators/
```

## Types

Contains TypeScript types and Express request extensions.

``` text
src/types/
```

## Utils

Contains reusable helper functions such as JWT, password, pagination,
logging, file, and tenant helpers.

``` text
src/utils/
```

---

# Planned Backend Request Flow

The intended backend flow is:

``` text
Frontend
   │
   ▼
Express Route
   │
   ▼
Authentication Middleware
   │
   ▼
Role/Tenant/Client Authorization
   │
   ▼
Validation
   │
   ▼
Controller
   │
   ▼
Service
   │
   ▼
Prisma Client
   │
   ▼
MySQL
```

For example:

``` text
GET /api/projects/123
        │
        ▼
JWT validation
        │
        ▼
Identify user
        │
        ▼
Identify agency
        │
        ▼
Check project belongs to agency
        │
        ▼
Check role/client permissions
        │
        ▼
Project service
        │
        ▼
Prisma
        │
        ▼
MySQL
```

---

# Git Commands Used So Far

Initial repository setup:

``` powershell
git init
```

Check status:

``` powershell
git status
```

Add files:

``` powershell
git add .
```

Initial commit:

``` powershell
git commit -m "initilize project"
```

Add GitHub remote:

``` powershell
git remote add origin <repository-url>

Push main:

``` powershell
git push -u origin main
```

Later commit:

``` powershell
git add .
git commit -m "Add MySQL docker setup"
git push
```

Check ignored `.env`:

``` powershell
git check-ignore -v .env
```

---

# Docker Commands Used

Check Docker:

``` powershell
docker --version
```

Check Docker Compose:

``` powershell
docker compose version
```

View running containers:

``` powershell
docker ps
```

Start MySQL:

``` powershell
docker compose up -d
```

View MySQL logs:

``` powershell
docker logs appzex-mysql
```

Verify MySQL version:

``` powershell
docker exec appzex-mysql mysql -u root -p"YOUR_ROOT_PASSWORD" -e "SELECT VERSION();"
```

Verify application database:

``` powershell
docker exec appzex-mysql mysql -u appzex_user -p"YOUR_APP_PASSWORD" -e "SHOW DATABASES;"
```

> Passwords are shown above only as placeholders. Do not commit real
> credentials or document them in the repository.

---

# Node/npm Commands Used

Initialize backend:

``` powershell
npm init -y
```

Install production dependencies:

``` powershell
npm install express @prisma/client jsonwebtoken bcryptjs zod helmet cors express-rate-limit dotenv multer morgan
```

Install development dependencies:

``` powershell
npm install -D typescript tsx @types/node @types/express @types/jsonwebtoken @types/bcryptjs @types/cors @types/multer @types/morgan prisma
```

Install Prisma MySQL adapter:

``` powershell
npm install @prisma/adapter-mariadb mariadb
```

View installed packages:

``` powershell
npm list --depth=0
```

Check TypeScript:

``` powershell
npx tsc --version
```

Type-check without generating JavaScript:

``` powershell
npx tsc --noEmit
```


---

# Important Prisma Note

The project is intentionally staying on:

``` text
Prisma 7.10.0
```

even though Prisma displayed an available prerelease major version.

No Prisma major upgrade should be performed during implementation unless
it is deliberately planned and tested.

---

# Important Security Rules for Development

Never commit:

``` text
.env
real passwords
API keys
JWT secrets
production credentials
```

Do not manually edit:

``` text
generated/prisma/
```

Generated Prisma Client files should be recreated using:

``` powershell
npx prisma generate
```

Database schema changes should be made in:

``` text
prisma/schema.prisma
```

and then applied through Prisma migrations.

---