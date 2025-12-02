# EduNotas - School Grade Management System

## Overview

EduNotas is a comprehensive school grade management system designed for educational institutions. The application provides separate interfaces for administrators/teachers and students, enabling efficient management of grades, students, teachers, classes, subjects, evaluations, and school communications.

The system is built as a full-stack web application with a React frontend and Express backend, using PostgreSQL for data persistence. It features role-based access control with three distinct user types (admin, professor, aluno) and provides both native authentication and Replit OAuth integration.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework & Build System**
- **React 18** with TypeScript for type-safe component development
- **Vite** as the build tool and development server for fast HMR and optimized production builds
- **Wouter** for lightweight client-side routing
- **TanStack Query (React Query)** for server state management, caching, and data synchronization

**UI Component Library**
- **shadcn/ui** component system built on Radix UI primitives
- **Tailwind CSS** for utility-first styling with custom design tokens
- **Material Design 3 (Material You)** principles for visual design consistency
- Theme system supporting light/dark modes with system preference detection

**Design Tokens & Styling**
- Custom CSS variables for colors, spacing, shadows, and typography
- Roboto font family for primary text, Roboto Mono for grade displays
- Consistent spacing scale (2, 4, 6, 8, 12, 16 units)
- Elevation system using shadow variables (shadow-xs, shadow-sm, etc.)
- Hover and active state elevations for interactive elements

**State Management Strategy**
- Server state managed via TanStack Query with query key conventions
- Authentication state tracked through `/api/auth/user` query
- Role-based UI rendering using `useAuth` hook
- Optimistic updates disabled (refetch on window focus and intervals set to false)

### Backend Architecture

**Server Framework**
- **Express.js** for HTTP server and routing
- HTTP server created separately to support potential WebSocket integration
- Middleware stack: JSON body parsing, URL-encoded forms, custom logging
- Request/response logging with timestamp and duration tracking

**Authentication & Authorization**
- **Dual authentication system**:
  - Native authentication using bcrypt for password hashing
  - Replit OAuth using OpenID Connect (OIDC) flow
- **Passport.js** for authentication strategy management
- **Express-session** with PostgreSQL session store (connect-pg-simple)
- Session TTL: 7 days with secure, httpOnly cookies
- Role-based middleware (`isAuthenticated`, `isAdmin`) for route protection

**API Design Pattern**
- RESTful endpoints under `/api` namespace
- Separate authentication routes for each user role:
  - `/api/auth/signup-student`, `/api/auth/login-student`
  - `/api/auth/signup-teacher`, `/api/auth/login-teacher`
  - `/api/auth/login-admin`
- Protected routes require authentication middleware
- Admin-only routes use additional role verification
- Error responses return status codes with descriptive messages

### Data Storage Solutions

**Database Technology**
- **PostgreSQL** via Neon serverless database
- **Drizzle ORM** for type-safe database queries and schema management
- WebSocket connection support using `ws` package for Neon compatibility

**Schema Architecture**
The database follows a relational model with the following core entities:

1. **Users Table** - Central authentication table
   - Stores email, password hash, role (enum: admin/professor/aluno), and status
   - Links to role-specific tables (students, teachers)
   - Supports both native auth and OAuth profiles

2. **Students Table** - Student-specific data
   - Links to user via `userId` foreign key
   - Contains registration number, enrollment date, class assignment
   - Includes relations to grades and class

3. **Teachers Table** - Teacher-specific data
   - Links to user via `userId` foreign key
   - Contains hire date and specialty information
   - Relations to subjects and classes via junction table

4. **Classes Table** - Academic class/grade levels
   - Contains year, section (e.g., "A", "B"), shift (enum: matutino/vespertino/noturno)
   - Links to students and subjects

5. **Subjects Table** - Course subjects
   - Name, code, description, workload hours
   - Links to evaluations and grades

6. **Evaluations Table** - Assessment definitions
   - Type (enum: prova/trabalho/projeto/recuperacao), weight, max score
   - Belongs to subject and class
   - Date and description fields

7. **Grades Table** - Student assessment scores
   - Links student to evaluation with actual score
   - Includes optional comments and grading timestamp

8. **Notices Table** - School communications
   - Title, content, target type (enum: geral/turma/aluno)
   - Optional targeting to specific class or student
   - Author tracking via teacher ID

9. **TeacherSubjectClass Junction Table** - Many-to-many relationships
   - Links teachers to the subjects they teach in specific classes

10. **Sessions Table** - Express session storage
    - SID (session ID), session data (JSONB), expiration timestamp
    - Indexed on expire column for efficient cleanup

**Database Operations Pattern**
- Storage layer abstraction in `server/storage.ts` provides interface methods
- Type-safe operations using Drizzle's query builder
- Relations pre-defined in schema for efficient joins
- Separate types for Insert, Update, and relations (e.g., `StudentWithRelations`)

### External Dependencies

**Core Framework Dependencies**
- `express` - Web server framework
- `react` & `react-dom` - Frontend UI library
- `vite` - Build tool and dev server
- `drizzle-orm` - Database ORM
- `@neondatabase/serverless` - Neon PostgreSQL client

**Authentication & Security**
- `passport` & `passport-local` - Authentication middleware
- `openid-client` - OAuth/OIDC integration for Replit auth
- `bcryptjs` - Password hashing
- `express-session` - Session management
- `connect-pg-simple` - PostgreSQL session store

**UI Component Libraries**
- `@radix-ui/*` - Headless UI primitives (20+ component packages)
- `lucide-react` - Icon system
- `tailwindcss` - Utility-first CSS framework
- `class-variance-authority` - Component variant management
- `cmdk` - Command palette component

**State Management & Data Fetching**
- `@tanstack/react-query` - Server state management
- `wouter` - Client-side routing
- `react-hook-form` - Form state management
- `@hookform/resolvers` - Form validation integration
- `zod` - Schema validation

**Utilities & Helpers**
- `clsx` & `tailwind-merge` - Conditional className utilities
- `date-fns` - Date formatting and manipulation
- `nanoid` - Unique ID generation
- `memoizee` - Function memoization

**Development Tools**
- `typescript` - Type system
- `tsx` - TypeScript execution for development
- `esbuild` - Production bundling
- `@replit/vite-plugin-*` - Replit-specific development plugins

**Database Tooling**
- `drizzle-kit` - Database migrations and schema management
- `drizzle-zod` - Zod schema generation from Drizzle schemas

**Build & Deployment**
- Production build combines Vite (client) and esbuild (server)
- Server dependencies bundled to reduce syscalls for improved cold start
- Allowlist strategy for bundling specific packages while externalizing others
- Environment variable requirements: `DATABASE_URL`, `SESSION_SECRET`, `ISSUER_URL`, `REPL_ID`