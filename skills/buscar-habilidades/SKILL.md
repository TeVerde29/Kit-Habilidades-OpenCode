---
name: buscar-habilidades
description: Ayuda a descubrir e instalar skills de agentes cuando el usuario pregunta "cómo hago X", "busca una skill para X", "hay alguna skill que..." o quiere ampliar capacidades. Úsala cuando el usuario busque funcionalidad que pueda existir como skill instalable.
---

# Buscar habilidades

Esta skill te ayuda a descubrir e instalar skills del ecosistema abierto de skills de agentes.

## Cuándo usar esta skill

Usa esta skill cuando el usuario:

- Pregunta "cómo hago X" donde X puede ser una tarea común con una skill existente
- Dice "busca una skill para X" o "hay alguna skill para X"
- Pregunta "puedes hacer X" donde X es una capacidad especializada
- Muestra interés en ampliar las capacidades del agente
- Quiere buscar herramientas, plantillas o flujos de trabajo
- Menciona que le gustaría ayuda con un dominio específico (diseño, pruebas, despliegue, etc.)

## Qué es el CLI de skills

El CLI de skills (`npx skills`) es el gestor de paquetes del ecosistema abierto de skills de agentes. Las skills son paquetes modulares que amplían las capacidades del agente con conocimiento especializado, flujos de trabajo y herramientas.

**Comandos clave:**

- `npx skills find [query] [--owner <owner>]` - Busca skills de forma interactiva o por palabra clave, opcionalmente limitado a un owner de GitHub
- `npx skills add <package>` - Instala una skill desde GitHub u otras fuentes
- `npx skills update` - Actualiza todas las skills instaladas

**Explora skills en:** https://skills.sh/

## Cómo ayudar a encontrar skills

### Paso 1: Entender lo que necesitan

Cuando un usuario pide ayuda con algo, identifica:

1. El dominio (p. ej. React, pruebas, diseño, despliegue)
2. La tarea específica (p. ej. escribir pruebas, crear animaciones, revisar PRs)
3. Si es una tarea lo bastante común para que probablemente exista una skill

### Paso 2: Revisar primero el ranking

Antes de buscar con el CLI, revisa el [ranking de skills.sh](https://skills.sh/) para ver si ya existe una skill conocida para ese dominio. El ranking ordena las skills por instalaciones totales, mostrando las más populares y probadas.

Por ejemplo, las mejores skills para desarrollo web incluyen:
- `vercel-labs/agent-skills` — React, Next.js, diseño web (más de 100K instalaciones cada una)
- `anthropics/skills` — Diseño frontend, procesamiento de documentos (más de 100K instalaciones)

### Paso 3: Buscar skills

Si el ranking no cubre lo que necesita el usuario, ejecuta el comando de búsqueda:

```bash
npx skills find [query] [--owner <owner>]
```

Por ejemplo:

- El usuario pregunta "cómo hago mi app React más rápida" → `npx skills find react performance`
- El usuario pregunta "me ayudas con revisiones de PR" → `npx skills find pr review`
- El usuario pregunta "necesito crear un changelog" → `npx skills find changelog`

### Paso 4: Verificar calidad antes de recomendar

**No recomiendes una skill basándote solo en los resultados de búsqueda.** Verifica siempre:

1. **Número de instalaciones** — Prefiere skills con más de 1K instalaciones. Ten cuidado con cualquiera por debajo de 100.
2. **Reputación de la fuente** — Las fuentes oficiales (`vercel-labs`, `anthropics`, `microsoft`) son más confiables que autores desconocidos.
3. **Estrellas en GitHub** — Revisa el repositorio fuente. Una skill de un repo con menos de 100 estrellas debe mirarse con escepticismo.

### Paso 5: Presentar opciones al usuario

Cuando encuentres skills relevantes, preséntalas al usuario con:

1. El nombre de la skill y lo que hace
2. El número de instalaciones y la fuente
3. El comando de instalación que puede ejecutar
4. Un enlace para saber más en skills.sh

Ejemplo de respuesta:

```
Encontré una skill que puede ayudar. La skill "react-best-practices" trae
guías de optimización de rendimiento para React y Next.js de Vercel Engineering.
(185K instalaciones)

Para instalarla:
npx skills add vercel-labs/agent-skills@react-best-practices

Saber más: https://skills.sh/vercel-labs/agent-skills/react-best-practices
```

### Paso 6: Ofrecer instalarla

Si el usuario quiere seguir adelante, puedes instalar la skill por él:

```bash
npx skills add <owner/repo@skill> -g -y
```

El flag `-g` instala a nivel global (nivel usuario) y `-y` salta las confirmaciones.

## Categorías comunes de skills

Al buscar, considera estas categorías comunes:

| Categoría        | Consultas de ejemplo                     |
| --------------- | ---------------------------------------- |
| Desarrollo web  | react, nextjs, typescript, css, tailwind |
| Pruebas         | testing, jest, playwright, e2e           |
| DevOps          | deploy, docker, kubernetes, ci-cd        |
| Documentación   | docs, readme, changelog, api-docs        |
| Calidad código  | review, lint, refactor, best-practices   |
| Diseño          | ui, ux, design-system, accessibility     |
| Productividad   | workflow, automation, git                |

## Consejos para búsquedas efectivas

1. **Usa palabras clave específicas**: "react testing" es mejor que solo "testing"
2. **Prueba términos alternos**: si "deploy" no funciona, prueba "deployment" o "ci-cd"
3. **Revisa fuentes populares**: muchas skills vienen de `vercel-labs/agent-skills` o `ComposioHQ/awesome-claude-skills`

## Cuando no se encuentran skills

Si no existen skills relevantes:

1. Reconoce que no se encontró ninguna skill existente
2. Ofrece ayudar con la tarea directamente usando tus capacidades generales
3. Sugiere que el usuario puede crear su propia skill con `npx skills init`

Ejemplo:

```
Busqué skills relacionadas con "xyz" pero no encontré coincidencias.
¡Igual puedo ayudarte con esta tarea directamente! ¿Quieres que siga?

Si esto es algo que haces a menudo, puedes crear tu propia skill:
npx skills init my-xyz-skill
```
