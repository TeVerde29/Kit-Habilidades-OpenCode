# kit-habilidades-opencode

Colección personal de **skills** para agentes de código + configuración de **MCP** para [OpenCode](https://opencode.ai).

## Contenido

```
kit-habilidades-opencode/
  skills/          # 27 skills (cada una con su SKILL.md)
  opencode.json    # configuración MCP (5 servidores, sin secretos)
  README.md
  .gitignore
```

## Skills (27)

| Skill | Qué hace |
|---|---|
| `analisis-repo-y-plan` | Intake Rigor: mapa rápido del repo y plan mínimo de reproducción (inference/eval/train) |
| `auditoria-codigo` | Auditoría de código solo lectura: espagueti, estructura, seguridad e informe con archivo:línea |
| `auditoria-sitio-web` | Audita un sitio con `squirrelscan` (SEO, performance, seguridad, a11y, 260+ reglas) y corrige en bucle |
| `buenas-practicas-playwright` | Playwright E2E/componentes/API/visual/a11y: POM, flaky tests, auth, CI/CD, mocks |
| `buscar-habilidades` | Descubre e instala skills del ecosistema abierto de agentes |
| `commit-rapido` | Commit rápido del día: mensaje terso en español + frenos mínimos |
| `convertir-documentos-markdown` | Convierte Word, PowerPoint, Excel, PDF, EPUB, CSV a Markdown con `anydoc` |
| `desarrollador-angular` | Código Angular y guía de arquitectura: señales, formularios, DI, routing, SSR, ARIA, testing, CLI |
| `design-ricra` | Sistema de diseño Tea Green: tipografía, paleta, temas claro/oscuro, tokens CSS y contraste |
| `despliegue-seguro` | Despliegue amplio: hooks pre-commit, validación, commit en español con contexto y push |
| `diagnosticar-errores` | Diagnóstico de bugs difíciles y regresiones de rendimiento, con redacción de secretos |
| `diseno-apple` | Diseño estilo Apple para web: motion físico, springs, sheets, materiales translúcidos, tipografía |
| `diseno-frontend` | Dirección de diseño distintiva: paleta, tipografía y layout con punto de vista propio |
| `diseno-modulos` | Vocabulario de diseño de módulos profundos: interfaces pequeñas, seams limpios, testabilidad |
| `generador-arquitectura` | Genera blueprints de arquitectura: detecta stack, patrones, diagramas C4/UML, ADRs |
| `generador-readme` | Genera READMEs completos desde `.github/copilot` y `copilot-instructions.md` |
| `java-springboot` | Buenas prácticas Spring Boot: starters, estructura por dominio, configuración |
| `mejorar-diseno-existente` | Eleva sitios/apps existentes a calidad premium sin romper funcionalidad |
| `optimizacion-sql` | Optimización SQL universal (MySQL, PostgreSQL, SQL Server, Oracle): planes, índices, paginación |
| `patrones-backend-nodejs` | Backends Node producción con Express/Fastify: middleware, errores, auth, API REST/GraphQL |
| `plan-pruebas-pymes` | Planes de prueba pragmáticos para pymes: alcance, casos con evidencia, incidencias y veredicto |
| `pruebas-jest` | Tests JS/TS con Jest: estructura, mocking, patrones comunes |
| `pruebas-springboot` | Testing Spring Boot 4 con JUnit 6, AssertJ e Instancio |
| `seguridad-y-blindaje` | Endurece código contra OWASP Top 10: inputs, auth, sesiones, dependencias, GDPR/CCPA |
| `tdd` | TDD red-green-refactor: tests de comportamiento por interfaces públicas |
| `ui-ux-pro-max` | Inteligencia UI/UX: estilos, paletas, fuentes, guías UX, iconos, presets GSAP, stacks |
| `verificacion-final` | Verificación antes de declarar done: evidencia ejecutada antes de afirmar |

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
