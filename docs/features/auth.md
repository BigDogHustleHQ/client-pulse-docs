# Auth — SID-61

**Branch:** `feature/sid-61-auth-sessions-and-tenant-scoping` (frontend only, not yet merged)
**Provider:** Clerk
**Repo:** `client-pulse-frontend`

---

## Overview

Auth is handled entirely by Clerk. The frontend holds all auth UI and session state. The API and backend will consume the Clerk JWT in a future ticket for tenant scoping.

---

## Routes

| Route | File | Purpose |
|-------|------|---------|
| `/login` | `src/app/login/[[...login]]/page.tsx` | Email/password + Google SSO login |
| `/registration` | `src/app/registration/[[...registration]]/page.tsx` | New account creation |
| `/sso-callback` | `src/app/sso-callback/page.tsx` | OAuth redirect handler — renders `<AuthenticateWithRedirectCallback />` |

All other routes are protected by the Clerk middleware in `proxy.ts`.

---

## Middleware

`src/proxy.ts` (Next.js 16 renamed `middleware.ts` → `proxy.ts`):

```ts
const isPublicRoute = createRouteMatcher([
  '/',
  '/login(.*)',
  '/registration(.*)',
  '/sso-callback(.*)',
]);

export default clerkMiddleware(async (auth, request) => {
  if (!isPublicRoute(request)) {
    await auth.protect();
  }
});
```

Any route not in the public list triggers `auth.protect()` — Clerk redirects unauthenticated users to `/login`.

---

## Client State

`src/store/auth/useAuthStore.ts` — Zustand store:

```ts
interface AuthUser {
  clerkId: string;
  email: string;
  firstName: string | null;
  lastName: string | null;
}
```

After a successful sign-in or sign-up, `setUser()` is called with data from the Clerk response. `clearAuth()` is called on sign-out. The store is exported from `src/store/index.ts`.

---

## Components

### `LoginForm`
`src/components/features/auth/LoginForm/LoginForm.tsx`

- Email + password fields with show/hide password toggle
- "Remember me" checkbox
- Forgot password link (`/login/forgot-password`)
- Google SSO button via `signIn.sso({ strategy: 'oauth_google' })`
- On success: calls `signIn.finalize()`, hydrates `useAuthStore`, pushes to `/dashboard`

### `RegistrationForm`
`src/components/features/auth/RegistrationForm/RegistrationForm.tsx`

- First name, last name (optional), email, password fields
- Google SSO button
- On success: calls `signUp.finalize()`, hydrates `useAuthStore`, pushes to `/dashboard`
- If `signUp.status !== 'complete'`: prompts email verification

---

## Environment Variables

```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=

NEXT_PUBLIC_CLERK_SIGN_IN_URL=/login
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/registration
```

---

## What's Not Done Yet (tenant scoping)

- API (`client-pulse-api`) does not yet validate Clerk JWTs
- Backend (`client-pulse-backend`) does not yet enforce tenant isolation via RLS
- `useAuthStore` `clerkId` is the bridge — future work will pass it as a header for server-side tenant resolution
