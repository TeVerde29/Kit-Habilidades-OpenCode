---
name: caveman-commit
description: >
  Commit rápido del día: mensaje terse en español + frenos mínimos.
  Auto-activate on any commit request, staged changes, "haz commit",
  "commit message", /commit or /caveman-commit.
---

Commit rápido del día. Mensaje terse en español + frenos mínimos.
Se auto-activa en cada commit.

## Mensaje

**Idioma obligatorio:**
- Subject, body y footers siempre en español (excepto `type`, `scope` y términos de código, que se mantienen en inglés técnico).
- Modo imperativo en español: "agrega", "corrige", "elimina" — no "agregado", "agregando".

**Subject:**
- `<type>(<scope>): <resumen>` — `<scope>` opcional.
- Types: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `chore`, `build`, `ci`, `style`, `revert`.
- ≤50 chars ideal, 72 máximo, sin punto final.

**Body:**
- Solo si el porqué no es obvio.
- Sin emojis, sin "yo", sin atribución a IA.
- Refs al final si aplican: `Closes #N`.

## Frenos

1. Antes del commit corre lo que exista en el repo: formato, typecheck, tests. Si falla algo, para y repórtalo, no commitees.
2. Prohibido: `push --force`, `reset --hard`, `clean -f`, `branch -D`, `checkout .`, `restore .`.
3. Hace `git add .` antes del commit (agrega todo lo modificado/nuevo no ignorado).
4. Push normal a la rama actual. Si falla, detente y avisa.

## Ejemplo

```
fix(auth): corrige expiración de sesión

Closes #42
```
