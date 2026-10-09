# kit-habilidades-opencode

Colección personal de **skills** para agentes de código + configuración de **MCP** para [OpenCode](https://opencode.ai).

## Contenido

```
kit-habilidades-opencode/
  skills/          # 25 skills (cada una con su SKILL.md)
  opencode.json    # configuración MCP (5 servidores, sin secretos)
  README.md
  .gitignore
```

## Skills (25)

| Skill | Qué hace |
|---|---|
| `angular-developer` | Código Angular y guía de arquitectura: señales, formularios, DI, routing, SSR, ARIA, testing, CLI |
| `apple-design` | Diseño estilo Apple para web: motion físico, springs, sheets, materiales translúcidos, tipografía |
| `architecture-blueprint-generator` | Genera blueprints de arquitectura: detecta stack, patrones, diagramas C4/UML, ADRs |
| `audit-website` | Audita un sitio con `squirrelscan` (SEO, performance, seguridad, a11y, 260+ reglas) y corrige en bucle |
| `caveman-commit` | Commit rápido del día: mensaje terso en español + frenos mínimos |
| `caveman-deploy` | Despliegue amplio: hooks pre-commit, validación, commit en español con contexto y push |
| `caveman-qa` | Planes de prueba pragmáticos para pymes: alcance, casos con evidencia, incidencias y veredicto |
| `codebase-design` | Vocabulario de diseño de módulos profundos: interfaces pequeñas, seams limpios, testabilidad |
| `convert-documents-to-markdown` | Convierte Word, PowerPoint, Excel, PDF, EPUB, CSV a Markdown con `anydoc` |
| `diagnosing-bugs` | Diagnóstico de bugs difíciles y regresiones de rendimiento, con redacción de secretos |
| `find-skills` | Descubre e instala skills del ecosistema abierto de agentes |
| `frontend-design` | Dirección de diseño distintiva: paleta, tipografía y layout con punto de vista propio |
| `java-springboot` | Buenas prácticas Spring Boot: starters, estructura por dominio, configuración |
| `javascript-typescript-jest` | Tests JS/TS con Jest: estructura, mocking, patrones comunes |
| `nodejs-backend-patterns` | Backends Node producción con Express/Fastify: middleware, errores, auth, API REST/GraphQL |
| `playwright-best-practices` | Playwright E2E/componentes/API/visual/a11y: POM, flaky tests, auth, CI/CD, mocks |
| `readme-blueprint-generator` | Genera READMEs completos desde `.github/copilot` y `copilot-instructions.md` |
| `redesign-existing-projects` | Eleva sitios/apps existentes a calidad premium sin romper funcionalidad |
| `repo-intake-and-plan` | Intake Rigor: mapa rápido del repo y plan mínimo de reproducción (inference/eval/train) |
| `security-and-hardening` | Endurece código contra OWASP Top 10: inputs, auth, sesiones, dependencias, GDPR/CCPA |
| `spring-boot-testing` | Testing Spring Boot 4 con JUnit 6, AssertJ e Instancio |
| `sql-optimization` | Optimización SQL universal (MySQL, PostgreSQL, SQL Server, Oracle): planes, índices, paginación |
| `tdd` | TDD red-green-refactor: tests de comportamiento por interfaces públicas |
| `ui-ux-pro-max` | Inteligencia UI/UX: estilos, paletas, fuentes, guías UX, iconos, presets GSAP, stacks |
| `verification-before-completion` | Verificación antes de declarar done: evidencia ejecutada antes de afirmar |

## MCP (`opencode.json`)

5 servidores, sin secretos:

| Servidor | Tipo | Comando / URL |
|---|---|---|
| `brave-devtools` | local | `npx -y brave-mcp@latest` |
| `context7` | remote | `https://mcp.context7.com/mcp` |
| `playwright` | local | `npx -y @playwright/mcp@latest` |
| `dbhub` | local (demo) | `npx -y @bytebase/dbhub@latest --transport stdio --demo` |
| `chrome-devtools` | local | `npx -y chrome-devtools-mcp@latest` |

## Instalación

```bash
# 1. Clonar
git clone https://github.com/TeVerde29/kit-habilidades-opencode.git

# 2a. OpenCode: copiar opencode.json a tu config
cp opencode.json ~/.config/opencode/opencode.json
# (Windows PowerShell: Copy-Item opencode.json $env:USERPROFILE\.config\opencode\opencode.json)

# 2b. Skills: copiar las que uses a tu carpeta de skills
cp -r skills/* ~/.agents/skills/
# (o solo algunas: cp -r skills/tdd skills/security-and-hardening ~/.agents/skills/)
```

## Lo que NO se sube (y por qué)

* `service.json` — credencial local de OpenCode Desktop (`OPENCODE_SERVER_PASSWORD`, basic auth user `opencode` para `opencode serve` en `127.0.0.1`). Si se expone, borrar y reiniciar OpenCode Desktop para regenerar.
* `node_modules/`, `package-lock.json`, `bun.lock`, `package.json` — dependencias locales, no aportan nada al repo.
* `.env*` — variables de entorno, posible fuente de secretos.

Ver `.gitignore`.

## Auditoría de secretos

Antes de cada push se verifica:

```powershell
# Buscar secretos reales en lo que se sube (debe dar 0 o solo ejemplos genéricos)
Select-String -Recurse -Path skills,opencode.json -Pattern 'sk-[A-Za-z0-9]|ghp_|xoxb|AKIA|password\s*[:=]\s*[''"][^''"]+[''"]' | Select-Object Path,LineNumber,Line
```

Estado actual: **limpio**. Los matches de `password|secret|token|api_key` en los skills son solo documentación genérica y ejemplos. El único email es `test@gmail.com` de ejemplo en `playwright-best-practices`.
