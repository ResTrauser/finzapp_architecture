# ADR-012: Estrategia de Testing y Aseguramiento de Calidad (QA)

## Estado
Aceptado

## Contexto
En sistemas financieros críticos como `finzapp_api`, un error en la lógica de negocio (cálculo de intereses, procesamiento de pagos) puede tener consecuencias legales y pérdidas económicas directas. Confiar únicamente en pruebas manuales (QA manual) es lento, propenso a errores y no escala con el equipo.

La falta de una suite de pruebas robusta genera "miedo al cambio" (refactoring), lo que estanca la evolución técnica del proyecto y acumula deuda técnica.

## Decisión
Adoptar una estrategia de **Testing Automatizado en Pirámide**, priorizando la velocidad y el feedback temprano.

## Diseño Detallado

### La Pirámide de Testing

```mermaid
graph BT
    subgraph Pyramid["Testing Pyramid"]
        E2E[E2E/API Tests (High Confidence, Slow)]
        Integration[Integration Tests (Database, Redis, Slices)]
        Unit[Unit Tests (Fast, Business Logic, Mocked Dependencies)]

        Unit --> Integration
        Integration --> E2E
    end

    style E2E fill:#ffccbc
    style Integration fill:#fff176
    style Unit fill:#c5e1a5
```

### Tipos de Tests
1.  **Unit Tests (Unitarios):** Prueban funciones y clases aisladas (Servicios, Modelos de Dominio). No tocan base de datos ni red. Deben ejecutarse en milisegundos.
2.  **Integration Tests (Integración):** Validan que los componentes (Slices) interactúen correctamente con la Base de Datos (SQLite en memoria o contenedor Docker efímero) y otros servicios (Redis).
3.  **E2E / API Tests:** Validan el flujo completo desde el punto de vista del usuario (HTTP Request -> HTTP Response), asegurando que Routers, Middlewares y Serializadores funcionen en conjunto.

### Herramientas
*   **Framework:** `pytest` (estándar de facto en Python).
*   **Fixtures:** Para setup/teardown de datos de prueba.
*   **Testcontainers:** Para levantar dependencias reales (PostgreSQL, Redis) en entornos de CI/CD.

## Consecuencias

### Positivas
*   **Confianza en el Refactor:** Si los tests pasan, el cambio es seguro. Permite limpiar código sin miedo.
*   **Documentación Viva:** Los tests describen cómo *debe* comportarse el sistema ante diferentes escenarios.
*   **Detección Temprana:** Los bugs se encuentran en desarrollo (barato), no en producción (caro).

### Negativas
*   **Tiempo de Desarrollo Inicial:** Escribir tests consume tiempo al principio (aunque lo ahorra a largo plazo).
*   **Mantenimiento:** Los tests deben mantenerse al día con los cambios de requerimientos.

## Cumplimiento
*   Ningún PR (Pull Request) se aprueba si reduce la cobertura de código (Code Coverage) o rompe tests existentes.
*   Todo bug reportado en producción debe replicarse primero con un test que falle (TDD) antes de arreglarlo.
