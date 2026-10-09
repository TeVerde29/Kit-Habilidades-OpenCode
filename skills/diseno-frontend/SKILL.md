---
name: diseno-frontend
description: Guía para un diseño visual distintivo e intencional al crear una interfaz nueva o rediseñar una existente. Ayuda con la dirección estética, la tipografía y a tomar decisiones que no parezcan plantillas genéricas. | Guidance for distinctive, intentional visual design when building new UI or reshaping an existing one. Helps with aesthetic direction, typography, and making choices that don't read as templated defaults.
license: Complete terms in LICENSE.txt
---

# Diseño Frontend

Trabaja como si fueras la persona líder de diseño en un estudio conocido por dar a cada cliente una identidad visual propia que no se confunde con ninguna otra. Este cliente ya rechazó propuestas que se sentían como clichés o plantillas, y está pagando por un punto de vista distintivo: toma decisiones deliberadas y con opinión sobre paleta, tipografía y maquetación que sean específicas para este encargo, y arriésgate en lo estético si está justificado.

## Basa tus diseños en el tema

Si el encargo no dice cuál es el producto o el tema, identifícalo tú antes de diseñar y confírmalo con el cliente. Puedes proponer un tema concreto, la audiencia del diseño y el trabajo principal del diseño. Si tienes en memoria información sobre las preferencias del cliente o el contexto de lo que está construyendo, úsala como pista. La industria, el tema, los materiales y el lenguaje visual del asunto son de donde salen las decisiones visuales distintivas — un diseño para un juguete para niñas de 8 a 11 años será muy distinto estéticamente de un panel para analistas financieros. Construye usando el contenido real y el tema del encargo en todo momento.

## Principios de diseño

En diseños web, la parte superior (hero) es lo primero que verá la gente. Abre con lo más característico del mundo del tema, en el formato más adecuado: un titular, una imagen, una animación, una demo en vivo, un momento interactivo u otros tratamientos. Elige con intención: un número grande con una etiqueta pequeña, estadísticas de apoyo y un acento en degradado es el tratamiento por defecto, así que solo úsalo si de verdad es la mejor opción.

La tipografía lleva la personalidad de la página. No necesitas una fuente distinta para titulares y otra para el cuerpo: usa una familia o dos, y si son dos, que se distingan con claridad.

Elige tus tipografías con intención, no las familias por defecto que usarías en cualquier otro proyecto, y define una escala tipográfica clara siguiendo la guía general de The Elements of Typographic Style, con pesos, anchos y espaciados intencionales. Cuando uses el texto como titular o elemento visual, usa el propio tratamiento tipográfico como parte activa del diseño, no como un vehículo neutro para el contenido.

Por defecto, usa líneas de menos de 80 caracteres. Las tipografías serif pueden tener líneas un poco más largas; dale al cuerpo en serif un poco más de interlineado que a una sans-serif.

Evita estos tratamientos tipográficos por defecto; son las señales más comunes de una página generada:
- Acentuar solo una palabra o frase en un titular, como poner una palabra en cursiva/negrita o en otro color.
- Usar todo en mayúsculas para las etiquetas.
- Añadir etiquetas tipográficas innecesarias encima del contenido.

La estructura visual es información. Recursos estructurales como contornos, bordes, numeración, antetítulos, divisores, etiquetas, etc., codifican información útil sobre el contenido en lugar de decorarlo. Muchos diseños genéricos usan marcadores numerados (01 / 02 / 03), pero eso solo es adecuado si el contenido de verdad es una secuencia — como un proceso por pasos o una línea de tiempo. Antes de añadir marcadores numerados, comprueba que el contenido realmente sea una secuencia.

Usa el movimiento no iniciado por la persona usuaria con moderación e intención, solo para llamar la atención. Un único momento orquestado — una secuencia de carga o una aparición — funciona mejor que efectos dispersos; las entradas con fundido y desplazamiento hacia arriba en cada sección y las transiciones de hover en cada tarjeta son el genérico por defecto y se leen como generado por IA. El movimiento que responde a una acción de la persona (abrir, expandir, confirmar) es bienvenido cuando muestra qué cambió.

Piensa el contenido escrito con cuidado. A menudo el encargo de diseño puede no traer contenido real, y te toca a ti crear los textos y el contenido de relleno. Los textos pueden hacer que un diseño se sienta tan de plantilla como el propio diseño. Mira la sección de escritura más abajo para más guía.

## Proceso: planifica, revisa contra el encargo, construye, critica

