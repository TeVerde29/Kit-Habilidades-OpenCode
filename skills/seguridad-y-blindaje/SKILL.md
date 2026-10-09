---
name: seguridad-y-blindaje
description: Blindaje de codigo contra vulnerabilidades. Hardens code against vulnerabilities. Use when auditing an input handler for vulnerabilities, when handling user input, authentication, data storage, or external integrations, or when checking a login flow is safe against the OWASP Top Ten. Use when building any feature that accepts untrusted data, manages user sessions, or interacts with third-party services. Use when auditing dependencies for known vulnerabilities, triaging package-manager audit findings, or assessing supply-chain risk in a new package. Use when personal data or privacy compliance (GDPR, CCPA) is involved.
---

# Seguridad y Blindaje

## Resumen

Prácticas de desarrollo con seguridad primero para aplicaciones web. Trata cada entrada externa como hostil, cada secreto como sagrado y cada revisión de autorización como obligatoria. La seguridad no es una fase: es una restricción en cada línea de código que toca datos de usuarios, autenticación o sistemas externos.

## Cuándo usarla

- Construir cualquier cosa que acepte entradas de usuarios
- Implementar autenticación o autorización
- Guardar o transmitir datos sensibles
- Integrar con APIs o servicios externos
- Añadir subidas de archivos, webhooks o callbacks
- Manejar pagos o datos PII

## Proceso: primero el modelo de amenazas

Controles añadidos sin un modelo de amenazas son conjeturas. Antes de blindar, dedica cinco minutos a pensar como un atacante:

1. **Mapea las fronteras de confianza.** ¿Dónde cruzan datos no confiables hacia tu sistema? Peticiones HTTP, campos de formularios, subidas de archivos, webhooks, APIs de terceros, colas de mensajes y **salida de LLM**; más los valores locales que parecen internos porque te los dio el sistema operativo: la línea de comandos de otro proceso o su entorno, nombres de archivo en un volumen compartido, una ruta en un payload de trabajo. La confianza sigue a quién *escribió* un valor, no al canal que lo trajo. Cada frontera es superficie de ataque.
2. **Nombra los activos.** ¿Qué vale la pena robar o romper? Credenciales, PII, datos de pago, acciones de administración, movimiento de dinero.
3. **Pasa STRIDE por cada frontera**, como lente rápida, no como ceremonia:

| Amenaza | Pregunta | Mitigación típica |
|---|---|---|
| **S**uplantación (Spoofing) | ¿Alguien puede suplantar a un usuario/servicio? | Autenticación, verificación de firmas |
| **M**anipulación (Tampering) | ¿Pueden alterarse datos en tránsito o en reposo? | Revisiones de integridad, consultas parametrizadas, HTTPS |
| **R**epudio (Repudiation) | ¿Puede negarse una acción después? | Registro de auditoría de eventos de seguridad |
| **D**ivulgación de información (Information disclosure) | ¿Pueden filtrarse datos? | Cifrado, listas de campos permitidos, errores genéricos |
| **D**enegación de servicio (Denial of service) | ¿Puede saturarse? | Rate limiting, límites de tamaño de entrada, timeouts |
| **E**levación de privilegios (Elevation of privilege) | ¿Puede un usuario ganar derechos indebidos? | Revisiones de autorización, mínimo privilegio |

4. **Escribe casos de abuso junto a los casos de uso.** Para cada función, pregunta "¿cómo abusaría yo de esto?" y convierte eso en tu primera prueba.

Si no puedes nombrar las fronteras de confianza de una función, no estás listo para asegurarla. Esto es OWASP **A04: Insecure Design**: la mayoría de las brechas empiezan en el diseño, no en el código.

## El sistema de fronteras de tres niveles

### Siempre (sin excepciones)

