---
name: diseno-apple
description: El enfoque de Apple para el diseño de interfaces y el movimiento fluido y físico, traducido a la web. Úsala al crear o revisar UI con gestos, animaciones con springs, interacciones de arrastrar/deslizar/paneles, impulso y transiciones interrumpibles, materiales translúcidos y profundidad, tipografía (tamaño óptico, tracking, interlineado), movimiento reducido o los fundamentos de diseño (feedback, consistencia espacial, contención) detrás de las interfaces estilo Apple. | Apple's approach to interface design and fluid, physical motion, translated for the web. Use when building or reviewing gesture-driven UI, spring animations, drag/swipe/sheet interactions, momentum and interruptible transitions, translucent materials and depth, typography (optical sizing, tracking, leading), reduced-motion, or the design foundations (feedback, spatial consistency, restraint) behind Apple-style interfaces.
---

# Diseño Apple

## Respuesta inicial

Cuando esta skill se invoca por primera vez sin una pregunta concreta, responde solo con:

> Estoy listo para ayudarte a construir interfaces fluidas estilo Apple en la web, mi conocimiento viene de las charlas de diseño de la WWDC de Apple, traducido a la web.

No des ninguna otra información hasta que la persona usuaria haga una pregunta.

Cómo Apple construye interfaces que dejan de sentirse como un ordenador y empiezan a sentirse como una extensión de ti. Este conocimiento viene de las charlas de diseño de la WWDC de Apple — sobre todo *Designing Fluid Interfaces* (WWDC 2018) — destilado y traducido a la plataforma web (CSS, Pointer Events, `requestAnimationFrame`, librerías de springs como Motion/Framer Motion).

La idea central: **una interfaz se siente viva cuando el movimiento arranca desde el valor actual en pantalla, hereda la velocidad de la persona usuaria, proyecta el impulso hacia adelante y se puede agarrar y revertir en cualquier instante.** Los springs son la herramienta que hace todo esto natural, porque son interrumpibles por naturaleza y sensibles a la velocidad.

## La idea central

> "Cuando alineamos la interfaz con cómo pensamos y nos movemos, pasa algo mágico — deja de sentirse como un ordenador y empieza a sentirse como una extensión fluida de nosotros."

Una interfaz es fluida cuando se comporta como el mundo físico: las cosas responden al instante, se mueven de forma continua, llevan impulso, resisten en los bordes y se pueden redirigir en mitad del movimiento. Todo lo de abajo es una forma de acercarse a eso.

Apple plantea el diseño como servir a cuatro necesidades humanas: **seguridad/previsibilidad, comprensión, logro y alegría.** Cada regla aquí sirve a una de ellas.

## 1. Respuesta — elimina la latencia

En cuanto aparece el retardo, la sensación de control directo "se cae por un acantilado". La respuesta es la base sobre la que se construye todo lo demás.

- **Responde al pulsar (pointer-down), no al soltar.** Resalta un botón en el instante en que se pulsa. Esperar al `click`/soltar para mostrar feedback se siente muerto.
- **Vigila cada latencia.** Audita debounces, temporizadores artificiales, esperas de transición y los ~300ms de retardo del tap. Todo lo que esté en la ruta de entrada y no sea esencial es una regresión.
- **El feedback debe ser continuo *durante* la interacción, no solo al final.** En un arrastre, slider o panel, actualiza la UI 1:1 con el puntero durante todo el recorrido — nunca animes solo cuando el gesto termina.

```css
/* El feedback vive en la pulsación, y es instantáneo */
.button:active {
  transform: scale(0.97);
  transition: transform 100ms ease-out;
}
```

## 2. Manipulación directa — seguimiento 1:1

> "El tacto y el contenido deben moverse juntos."

Cuando la persona usuaria arrastra algo, debe quedarse pegado al dedo — y respetar el desplazamiento desde *donde lo agarró*. Saltar al centro del elemento al agarrar rompe la ilusión al instante.

- Usa Pointer Events con `setPointerCapture` para que el seguimiento continúe aunque el puntero salga de los límites del elemento.
- Guarda un historial corto de **velocidad/posición** (últimos eventos `pointermove`), no solo el punto actual — necesitarás la velocidad al soltar.

```js
el.addEventListener('pointerdown', (e) => {
  el.setPointerCapture(e.pointerId);
  const grabOffset = e.clientY - el.getBoundingClientRect().top; // respect where they grabbed
  // ...track position + timestamp history for velocity
});
```

