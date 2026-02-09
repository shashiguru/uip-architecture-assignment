# Unified Integration Platform (UIP) on Azure

This repo captures a production-grade reference architecture for a Unified Integration Platform (UIP) on Microsoft Azure. It is designed to ingest data and events from 10-15 heterogeneous external systems, expose secure APIs to web/mobile clients, and deliver reliable, observable, and resilient integration workflows at scale.

The design emphasizes:
- **Security-first**: Edge and identity controls (Entra ID, WAF, APIM) with Zero Trust principles.
- **Resilience**: Microservices on AKS with Circuit Breakers, Retries, and asynchronous decoupling.
- **Event-Driven**: High-scale workflows using Service Bus & Event Hubs with real-time feedback (WebPubSub).
- **Polyglot Persistence**: Strategic use of Azure SQL (Transactional State), Cosmos DB (Documents), and Data Lake (Archives).
- **Observability**: End-to-end tracing and separation of operational (OLTP) vs. analytical (OLAP) reporting.

## Repository Contents

### 1. Core Deliverables
- **`DELIVERABLES.md`**: **The Submission Document** - A concise 2-page summary explaining the architectural decisions, trade-offs, and how the design addresses the core assignment requirements (Resilience, Scalability, Reporting).
- **`uip-architecture-design.jpg`**: **Visual Diagram** - The high-level architectural view showing the 7 core layers and their interactions.

### 2. Documentation
- **`ASSIGNMENT.md`**: **Requirements** - The original problem statement, system capabilities, and NFRs.
- **`ARCHITECTURE_DEEP_DIVE.md`**: **Technical Specification** - A comprehensive low-level design document (LLD). It includes:
  - **Swimlane Diagrams**: Visualizing Authentication (OIDC), Middleware logic, and Analytics pipelines.
  - **End-to-End Flows**: Detailed consolidated flows for Synchronous (Read), Asynchronous (Write), and Real-Time (WebPubSub) scenarios.
  - **Implementation Details**: Deep dives into Frontend (MSAL), AKS Platform Engineering, and Resilience Policies (Polly).

- [Figma Architecture Design](https://www.figma.com/board/zi7c9qPP2Wsni7kXYxt8nF/UIP-Architecture-Design?node-id=0-1&p=f&t=04HKvLfQyQe0EL8p-0)
