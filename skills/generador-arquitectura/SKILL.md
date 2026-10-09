---
name: generador-arquitectura
description: 'Generador completo de planos (blueprints) de arquitectura de proyectos: analiza el código para crear documentación detallada de arquitectura. Comprehensive project architecture blueprint generator that analyzes codebases to create detailed architectural documentation. Use when the user wants to analyze a codebase and generate architectural documentation, blueprints, diagrams, or implementation pattern guides. Detecta tecnologías y patrones automáticamente, genera diagramas visuales, documenta patrones de implementación y da planos extensibles para mantener la consistencia.'
---

# Generador completo de planos de arquitectura de proyecto

## Variables de configuración
${PROJECT_TYPE="Auto-detect|.NET|Java|React|Angular|Python|Node.js|Flutter|Other"} <!-- Tecnología principal -->
${ARCHITECTURE_PATTERN="Auto-detect|Clean Architecture|Microservices|Layered|MVVM|MVC|Hexagonal|Event-Driven|Serverless|Monolithic|Other"} <!-- Patrón de arquitectura principal -->
${DIAGRAM_TYPE="C4|UML|Flow|Component|None"} <!-- Tipo de diagrama de arquitectura -->
${DETAIL_LEVEL="High-level|Detailed|Comprehensive|Implementation-Ready"} <!-- Nivel de detalle a incluir -->
${INCLUDES_CODE_EXAMPLES=true|false} <!-- Incluir código de ejemplo para ilustrar patrones -->
${INCLUDES_IMPLEMENTATION_PATTERNS=true|false} <!-- Incluir patrones de implementación detallados -->
${INCLUDES_DECISION_RECORDS=true|false} <!-- Incluir registros de decisiones de arquitectura -->
${FOCUS_ON_EXTENSIBILITY=true|false} <!-- Enfocarse en puntos de extensión y patrones -->

## Prompt generado

"Crea un documento completo 'Project_Architecture_Blueprint.md' que analice a fondo los patrones de arquitectura del código y sirva como referencia definitiva para mantener la consistencia arquitectónica. Sigue este enfoque:

