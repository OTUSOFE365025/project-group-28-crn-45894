# Iteration 1
## Step 1: Review Inputs
(Not needed for further Iterations).

## Step 2: Establish Iteration Goal by Selecting Drivers
The goal of this iteration is to identify structures to support primary functionality by decomposing the high-level layers from Iteration 1 into more concrete modules and components. This will help in:
- Understanding how core functionalities are implemented across layers
- Addressing QA-5 (Modifiability) by defining clear module boundaries and interfaces
- Supporting team allocation and task distribution

The selected drivers for this iteration are:
- UC-3 (Check Next Exam Date) – Represents the core student query functionality
- UC-4 (View Personalized Dashboard) – Requires integration of multiple data sources and personalization logic
- UC-5 (Proactive Assignment Notification) – Involves proactive data retrieval and notification
- QA-5 (Modifiability) – To ensure the system can evolve with new data sources and AI models

## Step 3: Choose One or More Elements of the System to Refine
The elements selected for refinement in this iteration are the Presentation Layer, the Application/Business Layer, and the Integration Layer. These layers are central to supporting the selected primary use cases because they directly encapsulate the components responsible for user interaction, conversational logic, and data retrieval from external university systems.

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers
| Design Decisions and Location | Rationale |
| ----------- | ----------- |
| Create a Domain Model for the AIDAP system | Before decomposing functionality, it is necessary to define key entities (e.g., User, Course, Exam, Assignment) and their relationships. This ensures a shared understanding of the problem space and supports consistent module design |
| Identify Domain Objects that map to functional requirements | Each major function should be encapsulated within a domain object. This supports modifiability (QA-5) and clear responsibility assignment |
| Decompose Domain Objects into layered components | This follows the Layered pattern and supports separation of concerns. It allows teams to work on different parts of the system independently |
| Use Spring Boot and Hibernate for business and data layers | Spring Boot supports rapid development and dependency injection, which aids testing and modifiability. Hibernate provides ORM capabilities and integrates well with Spring. The team is familiar with these technologies |


## Step 5: Instantatiate Architectural Elements, Allocate Repsonsibilites, & Define Interfaces
| Design Decisions and Location                                                                                            | Rationale                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Presentation Layer – `ChatController` and `DashboardController` (Spring MVC REST controllers)                            | `ChatController` exposes `/api/chat/query` for conversational queries like “When is my next exam?” and routes them to the application services. `DashboardController` exposes `/api/dashboard` for UC-4, returning a personalized summary of upcoming exams, assignments, and notifications based on the authenticated user. This keeps all HTTP / JSON concerns at the edge and supports RS1–RS3.                                                                                         |
| Presentation Layer – `NotificationSettingsController`                                                                    | Provides endpoints such as `/api/users/{id}/notification-preferences` for reading/updating notification channels and frequency (used by UC-5 and RS6). Concentrating settings in one controller keeps the UI changes isolated from business logic and improves modifiability (QA-5).                                                                                                                                                                                                       |
| Application Layer – `ExamService`, `DashboardService`, `NotificationService` (Spring `@Service` classes)                 | `ExamService` encapsulates logic to compute the next exam date for a student using `ExamRepository` + integration clients (UC-3). `DashboardService` aggregates data from multiple repositories and integration clients to build the personalized dashboard view (UC-4). `NotificationService` determines which assignments require proactive reminders and schedules or triggers them (UC-5). Grouping this logic in services allows changes without touching controllers or persistence. |
| Application Layer – `ConversationOrchestrator`                                                                           | Coordinates request flow: interprets the intent from the AI layer (e.g., “next exam date” vs “show dashboard”), then delegates to `ExamService`, `DashboardService`, or `NotificationService`. This centralizes conversational routing and prevents controllers from depending directly on AI or external systems, improving separation of concerns.                                                                                                                                       |
| Integration Layer – `LmsClient`, `RegistrationClient`, `CalendarClient` (Spring `@Component` / Feign/WebClient adapters) | Each client encapsulates REST/GraphQL calls to one external system (LMS, registration, calendar). They expose narrow interfaces like `getStudentExams(studentId)` or `getCourseAssignments(courseId)`. This isolates protocol/endpoint details from services and makes it easier to swap endpoints or add new data sources (QA-5, R3).                                                                                                                                                     |
| Integration Layer – `NotificationGateway`                                                                                | Abstracts outbound channels (email, push, SMS, in-app). Provides methods like `sendAssignmentReminder(user, assignment, channel)`. By routing all outbound notifications through this gateway, the system can add/remove channels without touching business logic, supporting UC-5 and modifiability.                                                                                                                                                                                      |
| Persistence Layer – `ExamRepository`, `AssignmentRepository`, `UserPreferencesRepository` (Spring Data JPA interfaces)   | Each repository manages one aggregate rooted in the domain model (Exam, Assignment, UserPreferences) using Hibernate. This keeps SQL/Hibernate configuration behind clear interfaces and allows the domain + services to evolve without leaking persistence concerns, aligning with QA-5.                                                                                                                                                                                                  |


