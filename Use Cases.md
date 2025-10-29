# Use Cases

## Use Case 1: Check Personalized Dashboard
**Actor:** Student  
**Description:** Student views their upcoming events and academic performance  
**Preconditions:** Student is authenticated via SSO  
**Basic Flow:**
1. Student opens AIDAP mobile app
2. System authenticates via university SSO
3. Student selects "My Dashboard"
4. System fetches calendar events, grades, announcements
5. System displays personalized dashboard
**Postconditions:** Student sees their upcoming deadlines and performance metrics

## Use Case 2: Post Automated Reminder
**Actor:** Lecturer  
**Description:** Lecturer schedules automated deadline reminders  
**Preconditions:** Lecturer is authenticated and has course access  
**Basic Flow:**
1. Lecturer says "Remind CS101 students about assignment due Friday"
2. System confirms course authorization
3. Lecturer specifies reminder details and timing
4. System schedules automated notifications
5. System sends reminders at specified time
**Postconditions:** Students receive deadline reminders
