---
name: nextjs-best-practices
description: Next.js App Router principles — Server Components, data fetching, routing, API routes, caching, Server Actions. Use before frontend implementation on Next.js projects.
---

# Next.js Best Practices

## Server vs Client

```
Needs useState, effects, events? → 'use client'
Data fetch, static layout?       → Server Component (default)
Both?                            → Server parent + Client child
```

## Data fetching

| Pattern | Use |
|---------|-----|
| Default | Static (build cache) |
| `revalidate` | ISR |
| `no-store` | Per-request dynamic |

## Routing

| File | Purpose |
|------|---------|
| `page.tsx` | Route UI |
| `layout.tsx` | Shared layout |
| `loading.tsx` | Loading |
| `error.tsx` | Error boundary |

## API route handlers

- Validate with Zod; proper status codes; handle errors
- Prefer Edge when appropriate

## Performance

- `next/image` for images; dynamic import for heavy client bundles

## Anti-patterns

| Don't | Do |
|-------|-----|
| `'use client'` everywhere | Server by default |
| Fetch in client | Fetch in server |
| Skip loading/error UI | `loading.tsx` / `error.tsx` |

## Structure

```
app/
├── (group)/page.tsx
├── api/[resource]/route.ts
└── components/ui/
```