- **Valida toda entrada externa** en la frontera del sistema (rutas de API, manejadores de formularios)
- **Parametriza todas las consultas a la base de datos**: nunca concatenes entradas de usuarios en SQL
- **Codifica la salida** para prevenir XSS (usa el auto-escape del framework, no lo evites)
- **Usa HTTPS** para toda comunicación externa
- **Hashea contraseñas** con bcrypt/scrypt/argon2 (nunca guardes texto plano)
- **Configura cabeceras de seguridad** (CSP, HSTS, X-Frame-Options, X-Content-Type-Options)
- **Usa cookies httpOnly, secure, sameSite** para sesiones
- **Ejecuta la auditoría nativa del gestor de paquetes detectado** contra el lockfile confirmado antes de cada release

### Pregunta primero (requiere aprobación humana)

- Añadir nuevos flujos de autenticación o cambiar la lógica de auth
- Guardar nuevas categorías de datos sensibles (PII, datos de pago)
- Añadir nuevas integraciones con servicios externos
- Cambiar la configuración de CORS
- Añadir manejadores de subida de archivos
- Modificar rate limiting o throttling
- Otorgar permisos o roles elevados

### Nunca

- **Nunca subas secretos** al control de versiones (API keys, contraseñas, tokens)
- **Nunca registres datos sensibles en logs** (contraseñas, tokens, números completos de tarjeta)
- **Nunca confíes en la validación del cliente** como frontera de seguridad
- **Nunca desactives cabeceras de seguridad** por comodidad
- **Nunca uses `eval()` ni `innerHTML`** con datos aportados por usuarios
- **Nunca guardes sesiones en almacenamiento accesible desde el cliente** (localStorage para tokens de auth)
- **Nunca expongas stack traces** ni detalles internos de errores a los usuarios

## Controles de blindaje

Las reglas siguientes son el flujo de trabajo; la implementación concreta de cada una vive en [references/hardening-patterns.md](references/hardening-patterns.md). Abre la sección que necesites cuando llegues a ese código, no antes.

### Inyección, XSS y control de acceso

- Parametriza cada consulta. Nunca construyas SQL, NoSQL ni comandos de shell desde cadenas de entrada.
- Codifica la salida con el auto-escape del framework. Si el HTML crudo es inevitable, primero desinféctalo con un sanitizador de allowlist.
- Revisa la **autorización** en cada petición, no solo la autenticación: el usuario autenticado debe ser dueño del recurso específico o tener permiso sobre él (A01, IDOR).

