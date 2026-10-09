---
name: desarrollador-angular
description: Genera código Angular y da guía de arquitectura. Úsala al crear proyectos, componentes, servicios o comunicación HTTP, o para buenas prácticas de reactividad (signals, linkedSignal, resource, httpResource), formularios, inyección de dependencias, routing, SSR, accesibilidad (ARIA), animaciones, estilos (estilos de componente, Tailwind CSS), pruebas, convenciones de nombres o herramientas CLI. | Generates Angular code and provides architectural guidance. Trigger when creating projects, components, services, or HTTP communication, or for best practices on reactivity (signals, linkedSignal, resource, httpResource), forms, dependency injection, routing, SSR, accessibility (ARIA), animations, styling (component styles, Tailwind CSS), testing, naming conventions, or CLI tooling.
license: MIT
metadata:
  author: Copyright 2026 Google LLC
  version: '1.0'
---

# Guía de desarrollo en Angular

1. Analiza siempre la versión de Angular del proyecto antes de dar guía, porque las buenas prácticas y las funciones disponibles pueden variar mucho entre versiones. Si creas un proyecto nuevo con Angular CLI, no indiques una versión salvo que la persona usuaria lo pida.

2. Al generar código, sigue la guía de estilo y las buenas prácticas de Angular para que sea mantenible y rápido. Usa Angular CLI para andamiar componentes, servicios, directivas, pipes y rutas y así mantener la consistencia.

3. Cuando termines de generar código, ejecuta `ng build` para asegurar que no hay errores de compilación. Si hay errores, analiza los mensajes y corrígelos antes de seguir. No te saltes este paso, porque es clave para asegurar que el código generado es correcto y funcional.

## Crear proyectos nuevos

Si la persona usuaria no da pautas, estas son las reglas por defecto al crear un proyecto Angular nuevo:

1. Usa la última versión estable de Angular salvo que la persona usuaria diga otra cosa.
2. Usa Signal Forms para la gestión de formularios en proyectos nuevos (estable en Angular v22 y siguientes) [Más info](references/signal-forms.md).

**Reglas de ejecución para `ng new`:**
Cuando te pidan crear un proyecto Angular nuevo, debes decidir el comando correcto siguiendo estos pasos estrictos:

**Paso 1: Comprueba si la persona pide una versión explícita.**

- **SI** la persona pide una versión concreta (p. ej. Angular 15), ignora instalaciones locales y usa estrictamente `npx`.
- **Comando:** `npx @angular/cli@<requested_version> new <project-name>`

**Paso 2: Comprueba si ya hay Angular instalado.**

- **SI** no se pide una versión concreta, ejecuta `ng version` en la terminal para ver si Angular CLI ya está instalado en el sistema.
- **SI** el comando funciona y devuelve una versión instalada, usa la instalación local/global directamente.
- **Comando:** `ng new <project-name>`

**Paso 3: Usa la última versión si no hay instalación.**

- **SI** no se pide una versión concreta Y el comando `ng version` falla (no hay instalación de Angular), debes usar `npx` para traer la última versión.
- **Comando:** `npx @angular/cli@latest new <project-name>`

## Componentes

Al trabajar con componentes Angular, consulta estas referencias según la tarea:

- **Fundamentos**: anatomía, metadatos, conceptos centrales, etiquetas autocerradas y flujo de control en plantillas (@if, @for, @switch). Lee [components.md](references/components.md)
- **Inputs**: inputs basados en signals, transforms y model inputs. Lee [inputs.md](references/inputs.md)
- **Outputs**: outputs basados en signals y buenas prácticas de eventos propios. Lee [outputs.md](references/outputs.md)
- **Elementos host (Host Elements)**: host bindings e inyección de atributos. Lee [host-elements.md](references/host-elements.md)
- **Convenciones de nombres**: estilo moderno de nombres en Angular v20+ ("Intención antes que Rol") para archivos, componentes, servicios, directivas, pipes y modelos. Lee [naming-conventions.md](references/naming-conventions.md)

