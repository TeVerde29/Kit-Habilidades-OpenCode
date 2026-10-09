---
name: plan-pruebas-pymes
description: >
  Genera planes de prueba pragmáticos para empresas pequeñas:
  alcance, casos con evidencia real, incidencias y veredicto.
  Pragmatic test plans for small businesses: scope, evidence-backed
  cases, issues and verdict.
  Auto-activate on "plan de pruebas", "prueba esto" or /plan-pruebas-pymes.
---

Genera planes de prueba pragmáticos para empresas pequeñas:
alcance, casos con evidencia real, incidencias y veredicto.
Sin burocracia. Se activa con "plan de pruebas", "prueba
esto" o /plan-pruebas-pymes.

## 0. Pedir antes (no inventar)

- Sistema + versión, URL y usuarios de prueba por rol.
- Qué entra y qué NO entra (si no lo dicen, pregúntalo:
  es lo que más te protege).
- Carpeta para evidencias.

## 1. Estructura (solo 7 bloques)

1. **Alcance.** Qué módulos se prueban y cuáles quedan
   FUERA, con motivo. Una lista cada uno.
2. **Ambiente mínimo.** URL, usuarios/roles, versión del
   sistema, navegador. Tabla de 4 filas.
3. **Casos de prueba.** Un caso por funcionalidad, tabla:
   ID (CP-01...), Módulo, Pasos (numerados), Resultado
   esperado, Resultado obtenido, Evidencia, Estado
   (APROBADO / NO APROBADO).
4. **Registro de incidencias.** Solo los que fallaron:
   ID, caso, severidad (alta/media/baja), descripción,
   estado (abierta/corregida/re-probada).
5. **Resumen final.** X/Y aprobados, % y veredicto en una
   línea: "APTO" o "NO APTO + qué falta".
6. **Riesgos (5 filas máximo).** Riesgo + plan B.
7. **Aprobado por.** Nombre + fecha. Un mensaje basta.

## 2. Ejecución con brave-devtools

Por cada caso: navega, actúa (clic, login, formulario),
take_screenshot por paso clave en
`evidencias/CP-XX-paso-N.png`, compara con lo esperado
y marca el estado. Anexos jamás vacíos. Si el sistema
está caído o falta un dato, detente y pide eso.

## 3. Reglas

- Nada de resultados inventados: lo no probado se marca
  "NO EJECUTADO", nunca APROBADO.
- Un solo archivo Markdown + carpeta evidencias.
- Simple: si una sección no aporta, se recorta.
