---
name: buenas-practicas-playwright
description: Úsala al escribir pruebas Playwright, corregir pruebas flaky, depurar fallos, implementar Page Object Model, configurar CI/CD, optimizar rendimiento, mockear APIs, manejar autenticación u OAuth, probar accesibilidad (axe-core), subida/descarga de archivos, mocks de fecha/hora, WebSockets, geolocalización, permisos, flujos multi-tab/popup, layouts móvil/responsive, gestos táctiles, GraphQL, manejo de errores, modo offline, colaboración multi-usuario, servicios de terceros (pagos, verificación de email), monitoreo de errores de consola, setup/teardown global, anotaciones de prueba (skip, fixme, slow), tags de prueba (@smoke, @fast, @critical, filtrado con --grep), dependencias de proyecto, pruebas de seguridad (XSS, CSRF, auth), presupuestos de rendimiento (Web Vitals, Lighthouse), iframes, pruebas de componentes, canvas/WebGL, service workers/PWA, cobertura, i18n/localización, apps Electron o extensiones de navegador. Cubre pruebas E2E, componente, API, visuales, accesibilidad, seguridad, Electron y extensiones. | Use when writing Playwright tests, fixing flaky tests, debugging failures, implementing Page Object Model, configuring CI/CD, optimizing performance, mocking APIs, handling authentication or OAuth, testing accessibility (axe-core), file uploads/downloads, date/time mocking, WebSockets, geolocation, permissions, multi-tab/popup flows, mobile/responsive layouts, touch gestures, GraphQL, error handling, offline mode, multi-user collaboration, third-party services (payments, email verification), console error monitoring, global setup/teardown, test annotations (skip, fixme, slow), test tags (@smoke, @fast, @critical, filtering with --grep), project dependencies, security testing (XSS, CSRF, auth), performance budgets (Web Vitals, Lighthouse), iframes, component testing, canvas/WebGL, service workers/PWA, test coverage, i18n/localization, Electron apps, or browser extension testing. Covers E2E, component, API, visual, accessibility, security, Electron, and extension testing.
license: MIT
metadata:
  author: currents.dev
  version: "1.2"
---

# Buenas prácticas de Playwright

Esta skill trae guía completa para todo el desarrollo de pruebas Playwright, desde escribir pruebas nuevas hasta depurar y mantener suites existentes.

## Guía de referencia por actividad

Consulta estas referencias según lo que estés haciendo:

### Escribir pruebas nuevas

**Cuándo usarla**: al crear archivos de prueba nuevos, escribir casos de prueba, implementar escenarios

