---
name: migrate-nextjs-to-tanstack
description: "Autonomous one-shot migration Next.js App Router -> TanStack Start. Audit + scaffolding + family-by-family migration + cleanup + final validation (tsc + vite build + SSR smoke test). Applies TanStack Intent (createServerFn, inferred types, no use server/use client). Compatible with /goal unattended execution. Do not use for: React Router alone, pre-13 Pages Router, new project from scratch."
user-invocable: true
disable-model-invocation: true
argument-hint: "[project-path]"
allowed-tools:
  - Bash(git *)
  - Bash(bun *)
  - Bash(pnpm *)
  - Bash(yarn *)
  - Bash(npm *)
  - Bash(npx *)
  - Bash(find *)
  - Bash(grep *)
  - Bash(rm *)
  - Bash(curl *)
  - Bash(fuser *)
  - Bash(sleep *)
  - Bash(cd *)
  - Bash(echo *)
  - Bash(test *)
  - Bash(ls *)
  - Read
  - Edit
  - Write
  - Grep
  - Glob
---

# Next.js App Router -> TanStack Start Migration (AUTONOMOUS MODE)

Migrate a Next.js project end-to-end to TanStack Start **without human intervention**. Designed for test apps, POCs, or any migration where unattended execution is acceptable.

## Operating mode

**AUTONOMOUS ONE-SHOT.** No validation pauses, no intermediate questions. You chain phases in order, committing at each milestone. Checkpoints are **mechanical** (tsc, vite build, grep) and their output appears in the transcript so a `/goal` evaluator can prove completion.

**Target.** If `$ARGUMENTS` is non-empty, `cd "$ARGUMENTS"` BEFORE any command. Otherwise, work in the current cwd. Verify that `package.json` contains `"next"` in deps — if not, abort with a clear message.

If you encounter a case not covered (see "Grey areas"):
1. **DO NOT ask the user.**
2. Apply the documented **fallback strategy**.
3. Add a note in `MIGRATION_NOTES.md` at the root for later audit.
4. Continue.

## Goal & success criteria (verifiable from transcript)

**Success provable by these commands (to run at end of migration):**
- [ ] `grep -r "from ['\"]next/" src/ ; grep -r "from 'next'" src/` returns **empty**
- [ ] `grep -rE "['\"]use (server|client)['\"]" src/` returns **empty**
- [ ] `npx tsc --noEmit` returns **exit 0**
- [ ] `npx vite build` returns **exit 0**
- [ ] `vite dev` responds `HTTP 200` on `/` (SSR runtime smoke test)
- [ ] `package.json` no longer contains `"next"` or `"@next/*"` in dependencies
- [ ] No `next.config.*`, `middleware.ts` (Next), `next-env.d.ts` files remain

All these commands MUST be executed at the end of the run with their output visible in the transcript.

## Autonomous workflow (strict sequence)

### Phase 0 - Silent audit

```bash
# Check branch & working tree
git rev-parse --abbrev-ref HEAD
git status --short

# If working tree is dirty -> create a dedicated branch and commit everything before
# If on main/master -> create refactor/tanstack-migration
```

If the current branch is `main`/`master` OR if the working tree is dirty:
```bash
git checkout -b refactor/tanstack-migration
git add -A && git commit -m "chore: snapshot before TanStack migration" || true
```

Map all Next.js routes (Phase 0 = before the `git mv`, so we still read `src/app`):
```bash
find src/app -type f \( -name "page.tsx" -o -name "layout.tsx" -o -name "route.ts" -o -name "route.tsx" -o -name "loading.tsx" -o -name "error.tsx" -o -name "not-found.tsx" -o -name "middleware.ts" \) | sort
grep -rE "['\"]use (server|client)['\"]" src/ -l | sort -u
```

Record the list in context. No user prompt.

### Phase 1 - TanStack scaffolding

1. Install deps (detect package manager via `lockfile`):
   - If `bun.lockb`: `bun add @tanstack/react-router @tanstack/react-start nitro vite @vitejs/plugin-react @tailwindcss/vite @unpic/react`
   - If `pnpm-lock.yaml`: `pnpm add ...`
   - If `yarn.lock`: `yarn add ...`
   - Otherwise: `npm install ...`

