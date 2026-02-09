# UIP Architecture Deliverables

## 1. Architectural Diagram
![UIP Architecture Design](uip-architecture-design.jpg)

## 2. Architecture Design Explanation

### Overview
The Unified Integration Platform (UIP) is architected as a cloud-native solution on **Microsoft Azure**, utilizing a microservices architecture hosted on **Azure Kubernetes Service (AKS)**. This design provides the necessary agility, scalability, and isolation to serve both Web and Mobile frontends while robustly integrating with 10–15 diverse external enterprise systems.

### Key Design Decisions

1.  **Microservices on AKS**: We selected AKS as the core compute platform. This allows us to containerize individual integration adapters and business services, enabling granular scaling. For instance, the `UIP-Transformation-Service` can be scaled up independently during high-volume file ingestion without affecting the `UIP-CRM-Adapter`.
2.  **Gateway Offloading**: By placing **Azure Application Gateway** (WAF) and **Azure API Management (APIM)** in front of the cluster, we offload cross-cutting concerns like SSL termination, rate limiting, and authentication validation. This keeps the backend services focused on business logic.
3.  **Polyglot Persistence**: The architecture employs the "right store for the right job":
    *   **Azure SQL**: For relational, transactional business data requiring strict ACID compliance.
    *   **Cosmos DB**: For flexible, high-volume storage of processed integration data and audit logs.
    *   **Redis**: For high-speed caching to support low-latency dashboard reads.
    *   **Data Lake**: For cost-effective archival of raw events and files.

### Addressing Core Requirements

#### Third-Party System Integration & Resilience
Integrating with 10–15 external systems (SAP, Salesforce, MES, Partners) creates a high risk of cascading failures. We mitigate this through:
*   **Adapter Pattern**: Dedicated microservices (e.g., `UIP-ERP-Adapter`) encapsulate the specific logic, protocols (SOAP, REST, MQTT), and authentication for each external system.
*   **Resilience Policies**: A centralized `UIP-Resilience-Policy` mechanism enforces **Circuit Breakers** and **Retry with Exponential Backoff**. If "Partner-API-A" becomes unresponsive, its circuit breaker opens, failing fast locally rather than hanging threads and exhausting connection pools.
*   **Asynchronous Buffering**: **Azure Service Bus** acts as a shock absorber. Bursts of integration requests are queued and processed by consumers at a sustainable rate, protecting downstream legacy systems from being overwhelmed.

#### Scalability, Security, and Availability
*   **Scalability**: The architecture scales on multiple dimensions. **AKS** handles compute scaling via Horizontal Pod Autoscaling (HPA). **Event Hubs** partitions data streams to handle massive throughput for ingestion. **Cosmos DB** provides practically unlimited storage throughput.
*   **Security (Zero Trust)**:
    *   **Identity**: **Entra ID (Azure AD)** is the single source of truth, using OIDC/OAuth 2.0 for all user and service-to-service authentication.
    *   **Secrets**: No sensitive data is stored in code. **Azure Key Vault** manages connection strings and API keys, injected into pods at runtime.
    *   **Network**: **Azure VNet** isolates backend resources. Public access is strictly controlled via the Application Gateway, with WAF protecting against OWASP Top 10 vulnerabilities.
*   **Availability**: High availability is achieved by deploying AKS and critical data services (SQL, Cosmos) across **Azure Availability Zones**. Stateless services allow for instant recovery on healthy nodes if a failure occurs.

#### Real-Time vs. Daily Reporting
*   **Real-Time Dashboard**: We avoid database polling. The `UIP-WebPubSub` service maintains WebSocket connections to clients. Updates flow from the `UIP-Dashboard-Aggregator` (reading from Redis/Event Hubs) directly to the frontend, ensuring live visibility into integration status.
*   **Daily Reporting**: We employ a **CQRS-like pattern** where reporting is separated from operations. Operational data is replicated to **Azure Data Lake** and **Synapse Analytics**. Complex queries for daily summaries and KPIs run against this dedicated analytics layer, ensuring that heavy reporting jobs never degrade the performance of the live transactional system.

### Major Trade-offs Considered

**1. Microservices Complexity vs. Monolithic Simplicity**
*   *Decision*: We chose a distributed microservices architecture over a simpler monolithic App Service.
*   *Trade-off*: This significantly increases operational complexity (requires distributed tracing, container management, more complex CI/CD).
*   *Justification*: The requirement to integrate with 15+ external systems with different failure modes and scaling profiles made a monolith too risky. A memory leak in a legacy XML parser for one partner integration shouldn't crash the entire user dashboard. Isolation was prioritized over simplicity.

**2. Eventual Consistency for Dashboards**
*   *Decision*: The dashboard data flow relies on asynchronous events (Event Hubs -> Aggregator -> PubSub).
*   *Trade-off*: The dashboard may lag behind the source of truth by a few seconds (eventual consistency) rather than being instantly consistent.
*   *Justification*: Achieving strict strong consistency across distributed systems would require distributed transactions (2PC), which kill performance and availability. For an operational dashboard, "near real-time" is an acceptable trade-off for high system throughput and responsiveness.
