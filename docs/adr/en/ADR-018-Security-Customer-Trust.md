# ADR-018: Defense in Depth Security for Customer Trust

## Status
Accepted

## Context
In the B2B Enterprise and Fintech market, security is not an optional feature but a contractual and compliance requirement (SOC2, GDPR, PCI-DSS).
Relying on a single layer of defense (e.g., just authentication) is insufficient.
Corporate clients demand guarantees that their data is encrypted, isolated, and protected against advanced attacks.

## Decision
Implement a **Defense in Depth** strategy, applying multiple security controls at each layer of the technology stack.

## Detailed Design

### Security Layers

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

### Session Management (Refresh Tokens)
To minimize credential theft risk, Access Tokens are short-lived (15 min) and rotated via secure Refresh Tokens (HttpOnly Cookie).

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

## Consequences

### Positive
*   **B2B Sales:** Enables closing contracts with large companies that audit security.
*   **Trust:** End users trust the platform with their financial data.
*   **Resilience:** If one layer fails (e.g., WAF bypass), other layers protect the core (e.g., RLS in DB).

### Negative
*   **Development Friction:** Requires configuring and maintaining multiple tools (WAF, Vault, IAM).
*   **UX Complexity:** MFA and short sessions can annoy non-technical users if not designed well.

## Compliance
*   All secrets (API Keys, DB Passwords) must be managed in a Secret Manager, never in code.
*   Sensitive data (PII) must be encrypted at rest (AES-256) and in transit (TLS 1.3).
