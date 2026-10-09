# Skill: tdd

Desarrollo con pruebas primero: ciclo rojo → verde.
Se auto-activa al crear funciones o corregir errores
empezando por las pruebas, o al mencionar
"red-green-refactor" o pruebas de integración.

## Buena prueba

- Verifica comportamiento por interfaces públicas,
  no detalles internos. Debe sobrevivir a refactors.
- Se lee como especificación: "el usuario puede pagar
  con un carrito válido".
- Ejemplos en `tests.md`, mocks en `mocking.md`.

## Seams

- El **seam** es el borde público donde se prueba.
- Antes de escribir pruebas, anota los seams y
  confírmalos con el usuario. Sin seam confirmado,
  no hay prueba.
- Si la forma de la interfaz está en duda, consulta
  la skill `diseno-modulos` (vocabulario de módulo,
  interfaz, profundidad, seam, adaptador).

## Antipatrones

- Acoplado a implementación, tautológico, corte
  horizontal (todas las pruebas primero).
- Trabaja en cortes verticales: una prueba →
  una implementación → repetir.

## Reglas del ciclo

- Rojo antes que verde: primero la prueba que falla.
- Un corte a la vez: un seam, una prueba,
  una implementación mínima.
- El refactor es etapa de revisión, no del ciclo.