Patrones: [Injection](references/hardening-patterns.md#injection), [XSS](references/hardening-patterns.md#cross-site-scripting-xss), [Access control](references/hardening-patterns.md#broken-access-control).

### Autenticación y sesiones

- Hashea contraseñas con bcrypt (≥12 rounds), scrypt o argon2. El secreto de sesión viene del entorno, nunca del código.
- Las cookies de sesión son `httpOnly`, `secure` y `sameSite: 'lax'` o `'strict'` (la defensa contra CSRF; `'none'` envía la cookie en peticiones cross-site), con un `maxAge` acotado.

Patrón: [Authentication](references/hardening-patterns.md#broken-authentication).

### Cabeceras, CORS y respuestas

- Cabeceras de seguridad en cada respuesta (helmet o el equivalente del framework); la CSP parte de `default-src 'self'` y se endurece, no se afloja.
- CORS restringido a una lista explícita de orígenes desde la configuración. Nunca `*` con credenciales.
- Quita campos sensibles (`passwordHash`, tokens de reseteo) antes de cualquier respuesta. Los cuerpos de error son genéricos; los detalles internos van solo a los logs del servidor.

Patrones: [Misconfiguration](references/hardening-patterns.md#security-misconfiguration), [Sensitive data exposure](references/hardening-patterns.md#sensitive-data-exposure).

### Validación de entradas y subidas

- Valida en la frontera con un esquema: forma permitida (allowlist), longitudes, enums, formatos. Rechaza con 422 y detalles estructurados; el código interno usa solo el valor parseado y tipado.
- Subidas: permite solo tipos MIME de una allowlist, limita el tamaño, verifica el contenido (magic bytes) cuando importe. La extensión no prueba nada.

Patrones: [Schema validation](references/hardening-patterns.md#schema-validation-at-boundaries), [File upload](references/hardening-patterns.md#file-upload-safety).

### Peticiones al servidor (SSRF)

Cualquier URL que el usuario influya —webhooks, importación desde URL, proxies de imágenes, vistas previas de enlaces— puede apuntar a servicios internos. Permite solo esquema y host de una allowlist, resuelve **todos** los registros DNS y rechaza cualquier dirección privada o reservada (loopback, link-local `169.254.169.254`, privadas, unique-local, tanto en IPv4 como IPv6), y prohíbe redirecciones. Esa revisión aún tiene un hueco TOCTOU por re-enlace de DNS: para superficies de alto riesgo, fija la IP resuelta o pon un agente de filtrado delante.

Patrón: [SSRF](references/hardening-patterns.md#server-side-request-forgery-ssrf).

### Operaciones destructivas sobre rutas derivadas

Un borrado, movimiento o sobreescritura solo es tan seguro como el valor que nombra su objetivo, y la confianza sigue a quién *escribió* ese valor, no al canal que lo trajo: la línea de comandos de otro proceso está tan controlada por el atacante como un campo de formulario. Una revisión de forma solo prueba buena formación, no autorización. Antes de la llamada, exige las tres: el objetivo resuelto (symlinks resueltos) está bajo una **raíz de allowlist**; está al menos un nivel **por debajo** de esa raíz; y lleva **evidencia de propiedad leída antes de la operación**. Ante una negativa, registra el objetivo rechazado y detente; nunca caigas a una ruta por defecto más amplia.

Por qué la revisión es más débil de lo que parece (auto-certificación de marcadores, carreras check/use): [Destructive paths](references/hardening-patterns.md#destructive-operations-on-derived-paths). Código trabajado: `../../references/security-checklist.md`.

### Rate limiting

Limita la API en general y los endpoints de auth de forma estricta (unos 10 intentos por cada 15 minutos). Cuando más de un proceso sirve tráfico, los contadores en memoria se vuelven silenciosamente `máximo × instancias`, o nunca se disparan en serverless: respalda el limitador con un almacén compartido.

Patrón: [Rate limiting](references/hardening-patterns.md#rate-limiting).

### Secretos

Los secretos vienen del entorno. `.env.example` se sube con marcadores; los archivos `.env*` reales y el material de claves van en gitignore; revisa con grep el diff preparado antes de confirmar. **Un secreto que llega a un remoto queda comprometido en el momento en que aterriza: rótalo primero, luego purga el historial.**

Patrón: [Secrets management](references/hardening-patterns.md#secrets-management).

### Dependencias y cadena de suministro

1. **Encuentra la frontera de instalación y el gestor.** Usa la raíz del workspace dueña del lockfile, o un proyecto anidado independiente solo cuando esté fuera de ese workspace. Corrobora `packageManager` (cuando exista), el lockfile y el CI; detente ante desacuerdos o lockfiles en competencia. Fija la versión del gestor.
2. **Bloquea los scripts de dependencias antes de la primera ejecución.** Arranca con scripts desactivados o una política documentada de fallo cerrado (fail-closed), inspecciona la fuente de los scripts pendientes, aprueba solo el mínimo, confirma la política, luego verifica con una instalación limpia congelada/inmutable. Nunca apruebes todo en bloque.
3. **Ejecuta la auditoría nativa contra el lockfile confirmado antes de cada release.** Clasifica critical/high por **alcance** (rutas de runtime, build, pruebas, despliegue) y disponibilidad de corrección. Nunca apliques remediación forzada (`npm audit fix --force` o equivalente) de forma automática, ya que puede cruzar los rangos de dependencias declarados; previsualiza, lee changelogs, prueba cada actualización. Documenta cada aplazamiento con motivo y fecha de revisión.
4. **Las auditorías solo cubren avisos conocidos.** No detectan un paquete recién malicioso o con typosquatting (`cross-env` vs `crossenv`). Revisa juntas las dependencias nuevas, los diffs del lockfile y los cambios de política de scripts: titularidad, mantenimiento, antigüedad del release, procedencia, grafo transitivo. Verifica firmas del registro donde se soporte (`npm audit signatures`, `pnpm audit signatures`) y trata su ausencia como señal para investigar, no como prueba automática de compromiso (A06, LLM03).

Árbol de decisión de clasificación: [Dependency audit triage](references/hardening-patterns.md#dependency-audit-triage). Matriz de gestores y puerta de scripts de instalación: `../../references/security-checklist.md`.

### Datos personales y privacidad

El blindaje pregunta "¿puede un atacante leerlo?" La privacidad pregunta "¿deberíamos *nosotros* guardarlo, y por cuánto tiempo?" El dato más barato de proteger, de filtrar y de regular es el que nunca recogiste; trata los datos personales como un pasivo a minimizar.

- **Clasifica los campos cuando los añadas** (no personal, PII, sensible) y trata cada clase como corresponda. No puedes proteger, ni atender una solicitud de borrado, de datos que no puedes encontrar.
- **Recoge solo con un propósito declarado.** "Quizá sirva después" es alcance latente de brecha, no un propósito. Mantén la PII fuera de la telemetría (la skill `observability-and-instrumentation` dice lo mismo desde el lado de operaciones).
- **Define la retención por adelantado y luego borra de verdad.** Cada almacén de datos personales necesita un TTL y una ruta de borrado funcional, incluyendo backups, cachés, índices de búsqueda y copias de analítica.
- **Soporta los derechos de los titulares que exija tu jurisdicción** (GDPR, CCPA y similares): exportar, corregir, borrar. Diseña el esquema para que los datos de un usuario sean localizables y borrables, no esparcidos de forma irreversible por los sistemas.
- **El consentimiento precede a la recogida y al intercambio con terceros, y es auditable.** Enviar PII a un proveedor de analítica, publicidad o LLM es compartir; el proveedor necesita un acuerdo de tratamiento de datos. Haz de la región una política configurable, no una suposición fija.

Tabla de clasificación: [Data classification](references/hardening-patterns.md#data-classification). Un incidente de privacidad arranca el reloj de notificación de brechas; haz el postmortem con la skill `debugging-and-error-recovery`.

### Funciones de IA / LLM

Llamar a un LLM —chatbots, resúmenes, agentes, RAG— añade una nueva superficie de ataque; mapéala al [OWASP Top 10 for LLM Applications (2025)](https://genai.owasp.org/llm-top-10/):

- **La salida del modelo es entrada no confiable** (LLM05). Nunca a `eval`, SQL, shell, `innerHTML` ni rutas de archivo; parsea a la defensiva, valida contra un esquema, luego codifica.
- **Los prompts pueden secuestrarse** (LLM01). El texto no confiable en el contexto —un mensaje de usuario, una página obtenida, un PDF— puede llevar instrucciones. El system prompt no es una frontera de seguridad; aplica los permisos en código.
- **Mantén secretos, datos de otros inquilinos y el system prompt completo fuera de la ventana de contexto** (LLM02, LLM07); limita los permisos de herramientas, valida cada argumento de herramienta y confirma acciones destructivas (LLM06); limita tokens, tasa de peticiones y profundidad de recursión (LLM10); particiona los embeddings RAG por inquilino y valida documentos antes de indexar (LLM08).

Patrón: [LLM output handling](references/hardening-patterns.md#llm-output-handling).

## Lista de revisión

Antes de aprobar, recorre `../../references/security-checklist.md`: cubre autenticación, autorización, entradas, protección de datos y privacidad, cabeceras y CORS, dependencias y cadena de suministro, IA/LLM y manejo de errores, más las tablas de referencia rápida de OWASP.

## Racionalizaciones comunes

| Racionalización | Realidad |
|---|---|
| "Es una herramienta interna, la seguridad no importa" | Las herramientas internas se comprometen. Los atacantes buscan el eslabón más débil. |
| "Ya añadiremos seguridad después" | Reforzar después cuesta 10 veces más que construirla desde el inicio. Añádela ahora. |
| "Nadie intentaría explotar esto" | Los escáneres automatizados lo encontrarán. La seguridad por oscuridad no es seguridad. |
| "El framework se encarga de la seguridad" | Los frameworks dan herramientas, no garantías. Igual debes usarlas bien. |
| "Es solo un prototipo" | Los prototipos llegan a producción. Hábitos de seguridad desde el día uno. |
| "El modelado de amenazas es exagerado aquí" | Cinco minutos de "¿cómo atacaría yo esto?" previenen los fallos de diseño que ningún control parchea después. |
| "Es solo salida de LLM, es solo texto" | Ese "texto" puede ser una sentencia SQL, una etiqueta script o un comando de shell. Trátalo como cualquier entrada no confiable. |
| "La auditoría pasó, la dependencia es segura" | Las auditorías cubren avisos conocidos. No detectan un paquete recién malicioso ni hacen seguros los scripts de instalación sin revisar. |
| "Recógelo ahora, quizá lo necesitemos después" | Los datos que no guardas no pueden filtrarse, ni ser citados, ni borrarse mal. "Quizá lo necesitemos" es alcance de brecha, no un propósito. |
| "Las solicitudes de borrado las haremos a mano" | El borrado manual olvida backups, cachés y copias de analítica. Si el esquema no encuentra los datos de un usuario, no puedes atender la solicitud: diséñalo para eso. |
| "El cumplimiento es problema de legal, no nuestro" | Exportar, borrar, retener y consentir son esquema y código. Legal no puede añadirlos después de que esparciste PII por diez sistemas. |

## Señales de alerta

- Entradas de usuarios pasadas directo a consultas de base de datos, comandos de shell o renderizado HTML
- Un borrado, movimiento o sobreescritura cuyo objetivo viene de un payload, un valor de configuración o la línea de comandos de otro proceso, protegido solo por una revisión de forma de la ruta
- Secretos en el código fuente o en el historial de commits
- Endpoints de API sin revisiones de autenticación o autorización
- Configuración CORS ausente u orígenes comodín (`*`)
- Sin rate limiting en endpoints de autenticación, o un limitador en memoria frente a más de una instancia
- Stack traces o errores internos expuestos a los usuarios
- Dependencias con vulnerabilidades críticas conocidas, lockfiles en competencia en una frontera de instalación, instalaciones no reproducibles o scripts aprobados en bloque
- Peticiones del servidor a URLs aportadas por usuarios sin allowlist (SSRF)
- Salida de LLM/modelo pasada a una consulta, al DOM, a un shell o a `eval`
- Secretos, PII o el system prompt completo dentro de una ventana de contexto de LLM
- Datos personales recogidos sin propósito declarado, límite de retención ni ruta de borrado
- PII enviada a proveedores de analítica/publicidad/LLM sin consentimiento ni acuerdo de tratamiento de datos
- Un "borra mi cuenta" que solo cambia un flag mientras los datos personales siguen en almacenes y backups

## Verificación

Tras implementar código relevante para seguridad:

- [ ] La auditoría nativa no tiene hallazgos critical/high alcanzables sin mitigar; el CI conserva el lockfile autoritativo y bloquea scripts de dependencias sin revisar
- [ ] Sin secretos en el código fuente ni en el historial de git
- [ ] Toda entrada de usuario validada en las fronteras del sistema
- [ ] Las operaciones destructivas de archivos resuelven symlinks, luego verifican raíz de allowlist, profundidad mínima y propiedad antes de ejecutar
- [ ] Autenticación y autorización revisadas en cada endpoint protegido
- [ ] Cabeceras de seguridad presentes en la respuesta (revisa con DevTools del navegador)
- [ ] Las respuestas de error no exponen detalles internos
- [ ] Rate limiting activo en endpoints de auth, respaldado por almacén compartido cuando más de una instancia sirve tráfico
- [ ] Peticiones URL del servidor validadas contra una allowlist (sin SSRF)
- [ ] Salida de LLM/modelo validada y codificada antes de usarse (si hay funciones de IA)
- [ ] Los datos personales están clasificados, minimizados a un propósito declarado y con límite de retención
- [ ] Las solicitudes de borrado y exportación funcionan de punta a punta (incluyendo backups, cachés y copias de analítica)
