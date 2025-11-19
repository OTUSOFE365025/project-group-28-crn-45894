# Iteration 1
## Step 1: Review Inputs
| Category      | Details |
| ----------- | ----------- |
| Design Purpose | 	To establish the foundational structure for a cloud-native, AI-powered conversational platform |
| Primary Functional Requirements | UC-3 (Check Next Exam Date): Represents the core student query function. UC-6 (Onboard a New Data Source): Critical for long-term system evolvability |
| Quality Attrbiutes | QA-4 (Security), QA-5 (Modifiability) |

## Step 2: Establish Iteration Goal by Selecting Drivers
This is the first iteration in the design of a greenfield system. The goal is to establish the overall, high-level structure of the system, creating a foundation that addresses the core functional need of a conversational AI agent while being fundamentally scalable, secure, and integrable.

The selected drivers for this iteration are:
- UC-3 (Check Next Exam Date): This use case defines the primary system purpose: answering user questions by pulling data from integrated sources
- QA-5 (Modifiability): This quality is crucial for the system's longevity, ensuring it can adapt to new and changing university data sources (as seen in UC-6)
- CON-1 (Use Institution's SSO): This is a critical constraint that fundamentally shapes the security and user authentication model, requiring a dedicated authentication component
- CON-2 (Use Standard APIs): This constraint directly shapes the integration strategy and the interfaces between the system's components and external data sources, supporting QA-5

## Step 3: Choose One or More Elements of the System to Refine
We want to refine the entire AI-Powered Digital Assistant Platform (AIDAP) system to define its initial, high-level architectural structure.

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers
| Design Decisions and Location | Rationale |
| ----------- | ----------- |
| Logically structure the system using a Layered (N-Tier) Cloud-Native Architecture with a clear separation between Presentation, Application/Business, and Data layers | This provides a strong foundational structure that supports modifiability (QA-5) by isolating changes. The Presentation Layer can be developed independently for web, mobile, and voice (aligned with CON-4), while the business logic for use cases like UC-3 is centralized. It is the standard pattern for building scalable, cloud-native applications (providing a foundation for CON-3) |
| Implement an API Gateway as the single entry point for all client requests | The API Gateway is crucial for handling diverse clients (web, mobile, voice - CON-4) from a single backend. It provides a central point to implement security policies, including routing authentication requests to the SSO provider (CON-1). It also offloads common concerns like rate limiting and routing, contributing to availability (CON-3) |
| Do not implement a custom user authentication system. Instead, integrate with the external SSO provider via a standard protocol (e.g., OAuth 2.0 / OpenID Connect) | This directly satisfies constraint CON-1. It centralizes security logic, improves security (QA-4) by leveraging a proven system, and simplifies the architecture by removing the responsibility for credential management |
| Access all external university data sources (LMS, Calendar, etc.) through a dedicated Integration Layer that uses REST/GraphQL clients | This directly enforces CON-2 and is the primary mechanism to achieve modifiability (QA-5). By encapsulating all external communication into a dedicated layer, changes or additions to data sources (as in UC-6) are isolated, preventing ripple effects on the core business logic |

## Step 5: Instantatiate Architectural Elements, Allocate Repsonsibilites, & Define Interfaces
| Design Decisions and Location | Rationale |
| ----------- | ----------- |
| Create an Authentication and Session management service that integrates behind the API Gateway dexcribed above through the university's SSO. | Our system will not store passwords, it will just handle redirects into the universities' SSO, allowing existing university systems to handle authorization. This will satisfy requirements: (RS7, RS8, RA5)|
| Introduce a Persistent Storage Component meant to store internal system daata (ex. AI Conversation History, Settings, cached data) | Creating and implementing this component will seperate internal system data related to our application from university data. That will simplify including handling university privacy policies (RA5). It'll store historical interactions (R2), it'll allow easier interactivity with dashboards (RS3), and it'll allow the chatbot to learn from prior conversations (RS5) |

## Step 6: Sketch Views and Record Design Decisions

### View 1 – Layered Logical Architecture

| Element                   | Responsibility                                                               |
| ------------------------- | ---------------------------------------------------------------------------- |
| Chat UIs                  | Provide text and voice conversational interfaces for all stakeholders.       |
| Conversation Orchestrator | Routes each user request through NLU, session, personalization, and data.    |
| NLU/NLG Engine            | Uses AI models to interpret natural-language queries and generate responses. |
| Session Manager           | Tracks ongoing conversations and context per user.                           |
| Integration Gateway       | Single entry point to LMS, Registration, Calendar and Mail APIs.             |
| User Profile & History DB | Stores conversations and preferences for personalization.                    |
| Analytics & Dashboards    | Computes academic analytics and summary dashboards.                          |


<img width="1218" height="455" alt="AIDAP Diagram" src="https://github.com/user-attachments/assets/aa494d28-7c21-4909-9ebe-e26ff9349835" />

### View 2 – Data & Integration Detail View

| Element                    | Responsibility                                             |
| -------------------------- | ---------------------------------------------------------- |
| Interaction History DB     | Persists all past conversations and query logs.            |
| Config & Policy DB         | Stores security, retention, and personalization policies.  |
| Notification Service       | Sends deadline reminders and announcements to users.       |
| Integration Gateway        | Coordinates connectors to each external university system. |
| LMS / Reg / Calendar Conn. | Adapt each external API to the internal AIDAP model.       |

<img width="997" height="383" alt="Data   Integration View" src="https://github.com/user-attachments/assets/e4128760-dcd9-4637-90bc-3ccecf9035f6" />

### View 3 – Deployment View

| Element              | Responsibility                                                        |
| -------------------- | --------------------------------------------------------------------- |
| User Devices         | Browsers/mobile/voice clients used by students, lecturers, admins.    |
| API Gateway          | Single public endpoint; handles auth (SSO), routing, throttling.      |
| Conversation Service | Hosts orchestrator, NLU/NLG, session management.                      |
| Data & Integration   | Hosts notification service, connectors, and manages persistence.      |
| AIDAP Cluster        | Cloud deployment unit providing scalability and high availability.    |
| University Systems   | Existing LMS, Registration, Calendar/Mail services consumed via APIs. |


<img width="997" height="383" alt="Data   Integration View" src="https://github.com/user-attachments/assets/32c5a3dd-5dde-4892-a1e1-0d2591b858da" />

## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

| Not Addressed | Partially Addressed | Comepletely Addressed | Explanation |
|:---------------:|:---------------------:|:-----------------------:|-------------|
|  | UC-3 |  |  |
|  |  | CON-1 | We fully use hte universities' SSO for authentication, which satisfies teh constraint completely |
|  |  | CON-2 | Using a dedicated Integration Layer to communicate with university systems will fully satisfy this constraint |
|  | QA-4 |  | High level secuirty is tackled through SSO and the Authentication service, but low level details are still missing |
|  | QA-5 |  | Modifiability is mostly supported by our layered architecture adn connectors, but ways to extend arent yet defined |


