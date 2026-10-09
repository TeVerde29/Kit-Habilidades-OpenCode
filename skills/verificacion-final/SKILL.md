---
name: verificacion-final
description: Verificacion obligatoria antes de declarar un trabajo completo, corregido o aprobado. Use when about to claim work is complete, fixed, or passing, before committing or creating PRs - requires running verification commands and confirming output before making any success claims; evidence before assertions always
---

# Verificación Antes de Terminar

## Resumen

**Principio central:** Evidencia antes que afirmaciones, siempre.

**Violar la letra de esta regla es violar el espíritu de esta regla.**

## La ley de hierro

```
SIN EVIDENCIA DE VERIFICACIÓN FRESCA NO HAY AFIRMACIÓN DE TERMINADO
```

Si no ejecutaste el comando de verificación en este mensaje, no puedes afirmar que pasa.

## La función de puerta

```
ANTES de afirmar cualquier estado o expresar satisfacción:

1. IDENTIFICA: ¿Qué comando prueba esta afirmación?
2. EJECUTA: Corre el comando COMPLETO (fresco, completo)
3. LEE: Salida completa, revisa el código de salida, cuenta los fallos
4. VERIFICA: ¿La salida confirma la afirmación?
   - Si NO: Declara el estado real con evidencia
   - Si SÍ: Declara la afirmación CON evidencia
5. SOLO ENTONCES: Haz la afirmación

Saltar cualquier paso = mentir, no verificar
```

## Fallos comunes

| Afirmación | Requiere | No basta |
|------------|----------|-----------|
| Las pruebas pasan | Salida del comando de pruebas: 0 fallos | Ejecución anterior, "debería pasar" |
| El linter está limpio | Salida del linter: 0 errores | Revisión parcial, extrapolación |
| La compilación funciona | Comando de build: salida 0 | El linter pasa, los logs se ven bien |
| El error está corregido | Probar el síntoma original: pasa | El código cambió, se asume corregido |
| La prueba de regresión funciona | Ciclo rojo-verde verificado | La prueba pasa una vez |
| El agente terminó | El diff del VCS muestra cambios | El agente dice "éxito" |
| Requisitos cumplidos | Lista de revisión línea por línea | Las pruebas pasan |

## Señales de alerta - DETENTE

- Usar "debería", "probablemente", "parece que"
- Expresar satisfacción antes de verificar ("¡Genial!", "¡Perfecto!", "¡Listo!", etc.)
- Estar por hacer commit/push/PR sin verificación
- Confiar en informes de éxito de agentes
- Apoyarse en verificación parcial
- Pensar "solo esta vez"
- Estar cansado y querer terminar
- **CUALQUIER frase que implique éxito sin haber corrido la verificación**

## Prevención de racionalizaciones

| Excusa | Realidad |
|--------|----------|
| "Debería funcionar ahora" | CORRE la verificación |
| "Estoy seguro" | Confianza ≠ evidencia |
| "Solo esta vez" | Sin excepciones |
| "El linter pasó" | Linter ≠ compilador |
| "El agente dijo éxito" | Verifica de forma independiente |
| "Estoy cansado" | El agotamiento ≠ excusa |
| "Una revisión parcial basta" | Lo parcial no prueba nada |
| "Con otras palabras la regla no aplica" | El espíritu sobre la letra |

## Patrones clave

**Pruebas:**
```
✅ [Correr comando de pruebas] [Ver: 34/34 pasan] "Todas las pruebas pasan"
❌ "Debería pasar ahora" / "Se ve correcto"
```

**Pruebas de regresión (TDD rojo-verde):**
```
✅ Escribir → Correr (pasa) → Revertir corrección → Correr (DEBE FALLAR) → Restaurar → Correr (pasa)
❌ "Escribí una prueba de regresión" (sin verificación rojo-verde)
```

**Build:**
```
✅ [Correr build] [Ver: salida 0] "El build pasa"
❌ "El linter pasó" (el linter no revisa la compilación)
```

**Requisitos:**
```
✅ Releer el plan → Crear lista → Verificar cada uno → Informar huecos o fin
❌ "Las pruebas pasan, fase completa"
```

**Delegación a agentes:**
```
✅ El agente dice éxito → Revisar diff del VCS → Verificar cambios → Informar el estado real
❌ Confiar en el informe del agente
```

## Cuándo aplicarla

**SIEMPRE antes de:**
- CUALQUIER variación de afirmaciones de éxito/fin
- CUALQUIER expresión de satisfacción
- CUALQUIER frase positiva sobre el estado del trabajo
- Hacer commit, crear PR, terminar la tarea
- Pasar a la siguiente tarea
- Delegar a agentes

**La regla aplica a:**
- Frases exactas
- Paráfrasis y sinónimos
- Implicaciones de éxito
- CUALQUIER comunicación que sugiera fin/corrección
