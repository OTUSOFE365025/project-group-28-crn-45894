# Quality Attributes

| **Attribute ID** | **Attribute** | **Scenario** | **Stimulus** | **Source** | **Environment** | **Artifact** | **Response** | **Response Measure** |
|:--|:--|:--|:--|:--|:--|:--|:--|:--|
| QA1 | Availability | System remains accessible during peak usage. | 5,000 concurrent users during registration period. | Multiple student and lecturer sessions. | Normal system operation under peak load conditions. | Entire AIDAP platform and load balancers. | System remains responsive, automatic failover if needed. | 99.5 % uptime monthly; recovery < 30 seconds. |
| QA2 | Usability | Intuitive conversational interface for new users. | New student uses AIDAP for the first time. | Student user interface (mobile / web / voice). | First-time user context, no prior training. | Conversation engine and UI components. | System provides clear prompts and helpful responses. | 90 % of users complete tasks without help; satisfaction ≥ 4 / 5. |
