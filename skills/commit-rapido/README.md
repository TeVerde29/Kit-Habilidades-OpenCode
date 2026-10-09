# Skill: commit-rapido

Commit rápido del día: mensaje terse en español + frenos
mínimos. Se auto-activa en cada commit.

## Mensaje

- `<type>(<scope>): <resumen>` en español imperativo:
  "agrega", "corrige", "elimina".
- Types: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`,
  `chore`, `build`, `ci`, `style`, `revert`.
- ≤50 chars ideal, 72 máximo, sin punto final.
- Body solo si el porqué no es obvio. Sin emojis, sin "yo",
  sin atribución a IA.

## Frenos

- Antes del commit corre lo que exista: formato, typecheck,
  tests. Si falla algo, para y repórtalo, no commitees.
- Prohibido: `push --force`, `reset --hard`, `clean -f`,
  `branch -D`, `checkout .`, `restore .`.
- Hace `git add .` antes del commit.
  Push normal a la rama actual. Si falla, detente y avisa.
