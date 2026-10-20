# 0005 — Next.js 16 (App Router) y TypeScript strict

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

El backoffice está detrás de login, así que SEO y performance pública no pesan. El valor que se busca es un stack unificado con la web pública, un BFF seguro (tokens en cookies httpOnly) y buena DX. Además, TypeScript 7 (el compilador nativo en Go) es estable desde agosto de 2026 y compila 8–12 veces más rápido, pero hasta la 7.1 no tendrá API programática, así que herramientas como typescript-eslint y ts-jest todavía no funcionan con él.

## Opciones consideradas

1. Next.js 16 App Router.
2. React SPA con Vite + TanStack Router.
3. Refine o React Admin como base.

## Decisión

Elegimos **Next.js 16 con App Router**:

- Render dinámico por request en rutas autenticadas, sin caché de datos de admin.
- Protección de rutas en `proxy.ts` (antes `middleware.ts`).
- **TypeScript strict.** TS 7 (`tsgo`) se usa para el typecheck en editor y CI, y `typescript@6` queda instalado para las herramientas que necesitan la API del compilador, hasta que salga la 7.1.

## Consecuencias

**Positivas**

- Mismo framework para admin y web: un solo monorepo y una sola forma de trabajar.
- Typecheck mucho más rápido con `tsgo`.

**Negativas / costos**

- La caché de Next tiene trampas en CRUD (datos obsoletos); se mitiga con render dinámico y revalidación explícita.
- Durante un tiempo convivirán dos versiones de TypeScript.

**Pendiente para implementarla**

- [ ] Cambiar `typescript ^5` por `typescript@6` y agregar `@typescript/native-preview` (`tsgo`) en `apps/admin`.
- [ ] Script `typecheck` con `tsgo --noEmit`.
- [ ] Pasar el gate de CI a `tsgo` cuando salga TS 7.1.

## Más información

- [TypeScript 7 released (InfoQ)](https://infoq.com/news/2026/08/typescript-7-released/)
