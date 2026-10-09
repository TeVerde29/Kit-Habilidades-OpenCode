# Skill: despliegue-seguro

Despliegue de cambios amplios: prepara el repo con hooks
pre-commit y barandas git, valida todo, commitea en español
con contexto completo y pushea. Se auto-activa cuando el
usuario pide deploy o commit grande.

## 1. Mensaje (igual que commit-rapido)

Mismas reglas: `<type>(<scope>): <resumen>` en español
imperativo, ≤72 chars, body obligatorio aquí porque el
cambio es amplio: qué cambió, por qué, riesgos, migración.
Refs al final: `Closes #N`.

## 2. Pre-commit completo (de setup-pre-commit)

1. Detecta package manager: `package-lock.json` npm,
   `pnpm-lock.yaml` pnpm, `yarn.lock` yarn, `bun.lockb` bun.
   Default npm.
2. Instala devDeps: `husky lint-staged prettier`.
3. `npx husky init` (crea `.husky/` + script `prepare`).
4. Escribe `.husky/pre-commit` (Husky v9+, sin shebang):
   `npx lint-staged` + `<pm> run typecheck` + `<pm> run test`.
   Si el repo no tiene esos scripts, omite la línea y avisa.
5. `.lintstagedrc`: `{ "*": "prettier --ignore-unknown --write" }`.
6. `.prettierrc` solo si no hay config: `useTabs false`,
   `tabWidth 2`, `printWidth 80`, `singleQuote false`,
   `trailingComma es5`, `semi true`, `arrowParens always`.
7. Verifica: `.husky/pre-commit` ejecutable, `.lintstagedrc`
   existe, `prepare: husky` en package.json, config prettier
   existe, y corre `npx lint-staged`.
8. Si algo falla, detente y repórtalo. No sigas al commit.

## 3. Barandas git (de git-guardrails-claude-code)

Bloqueado para el agente: `git push` (todas las variantes),
`reset --hard`, `clean -f`/`-fd`, `branch -D`,
`checkout .`, `restore .`, `push --force`.
Pregunta scope: solo proyecto (`.claude/settings.json`) o
global (`~/.claude/settings.json`). Instala el hook
`block-dangerous-git.sh` (patrones de arriba, sale con
código 2 y mensaje BLOCKED) y mézclalo en `hooks.PreToolUse`
sin borrar otros settings. Verifica con:
`echo '{"tool_input":{"command":"git push origin main"}}'`
pasado al script: debe salir código 2.

## 4. Flujo de despliegue

1. Revisa el diff completo antes de commitear.
2. Corre freno del punto 2. Si falla, para.
3. Commit de lo staged con mensaje + body completo.
4. Push normal a la rama actual. Nada forzado.
5. Reporta en simple: qué se subió, a qué rama, y qué
   quedó pendiente si algo falló.
