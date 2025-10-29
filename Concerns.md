### Concerns
| ID | Concern |
|:-----|:-----|
| CRN-1 | The architecture must strictly enforce data segregation and role-based access control to ensure that sensitive user data (e.g., student grades, performance) is only accessible to the authenticated user and authorized roles (e.g., their lecturers). |
| CRN-2 | The architecture must be designed to gracefully handle the frequent unavailability or slowness of external data source systems without becoming unresponsive, while also managing the state of data synchronization to prevent inconsistencies. |
| CRN-3 | The system relies on real time data from multiple sources such as LMS, registration, and more. Anything from downtime to integration failures would result in the functionality of the system being lost, and it cold lead to users recieving incorrect or even misleading infromation. |