Si necesitas documentación más profunda que no esté en las referencias de arriba, lee la documentación en `https://angular.dev/guide/components`.

## Reactividad y gestión de datos

Para gestionar estado y reactividad de datos, usa Signals de Angular y consulta estas referencias:

- **Resumen de Signals**: conceptos centrales de signals (`signal`, `computed`), contextos reactivos y `untracked`. Lee [signals-overview.md](references/signals-overview.md)
- **Estado dependiente (`linkedSignal`)**: crear estado escribible ligado a signals de origen. Lee [linked-signal.md](references/linked-signal.md)
- **Reactividad asíncrona (`resource`)**: traer datos asíncronos directo al estado con signals. Lee [resource.md](references/resource.md)
- **Efectos secundarios (`effect`)**: logs, manipulación del DOM de terceros (`afterRenderEffect`) y cuándo NO usar effects. Lee [effects.md](references/effects.md)

## Comunicación HTTP

Al comunicarte con servicios backend, usa las APIs HTTP de Angular y consulta esta referencia:

- **Cliente HTTP y recursos**: `provideHttpClient`, `HttpClient`, interceptors y `httpResource`. Lee [http-client.md](references/http-client.md)

## Formularios

En la mayoría de apps nuevas, **prefiere signal forms**. Al decidir el tipo de formulario, analiza el proyecto y ten en cuenta estas pautas:

- Si la app usa v22 o siguiente y es un formulario nuevo, **prefiere Signal Forms**.
- Para apps más viejas o al trabajar con formularios existentes, usa el tipo de formulario que ya use la app.

- **Signal Forms**: usa signals para gestionar el estado del formulario. Lee [signal-forms.md](references/signal-forms.md)
- **Formularios por plantilla (template-driven)**: úsalos para formularios simples. Lee [template-driven-forms.md](references/template-driven-forms.md)
- **Formularios reactivos**: úsalos para formularios complejos. Lee [reactive-forms.md](references/reactive-forms.md)

## Inyección de dependencias

Al implementar inyección de dependencias en Angular, sigue estas pautas:

- **Fundamentos**: resumen de inyección de dependencias, servicios y la función `inject()`. Lee [di-fundamentals.md](references/di-fundamentals.md)
- **Crear y usar servicios**: crear servicios, la opción `providedIn: 'root'` e inyectar en componentes u otros servicios. Lee [creating-services.md](references/creating-services.md)
- **Definir proveedores de dependencias**: provisión automática frente a manual, `InjectionToken`, `useClass`, `useValue`, `useFactory` y ámbitos (scopes). Lee [defining-providers.md](references/defining-providers.md)
- **Contexto de inyección**: dónde se permite `inject()`, `runInInjectionContext` y `assertInInjectionContext`. Lee [injection-context.md](references/injection-context.md)
- **Inyectores jerárquicos**: `EnvironmentInjector` frente a `ElementInjector`, reglas de resolución, modificadores (`optional`, `skipSelf`) y `providers` frente a `viewProviders`. Lee [hierarchical-injectors.md](references/hierarchical-injectors.md)

## Pipes

Al formatear valores en plantillas, crear pipes propios o reutilizar lógica tipo pipe en TypeScript, consulta esta referencia. Prefiere pipes en plantillas; fuera de plantillas, evita inyectar clases de pipe solo para llamar a `transform()`.

- **Pipes**: imports de pipes incluidos, nombre e implementación de pipes propios, pipes puros frente a impuros, y patrones de reutilización en TypeScript con funciones de formato standalone o funciones planas extraídas. Lee [pipes.md](references/pipes.md)

## Angular Aria

Al construir componentes propios accesibles para cualquiera de estos patrones: Accordion, Listbox, Combobox, Menu, Tabs, Toolbar, Tree, Grid, consulta esta referencia:

