# 0010 — Formularios con TanStack Form y Zod

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

Los formularios del admin (alumnas, clases, matrículas) deben validar con las mismas reglas que la API ([0007](0007-contrato-de-api-openapi.md)) y enviarse mediante Server Actions ([0006](0006-datos-y-estado-en-el-admin.md)).

## Opciones consideradas

1. TanStack Form + Zod.
2. react-hook-form + Zod.
3. Formik.

## Decisión

Elegimos **TanStack Form + Zod**, con los esquemas de `packages/schemas`. Su v1 es estable, es type-safe de punta a punta y encaja con el resto de TanStack (Table y Query).

## Consecuencias

**Positivas**

- La misma regla de validación se aplica en el cliente, en la Server Action y en la API.

**Negativas / costos**

- Tiene menos ejemplos y comunidad que react-hook-form.
