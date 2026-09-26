# vishal-lab

A place for ideas and proofs of concept that don't belong on the portfolio
(`vishal-portfolio`). The first thing in it is the **multi-client OAuth POC**:
an Auth.js server that several front ends sign in through, plus two versions
of the portfolio's client for it.

It was a learning exercise rather than a reference design. The code is kept as it was
when it left the portfolio, and "Known rough edges" below lists what I'd do
differently.

## Layout

| Path | What it is | Runs? |
|---|---|---|
| `apps/auth-server/` | Next.js 15 + Auth.js (next-auth v5 beta) + Prisma/Postgres. Identifies the calling client from its origin and uses that client's GitHub/Google OAuth app. Deployed as `my-oauth-proxy.vercel.app`. Has its own Docusaurus docs in `docs/`. | Yes: `pnpm install`, then `pnpm dev` (port 4000). Needs a `.env` (see `apps/auth-server/docs/docs/configuration.md`). |
| `archive/portfolio-web-client/` | The 2026 Vite + React Router client from `vishal-portfolio/apps/web`: `AuthProvider`, `/signin`, `/account`, callback routes, `AccountMenu`, runtime config schema. | No. Reference only; it imports the portfolio's `@/ui` and `@/config` modules. |
| `archive/portfolio-legacy-client/` | The original CRA client from `vishal-portfolio/apps/portfolio`: sign-in pages (MUI, Toolpad, custom), callbacks, `AuthContext`, debug tools, integration notes. | No. Reference only. |
| `specs/auth-integration/spec.md` | The OpenSpec requirements the web client was built against. | n/a |

## Where it came from

- `apps/auth-server` started as [biyani701/my-oauth-proxy](https://github.com/biyani701/my-oauth-proxy)
  (full history there). It was imported into `vishal-portfolio` on 2026-09-13
  and copied here from `vishal-portfolio@9825961`.
- Both archived clients were copied from `vishal-portfolio@9825961`.
  Their history stays in that repo: `git log -- apps/web/src/auth` and
  `git log -- apps/portfolio/src/components/auth`.

The live deployment is unchanged: `my-oauth-proxy.vercel.app` still serves the
legacy portfolio until it is cut over. That Vercel project still builds from
`vishal-portfolio/apps/auth-server`, not from this repo.

## Known rough edges

- **Client identification by substring.** `identifyClient` in
  `src/auth.config.ts` uses `origin.includes(...)`, so any `*.github.io` site
  counts as the portfolio. An exact allow-list of origins would be safer.
- **CORS.** `vercel.json` sends `Access-Control-Allow-Origin: *` together with
  `Access-Control-Allow-Credentials: true`. Browsers refuse credentialed
  requests with a wildcard origin, so the allowed origin should be echoed from
  the same allow-list.
- **Pre-release auth library.** `next-auth` is pinned to a v5 beta.
- **Monorepo leftovers.** `vercel.json` runs `npx turbo-ignore`, which assumes
  the Turborepo it came from.
