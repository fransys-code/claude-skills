# Conversion Table: Next.js App Router -> TanStack Start

Exhaustive reference to load during migration. All patterns come from TanStack Intent (official skill registry) and the TanStack Start migration guide.

**Path convention**: the migration renames `src/app/` -> `src/routes/` (TanStack Start default) via `git mv` in Phase 1. All target paths below refer to `src/routes/`.

## 1. File structure

| Next.js | TanStack Start | Notes |
|---|---|---|
| `src/app/layout.tsx` | `src/routes/__root.tsx` | Root route, exports `Route = createRootRoute(...)` |
| `src/app/page.tsx` | `src/routes/index.tsx` | Index route |
| `src/app/posts/page.tsx` | `src/routes/posts.tsx` | Route segment |
| `src/app/posts/[slug]/page.tsx` | `src/routes/posts/$slug.tsx` | Dynamic param |
| `src/app/posts/[...slug]/page.tsx` | `src/routes/posts/$.tsx` | Catch-all (splat) |
| `src/app/api/foo/route.ts` | `src/routes/api/foo.ts` | API route via `server.handlers` |
| `src/app/loading.tsx` | -> `pendingComponent` in the route | No file convention |
| `src/app/error.tsx` | -> `errorComponent` in the route | No file convention |
| `src/app/not-found.tsx` | -> `notFoundComponent` in `__root` | Root-level configuration |
| `src/app/(marketing)/page.tsx` | `src/routes/(marketing).tsx` + child routes | Pathless layout via TanStack layout route |
| `middleware.ts` | Logic in `beforeLoad` or `createServerFn` middleware | Case by case |

## 2. Root layout (__root.tsx)

### Next.js
```tsx
// src/app/layout.tsx
export const metadata = {
  title: 'My App',
  description: '...',
}

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

### TanStack Start
```tsx
// src/routes/__root.tsx
import { createRootRoute, HeadContent, Outlet, Scripts } from '@tanstack/react-router'

export const Route = createRootRoute({
  head: () => ({
    meta: [
      { title: 'My App' },
      { name: 'description', content: '...' },
    ],
    links: [
      { rel: 'icon', href: '/favicon.ico' },
    ],
  }),
  component: RootComponent,
})

function RootComponent() {
  return (
    <html lang="en">
      <head>
        <HeadContent />
      </head>
      <body>
        <Outlet />
        <Scripts />
      </body>
    </html>
  )
}
```

## 3. Page route (static)

### Next.js
```tsx
// src/app/about/page.tsx
export default function About() {
  return <h1>About</h1>
}
```

### TanStack Start
```tsx
// src/routes/about.tsx
import { createFileRoute } from '@tanstack/react-router'

export const Route = createFileRoute('/about')({
  component: About,
})

function About() {
  return <h1>About</h1>
}
```

## 4. Dynamic route with loader

### Next.js
```tsx
// src/app/posts/[slug]/page.tsx
async function getPost(slug: string) {
  const res = await fetch(`https://api.example.com/posts/${slug}`)
  return res.json()
}

export default async function Post({ params }: { params: { slug: string } }) {
  const post = await getPost(params.slug)
  return <article>{post.title}</article>
}
```

### TanStack Start
```tsx
// src/routes/posts/$slug.tsx
import { createFileRoute } from '@tanstack/react-router'

export const Route = createFileRoute('/posts/$slug')({
  loader: async ({ params }) => {
    const res = await fetch(`https://api.example.com/posts/${params.slug}`)
    return res.json()
  },
  component: Post,
})

function Post() {
  const post = Route.useLoaderData()
  return <article>{post.title}</article>
}
```

## 5. Catch-all (splat) route

### Next.js
```tsx
// src/app/docs/[...slug]/page.tsx
export default function Docs({ params }: { params: { slug: string[] } }) {
  return <div>{params.slug.join('/')}</div>
}
```

### TanStack Start
```tsx
// src/routes/docs/$.tsx
import { createFileRoute } from '@tanstack/react-router'