## 3. Interrumpibilidad — el principio más importante

> "El pensamiento y el gesto ocurren en paralelo."

Toda animación debe poder interrumpirse y redirigirse en cualquier momento. Una persona debe poder agarrar un elemento en movimiento en pleno vuelo y revertirlo sin esperar a que la animación termine. Un modal que se está cerrando y la persona vuelve a agarrar debe seguir al dedo — no terminar de cerrarse primero y luego reabrirse.

- **Nunca bloquees la entrada durante una transición.**
- **Anima siempre desde el valor *actual* (presentation), nunca desde el valor objetivo.** Al interrumpir, lee la transformada viva en pantalla del elemento y arranca la nueva animación desde ahí. Empezar desde el valor lógico/objetivo causa un salto visible.
- **Evita CSS transitions y `@keyframes` para todo lo guiado por gestos** — no se pueden agarrar y revertir con suavidad en mitad del vuelo. Los springs animan desde el valor actual por defecto, que es justo lo que la interrupción necesita.
- **Cuando un gesto se revierte, mezcla la velocidad — no la cortes en seco.** Reemplazar una animación por otra en una reversión crea una discontinuidad de velocidad, un "muro de ladrillo". Las librerías de springs que conservan la velocidad al cambiar de objetivo lo evitan. (Esto es lo que iOS hace de forma nativa con *additive animations*; en la web, elige una librería de springs que cambie de objetivo desde la velocidad actual.)
- **Descompón el movimiento 2D en springs X e Y independientes.** Un solo spring sobre una distancia 2D se desincroniza cuando X e Y tienen velocidades distintas.

## 4. Comportamiento antes que animación — usa springs

> "Piensa la animación como una conversación entre tú y el objeto, no como algo prescrito por la interfaz."

Una animación preguionizada con duración fija no puede responder a nueva entrada. Un spring sí — la nueva entrada solo cambia el objetivo, y el movimiento sigue siendo continuo. Usa springs para todo lo que la persona pueda tocar.

Apple reemplazó a propósito el trío de física (mass/stiffness/damping) por dos parámetros amables para diseñadores. Piensa en estos:

- **Damping ratio** — controla el sobreimpulso. `1.0` = amortiguación crítica, sin rebote, asentamiento suave. `< 1.0` = sobrepasa y oscila. Más bajo = más rebote.
- **Response** — qué tan rápido el valor llega al objetivo, en segundos. Más bajo = más ágil. **Esto no es "duración"** — un spring no tiene duración fija; su tiempo de asentamiento surge de los parámetros.

**Valores por defecto:**
- Arranca la mayoría de la UI en **damping `1.0`** (amortiguación crítica) — elegante y sin distraer.
- Añade rebote (**damping ~`0.8`**) **solo cuando el propio gesto traía impulso** (un flick, un lanzamiento, soltar un arrastre). El sobreimpulso en un menú que acaba de aparecer se siente mal; el sobreimpulso en una tarjeta que lanzaste se siente bien.

**Valores concretos que Apple usa:**

| Interacción | Damping | Response |
| --- | --- | --- |
| Mover / reposicionar (p. ej. PiP) | `1.0` | `0.4` |
| Rotación | `0.8` | `0.4` |
| Cajón / panel (drawer / sheet) | `0.8` | `0.3` |

**Correspondencia en web (Motion / Framer Motion):** la API de springs con `bounce` + `duration` se corresponde bien con damping + response de Apple. Un estilo de casa seguro es springs con `damping: 1.0` en todas partes por defecto; reserva el rebote para interacciones físicas con impulso.

```js
import { animate } from 'motion';

// Critically damped default (no overshoot)
animate(el, { y: 0 }, { type: 'spring', bounce: 0, duration: 0.4 });

// Momentum interaction — a little bounce, only because a flick preceded it
animate(el, { y: target }, { type: 'spring', bounce: 0.2, duration: 0.4 });
```

## 5. Entrega de velocidad — la costura entre arrastre y animación

Cuando un gesto termina, la animación debe **continuar a la velocidad exacta del dedo**, para que no haya costura visible entre arrastrar y animar. Este es el detalle que más separa "fluido" de "aceptable".

Pasa la velocidad de soltado del puntero como velocidad inicial del spring. Algunas APIs de springs quieren velocidad **relativa** — normalízala por la distancia restante hasta el objetivo:

