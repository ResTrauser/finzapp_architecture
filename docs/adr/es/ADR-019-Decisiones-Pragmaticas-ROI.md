# ADR-019: Arquitectura Pragmática basada en Retorno de Inversión (ROI)

## Estado
Aceptado

## Contexto
En el ecosistema tecnológico actual, existe una fuerte presión por adoptar arquitecturas complejas (Microservicios, Kubernetes, Serverless) prematuramente.
Sin embargo, para una startup o un producto en crecimiento, el recurso más escaso es el tiempo de ingeniería.
Invertir meses en infraestructura distribuida antes de validar el modelo de negocio es un riesgo financiero alto (Costo de Oportunidad).

## Decisión
Priorizar un enfoque de **Monolito Modular (Vertical Slice)** sobre Microservicios Distribuidos para la etapa actual del negocio.

La decisión se basa en maximizar el ROI: obtener la mayor funcionalidad de negocio con la menor complejidad accidental posible.

## Análisis de Costo-Beneficio

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

### Tiempo al Mercado (Time to MVP)

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

## Consecuencias

### Positivas
*   **Velocidad:** El equipo se enfoca en resolver problemas de usuario, no en configurar redes mesh.
*   **Costos Operativos:** Una sola instancia (o cluster pequeño) es más barata que orquestar docenas de contenedores.
*   **Simplicidad:** Debugging y testing son triviales en un solo proceso.

### Negativas
*   **Escalado de Equipo:** A partir de cierto tamaño (e.g. 50 ingenieros), el monolito puede causar conflictos de merge (se mitiga con módulos estrictos).
*   **Tecnología Homogénea:** Todo el sistema debe usar el mismo lenguaje/framework principal.

## Cumplimiento
*   Cualquier propuesta de introducir un nuevo servicio independiente debe venir acompañada de un análisis de ROI justificando por qué no puede vivir dentro del monolito modular.
