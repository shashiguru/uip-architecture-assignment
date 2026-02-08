# Senior Developer Take-Home Assignment: Unified Integration Platform (UIP) Architecture

## Objective
As part of this assignment, you are required to design the architecture for a Unified Integration Platform (UIP) that serves both web and mobile front-ends and is deployed on Microsoft Azure. The goal is to demonstrate your ability to architect a cloud-native enterprise integration platform that connects core services with multiple external systems, while considering both functional and non-functional requirements.

## System Capabilities
Your design should account for the following core functionalities of the UIP:

1.  **Identity and Access Management (IAM) / Single Sign-On (SSO)**
    Users should authenticate through SSO mechanisms (e.g., OAuth2/OIDC with an enterprise identity provider). Access must be controlled through roles and permissions.

2.  **API Gateway and Client Access Layer**
    All web and mobile clients should access UIP through a secure API gateway capable of routing, throttling, validation, and request enrichment.
    You may optionally introduce a Backend-for-Frontend (BFF) layer—if you do, justify its purpose and placement.

3.  **Real-Time / Near Real-Time Operational Dashboard**
    UIP must provide a dashboard that aggregates data from internal services and 10–15 external systems (e.g., ERP, MES, CRM, LIMS, partner APIs).
    Focus less on UI rendering and more on data flow, streaming vs polling, transformation, and aggregation strategy.

4.  **File and Data Ingestion**
    Users should be able to upload structured or semi-structured files (e.g., CSV, JSON, Excel). Ingestion must trigger validation, transformation, and asynchronous processing pipelines with clear failure-handling.

5.  **Event-Driven Processing**
    UIP should publish and consume domain events to support decoupled, asynchronous workflows across internal services and integration components.

6.  **Third-Party System Integration**
    The platform will integrate with 10–15 external systems or APIs with varying SLAs, authentication methods, and reliability levels.
    UIP should isolate failures, implement retry/backoff strategies, and degrade gracefully when dependencies are slow or unavailable.

7.  **Daily Summary and Reporting**
    UIP should support daily summary and reporting workloads, such as integration counts, failures, and per-system KPIs.
    Focus on data storage strategy, query efficiency, and separation of transactional vs reporting data.

## Non-Functional Requirements (NFRs)

1.  **High Availability and Resilience**
    Design UIP to remain available despite failures of components, nodes, or external systems. Include failover and graceful degradation strategies.

2.  **Scalability**
    The architecture should scale with increases in users, tenants, API traffic, and integration workloads, using Azure as the cloud platform.

3.  **Quality of Service (QoS)**
    Show how UIP manages external systems with varying performance, rate limits, and reliability, and how you isolate unstable integrations.

4.  **Data Management and Recovery**
    Include backup, restore, disaster recovery, idempotency, historical data retention, and consistency considerations.

5.  **Cybersecurity and Defense in Depth**
    Address authentication, authorization, secrets management, input validation, network segmentation, least privilege, and auditability.

6.  **Observability**
    Your design should enable effective logging, metrics, tracing, and flow monitoring across services and integrations.

7.  **Multi-Tenancy and Isolation**
    If applicable, describe how tenant data and workloads are isolated and how per-tenant limits or throttles could be enforced.

## Your Task
1.  Produce an architectural design or diagram that illustrates how UIP operates on Microsoft Azure.
2.  Your design should show how the core services interact, how the platform integrates with 10–15 external APIs in a scalable and resilient manner, how events and data flows support real-time dashboards and daily reporting, and how Azure serves as the cloud foundation without referencing specific Azure services.
3.  You are not expected to provide a fully detailed end-to-end solution. We are evaluating the clarity of your architecture, your reasoning, and the trade-offs you make.

## Deliverables
1.  **Architectural Diagram**
    A clear diagram showing the main components of UIP and how they interact, including client access, core services, integration layers, event processing, data flows, and cross-cutting concerns. It should be evident that the platform is intended for deployment on Azure.

2.  **Brief Explanation (1–2 pages)**
    A concise summary of your key design decisions. This should address:
    *   How third-party API integration is handled (including reliability and failure scenarios)
    *   How scalability, security, and availability are achieved
    *   How real-time/high-volume data flows and daily reporting are supported
    *   One or two major trade-offs you considered

## Submission
Please send your deliverables to your HR contact **12 hours before your scheduled interview**. Be prepared to present and explain your approach to the panel during the interview.