```
relativeVelocity = gestureVelocity / (targetValue − currentValue)
```

Ejemplo: elemento en `y=50`, objetivo `y=150` (quedan 100px), dedo moviéndose a 50px/s → velocidad inicial del spring = `50 / 100 = 0.5`. Framer Motion / Motion aceptan velocidad absoluta en px/s directamente (opción `velocity`), así que normalmente les pasas el valor tal cual.

## 6. Proyección de impulso — anima hacia donde *va* el gesto

> "Toma una entrada pequeña y produce una salida grande."

No saltes al borde más cercano desde el *punto de soltado*. Usa la velocidad para **proyectar la posición de reposo** — igual que la desaceleración del scroll — y luego salta al objetivo más cercano a ese punto proyectado. Esto es lo que hace que un flick se sienta como lanzar el elemento.

La función exacta de proyección de Apple (del código de ejemplo de *Designing Fluid Interfaces*):

```js
// decelerationRate ≈ 0.998 for normal scroll feel; 0.99 for snappier
function project(initialVelocity /* px/s */, decelerationRate = 0.998) {
  return (initialVelocity / 1000) * decelerationRate / (1 - decelerationRate);
}

const projectedEndpoint = currentPosition + project(releaseVelocity);
const target = nearestSnapPoint(projectedEndpoint);   // choose target from the projection
animateSpringTo(target, { velocity: releaseVelocity }); // then hand off velocity (§5)
```

Nota: la fórmula de libro de física `v²/(2·decel)` *no* es lo que Apple usa — usa la forma de decaimiento exponencial de arriba. Este es el comportamiento estándar en buenos bottom-sheets y carruseles (Vaul, Embla).

## 7. Consistencia espacial — rutas simétricas, orígenes anclados

> "Si algo desaparece en una dirección, esperamos que emerja de donde vino."

- **Entra y sal por el mismo camino.** Un panel que entra desde la derecha debe salir hacia la derecha. Entrar por la derecha / salir por abajo se siente desconectado y confunde.
- **Ancla las interacciones a su origen.** Un menú, popover o panel debe originarse desde el elemento que lo disparó — pon `transform-origin` en el disparador, para que la relación espacial entre botón y contenido sea obvia. (Es el mismo punto de conciencia de origen que los popovers que escalan desde su disparador, no desde su centro.)
- **Refleja el easing en transiciones reversibles** para que el camino de ida coincida con el de vuelta (usa puntos de control cubic-bézier inversos para las dos direcciones).

## 8. Sugiere en la dirección del gesto

Las personas predicen un estado final desde una trayectoria. El movimiento intermedio debe anticipar a dónde van las cosas — los módulos del Control Center "crecen hacia arriba y hacia tu dedo". Haz que los fotogramas intermedios apunten al resultado, no que solo interpolen a ciegas hacia él.

## 9. Rubber-banding — bordes suaves

En un borde, resiste de forma progresiva en lugar de parar en seco. Una parada dura se lee como "congelado"; una resistencia continua se lee como "responde, pero no hay nada más aquí". Aplica una amortiguación que aumenta cuanto más pase la persona el borde al arrastrar.

```js
// The further past the bound, the less the element follows — real things slow before they stop
function rubberband(overshoot, dimension, constant = 0.55) {
  return (overshoot * dimension * constant) / (dimension + constant * Math.abs(overshoot));
}
```

## 10. Detalles de diseño de gestos (la lista de "sensación")

- **Tap:** resalta al *pulsar* (instantáneo), confirma al *soltar*. Añade ~10px de histéresis/margen de toque alrededor del objetivo, y permite cancelar arrastrando fuera y volviendo.
- **Arrastrar/deslizar:** exige un pequeño umbral de movimiento (histéresis, ~10px) antes de fijar una dirección, y luego sigue 1:1.
- **Detecta todos los gestos plausibles en paralelo desde el primer movimiento**, y luego cancela con seguridad los perdedores cuando la intención esté clara. Evita reconocedores que solo informan un estado *final* (eventos tipo `swipeleft`) — tiran el seguimiento continuo que necesitas para el feedback.
- **Minimiza los retardos de desambiguación.** La detección de doble tap retrasa inevitablemente los taps simples; solo paga ese coste donde el doble tap exista de verdad.

## 11. Suavidad a nivel de fotograma