### 1. Detección y análisis de arquitectura
- ${PROJECT_TYPE == "Auto-detect" ? "Analyze the project structure to identify all technology stacks and frameworks in use by examining:
  - Project and configuration files
  - Package dependencies and import statements
  - Framework-specific patterns and conventions
  - Build and deployment configurations" : "Focus on ${PROJECT_TYPE} specific patterns and practices"}
  
- ${ARCHITECTURE_PATTERN == "Auto-detect" ? "Determine the architectural pattern(s) by analyzing:
  - Folder organization and namespacing
  - Dependency flow and component boundaries
  - Interface segregation and abstraction patterns
  - Communication mechanisms between components" : "Document how the ${ARCHITECTURE_PATTERN} architecture is implemented"}

### 2. Vista general de la arquitectura
- Explica de forma clara y corta el enfoque general de arquitectura
- Documenta los principios guía que se ven en las decisiones de arquitectura
- Identifica las fronteras de arquitectura y cómo se hacen cumplir
- Nota cualquier patrón híbrido o adaptación de patrones estándar

### 3. Visualización de la arquitectura
${DIAGRAM_TYPE != "None" ? `Create ${DIAGRAM_TYPE} diagrams at multiple levels of abstraction:
- High-level architectural overview showing major subsystems
- Component interaction diagrams showing relationships and dependencies
- Data flow diagrams showing how information moves through the system
- Ensure diagrams accurately reflect the actual implementation, not theoretical patterns` : "Describe the component relationships based on actual code dependencies, providing clear textual explanations of:
- Subsystem organization and boundaries
- Dependency directions and component interactions
- Data flow and process sequences"}

### 4. Componentes centrales de la arquitectura
Para cada componente de arquitectura encontrado en el código:

- **Propósito y responsabilidad (Purpose and Responsibility)**:
  - Función principal dentro de la arquitectura
  - Dominios de negocio o temas técnicos que cubre
  - Fronteras y límites de alcance

- **Estructura interna (Internal Structure)**:
  - Organización de clases y módulos dentro del componente
  - Abstracciones clave y sus implementaciones
  - Patrones de diseño usados

- **Patrones de interacción (Interaction Patterns)**:
  - Cómo se comunica el componente con los demás
  - Interfaces que expone y que consume
  - Patrones de inyección de dependencias (dependency injection)
  - Mecanismos de publicar y suscribirse a eventos (event publishing/subscription)

- **Patrones de evolución (Evolution Patterns)**:
  - Cómo se puede extender el componente
  - Puntos de variación y mecanismos de plugins
  - Enfoques de configuración y personalización

### 5. Capas de arquitectura y dependencias
- Mapea la estructura de capas tal como está implementada en el código
- Documenta las reglas de dependencia entre capas
- Identifica los mecanismos de abstracción que permiten separar capas
- Nota cualquier dependencia circular o violación de capas
- Documenta los patrones de inyección de dependencias usados para mantener la separación

### 6. Arquitectura de datos
- Documenta la estructura y organización del modelo de dominio
- Mapea relaciones entre entidades y patrones de agregación
- Identifica patrones de acceso a datos (repositorios, mapeadores de datos, etc.)
- Documenta cómo se transforman y mapean los datos
- Nota estrategias de caché e implementaciones
- Documenta patrones de validación de datos

### 7. Implementación de temas transversales (cross-cutting concerns)
Documenta los patrones de implementación para temas transversales:

- **Autenticación y autorización (Authentication & Authorization)**:
  - Cómo está implementado el modelo de seguridad
  - Patrones para aplicar permisos
  - Enfoque de gestión de identidad
  - Patrones de fronteras de seguridad

- **Manejo de errores y resiliencia (Error Handling & Resilience)**:
  - Patrones de manejo de excepciones
  - Implementaciones de reintentos y cortacircuitos (retry and circuit breaker)
  - Estrategias de respaldo y degradación elegante (fallback and graceful degradation)
  - Enfoques de reporte y monitoreo de errores

- **Registro y monitoreo (Logging & Monitoring)**:
  - Patrones de instrumentación
  - Cómo está implementada la observabilidad
  - Flujo de información de diagnóstico
  - Enfoque de monitoreo de rendimiento

- **Validación (Validation)**:
  - Estrategias de validación de entradas
  - Cómo se implementa la validación de reglas de negocio
  - Distribución de responsabilidades de validación
  - Patrones de reporte de errores

- **Gestión de configuración (Configuration Management)**:
  - Patrones de fuentes de configuración
  - Estrategias de configuración por ambiente
  - Enfoque de manejo de secretos
  - Implementación de banderas de funcionalidad (feature flags)

### 8. Patrones de comunicación entre servicios
- Documenta cómo están definidos los límites de cada servicio
- Identifica protocolos y formatos de comunicación
- Mapea patrones síncronos vs. asíncronos
- Documenta estrategias de versionado de API
- Identifica mecanismos de descubrimiento de servicios
- Nota patrones de resiliencia en la comunicación

### 9. Patrones de arquitectura específicos por tecnología
${PROJECT_TYPE == "Auto-detect" ? "For each detected technology stack, document specific architectural patterns:" : `Document ${PROJECT_TYPE}-specific architectural patterns:`}

${(PROJECT_TYPE == ".NET" || PROJECT_TYPE == "Auto-detect") ? 
"#### .NET Architectural Patterns (if detected)
- Host and application model implementation
- Middleware pipeline organization
- Framework service integration patterns
- ORM and data access approaches
- API implementation patterns (controllers, minimal APIs, etc.)
- Dependency injection container configuration" : ""}

${(PROJECT_TYPE == "Java" || PROJECT_TYPE == "Auto-detect") ? 
"#### Java Architectural Patterns (if detected)
- Application container and bootstrap process
- Dependency injection framework usage (Spring, CDI, etc.)
- AOP implementation patterns
- Transaction boundary management
- ORM configuration and usage patterns
- Service implementation patterns" : ""}

${(PROJECT_TYPE == "React" || PROJECT_TYPE == "Auto-detect") ? 
"#### React Architectural Patterns (if detected)
- Component composition and reuse strategies
- State management architecture
- Side effect handling patterns
- Routing and navigation approach
- Data fetching and caching patterns
- Rendering optimization strategies" : ""}

${(PROJECT_TYPE == "Angular" || PROJECT_TYPE == "Auto-detect") ? 
"#### Angular Architectural Patterns (if detected)
- Module organization strategy
- Component hierarchy design
- Service and dependency injection patterns
- State management approach
- Reactive programming patterns
- Route guard implementation" : ""}

${(PROJECT_TYPE == "Python" || PROJECT_TYPE == "Auto-detect") ? 
"#### Python Architectural Patterns (if detected)
- Module organization approach
- Dependency management strategy
- OOP vs. functional implementation patterns
- Framework integration patterns
- Asynchronous programming approach" : ""}

### 10. Patrones de implementación
${INCLUDES_IMPLEMENTATION_PATTERNS ? 
"Document concrete implementation patterns for key architectural components:

- **Interface Design Patterns**:
  - Interface segregation approaches
  - Abstraction level decisions
  - Generic vs. specific interface patterns
  - Default implementation patterns

- **Service Implementation Patterns**:
  - Service lifetime management
  - Service composition patterns
  - Operation implementation templates
  - Error handling within services

- **Repository Implementation Patterns**:
  - Query pattern implementations
  - Transaction management
  - Concurrency handling
  - Bulk operation patterns

- **Controller/API Implementation Patterns**:
  - Request handling patterns
  - Response formatting approaches
  - Parameter validation
  - API versioning implementation

- **Domain Model Implementation**:
  - Entity implementation patterns
  - Value object patterns
  - Domain event implementation
  - Business rule enforcement" : "Mention that detailed implementation patterns vary across the codebase."}

### 11. Arquitectura de pruebas (testing)
- Documenta estrategias de pruebas alineadas con la arquitectura
- Identifica patrones de fronteras de prueba (unitarias, integración, sistema)
- Mapea dobles de prueba y enfoques de simulación (test doubles and mocking)
- Documenta estrategias de datos de prueba
- Nota la integración de herramientas y frameworks de pruebas

### 12. Arquitectura de despliegue
- Documenta la topología de despliegue según la configuración
- Identifica adaptaciones de arquitectura por ambiente
- Mapea patrones de resolución de dependencias en ejecución
- Documenta la gestión de configuración entre ambientes
- Identifica enfoques de contenedores y orquestación
- Nota patrones de integración con servicios en la nube

### 13. Patrones de extensión y evolución
${FOCUS_ON_EXTENSIBILITY ? 
"Provide detailed guidance for extending the architecture:

- **Feature Addition Patterns**:
  - How to add new features while preserving architectural integrity
  - Where to place new components by type
  - Dependency introduction guidelines
  - Configuration extension patterns

- **Modification Patterns**:
  - How to safely modify existing components
  - Strategies for maintaining backward compatibility
  - Deprecation patterns
  - Migration approaches

- **Integration Patterns**:
  - How to integrate new external systems
  - Adapter implementation patterns
  - Anti-corruption layer patterns
  - Service facade implementation" : "Document key extension points in the architecture."}

${INCLUDES_CODE_EXAMPLES ? 
"### 14. Architectural Pattern Examples
Extract representative code examples that illustrate key architectural patterns:

- **Layer Separation Examples**:
  - Interface definition and implementation separation
  - Cross-layer communication patterns
  - Dependency injection examples

- **Component Communication Examples**:
  - Service invocation patterns
  - Event publication and handling
  - Message passing implementation

- **Extension Point Examples**:
  - Plugin registration and discovery
  - Extension interface implementations
  - Configuration-driven extension patterns

Include enough context with each example to show the pattern clearly, but keep examples concise and focused on architectural concepts." : ""}

${INCLUDES_DECISION_RECORDS ? 
"### 15. Architectural Decision Records
Document key architectural decisions evident in the codebase:

- **Architectural Style Decisions**:
  - Why the current architectural pattern was chosen
  - Alternatives considered (based on code evolution)
  - Constraints that influenced the decision

- **Technology Selection Decisions**:
  - Key technology choices and their architectural impact
  - Framework selection rationales
  - Custom vs. off-the-shelf component decisions

- **Implementation Approach Decisions**:
  - Specific implementation patterns chosen
  - Standard pattern adaptations
  - Performance vs. maintainability tradeoffs

For each decision, note:
- Context that made the decision necessary
- Factors considered in making the decision
- Resulting consequences (positive and negative)
- Future flexibility or limitations introduced" : ""}

### ${INCLUDES_DECISION_RECORDS ? "16" : INCLUDES_CODE_EXAMPLES ? "15" : "14"}. Gobierno de la arquitectura
- Documenta cómo se mantiene la consistencia arquitectónica
- Identifica verificaciones automáticas de cumplimiento de arquitectura
- Nota los procesos de revisión de arquitectura que se ven en el código
- Documenta las prácticas de documentación de arquitectura

### ${INCLUDES_DECISION_RECORDS ? "17" : INCLUDES_CODE_EXAMPLES ? "16" : "15"}. Plano para desarrollo nuevo
Crea una guía clara de arquitectura para implementar funciones nuevas:

- **Flujo de desarrollo (Development Workflow)**:
  - Puntos de partida para cada tipo de función
  - Secuencia para crear componentes
  - Pasos de integración con la arquitectura existente
  - Enfoque de pruebas por capa de arquitectura

- **Plantillas de implementación (Implementation Templates)**:
  - Plantillas de clases e interfaces base para componentes clave de arquitectura
  - Organización estándar de archivos para componentes nuevos
  - Patrones para declarar dependencias
  - Requisitos de documentación

- **Errores comunes (Common Pitfalls)**:
  - Violaciones de arquitectura que hay que evitar
  - Errores arquitectónicos comunes
  - Consideraciones de rendimiento
  - Puntos ciegos en pruebas

Incluye cuándo se generó este plano y recomendaciones para mantenerlo actualizado a medida que evoluciona la arquitectura."
