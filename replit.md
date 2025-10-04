# Winter Arc 2025 - Personal Productivity Dashboard

## Overview

Winter Arc is a personal productivity and goal-tracking application designed to help users maintain focus and achieve their objectives during the "winter arc" period of 2025. The application provides a comprehensive dashboard for managing daily tasks, tracking weekly goals, maintaining a vision board, and monitoring progress through gamification elements (streaks, XP, levels). It features a winter-themed UI with snowfall animations and motivational content to keep users engaged and motivated.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React 18 with TypeScript and Vite as the build tool

**UI Component Library**: Radix UI primitives with shadcn/ui styling system
- Comprehensive set of accessible, customizable components
- Tailwind CSS for styling with custom winter theme color palette
- CSS variables for dynamic theming
- Custom fonts: Inter (sans), Plus Jakarta Sans (headings), JetBrains Mono (monospace)

**Routing**: Wouter for lightweight client-side routing
- Single-page application with minimal routing complexity
- Main dashboard route at root path

**State Management**: TanStack Query (React Query) v5
- Server state management with caching and automatic refetching
- Custom hooks pattern for data operations (useTasks, useGoals, useVisionBoard, useStats)
- Optimistic updates and invalidation strategies

**Design Patterns**:
- Component composition with shadcn/ui base components
- Custom hooks for business logic separation
- Form validation using React Hook Form with Zod schemas
- Responsive design with mobile-first approach using custom `useIsMobile` hook

### Backend Architecture

**Runtime**: Node.js with Express.js framework

**Language**: TypeScript with ES modules

**API Design**: RESTful architecture
- Resource-based endpoints (/api/tasks, /api/goals, /api/vision-board, /api/progress-logs, /api/user-stats)
- CRUD operations with standard HTTP methods (GET, POST, PATCH, DELETE)
- JSON request/response format with Zod schema validation

**Code Organization**:
- `/server/index.ts` - Express server setup and middleware
- `/server/routes.ts` - API route handlers
- `/server/storage.ts` - Data storage abstraction layer with IStorage interface
- `/shared/schema.ts` - Shared type definitions and Zod schemas

**Development Setup**:
- Vite middleware integration for HMR in development
- Static file serving in production
- Request logging middleware with response timing
- Error handling with try-catch patterns

### Data Storage Solutions

**Database**: PostgreSQL (via Neon serverless)
- Drizzle ORM for type-safe database operations
- Schema-first approach with migrations in `/migrations` directory
- Connection via `@neondatabase/serverless` adapter

**Schema Design**:
- `tasks` table: User tasks with priority, category, time slots, completion status
- `goals` table: Weekly/biweekly goals with progress tracking (current/target values)
- `vision_board_items` table: Inspirational items with optional images and positioning
- `progress_logs` table: Daily progress snapshots for analytics
- `user_stats` table: Gamification metrics (streaks, XP, levels)

**Current Implementation**: In-memory storage (`MemStorage` class) for development/testing
- Implements IStorage interface for easy swapping with database implementation
- UUID-based primary keys for all entities
- Mock data initialization for demo purposes

**Migration Strategy**: Drizzle Kit configured for PostgreSQL
- Schema location: `./shared/schema.ts`
- Migration output: `./migrations`
- Push command: `npm run db:push`

### Authentication and Authorization

**Current State**: No authentication implemented
- Single-user application design
- No user management or session handling
- Future consideration: Session-based auth with `connect-pg-simple` (already in dependencies)

### External Dependencies

**Frontend Libraries**:
- `@tanstack/react-query` - Server state management
- `react-hook-form` + `@hookform/resolvers` - Form handling
- `zod` + `drizzle-zod` - Schema validation and type inference
- `wouter` - Client-side routing
- `date-fns` - Date formatting and manipulation
- `recharts` - Data visualization and charts
- `embla-carousel-react` - Carousel functionality
- `class-variance-authority` + `clsx` + `tailwind-merge` - Styling utilities

**Backend Libraries**:
- `express` - Web framework
- `drizzle-orm` - Database ORM
- `@neondatabase/serverless` - PostgreSQL adapter
- `connect-pg-simple` - PostgreSQL session store (available but unused)

**Development Tools**:
- `vite` - Build tool and dev server
- `tsx` - TypeScript execution for development
- `esbuild` - Production build bundling
- `drizzle-kit` - Database migrations and schema management
- `@replit/vite-plugin-*` - Replit-specific development enhancements

**Build Process**:
- Client build: Vite bundles React app to `dist/public`
- Server build: esbuild bundles Express server to `dist/index.js` with external packages
- Separate TypeScript compilation check via `npm run check`

**Database Configuration**:
- Requires `DATABASE_URL` environment variable for PostgreSQL connection
- Drizzle configured for PostgreSQL dialect with automatic UUID generation via `gen_random_uuid()`