# 0001 — Registrar decisiones con ADRs

- **Estado:** Propuesto
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

La migración de Allegra reemplaza tres sistemas (web Angular, API Lumen y backoffice Laravel) y toma muchas decisiones técnicas en poco tiempo. Además se trabaja con asistentes de IA (Claude) que necesitan leer el porqué de cada decisión desde el repo, no desde la memoria de una conversación.

## Opciones consideradas

1. ADRs en Markdown dentro del repo (`docs/adr/`), formato MADR.
2. Páginas en Confluence o Notion.
3. Sin registro formal (decisiones en PRs y chats).

## Decisión

Elegimos **ADRs en Markdown en `docs/adr/`, formato MADR**, porque viajan con el código en cada PR y los agentes de IA los leen igual que cualquier archivo. Confluence mantiene solo un índice que enlaza aquí, para no tener dos copias.

## Consecuencias

**Positivas**

- El historial de decisiones queda versionado junto al código.
- Claude Code y otros agentes pueden citarlos al proponer cambios.

**Negativas / costos**

- Hay que mantener actualizado el índice de Confluence a mano.

**Pendiente para implementarla**

- [x] Plantilla en [`template.md`](template.md) e índice en [`README.md`](README.md).
- [ ] Skill `new-adr` en `.claude/skills/` para crear ADRs con la plantilla.
- [ ] Enlazar cada ADR desde el índice de Confluence.

## Más información

- [MADR](https://adr.github.io/madr/)
