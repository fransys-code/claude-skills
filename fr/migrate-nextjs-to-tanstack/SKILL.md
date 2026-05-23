---
name: migrate-nextjs-to-tanstack
description: "Migration autonome one-shot Next.js App Router -> TanStack Start. Audit + scaffolding + migration par familles + cleanup + validation finale (tsc + vite build + smoke SSR). Applique TanStack Intent (createServerFn, types inferes, no use server/use client). Compatible /goal execution unattended. Ne pas utiliser pour : React Router seul, Pages Router pre-13, nouveau projet from scratch."
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

# Migration Next.js App Router -> TanStack Start (MODE AUTONOME)

Migre un projet Next.js de bout en bout vers TanStack Start **sans intervention humaine**. Conçu pour des applis de test ou des POC ou une migration unattended est acceptable.

## Mode operatoire

**AUTONOME ONE-SHOT.** Aucune pause de validation, aucune question intermediaire. Tu enchaines les phases dans l'ordre, en commitant a chaque palier. Les checkpoints sont **mecaniques** (tsc, vite build, grep) et leur sortie apparait dans le transcript pour qu'un evaluateur `/goal` puisse prouver la completion.

**Cible.** Si `$ARGUMENTS` est non vide, `cd "$ARGUMENTS"` AVANT toute commande. Sinon, travailler dans le cwd courant. Verifier que le `package.json` contient `"next"` en deps — si non, abort avec un message clair.

Si tu rencontres un cas non couvert (voir "Zones grises") :
1. **NE PAS demander a l'utilisateur.**
2. Appliquer la **fallback strategy** documentee.
3. Ajouter une note dans `MIGRATION_NOTES.md` a la racine pour audit posterieur.
4. Continuer.

## Goal & success criteria (verifiable depuis le transcript)

**Succes prouvable par ces commandes (a executer en fin de migration) :**
- [ ] `grep -r "from ['\"]next/" src/ ; grep -r "from 'next'" src/` ressort **vide**
- [ ] `grep -rE "['\"]use (server|client)['\"]" src/` ressort **vide**
- [ ] `npx tsc --noEmit` ressort **exit 0**
- [ ] `npx vite build` ressort **exit 0**
- [ ] `vite dev` repond `HTTP 200` sur `/` (smoke test SSR runtime)
- [ ] `package.json` ne contient plus `"next"` ni `"@next/*"` dans dependencies
- [ ] Aucun fichier `next.config.*`, `middleware.ts` (Next), `next-env.d.ts` ne subsiste

Toutes ces commandes DOIVENT etre executees en fin de run et leur sortie visible dans le transcript.

## Workflow autonome (enchainement strict)

### Phase 0 - Audit silencieux

```bash
# Verifier branche & working tree
git rev-parse --abbrev-ref HEAD
git status --short

# Si working tree sale -> creer une branche dediee et tout commiter avant
# Si on est sur main/master -> creer refactor/tanstack-migration
```

Si la branche actuelle est `main`/`master` OU si le working tree est sale :
```bash
git checkout -b refactor/tanstack-migration
git add -A && git commit -m "chore: snapshot before TanStack migration" || true
```

Cartographier toutes les routes Next.js (Phase 0 = avant le `git mv`, donc on lit encore `src/app`) :
```bash
find src/app -type f \( -name "page.tsx" -o -name "layout.tsx" -o -name "route.ts" -o -name "route.tsx" -o -name "loading.tsx" -o -name "error.tsx" -o -name "not-found.tsx" -o -name "middleware.ts" \) | sort
grep -rE "['\"]use (server|client)['\"]" src/ -l | sort -u
```

Enregistrer la liste dans le contexte. Pas de demande utilisateur.

### Phase 1 - Scaffolding TanStack

1. Installer les deps (detecter le package manager via `lockfile`) :
   - Si `bun.lockb` : `bun add @tanstack/react-router @tanstack/react-start nitro vite @vitejs/plugin-react @tailwindcss/vite @unpic/react`
   - Si `pnpm-lock.yaml` : `pnpm add ...`
   - Si `yarn.lock` : `yarn add ...`
   - Sinon : `npm install ...`

2. Desinstaller : `next`, `@next/*`, `eslint-config-next` (a faire seulement en Phase 4).

3. Creer `vite.config.ts` avec **ordre critique** :
   ```ts
   import { defineConfig } from 'vite'
   import { tanstackStart } from '@tanstack/react-start/plugin/vite'
   import react from '@vitejs/plugin-react'
   import tailwindcss from '@tailwindcss/vite'

   export default defineConfig({
     server: { port: 3000 },
     resolve: { tsconfigPaths: true },  // alias @/* heritage de Next.js
     plugins: [
       tanstackStart({ srcDirectory: 'src' }),  // routesDirectory: 'src/routes' par defaut
       react(),       // OBLIGATOIRE: APRES tanstackStart()
       tailwindcss(),
     ],
   })
   ```

   Verifier que `tailwindcss` (peer) est bien installe en plus de `@tailwindcss/vite` :
   ```bash
   <pm> add -D tailwindcss @tailwindcss/vite
   ```

