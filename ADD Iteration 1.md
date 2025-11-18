# Iteration 1
## Step 1
| Category      | Details |
| ----------- | ----------- |
| Design Purpose | 	To establish the foundational structure for a cloud-native, AI-powered conversational platform |
| Primary Functional Requirements | UC-3 (Check Next Exam Date): Represents the core student query function. UC-6 (Onboard a New Data Source): Critical for long-term system evolvability |
| Quality Attrbiutes | QA-4 (Security), QA-5 (Modifiability) |

## Step 2
This is the first iteration in the design of a greenfield system. The goal is to establish the overall, high-level structure of the system, creating a foundation that addresses the core functional need of a conversational AI agent while being fundamentally scalable, secure, and integrable.

The selected drivers for this iteration are:
- UC-3 (Check Next Exam Date): This use case defines the primary system purpose: answering user questions by pulling data from integrated sources
- QA-5 (Modifiability): This quality is crucial for the system's longevity, ensuring it can adapt to new and changing university data sources (as seen in UC-6)
- CON-1 (Use Institution's SSO): This is a critical constraint that fundamentally shapes the security and user authentication model, requiring a dedicated authentication component
- CON-2 (Use Standard APIs): This constraint directly shapes the integration strategy and the interfaces between the system's components and external data sources, supporting QA-5

## Step 3
We want to refine the entire AI-Powered Digital Assistant Platform (AIDAP) system to define its initial, high-level architectural structure.

## Step 4
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

## Step 6


## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

| Not Addressed | Partially Addressed | Comepletely Addressed | Explanation |
|---------------|---------------------|-----------------------|-------------|
|  | UC-3 |  |  |
|  |  | CON-1 | We fully use hte universities' SSO for authentication, which satisfies teh constraint completely |
|  |  | CON-2 | Using a dedicated Integration Layer to communicate with university systems will fully satisfy this constraint |
|  | QA-4 |  | High level secuirty is tackled through SSO and the Authentication service, but low level details are still missing |
|  | QA-5 |  | Modifiability is mostly supported by our layered architecture adn connectors, but ways to extend arent yet defined |


