---
name: diseno-modulos
description: Vocabulario compartido para diseñar módulos profundos. Shared vocabulary for designing deep modules. Use when the user wants to design or improve a module's interface, find deepening opportunities, decide where a seam goes, make code more testable or AI-navigable, or when another skill needs the deep-module vocabulary.
---

# Diseño de código (Codebase Design)

Diseña **módulos profundos (deep modules)**: mucho comportamiento detrás de una interfaz (interface) pequeña, colocados en una unión (seam) limpia y que se pueden probar a través de esa interfaz. Usa este lenguaje y estos principios donde sea que diseñes o reestructures código. El objetivo es ganancia (leverage) para quien llama al módulo, localidad (locality) para quien lo mantiene y facilidad de prueba para todos.

## Glosario

Usa estos términos de forma exacta: no los cambies por "component", "service", "API" ni "boundary". El lenguaje consistente es todo el punto.

**Module** (módulo): cualquier cosa con una interfaz y una implementación (implementation). A propósito no depende de la escala: puede ser una función, una clase, un paquete o una parte que cruza varias capas. _Evita_: unit, component, service.

**Interface** (interfaz): todo lo que quien llama al módulo necesita saber para usarlo bien: la firma de tipos, pero también las reglas que siempre se cumplen (invariants), las restricciones de orden, los modos de error, la configuración necesaria y cómo rinde. _Evita_: API, signature (muy limitados, solo cubren la superficie a nivel de tipos).

**Implementation** (implementación): lo que está dentro de un módulo, su cuerpo de código. Es distinto de **Adapter** (adaptador): algo puede ser un adaptador pequeño con una implementación grande (un repositorio de Postgres) o un adaptador grande con una implementación pequeña (un falso en memoria). Usa "adapter" cuando el tema sea la unión; "implementation" en los demás casos.

**Depth** (profundidad): ganancia en la interfaz. Cuánto comportamiento puede usar quien llama (o un test) por cada unidad de interfaz que tiene que aprender. Un módulo es **deep** (profundo) cuando mucho comportamiento vive detrás de una interfaz pequeña, y **shallow** (superficial) cuando la interfaz es casi tan compleja como la implementación.

**Seam** (unión, _Michael Feathers_): un lugar donde puedes cambiar el comportamiento sin editar en ese lugar; el _sitio_ donde vive la interfaz de un módulo. Dónde poner la unión es una decisión de diseño propia, distinta de qué pones detrás de ella. _Evita_: boundary (se confunde con el bounded context de DDD).

**Adapter** (adaptador): algo concreto que cumple una interfaz en una unión. Describe el _rol_ (qué lugar ocupa), no la sustancia (qué tiene dentro).

**Leverage** (ganancia): lo que la profundidad le da a quien llama al módulo. Más capacidad por cada unidad de interfaz que aprende. Una implementación rinde en N lugares que la llaman y M tests.

**Locality** (localidad): lo que la profundidad le da a quien mantiene el módulo. El cambio, los errores, el conocimiento y la verificación se concentran en un solo lugar en vez de regarse entre quienes lo llaman. Se arregla una vez, queda arreglado en todas partes.

## Profundo vs superficial

**Módulo profundo (deep module)** = interfaz pequeña + mucha implementación:

```
┌─────────────────────┐
│   Small Interface   │  ← Few methods, simple params
├─────────────────────┤
│                     │
│  Deep Implementation│  ← Complex logic hidden
│                     │
└─────────────────────┘
```

**Módulo superficial (shallow module)** = interfaz grande + poca implementación (evítalo):

```
┌─────────────────────────────────┐
│       Large Interface           │  ← Many methods, complex params
├─────────────────────────────────┤
│  Thin Implementation            │  ← Just passes through
└─────────────────────────────────┘
```

Al diseñar una interfaz, pregúntate:

- ¿Puedo reducir la cantidad de métodos?
- ¿Puedo simplificar los parámetros?
- ¿Puedo esconder más complejidad adentro?

## Principios

- **La profundidad (depth) es una propiedad de la interfaz, no de la implementación.** Un módulo profundo puede estar compuesto por dentro de partes pequeñas, simulables (mockable) e intercambiables; simplemente no son parte de la interfaz. Un módulo puede tener **uniones internas (internal seams)** (privadas de su implementación, usadas por sus propios tests) además de la **unión externa (external seam)** que está en su interfaz.
- **La prueba de borrar.** Imagina que borras el módulo. Si la complejidad desaparece, era solo un intermediario (pass-through). Si la complejidad reaparece en N lugares que lo llamaban, estaba ganándose su lugar.
- **La interfaz es la superficie de prueba.** Quienes llaman y los tests cruzan la misma unión. Si quieres probar *más allá* de la interfaz, el módulo probablemente tiene la forma equivocada.
- **Un adaptador significa una unión hipotética. Dos adaptadores significan una unión real.** No pongas una unión a menos que algo de verdad varíe a través de ella.

## Diseñar para poder probar

Las buenas interfaces hacen que probar sea natural:

1. **Acepta dependencias, no las crees.**

   ```typescript
   // Testable
   function processOrder(order, paymentGateway) {}

   // Hard to test
   function processOrder(order) {
     const gateway = new StripeGateway();
   }
   ```

2. **Devuelve resultados, no produzcas efectos secundarios.**

   ```typescript
   // Testable
   function calculateDiscount(cart): Discount {}

   // Hard to test
   function applyDiscount(cart): void {
     cart.total -= discount;
   }
   ```

3. **Superficie pequeña.** Menos métodos = menos tests necesarios. Menos parámetros = preparación de tests más simple.

## Relaciones

- Un **Module** tiene exactamente una **Interface** (la superficie que muestra a quienes lo llaman y a los tests).
- La **Depth** es una propiedad de un **Module**, medida contra su **Interface**.
- Una **Seam** es donde vive la **Interface** de un **Module**.
- Un **Adapter** se sienta en una **Seam** y cumple la **Interface**.
- La **Depth** produce **Leverage** para quienes llaman y **Locality** para quienes mantienen.

## Enfoques rechazados

- **La profundidad como proporción de líneas de implementación sobre líneas de interfaz** (Ousterhout): premia rellenar la implementación. En cambio usamos profundidad-como-ganancia (depth-as-leverage).
- **"Interface" como la palabra clave `interface` de TypeScript o los métodos públicos de una clase**: muy limitado: la interfaz aquí incluye todo lo que quien llama debe saber.
- **"Boundary"**: se confunde con el bounded context de DDD. Mejor di **seam** o **interface**.

## Para ir más a fondo

- **Para profundizar un grupo dado sus dependencias**, ver [DEEPENING.md](DEEPENING.md): categorías de dependencias, disciplina de uniones y testing de reemplazar-en-vez-de-agregar-capas (replace-don't-layer).
- **Para explorar interfaces alternativas**, ver [DESIGN-IT-TWICE.md](DESIGN-IT-TWICE.md): lanza subagentes en paralelo para diseñar la interfaz de varias formas radicalmente distintas, y luego compara por profundidad, localidad y colocación de la unión.
