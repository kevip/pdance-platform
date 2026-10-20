# 0004 — Runtime Node LTS y pnpm

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

En producción, Next.js (modo `standalone`) y NestJS corren sobre Node. Hace falta fijar versiones para que local, CI y el VPS se comporten igual.

## Opciones consideradas

1. pnpm.
2. npm.
3. Bun como package manager y/o runtime.

## Decisión

Elegimos **Node LTS + pnpm**.

- **Node:** 24 LTS hoy; pasar a 26 cuando se estabilice su LTS (entra en octubre de 2026).
- **pnpm:** fijado con `packageManager` en el `package.json` raíz (hoy `pnpm@12.14.0`).
- Versiones forzadas con `.nvmrc` y `engines`, y verificadas en CI.

Bun se descarta: el runtime de producción es Node igual, y la ganancia de velocidad al instalar es mínima en un proyecto de este tamaño.

## Consecuencias

**Positivas**

- pnpm + Turborepo es la combinación más probada para monorepos.
- Instalaciones deterministas y con poco disco gracias al store de pnpm.

**Negativas / costos**

- Hay que actualizar Node a mano cuando cambie el LTS.

**Pendiente para implementarla**

- [ ] `.nvmrc` con `24` y `engines.node` en el `package.json` raíz.
- [ ] Mover `packageManager` de `apps/admin/package.json` al `package.json` raíz.
- [ ] Actualizar `@types/node` de `^20` a `^24` en `apps/admin`.

## Más información

- [Evolving the Node.js Release Schedule](https://nodejs.org/en/blog/announcements/evolving-the-nodejs-release-schedule)
