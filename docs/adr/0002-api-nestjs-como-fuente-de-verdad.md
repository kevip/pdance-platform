# 0002 — API NestJS como única fuente de verdad

- **Estado:** Aceptado
- **Fecha:** 2026-10-19 (primera versión aceptada el 2026-10-09 con Laravel; ver Historia)
- **Decisores:** Kevin Castañeda

## Contexto

Hoy hay dos aplicaciones escribiendo en la misma base de datos MySQL: la API Lumen 8.3 (`pdance-api`), que usa la web pública, y el backoffice Laravel 8.75 (`pdance-admin`). Ambos frameworks están en versiones sin soporte de seguridad, y Laravel ya no recomienda Lumen para proyectos nuevos. El nuevo stack (admin y web en Next.js, matrícula en React Native) necesita un único dueño de los datos y las reglas de negocio.

## Opciones consideradas

1. **A.** Extender Lumen con endpoints `/admin`.
2. **B.** Que Next.js acceda directo a MySQL (Prisma o Drizzle).
3. **C.** Consolidar Lumen y el backoffice en una API Laravel 12/13.
4. **D.** Consolidar todo en una nueva API NestJS.

## Decisión

Elegimos **D: una sola API NestJS** que reemplaza a Lumen y al backoffice Laravel. Admin, web y mobile son solo consumidores. Next.js actúa como BFF y **nunca accede a la base de datos**.

- Todo el stack queda en TypeScript, y los esquemas Zod se comparten con admin y web (ver [0007](0007-contrato-de-api-openapi.md)).
- El dominio es chico (4 módulos, ~24 rutas, 2 mails, sin crons), así que reescribirlo es abordable.
- El costo de servidor es el mismo que con PHP.

Descartadas: A, porque invierte en un framework sin futuro; B, porque deja dos escritores sobre la misma DB y duplica reglas de negocio.

## Historia

- **2026-10-09:** se aceptó la opción C (Laravel). Reutilizaba los modelos y validaciones del backoffice actual.
- **2026-10-19:** se cambió a D (NestJS) para tener un solo lenguaje y compartir tipos. El principio (una sola API dueña de los datos) no cambió.

## Consecuencias

**Positivas**

- Una sola fuente de verdad: se elimina el riesgo de dos escritores sobre MySQL.
- Tipos y validaciones compartidos de punta a punta.

**Negativas / costos**

- Hay que reescribir rutas, validaciones y los mails `RegistrationPassword` y `ResetPassword`.
- Se pierde lo que Laravel daba ya resuelto (auth, policies, mail).

**Pendiente para implementarla**

- [ ] Crear `apps/api` (NestJS) sobre la MySQL existente, sin migrar el esquema.
- [ ] Discovery de `pdance-api`: endpoints que usa la web pública.
- [ ] Reemplazar las rutas que cambian estado vía GET (`eliminar-usuario`, `habilitar-usuario`) por `POST`/`DELETE`.

## Más información

- Design doc y Discovery en el espacio de Confluence `ALLEGRA`.
- [Lumen — installation](https://github.com/laravel/lumen-docs/blob/11.x/installation.md)