| Actividad                                | Archivos de referencia                                                                                                                    |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **Escribir pruebas E2E**                 | [test-suite-structure.md](core/test-suite-structure.md), [locators.md](core/locators.md), [assertions-waiting.md](core/assertions-waiting.md) |
| **Escribir pruebas de componentes**      | [component-testing.md](testing-patterns/component-testing.md), [test-suite-structure.md](core/test-suite-structure.md)                        |
| **Escribir pruebas de API**              | [api-testing.md](testing-patterns/api-testing.md), [test-suite-structure.md](core/test-suite-structure.md)                                    |
| **Escribir pruebas de GraphQL**          | [graphql-testing.md](testing-patterns/graphql-testing.md), [api-testing.md](testing-patterns/api-testing.md)                                  |
| **Escribir pruebas visuales**            | [visual-regression.md](testing-patterns/visual-regression.md), [canvas-webgl.md](testing-patterns/canvas-webgl.md)                            |
| **Estructurar código con POM**           | [page-object-model.md](core/page-object-model.md), [test-suite-structure.md](core/test-suite-structure.md)                                    |
| **Datos de prueba / fixtures**           | [fixtures-hooks.md](core/fixtures-hooks.md), [test-data.md](core/test-data.md)                                                                |
| **Manejar autenticación**                | [authentication.md](advanced/authentication.md), [authentication-flows.md](advanced/authentication-flows.md)                                  |
| **Probar funciones de fecha/hora**       | [clock-mocking.md](advanced/clock-mocking.md)                                                                                                 |
| **Probar subida/descarga de archivos**   | [file-operations.md](testing-patterns/file-operations.md), [file-upload-download.md](testing-patterns/file-upload-download.md)                |
| **Probar formularios/validación**        | [forms-validation.md](testing-patterns/forms-validation.md)                                                                                   |
| **Probar arrastrar y soltar**            | [drag-drop.md](testing-patterns/drag-drop.md)                                                                                                 |
| **Probar accesibilidad**                 | [accessibility.md](testing-patterns/accessibility.md)                                                                                         |
| **Probar seguridad (XSS, CSRF)**         | [security-testing.md](testing-patterns/security-testing.md)                                                                                   |
| **Usar anotaciones de prueba**           | [annotations.md](core/annotations.md)                                                                                                         |
| **Usar tags de prueba**                  | [test-tags.md](core/test-tags.md)                                                                                                             |
| **Probar iframes**                       | [iframes.md](browser-apis/iframes.md)                                                                                                         |
| **Probar canvas/WebGL**                  | [canvas-webgl.md](testing-patterns/canvas-webgl.md)                                                                                           |
| **Internacionalización (i18n)**          | [i18n.md](testing-patterns/i18n.md)                                                                                                           |
| **Probar apps Electron**                 | [electron.md](testing-patterns/electron.md)                                                                                                   |
| **Probar extensiones del navegador**     | [browser-extensions.md](testing-patterns/browser-extensions.md)                                                                               |

### Pruebas móvil y responsive

**Cuándo usarla**: al probar dispositivos móviles, interacciones táctiles, layouts responsive

| Actividad                          | Archivos de referencia                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------- |
| **Emulación de dispositivos**      | [mobile-testing.md](advanced/mobile-testing.md)                                  |
| **Gestos táctiles (swipe, tap)**   | [mobile-testing.md](advanced/mobile-testing.md)                                  |
| **Pruebas de viewport/breakpoints**| [mobile-testing.md](advanced/mobile-testing.md)                                  |
| **UI específica de móvil**         | [mobile-testing.md](advanced/mobile-testing.md), [locators.md](core/locators.md) |

### Tiempo real y APIs del navegador

**Cuándo usarla**: al probar WebSockets, geolocalización, permisos, flujos multi-tab

| Actividad                       | Archivos de referencia                                                                   |
| ------------------------------- | ---------------------------------------------------------------------------------------- |
| **WebSocket / tiempo real**     | [websockets.md](browser-apis/websockets.md)                                              |
| **Mock de geolocalización**     | [browser-apis.md](browser-apis/browser-apis.md)                                          |
| **Manejo de permisos**          | [browser-apis.md](browser-apis/browser-apis.md)                                          |
| **Pruebas de portapapeles**     | [browser-apis.md](browser-apis/browser-apis.md)                                          |
| **Mock de cámara/micrófono**    | [browser-apis.md](browser-apis/browser-apis.md)                                          |
| **Flujos multi-tab/popup**      | [multi-context.md](advanced/multi-context.md)                                            |
| **Manejo de popup OAuth**       | [third-party.md](advanced/third-party.md), [multi-context.md](advanced/multi-context.md) |

### Depuración y solución de problemas

**Cuándo usarla**: fallos de pruebas, elemento no encontrado, timeouts, comportamiento inesperado

