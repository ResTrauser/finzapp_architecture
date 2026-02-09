# FinzApp API - Architecture Documentation

Welcome to the architectural documentation repository for `finzapp_api`. This repository contains the **Architecture Decision Records (ADRs)** that define the design, structure, and technical principles of our financial platform.

The goal of this documentation is to provide a transparent, professional, and deep insight into the decisions that shape our system, serving as a guide for new engineers, stakeholders, and anyone interested in modern backend architecture.

## 📚 Documentation Index

The documentation is available in both English and Spanish.

| ID | Title (English) | Título (Español) | Topic |
|:---|:---|:---|:---|
| 001 | [Distributed Monolith](docs/adr/en/ADR-001-Distributed-Monolith.md) | [Monolito Distribuido](docs/adr/es/ADR-001-Monolito-Distribuido.md) | Architecture Style |
| 002 | [Vertical Slice Architecture](docs/adr/en/ADR-002-Vertical-Slice-Architecture.md) | [Vertical Slice Architecture](docs/adr/es/ADR-002-Vertical-Slice-Architecture.md) | Code Organization |
| 003 | [Observer Pattern (Event Bus)](docs/adr/en/ADR-003-Observer-Pattern-Decoupling.md) | [Patrón Observer](docs/adr/es/ADR-003-Patron-Observer-Desacoplamiento.md) | Decoupling |
| 004 | [Strategy Pattern (MFA)](docs/adr/en/ADR-004-Strategy-Pattern-MFA.md) | [Patrón Strategy (MFA)](docs/adr/es/ADR-004-Patron-Strategy-MFA.md) | Authentication |
| 005 | [Chain of Responsibility](docs/adr/en/ADR-005-Chain-of-Responsibility-Validations.md) | [Chain of Responsibility](docs/adr/es/ADR-005-Chain-of-Responsibility-Validations.md) | Validations |
| 006 | [Unit of Work](docs/adr/en/ADR-006-Unit-of-Work-Consistency.md) | [Unit of Work](docs/adr/es/ADR-006-Unit-of-Work-Consistencia.md) | Data Consistency |
| 007 | [Redis Caching](docs/adr/en/ADR-007-Redis-Caching-Strategy.md) | [Caching con Redis](docs/adr/es/ADR-007-Redis-Caching-Strategy.md) | Performance |
| 008 | [Backend Design Systems](docs/adr/en/ADR-008-Design-Systems-Backend.md) | [Backend Design Systems](docs/adr/es/ADR-008-Design-Systems-Backend.md) | API Standards |
| 009 | [Multi-Tenancy Context](docs/adr/en/ADR-009-Multi-Tenancy-Context.md) | [Contexto Multi-Tenant](docs/adr/es/ADR-009-Multi-Tenancy-Context.md) | SaaS Architecture |
| 010 | [Background Tasks (Celery)](docs/adr/en/ADR-010-Background-Tasks-Celery.md) | [Tareas en Segundo Plano](docs/adr/es/ADR-010-Background-Tasks-Celery.md) | Async Processing |
| 011 | [Rate Limiting](docs/adr/en/ADR-011-Rate-Limiting-Protection.md) | [Rate Limiting](docs/adr/es/ADR-011-Rate-Limiting-Protection.md) | Security |
| 012 | [Testing Strategy](docs/adr/en/ADR-012-Testing-Strategy-Senior.md) | [Estrategia de Testing](docs/adr/es/ADR-012-Testing-Strategy-Senior.md) | QA |
| 013 | [Monitoring & Logging](docs/adr/en/ADR-013-Monitoring-and-Logging.md) | [Monitoreo y Logging](docs/adr/es/ADR-013-Monitoring-and-Logging.md) | Observability |
| 014 | [DB Migrations](docs/adr/en/ADR-014-DB-Migrations-Safety.md) | [Migraciones de BD](docs/adr/es/ADR-014-DB-Migrations-Safety.md) | Operations |
| 015 | [Architecture for Agility](docs/adr/en/ADR-015-Architecture-Business-Agility.md) | [Arquitectura para Agilidad](docs/adr/es/ADR-015-Arquitectura-Agilidad-Negocio.md) | Business Alignment |
| 016 | [Data Integrity (Ledger)](docs/adr/en/ADR-016-Business-Assets-Integrity.md) | [Integridad de Activos](docs/adr/es/ADR-016-Integridad-Activos-Negocio.md) | Financial Reliability |
| 017 | [Performance & Retention](docs/adr/en/ADR-017-Performance-User-Retention.md) | [Performance y Retención](docs/adr/es/ADR-017-Performance-Retencion-Usuarios.md) | Product KPI |
| 018 | [Security (Defense in Depth)](docs/adr/en/ADR-018-Security-Customer-Trust.md) | [Seguridad y Confianza](docs/adr/es/ADR-018-Seguridad-Confianza-Cliente.md) | Security Strategy |
| 019 | [Pragmatic ROI](docs/adr/en/ADR-019-Pragmatic-Decisions-ROI.md) | [Decisiones Pragmáticas](docs/adr/es/ADR-019-Decisiones-Pragmaticas-ROI.md) | Decision Making |
| 020 | [Backend as a Product](docs/adr/en/ADR-020-Backend-As-Product.md) | [Backend como Producto](docs/adr/es/ADR-020-Backend-Como-Producto.md) | Engineering Culture |

## 🏗️ Core Principles

1.  **Vertical Slice Architecture:** We group code by feature, not technical layer.
2.  **Modular Monolith:** We deploy a single unit but maintain strict logical boundaries.
3.  **Product Mindset:** Every line of code must serve a business purpose (ROI, Retention, Trust).

---
*Built with ❤️ by the FinzApp Engineering Team.*
