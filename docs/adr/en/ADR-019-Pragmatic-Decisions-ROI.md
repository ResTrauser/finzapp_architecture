# ADR-019: Pragmatic Architecture Based on Return on Investment (ROI)

## Status
Accepted

## Context
In the current tech ecosystem, there is strong pressure to adopt complex architectures (Microservices, Kubernetes, Serverless) prematurely.
However, for a startup or growing product, the scarcest resource is engineering time.
Investing months in distributed infrastructure before validating the business model is a high financial risk (Opportunity Cost).

## Decision
Prioritize a **Modular Monolith (Vertical Slice)** approach over Distributed Microservices for the current business stage.

The decision is based on maximizing ROI: obtaining the most business functionality with the least accidental complexity possible.

## Cost-Benefit Analysis

```mermaid
quadrantChart
    title Complexity vs Value Analysis
    x-axis Low Complexity --> High Complexity
    y-axis Low Value --> High Value
    quadrant-1 "Strategic Investment"
    quadrant-2 "Quick Wins"
    quadrant-3 "Don't Do It"
    quadrant-4 "Technological Black Hole"

    "Modular Monolith": [0.3, 0.8]
    "Microservices (Premature)": [0.9, 0.4]
    "Spaghetti Code": [0.1, 0.1]
    "Serverless Functions": [0.6, 0.6]
```

### Time to Market (MVP)

```mermaid
gantt
    title Time to MVP (3 Months)
    dateFormat YYYY-MM-DD

    section Microservices
    Infra Setup (K8s, Istio) : 2024-01-01, 30d
    Service A Boilerplate    : 2024-02-01, 10d
    Service B Boilerplate    : 2024-02-11, 10d
    Business Logic           : 2024-02-21, 20d
    Integration Testing      : 2024-03-12, 15d

    section Monolith
    Infra Setup (PaaS)       : 2024-01-01, 5d
    Shared Kernel            : 2024-01-06, 10d
    Business Logic (Features): 2024-01-16, 60d
    Deploy                   : 2024-03-17, 2d
```

## Consequences

### Positive
*   **Speed:** The team focuses on solving user problems, not configuring mesh networks.
*   **Operational Costs:** A single instance (or small cluster) is cheaper than orchestrating dozens of containers.
*   **Simplicity:** Debugging and testing are trivial in a single process.

### Negative
*   **Team Scaling:** Beyond a certain size (e.g., 50 engineers), the monolith can cause merge conflicts (mitigated by strict modules).
*   **Homogeneous Technology:** The entire system must use the same main language/framework.

## Compliance
*   Any proposal to introduce a new independent service must come with an ROI analysis justifying why it cannot live inside the modular monolith.