| Actividad                                            | Archivos de referencia                                                                                                                                |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Depurar fallos de pruebas**                        | [debugging.md](debugging/debugging.md), [assertions-waiting.md](core/assertions-waiting.md)                                                    |
| **Corregir pruebas flaky**                           | [flaky-tests.md](debugging/flaky-tests.md), [debugging.md](debugging/debugging.md), [assertions-waiting.md](core/assertions-waiting.md)        |
| **Depurar runs paralelos flaky**                     | [flaky-tests.md](debugging/flaky-tests.md), [performance.md](infrastructure-ci-cd/performance.md), [fixtures-hooks.md](core/fixtures-hooks.md) |
| **Aislamiento / evitar fuga de estado**              | [flaky-tests.md](debugging/flaky-tests.md), [fixtures-hooks.md](core/fixtures-hooks.md), [performance.md](infrastructure-ci-cd/performance.md) |
| **Corregir problemas de selectores**                 | [locators.md](core/locators.md), [debugging.md](debugging/debugging.md)                                                                        |
| **Investigar problemas de timeout**                  | [assertions-waiting.md](core/assertions-waiting.md), [debugging.md](debugging/debugging.md)                                                    |
| **Usar el visor de trazas**                          | [debugging.md](debugging/debugging.md)                                                                                                         |
| **Depurar condiciones de carrera**                   | [flaky-tests.md](debugging/flaky-tests.md), [debugging.md](debugging/debugging.md), [assertions-waiting.md](core/assertions-waiting.md)        |
| **Depurar errores de consola/JS**                    | [console-errors.md](debugging/console-errors.md), [debugging.md](debugging/debugging.md)                                                       |

### Pruebas de errores y casos borde

**Cuándo usarla**: al probar estados de error, modo offline, fallos de red, validación

| Actividad                       | Archivos de referencia                                                                                       |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Pruebas de bordes de error**  | [error-testing.md](debugging/error-testing.md)                                                        |
| **Simular fallos de red**       | [error-testing.md](debugging/error-testing.md), [network-advanced.md](advanced/network-advanced.md)   |
| **Probar modo offline**         | [error-testing.md](debugging/error-testing.md), [service-workers.md](browser-apis/service-workers.md) |
| **Probar service workers**      | [service-workers.md](browser-apis/service-workers.md)                                                 |
| **Probar estados de carga**     | [error-testing.md](debugging/error-testing.md)                                                        |
| **Probar validación de formularios** | [error-testing.md](debugging/error-testing.md)                                                     |

### Pruebas multi-usuario y colaboración

**Cuándo usarla**: al probar funciones con varios usuarios, roles o colaboración en tiempo real

| Actividad                          | Archivos de referencia                                                                       |
| ---------------------------------- | ------------------------------------------------------------------------------------ |
| **Varios usuarios en una prueba**  | [multi-user.md](advanced/multi-user.md)                                              |
| **Colaboración en tiempo real**    | [multi-user.md](advanced/multi-user.md), [websockets.md](browser-apis/websockets.md) |
| **Pruebas de acceso por rol**      | [multi-user.md](advanced/multi-user.md)                                              |
| **Pruebas de acciones concurrentes** | [multi-user.md](advanced/multi-user.md)                                            |

### Decisiones de arquitectura

**Cuándo usarla**: al elegir patrones de prueba, decidir entre enfoques, planear la arquitectura de pruebas

| Actividad                      | Archivos de referencia                                        |
| ------------------------------ | --------------------------------------------------------- |
| **Decisión POM vs fixtures**   | [pom-vs-fixtures.md](architecture/pom-vs-fixtures.md)     |
| **Selección de tipo de prueba**| [test-architecture.md](architecture/test-architecture.md) |
| **Mock vs servicios reales**   | [when-to-mock.md](architecture/when-to-mock.md)           |
| **Estructura de la suite**     | [test-suite-structure.md](core/test-suite-structure.md)   |

### Pruebas por framework

**Cuándo usarla**: al probar aplicaciones React, Angular, Vue o Next.js

| Actividad                 | Archivos de referencia                  |
| ------------------------- | ----------------------------------- |
| **Probar apps React**     | [react.md](frameworks/react.md)     |
| **Probar apps Angular**   | [angular.md](frameworks/angular.md) |
| **Probar apps Vue/Nuxt**  | [vue.md](frameworks/vue.md)         |
| **Probar apps Next.js**   | [nextjs.md](frameworks/nextjs.md)   |

### Refactor y mantenimiento

**Cuándo usarla**: al mejorar pruebas existentes, revisar código, reducir duplicación

