---
name: pruebas-springboot
description: Especialista en pruebas de Spring Boot 4 que elige las mejores técnicas de prueba para tu caso con JUnit 6 y AssertJ. Expert Spring Boot 4 testing specialist that selects the best Spring Boot testing techniques for your situation with JUnit 6 and AssertJ.
license: MIT
compatibility: Requires Spring Boot 4.0+, JUnit 6, AssertJ, Instancio, Java 25+
metadata:
  version: "1.1.0"
---

# Pruebas en Spring Boot

Guía experta para probar aplicaciones Spring Boot 4 con patrones modernos y buenas prácticas.

## Principios clave

1. **Pirámide de pruebas**: Unit (rápidas) > Slice (enfocadas) > Integration (completas)
2. **La herramienta justa**: usa el slice más angosto que te dé confianza
3. **Estilo AssertJ**: aserciones fluidas y legibles en vez de matchers verbosos
4. **APIs modernas**: prefiere MockMvcTester y RestTestClient sobre las alternativas legacy

## ¿Qué slice de prueba?

| Escenario | Anotación | Referencia |
|-----------|------------|-----------|
| Controlador + semántica HTTP | `@WebMvcTest` | [references/webmvctest.md](references/webmvctest.md) |
| Repositorio + consultas JPA | `@DataJpaTest` | [references/datajpatest.md](references/datajpatest.md) |
| Cliente REST + APIs externas | `@RestClientTest` | [references/restclienttest.md](references/restclienttest.md) |
| (De)serialización JSON | `@JsonTest` | Ver references/test-slices-overview.md |
| Aplicación completa | `@SpringBootTest` | Ver references/test-slices-overview.md |

## Referencia de slices de prueba

- [references/test-slices-overview.md](references/test-slices-overview.md) - Matriz de decisión y comparación
- [references/webmvctest.md](references/webmvctest.md) - Capa web con MockMvc
- [references/datajpatest.md](references/datajpatest.md) - Capa de datos con Testcontainers
- [references/restclienttest.md](references/restclienttest.md) - Pruebas de clientes REST

## Referencia de herramientas de prueba

- [references/mockmvc-tester.md](references/mockmvc-tester.md) - MockMvc estilo AssertJ (3.2+)
- [references/mockmvc-classic.md](references/mockmvc-classic.md) - MockMvc tradicional (pre-3.2)
- [references/resttestclient.md](references/resttestclient.md) - Cliente REST de Spring Boot 4+
- [references/mockitobean.md](references/mockitobean.md) - Mocks de dependencias

## Librerías de aserciones

- [references/assertj-basics.md](references/assertj-basics.md) - Escalares, strings, booleanos, fechas
- [references/assertj-collections.md](references/assertj-collections.md) - Listas, Sets, Maps, arreglos

## Testcontainers

- [references/testcontainers-jdbc.md](references/testcontainers-jdbc.md) - PostgreSQL, MySQL, etc.

## Generación de datos de prueba

- [references/instancio.md](references/instancio.md) - Genera objetos complejos de prueba (3+ propiedades)

## Rendimiento y migración

- [references/context-caching.md](references/context-caching.md) - Acelera tus suites de pruebas
- [references/sb4-migration.md](references/sb4-migration.md) - Cambios de Spring Boot 4.0

## Árbol de decisión rápida

```
¿Pruebas un endpoint de controlador?
  Sí → @WebMvcTest con MockMvcTester

¿Pruebas consultas de un repositorio?
  Sí → @DataJpaTest con Testcontainers (DB real)

¿Pruebas lógica de negocio en un servicio?
  Sí → JUnit plano + Mockito (sin contexto Spring)

¿Pruebas un cliente de API externa?
  Sí → @RestClientTest con MockRestServiceServer

¿Pruebas mapeo JSON?
  Sí → @JsonTest

¿Necesitas una prueba de integración completa?
  Sí → @SpringBootTest con configuración mínima de contexto
```

## Novedades de Spring Boot 4

- **RestTestClient**: alternativa moderna a TestRestTemplate
- **@MockitoBean**: reemplaza a @MockBean (obsoleto)
- **MockMvcTester**: aserciones estilo AssertJ para pruebas web
- **Starters modulares**: starters de prueba por tecnología
- **Pausa de contextos**: pausa automática de contextos cacheados (Spring Framework 7)

## Buenas prácticas de pruebas

### Evaluación de complejidad del código

Cuando un método o clase es muy complejo para probarlo bien:

1. **Analiza la complejidad** - Si necesitas más de 5-7 casos para cubrir un solo método, probablemente es muy complejo
2. **Recomienda refactor** - Sugiere dividir el código en funciones pequeñas y enfocadas
3. **Decide el usuario** - Si el usuario acepta refactorizar, ayuda a identificar puntos de extracción
4. **Sigue si toca** - Si el usuario decide seguir con el código complejo, implementa las pruebas aunque cueste

**Ejemplo de recomendación de refactor:**
```java
// Antes: método complejo difícil de probar
public Order processOrder(OrderRequest request) {
  // Validación, cálculo de descuento, pago, inventario, notificación...
  // 50+ líneas de concerns mezclados
}

// Después: refactorizado en unidades probables
public Order processOrder(OrderRequest request) {
  validateOrder(request);
  var order = createOrder(request);
  applyDiscount(order);
  processPayment(order);
  updateInventory(order);
  sendNotification(order);
  return order;
}
```

### Evita redundancia en el código

Crea métodos ayudantes para objetos de uso común y setup de mocks para mejorar legibilidad y mantenimiento.

### Organización de pruebas con @DisplayName

Usa nombres descriptivos para aclarar la intención de la prueba:

```java
@Test
@DisplayName("Should calculate discount for VIP customer")
void shouldCalculateDiscountForVip() { }

@Test
@DisplayName("Should reject order when customer has insufficient credit")
void shouldRejectOrderForInsufficientCredit() { }
```

### Orden de cobertura de pruebas

Estructura siempre las pruebas en este orden:

1. **Escenario principal** - El camino feliz, el caso más común
2. **Otras rutas** - Escenarios válidos alternos, casos borde
3. **Excepciones/Errores** - Entradas inválidas, condiciones de error, modos de fallo

### Prueba escenarios de producción

Escribe pruebas pensando en escenarios reales de producción. Esto hace las pruebas más cercanas y ayuda a entender cómo se comporta el código en casos reales.

### Metas de cobertura de pruebas

Apunta a 80% de cobertura como balance práctico entre calidad y esfuerzo. Más cobertura ayuda, pero no es la única meta.

Usa el plugin Jacoco de maven para reportes y seguimiento de cobertura.


**Reglas de cobertura:**
- 80+% de cobertura mínimo
- Enfócate en aserciones con sentido, no solo en ejecutar código

**Qué priorizar:**
1. Rutas críticas de negocio (procesar pagos, validar órdenes)
2. Algoritmos complejos (precios, cálculo de descuentos)
3. Manejo de errores (excepciones, casos borde)
4. Puntos de integración (APIs externas, bases de datos)

## Dependencias (Spring Boot 4)

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>

<!-- Para pruebas WebMvc -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-webmvc-test</artifactId>
  <scope>test</scope>
</dependency>

<!-- Para Testcontainers -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-testcontainers</artifactId>
  <scope>test</scope>
</dependency>
```
