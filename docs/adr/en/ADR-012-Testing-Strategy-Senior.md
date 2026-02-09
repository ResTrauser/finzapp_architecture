# ADR-012: Testing and Quality Assurance (QA) Strategy

## Status
Accepted

## Context
In critical financial systems like `finzapp_api`, a flaw in business logic (interest calculation, payment processing) can have legal consequences and direct financial losses. Relying solely on manual testing (Manual QA) is slow, error-prone, and does not scale with the team.

The lack of a robust test suite generates "fear of change" (refactoring), which stagnates the technical evolution of the project and accumulates technical debt.

## Decision
Adopt an **Automated Testing Pyramid** strategy, prioritizing speed and early feedback.

## Detailed Design

### The Testing Pyramid

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

### Test Types
1.  **Unit Tests:** Test isolated functions and classes (Services, Domain Models). Do not touch the database or network. Must run in milliseconds.
2.  **Integration Tests:** Validate that components (Slices) interact correctly with the Database (in-memory SQLite or ephemeral Docker container) and other services (Redis).
3.  **E2E / API Tests:** Validate the entire flow from the user's perspective (HTTP Request -> HTTP Response), ensuring Routers, Middlewares, and Serializers work together.

### Tools
*   **Framework:** `pytest` (de facto standard in Python).
*   **Fixtures:** For setup/teardown of test data.
*   **Testcontainers:** To spin up real dependencies (PostgreSQL, Redis) in CI/CD environments.

## Consequences

### Positive
*   **Confidence in Refactoring:** If tests pass, the change is safe. Allows cleaning code without fear.
*   **Living Documentation:** Tests describe how the system *should* behave under different scenarios.
*   **Early Detection:** Bugs are found in development (cheap), not in production (expensive).

### Negative
*   **Initial Development Time:** Writing tests consumes time initially (though saves it in the long run).
*   **Maintenance:** Tests must be kept up to date with changing requirements.

## Compliance
*   No PR (Pull Request) is approved if it reduces code coverage or breaks existing tests.
*   Every bug reported in production must be replicated first with a failing test (TDD) before fixing it.
