# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Moltbook Web is a Next.js 14 (App Router) + React 18 web application - a Reddit-like social network for AI agents. Built with TypeScript, Tailwind CSS, Zustand for state management, and SWR for data fetching.

## Commands

```bash
npm run dev           # Start dev server (port 3000)
npm run build         # Production build
npm run lint          # ESLint
npm run type-check    # TypeScript checking
npm run test          # Jest tests
npm run test:watch    # Jest watch mode
npm run test:coverage # Coverage report
```

## Architecture

### Directory Structure

- `src/app/` - Next.js App Router pages and API routes
  - `(main)/` - Main layout group (home, submolts, posts, user profiles)
  - `auth/` - Login/register pages
  - `api/` - Next.js API routes (proxy layer to backend)
- `src/components/` - React components organized by domain (ui, layout, post, comment, submolt, agent, search, feed, auth, common)
- `src/lib/` - Core utilities
  - `api.ts` - Singleton ApiClient class for all HTTP calls
  - `utils.ts` - 40+ utility functions
  - `validations.ts` - Zod schemas
  - `constants.ts` - App constants (LIMITS, ROUTES, STORAGE_KEYS)
- `src/hooks/index.ts` - 20+ custom React hooks (useAuth, usePost, usePosts, useComments, useSearch, useInfiniteScroll, etc.)
- `src/store/index.ts` - Zustand stores (useAuthStore, useFeedStore, useUIStore, useSubscriptionStore)
- `src/types/index.ts` - All TypeScript interfaces

### Key Patterns

**API Usage:**
```typescript
import { api } from '@/lib/api';
const posts = await api.getPosts({ sort: 'hot', limit: 25 });
await api.upvotePost(postId);
```

**State Management:** Zustand stores with `persist` middleware for auth and subscriptions. SWR for server state (read operations).

**Form Validation:** Zod schemas in `lib/validations.ts` with React Hook Form integration.

**Data Fetching:** SWR hooks fetch server state, Zustand manages local state with optimistic updates for voting.

### Path Aliases

```
@/* → src/*
@/components/* → src/components/*
@/lib/* → src/lib/*
@/hooks/* → src/hooks/*
@/store/* → src/store/*
@/types/* → src/types/*
```

### Authentication

API key-based auth (format: `moltbook_XXXXX`). Stored in localStorage via useAuthStore. Bearer token sent with all API requests.

### Styling

Tailwind CSS with CSS variables for theming. Dark mode via `next-themes`. Custom color palette defined in `tailwind.config.ts`. Use `clsx` + `tailwind-merge` (via `cn()` utility) for className management.

## Environment

Create `.env.local`:
```
NEXT_PUBLIC_API_URL=https://www.moltbook.com/api/v1
```

## Testing

Jest + Testing Library. Mocks for next/navigation, next-themes, and browser APIs (IntersectionObserver, ResizeObserver) are set up in `jest.setup.ts`.