export const Route = createFileRoute('/docs/$')({
  component: Docs,
})

function Docs() {
  const { _splat } = Route.useParams()
  return <div>{_splat}</div>
}
```

## 6. Search params

### Next.js
```tsx
'use client'
import { useSearchParams } from 'next/navigation'

export default function Search() {
  const params = useSearchParams()
  const q = params.get('q')
  return <div>Query: {q}</div>
}
```

### TanStack Start
```tsx
import { createFileRoute } from '@tanstack/react-router'
import { z } from 'zod'

export const Route = createFileRoute('/search')({
  validateSearch: z.object({ q: z.string().optional() }),
  component: Search,
})

function Search() {
  const { q } = Route.useSearch()
  return <div>Query: {q}</div>
}
```

## 7. Server Action -> Server Function

### Next.js
```tsx
// src/app/actions.ts
'use server'

export async function createPost(formData: FormData) {
  const title = formData.get('title')
  await db.post.create({ data: { title } })
}
```

### TanStack Start
```tsx
// src/lib/actions.ts  (outside src/routes/ to avoid being treated as a route)
import { createServerFn } from '@tanstack/react-start'
import { z } from 'zod'

export const createPost = createServerFn({ method: 'POST' })
  .validator(z.object({ title: z.string() }))
  .handler(async ({ data }) => {
    await db.post.create({ data: { title: data.title } })
  })

// Client-side usage: await createPost({ data: { title: 'Hello' } })
```

## 8. API Route

### Next.js
```ts
// src/app/api/posts/route.ts
export async function GET() {
  const posts = await db.post.findMany()
  return Response.json(posts)
}

export async function POST(req: Request) {
  const body = await req.json()
  const post = await db.post.create({ data: body })
  return Response.json(post)
}
```

### TanStack Start
```ts
// src/routes/api/posts.ts
import { createFileRoute } from '@tanstack/react-router'

export const Route = createFileRoute('/api/posts')({
  server: {
    handlers: {
      GET: async () => {
        const posts = await db.post.findMany()
        return Response.json(posts)
      },
      POST: async ({ request }) => {
        const body = await request.json()
        const post = await db.post.create({ data: body })
        return Response.json(post)
      },
    },
  },
})
```

**Note**: import from `@tanstack/react-router` (not `@tanstack/react-start/server`). Server routes and UI routes share the same `createFileRoute`; the `server.handlers` property distinguishes an API route from a UI route. A route can even contain both (`component` + `server.handlers`).

## 9. Links

### Next.js
```tsx
import Link from 'next/link'

<Link href={`/posts/${post.id}`}>Read</Link>
<Link href={`/search?q=${query}`}>Search</Link>
```

### TanStack Start
```tsx
import { Link } from '@tanstack/react-router'

<Link to="/posts/$postId" params={{ postId: post.id }}>Read</Link>
<Link to="/search" search={{ q: query }}>Search</Link>
```

**WARNING**: no template literal in `to`. Always separate params/search.

## 10. Programmatic navigation

### Next.js
```tsx
'use client'
import { useRouter } from 'next/navigation'

const router = useRouter()
router.push(`/posts/${id}`)
router.replace('/login')
router.back()
```

### TanStack Start
```tsx
import { useNavigate } from '@tanstack/react-router'

const navigate = useNavigate()
navigate({ to: '/posts/$postId', params: { postId: id } })
navigate({ to: '/login', replace: true })
navigate({ to: '..' })  // back
```

## 11. Image

### Next.js
```tsx
import Image from 'next/image'

<Image src="/hero.jpg" alt="Hero" width={800} height={600} priority />
<Image src="/bg.jpg" alt="" fill />
```

### TanStack Start (via @unpic/react)
```tsx
import { Image } from '@unpic/react'

<Image src="/hero.jpg" alt="Hero" width={800} height={600} priority />
<Image src="/bg.jpg" alt="" layout="fullWidth" />
```

## 12. Font

### Next.js
```tsx
// src/app/layout.tsx
import { Inter } from 'next/font/google'