2. Uninstall: `next`, `@next/*`, `eslint-config-next` (only in Phase 4).

3. Create `vite.config.ts` with **critical plugin order**:
   ```ts
   import { defineConfig } from 'vite'
   import { tanstackStart } from '@tanstack/react-start/plugin/vite'
   import react from '@vitejs/plugin-react'
   import tailwindcss from '@tailwindcss/vite'

   export default defineConfig({
     server: { port: 3000 },
     resolve: { tsconfigPaths: true },  // @/* alias inherited from Next.js
     plugins: [
       tanstackStart({ srcDirectory: 'src' }),  // routesDirectory: 'src/routes' by default
       react(),       // MANDATORY: AFTER tanstackStart()
       tailwindcss(),
     ],
   })
   ```

   Ensure `tailwindcss` (peer) is installed in addition to `@tailwindcss/vite`:
   ```bash
   <pm> add -D tailwindcss @tailwindcss/vite
   ```

4. Create `src/router.tsx` (standard TanStack Start boilerplate).

5. **Rename the routes folder to the TanStack convention**:
   ```bash
   git mv src/app src/routes
   ```
   The rest of the migration now works on `src/routes/`. The `routeTree.gen.ts` will be auto-generated on the first `vite dev` / `vite build` from this folder (TanStack Start default).

6. Rename `src/routes/layout.tsx` -> `src/routes/__root.tsx` (with `createRootRoute`, `<Outlet />`, `<HeadContent />`, `<Scripts />`, `head()` method for metadata).

7. Update `package.json` scripts: `dev` -> `vite dev`, `build` -> `vite build`, `start` -> `node .output/server/index.mjs`.

```bash
git add -A
git commit -m "chore(tanstack): scaffold TanStack Start setup"
```

### Phase 2 - Family-by-family migration (automatic cascade)

Attack order (simplest to most complex), automatic loop:

1. **Static routes** (`page.tsx` without dynamic params, without server action)
2. **Simple dynamic routes** (`[slug]/page.tsx`)
3. **Catch-all routes** (`[...slug]/page.tsx`)
4. **API routes** (`route.ts`)
5. **Routes with server actions** (`"use server"`)
6. **Pathless layouts** (`(marketing)`, `(auth)`)
7. **Special conventions** (`loading.tsx`, `error.tsx`, `not-found.tsx`)

For each family:
1. List the member routes.
2. Apply the conversion table (see `CONVERSION_TABLE.md`).
3. After each migrated route, silently run:
   ```bash
   npx tsc --noEmit 2>&1 | tail -20
   ```
   If error on the route: fix it immediately. If error elsewhere: note and continue (will be resolved after dependencies migrated).
4. After the whole family:
   ```bash
   grep -r "from ['\"]next/" src/routes/<family>/ || echo "OK: no next/ imports"
   ```
5. Atomic commit:
   ```bash
   git add -A
   git commit -m "refactor(routing): migrate <family> to TanStack Start"
   ```

**Do not wait for validation between families.** Chain them.

### Phase 3 - Shared components

1. Replace `next/link` with `@tanstack/react-router` Link throughout `src/components/`:
   ```bash
   grep -rl "from 'next/link'" src/
   ```
   Conversion: `<Link href={...}>` -> `<Link to="..." params={...}>`. Watch for template literals (`/posts/${id}` -> `to="/posts/$postId" params={{ postId: id }}`).

2. Replace `next/navigation` (`useRouter`, `usePathname`, `useSearchParams`) with TanStack equivalent hooks.

3. Replace `next/image` with `@unpic/react`:
   ```bash
   grep -rl "from 'next/image'" src/
   ```

4. Replace `next/font` with Fontsource (add import in `globals.css`).

5. Remove all remaining `"use server"` and `"use client"`. Wrap in `createServerFn` what must be server-only.

6. Checkpoint:
   ```bash
   grep -rE "['\"]use (server|client)['\"]" src/ && echo "REMAINING DIRECTIVES" || echo "OK"
   grep -r "from ['\"]next/" src/ && echo "REMAINING NEXT IMPORTS" || echo "OK"
   ```

