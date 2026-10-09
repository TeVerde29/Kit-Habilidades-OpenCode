# Skill: buscar-habilidades

Descubre e instala skills del ecosistema abierto
(`npx skills`). Se auto-activa cuando el usuario
pregunta "cómo hago X", "busca una skill para X"
o quiere ampliar capacidades.

## Flujo

1. Entiende dominio y tarea específica.
2. Revisa primero el ranking en https://skills.sh/.
3. Si no basta, busca: `npx skills find [query]`.
4. Verifica calidad antes de recomendar: +1K
   instalaciones, fuente confiable (`vercel-labs`,
   `anthropics`, `microsoft`), estrellas en GitHub.
5. Presenta opciones con nombre, instalaciones,
   comando de instalación y enlace a skills.sh.
6. Ofrece instalar: `npx skills add <owner/repo@skill> -g -y`.

## Si no hay nada

- Reconócelo, ofrece ayudar directamente y sugiere
  crear una propia con `npx skills init`.
