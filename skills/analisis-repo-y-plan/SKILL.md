---
name: analisis-repo-y-plan
description: Ayudante de entrada Rigor para reproducir repos de deep learning empezando por el README. Rigor Intake helper for README-first deep learning repo reproduction. Use when the task is specifically to scan a repository, read the README and common project files, extract documented commands, classify inference, evaluation, and training candidates, and return the smallest trustworthy reproduction plan to the main orchestrator. Do not use for environment setup, asset download, command execution, final reporting, paper lookup, or end-to-end orchestration.
---

# analisis-repo-y-plan

Úsalo como ayudante de entrada (Intake) de Rigor. El slug instalado es
`analisis-repo-y-plan` por compatibilidad.

## Cuándo usarlo

- Al empezar un trabajo de reproducción que parte del README.
- Cuando la skill principal necesita un mapa rápido de la estructura del repo y sus comandos documentados.
- Cuando hay que clasificar con prudencia los candidatos de inferencia (inference), evaluación (evaluation) y entrenamiento (training).
- Cuando el usuario quiere revisar el repo primero y todavía no ejecutar nada.

## Cuándo no usarlo

- Cuando la ejecución ya empezó y la tarea ahora es correr comandos o escribir salidas.
- Cuando el objetivo no es una tarea de reproducción apoyada en un repositorio.
- Cuando el usuario solo quiere interpretar un artículo (paper) sin revisar el repo.
- Cuando el usuario ya tiene un comando documentado elegido y solo necesita preparación o ejecución.

## Límites claros

- Esta skill explora y planea.
- Esta skill es de nivel ayudante y normalmente la llama el orquestador.
- No instala entornos.
- No prepara archivos grandes (assets).
- No ejecuta comandos importantes de reproducción.
- No decide parches riesgosos.

## Lo que necesita de entrada

- Ruta del repositorio objetivo.
- Acceso al README y a los archivos comunes del proyecto, si existen.
- Pistas opcionales del usuario sobre la prioridad, por ejemplo inferencia-primero (inference-first).

## Lo que debe entregar

- Resumen corto de la estructura del repo
- Inventario de comandos documentados
- Categorías inferidas de candidatos: inferencia (inference), evaluación (evaluation), entrenamiento (training), otros
- Recomendación mínima y confiable de reproducción
- Lista de ambigüedades o riesgos notables

## Notas

Usa `references/repo-scan-rules.md` y los scripts de ayuda en `scripts/`.
