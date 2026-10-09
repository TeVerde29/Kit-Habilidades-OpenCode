---
name: patrones-backend-nodejs
description: Crea servicios backend en Node.js listos para producción con Express/Fastify, aplicando patrones de middleware, manejo de errores, autenticación, integración con bases de datos y buenas prácticas de diseño de APIs. Úsala al crear servidores Node.js, APIs REST, backends GraphQL o arquitecturas de microservicios. | Build production-ready Node.js backend services with Express/Fastify, implementing middleware patterns, error handling, authentication, database integration, and API design best practices. Use when creating Node.js servers, REST APIs, GraphQL backends, or microservices architectures.
---

# Patrones de Backend en Node.js

Guía completa para construir aplicaciones backend en Node.js escalables, mantenibles y listas para producción, con frameworks modernos, patrones de arquitectura y buenas prácticas.

## Cuándo usar esta skill

- Construir APIs REST o servidores GraphQL
- Crear microservicios con Node.js
- Implementar autenticación y autorización
- Diseñar arquitecturas backend escalables
- Configurar middleware y manejo de errores
- Integrar bases de datos (SQL y NoSQL)
- Construir apps en tiempo real con WebSockets
- Implementar procesamiento de trabajos en segundo plano

## Patrones detallados y ejemplos trabajados

La documentación detallada de patrones vive en `references/details.md`. Lee ese archivo cuando el nivel de navegación de arriba no baste.

## Buenas prácticas

1. **Usa TypeScript**: la seguridad de tipos evita errores en ejecución
2. **Implementa buen manejo de errores**: usa clases de error propias
3. **Valida la entrada**: usa librerías como Zod o Joi
4. **Usa variables de entorno**: nunca pongas secretos en el código
5. **Implementa logging**: usa logging estructurado (Pino, Winston)
6. **Añade rate limiting**: evita abusos
7. **Usa HTTPS**: siempre en producción
8. **Implementa CORS bien**: no uses `*` en producción
9. **Usa inyección de dependencias**: pruebas y mantenimiento más fáciles
10. **Escribe pruebas**: pruebas unitarias, de integración y E2E
11. **Maneja el apagado elegante**: limpia recursos
12. **Usa connection pooling**: para bases de datos
13. **Implementa health checks**: para monitoreo
14. **Usa compresión**: reduce el tamaño de respuesta
15. **Monitorea el rendimiento**: usa herramientas APM

## Patrones de pruebas

Mira la skill `javascript-testing-patterns` para una guía completa de pruebas.
