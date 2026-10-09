---
name: pruebas-jest
description: Buenas prácticas para escribir pruebas en JavaScript/TypeScript con Jest, incluyendo estrategias de mocks, estructura de pruebas y patrones comunes. | Best practices for writing JavaScript/TypeScript tests using Jest, including mocking strategies, test structure, and common patterns.
---

# Pruebas con Jest en JavaScript/TypeScript

### Estructura de pruebas
- Nombra los archivos de prueba con el sufijo `.test.ts` o `.test.js`
- Pon los archivos de prueba junto al código que prueban o en un directorio dedicado `__tests__`
- Usa nombres de prueba descriptivos que expliquen el comportamiento esperado
- Usa bloques describe anidados para organizar pruebas relacionadas
- Sigue el patrón: `describe('Componente/Función/Clase', () => { it('should hacer algo', () => {}) })`

### Mocks efectivos
- Haz mock de dependencias externas (APIs, bases de datos, etc.) para aislar tus pruebas
- Usa `jest.mock()` para mocks a nivel de módulo
- Usa `jest.spyOn()` para mocks de funciones concretas
- Usa `mockImplementation()` o `mockReturnValue()` para definir el comportamiento del mock
- Reinicia los mocks entre pruebas con `jest.resetAllMocks()` en `afterEach`

### Probar código asíncrono
- Retorna siempre las promesas o usa sintaxis async/await en las pruebas
- Usa los matchers `resolves`/`rejects` para promesas
- Pon timeouts adecuados para pruebas lentas con `jest.setTimeout()`

### Pruebas de snapshot
- Usa pruebas de snapshot para componentes UI u objetos complejos que cambian poco
- Mantén los snapshots pequeños y enfocados
- Revisa los cambios de snapshot con cuidado antes de hacer commit

### Probar componentes React
- Usa React Testing Library mejor que Enzyme para probar componentes
- Prueba el comportamiento de la persona usuaria y la accesibilidad del componente
- Busca elementos por roles de accesibilidad, etiquetas o contenido de texto
- Usa `userEvent` mejor que `fireEvent` para interacciones más realistas

## Matchers comunes de Jest
- Básicos: `expect(value).toBe(expected)`, `expect(value).toEqual(expected)`
- Veracidad: `expect(value).toBeTruthy()`, `expect(value).toBeFalsy()`
- Números: `expect(value).toBeGreaterThan(3)`, `expect(value).toBeLessThanOrEqual(3)`
- Cadenas: `expect(value).toMatch(/pattern/)`, `expect(value).toContain('substring')`
- Arrays: `expect(array).toContain(item)`, `expect(array).toHaveLength(3)`
- Objetos: `expect(object).toHaveProperty('key', value)`
- Excepciones: `expect(fn).toThrow()`, `expect(fn).toThrow(Error)`
- Funciones mock: `expect(mockFn).toHaveBeenCalled()`, `expect(mockFn).toHaveBeenCalledWith(arg1, arg2)`
