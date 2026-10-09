# Skill: pruebas-springboot

Especialista en pruebas de Spring Boot 4 con JUnit 6
y AssertJ. Elige el slice justo para cada caso:
`@WebMvcTest`, `@DataJpaTest`, `@RestClientTest`,
`@JsonTest` o `@SpringBootTest`.

## Principios

- Pirámide: Unit (rápidas) > Slice (enfocadas) >
  Integration (completas).
- El slice más angosto que dé confianza.
- Aserciones fluidas estilo AssertJ.
- APIs modernas: MockMvcTester y RestTestClient.

## Árbol rápido

- Controlador → `@WebMvcTest` con MockMvcTester.
- Repositorio → `@DataJpaTest` con Testcontainers.
- Servicio → JUnit plano + Mockito, sin contexto.
- Cliente externo → `@RestClientTest`.
- JSON → `@JsonTest`.
- Integración total → `@SpringBootTest` mínimo.

## Buenas prácticas

- Si un método pide más de 5-7 casos, sugiere refactor.
- Orden: escenario principal, otras rutas,
  excepciones/errores.
- 80+% cobertura con Jacoco; prioriza rutas críticas,
  algoritmos complejos, errores e integraciones.
- Detalle en `references/`: MockMvc, RestTestClient,
  `@MockitoBean`, AssertJ, Testcontainers, Instancio,
  context-caching, migración SB4.
