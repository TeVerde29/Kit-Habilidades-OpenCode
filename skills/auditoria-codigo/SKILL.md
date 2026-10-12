---
name: auditoria-codigo
description: >
  Auditoría completa de código, solo lectura. Detecta código
  espagueti, malas prácticas y mala estructura, y entrega
  informe con notas, graves con archivo:línea y ruta de fix.
  Auto-activate on "audita esto", "informe completo",
  "revisa el código" or /auditoria-codigo.
---

Auditoría completa de código, solo lectura. Detecta código
espagueti, malas prácticas y mala estructura, y entrega
informe con notas, graves con archivo:línea y ruta de fix.
PROHIBIDO modificar código. Se activa con "audita esto",
"informe completo", "revisa el código" o /auditoria-codigo.

## Regla de oro

Solo lee y analiza. Cero ediciones, cero commits, cero
instalaciones. Si algo no se puede leer, se reporta como
"No verificado", nunca se inventa.

## Los 6 ejes (todos, siempre)

1. **Estructura backend.** Capas separadas (rutas finas,
   SQL en un solo lugar), config extraída, middlewares
   puros. Marca god-controllers y mezcla de capas.
2. **Estructura frontend.** Componentes chicos, servicios
   por entidad, guards. Marca componentes gigantes
   (+400 líneas), lógica duplicada, CSS con deuda.
3. **Código espagueti.** Busca: anidamiento +3 niveles,
   funciones +50 líneas, archivos +500 líneas, variables
   globales abusadas, copiar-pegar en 3+ lugares,
   imports circulares, `any` que esconde errores.
4. **Seguridad.** Queries parametrizadas, hash de claves,
   validación de entradas, auth en rutas sensibles,
   secretos fuera del código, cookies bien puestas,
   rate-limit en login y endpoints caros.
5. **Errores y testing.** Sin fuga de stacks, códigos HTTP
   coherentes, formato de respuesta único. Tests: qué hay,
   qué falta, qué es más urgente testear primero.
6. **Documentación y transversal.** README al día,
   comentarios que explican el porqué, git limpio
   (identidad única, sin secretos commiteados), deploy
   sano (env validados, estáticos bien servidos).

## Formato del informe (obligatorio)

1. **Veredicto general.** Tabla: eje, nota x/10, resumen
   de una línea. Más respuesta directa: "¿Es espagueti?
   Sí/No y por qué".
2. **Lo bueno.** 3-5 bullets de lo que está bien hecho.
3. **Los graves por eje.** Cada uno con archivo:línea,
   qué pasa, por qué es grave y cómo se arregla en una
   línea. Máximo 5 por eje, ordenados por severidad.
4. **Otros menores.** Lista corta sin detalle.
5. **Transversal.** Git, deploy, duplicación entre capas.
6. **Ruta sugerida.** Fixes ordenados por impacto/tiempo
   (qué hacer primero), sin implementarlos.
7. **Conclusión.** 3 líneas: nivel general, tipo de deuda
   (enredo vs duplicación) y si exige reescribir o no.
