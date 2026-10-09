---
name: generador-readme
description: 'Generador inteligente de README.md que analiza la estructura de documentación del proyecto y crea documentación completa del repositorio. Intelligent README.md generation prompt that analyzes project documentation structure and creates comprehensive repository documentation. Escanea los archivos del directorio .github/copilot y copilot-instructions.md para extraer información del proyecto, stack tecnológico, arquitectura, flujo de desarrollo, estándares de código y enfoque de pruebas, generando documentación markdown bien estructurada con formato adecuado, referencias cruzadas y contenido enfocado al desarrollador.'
---

# Prompt generador de README

Genera un README.md completo para este repositorio analizando los archivos de documentación del directorio .github/copilot y el archivo copilot-instructions.md. Sigue estos pasos:

1. Revisa todos los archivos de la carpeta .github/copilot, como:
   - Architecture
   - Code_Exemplars
   - Coding_Standards
   - Project_Folder_Structure
   - Technology_Stack
   - Unit_Tests
   - Workflow_Analysis

2. Revisa también el archivo copilot-instructions.md de la carpeta .github

3. Crea un README.md con las siguientes secciones:

## Nombre y descripción del proyecto
- Extrae el nombre del proyecto y su propósito principal de la documentación
- Incluye una descripción corta de lo que hace el proyecto

## Stack tecnológico
- Lista las tecnologías, lenguajes y frameworks principales usados
- Incluye información de versiones cuando esté disponible
- Obtén esta información sobre todo del archivo Technology_Stack

## Arquitectura del proyecto
- Da una vista general de alto nivel de la arquitectura
- Considera incluir un diagrama simple si la documentación lo describe
- Obtén la información del archivo Architecture

## Primeros pasos
- Incluye instrucciones de instalación según el stack tecnológico
- Agrega pasos de setup y configuración
- Incluye cualquier prerrequisito

## Estructura del proyecto
- Vista breve de la organización de carpetas
- Obtén la información del archivo Project_Folder_Structure

## Funciones principales
- Lista las funciones y características principales del proyecto
- Extrae la información de los varios archivos de documentación

## Flujo de desarrollo
- Resume el proceso de desarrollo
- Incluye información sobre la estrategia de ramas si está disponible
- Obtén la información del archivo Workflow_Analysis

## Estándares de código
- Resume los estándares y convenciones clave de código
- Obtén la información del archivo Coding_Standards

## Pruebas
- Explica el enfoque y las herramientas de pruebas
- Obtén la información del archivo Unit_Tests

## Cómo contribuir
- Guías para contribuir al proyecto
- Refiere a los ejemplos de código como guía
- Obtén la información de Code_Exemplars y copilot-instructions

## Licencia
- Incluye la información de licencia si está disponible

Da formato al README con Markdown adecuado, incluyendo:
- Encabezados y subencabezados claros
- Bloques de código donde aplique
- Listas para mejor lectura
- Enlaces a otros archivos de documentación
- Badges de estado de build, versión, etc. si la información está disponible

Mantén el README conciso pero informativo, enfocado en lo que un desarrollador nuevo o un usuario necesitaría saber del proyecto.