```bash
git add -A
git commit -m "refactor(deps): replace Next.js primitives with TanStack/Unpic/Fontsource"
```

### Phase 4 - Final cleanup

1. Uninstall Next.js:
   ```bash
   <package-manager> remove next @next/* eslint-config-next 2>/dev/null || true
   ```

2. Remove residual files:
   ```bash
   rm -f next.config.js next.config.ts next.config.mjs next-env.d.ts
   rm -f middleware.ts src/middleware.ts
   ```

3. Update `tsconfig.json`: remove the `next` plugin, adjust `jsx`/`moduleResolution` if necessary.

4. **Final validation (CRITICAL - output in transcript for /goal):**
   ```bash
   echo "=== CHECKPOINT: next/ imports ===" && (grep -r "from ['\"]next/" src/ ; grep -r "from 'next'" src/) || echo "PASS: no next/ imports"
   echo "=== CHECKPOINT: use server/client ===" && grep -rE "['\"]use (server|client)['\"]" src/ || echo "PASS: no directives"
   echo "=== CHECKPOINT: package.json ===" && (grep -E '"next"|"@next/' package.json && echo "FAIL" || echo "PASS: no next in package.json")
   echo "=== CHECKPOINT: tsc ===" && npx tsc --noEmit && echo "PASS: tsc clean" || echo "FAIL: tsc errors above"
   echo "=== CHECKPOINT: build ===" && npx vite build && echo "PASS: vite build success" || echo "FAIL: build errors above"
   echo "=== CHECKPOINT: smoke test SSR ===" && \
     (npx vite dev --port 3000 >/tmp/tanstack-dev.log 2>&1 &) && \
     sleep 8 && \
     curl -fsS -o /dev/null -w "HTTP %{http_code}\n" http://localhost:3000/ && \
     echo "PASS: dev server responds" || echo "FAIL: dev server check (see /tmp/tanstack-dev.log)"
   fuser -k 3000/tcp 2>/dev/null || true
   ```

   The final smoke test launches `vite dev` in the background, waits 8s, GETs `/` to prove SSR responds, then kills the port. Without it, `vite build` can pass but the app may crash at runtime (broken route tree, missing env vars, hydration errors).

5. Final commit:
   ```bash
   git add -A
   git commit -m "chore(tanstack): complete Next.js to TanStack Start migration"
   ```

6. **Report back** to the user:
   - Branch: `refactor/tanstack-migration`
   - N commits created
   - Result of the 5 checkpoints
   - Files added to `MIGRATION_NOTES.md` (if grey areas encountered)

## Non-negotiable rules (TanStack Intent)

### Execution model
- TanStack Start is **isomorphic by default**: all code runs client AND server unless wrapped in `createServerFn`. This is the **OPPOSITE** of Next.js Server Components.
- **Remove** all `"use server"` and `"use client"` directives.
- Any server code (DB, secrets, fs, private env vars, API keys) MUST be inside `createServerFn(...).handler(...)`.

### Types
- Types are **fully inferred** by TanStack Router. **Never** cast, **never** annotate an inferred value (params, search, loader data, context).
- No `any`, no type assertions to patch TS errors post-scaffolding.

### Vite config
- `tanstackStart()` BEFORE `react()` in `vite.config.ts`. Silent failure otherwise.

## Grey areas - Fallback strategies (DO NOT ask the user)

For each case, apply the fallback AND log in `MIGRATION_NOTES.md`.

