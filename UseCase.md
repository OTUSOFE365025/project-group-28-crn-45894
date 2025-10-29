# Use Cases

| **Use Case ID** | **Title** | **Actor** | **Description** | **Preconditions** | **Basic Flow** | **Postconditions** |
|:--|:--|:--|:--|:--|:--|:--|
| UC1 | Check Personalized Dashboard | Student | Student views their upcoming events and academic performance. | Student is authenticated via SSO. | 1. Student opens AIDAP mobile app.<br>2. System authenticates via university SSO.<br>3. Student selects "My Dashboard".<br>4. System fetches calendar events, grades, announcements.<br>5. System displays personalized dashboard. | Student sees their upcoming deadlines and performance metrics. |
| UC2 | Post Automated Reminder | Lecturer | Lecturer schedules automated deadline reminders. | Lecturer is authenticated and has course access. | 1. Lecturer says "Remind CS101 students about assignment due Friday".<br>2. System confirms course authorization.<br>3. Lecturer specifies reminder details and timing.<br>4. System schedules automated notifications.<br>5. System sends reminders at specified time. | Students receive deadline reminders. |
