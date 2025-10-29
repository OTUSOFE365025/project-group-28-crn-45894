# Architectural Concerns

| **ID** | **Concern** | **Description** |
|:--|:--|:--|
| CN-1 | Privacy and Data Segregation | The architecture must ensure personal data from different users and departments remain isolated, even during shared query processing. |
| CN-2 | Resilience to External Failures | The system must remain stable and responsive even when an external data source (like the LMS or calendar API) experiences downtime. |
