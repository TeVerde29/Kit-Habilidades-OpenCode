---
name: mejorar-diseno-existente
description: Mejora sitios y apps existentes a calidad premium. Upgrades existing websites and apps to premium quality. Use when redesigning an existing project, auditing current design, identifying generic AI patterns, or applying high-end design standards without breaking functionality. Works with any CSS framework or vanilla CSS.
---

# Skill de rediseño

## Cómo funciona

Al aplicarla a un proyecto existente, sigue esta secuencia:

1. **Explora (Scan)** — Lee el código. Identifica el framework, el método de estilos (Tailwind, CSS puro, styled-components, etc.) y los patrones de diseño actuales.
2. **Diagnostica (Diagnose)** — Repasa la auditoría de abajo. Lista cada patrón genérico, punto débil y estado faltante que encuentres.
3. **Arregla (Fix)** — Aplica mejoras puntuales trabajando con lo que ya hay. No reescribas desde cero. Mejora lo existente.

## Auditoría de diseño

### Tipografía

Revisa estos problemas y arréglalos:

- **Fuentes por defecto del navegador o Inter en todas partes.** Cambia por una fuente con carácter. Buenas opciones: `Geist`, `Outfit`, `Cabinet Grotesk`, `Satoshi`. Para proyectos editoriales o creativos, combina un serif para títulos con un sans-serif para el cuerpo.
- **Titulares sin presencia.** Agranda el texto destacado (display), aprieta el espacio entre letras (letter-spacing) y reduce la altura de línea (line-height). Los titulares deben sentirse pesados e intencionales.
- **Texto del cuerpo muy ancho.** Limita el ancho de los párrafos a unos 65 caracteres. Aumenta la altura de línea para que se lea mejor.
- **Solo se usan Regular (400) y Bold (700).** Agrega Medium (500) y SemiBold (600) para una jerarquía más sutil.
- **Números en fuente proporcional.** Usa una fuente monoespaciada o activa cifras tabulares (`font-variant-numeric: tabular-nums`) para interfaces con muchos datos.
- **Falta ajuste del espacio entre letras.** Usa seguimiento (tracking) negativo en títulos grandes y positivo en versalitas o etiquetas.
- **Subtítulos todo en mayúsculas.** Prueba itálicas en minúsculas, tipo oración o versalitas.
- **Palabras huérfanas.** Palabras solas en la última línea. Arrégalo con `text-wrap: balance` o `text-wrap: pretty`.

### Color y superficies

- **Fondo negro puro `#000000`.** Cámbialo por negro suave, gris carbón oscuro o un oscuro con tinte (`#0a0a0a`, `#121212` o un azul noche).
- **Colores de acento sobresaturados.** Mantén la saturación bajo el 80%. Baja la saturación de los acentos para que se mezclen con los neutros en vez de gritar.
- **Más de un color de acento.** Elige uno. Quita los demás. La consistencia gana sobre la variedad.
- **Mezcla de grises cálidos y fríos.** Quédate con una sola familia de grises. Tiñe todos los grises con un matiz consistente (cálido o frío, no ambos).
- **Estética de degradado morado/azul "de IA".** Es la huella más común del diseño hecho con IA. Cámbiala por bases neutras y un solo acento bien pensado.
- **`box-shadow` genérica.** Tiñe las sombras con el matiz del fondo. Usa sombras con color (p. ej. sombra azul oscura sobre fondo azul) en vez de negro puro con poca opacidad.
- **Diseño plano sin nada de textura.** Agrega ruido sutil, grano o micropatrones a los fondos. Los vectores planos y perfectos se sienten estériles.
- **Degradados perfectamente uniformes.** Rompe la uniformidad con degradados radiales, capas de ruido o degradados de malla en vez de los lineales típicos de 45 grados.
- **Dirección de luz inconsistente.** Revisa todas las sombras para que sugieran una sola fuente de luz.
- **Secciones oscuras al azar en una página clara (o al revés).** Una sola sección con fondo oscuro en medio de una página clara parece un pegado por accidente. O te comprometes con un modo oscuro completo o mantienes un tono de fondo consistente. Si necesitas contraste, usa un tono un poco más oscuro de la misma paleta, no un salto brusco a `#111` en medio de una página crema.
- **Secciones vacías y planas sin profundidad (depth) visual.** Las secciones que son solo texto sobre un fondo liso se sienten sin terminar. Agrega imágenes de fondo de calidad (difuminadas, con capas o enmascaradas), patrones sutiles o degradados de ambiente. Usa fuentes confiables de imágenes de relleno como `https://picsum.photos/seed/{name}/1920/1080` cuando no haya recursos reales. Prueba imágenes de fondo en héroes, bloques de funciones o llamadas a la acción (CTA): hasta una foto sutil a lo ancho y con poca opacidad da presencia.

