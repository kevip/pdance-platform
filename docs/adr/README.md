# Architecture Decision Records (ADRs)

Cada decisión técnica relevante del proyecto queda registrada aquí, en formato [MADR](https://adr.github.io/madr/). Este directorio es la **fuente de verdad**; Confluence solo mantiene un índice que enlaza a estos archivos.

## Cómo agregar un ADR

1. Copia [`template.md`](template.md) como `NNNN-titulo-en-kebab-case.md` con el siguiente número libre.
2. Créalo con estado **Propuesto** y ábrelo en un PR.
3. Al aprobar el PR, cambia el estado a **Aceptado**.
4. Un ADR aceptado no se reescribe: si la decisión cambia, crea uno nuevo y marca el anterior como **Reemplazado por NNNN**.

## Índice

| ADR | Decisión | Estado |
| --- | --- | --- |
| [0001](0001-registrar-decisiones-con-adrs.md) | Registrar decisiones con ADRs (MADR) | Propuesto |
| [0002](0002-api-nestjs-como-fuente-de-verdad.md) | API NestJS como única fuente de verdad; Next como BFF | Aceptado |
| [0003](0003-monorepo-pnpm-turborepo.md) | Monorepo pnpm + Turborepo; mobile en repo aparte | Aceptado |
| [0004](0004-runtime-y-package-manager.md) | Node LTS + pnpm | Aceptado |
| [0005](0005-nextjs-y-typescript.md) | Next.js 16 App Router + TypeScript strict | Aceptado |
| [0006](0006-datos-y-estado-en-el-admin.md) | Server Components + Server Actions; sin estado global | Aceptado |
| [0007](0007-contrato-de-api-openapi.md) | Contrato con Zod compartido + OpenAPI | Propuesto |
| [0008](0008-autenticacion.md) | Token emitido por la API, cookie httpOnly en Next | Aceptado |
| [0009](0009-ui-tailwind-shadcn.md) | Tailwind v4 + shadcn/ui + TanStack Table | Aceptado |
| [0010](0010-formularios-tanstack-form-zod.md) | TanStack Form + Zod | Aceptado |
| [0011](0011-calidad-de-codigo-biome.md) | Biome + Conventional Commits | Aceptado |
| [0012](0012-testing.md) | Vitest + Testing Library + MSW; Playwright | Propuesto |
| [0013](0013-hosting-ovh-vps.md) | OVH VPS-1 + Docker Compose + Cloudflare | Aceptado |
