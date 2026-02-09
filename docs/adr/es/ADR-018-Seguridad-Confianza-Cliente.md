# ADR-018: Seguridad en Capas (Defense in Depth) para Confianza del Cliente

## Estado
Aceptado

## Contexto
En el mercado B2B Enterprise y Fintech, la seguridad no es un "feature" opcional, sino un requisito contractual y de cumplimiento (SOC2, GDPR, PCI-DSS).
Confiar en una sola capa de defensa (e.g., solo autenticación) es insuficiente.
Los clientes corporativos exigen garantías de que sus datos están cifrados, aislados y protegidos contra ataques avanzados.

## Decisión
Implementar una estrategia de **Defensa en Profundidad (Defense in Depth)**, aplicando múltiples controles de seguridad en cada capa del stack tecnológico.

## Diseño Detallado

### Capas de Seguridad

```mermaid
graph TD
    subgraph SecurityLayers["Defense in Depth"]
        L1(Edge: Cloudflare / WAF)
        L2(Network: VPC / Rate Limiting)
        L3(App: Auth / MFA / Middleware)
        L4(Data: Encryption at Rest / Row Level Security)

        L1 --> L2
        L2 --> L3
        L3 --> L4
    end

    style L1 fill:#ffebee
    style L2 fill:#ffcdd2
    style L3 fill:#ef9a9a
    style L4 fill:#e57373
```

### Gestión de Sesiones (Refresh Tokens)
Para minimizar el riesgo de robo de credenciales, los Access Tokens tienen vida corta (15 min) y se rotan mediante Refresh Tokens seguros (HttpOnly Cookie).

```mermaid
sequenceDiagram
    participant App
    participant Auth
    participant DB

    App->>Auth: Request Data (Access Token Expired)
    Auth-->>App: 401 Unauthorized

    App->>Auth: POST /refresh-token (Refresh Token)
    Auth->>DB: Validate Refresh Token (Not Revoked?)
    Auth-->>App: New Access Token (15min) + New Refresh Token

    Note over App: Seamless UX:\nUser doesn't log in again
```

## Consecuencias

### Positivas
*   **Ventas B2B:** Habilita cerrar contratos con grandes empresas que auditan la seguridad.
*   **Confianza:** Los usuarios finales confían sus datos financieros a la plataforma.
*   **Resiliencia:** Si una capa falla (e.g. WAF bypass), hay otras capas protegiendo el núcleo (e.g. RLS en DB).

### Negativas
*   **Fricción de Desarrollo:** Requiere configurar y mantener múltiples herramientas (WAF, Vault, IAM).
*   **Complejidad de UX:** El uso de MFA y sesiones cortas puede incomodar a usuarios no técnicos si no se diseña bien.

## Cumplimiento
*   Todos los secretos (API Keys, DB Passwords) deben gestionarse en un Secret Manager, nunca en código.
*   Los datos sensibles (PII) deben estar cifrados en reposo (AES-256) y en tránsito (TLS 1.3).
