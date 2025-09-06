# Erasor Clone

A Next.js 14 app that combines a rich text editor (Editor.js) and a collaborative whiteboard (Excalidraw), backed by Convex for realtime data and Kinde for authentication.

## Tech Stack
- Next.js 14 (App Router)
- React 18
- Tailwind CSS
- Editor.js (header, list, checklist, paragraph, warning)
- Excalidraw
- Convex (database + serverless functions)
- Kinde Auth (Next.js SDK)
- Radix UI + shadcn/ui components

## Features
- Kinde-based auth with middleware protection for `/dashboard`
- Team and file management via Convex functions
- Rich text editing with Editor.js, persisted to Convex
- Whiteboard editing with Excalidraw, persisted to Convex
- Theming and basic UI primitives

## Getting Started

### Prerequisites
- Node.js 18+
- A Convex project (get the deployment URL)
- A Kinde application (domain and credentials)

### Installation
```bash
npm install
```

### Environment Variables
Create a `.env.local` file in the project root with the following variables:

```bash
# Convex
NEXT_PUBLIC_CONVEX_URL="https://<your-convex-deployment>.convex.cloud"

# Kinde
KINDE_ISSUER_URL="https://<your-kinde-domain>.kinde.com"
KINDE_SITE_URL="http://localhost:3000"
KINDE_POST_LOGOUT_REDIRECT_URL="http://localhost:3000"
KINDE_CLIENT_ID="<your-kinde-client-id>"
KINDE_CLIENT_SECRET="<your-kinde-client-secret>"
```

Notes:
- `NEXT_PUBLIC_CONVEX_URL` is used by `app/ConvexClientProvider.tsx`.
- Kinde routes are handled by `app/api/auth/[kindeAuth]/route.js` and `middleware.ts` protects `/dashboard`.

### Development
```bash
npm run dev
```
The app runs at `http://localhost:3000`.

### Build
```bash
npm run build
npm start
```

## Project Structure
```
app/
  (routes)/
    dashboard/
    teams/
    workspace/
  _components/
  api/auth/[kindeAuth]/
convex/
  files.tsx
  teams.tsx
  user.tsx
components/ui/
```

Key files:
- `app/ConvexClientProvider.tsx`: initializes `ConvexReactClient` using `NEXT_PUBLIC_CONVEX_URL`.
- `middleware.ts`: redirects unauthenticated users to Kinde login for `/dashboard`.
- `convex/files.tsx`: file CRUD and document/whiteboard mutations.
- `app/(routes)/workspace/_components/Editor.tsx`: Editor.js integration and save logic.
- `app/(routes)/workspace/_components/Canvas.tsx`: Excalidraw integration and save logic.

## Convex
- Define your schema and deploy with the Convex CLI.
- The client uses generated APIs from `convex/_generated/api`.

## Authentication (Kinde)
- Uses `@kinde-oss/kinde-auth-nextjs`.
- `middleware.ts` checks `isAuthenticated` and redirects to `/api/auth/login?post_login_redirect_url=/dashboard`.

## Images
- Next Image is configured to allow `lh3.googleusercontent.com` in `next.config.mjs`.

## Scripts
- `npm run dev`: Start dev server
- `npm run build`: Build for production
- `npm start`: Run production build
- `npm run lint`: Lint

## License
This repository is provided as-is for educational purposes. Add your preferred license.
