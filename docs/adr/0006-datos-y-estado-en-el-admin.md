# 0006 — Datos y estado en el admin

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

Con App Router hay dos formas de manejar datos: Server Components + Server Actions, o Client Components con TanStack Query. Usar las dos en todo el proyecto duplica la caché y la manera de cargar datos. Además, el admin tiene un solo usuario y pocos datos compartidos entre pantallas.

## Opciones consideradas

1. Server Components + Server Actions como base; TanStack Query solo por pantalla.
2. TanStack Query en todo (patrón SPA).
3. Agregar una librería de estado global (Zustand o Redux).

## Decisión

Elegimos la **opción 1**:

| Tipo de estado | Dónde vive |
| --- | --- |
| Lecturas | Server Components |
| Escrituras | Server Actions + `revalidatePath` |
| Filtros, paginación, mes del calendario | URL con `nuqs` |
| Pantallas muy interactivas (calendario) | TanStack Query |
| Formularios | TanStack Form ([0010](0010-formularios-tanstack-form-zod.md)) |
| UI local | `useState` |
| UI compartida (sidebar, tema) | Cookie + Context |

**No se usa librería de estado global.**

## Consecuencias

**Positivas**

- Menos código y menos JavaScript en el cliente.
- El estado en la URL se puede compartir y sobrevive a un refresh.

**Negativas / costos**

- Hay que decidir pantalla por pantalla cuándo justifica TanStack Query.

**Pendiente para implementarla**

- [ ] Instalar `nuqs` y `@tanstack/react-query` (este último solo cuando llegue el calendario).
