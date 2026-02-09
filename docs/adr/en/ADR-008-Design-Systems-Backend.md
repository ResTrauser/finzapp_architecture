# ADR-008: API Response Standardization (Backend Design System)

## Status
Accepted

## Context
In an environment with multiple developers and consumers (Frontend, Mobile, Partners), inconsistency in API responses is a major source of friction. Some endpoints return direct lists, others wrap in `data` objects, errors have disparate formats, and HTTP codes are not always respected.

This makes it difficult to create robust clients and increases integration time.

## Decision
Implement a **Backend Design System** that standardizes all API inputs and outputs.

Mandatory use of DTOs (Data Transfer Objects) and a unified `ApiResponse<T>` response format is established.

## Detailed Design

### Standard Response Structure
All successful responses must follow this JSON schema:

```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": { ... }, // The actual payload
  "meta": { ... }  // Pagination, traces, etc.
}
```

All error responses must follow this schema:

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

### Data Flow Diagram

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

## Consequences

### Positive
*   **Predictability:** The frontend knows exactly how to parse any response.
*   **Type Safety:** DTOs act as a strict contract, preventing accidental exposure of sensitive database fields (e.g., `password_hash`).
*   **Maintainability:** Changes in the database model do not break the API if the mapping to the DTO is updated.

### Negative
*   **Boilerplate:** Requires writing DTO classes and mappers for each entity.
*   **Verbosity:** Simple responses (e.g., a boolean) are wrapped in a larger JSON object.

## Compliance
*   Prohibited to return ORM models (SQLAlchemy, Django Models) directly in the controller.
*   All endpoints must be typed with `Response[ApiResponse[MyDto]]`.
