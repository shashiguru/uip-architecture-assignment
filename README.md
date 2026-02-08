# Unified Integration Platform (UIP) on Azure

This repo captures a production-grade reference architecture for a Unified Integration Platform (UIP) on Microsoft Azure. It is designed to ingest data and events from 10-15 heterogeneous external systems, expose secure APIs to web/mobile clients, and deliver reliable, observable, and resilient integration workflows at scale.

The design emphasizes:
- Security-first edge and identity controls (Entra ID, WAF, APIM)
- Resilient microservices on AKS with clear data model boundaries
- Event-driven workflows for decoupling and scale (Service Bus + Event Hubs)
- OLTP/OLAP separation with daily KPI reporting
- Clear operational runbooks and failure handling (DLQ, replay, retries)

## Repository Contents
- `ASSIGNMENT.md`: **Start Here** - Contains the full problem statement, system capabilities, NFRs, and deliverable requirements for the Senior Developer Take-Home Assignment.
- `ARCHITECTURE_DEEP_DIVE.md`: **Technical Specification** - The comprehensive low-level design document. It includes:
  - Detailed swimlane diagrams for authentication, request processing, and saga patterns.
  - Deep dives into Frontend (MSAL/OIDC), Middleware (WAF/APIM), and Backend (AKS/Eventing) implementation.
  - Protocol specifics, resilience policies (Polly), and analytics pipelines.
- `README.md`: **Executive Summary** - High-level system overview, architecture principles, and quick reference.
- [Figma Architecture Design](https://www.figma.com/board/zi7c9qPP2Wsni7kXYxt8nF/UIP-Architecture-Design?node-id=0-1&p=f&t=04HKvLfQyQe0EL8p-0) 

