---
name: diagnosticar-errores
description: Ciclo de diagnóstico para errores difíciles y regresiones de rendimiento. Diagnosis loop for hard bugs and performance regressions. Use when the user says "diagnose"/"debug this", or reports something broken/throwing/failing/slow.
---

# Diagnosticar Errores

Una disciplina para errores difíciles. Omite fases solo cuando esté explícitamente justificado.

Al explorar el código, lee `GLOSSARY.md` (si existe) para tener un modelo mental claro de los módulos relevantes, y revisa los ADRs de la zona que vayas a tocar.

## Oculta los secretos

Esta skill te pide mostrar comandos, salidas y artefactos capturados. **Oculta cada secreto primero**: escribe `<REDACTED>` en su lugar. Apoya los ciclos en variables de entorno, para que la credencial quede en el entorno y no en lo que muestras. Los artefactos capturados llevan cabeceras de autenticación: cita solo las líneas que aportan la señal.

Si la salida ocultada no basta para diagnosticar el error, dilo y pregunta al usuario.

## Fase 1: Construye un ciclo de retroalimentación

**Esta es la skill.** Todo lo demás es mecánico. Si tienes una señal de pasa/falla **ajustada** para el error (una que se ponga en rojo con _este_ error), encontrarás la causa; la bisección, la prueba de hipótesis y la instrumentación solo la consumen. Si no la tienes, ninguna cantidad de mirar código te salvará.

Dedica un esfuerzo desproporcionado aquí. **Sé agresivo. Sé creativo. Niégate a rendirte.**

### Formas de construir uno, más o menos en este orden

1. **Prueba que falla** en el seam que alcance el error: unitaria, de integración, e2e.
2. **Script de Curl / HTTP** contra un servidor de desarrollo en marcha.
3. **Invocación CLI** con una entrada fija (fixture), comparando la salida con un snapshot correcto conocido.
4. **Script de navegador headless** (Playwright / Puppeteer) que maneje la interfaz y verifique DOM/consola/red.
5. **Reproduce una traza capturada.** Guarda una petición de red / payload / registro de eventos real en disco; repásalo por la ruta de código de forma aislada.
6. **Arnes desechable.** Levanta un subconjunto mínimo del sistema (un servicio, dependencias con mock) que ejercite la ruta del error con una sola llamada de función.
7. **Ciclo de propiedades / fuzz.** Si el error es "a veces la salida es incorrecta", ejecuta 1000 entradas aleatorias y busca el modo de fallo.
8. **Arnes de bisección.** Si el error apareció entre dos estados conocidos (commit, dataset, versión), automatiza "arrancar en el estado X, comprobar, repetir" para poder hacer `git bisect run`.
9. **Ciclo diferencial.** Pasa la misma entrada por la versión vieja y la nueva (o dos configuraciones) y compara salidas.
10. **Script bash con humano en el ciclo (HITL).** Último recurso. Si una persona debe hacer clic, guíala con `scripts/hitl-loop.template.sh` para que el ciclo siga estructurado. La salida capturada te retroalimenta.

Construye el ciclo de retroalimentación correcto y el error está 90% resuelto.

### Ajusta el ciclo

Trata el ciclo como un producto. Una vez que tengas _un_ ciclo, **ajústalo**:

- ¿Puedo hacerlo más rápido? (Cachear la preparación, saltar inicialización irrelevante, acotar el alcance de la prueba.)
- ¿Puedo afinar la señal? (Verificar el síntoma específico, no "no se cayó".)
- ¿Puedo hacerlo más determinista? (Fijar el tiempo, fijar la semilla del RNG, aislar el sistema de archivos, congelar la red.)

Un ciclo inestable de 30 segundos apenas es mejor que ninguno; uno determinista de 2 segundos es ajustado, un superpoder para depurar.

### Errores no deterministas

El objetivo no es una reproducción limpia sino una **tasa de reproducción más alta**. Repite el disparador 100 veces, paraleliza, añade estrés, estrecha las ventanas de tiempo, inyecta pausas. Un error que falla el 50% es depurable; uno del 1% no, así que sigue subiendo la tasa hasta que sea depurable.

### Cuando de verdad no puedes construir un ciclo

Detente y dilo explícitamente. Lista lo que intentaste. Pide al usuario: (a) acceso al entorno que lo reproduce, (b) un artefacto capturado y ocultado (archivo HAR, volcado de logs, core dump, grabación de pantalla con marcas de tiempo), o (c) permiso para añadir instrumentación temporal en producción. **No** pases a hipotetizar sin un ciclo.

### Criterio de fin: un ciclo ajustado que se ponga en rojo

La Fase 1 termina cuando el ciclo es **ajustado** y **capaz de ponerse en rojo**: puedes nombrar **un comando** (una ruta de script, una invocación de prueba, un curl) que **ya ejecutaste al menos una vez** (muestra la invocación y su salida, ocultada), y que:

