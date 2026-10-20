# 0011 — Calidad de código con Biome

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

Se necesita lint y formato consistentes en todo el monorepo, rápidos en CI y en los hooks de pre-commit. ESLint depende de la API de TypeScript (vía typescript-eslint), que bloquea el paso a TS 7 hasta la 7.1 ([0005](0005-nextjs-y-typescript.md)).

## Opciones consideradas

1. Biome (lint + formato).
2. ESLint 9 + Prettier.
3. Oxlint como pre-paso, junto con ESLint.

## Decisión

Elegimos **Biome** para lint y formato, con la configuración compartida en `packages/biome-config`. Se complementa con:

- Conventional Commits validados con commitlint.
- lefthook + lint-staged en pre-commit.

## Consecuencias

**Positivas**

- Un solo binario rápido para lint y formato.
- No bloquea la adopción de TS 7.
- Cubre las reglas de hooks de React (`useExhaustiveDependencies`) y reglas de accesibilidad.

**Negativas / costos**

- No cubre todas las reglas de `eslint-plugin-next` ni el plugin de TanStack Query.
- El ecosistema de plugins es más chico que el de ESLint.

**Pendiente para implementarla**

- [ ] Quitar `eslint`, `eslint-config-next` y `eslint.config.mjs` de `apps/admin`.
- [ ] Agregar `biome.json` en la raíz y el script `lint` con Biome.
- [ ] Configurar commitlint y lefthook.

## Más información

- [Biome vs ESLint vs Oxlint 2026](https://www.pkgpulse.com/guides/biome-vs-eslint-vs-oxlint-2026)