4. Creer `src/router.tsx` (boilerplate standard TanStack Start).

5. **Renommer le dossier des routes vers la convention TanStack** :
   ```bash
   git mv src/app src/routes
   ```
   Tout le reste de la migration travaille desormais sur `src/routes/`. Le `routeTree.gen.ts` sera genere automatiquement au premier `vite dev` / `vite build` a partir de ce dossier (defaut TanStack Start).

6. Renommer `src/routes/layout.tsx` -> `src/routes/__root.tsx` (avec `createRootRoute`, `<Outlet />`, `<HeadContent />`, `<Scripts />`, `head()` method pour les metadata).

7. Mettre a jour `package.json` scripts : `dev` -> `vite dev`, `build` -> `vite build`, `start` -> `node .output/server/index.mjs`.

```bash
git add -A
git commit -m "chore(tanstack): scaffold TanStack Start setup"
```

### Phase 2 - Migration par familles (en cascade automatique)

Ordre d'attaque (du plus simple au plus complexe), boucle automatique :

1. **Routes statiques** (`page.tsx` sans dynamic params, sans server action)
2. **Routes dynamiques simples** (`[slug]/page.tsx`)
3. **Routes catch-all** (`[...slug]/page.tsx`)
4. **API routes** (`route.ts`)
5. **Routes avec server actions** (`"use server"`)
6. **Pathless layouts** (`(marketing)`, `(auth)`)
7. **Conventions speciales** (`loading.tsx`, `error.tsx`, `not-found.tsx`)

Pour chaque famille :
1. Lister les routes membres.
2. Appliquer le tableau de conversion (voir `CONVERSION_TABLE.md`).
3. Apres chaque route migree, executer **silencieusement** :
   ```bash
   npx tsc --noEmit 2>&1 | tail -20
   ```
   Si erreur sur la route : la fixer immediatement. Si erreur ailleurs : noter et continuer (sera resolu apres migration des dependances).
4. Apres la famille entiere :
   ```bash
   grep -r "from ['\"]next/" src/routes/<famille>/ || echo "OK: no next/ imports"
   ```
5. Commit atomique :
   ```bash
   git add -A
   git commit -m "refactor(routing): migrate <famille> to TanStack Start"
   ```

**Ne pas attendre de validation entre familles.** Enchainer.

### Phase 3 - Composants partages

1. Remplacer `next/link` par `@tanstack/react-router` Link dans tout `src/components/` :
   ```bash
   grep -rl "from 'next/link'" src/
   ```
   Conversion : `<Link href={...}>` -> `<Link to="..." params={...}>`. Attention aux template literals (`/posts/${id}` -> `to="/posts/$postId" params={{ postId: id }}`).

2. Remplacer `next/navigation` (`useRouter`, `usePathname`, `useSearchParams`) par les hooks TanStack equivalents.

3. Remplacer `next/image` par `@unpic/react` :
   ```bash
   grep -rl "from 'next/image'" src/
   ```

4. Remplacer `next/font` par Fontsource (ajouter import dans `globals.css`).

5. Supprimer tous les `"use server"` et `"use client"` restants. Wrapper dans `createServerFn` ce qui doit etre server-only.

6. Checkpoint :
   ```bash
   grep -rE "['\"]use (server|client)['\"]" src/ && echo "REMAINING DIRECTIVES" || echo "OK"
   grep -r "from ['\"]next/" src/ && echo "REMAINING NEXT IMPORTS" || echo "OK"
   ```

```bash
git add -A
git commit -m "refactor(deps): replace Next.js primitives with TanStack/Unpic/Fontsource"
```

### Phase 4 - Cleanup final

1. Desinstaller Next.js :
   ```bash
   <package-manager> remove next @next/* eslint-config-next 2>/dev/null || true
   ```

2. Supprimer les fichiers residuels :
   ```bash
   rm -f next.config.js next.config.ts next.config.mjs next-env.d.ts
   rm -f middleware.ts src/middleware.ts
   ```

3. Mettre a jour `tsconfig.json` : retirer le plugin `next`, ajuster `jsx`/`moduleResolution` si necessaire.

4. **Validation finale (CRITIQUE - sortie dans le transcript pour /goal) :**
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

   Le smoke test final lance `vite dev` en arriere-plan, attend 8s, fait un GET sur `/` pour prouver que SSR repond, puis tue le port. Sans ca, `vite build` peut passer mais l'app crash au runtime (route tree mal genere, env vars manquantes, erreur d'hydratation).

5. Commit final :
   ```bash
   git add -A
   git commit -m "chore(tanstack): complete Next.js to TanStack Start migration"
   ```

6. **Restituer le rapport final** a l'utilisateur :
   - Branche : `refactor/tanstack-migration`
   - N commits crees
   - Resultat des 5 checkpoints
   - Fichiers ajoutes a `MIGRATION_NOTES.md` (si zones grises rencontrees)

## Regles non negociables (TanStack Intent)