- [ ] **Puede ponerse en rojo**: recorre la ruta real del error y verifica el **síntoma exacto del usuario**, así puede fallar con este error y pasar cuando se corrija. No "corre sin romperse"; debe poder _atrapar este error específico_.
- [ ] **Determinista**: mismo veredicto en cada ejecución (errores inestables: tasa de reproducción alta y fijada, según lo anterior).
- [ ] **Rápido**: segundos, no minutos.
- [ ] **Ejecutable por el agente**: puedes correrlo sin supervisión; una persona participa solo vía `scripts/hitl-loop.template.sh`.

Si te descubres leyendo código para armar una teoría antes de que exista este comando, **detente: saltar directo a una hipótesis es justo el fallo que esta skill evita.** Sin comando capaz de ponerse en rojo, no hay Fase 2.

## Fase 2: Reproduce + minimiza

Corre el ciclo. Míralo ponerse en rojo cuando aparece el error.

Confirma:

- [ ] El ciclo produce el modo de fallo que el **usuario** describió, no otro fallo distinto que casualmente está cerca. Error equivocado = corrección equivocada.
- [ ] El fallo es reproducible en varias ejecuciones (o, para errores no deterministas, reproducible con tasa suficiente para depurar).
- [ ] Capturaste el síntoma exacto (mensaje de error, salida incorrecta, tiempo lento) para que las fases siguientes verifiquen que la corrección lo resuelve de verdad.

### Minimiza

Una vez en rojo, reduce la reproducción al **escenario más pequeño que siga en rojo**. Recorta entradas, llamadores, configuración, datos y pasos **uno a la vez**, re-ejecutando el ciclo tras cada recorte, y conserva solo lo que sostiene el fallo.

Por qué molestarse: una reproducción mínima achica el espacio de hipótesis en la Fase 3 (menos piezas sospechosas) y se convierte en la prueba de regresión limpia en la Fase 5.

Termina cuando **cada elemento restante sostiene el fallo**: quitar cualquiera de ellos pone el ciclo en verde.

No avances hasta haber reproducido **y** minimizado.

## Fase 3: Hipotetiza

Genera **de 3 a 5 hipótesis ordenadas** antes de probar ninguna. Generar una sola hipótesis te ancla en la primera idea plausible.

Cada hipótesis debe ser **falsable**: indica la predicción que hace.

> Formato: "Si <X> es la causa, entonces <cambiar Y> hará que el error desaparezca / <cambiar Z> lo empeorará."

Si no puedes enunciar la predicción, la hipótesis es una corazonada: descártala o afílala.

**Muestra la lista ordenada al usuario antes de probar.** A menudo tiene conocimiento del dominio que reordena al instante ("acabamos de desplegar un cambio en el #3"), o sabe qué hipótesis ya descartó. Punto de control barato, gran ahorro de tiempo. No te bloquees; sigue con tu orden si el usuario no está.

## Fase 4: Instrumenta

Cada sonda debe corresponder a una predicción específica de la Fase 3. **Cambia una variable a la vez.**

Preferencia de herramientas:

1. **Depurador / inspección REPL** si el entorno lo permite. Un breakpoint vale más que diez logs.
2. **Logs dirigidos** en las fronteras que distinguen hipótesis.
3. Nunca "registra todo y filtra con grep".

**Etiqueta cada log de depuración** con un prefijo único, p. ej. `[DEBUG-a4f2]`. La limpieza final se vuelve un solo grep. Los logs sin etiqueta sobreviven; los etiquetados mueren.

**Rama de rendimiento.** Para regresiones de rendimiento, los logs suelen ser lo incorrecto. En su lugar: establece una medición base (arnés de tiempos, `performance.now()`, profiler, plan de consulta), luego biseca. Mide primero, corrige después.

## Fase 5: Corrige + prueba de regresión

Escribe la prueba de regresión **antes de la corrección**, pero solo si hay un **seam correcto** para ella.

Un seam correcto es uno donde la prueba ejercita el **patrón real del error** tal como ocurre en el punto de llamada. Si el único seam disponible es demasiado superficial (prueba de un solo llamador cuando el error necesita varios llamadores, prueba unitaria que no puede replicar la cadena que disparó el error), una prueba de regresión ahí da falsa confianza.

**Si no existe un seam correcto, ese es el hallazgo.** Anótalo. La arquitectura del código impide blindar el error. Márcalo para la siguiente fase.

Si existe un seam correcto:

1. Convierte la reproducción minimizada en una prueba que falla en ese seam.
2. Mírala fallar.
3. Aplica la corrección.
4. Mírala pasar.
5. Re-ejecuta el ciclo de la Fase 1 contra el escenario original (sin minimizar).

## Fase 6: Limpieza

Obligatorio antes de declarar terminado:

- [ ] La reproducción original ya no reproduce (re-ejecuta el ciclo de la Fase 1)
- [ ] La prueba de regresión pasa (o la ausencia de seam está documentada)
- [ ] Toda la instrumentación `[DEBUG-...]` eliminada (busca con `grep` el prefijo)
- [ ] Prototipos desechables borrados (o movidos a una ubicación de depuración claramente marcada)
- [ ] La hipótesis que resultó correcta está enunciada en el mensaje del commit / PR, para que el próximo que depure aprenda
