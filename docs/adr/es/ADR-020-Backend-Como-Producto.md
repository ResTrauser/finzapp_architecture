# ADR-020: El Backend como Producto Interno

## Estado
Aceptado

## Contexto
En organizaciones modernas, la API es consumida por múltiples "clientes" (App Móvil, Web SPA, Partners, Equipos de Data).
Tratar el backend como un simple "script que devuelve datos" genera fricción: documentación desactualizada, errores crípticos ("Internal Server Error") y contratos (JSON) que cambian sin aviso, rompiendo los clientes.
Esto convierte al equipo de Backend en un cuello de botella para la organización.

## Decisión
Adoptar la filosofía de **"Backend as a Product"**. La API interna se trata con el mismo rigor que un producto público (como Stripe o Twilio).

## Diseño Detallado

### Ecosistema de Consumo

```mermaid
mindmap
  root((Backend as Product))
    Quality Contracts
      Strict DTOs
      Consistent Errors
    Developer Experience
      Auto-Documentation
      Type Hints
    Reliability
      Uptime 99.9%
      Zero Downtime Deploys
    Business Value
      Faster Time-to-Market
      Data Integrity
```

### Generación Automática de SDKs
La documentación (OpenAPI/Swagger) es la fuente de verdad. No se escriben clientes HTTP a mano.

```mermaid
graph TD
    API[Core API] -->|Auto-Generates| Spec[OpenAPI / Swagger.json]

    Spec --> Docs[Redoc / Swagger UI]
    Spec --> Clients[Generated SDKs]

    Clients --> JS[TypeScript Client]
    Clients --> Py[Python Library]

    subgraph "Developer Experience"
        Docs
        JS
        Py
    end

    style Docs fill:#fff9c4
    style Clients fill:#e1f5fe
```

## Consecuencias

### Positivas
*   **Autonomía del Frontend:** Los equipos de frontend pueden trabajar más rápido usando SDKs tipados y documentación clara.
*   **Reducción de Errores:** Los contratos estrictos evitan `undefined is not a function` en el cliente.
*   **Onboarding:** Nuevos desarrolladores entienden el sistema leyendo la documentación interactiva.

### Negativas
*   **Rigor:** Requiere disciplina para mantener los esquemas DTO actualizados y bien descritos.
*   **Tooling:** Necesidad de configurar pipelines para generar y publicar los SDKs/Docs.

## Cumplimiento
*   Todos los endpoints deben tener descripciones, ejemplos de respuesta y códigos de error documentados en el código (anotaciones).
*   No se acepta un PR si la especificación OpenAPI generada es inválida.