| Actividad                               | Archivos de referencia                                                                                             |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Migrar a Page Object Model**          | [page-object-model.md](core/page-object-model.md), [test-suite-structure.md](core/test-suite-structure.md) |
| **Mejorar organización de pruebas**     | [test-suite-structure.md](core/test-suite-structure.md), [page-object-model.md](core/page-object-model.md) |
| **Extraer setup/teardown común**        | [fixtures-hooks.md](core/fixtures-hooks.md)                                                                |
| **Reemplazar selectores frágiles**      | [locators.md](core/locators.md)                                                                            |
| **Quitar esperas explícitas**           | [assertions-waiting.md](core/assertions-waiting.md)                                                        |
| **Crear fábricas de datos de prueba**   | [test-data.md](core/test-data.md)                                                                          |
| **Setup de configuración**              | [configuration.md](core/configuration.md)                                                                  |

### Infraestructura y configuración

**Cuándo usarla**: al montar proyectos, configurar CI/CD, optimizar rendimiento

| Actividad                                   | Archivos de referencia                                                                                                           |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Configurar proyecto Playwright**          | [configuration.md](core/configuration.md), [projects-dependencies.md](core/projects-dependencies.md)                     |
| **Montar pipelines CI/CD**                  | [ci-cd.md](infrastructure-ci-cd/ci-cd.md), [github-actions.md](infrastructure-ci-cd/github-actions.md)                   |
| **Setup de GitHub Actions**                 | [github-actions.md](infrastructure-ci-cd/github-actions.md)                                                              |
| **Setup de GitLab CI**                      | [gitlab.md](infrastructure-ci-cd/gitlab.md)                                                                              |
| **Otros proveedores CI**                    | [other-providers.md](infrastructure-ci-cd/other-providers.md)                                                            |
| **Setup Docker/contenedores**               | [docker.md](infrastructure-ci-cd/docker.md)                                                                              |
| **Setup y teardown global**                 | [global-setup.md](core/global-setup.md)                                                                                  |
| **Dependencias de proyecto**                | [projects-dependencies.md](core/projects-dependencies.md)                                                                |
| **Optimizar rendimiento de pruebas**        | [performance.md](infrastructure-ci-cd/performance.md), [test-suite-structure.md](core/test-suite-structure.md)           |
| **Configurar ejecución paralela**           | [parallel-sharding.md](infrastructure-ci-cd/parallel-sharding.md), [performance.md](infrastructure-ci-cd/performance.md) |
| **Aislar datos entre workers**              | [fixtures-hooks.md](core/fixtures-hooks.md), [performance.md](infrastructure-ci-cd/performance.md)                       |
| **Cobertura de pruebas**                    | [test-coverage.md](infrastructure-ci-cd/test-coverage.md)                                                                |
| **Reportes/artefactos de prueba**           | [reporting.md](infrastructure-ci-cd/reporting.md)                                                                        |

### Patrones avanzados

**Cuándo usarla**: escenarios complejos, mocks de API, intercepción de red

| Actividad                                | Archivos de referencia                                                                                               |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Mockear respuestas de API**            | [test-suite-structure.md](core/test-suite-structure.md), [network-advanced.md](advanced/network-advanced.md) |
| **Intercepción de red**                  | [network-advanced.md](advanced/network-advanced.md), [assertions-waiting.md](core/assertions-waiting.md)     |
| **Mocks de GraphQL**                     | [network-advanced.md](advanced/network-advanced.md)                                                          |
| **Grabar/reproducir HAR**                | [network-advanced.md](advanced/network-advanced.md)                                                          |
| **Fixtures personalizados**              | [fixtures-hooks.md](core/fixtures-hooks.md)                                                                  |
| **Estrategias avanzadas de espera**      | [assertions-waiting.md](core/assertions-waiting.md)                                                          |
| **Mocks de OAuth/SSO**                   | [third-party.md](advanced/third-party.md), [multi-context.md](advanced/multi-context.md)                     |
| **Mocks de pasarela de pago**            | [third-party.md](advanced/third-party.md)                                                                    |
| **Mocks de verificación email/SMS**      | [third-party.md](advanced/third-party.md)                                                                    |
| **Fallar con errores de consola**        | [console-errors.md](debugging/console-errors.md)                                                             |
| **Pruebas de seguridad (XSS, CSRF)**     | [security-testing.md](testing-patterns/security-testing.md)                                                  |
| **Presupuestos rendimiento y Web Vitals**| [performance-testing.md](testing-patterns/performance-testing.md)                                            |
| **Integración con Lighthouse**           | [performance-testing.md](testing-patterns/performance-testing.md)                                            |
| **Anotaciones (skip, fixme)**            | [annotations.md](core/annotations.md)                                                                        |
| **Tags (@smoke, @fast)**                 | [test-tags.md](core/test-tags.md)                                                                            |
| **Pasos de prueba para reportes**        | [annotations.md](core/annotations.md)                                                                        |