### Diseño de página (layout)

- **Todo centrado y simétrico.** Rompe la simetría con márgenes desplazados, proporciones mezcladas o títulos alineados a la izquierda sobre contenido centrado.
- **Tres tarjetas iguales en fila como bloque de funciones.** Es el diseño más genérico de IA. Cámbialo por zig-zag de 2 columnas, rejilla asimétrica, desplazamiento horizontal o diseño de mosaico (masonry).
- **Usar `height: 100vh` para secciones de pantalla completa.** Cámbialo por `min-height: 100dvh` para evitar saltos en navegadores móviles (error del viewport en iOS Safari).
- **Matemática complicada de porcentajes con flexbox.** Usa CSS Grid para estructuras de varias columnas más confiables.
- **Sin contenedor con ancho máximo.** Agrega una restricción de contenedor (unos 1200-1440px) con márgenes automáticos para que el contenido no se estire de borde a borde en pantallas anchas.
- **Tarjetas de igual altura forzadas con flexbox.** Permite alturas variables o usa mosaico cuando el contenido varía de largo.
- **Mismo radio de borde en todo.** Varía el radio: más cerrado en elementos internos, más suave en contenedores.
- **Sin traslapes ni profundidad.** Los elementos están planos uno junto al otro. Usa márgenes negativos para crear capas y profundidad visual.
- **Relleno vertical simétrico.** El relleno de arriba y abajo siempre idéntico. Ajusta a ojo: el de abajo muchas veces necesita ser un poco mayor.
- **El panel siempre con barra lateral izquierda.** Prueba navegación superior, menú flotante de comandos o panel plegable.
- **Falta espacio en blanco.** Duplica el espaciado. Deja respirar el diseño. Los diseños densos sirven para paneles de datos, no para páginas de marketing.
- **Botones no alineados abajo en grupos de tarjetas.** Cuando las tarjetas tienen distinto largo de contenido, los botones quedan a alturas al azar. Fija los botones abajo de cada tarjeta para que formen una línea horizontal limpia sin importar el contenido de arriba.
- **Listas de funciones que empiezan a distinta altura.** En tablas de precios o tarjetas comparativas, la lista de funciones debe empezar en la misma posición Y en todas las columnas. Usa espaciado consistente arriba de la lista o bloques de título/precio con altura fija.
- **Ritmo vertical inconsistente en elementos lado a lado.** Al poner tarjetas, columnas o paneles juntos, alinea los elementos compartidos (títulos, descripciones, precios, botones) entre todos. Las líneas base desalineadas hacen que se vea roto.
- **Alineación matemática que a ojo se ve mal.** Centrar con matemática no siempre se ve centrado. Iconos junto a texto, botones de reproducir en círculos o texto en botones muchas veces necesitan ajustes ópticos de 1-2px para sentirse bien.

### Interactividad y estados

- **Sin estados hover en botones.** Agrega cambio de fondo, escala leve o desplazamiento al pasar el cursor.
- **Sin respuesta al presionar (active).** Agrega un `scale(0.98)` o `translateY(1px)` sutil al presionar para simular un clic físico.
- **Transiciones instantáneas sin duración.** Agrega transiciones suaves (200-300ms) a todos los elementos interactivos.
- **Falta anillo de foco.** Asegura indicadores de foco visibles para navegar con teclado. Es un requisito de accesibilidad, no opcional.
- **Sin estados de carga.** Cambia los círculos giratorios genéricos por cargadores esqueleto (skeleton) que sigan la forma del diseño.
- **Sin estados vacíos.** Un panel vacío que no muestra nada es una oportunidad perdida. Diseña una vista de "primeros pasos" bien compuesta.
- **Sin estados de error.** Agrega mensajes de error claros en línea para formularios. No uses `window.alert()`.
- **Enlaces muertos.** Botones que enlazan a `#`. O enlazan a destinos reales o se muestran deshabilitados.
- **Sin indicar la página actual en la navegación.** Dale otro estilo al enlace activo para que el usuario sepa dónde está.
- **Saltos al desplazar.** Los clics en anclas saltan de golpe. Agrega `scroll-behavior: smooth`.
- **Animaciones que usan `top`, `left`, `width`, `height`.** Cambia a `transform` y `opacity` para animaciones suaves con GPU.

