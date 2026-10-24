# pdance-platform

Monorepo de la nueva plataforma de **Allegra Pole Dance**, una academia de pole dance en Lima. Reemplaza tres sistemas legacy: la web pública en Angular 16, la API en Lumen 8.3 (`kevip/pdance-api`) y el backoffice en Laravel 8.75 (`kevip/pdance-admin`).

Antes de empezar una tarea, lee [`docs/handoff.md`](docs/handoff.md): resume en qué punto está el proyecto y qué falta.

## Estado actual

- **Fase 0 — Discovery:** hecha.
- **Fase 1 — Technical foundation:** en curso. Los ADRs están escritos; falta el scaffold del monorepo.
- Hoy solo existe `apps/admin` (generado con `create-next-app`). `pnpm-workspace.yaml` y `pnpm-lock.yaml` todavía están dentro de `apps/admin/`; se mueven a la raíz en el scaffold.

## Arquitectura objetivo

```
pdance-platform/
├─ apps/
│  ├─ admin/        # Next.js 16 — backoffice (UI + BFF)
│  ├─ web/          # Next.js — web pública
│  └─ api/          # NestJS — única fuente de verdad, única que toca MySQL
├─ packages/
│  ├─ schemas/      # Zod compartido
│  ├─ api-client/   # cliente tipado generado desde OpenAPI
│  ├─ ui/           # componentes y tokens
│  ├─ biome-config/
│  └─ tsconfig/
└─ docs/  (adr/, agents/, handoff.md)
```

La app de matrícula (Expo / React Native) vive en otro repo: `kevip/pdance-mobile`. Consume la API generando su cliente desde el `openapi.json`.

## Stack

| Área | Decisión | ADR |
| --- | --- | --- |
| Dueño de los datos | API NestJS; Next es solo BFF y nunca accede a la DB | [0002](docs/adr/0002-api-nestjs-como-fuente-de-verdad.md) |
| Repo | Monorepo pnpm + Turborepo; mobile aparte | [0003](docs/adr/0003-monorepo-pnpm-turborepo.md) |
| Runtime | Node 24 LTS + pnpm | [0004](docs/adr/0004-runtime-y-package-manager.md) |
| Framework | Next.js 16 App Router, TypeScript strict (TS 7 `tsgo` para typecheck) | [0005](docs/adr/0005-nextjs-y-typescript.md) |
| Datos y estado | Server Components + Server Actions; TanStack Query solo en el calendario; sin estado global | [0006](docs/adr/0006-datos-y-estado-en-el-admin.md) |
| Contrato | Zod → `nestjs-zod` → OpenAPI → cliente tipado | [0007](docs/adr/0007-contrato-de-api-openapi.md) |
| Auth | La API emite el token; Next lo guarda en cookie httpOnly | [0008](docs/adr/0008-autenticacion.md) |
| UI | Tailwind v4 + shadcn/ui + TanStack Table | [0009](docs/adr/0009-ui-tailwind-shadcn.md) |
| Formularios | TanStack Form + Zod | [0010](docs/adr/0010-formularios-tanstack-form-zod.md) |
| Lint y formato | Biome (no ESLint) | [0011](docs/adr/0011-calidad-de-codigo-biome.md) |
| Testing | Vitest + Testing Library (sin MSW); Playwright | [0012](docs/adr/0012-testing.md) |
| Hosting | OVH VPS-1 + Docker Compose + Cloudflare | [0013](docs/adr/0013-hosting-ovh-vps.md) |

## Reglas

- **Next.js nunca accede a MySQL.** Toda lectura y escritura pasa por la API.
- **Los ADRs mandan.** Si una propuesta contradice un ADR, dilo explícitamente. Un ADR aceptado no se reescribe: se crea uno nuevo que lo reemplaza.
- **Flujo de trabajo:** issue → rama → PR con `Closes #<issue>`. Nada de commits directos a `main`.
- **Idioma:** docs en español; código, commits (Conventional Commits), issues y PRs en inglés.
- **Sin secretos en el repo.** Solo `.env.example`.
- **Un solo usuario admin** con acceso total. Los roles múltiples son to-be: no los implementes sin un ADR.
- `apps/admin/AGENTS.md` tiene reglas propias de Next.js 16. Léelo antes de escribir código en esa app.

## Comandos

Mientras no exista el workspace en la raíz, todo corre desde la app:

```bash
cd apps/admin
pnpm dev     # servidor de desarrollo
pnpm build
pnpm lint    # todavía ESLint; pasa a Biome en el scaffold (ADR 0011)
```

## Dónde está cada cosa

| Qué | Dónde |
| --- | --- |
| Decisiones técnicas | [`docs/adr/`](docs/adr/README.md) |
| Vocabulario del dominio | [`GLOSSARY.md`](GLOSSARY.md) |
| Estado y pendientes | [`docs/handoff.md`](docs/handoff.md) |
| Design doc, discovery, benchmark de hosting | Confluence, espacio `ALLEGRA`: <https://allegrapoledance.atlassian.net/wiki/spaces/ALLEGRA/overview> |
| Issues y PRs | GitHub, `kevip/pdance-platform` |

## Agent skills

### Issue tracker

Issues are tracked in GitHub Issues (`kevip/pdance-platform`) via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default vocabulary: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `GLOSSARY.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
