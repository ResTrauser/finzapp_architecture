# ADR-008: Estandarización de Respuestas API (Backend Design System)

## Estado
Aceptado

## Contexto
En un entorno con múltiples desarrolladores y consumidores (Frontend, Mobile, Partners), la inconsistencia en las respuestas de la API es una fuente importante de fricción. Algunos endpoints devuelven listas directas, otros envuelven en objetos `data`, los errores tienen formatos dispares, y los códigos HTTP no siempre se respetan.

Esto dificulta la creación de clientes robustos y aumenta el tiempo de integración.

## Decisión
Implementar un **Backend Design System** que estandarice todas las entradas y salidas de la API.

Se establece el uso obligatorio de DTOs (Data Transfer Objects) y un formato de respuesta unificado `ApiResponse<T>`.

## Diseño Detallado

### Estructura de Respuesta Estándar
Todas las respuestas exitosas deben seguir este esquema JSON:

```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": { ... }, // El payload real
  "meta": { ... }  // Paginación, trazas, etc.
}
```

Todas las respuestas de error deben seguir este esquema:

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "User with ID 123 not found",
    "details": []
  }
}
```

### Diagrama de Flujo de Datos

```mermaid
graph LR
    Request[HTTP Request] --> DTO_In[Input DTO (Pydantic)]
    DTO_In -- Validate --> Handler[Business Logic]
    Handler -- Result --> Domain[Domain Entity]
    Domain -- Map --> DTO_Out[Output DTO]
    DTO_Out -- Wrap --> Response[ApiResponse JSON]

    style DTO_In fill:#e1f5fe
    style DTO_Out fill:#e1f5fe
    style Response fill:#fff9c4
```

## Consecuencias

### Positivas
*   **Previsibilidad:** El frontend sabe exactamente cómo parsear cualquier respuesta.
*   **Seguridad de Tipos:** Los DTOs actúan como contrato estricto, evitando exponer accidentalmente campos sensibles de la base de datos (e.g. `password_hash`).
*   **Mantenibilidad:** Cambios en el modelo de base de datos no rompen la API si el mapeo al DTO se actualiza.

### Negativas
*   **Boilerplate:** Requiere escribir clases DTO y mappers para cada entidad.
*   **Verbosity:** Las respuestas simples (ej. un booleano) se envuelven en un objeto JSON más grande.

## Cumplimiento
*   Prohibido devolver modelos de ORM (SQLAlchemy, Django Models) directamente en el controlador.
*   Todos los endpoints deben estar tipados con `Response[ApiResponse[MyDto]]`.
