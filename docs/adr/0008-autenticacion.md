# 0008 — Autenticación: token emitido por la API

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

Hay un solo usuario admin (un rol `viewer` de solo lectura queda como to-be). La app mobile también necesitará login de alumnas contra la misma API. Auth.js lo mantiene desde septiembre de 2025 el equipo de Better Auth, solo con parches de seguridad, y ellos recomiendan Better Auth para proyectos nuevos.

## Opciones consideradas

1. Auth en la API; Next solo guarda el token.
2. Auth.js en Next.
3. Better Auth en Next.

## Decisión

Elegimos la **opción 1**: **la API NestJS emite el token** y Next lo guarda en una cookie httpOnly + Secure + SameSite=Lax. La sesión se verifica en `proxy.ts` y en la API. Si el auth en la API crece, se usará Better Auth dentro de NestJS, que tiene integración oficial; eso queda fuera del alcance del admin.

Poner el auth en Next crearía dos sistemas de usuarios, porque mobile también se autentica contra la API.

**Autorización:** todo chequeo pasa por un único punto (`AdminGuard` en NestJS y `requireAdmin()` en Next), que hoy solo verifica que haya sesión. Agregar roles más adelante cambia ese punto, no las pantallas.

## Consecuencias

**Positivas**

- Un solo sistema de usuarios para admin y mobile.
- El token nunca llega al JavaScript del navegador.

**Negativas / costos**

- El admin depende de que la API esté disponible para hacer login.

**Pendiente para implementarla**

- [ ] Crear el único admin con un seed o comando, sin registro público.

## Más información

- [Auth.js joins Better Auth](https://www.better-auth.com/blog/authjs-joins-better-auth)
- [Better Auth — NestJS integration](https://better-auth.com/docs/integrations/nestjs)
