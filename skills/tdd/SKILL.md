---
name: tdd
description: Desarrollo con pruebas primero (TDD). Úsala cuando el usuario quiera crear funciones o corregir errores empezando por las pruebas, mencione "red-green-refactor" o quiera pruebas de integración. | Test-driven development. Use when the user wants to build features or fix bugs test-first, mentions "red-green-refactor", or wants integration tests.
---

# Desarrollo con pruebas primero (TDD)

TDD es el ciclo rojo → verde. Esta skill es la referencia para que ese ciclo produzca pruebas que valga la pena conservar: qué es una buena prueba, dónde van las pruebas, los antipatrones y las reglas del ciclo. Cada sección aplica en cada ciclo: consúltalas antes y durante el ciclo, no después.

Al explorar el código, lee `GLOSSARY.md` (si existe) para que los nombres de las pruebas y el vocabulario de las interfaces coincidan con el lenguaje del dominio del proyecto, y respeta los ADRs de la zona que vayas a tocar.

## Qué es una buena prueba

Las pruebas verifican comportamiento a través de interfaces públicas, no detalles de implementación. El código puede cambiar por completo; las pruebas no deberían. Una buena prueba se lee como una especificación: "el usuario puede pagar con un carrito válido" te dice exactamente qué capacidad existe, y sobrevive a refactors porque no depende de la estructura interna.

Ver [tests.md](tests.md) para ejemplos y [mocking.md](mocking.md) para guías de mocks.

## Seams: dónde van las pruebas

Un **seam** es el borde público donde haces pruebas: la interfaz donde observas el comportamiento sin meterte dentro. Las pruebas viven en los seams, nunca contra detalles internos.

**Haz pruebas solo en seams acordados de antemano.** Antes de escribir cualquier prueba, anota los seams bajo prueba y confírmalos con el usuario. Ninguna prueba se escribe en un seam sin confirmar. No puedes probar todo, así que acordar los seams por adelantado es la forma de que el esfuerzo de pruebas caiga en las rutas críticas y la lógica compleja en vez de en cada caso borde.

Pregunta: "¿Cuál es la interfaz pública y en qué seams deberíamos probar?"

Cuando la forma de esa interfaz está en duda (qué tan profundo es el módulo, dónde va el seam, qué debería exponer la interfaz), llama a la herramienta Skill con "diseno-modulos" para el vocabulario. Es la fuente compartida de los términos módulo, interfaz, profundidad, seam, adaptador, leverage y localidad, y es una referencia para consultar, no una sesión para ejecutar.

## Antipatrones

- **Acoplado a la implementación**: usa mocks de colaboradores internos, prueba métodos privados o verifica por un canal lateral (consultar la base de datos en vez de usar la interfaz). La señal: la prueba se rompe cuando refactorizas pero el comportamiento no cambió.
- **Tautológico**: la aserción recalcula el valor esperado de la misma forma que el código (`expect(add(a, b)).toBe(a + b)`, un snapshot derivado a mano de la misma forma, una constante comparada consigo misma), así que pasa por construcción y nunca puede estar en desacuerdo con el código. Los valores esperados deben venir de una fuente independiente de verdad: un literal conocido-bueno, un ejemplo trabajado, la especificación.
- **Corte horizontal**: escribir todas las pruebas primero y luego toda la implementación. Las pruebas en bloque verifican comportamiento _imaginado_: pruebas la _forma_ de las cosas en vez del comportamiento visible al usuario, las pruebas se vuelven insensibles a cambios reales y te comprometes con la estructura de las pruebas antes de entender la implementación. Trabaja en **cortes verticales**: una prueba → una implementación → repetir, cada prueba una **bala trazadora** que responde a lo que te enseñó el ciclo anterior.

## Reglas del ciclo

- **Rojo antes que verde.** Escribe primero la prueba que falla, luego solo el código necesario para pasarla. No anticipes pruebas futuras ni agregues funciones especulativas.
- **Un corte a la vez.** Un seam, una prueba, una implementación mínima por ciclo.
- **El refactor no es parte del ciclo.** Pertenece a la etapa de revisión (ver la skill `code-review`), no al ciclo rojo → verde de implementación.