## Step 6: Sketch Views and Record Design Decisions

### View 1 – Layered Logical Architecture

| Element                    | Responsibility                                             |
| -------------------------- | ---------------------------------------------------------- |
| ChatController     | Handles incoming conversational requests from users and manages the text/voice interface for the AI assistant. |
| DashboardController | Processes requests for personalized dashboard views and serves aggregated academic information to authenticated users. |
| NotificationSettingsController | Manages user preferences for notification delivery methods and frequency across different communication channels. |
| ConversationOrchestrator | Coordinates the flow of conversational requests through NLU processing, context management, and response generation. |
| ExamService | Contains business logic specific to exam-related queries, including date retrieval and exam information validation. |
| DashboardService | Aggregates data from multiple sources to compile personalized dashboard content for individual users. |
| NotificationService | Manages the scheduling and delivery of proactive notifications to users based on their preferences and academic events. |
| RegistrationClient | Handles API communication with the university registration system to retrieve course enrollment and student information. |
| LmsClient |	Interfaces with the Learning Management System to access course materials, assignments, and grade information. |
| CalendarClient | 	Connects to university calendar systems to retrieve academic events, deadlines, and schedule information. |
| NotificationGateway | Routes notifications to appropriate delivery channels (email, SMS, push) based on user preferences and system configuration. |
| ExamRepository | Manages persistence and retrieval of exam-related data, including dates, locations, and exam-specific metadata. |
| AssignmentRepository | Handles data access operations for assignment information, including due dates, requirements, and submission status. |
| UserPreferencesRepository | 	Stores and retrieves user-specific settings for notification preferences, language selection, and dashboard customization. |

<img width="841" height="394" alt="image" src="https://github.com/user-attachments/assets/6882038d-621f-4e7d-93ab-f340973f8fa8" />

### View 2 – Data & Integration Detail View

| Element | Responsibility |
|---------|----------------|
| ConversationOrchestrator | Coordinates the end-to-end flow of conversational requests, managing interactions between services and ensuring coherent response generation. |
| ExamService | Contains business logic for processing exam-related queries, including date lookups, conflict detection, and exam information validation. |
| ExamRepository | Handles data persistence and retrieval operations for exam entities, including scheduling information and exam metadata. |
| LmsClient | Manages API communication with the Learning Management System to access course content, assignments, and academic materials. |
| CalendarClient | Interfaces with university calendar systems to retrieve and synchronize academic events, schedules, and institutional deadlines. |
| NotificationService | Orchestrates the delivery of proactive notifications and reminders based on academic events and user subscription preferences. |
| AssignmentRepository | Manages data access operations for assignment-related information, including due dates, requirements, and submission tracking. |
| UserPreferencesRepository | Stores and retrieves user-specific configuration settings, including notification preferences and personalization options. |
| NotificationGateway | Routes outgoing notifications to appropriate delivery channels (email, SMS, push) based on system configuration and user choices. |


<img width="1080" height="390" alt="image" src="https://github.com/user-attachments/assets/398642fe-1785-48f1-bbe7-47853ff42adc" />

## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose
| Not Addressed | Partially Addressed | Comepletely Addressed | Explanation                                                                                                                                                                                                                                                                                                                                                 |
| :-----------: | :-----------------: | :-------------------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|               |                     |           UC-3           | UC-3 – Check Next Exam Date: We instantiated `ExamService`, `ExamRepository`, and integration clients so the system can compute the next exam date for a student via `ChatController`. Details of the AI NLU model and exact query patterns will be refined in later iterations.                                                                            |
|               |                     |           UC-4           | UC-4 – View Personalized Dashboard: `DashboardController`, `DashboardService`, and the domain repositories/clients provide a clear path to assemble dashboard data from multiple sources. Visual layout and advanced analytics widgets are left for future iterations.                                                                                      |
|               |          UC-5          |                       | UC-5 – Proactive Assignment Notification: `NotificationService`, `NotificationGateway`, and `UserPreferencesRepository` are defined, but scheduling policies (e.g., cron jobs, event triggers) and retry strategies are only conceptually identified, not fully designed.                                                                                   |
|               |                     |           QA-5           | QA-5 – Modifiability: Responsibilities are split across layers with clear service, repository, and client interfaces, using Spring Boot + Hibernate to support dependency injection and loose coupling. Further refinement (e.g., module boundaries in the codebase, separate Maven/Gradle modules) can be done in later iterations.                        |
|               |          Iteration Goal          |                       | Overall Iteration Goal: The iteration successfully decomposed high-level layers into concrete controllers, services, repositories, and integration clients for the selected use cases. Some cross-cutting concerns (security, monitoring, AI model lifecycle) are recognized but not yet designed, so additional iterations are needed to fully cover them. |


