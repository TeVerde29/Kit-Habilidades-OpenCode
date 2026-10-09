---
name: java-springboot
description: >
  Buenas prácticas para desarrollar aplicaciones con Spring Boot.
  Best practices for developing applications with Spring Boot.
---

# Buenas prácticas con Spring Boot

Tu objetivo es ayudarme a escribir aplicaciones Spring Boot de alta calidad siguiendo buenas prácticas establecidas.

## Configuración del proyecto y estructura

- **Herramienta de construcción:** usa Maven (`pom.xml`) o Gradle (`build.gradle`) para la gestión de dependencias.
- **Starters:** usa starters de Spring Boot (p. ej. `spring-boot-starter-web`, `spring-boot-starter-data-jpa`) para simplificar la gestión de dependencias.
- **Estructura de paquetes:** organiza el código por funcionalidad/dominio (p. ej. `com.example.app.order`, `com.example.app.user`) en lugar de por capa (p. ej. `com.example.app.controller`, `com.example.app.service`).

## Inyección de dependencias y componentes

- **Inyección por constructor:** usa siempre inyección por constructor para dependencias obligatorias. Esto facilita las pruebas y hace explícitas las dependencias.
- **Inmutabilidad:** declara los campos de dependencias como `private final`.
- **Estereotipos de componente:** usa las anotaciones `@Component`, `@Service`, `@Repository` y `@Controller`/`@RestController` de forma adecuada para definir beans.

## Configuración

- **Configuración externalizada:** usa `application.yml` (o `application.properties`) para la configuración. YAML suele preferirse por su legibilidad y estructura jerárquica.
- **Propiedades tipadas:** usa `@ConfigurationProperties` para enlazar la configuración a objetos Java fuertemente tipados.
- **Perfiles:** usa perfiles de Spring (`application-dev.yml`, `application-prod.yml`) para gestionar configuraciones específicas por entorno.
- **Gestión de secretos:** no escribas secretos directamente en el código. Usa variables de entorno o una herramienta dedicada como HashiCorp Vault o AWS Secrets Manager.

## Capa web (controladores)

- **APIs RESTful:** diseña endpoints RESTful claros y consistentes.
- **DTOs (objetos de transferencia de datos):** usa DTOs para exponer y consumir datos en la capa API. No expongas entidades JPA directamente al cliente.
- **Validación:** usa validación de beans Java (JSR 380) con anotaciones (`@Valid`, `@NotNull`, `@Size`) en los DTOs para validar las peticiones.
- **Manejo de errores:** implementa un manejador global de excepciones con `@ControllerAdvice` y `@ExceptionHandler` para respuestas de error consistentes.

## Capa de servicio

- **Lógica de negocio:** encapsula toda la lógica de negocio en clases `@Service`.
- **Sin estado:** los servicios deben ser stateless (sin estado).
- **Gestión de transacciones:** usa `@Transactional` en métodos de servicio para gestionar transacciones de forma declarativa. Aplícalo al nivel más granular necesario.

## Capa de datos (repositorios)

- **Spring Data JPA:** usa repositorios Spring Data JPA extendiendo `JpaRepository` o `CrudRepository` para operaciones estándar de base de datos.
- **Consultas personalizadas:** para consultas complejas, usa `@Query` o la Criteria API de JPA.
- **Proyecciones:** usa proyecciones DTO para traer solo los datos necesarios de la base de datos.

## Registro (logging)

- **SLF4J:** usa la API SLF4J para el registro.
- **Declaración del logger:** `private static final Logger logger = LoggerFactory.getLogger(MyClass.class);`
- **Registro parametrizado:** usa mensajes parametrizados (`logger.info("Processing user {}...", userId);`) en lugar de concatenar cadenas para mejorar el rendimiento.

## Pruebas

- **Pruebas unitarias:** escribe pruebas unitarias para servicios y componentes usando JUnit 5 y un framework de mocks como Mockito.
- **Pruebas de integración:** usa `@SpringBootTest` para pruebas de integración que cargan el contexto de la aplicación Spring.
- **Slices de prueba:** usa anotaciones de slice como `@WebMvcTest` (para controladores) o `@DataJpaTest` (para repositorios) para probar partes aisladas.
- **Testcontainers:** considera usar Testcontainers para pruebas de integración fiables con bases de datos reales, brokers de mensajes, etc.

## Seguridad

- **Spring Security:** usa Spring Security para autenticación y autorización.
- **Codificación de contraseñas:** codifica siempre las contraseñas con un algoritmo robusto como BCrypt.
- **Saneamiento de entradas:** evita inyección SQL usando Spring Data JPA o consultas parametrizadas. Evita Cross-Site Scripting (XSS) codificando correctamente la salida.
