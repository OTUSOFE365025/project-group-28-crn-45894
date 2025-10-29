### Quality Attributes
| ID | Quality Attribute | Scenario | Associated Use Case |
|:-------|:-----|:-----|:-----|
| QA-1 | Usability | An administrator wants to assess the platform's adoption. They navigate to the analytics dashboard and successfully generate a usage report showing active users and popular query types for the past month. | UC-1 |
| QA-2 | Availability | During peak registration period, an administrator initiates a critical system update. The deployment process completes successfully, and throughout the entire procedure, the maximum 5,000 concurrent users remain able to submit queries and receive error-free responses within 2 seconds. | UC-2 |
| QA-3 | Performance | When a student requests information (ex. Next exam date) the system should respond quickly (<2 seconds). | UC-3 | 
| QA-4 | Security | When a student accesses their personalized dashboard, all of the inforamtion that is presented to them (ex. Upcoming events, Exams, Assignments) must be information that is owned by that student (A student can only see their own Upcoming events, and not other students'). | UC-4 |
| QA-5 | Modifiability | The university decides to switch to a new Learning Management System (LMS). A developer uses the system's predefined integration interface to write a new connector plugin. This change is completed and deployed into production within three person-days without requiring any modifications to the core AI processing or user interface components. | UC-6 |
