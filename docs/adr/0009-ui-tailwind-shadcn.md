# 0009 — UI con Tailwind CSS v4, shadcn/ui y TanStack Table

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

El backoffice es sobre todo tablas, formularios y un calendario. Se quiere renovar el look and feel con control total sobre los componentes y compartir tokens de diseño con la web pública.

## Opciones consideradas

1. Tailwind CSS v4 + shadcn/ui (Radix) + TanStack Table.
2. MUI.
3. Mantine.

## Decisión

Elegimos **Tailwind CSS v4 + shadcn/ui + TanStack Table**. Los tokens y componentes compartidos viven en `packages/ui`.

## Consecuencias

**Positivas**

- Los componentes de shadcn se copian al repo y se modifican libremente.
- Radix ya resuelve teclado y ARIA.
- El data-table de shadcn ya está construido sobre TanStack Table.

**Negativas / costos**

- Las actualizaciones de shadcn no llegan solas: hay que traerlas a mano.

**Pendiente para implementarla**

- [x] Tailwind v4 instalado en `apps/admin`.
- [ ] Inicializar shadcn/ui y crear `packages/ui`.