### Contenido

- **Nombres genéricos como "John Doe" o "Jane Smith".** Usa nombres diversos que suenen reales.
- **Números falsos y redondos como `99.99%`, `50%`, `$100.00`.** Usa datos orgánicos y menos perfectos: `47.2%`, `$99.00`, `+1 (312) 847-1928`.
- **Empresas de relleno como "Acme Corp", "Nexus", "SmartFlow".** Inventa marcas creíbles según el contexto.
- **Clichés de textos hechos con IA.** Nunca uses "Elevate", "Seamless", "Unleash", "Next-Gen", "Game-changer", "Delve", "Tapestry" ni "In the world of...". Escribe simple y específico.
- **Signos de exclamación en mensajes de éxito.** Quítalos. Sé seguro, no ruidoso.
- **Mensajes de error tipo "Oops!".** Sé directo: "Conexión fallida. Inténtalo de nuevo."
- **Voz pasiva.** Usa voz activa: "No pudimos guardar tus cambios" en vez de "Se cometieron errores".
- **Todas las fechas del blog iguales.** Varía las fechas para que se vean reales.
- **Misma imagen de avatar para varios usuarios.** Usa recursos únicos para cada persona.
- **Lorem Ipsum.** Nunca uses texto latino de relleno. Escribe textos reales aunque sean borrador.
- **Mayúsculas en cada palabra de los títulos.** Usa tipo oración en vez de Título Con Mayúsculas.

### Patrones de componentes

- **Tarjeta genérica (borde + sombra + fondo blanco).** Quita el borde, o usa solo color de fondo, o usa solo espaciado. Las tarjetas solo deben existir cuando la elevación comunica jerarquía.
- **Siempre un botón relleno + un botón fantasma.** Agrega enlaces de texto o estilos terciarios para reducir el ruido visual.
- **Insignias "New" y "Beta" en forma de píldora.** Prueba insignias cuadradas, banderas o etiquetas de texto simple.
- **Secciones de preguntas con acordeón.** Usa lista lado a lado, ayuda con buscador o revelado progresivo en línea.
- **Testimonios en carrusel de 3 tarjetas con puntos.** Cambia por muro de mosaico, publicaciones sociales integradas o una sola cita rotativa.
- **Tabla de precios con 3 torres.** Destaca el plan recomendado con color y énfasis, no solo con más altura.
- **Modales para todo.** Usa edición en línea, paneles laterales deslizables o secciones expandibles en vez de ventanas para acciones simples.
- **Solo círculos para avatares.** Prueba squircles o cuadrados redondeados para un look menos genérico.
- **El cambio claro/oscuro siempre un interruptor sol/luna.** Usa un menú, detección de preferencia del sistema o intégralo en ajustes.
- **Pie con granja de enlaces de 4 columnas.** Simplifica. Enfócate en las rutas principales y los enlaces legales obligatorios.

### Iconografía

- **Solo iconos Lucide o Feather.** Son la opción "por defecto" de la IA. Usa Phosphor, Heroicons o un juego propio para diferenciarte.
- **Cohete para "lanzar", escudo para "seguridad".** Cambia las metáforas obvias por iconos menos vistos (rayo, huella, chispa, bóveda).
- **Grosores de trazo inconsistentes.** Revisa todos los iconos y unifica un solo grosor.
- **Falta favicon.** Siempre incluye un favicon de la marca.
- **Fotos de stock de "equipo diverso".** Usa fotos reales del equipo, tomas espontáneas o un estilo de ilustración consistente en vez de imágenes de stock extrañas.

### Calidad de código

- **Sopa de divs.** Usa HTML semántico: `<nav>`, `<main>`, `<article>`, `<aside>`, `<section>`.
- **Estilos en línea mezclados con clases CSS.** Mueve todo el estilo al sistema de estilos del proyecto.
- **Anchos fijos en píxeles.** Usa unidades relativas (`%`, `rem`, `em`, `max-width`) para diseños flexibles.
- **Falta texto alternativo en imágenes.** Describe el contenido para lectores de pantalla. Nunca dejes `alt=""` ni `alt="image"` en imágenes con significado.
- **Valores de z-index arbitrarios como `9999`.** Define una escala limpia de z-index en el tema o variables.
- **Código muerto comentado.** Quita todo resto de depuración antes de entregar.
- **Importaciones alucinadas.** Verifica que cada importación exista de verdad en `package.json` o las dependencias del proyecto.
- **Faltan metaetiquetas.** Agrega `<title>`, `description`, `og:image` y metaetiquetas para compartir en redes.