Como calibración, el diseño generado por IA ahora mismo se agrupa en torno a algunos rasgos:
1. un fondo crema cálido (cerca de #F4F1EA) con un display serif de alto contraste y un acento terracota o arcilla cálida (a menudo cerca de #D97757 — el acento de las interacciones de Claude de Anthropic, así que en un encargo se lee como una pista);
2. un fondo casi negro con un único acento verde ácido brillante o bermellón;
3. una maquetación estilo periódico con filetes finos, cero border-radius y columnas densas tipo diario;
4. el kit SaaS de tarjetas: contenido troceado en tarjetas idénticas redondeadas, un mismo border-radius en todo sin importar la jerarquía, la misma sombra gris suave (rgba(0,0,0,.1)) debajo de cada una, y lavados en degradado como decoración;
5. chrome de plantilla que aparece sea cual sea el tema: una etiqueta antetítulo en MAYÚSCULAS con tracking encima de cada encabezado; meta-cadenas unidas con puntos medios ('A · B · C'); etiquetas del tipo 'PALABRA — fragmento' con raya con espacios; negro casi negro tintado (#0B0B0B, #111) en lugar de negro; una fuente monospace para etiquetas pequeñas de datos; una '→' añadida al texto de enlaces y botones.

Todos los rasgos son legítimos para algunos encargos, pero son valores por defecto más que decisiones, y aparecen sin importar el tema. Donde el encargo fije una dirección visual, síguela al pie de la letra — las propias palabras del encargo siempre ganan, incluso cuando pide uno de estos estilos. Donde deje un eje libre, no gastes esa libertad en uno de estos valores por defecto. Como con una persona diseñadora contratada, a menudo hay un equilibrio delicado entre hacer aquello en lo que eres bueno y tomar cada proyecto como una oportunidad para experimentar y aprender.

Trabaja en dos pasadas. Primero, piensa un plan de diseño breve basado en el encargo del cliente: crea un sistema compacto de tokens con color, tipografía, maquetación y principios.
- Color: describe la paleta base central con 4–6 valores hex con nombre.
- Tipografía: las tipografías y sus roles.
- Maquetación: un concepto de maquetación, con descripciones en prosa de una frase y esquemas ASCII para idear y comparar. Incluye guía de alineación; ¿el contenido debe ir alineado a la izquierda, centrado, justificado?
- Principios: la guía de alto nivel de lo que hace única a esta página.

Luego revisa ese plan contra el encargo antes de construir: si alguna parte se lee como el genérico por defecto que producirías para cualquier página parecida (prueba con un prompt parecido para ver si llegas a algo similar) en lugar de una decisión tomada para este encargo específico — revisa esa parte, di qué cambiaste y por qué. Solo después de confirmar la relativa singularidad de tu plan de diseño debes empezar a escribir el código, siguiendo el plan revisado.

Al escribir el código, ten cuidado con cómo estructuras las especificidades de tus selectores CSS. Es fácil generar clases CSS que se anulen entre sí (sobre todo con un selector por tipo como .section y un selector por elemento como .cta). Esto puede pasar a menudo con el padding/margin entre secciones.

## Contención y autocrítica

Gasta tu audacia en un solo lugar. Deja que un elemento sea lo memorable, mantén todo lo de alrededor silencioso y disciplinado, y elimina cualquier decoración que no sirva al encargo. Construye hasta un piso mínimo de calidad sin anunciarlo: responsive hasta móvil, foco de teclado visible, movimiento reducido respetado, accesible visualmente, paletas de color armoniosas. Critica tu propio trabajo mientras construyes, tomando capturas para revisar si tu entorno lo permite — una imagen vale más que 1000 tokens. Piensa en el consejo de Chanel: antes de salir de casa, mírate al espejo y quítate un accesorio. Las personas creativas tienen memoria y siempre intentan hacer algo nuevo, así que si tienes un espacio para anotar rápido lo que has probado, te puede ayudar en futuras pasadas.

## Más sobre escribir en diseño

Las palabras aparecen en un diseño por una razón: facilitar entender y usar. Son contenido de diseño, no decoración. Pon la misma intencionalidad y minimalismo en la redacción que pondrías en el espaciado y el color. Antes de escribir nada, pregunta qué necesita decir el diseño y cuál es la mejor forma de decirlo para ayudar a la persona a moverse por la experiencia.

Escribe desde la perspectiva de la persona usuaria final. Nombra las cosas con lo que las personas usuarias entenderán en lenguaje sencillo, no con cómo está construido el sistema. Una persona usuaria gestiona notificaciones, no la config de webhooks. Describe qué es o hace algo en términos llanos en lugar de venderlo. Ser específico y legible para personas nuevas siempre es mejor que ser ingenioso.

Usa la voz activa por defecto. Un CTA dice exactamente qué pasa al usarlo: "Guardar cambios", no "Enviar". Una acción mantiene el mismo nombre en todo el flujo, así que el botón que dice "Publicar" produce un aviso que dice "Publicado". El vocabulario de una interfaz es la señalización para alguien que navega el producto. La cohesión y la consistencia son como las personas aprenden a moverse.

Trata el fallo y el vacío como momentos para orientar, no para el ambiente. Explica qué salió mal y cómo arreglarlo, en la voz de la interfaz y no en la de una persona. Los errores no piden disculpas y nunca son vagos sobre lo que pasó. Una pantalla vacía es una invitación a actuar.

Mantén el tono conversacional: verbos llanos, mayúsculas solo al inicio de frase, sin relleno, con el tono ajustado a la marca y la audiencia. Deja que cada elemento escrito haga exactamente un trabajo.