const inter = Inter({ subsets: ['latin'] })

export default function RootLayout({ children }) {
  return <body className={inter.className}>{children}</body>
}
```

### TanStack Start (via Fontsource)
```bash
bun add @fontsource/inter
```

```css
/* src/globals.css */
@import '@fontsource/inter/400.css';
@import '@fontsource/inter/700.css';

body {
  font-family: 'Inter', system-ui, sans-serif;
}
```

## 13. Metadata

### Next.js
```tsx
export const metadata = {
  title: 'Post Title',
  description: 'Post description',
  openGraph: { images: ['/og.png'] },
}
```

### TanStack Start
```tsx
export const Route = createFileRoute('/posts/$slug')({
  head: ({ loaderData }) => ({
    meta: [
      { title: loaderData.title },
      { name: 'description', content: loaderData.description },
      { property: 'og:image', content: '/og.png' },
    ],
  }),
  loader: ...,
  component: ...,
})
```

## 14. Middleware

### Next.js
```ts
// middleware.ts
export function middleware(req: NextRequest) {
  if (!req.cookies.get('session')) {
    return NextResponse.redirect(new URL('/login', req.url))
  }
}
```

### TanStack Start - Option A: beforeLoad (simple auth)
```tsx
// src/routes/(authed).tsx
export const Route = createFileRoute('/(authed)')({
  beforeLoad: ({ context }) => {
    if (!context.session) {
      throw redirect({ to: '/login' })
    }
  },
})
```

### TanStack Start - Option B: Server fn middleware
```ts
import { createMiddleware } from '@tanstack/react-start'

const authMiddleware = createMiddleware().server(async ({ next }) => {
  const session = await getSession()
  if (!session) throw new Error('Unauthorized')
  return next({ context: { session } })
})

export const protectedAction = createServerFn()
  .middleware([authMiddleware])
  .handler(...)
```

## 15. Loading / Error / NotFound

### Next.js
```
src/app/loading.tsx
src/app/error.tsx
src/app/not-found.tsx
```

### TanStack Start
```tsx
// Loading
export const Route = createFileRoute('/...')({
  pendingComponent: () => <Spinner />,
  ...
})

// Error
export const Route = createFileRoute('/...')({
  errorComponent: ({ error }) => <ErrorDisplay error={error} />,
  ...
})

// NotFound (in __root.tsx)
export const Route = createRootRoute({
  notFoundComponent: () => <NotFound />,
  ...
})
```

## 16. Vite config (CRITICAL)

```ts
// vite.config.ts
import { defineConfig } from 'vite'
import { tanstackStart } from '@tanstack/react-start/plugin/vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  server: {
    port: 3000,  // preserve Next.js dev port
  },
  resolve: {
    tsconfigPaths: true,  // required for @/* alias inherited from Next.js
  },
  plugins: [
    tanstackStart({ srcDirectory: 'src' }),  // routesDirectory: 'src/routes' by default
    react(),  // MANDATORY: AFTER tanstackStart()
    tailwindcss(),
  ],
})
```

**Note**: the migration moves `src/app` -> `src/routes` (TanStack Start default) via `git mv` in Phase 1, so no need to override `routesDirectory`.

## 17. router.tsx

```tsx
// src/router.tsx
import { createRouter } from '@tanstack/react-router'
import { routeTree } from './routeTree.gen'

export function createAppRouter() {
  return createRouter({
    routeTree,
    defaultPreload: 'intent',
    defaultPreloadStaleTime: 0,
  })
}

declare module '@tanstack/react-router' {
  interface Register {
    router: ReturnType<typeof createAppRouter>
  }
}
```

## 18. package.json scripts

### Next.js
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  }
}
```

### TanStack Start
```json
{
  "scripts": {
    "dev": "vite dev",
    "build": "vite build",
    "start": "node .output/server/index.mjs",
    "typecheck": "tsc --noEmit"
  }
}
```
