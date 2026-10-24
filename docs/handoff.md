# Handoff

Estado del proyecto para retomar el trabajo en una sesión nueva. Actualízalo al cerrar una tarea que cambie el estado.

**Última actualización:** 2026-10-24

## Dónde estamos

| Fase | Estado |
| --- | --- |
| 0 — Discovery del sistema legacy | Hecha |
| 1 — Technical foundation (ADRs, monorepo, CI, setup de agentes) | En curso: ADRs listos, falta el scaffold |
| 2 — API NestJS | Pendiente |
| 3 — Walking skeleton (login + un CRUD en staging) | Pendiente |
| 4 — Migración por módulo: clases → calendario → alumnas → matrículas | Pendiente |
| 5 — Cutover y apagado del legacy | Pendiente |

## Decisiones tomadas

Todas están en [`docs/adr/`](adr/README.md). Las que más condicionan el trabajo:

- **Una sola API NestJS** reemplaza a Lumen y Laravel; Next.js nunca toca MySQL (0002).
- **Monorepo** con admin, web y API; **mobile en `kevip/pdance-mobile`** (0003).
- **Admin:** Server Components + Server Actions; TanStack Query solo en el calendario; sin estado global (0006).
- **Biome** en lugar de ESLint (0011). **Sin MSW**: las integraciones mockean `api-client` con `vi.mock` (0012).
- **Un solo admin** con acceso total; todo chequeo pasa por `AdminGuard` / `requireAdmin()` (0008).
- **Hosting:** OVH VPS-1 con Docker Compose y Cloudflare (0013).

ADRs todavía **Propuestos**: 0001, 0007 y 0012.

## Pendientes, en orden

1. [ ] Confirmar y aceptar los ADRs 0001, 0007 y 0012.
2. [ ] Crear en GitHub las labels de triage (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`).
3. [ ] Scaffold, en PRs pequeños con issue:
   1. Root workspace: `pnpm-workspace.yaml` y lockfile a la raíz, `package.json` raíz, `turbo.json`, `.nvmrc`; Biome en lugar de ESLint; TS 6 + `tsgo`; `@types/node` 24.
   2. Packages: `tsconfig`, `biome-config`, `schemas`, `api-client`, `ui`.
   3. `apps/api` (NestJS) sobre la MySQL existente, con health check y OpenAPI.
   4. CI en GitHub Actions (typecheck, lint, test, build de lo afectado).
4. [ ] Discovery de `pdance-api`: qué endpoints usa la web pública.
5. [ ] Confirmar precio y región de OVH VPS-1.
6. [ ] Más adelante: regenerar `pdance-mobile` con el SDK de Expo vigente.

## Datos útiles del sistema legacy

- **4 módulos en el backoffice:** clases, calendario, alumnas y matrículas. ~24 rutas en `routes/web.php`. Solo 2 mails (`RegistrationPassword`, `ResetPassword`) y ningún cron.
- **Cupos:** `classes.vacancies` y `classes.enrolled`. Para evitar sobreventa, el incremento tiene que ser atómico en la API: `UPDATE classes SET enrolled = enrolled + 1 WHERE id = ? AND enrolled < vacancies`.
- **Riesgos ya identificados:** rutas que cambian estado vía GET (`eliminar-usuario`, `habilitar-usuario`) y ningún chequeo de rol (cualquier sesión tiene acceso total).
- **Base de datos:** MySQL compartida hoy por Lumen y Laravel. No se migra el esquema.

## Fuera del repo

- **Confluence**, espacio `ALLEGRA`: [Inicio](https://allegrapoledance.atlassian.net/wiki/spaces/ALLEGRA/overview), design doc del backoffice, discovery, benchmark de hosting e índice de ADRs.
- **Repos legacy** (solo lectura): `kevip/pdance-admin`, `kevip/pdance-api`.