- **Componentes de Angular Aria**: construir componentes headless accesibles (Accordion, Listbox, Combobox, Menu, Tabs, Toolbar, Tree, Grid) y dar estilo a atributos ARIA. Lee [angular-aria.md](references/angular-aria.md)

## Routing

Al implementar navegación en Angular, consulta estas referencias:

- **Definir rutas**: paths de URL, segmentos estáticos frente a dinámicos, comodines y redirecciones. Lee [define-routes.md](references/define-routes.md)
- **Estrategias de carga de rutas**: carga eager frente a lazy, y carga según contexto. Lee [loading-strategies.md](references/loading-strategies.md)
- **Mostrar rutas con outlets**: uso de `<router-outlet>`, outlets anidados y outlets con nombre. Lee [show-routes-with-outlets.md](references/show-routes-with-outlets.md)
- **Navegar a rutas**: navegación declarativa con `RouterLink` y navegación programática con `Router`. Lee [navigate-to-routes.md](references/navigate-to-routes.md)
- **Controlar acceso con guards**: implementar `CanActivate`, `CanMatch` y otros guards para seguridad. Lee [route-guards.md](references/route-guards.md)
- **Resolvers de datos**: precargar datos antes de activar la ruta con `ResolveFn`. Lee [data-resolvers.md](references/data-resolvers.md)
- **Ciclo de vida y eventos del router**: orden cronológico de eventos de navegación y depuración. Lee [router-lifecycle.md](references/router-lifecycle.md)
- **Estrategias de renderizado**: CSR, SSG (prerenderizado) y SSR con hidratación. Lee [rendering-strategies.md](references/rendering-strategies.md)
- **Animaciones de transición de ruta**: activar y personalizar la View Transitions API. Lee [route-animations.md](references/route-animations.md)

Si necesitas documentación más profunda o más contexto, visita la [guía oficial de Routing de Angular](https://angular.dev/guide/routing).

## Estilos y animaciones

Al implementar estilos y animaciones en Angular, consulta estas referencias:

- **Usar Tailwind CSS con Angular**: integrar Tailwind CSS en proyectos Angular. Lee [tailwind-css.md](references/tailwind-css.md)
- **Animaciones en Angular**: usar CSS nativo (recomendado) o el DSL heredado para efectos dinámicos. Lee [angular-animations.md](references/angular-animations.md)
- **Estilos de componentes**: buenas prácticas para estilos de componentes y encapsulación. Lee [component-styling.md](references/component-styling.md)

## Pruebas

Al escribir o actualizar pruebas, consulta estas referencias según la tarea:

- **Fundamentos**: buenas prácticas de pruebas unitarias (Vitest), patrones async y `TestBed`. Lee [testing-fundamentals.md](references/testing-fundamentals.md)
- **Harnesses de componentes**: patrones estándar para interacción robusta con componentes. Lee [component-harnesses.md](references/component-harnesses.md)
- **Pruebas del router**: usar `RouterTestingHarness` para pruebas de navegación fiables. Lee [router-testing.md](references/router-testing.md)
- **Pruebas end-to-end (E2E)**: configurar y ejecutar pruebas E2E. Lee [e2e-testing.md](references/e2e-testing.md)

## Herramientas

Al trabajar con herramientas de Angular, consulta estas referencias:

- **Angular CLI**: crear aplicaciones, generar código (componentes, rutas, servicios), servir y compilar. Lee [cli.md](references/cli.md)
- **Modernización de código**: refactorizar automáticamente a estándares modernos con migraciones. Lee [migrations.md](references/migrations.md)
- **Servidor MCP de Angular**: herramientas disponibles, configuración y funciones experimentales. Lee [mcp.md](references/mcp.md)
- **Configuración de entornos**: estrategias para configuración en tiempo de compilación y en tiempo de ejecución. Lee [environment-configuration.md](references/environment-configuration.md)
