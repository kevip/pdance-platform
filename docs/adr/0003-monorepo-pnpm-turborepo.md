# 0003 — Monorepo con pnpm y Turborepo; mobile en repo aparte

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

Admin, web y API son TypeScript y comparten esquemas, cliente de API y componentes. La app de matrícula (Expo / React Native) también consume la API, pero tiene su propio ciclo de builds (EAS) y Metro, el bundler de React Native, suele necesitar ajustes con los symlinks de pnpm.

## Opciones consideradas

1. Monorepo con todo, incluido mobile.
2. Monorepo para admin, web y API; mobile en repo aparte.
3. Un repo por app (polyrepo).

## Decisión

Elegimos **un monorepo `pdance-platform` con pnpm workspaces + Turborepo** para admin, web y API, y **mobile en su propio repo (`pdance-mobile`)**.

```
pdance-platform/
├─ apps/
│  ├─ admin/        # Next.js 16 — backoffice
│  ├─ web/          # Next.js — web pública
│  └─ api/          # NestJS — fuente de verdad
├─ packages/
│  ├─ schemas/      # Zod compartido
│  ├─ api-client/   # cliente tipado desde OpenAPI
│  ├─ ui/           # componentes y tokens
│  ├─ biome-config/
│  └─ tsconfig/
├─ docs/  (adr/, specs/)
├─ pnpm-workspace.yaml   # en la raíz
├─ turbo.json
└─ docker-compose.yml
```

Mobile genera su propio cliente desde el `openapi.json` que publica la API (ver [0007](0007-contrato-de-api-openapi.md)), sin necesidad de publicar paquetes npm.

## Consecuencias

**Positivas**

- Un cambio de contrato rompe el typecheck de API, admin y web en el mismo PR.
- Turborepo construye y prueba solo lo afectado.
- Mobile queda con la configuración estándar de Expo.

**Negativas / costos**

- Mobile no detecta cambios de la API hasta regenerar su cliente.

**Pendiente para implementarla**

- [x] Mover `admin/` a `apps/admin/` (commit `f2c128a`).
- [ ] Mover `pnpm-workspace.yaml` y `pnpm-lock.yaml` de `apps/admin/` a la raíz, y crear el `package.json` raíz.
- [ ] Agregar `turbo.json`, `apps/api` y los `packages/` base.
- [ ] Regenerar `pdance-mobile` con el SDK de Expo vigente cuando empiece esa fase.
