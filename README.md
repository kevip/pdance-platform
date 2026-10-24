# pdance-platform

Nueva plataforma de **Allegra Pole Dance**, una academia de pole dance en Lima. Reemplaza tres sistemas legacy: la web pública en Angular, la API en Lumen y el backoffice en Laravel.

| App | Stack | Estado |
| --- | --- | --- |
| `apps/admin` | Next.js 16 — backoffice | Generada, sin funcionalidad |
| `apps/api` | NestJS — fuente de verdad | Pendiente |
| `apps/web` | Next.js — web pública | Pendiente |

La app de matrícula (Expo / React Native) vive en [`kevip/pdance-mobile`](https://github.com/kevip/pdance-mobile).

## Requisitos

- **Node.js 24 LTS**
- **pnpm**, en la versión fijada en `packageManager` (hoy `pnpm@12.14.0`). Con Corepack se activa sola: `corepack enable`.
- **Git**. En Windows funciona igual; el repo no usa symlinks.

## Levantar el proyecto

```bash
git clone https://github.com/kevip/pdance-platform.git
cd pdance-platform
corepack enable
```

Mientras no exista el workspace en la raíz (se agrega en el scaffold de la Fase 1), cada app se levanta desde su carpeta:

```bash
cd apps/admin
pnpm install
pnpm dev        # http://localhost:3000
```

## Trabajar con Claude Code

El repo trae la configuración compartida, así que cualquier persona que lo clone obtiene el mismo entorno de agente.

| Archivo | Qué configura |
| --- | --- |
| `AGENTS.md` / `CLAUDE.md` | Contexto del proyecto, reglas y dónde está cada doc. El agente los lee en cada sesión |
| `.claude/settings.json` | Plugin [mattpocock-skills](https://github.com/mattpocock/skills) y bloqueo de lectura de archivos `.env` |
| `.mcp.json` | Servidor MCP [Context7](https://context7.com), para consultar documentación actualizada de las librerías |
| `docs/agents/` | Cómo usan las skills el issue tracker, las labels de triage y los docs del dominio |

### Primera vez

1. Instala [Claude Code](https://code.claude.com/docs) y ábrelo **en la raíz del repo**.
2. Acepta el aviso de **confianza en la carpeta**. Hasta que lo hagas, Claude Code no activa el plugin ni el MCP declarados por el repo.
3. Acepta la instalación del plugin `mattpocock-skills` y del servidor `context7` cuando te lo ofrezca.
4. Comprueba que todo cargó:
   - `/mcp` debe listar `context7`.
   - Al escribir `/mattpocock-skills:` deben aparecer las skills (`to-tickets`, `grill-with-docs`, `pr`, etc.).
5. Instala la [CLI de GitHub](https://cli.github.com) y autentícate con `gh auth login`. Las skills la usan para leer y crear issues.

Tus preferencias personales van en `.claude/settings.local.json`, que no se commitea.

### Opcional: Confluence

Los docs de lectura (design doc, discovery, benchmark de hosting) están en Confluence. Para que el agente pueda consultarlos, conecta el MCP de Atlassian con tu propia cuenta:

```bash
claude mcp add --transport http atlassian https://mcp.atlassian.com/v1/mcp/authv2
```

Después ejecuta `/mcp` dentro de Claude Code para iniciar sesión.

### Por dónde empezar

1. Lee [`docs/handoff.md`](docs/handoff.md): estado del proyecto y próximos pasos.
2. Si no sabes qué skill usar, ejecuta `/mattpocock-skills:ask-matt` y describe lo que quieres hacer.

## Flujo de trabajo

- Todo cambio parte de un **issue** en GitHub.
- Una rama por issue y un **PR** con `Closes #<issue>` en la descripción. Nada de commits directos a `main`.
- Commits en inglés con [Conventional Commits](https://www.conventionalcommits.org).
- Si un cambio contradice un ADR, se escribe un ADR nuevo antes de implementarlo.

## Documentación

| Qué | Dónde |
| --- | --- |
| Decisiones técnicas (ADRs) | [`docs/adr/`](docs/adr/README.md) |
| Vocabulario del dominio | [`GLOSSARY.md`](GLOSSARY.md) |
| Estado y pendientes | [`docs/handoff.md`](docs/handoff.md) |
| Design doc, discovery, benchmark de hosting | [Confluence — espacio ALLEGRA](https://allegrapoledance.atlassian.net/wiki/spaces/ALLEGRA/overview) |

## Licencia

[MIT](LICENSE)
