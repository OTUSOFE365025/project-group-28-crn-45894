# **ATAM Analysis for AIDAP**

### Selected Scenario (Use Case / Growth Scenario):
**UC-2: Deploy a Zero-Downtime Update**  
*During peak registration, a system maintainer deploys a new AI model. The system must continue serving 5,000 concurrent users with <2-second response times throughout the update.*

**Relevant Quality Attributes:**  
- **Availability** (QA-2)  
- **Performance** (QA-3)  
- **Modifiability** (QA-5)

---

## **Detailed Sensitivity, Tradeoff, Risk, and Nonrisk**

### Sensitivities
- S1: The **switching time between blue and green environments** is sensitive to network latency and health check intervals. If the switchover takes >5 seconds, user sessions may drop
- S2: The Database Connection Pool size is sensitive to the number of active replicas during deployment. During the Blue-Green overlap, the number of application instances doubles (Blue + Green), which could exhaust the maximum allowed database connections, causing queries to fail.
- S3: Message queue throughput is sensitive to burst traffic during deployment. If the queue becomes saturated, notification delays may affect user experience

### Trade-Offs
- T1: **Blue-Green Deployment improves Availability (+) but increases Infrastructure Cost (-).** Maintaining two identical environments doubles cloud resource usage during updates
- T2: Asynchronous Message Queuing improves Performance (Response Latency) (+) by offloading heavy tasks (like notifications) from the main thread, but it negatively impacts Data Consistency/Timeliness (-). Users receive immediate UI confirmation, but the actual action (e.g., email receipt) happens with eventual consistency.
- T3: Message Queue improves Performance (+) and Modifiability (+) but introduces Latency (-) for asynchronous tasks, which may affect real-time notification delivery

### Risks
- R1: **Session state loss during blue-green switchover.** If user session data is not replicated between environments, users may lose context or be logged out
- R2: Database Schema Incompatibility. If the new "Green" version requires a database schema change (e.g., renaming a column) that is not backward compatible, the "Blue" version (which is still live during the transition) may start failing, causing system-wide errors.
- R3: Message queue single point of failure. If the queue service fails, asynchronous operations (notifications, data sync) will halt, affecting system functionality

### Non-Risks
- N1: **API Gateway maintains routing consistency during cutover.** The gateway continues to route traffic to the active environment without disruption
- N2: Single Container/Pod Failure. Because Active Redundancy is implemented with Kubernetes replicas, the failure of a single ConversationService node is a non-risk. The Load Balancer will automatically detect the failure and route traffic to the remaining healthy replicas without service interruption.
- N3: Integration Layer abstraction allows AI model updates without modifying external system connectors, supporting modifiability

---

### Architectural Decisions
| ID | Architectural Decision | Description |
|----|------------------------|-------------|
| **AD1** | **Layered (3-Tier) Cloud-Native Architecture** | Separation into Presentation, Business, and Data/Integration layers. |
| **AD2** | **API Gateway as Single Entry Point** | Centralized routing, authentication (SSO), rate limiting, and load distribution. |
| **AD3** | **Blue-Green Deployment Strategy** | Two identical environments (blue/green) for zero-downtime updates. |
| **AD4** | **Kubernetes-based Service Replication** | Critical services (Conversation Service, Data Service) run with multiple replicas managed by Kubernetes. |
| **AD5** | **Message Queue for Asynchronous Processing** | Notifications and data sync tasks are decoupled via a message queue (e.g., RabbitMQ). |
| **AD6** | **Dedicated Integration Layer with REST/GraphQL Clients** | Encapsulates all external system communications behind standardized interfaces. |

### **Analysis Table for Scenario UC-2**

| Analyzing Scenario | UC-2 (Deploy Zero-Downtime Update) |
|-------------------|--------------------------------------|
| **Attributes**    | Availability, Performance, Modifiability |
| **Stimulus**      | System maintainer initiates deployment of new AI model during peak load. |
| **Environment**   | System under peak load (5,000 concurrent users). |
| **Response**      | Update completes successfully with no downtime and maintained <2-second response time. |

| Architecture Decision | Sensitivity | Tradeoff | Risk | Nonrisk |
|-----------------------|:-----------:|:--------:|:----:|:-------:|
| **AD1: Layered Architecture** | | | | |
| **AD2: API Gateway** | | | | N1 |
| **AD3: Blue-Green Deployment** | S1, S2 | T1 | R1, R2 | |
| **AD4: Kubernetes Replication** | S2 | | | N2 |
| **AD5: Message Queue** | S3 | T2, T3 | R3 | |
| **AD6: Dedicated Integration Layer** | | | | N3 |