La suavidad trata de *qué hay en los fotogramas*, no solo de la tasa de fotogramas.

- Mantén el cambio de posición por fotograma por debajo del umbral de percepción para evitar parpadeo (strobing).
- Para movimiento muy rápido, un sutil **desenfoque/estiramiento de movimiento** codifica la velocidad y se lee mejor que una estela dura y nítida.
- `requestAnimationFrame` es el reloj sincronizado con la pantalla en la web (Apple usa `CADisplayLink`). Anima solo propiedades amables con el compositor — `transform` y `opacity` — y avisa con `will-change` donde el movimiento sea inminente.

## 12. Materiales y profundidad — la translucidez transmite jerarquía

Apple usa materiales translúcidos como una capa flotante funcional que da estructura sin robar el foco. En la web, aproxímalo con `backdrop-filter`.

- **Construye navs/barras/paneles como capas translúcidas** (`backdrop-filter: blur()` + un fondo semitransparente) con el contenido desplazándose por debajo — no barras opacas que consumen una franja fija.
- **El peso del material codifica jerarquía:** materiales más oscuros/pesados separan regiones estructurales (sidebars); materiales más ligeros llaman la atención a elementos interactivos (botones). **Nunca apiles una superficie translúcida clara sobre otra** — la legibilidad colapsa.
- **Las superficies más grandes deben leerse como más gruesas:** blur más fuerte + sombra más profunda que los chips pequeños. Piensa en sombra según contexto — más pesada sobre contenido denso/texto para separar, más ligera sobre fondos lisos.
- **Oscurece para enfocar, separa para mantener el flujo.** Una tarea modal empareja la superficie con un velo que oscurece y empuja el fondo hacia atrás/abajo. Un panel paralelo no bloqueante usa translucidez y desplazamiento *sin* velo para no romper el flujo. Para paneles apilados, oscurece y empuja progresivamente cada capa padre.
- **La vibrancia (vibrancy) mantiene el texto legible sobre fondos cambiantes.** Sobre superficies borrosas/translúcidas, no uses texto gris plano — usa más contraste, peso un poco mayor y un pequeño aumento de espaciado entre letras. Pon el color en una capa sólida, no en el frente translúcido.
- **Efectos de borde al hacer scroll, no divisores duros.** En lugar de un borde de 1px bajo un header fijo, funde una pequeña máscara de blur/degradado donde el contenido toca el chrome flotante — solo donde la UI flotante realmente solapa contenido.
- **Materializa, no solo fundas.** Para superficies de vidrio/blur, anima el radio de blur y la escala juntos al entrar/salir, para que la superficie se lea como un material real que llega y no como un simple fundido de opacidad.

```css
.toolbar {
  background: rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(20px) saturate(180%);
  border-top: 1px solid rgba(255, 255, 255, 0.4); /* bright top edge = light catching the material */
}
```

## 13. Feedback multimodal — movimiento + sonido + háptica

Tres reglas para combinar sentidos (de *Designing Audio-Haptic Experiences*):

1. **Causalidad** — debe ser obvio qué causó el feedback. Dispáralo en el evento causal real (el interruptor cambiando, el elemento encajando en su sitio), y ajusta su carácter a la fisicalidad de la acción.
2. **Armonía** — lo visual, el sonido y la háptica deben dispararse en el **mismo fotograma**. La latencia entre ellos destruye la ilusión. No dejes que una CSS transition retrase el audio/háptica (Vibration API).
3. **Utilidad** — añade feedback solo donde lo merezca. Reserva háptica/sonido para momentos con sentido (éxito, error, confirmación, encaje). El exceso entrena a ignorarlo todo.

## 14. Movimiento reducido y accesibilidad

Movimiento reducido no significa *sin* feedback — significa un equivalente más suave y no vestibular. Responde a tres señales independientes e intégralas en tus componentes:

- **`prefers-reduced-motion: reduce`** — reemplaza deslizamientos/springs/parallax por **fundidos cruzados de opacidad o transiciones estáticas** cortos. Quita el elástico/sobreimpulso. Mantén los cambios de opacidad/color que ayuden a comprender.
- **`prefers-reduced-transparency: reduce`** — haz las superficies translúcidas más opacas/sólidas: sube la opacidad del fondo, quita el blur.
- **`prefers-contrast: more`** — fondos casi sólidos con un borde definido y contrastado.

