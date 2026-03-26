---
description: "Project auto-detection: automatically scan tech stack and load relevant context on first contact"
---
# Project Auto-Detection

## The Rule

When starting work on a codebase for the first time (or when `context/STATUS.md` has no project details filled in), automatically detect the tech stack and environment. Do not ask the user "what framework are you using?" — figure it out.

## Detection Sequence

Run these checks silently on first contact with a new project:

### 1. Package Manager & Language
| File Found | Means |
|------------|-------|
| `package.json` | Node.js / JavaScript / TypeScript |
| `requirements.txt` or `pyproject.toml` | Python |
| `Cargo.toml` | Rust |
| `go.mod` | Go |
| `Gemfile` | Ruby |
| `composer.json` | PHP |

### 2. Framework (from package.json dependencies or equivalent)
| Dependency | Framework |
|------------|-----------|
| `next` | Next.js |
| `react` (no next) | React SPA |
| `express` | Express.js backend |
| `fastapi` or `flask` or `django` | Python web framework |
| `hono` | Hono |
| `nuxt` | Nuxt (Vue) |
| `svelte` or `@sveltejs/kit` | SvelteKit |

### 3. Database
| File/Dependency Found | Database |
|----------------------|----------|
| `supabase` in deps or `supabase/` dir | Supabase (PostgreSQL) |
| `prisma` in deps or `prisma/` dir | Prisma ORM |
| `drizzle` in deps | Drizzle ORM |
| `mongoose` in deps | MongoDB |
| `pg` or `postgres` in deps | Raw PostgreSQL |
| `better-sqlite3` in deps | SQLite |

### 4. Testing
| File/Dependency Found | Test Framework |
|----------------------|----------------|
| `vitest` in deps | Vitest |
| `jest` in deps | Jest |
| `playwright` in deps | Playwright (E2E) |
| `cypress` in deps | Cypress (E2E) |
| `pytest` in deps | pytest |

### 5. Deployment
| File Found | Platform |
|------------|----------|
| `vercel.json` or `.vercel/` | Vercel |
| `netlify.toml` | Netlify |
| `Dockerfile` | Docker / containerized |
| `fly.toml` | Fly.io |
| `railway.json` or `railway.toml` | Railway |
| `.github/workflows/` | GitHub Actions CI/CD |

### 6. Configuration
| File Found | What It Tells You |
|------------|-------------------|
| `tsconfig.json` | TypeScript enabled — check strict mode |
| `.env.example` or `.env.local.example` | Required environment variables |
| `tailwind.config.*` | Tailwind CSS for styling |
| `.eslintrc.*` or `eslint.config.*` | ESLint for linting |
| `.prettierrc` | Prettier for formatting |

## What to Do After Detection

1. **Update STATUS.md** — Fill in the tech stack section if it's empty.
2. **Load relevant agent context** — If it's a Next.js + Supabase project, Andre's PostgreSQL patterns and Liam's Next.js patterns are immediately relevant.
3. **Check for existing tests** — If a test framework is detected, note the test command (`npm test`, `npm run test:e2e`, etc.).
4. **Check for build command** — Find the build script in package.json or equivalent.
5. **Brief the user** — One sentence: "Detected: Next.js 14 + Supabase + Vitest + Tailwind, deployed on Vercel."

## When to Re-Detect

- When the user switches to a different project directory
- When major dependencies are added or removed
- When the user says "we're using X now" (verify and update)

## What NOT to Do

- Do not ask the user what stack they're using if you can detect it
- Do not assume a stack without checking — always verify files exist
- Do not overwhelm the user with detection details — one summary line is enough
- Do not reconfigure the project based on detection — just load context
