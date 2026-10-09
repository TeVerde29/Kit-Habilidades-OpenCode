# Skill: buenas-practicas-playwright

Guía completa de pruebas Playwright: escribir E2E,
componente, API, GraphQL, visuales, accesibilidad,
seguridad y más. Incluye depuración de flaky tests,
Page Object Model, CI/CD, mocks de red y terceros,
móvil, multi-tab, frameworks (React, Angular, Vue,
Next.js) e infra (Docker, sharding, reportes).

## Uso

- El SKILL.md es una guía por actividad: cada tabla
  dice qué archivos de referencia consultar según
  lo que hagas (escribir, depurar, errores,
  arquitectura, refactor, infra, patrones).
- El árbol de decisión rápida lleva del "qué estoy
  haciendo" al archivo exacto.
- Rutas de referencia intactas en `core/`,
  `advanced/`, `browser-apis/`, `debugging/`,
  `testing-patterns/`, `architecture/`,
  `frameworks/`, `infrastructure-ci-cd/`.

## Ciclo de validación

1. `npx playwright test --reporter=list`.
2. Si falla: revisa error y traza
   (`npx playwright show-trace`), corrige
   locators, esperas o aserciones, repite.
3. Sigue solo cuando todo pase.
4. Repite críticas: `npx playwright test --repeat-each=5`.