Además: evita fondos en movimiento a pantalla completa, oscilaciones lentas en bucle (cerca de 0.2 Hz / un ciclo cada 5s) y saltos bruscos de brillo (suaviza los cambios de tema oscuro↔claro). Haz semitransparentes los objetos grandes en movimiento mientras viajan, y funde las superficies grandes al reposicionarlas mucho y vuelve a mostrarlas al asentarse.

```css
@media (prefers-reduced-motion: reduce) {
  .sheet { transition: opacity 200ms ease; transform: none !important; }
}
@media (prefers-reduced-transparency: reduce) {
  .toolbar { background: white; backdrop-filter: none; }
}
```

## 15. Tipografía — tamaño óptico, tracking, interlineado

Apple diseña el texto para cambiar de forma con el tamaño; la misma disciplina aplica en la web. (De *The Details of UI Typography*, WWDC 2020.)

- **El tracking (letter-spacing) depende del tamaño — nunca un solo valor para todo.** El texto grande de display quiere tracking *negativo* (las letras se leen demasiado separadas al crecer); el texto pequeño quiere tracking un poco *positivo* para legibilidad. Un `letter-spacing` fijo está mal en algún sitio. Aprieta los encabezados, deja el cuerpo cerca de `0`.
- **El interlineado (line-height) va inverso al tamaño.** Apretado en encabezados grandes, más suelto en cuerpo. Auméntalo para escrituras con ascendentes/descendentes altos; apriétalo para UI densa con mucha información.
- **Construye jerarquía con peso + tamaño + interlineado como conjunto,** no solo con tamaño. Enfatiza con peso — da presencia sin ocupar más espacio.
- **Respeta el ajuste de tamaño de texto de la persona** (Dynamic Type). Escala la maquetación *con* el texto — espaciado en `rem`/`em`, no en px fijos — para que una fuente más grande no rompa la maquetación.
- **Usa por defecto la fuente del sistema de la plataforma** antes que una personalizada; ya trae tamaño óptico, tablas de tracking y ajuste de legibilidad. Cambia solo con motivo.

```css
:root { font: 100%/1.5 system-ui, sans-serif; } /* body: system font, comfortable leading */

.display {
  font-size: clamp(2rem, 5vw, 4rem);
  line-height: 1.05;        /* tight leading for large text */
  letter-spacing: -0.02em;  /* negative tracking as it grows */
  font-optical-sizing: auto;
}
```

## 16. Fundamentos de diseño — los ocho principios

El movimiento y el oficio de arriba sirven a los ocho principios de diseño de Apple (*Principles of Great Design*, WWDC 2026). Usa estos como los nombres con los que razonas:

1. **Propósito.** Crea con intención; decide qué *no* construir. Cada función pide tiempo, atención y confianza de la persona usuaria — gasta ese presupuesto solo donde compense.
2. **Agencia.** Mantén a las personas en control: ofrece opciones, no fuerces un único camino. Respáldalo con perdón — deshacer fácil para errores, un diálogo de confirmación solo para acciones de verdad destructivas e irreversibles (úsalo poco; abusar entrena a clicar sin leer).
3. **Responsabilidad.** Actúa en interés de la persona usuaria. Privacidad: pregunta en el momento justo, solo lo necesario, con transparencia. Seguridad: anticipa mal uso y daño — sobre todo con IA (una app de recetas para alérgicos no debe sugerir un ingrediente dañino). Añade vistas previas, confirmaciones, avisos; corta una función cuyo riesgo supere su valor.
4. **Familiaridad.** Construye sobre lo que la gente ya conoce. Usa metáforas ni demasiado literales ni demasiado abstractas (una papelera significa borrar), y respeta su física. Sé consistente: las cosas que se ven igual deben comportarse igual y vivir en el mismo sitio (cerrar siempre arriba a la izquierda en macOS) para que la gente pueda predecir qué pasa. Solo rompe un patrón familiar si puedes probar que es mejor — y pruébalo, no lo supongas.
5. **Flexibilidad.** Diseña para distintos contextos, dispositivos y todo el rango de capacidades. Adáptate a la plataforma (iPhone = toques rápidos; escritorio = flujos profundos con control preciso de puntero) y a la situación. Diseña de forma inclusiva (edad, idioma, experiencia, accesibilidad). Cuando ninguna maquetación sirva para todos, deja personalizar — reordenar controles, ocultar lo que no se usa.
6. **Simplicidad — no minimalismo.** Quita lo innecesario para que brille el propósito central; enterrar todo en un solo sitio se ve minimalista pero no es simple. Sé conciso (lenguaje llano, sin jerga, menos pasos) y claro (usa jerarquía — orden, espaciado, contraste — para que lo más importante sea lo más obvio). Cada elemento gana su lugar; a veces *añadir* contexto simplifica (un scrubber de vídeo que muestra el tiempo restante). Muestra primero el camino común, las opciones avanzadas un nivel más adentro.
7. **Oficio (Craft).** Atención al detalle sin concesiones genera confianza. Tipografía bonita, colores que se adaptan a claro/oscuro, iconografía clara y animaciones responsive que dan feedback inmediato y natural. Nada es aleatorio — cada valor de espaciado, tiempo y alineación es una decisión deliberada que puedes defender. El scroll con tirones, los iconos desalineados y las maquetaciones que se rompen al rotar se leen como descuido. El oficio necesita iteración y longevidad — sigue evolucionando el diseño cuando cambien funciones y hardware.
8. **Deleite.** El resultado de hacer bien los otros siete, no confeti pegado encima. Decide la emoción que quieres que la gente sienta (calma, confianza, entusiasmo) y refuérzala en cada decisión.