### Omisiones típicas (lo que la IA suele olvidar)

- **Sin enlaces legales.** Agrega política de privacidad y términos de servicio en el pie.
- **Sin navegación de "atrás".** Callejones sin salida en el flujo. Cada página necesita una forma de volver.
- **Sin página 404 propia.** Diseña una experiencia de "página no encontrada" útil y con marca.
- **Sin validación de formularios.** Agrega validación del lado del cliente para correos, campos obligatorios y formatos.
- **Sin enlace de "saltar al contenido".** Esencial para usuarios de teclado. Agrega un enlace oculto de salto.
- **Sin aviso de cookies.** Si la ley local lo exige, agrega un banner de consentimiento que cumpla.

## Técnicas de mejora

Al mejorar un proyecto, usa estas técnicas de alto impacto para reemplazar patrones genéricos:

### Mejoras de tipografía
- **Animación con fuentes variables.** Interpola el peso o el ancho al desplazar o al pasar el cursor, para texto que se siente vivo.
- **Transiciones de contorno a relleno.** El texto empieza como solo contorno y se rellena con color al entrar en pantalla o al interactuar.
- **Revelados con máscara de texto.** Tipografía grande que funciona como ventana a video o imágenes animadas detrás.

### Mejoras de diseño de página
- **Rejilla rota / asimetría.** Elementos que ignoran las columnas a propósito: se traslapan, se salen de pantalla o se desplazan con aleatoriedad calculada.
- **Espacio en blanco al máximo.** Uso agresivo del espacio negativo para forzar el foco en un solo elemento.
- **Pilas de tarjetas con parallax.** Secciones que se fijan y se apilan físicamente al desplazar.
- **Pantalla dividida con scroll.** Dos mitades que se deslizan en direcciones opuestas.

### Mejoras de movimiento
- **Scroll suave con inercia.** Separa el desplazamiento del navegador por defecto para una sensación más pesada y cinematográfica.
- **Entrada escalonada.** Los elementos caen en cascada con leves retrasos, combinando desplazamiento en Y con fundido de opacidad. Nunca muestres todo a la vez.
- **Física de resortes.** Cambia el easing lineal por movimiento con resortes para una sensación natural y con peso en todo lo interactivo.
- **Revelados atados al scroll.** Contenido que entra con máscaras que se expanden, barridos o trazos SVG que se dibujan según el avance del scroll.

### Mejoras de superficie
- **Cristal (glassmorphism) de verdad.** Más allá de `backdrop-filter: blur`. Agrega un borde interno de 1px y una sombra interna sutil para simular la refracción del borde.
- **Bordes con reflector.** Bordes de tarjetas que se iluminan siguiendo el cursor.
- **Capas de grano y ruido.** Una capa fija con ruido sutil y sin eventos de puntero para romper lo plano digital.
- **Sombras teñidas con color.** Sombras que llevan el matiz del fondo en vez de negro genérico.

## Prioridad de arreglos

Aplica los cambios en este orden para lograr el mayor impacto visual con el menor riesgo:

1. **Cambio de fuente** — la mejora instantánea más grande, el riesgo más bajo
2. **Limpieza de paleta de color** — quita colores que chocan o están sobresaturados
3. **Estados hover y active** — hace que la interfaz se sienta viva
4. **Diseño de página y espaciado** — rejilla correcta, ancho máximo, relleno consistente
5. **Reemplaza componentes genéricos** — cambia patrones obvios por alternativas modernas
6. **Agrega estados de carga, vacíos y de error** — hace que se sienta terminado
7. **Pule escala tipográfica y espaciado** — el toque final premium

## Reglas

- Trabaja con lo que ya hay. No migres de framework ni de librería de estilos.
- No rompas lo que ya funciona. Prueba después de cada cambio.
- Antes de importar una librería nueva, revisa primero el archivo de dependencias del proyecto.
- Si el proyecto usa Tailwind, revisa la versión (v3 vs v4) antes de tocar la configuración.
- Si el proyecto no tiene framework, usa CSS puro.
- Mantén los cambios revisables y enfocados. Mejoras pequeñas y puntuales sobre reescrituras grandes.
