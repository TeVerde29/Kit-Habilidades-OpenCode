---
name: design-ricra
description: Sistema de diseño visual "Tea Green" — tipografía, tamaños, paleta de color, temas claro/oscuro, estados, radios, sombras y reglas de contraste. Úsala al crear o revisar UI para aplicar los tokens, tipografías y colores Tea Green.
---

# Design Ricra · Sistema de diseño Tea Green

## 1. Dirección de arte

Estética natural, moderna, minimalista, elegante y tecnológica, inspirada en hojas de té verde, matcha, verde salvia y tonos crema. Debe transmitir profesionalismo, pensamiento analítico, cuidado por el detalle y capacidad técnica.

**Evitar:** verdes neón, apariencia gamer, fondos negros puros, exceso de glassmorphism e interfaces saturadas.

## 2. Tipografía

| Uso | Fuente |
|---|---|
| Titulares | Space Grotesk |
| Texto y UI | Inter o Manrope |

**Escala tipográfica de referencia** (ajustable):

| Elemento | Tamaño | Peso |
|---|---|---|
| H1 (hero) | 48–64 px (móvil: 36–40 px) | 700 |
| H2 (secciones) | 32–40 px (móvil: 28 px) | 600–700 |
| H3 (tarjetas) | 20–24 px | 600 |
| Texto base | 16 px | 400 |
| Texto secundario / etiquetas | 14 px | 400–500 |
| Chips y metadatos | 12–13 px | 500 |

Jerarquía tipográfica clara, interlineado cómodo (≈1.5–1.7 en texto corrido, ≈1.1–1.25 en titulares).

## 3. Paleta oficial

### Colores de marca

| Nombre | Hex |
|---|---|
| Tea Green | `#AFC49A` |
| Forest Green | `#294635` |
| Deep Leaf | `#4F7256` |
| Sage Green | `#829B78` |
| Light Matcha | `#DDE8C8` |

### Tema claro

| Token | Hex |
|---|---|
| Fondo | `#F7F8F1` |
| Superficie de tarjetas | `#FFFFFF` |
| Superficie secundaria | `#EEF2E8` |
| Texto principal | `#1E2A21` |
| Texto secundario | `#5E6E5F` |
| Bordes y separadores | `#D9E2D5` |
| Foco y selección | `#567B3B` |

### Tema oscuro

| Token | Hex |
|---|---|
| Fondo | `#111A13` |
| Superficie de tarjetas | `#19241B` |
| Superficie elevada | `#222F24` |
| Verde de acento | `#C2D7A3` |
| Texto principal | `#F1F5EB` |
| Texto secundario | `#B5C0B2` |
| Bordes | `#334436` |

### Estados de interfaz

| Estado | Hex |
|---|---|
| Éxito | `#337652` |
| Advertencia | `#976719` |
| Error | `#B44743` |
| Información | `#3F7082` |

## 4. Reglas de aplicación del color

1. Fondo predeterminado del tema claro: `#F7F8F1`.
2. Títulos y contenido principal: `#1E2A21`.
3. `#294635` se reserva para botones primarios, navegación activa y elementos importantes.
4. `#AFC49A` y `#DDE8C8` se usan como acentos, fondos de etiquetas, elementos gráficos y resaltados discretos.
5. Tarjetas principalmente blancas, con borde `#D9E2D5` y sombras suaves.
6. `#EEF2E8` para superficies secundarias y pequeñas áreas de contraste.
7. Evitar fondos verdes intensos en secciones completas si perjudican la legibilidad.
8. Tema oscuro: fondo `#111A13`, tarjetas `#19241B`; ajustar colores de texto y controles para conservar el contraste.
9. Tema oscuro: botones primarios con fondo `#C2D7A3` y texto `#1E2A21`.
10. Estados hover, focus, disabled, error y success aplicados de forma coherente.
11. Texto con contraste mínimo WCAG AA (4.5:1 texto normal, 3:1 texto grande y componentes de UI).
12. Cada token de color mantiene el mismo significado en toda la interfaz.
13. No introducir colores de marca adicionales; los fondos especiales usan únicamente colores del sistema.

## 5. Fondos especiales

- **Hero:** fondo claro con degradado muy sutil entre `#F7F8F1` y `#E8EFDF`.

## 6. Forma, espacio y profundidad

- Espacios generosos.
- Radios moderados (referencia: 8–16 px; chips y botones hasta 999 px si se desea forma de píldora).
- Bordes finos (1 px).
- Sombras discretas y suaves, de baja opacidad.
- Lenguaje visual editorial, moderno y profesional.

## 7. Responsive

Los mismos tokens se aplican a escritorio, tablet y móvil. Botones, títulos, tarjetas y formularios deben conservar contraste y legibilidad en todas las resoluciones.

## 8. Tokens CSS de referencia

```css
:root {
  /* Marca */
  --tea-green: #AFC49A;
  --forest-green: #294635;
  --deep-leaf: #4F7256;
  --sage-green: #829B78;
  --light-matcha: #DDE8C8;

  /* Tema claro */
  --bg: #F7F8F1;
  --surface: #FFFFFF;
  --surface-2: #EEF2E8;
  --text: #1E2A21;
  --text-2: #5E6E5F;
  --border: #D9E2D5;
  --focus: #567B3B;
  --hero-gradient: linear-gradient(180deg, #F7F8F1, #E8EFDF);

  /* Estados */
  --success: #337652;
  --warning: #976719;
  --error: #B44743;
  --info: #3F7082;

  /* Tipografía */
  --font-heading: "Space Grotesk", sans-serif;
  --font-body: "Inter", "Manrope", sans-serif;
}

[data-theme="dark"] {
  --bg: #111A13;
  --surface: #19241B;
  --surface-2: #222F24;
  --accent: #C2D7A3;
  --text: #F1F5EB;
  --text-2: #B5C0B2;
  --border: #334436;
}
```