Reglas tácticas que sirven a estos principios:

- **El feedback viene en cuatro tipos:** estado, finalización, aviso, error. Confirma acciones con sentido, muestra el estado en curso, avisa antes de los problemas, valida en línea (no al enviar).
- **Orientación (Wayfinding).** Cada pantalla debe responder: ¿dónde estoy? ¿a dónde puedo ir? ¿qué hay ahí? ¿cómo salgo? Nunca atrapes a la persona.
- **Agrupación y correspondencia.** La proximidad implica relación; pon un control cerca de lo que afecta y ordena los controles reflejando lo que cambian. Si necesitas una etiqueta para explicar un control, la correspondencia es débil.
- **Etiquetas directas y específicas mejor que genéricas seguras.** Nombra los elementos de navegación por su contenido ("Progreso", "Biblioteca"), no con paraguas vagos ("Inicio"). Lo específico crea previsibilidad.

## 17. Proceso

- **Prototipa de forma interactiva — una demo interactiva vale "un millón de diseños estáticos".** Descubres la interfaz construyéndola y jugando con ella; un prototipo funcional también pone un listón concreto que evita una implementación final mediocre.
- **Diseña interacción y visuales juntos.** "No deberías poder decir dónde termina una y empieza la otra". El movimiento no es una capa que se añade después de los píxeles.
- **Prueba con personas reales en contexto real**, y revisa el movimiento con ojos frescos — reprodúcelo en cámara lenta / fotograma a fotograma para cazar lo invisible a velocidad normal.

## Referencia rápida

| Necesidad | Técnica | Valor concreto |
| --- | --- | --- |
| Spring por defecto en UI | Amortiguación crítica, sin sobreimpulso | `damping 1.0`, `response 0.3–0.4` |
| Spring con impulso / flick | Subamortiguado, poco rebote | `damping ~0.8`, `response 0.3–0.4` |
| Gesto → velocidad del spring | Entrega la velocidad de soltado | `gestureVelocity / (target − current)` si es normalizada |
| Punto de aterrizaje del flick | Proyecta el impulso | `current + (v/1000)·d/(1−d)`, `d ≈ 0.998` |
| Interrumpir limpio | Arranca desde el valor actual (presentation) | lee la transformada en pantalla |
| Evitar "muro" al revertir | Conserva la velocidad al cambiar de objetivo | spring que mezcla velocidad |
| Transición reversible | Refleja la curva de easing | cubic-bézier inverso |
| Decidir revertir o confirmar | Usa el **signo** de la velocidad, no la posición | al soltar |
| Arrastre 1:1 | Pointer Events + captura | respeta el punto de agarre |
| Feedback | Al pulsar, continuo | nunca solo al final |
| Borde | Rubber-band, no parada dura | resistencia progresiva |
| Chrome translúcido | capa con `backdrop-filter` | el contenido pasa por debajo |
| Tracking en texto | Por tamaño, nunca fijo | aprieta texto grande (`-0.02em`), cuerpo cerca de `0` |
| Movimiento reducido | Fundido cruzado, no slide/spring | `@media (prefers-reduced-motion)` |
