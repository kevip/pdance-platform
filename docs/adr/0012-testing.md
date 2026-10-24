# 0012 — Testing

- **Estado:** Propuesto
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

El monorepo tiene apps Next.js y una API NestJS. Conviene usar un solo runner para todo. Los Server Components async todavía no se prueban bien con unit tests.

## Opciones consideradas

1. Vitest en todo + Testing Library; Playwright para e2e.
2. La opción 1 + MSW para simular la API a nivel de red.
3. Jest (default de NestJS) + Vitest en el front.
4. Cypress para e2e.

## Decisión

Elegimos la **opción 1**:

| Nivel | Herramienta | Qué cubre |
| --- | --- | --- |
| Unit | Vitest | `packages/schemas`, utilidades y servicios de NestJS (con SWC para los decoradores) |
| Integración | Vitest + Testing Library | Pantallas y Client Components; las llamadas a la API se simulan con `vi.mock` sobre `packages/api-client` |
| E2E | Playwright (+ axe) | Login y CRUD críticos; Server Components async |

**MSW queda fuera por ahora.** Con pocas pantallas que llaman a la API desde el cliente (la mayoría lee en Server Components), mockear el módulo `api-client` alcanza. Se reevalúa si esos mocks se vuelven difíciles de mantener.

## Consecuencias

**Positivas**

- Un solo runner y una sola configuración en todo el monorepo.

**Negativas / costos**

- NestJS necesita configurar `unplugin-swc` para que Vitest entienda la metadata de los decoradores.
- Sin MSW, los tests de integración simulan el módulo `api-client` y no la red: si cambia la forma en que se llama al cliente, hay que ajustar los mocks.

**Pendiente para implementarla**

- [ ] Configuración base de Vitest compartida.
- [ ] Playwright con un test de humo del login.
