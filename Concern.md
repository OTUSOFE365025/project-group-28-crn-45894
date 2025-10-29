# Architectural Concerns

| **ID** | **Concern** | **Description** | **Stakeholders** | **Impact** |
|:--|:--|:--|:--|:--|
| CN-1 | Data Consistency Across Integrations | Multiple data sources (LMS, calendars, registration) update at different times, risking inconsistencies. | System Maintainers, Administrators | Requires a synchronization strategy and conflict-resolution logic. |
| CN-2 | AI Model Transparency | Users may question how the AI produces study plans or flags engagement levels. | Students, Lecturers | Requires explainable-AI components and clear justification in responses. |
| CN-3 | System Scalability During Exam Seasons | Load spikes during exams can cause performance degradation. | Students, IT Operations | Demands elastic resource allocation and robust load balancing. |