### Modele d'execution
- TanStack Start est **isomorphique par defaut** : tout code tourne client ET serveur sauf si wrappe dans `createServerFn`. C'est l'**INVERSE** de Next.js Server Components.
- **Supprimer** toutes les directives `"use server"` et `"use client"`.
- Tout code serveur (DB, secrets, fs, env vars privees, API keys) DOIT etre dans `createServerFn(...).handler(...)`.

### Types
- Types **entierement inferes** par TanStack Router. **Jamais** caster, **jamais** annoter une valeur inferee (params, search, loader data, context).
- Pas de `any`, pas d'assertions de type pour patcher des erreurs TS post-scaffolding.

### Vite config
- `tanstackStart()` AVANT `react()` dans `vite.config.ts`. Erreur silencieuse sinon.

## Zones grises - Fallback strategies (NE PAS demander a l'utilisateur)

Pour chaque cas, appliquer le fallback ET logger dans `MIGRATION_NOTES.md`.

| Cas Next.js | Fallback strategy |
|---|---|
| Server Components avec streaming | Migrer en composant standard + `Route.loader` avec `Suspense` cote client. Logger : "streaming downgrade" |
| `after()` de `next/server` | Remplacer par `setTimeout(() => fn(), 0)` dans server fn (best-effort). Logger : "after() approximated" |
| Pathless layout groups `(marketing)` | Conserver le dossier, ajouter un fichier `<groupe>.tsx` qui exporte un layout via `createFileRoute` avec `<Outlet />`. Logger : "pathless layout via TanStack layout route" |
| `not-found.tsx` | Utiliser `notFoundComponent` dans `createRootRoute`. Logger : "not-found via root config" |
| `error.tsx` | Utiliser `errorComponent` dans la route. Logger : "error boundary via route config" |
| `loading.tsx` | Utiliser `pendingComponent` dans la route. Logger : "loading via pendingComponent" |
| `middleware.ts` Next | Migrer la logique dans un `createServerFn` middleware OU dans les loaders. Si redirect/rewrite : utiliser `beforeLoad` dans `__root` ou route. Logger : "middleware migrated to <strategy>" |
| ISR / `revalidate` / `unstable_cache` | Pas d'equivalent direct. Implementer cache via `Route.staleTime` + `Route.gcTime`. Logger : "ISR approximated via staleTime" |
| Parallel routes (`@modal`) | Convertir en route simple + state global. Logger : "parallel route flattened" |
| Intercepting routes (`(.)photo/[id]`) | Convertir en route simple. Logger : "intercepting route flattened" |

## Tableau de conversion (succinct - voir CONVERSION_TABLE.md pour les details)

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
| `export const metadata` | `head()` method retournant `{ meta, links }` |

## Gotchas

1. **Ordre Vite plugins** : `tanstackStart()` AVANT `react()`. Test : si HMR ne marche pas ou si les routes ne sont pas detectees, c'est ca.

2. **Chemins avec `(parentheses)`** : toujours quoter en bash. Ex: `git diff "src/routes/(web).tsx"`. Sinon le shell interprete les parentheses et lance une erreur.

3. **`routesDirectory`** : on migre `src/app` -> `src/routes` (defaut TanStack Start) via `git mv` en Phase 1. Pas besoin d'override dans `vite.config.ts`. L'historique git suit le `mv`.

4. **`next/image` -> `@unpic/react`** : quasi drop-in. Verifier les props `fill`, `sizes`, `priority`.

5. **`next/font`** : remplacer `Inter({ subsets: ['latin'] })` par `@fontsource/inter` importe dans `globals.css`.

6. **Metadata** : `export const metadata = { title: ... }` devient :
   ```ts
   export const Route = createFileRoute(...)({
     head: () => ({ meta: [{ title: '...' }] }),
     component: ...
   })
   ```

7. **Server Components passifs** (sans fetch ni acces serveur) : migrer en composant standard. Ne PAS wrapper systematiquement en `createServerFn`.

8. **API routes** : `src/app/api/foo/route.ts` -> `src/routes/api/foo.ts` avec `createFileRoute('/api/foo')({ server: { handlers: { GET, POST } } })`. Pas de fichier `route.ts` dans TanStack ; l'import vient de `@tanstack/react-router` (pas `@tanstack/react-start/server`).

9. **`tsc --noEmit` peut planter pendant la migration** : c'est attendu jusqu'a la fin de Phase 3. Ne pas paniquer, ne pas patcher avec des `any`. La validation finale est en Phase 4.

## Files

- `CONVERSION_TABLE.md` - tableau exhaustif Next.js -> TanStack Start (a charger pour les details d'API)

## References externes

- Guide officiel : https://tanstack.com/start/latest/docs/framework/react/migrate-from-next-js
- TanStack Intent : https://tanstack.com/intent/registry/@tanstack__react-start
- CLI scaffolding : https://github.com/sidiDev/next-to-tanstack
- Case study Inngest : https://www.inngest.com/blog/migrating-off-nextjs-tanstack-start
- Doc `/goal` : https://code.claude.com/docs/en/goal
