---
name: auditoria-sitio-web
description: 'Audita un sitio web con el CLI de squirrelscan y corrige los hallazgos en el codigo. Audit a website with the squirrelscan CLI and fix the findings in code. Runs SEO, performance, security, technical, content, accessibility, and 15 other rule categories (260+ rules), returns an LLM-optimized report, then drives an iterative fix loop, mapping issues to source files, applying fixes, and re-auditing until the site scores well. Use to discover and assess website or webapp issues and drive them to fixed.'
license: See LICENSE file in repository root
compatibility: Requires squirrel CLI installed and accessible in PATH
metadata:
  author: squirrelscan
  version: "2.1"
allowed-tools: Bash(squirrel:*) Read Edit Grep Glob
---

# Audita un Sitio Web y Corrígelo

Ejecuta una auditoría de squirrelscan contra un sitio web, lee el informe LLM, relaciona cada problema con el código o contenido que lo causa, corrige por lotes y vuelve a auditar hasta alcanzar el objetivo de puntuación.

Requiere el CLI `squirrel` ([squirrelscan.com/download](https://squirrelscan.com/download); verifica con `squirrel --version`). Para configuración del CLI, login, publicación, MCP y uso general del CLI, usa la skill compañera `squirrelscan`.

## Documentación de reglas

Consulta cualquier regla en `https://docs.squirrelscan.com/rules/{rule_category}/{rule_id}`, por ejemplo:

https://docs.squirrelscan.com/rules/links/external-links

## Cómo ejecutar la auditoría

```bash
squirrel audit https://example.com --format llm
```

- Usa `--format llm`: es compacto, exhaustivo y está hecho para agentes.
- Si el usuario no da una URL, pregunta qué sitio auditar.
- Prefiere auditar el sitio en vivo: solo ahí ves el renderizado real, el rendimiento y el comportamiento de redirecciones. Si existen tanto un servidor local de desarrollo como un sitio en vivo, sugiere el sitio en vivo; aplica las correcciones al código local de todos modos.
- Las auditorías se guardan en caché local. Vuelve a mostrarlas después sin re-rastrear: `squirrel report <audit-id> --format llm`.

### Progresión del rastreo

1. **Primera pasada, cobertura rápida** (el valor por defecto): un rastreo rápido y superficial para conocer la estructura del sitio, su tecnología y sus problemas más grandes sin afectar al sitio.
2. **Segunda pasada, cobertura más profunda**: `-C surface` (una página por patrón de URL) para cobertura a nivel de plantillas, o `-C full` para un rastreo completo antes de la aprobación final.

| Modo | Páginas por defecto | Uso |
|------|---------------------|-----|
| `quick` | 25 | Primera vista, revisiones de CI |
| `surface` | 100 | Cobertura a nivel de plantillas (una muestra por patrón como `/blog/{slug}`) |
| `full` | 500 | Verificación final, análisis profundo |

Flags útiles: `--refresh` (ignora la caché, re-obtiene todo), `--resume` (continúa un rastreo interrumpido), `-m <n>` (límite de páginas), `--verbose` (detalle del progreso).

Si el sitio bloquea rastreadores desconocidos (Shopify / Cloudflare), pasa cabeceras de Web Bot Auth con flags repetidos `-H "Name: Value"`. Los valores de las cabeceras son secretos y se ocultan en la salida. Ver https://docs.squirrelscan.com/guides/web-bot-auth

## El ciclo de corrección

1. **Presenta el informe**: puntuación, calificación, problemas principales por severidad.
2. **Propón correcciones**: lista los problemas que puedes corregir y confirma con el usuario antes de cambiar nada.
3. **Relaciona problemas con fuente**: encuentra la plantilla, componente o archivo de contenido detrás de cada hallazgo.
4. **Corrige por lotes**: aplica las correcciones aprobadas.
5. **Vuelve a auditar** (usa `--refresh` tras despliegues o cambios de contenido) y muestra las puntuaciones antes/después.
6. **Repite** hasta cumplir el objetivo o hasta que solo queden decisiones de criterio (por ejemplo "¿debería eliminarse este enlace?"). Márcalas para revisión del usuario en lugar de adivinar.

Tras cada lote, verifica que el proyecto siga compilando y que las revisiones existentes pasen.

### Objetivos de puntuación

| Puntuación inicial | Objetivo | Trabajo esperado |
|--------------------|----------|------------------|
| < 50 (F) | 75+ (C) | Correcciones mayores |
| 50-70 (D) | 85+ (B) | Correcciones moderadas |
| 70-85 (C) | 90+ (A) | Pulido |
| > 85 (B+) | 95+ | Ajuste fino |

Aprueba contra un rastreo `-C full`, ya que la pasada rápida solo muestrea parte del sitio.

Las reglas llevan un nivel (error, warning, notice) y un rango (1-10): corrige primero los errores, luego los warnings de rango alto. Los hallazgos que necesitan edición de contenido cuentan igual que los que necesitan edición de código. Los enlaces rotos suelen necesitar una decisión humana (quitar, reemplazar o conservar): márcalos en lugar de adivinar.

## Cómo verificar regresiones

Compara contra una base para probar la mejora o detectar regresiones:

```bash
squirrel report --diff <baseline-audit-id> --format llm
squirrel report --regression-since example.com --format llm
```

## Finalización

Terminado significa: todos los errores corregidos; warnings corregidos o documentados como pendientes de revisión humana; una re-auditoría confirma la mejora; y el usuario vio la comparación de puntuación antes/después más un resumen de cada cambio hecho. Vuelve a auditar con regularidad para mantener el sitio sano. Si el usuario quiere compartir resultados, ofrece un informe publicado (ver la skill `squirrelscan`).

## Formato del informe

El informe LLM es un híbrido compacto de XML/texto optimizado para eficiencia de tokens: resumen con puntuación de salud, problemas agrupados por categoría con URLs afectadas, enlaces rotos y recomendaciones priorizadas. Especificación completa: [OUTPUT-FORMAT.md](references/OUTPUT-FORMAT.md)
