# 0007 — Contrato de API con Zod compartido y OpenAPI

- **Estado:** Aceptado
- **Fecha:** 2026-10-24 (propuesto el 2026-10-19)
- **Decisores:** Kevin Castañeda

## Contexto

La API NestJS ([0002](0002-api-nestjs-como-fuente-de-verdad.md)) la consumen el admin y la web, dentro del monorepo, y la app mobile, en otro repo ([0003](0003-monorepo-pnpm-turborepo.md)). Las mismas validaciones deben aplicarse en la API y en los formularios.

## Opciones consideradas

1. Zod compartido → DTOs de NestJS → OpenAPI → cliente generado.
2. Tipos y cliente escritos a mano.
3. tRPC u oRPC.

## Decisión

Elegimos la **opción 1**:

1. Los esquemas Zod viven en `packages/schemas`.
2. NestJS los usa como DTOs con `nestjs-zod`.
3. `@nestjs/swagger` genera el `openapi.json`.
4. `openapi-typescript` + `openapi-fetch` generan el cliente en `packages/api-client`, que usan admin y web.
5. Mobile genera su propio cliente desde el `openapi.json` publicado.

Se descarta tRPC/oRPC porque acopla más los clientes al servidor y no deja documentación estándar para un repo externo como mobile.

## Consecuencias

**Positivas**

- Cada esquema se define una sola vez y sirve para la API, los formularios y mobile.
- El OpenAPI documenta la API.

**Negativas / costos**

- `nestjs-zod` es una librería de la comunidad. Plan B: un `ZodValidationPipe` propio (~20 líneas).

**Pendiente para implementarla**

- [ ] Crear `packages/schemas` y `packages/api-client`.
- [ ] Publicar `openapi.json` en un endpoint de la API o como artefacto de CI.