## Árbol de decisión rápida

```
¿Qué estás haciendo?
│
├─ ¿Escribes una prueba nueva?
│  ├─ Prueba E2E → core/test-suite-structure.md, core/locators.md, core/assertions-waiting.md
│  ├─ Prueba de componentes → testing-patterns/component-testing.md
│  ├─ Prueba de API → testing-patterns/api-testing.md, core/test-suite-structure.md
│  ├─ Prueba de GraphQL → testing-patterns/graphql-testing.md
│  ├─ Regresión visual → testing-patterns/visual-regression.md
│  ├─ Prueba visual/canvas → testing-patterns/canvas-webgl.md, core/test-suite-structure.md
│  ├─ Prueba de accesibilidad → testing-patterns/accessibility.md
│  ├─ Prueba móvil/responsive → advanced/mobile-testing.md
│  ├─ Prueba i18n/locale → testing-patterns/i18n.md
│  ├─ Prueba de app Electron → testing-patterns/electron.md
│  ├─ Prueba de extensión → testing-patterns/browser-extensions.md
│  ├─ Prueba multi-usuario → advanced/multi-user.md
│  ├─ Prueba de validación de formularios → testing-patterns/forms-validation.md
│  └─ Prueba de arrastrar y soltar → testing-patterns/drag-drop.md
│
├─ ¿Pruebas funciones específicas?
│  ├─ Subida/descarga de archivos → testing-patterns/file-operations.md, testing-patterns/file-upload-download.md
│  ├─ Dependiente de fecha/hora → advanced/clock-mocking.md
│  ├─ WebSocket/tiempo real → browser-apis/websockets.md
│  ├─ Geolocalización/permisos → browser-apis/browser-apis.md
│  ├─ Mocks OAuth/SSO → advanced/third-party.md, advanced/multi-context.md
│  ├─ Pagos/email/SMS → advanced/third-party.md
│  ├─ iFrames → browser-apis/iframes.md
│  ├─ Canvas/WebGL/gráficos → testing-patterns/canvas-webgl.md
│  ├─ Service workers/PWA → browser-apis/service-workers.md
│  ├─ i18n/localización → testing-patterns/i18n.md
│  ├─ Seguridad (XSS, CSRF) → testing-patterns/security-testing.md
│  └─ Rendimiento/Web Vitals → testing-patterns/performance-testing.md
│
├─ ¿Decisiones de arquitectura?
│  ├─ POM vs fixtures → architecture/pom-vs-fixtures.md
│  ├─ Selección de tipo de prueba → architecture/test-architecture.md
│  ├─ Mock vs servicios reales → architecture/when-to-mock.md
│  └─ Estructura de la suite → core/test-suite-structure.md
│
├─ ¿Pruebas por framework?
│  ├─ App React → frameworks/react.md
│  ├─ App Angular → frameworks/angular.md
│  ├─ App Vue/Nuxt → frameworks/vue.md
│  └─ App Next.js → frameworks/nextjs.md
│
├─ ¿Pruebas de autenticación?
│  ├─ Patrones básicos de auth → advanced/authentication.md
│  └─ Flujos complejos (MFA, reset) → advanced/authentication-flows.md
│
├─ ¿La prueba falla o es flaky?
│  ├─ Investigar prueba flaky → debugging/flaky-tests.md
│  ├─ Elemento no encontrado → core/locators.md, debugging/debugging.md
│  ├─ Problemas de timeout → core/assertions-waiting.md, debugging/debugging.md
│  ├─ Condiciones de carrera → debugging/flaky-tests.md, debugging/debugging.md
│  ├─ Flaky solo con varios workers → debugging/flaky-tests.md, infrastructure-ci-cd/performance.md
│  ├─ Fuga de estado / aislamiento → debugging/flaky-tests.md, core/fixtures-hooks.md
│  ├─ Errores de consola/JS → debugging/console-errors.md, debugging/debugging.md
│  └─ Depuración general → debugging/debugging.md
│
├─ ¿Pruebas escenarios de error?
│  ├─ Fallos de red → debugging/error-testing.md, advanced/network-advanced.md
│  ├─ Offline (inesperado) → debugging/error-testing.md
│  ├─ Offline-first/PWA → browser-apis/service-workers.md
│  ├─ Bordes de error → debugging/error-testing.md
│  └─ Validación de formularios → testing-patterns/forms-validation.md, debugging/error-testing.md
│
├─ ¿Refactorizas código existente?
│  ├─ Implementar POM → core/page-object-model.md
│  ├─ Mejorar selectores → core/locators.md
│  ├─ Extraer fixtures → core/fixtures-hooks.md
│  ├─ Crear fábricas de datos → core/test-data.md
│  └─ Setup de configuración → core/configuration.md
│
├─ ¿Montas infraestructura?
│  ├─ CI/CD → infrastructure-ci-cd/ci-cd.md
│  ├─ GitHub Actions → infrastructure-ci-cd/github-actions.md
│  ├─ GitLab CI → infrastructure-ci-cd/gitlab.md
│  ├─ Otros proveedores CI → infrastructure-ci-cd/other-providers.md
│  ├─ Docker/contenedores → infrastructure-ci-cd/docker.md
│  ├─ Sharding/paralelo → infrastructure-ci-cd/parallel-sharding.md
│  ├─ Reportes/artefactos → infrastructure-ci-cd/reporting.md
│  ├─ Setup/teardown global → core/global-setup.md
│  ├─ Dependencias de proyecto → core/projects-dependencies.md
│  ├─ Rendimiento de pruebas → infrastructure-ci-cd/performance.md
│  ├─ Cobertura → infrastructure-ci-cd/test-coverage.md
│  └─ Config del proyecto → core/configuration.md, core/projects-dependencies.md
│
├─ ¿Organizas pruebas?
│  ├─ Pruebas skip/fixme/slow → core/annotations.md
│  ├─ Tags (@smoke, @fast) → core/test-tags.md
│  ├─ Filtrar pruebas (--grep) → core/test-tags.md
│  ├─ Pasos de prueba → core/annotations.md
│  └─ Ejecución condicional → core/annotations.md
│
└─ ¿Ejecutas un subconjunto de pruebas?
   ├─ Por tag (@smoke, @critical) → core/test-tags.md
   ├─ Excluir lentas/flaky → core/test-tags.md
   ├─ PR vs nocturnas → core/test-tags.md, infrastructure-ci-cd/ci-cd.md
   └─ Filtrado por proyecto → core/test-tags.md, core/configuration.md
```

## Ciclo de validación de pruebas

Después de escribir o modificar pruebas:

1. **Corre las pruebas**: `npx playwright test --reporter=list`
2. **Si fallan**:
   - Revisa la salida de error y la traza (`npx playwright show-trace`)
   - Corrige locators, esperas o aserciones
   - Vuelve a correr las pruebas
3. **Sigue solo cuando todas pasen**
4. **Repite varias veces** las pruebas críticas: `npx playwright test --repeat-each=5`
