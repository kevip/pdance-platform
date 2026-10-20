# 0012 — Testing

- **Estado:** Propuesto
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

El monorepo tiene apps Next.js y una API NestJS. Conviene usar un solo runner para todo. Los Server Components async todavía no se prueban bien con unit tests.

## Opciones consideradas

1. Vitest en todo + Testing Library + MSW; Playwright para e2e.
2. Jest (default de NestJS) + Vitest en el front.
3. Cypress para e2e.

## Decisión

Elegimos la **opción 1**:

| Nivel | Herramienta | Qué cubre |
| --- | --- | --- |
| Unit | Vitest | `packages/schemas`, utilidades y servicios de NestJS (con SWC para los decoradores) |
| Integración | Vitest + Testing Library + MSW | Pantallas y Client Components |
| E2E | Playwright (+ axe) | Login y CRUD críticos; Server Components async |

## Consecuencias

**Positivas**

- Un solo runner y una sola configuración en todo el monorepo.

**Negativas / costos**

- NestJS necesita configurar `unplugin-swc` para que Vitest entienda la metadata de los decoradores.

**Pendiente para implementarla**

- [ ] Configuración base de Vitest compartida.
- [ ] Playwright con un test de humo del login.
