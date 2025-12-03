# Iteration 3
## Step 1: Review Inputs
(Not needed for further Iterations).

## Step 2: Establish Iteration Goal by Selecting Drivers
Building on the fundamental structural decisions from Iterations 1 and 2, this iteration focuses on the fulfillment of a critical quality attribute related to system reliability and continuity.

Selected Driver:
- QA-2 (Availability): During peak registration period, an administrator initiates a critical system update. The deployment process completes successfully, and throughout the entire procedure, the maximum 5,000 concurrent users remain able to submit queries and receive error-free responses within 2 seconds.

## Step 3: Choose One or More Elements of the System to Refine
For this availability and deployment scenario, the elements to be refined are the physical deployment units identified in the initial deployment view:

- API Gateway (public entry point)
- Conversation Service Cluster (hosts orchestrator, NLU/NLG, session management)
- Data & Integration Service Cluster (hosts notification service, connectors, and repositories)

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers
| Design Decisions and Location | Rationale |
| ----------- | ----------- |
| Introduce the Active Redundancy tactic by replicating critical services (Conversation Service, Data & Integration Service) across multiple nodes | Replication allows the system to withstand the failure of individual nodes without affecting functionality, directly supporting high availability (CON-3, RS11) |
| Introduce a Load Balancer in front of service clusters | Distributing traffic across replicas prevents overloading a single instance and ensures even load distribution during peak usage, supporting the 5,000 concurrent user requirement (RA7) |
| Use a Blue-Green Deployment strategy for the Conversation Service | This enables zero-downtime updates (RM1) by routing traffic between two identical environments (blue and green), allowing one to be updated while the other serves live traffic. This directly addresses QA-2 and UC-2 |
| Introduce a Message Queue for asynchronous processing of notifications and data synchronization tasks | Decouples time-consuming tasks (e.g., sending batch notifications, syncing LMS data) from the main request/response flow. This improves responsiveness (RS10) and ensures system stability during updates |

## Step 5: Instantatiate Architectural Elements, Allocate Repsonsibilites, & Define Interfaces
| Design Decisions and Location | Rationale |
| ------------------------------ | ------------------------------ |
| Deploy a Load Balancer (e.g., AWS ALB, NGINX) in front of the API Gateway and service clusters | The Load Balancer distributes incoming user requests across healthy instances of the API Gateway and backend services. It performs health checks and reroutes traffic away from failed nodes, ensuring continuous availability during updates or failures (supports QA-2, CON-3) |
| Configure the Conversation Service and Data & Integration Service to run in a Kubernetes Deployment with at least 3 replicas per service | Kubernetes manages pod lifecycle, automatically restarts failed containers, and scales replicas based on load. This provides active redundancy and supports rolling updates without downtime (aligns with RM1, QA-2) |
| Implement Blue-Green Deployment using Kubernetes Services and Ingress routing | Two identical environments (blue and green) are maintained. The Ingress controller routes traffic to the active (green) environment. During an update, the new version is deployed to the inactive (blue) environment, tested, and then traffic is switched. This ensures zero downtime (UC-2, RM1) |
| Introduce a Message Queue (e.g., Apache Kafka, RabbitMQ) for asynchronous tasks such as sending notifications and synchronizing external data | The NotificationService publishes events to the queue instead of processing them synchronously. Separate worker services consume these events. This decouples time-intensive operations from the main request flow, maintaining response times under 2 seconds during peak load (RS10, QA-2) |
| Create a DeploymentCoordinator service (part of the maintainer’s toolset) to automate the Blue-Green switchover and post-deployment validation | This service manages the deployment pipeline, triggers traffic switching, and runs health checks to ensure the new version is stable before fully cutting over. It supports RM1 and reduces human error during updates |

## Step 6: Sketch Views and Record Design Decisions

### View 1 - Deployment View

| Element              | Responsibility |
|----------------------|----------------|
| LoadBalancer         | Acts as the single entry point for traffic. It performs health checks and distributes incoming requests across the active APIGateway replicas. If a node fails, it reroutes traffic to healthy nodes. |
| MessageQueue         | A persistent buffer (e.g., Kafka/RabbitMQ) that decouples the ConversationService from the DataIntegrationService. It ensures that tasks (like notifications) are not lost if the processing service is temporarily down during an update. |
| DeploymentCoordinator | An external tool/service that manages the Blue-Green deployment pipeline. It orchestrates the creation of new replica sets and updates the Ingress/LoadBalancer rules. |

<img width="837" height="1028" alt="DeploymentDiagram" src="https://github.com/user-attachments/assets/45ce6a6e-8601-4a7c-9f11-2ac50223d00c" />


### View 2 – Sequence Diagram

| Element                | Responsibility |
|------------------------|----------------|
| Ingress/LoadBalancer   | Receives the initial HTTP request and routes it to the least busy ConversationService instance. |
| ConversationService    | Validates the request and immediately acknowledges receipt to the user (ensuring the <2s response time). It then offloads the heavy processing by publishing an event to the MessageQueue. |
| DataIntegrationService | Asynchronously consumes the event from the queue to process the logic (e.g., sending emails or updating the LMS) without blocking the user's interface. |

<img width="1071" height="540" alt="SequenceDiagram" src="https://github.com/user-attachments/assets/d614a4e7-08c3-440e-980c-4e3ae1fb5c8f" />


## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose
| Not Addressed | Partially Addressed | Comepletely Addressed | Explanation |
| :-----------: | :-----------------: | :-------------------: | ------------ |