| Next.js case | Fallback strategy |
|---|---|
| Server Components with streaming | Migrate as standard component + `Route.loader` with client-side `Suspense`. Log: "streaming downgrade" |
| `after()` from `next/server` | Replace with `setTimeout(() => fn(), 0)` in server fn (best-effort). Log: "after() approximated" |
| Pathless layout groups `(marketing)` | Keep folder, add a `<group>.tsx` file exporting a layout via `createFileRoute` with `<Outlet />`. Log: "pathless layout via TanStack layout route" |
| `not-found.tsx` | Use `notFoundComponent` in `createRootRoute`. Log: "not-found via root config" |
| `error.tsx` | Use `errorComponent` in the route. Log: "error boundary via route config" |
| `loading.tsx` | Use `pendingComponent` in the route. Log: "loading via pendingComponent" |
| `middleware.ts` Next | Migrate logic to a `createServerFn` middleware OR to loaders. For redirect/rewrite: use `beforeLoad` in `__root` or route. Log: "middleware migrated to <strategy>" |
| ISR / `revalidate` / `unstable_cache` | No direct equivalent. Implement cache via `Route.staleTime` + `Route.gcTime`. Log: "ISR approximated via staleTime" |
| Parallel routes (`@modal`) | Convert to plain route + global state. Log: "parallel route flattened" |
| Intercepting routes (`(.)photo/[id]`) | Convert to plain route. Log: "intercepting route flattened" |

## Conversion table (concise - see CONVERSION_TABLE.md for details)

| Next.js | TanStack Start |
|---|---|
| `src/app/layout.tsx` | `src/routes/__root.tsx` |
| `src/app/page.tsx` | `src/routes/index.tsx` |
| `src/app/posts/page.tsx` | `src/routes/posts.tsx` |
| `src/app/posts/[slug]/page.tsx` | `src/routes/posts/$slug.tsx` |
| `src/app/posts/[...slug]/page.tsx` | `src/routes/posts/$.tsx` |
| `src/app/api/foo/route.ts` | `src/routes/api/foo.ts` |
| `next/link` `<Link href="/posts/${id}">` | `<Link to="/posts/$postId" params={{ postId: id }}>` |
| `useRouter().push(...)` | `useNavigate()({ to: ... })` |
| `useParams()` | `Route.useParams()` |
| `useSearchParams()` | `Route.useSearch()` |
| Server Component fetch | `Route.loader` + `Route.useLoaderData()` |
| Server Action (`"use server"`) | `createServerFn({ method: 'POST' }).handler(...)` |
| `next/image` | `@unpic/react` |
| `next/font` | Fontsource via Tailwind |
| `export const metadata` | `head()` method returning `{ meta, links }` |

## Gotchas

1. **Vite plugin order**: `tanstackStart()` BEFORE `react()`. Test: if HMR doesn't work or routes aren't detected, that's why.

2. **Paths with `(parentheses)`**: always quote them in bash. Ex: `git diff "src/routes/(web).tsx"`. Otherwise the shell interprets the parentheses and throws an error.

3. **`routesDirectory`**: we migrate `src/app` -> `src/routes` (TanStack Start default) via `git mv` in Phase 1. No need to override in `vite.config.ts`. Git history follows the `mv`.

4. **`next/image` -> `@unpic/react`**: near drop-in. Verify `fill`, `sizes`, `priority` props.

5. **`next/font`**: replace `Inter({ subsets: ['latin'] })` with `@fontsource/inter` imported in `globals.css`.

6. **Metadata**: `export const metadata = { title: ... }` becomes:
   ```ts
   export const Route = createFileRoute(...)({
     head: () => ({ meta: [{ title: '...' }] }),
     component: ...
   })
   ```

7. **Passive Server Components** (no fetch or server access): migrate to standard component. Do NOT systematically wrap in `createServerFn`.

8. **API routes**: `src/app/api/foo/route.ts` -> `src/routes/api/foo.ts` with `createFileRoute('/api/foo')({ server: { handlers: { GET, POST } } })`. No `route.ts` file in TanStack; the import comes from `@tanstack/react-router` (not `@tanstack/react-start/server`).

9. **`tsc --noEmit` may break during migration**: expected until end of Phase 3. Don't panic, don't patch with `any`. Final validation is in Phase 4.

## Files

- `CONVERSION_TABLE.md` - exhaustive Next.js -> TanStack Start table (load for API details)

## External references

- Official guide: https://tanstack.com/start/latest/docs/framework/react/migrate-from-next-js
- TanStack Intent: https://tanstack.com/intent/registry/@tanstack__react-start
- Scaffolding CLI: https://github.com/sidiDev/next-to-tanstack
- Inngest case study: https://www.inngest.com/blog/migrating-off-nextjs-tanstack-start
- `/goal` doc: https://code.claude.com/docs/en/goal
