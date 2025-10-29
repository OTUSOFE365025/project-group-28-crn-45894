### Business Case
The AI-Powered Digital Assistant Platform (AIDAP) is proposed to enhance the university experience by providing a centralized, conversational interface for students, faculty, and staff to access institutional data and services. By integrating with existing systems like the LMS and calendar, AIDAP will use AI to deliver immediate, personalized answers on schedules, deadlines, and analytics, thereby reducing administrative burden, improving student engagement, and ensuring scalable, secure, and always-available access to critical campus information.

### Use Cases
| Use Case | Description |
|:-------|:--------|
| UC-1: Broadcast Campus-Wide Announcement | An administrator needs to quickly inform all students and faculty about an important event such as a school closure. They use the AIDAP administrative interface to compose and send a broadcast announcement. The system _immediately_ delivers this announcement to all users via their preferred conversational interface. (RA3, RA4) |
| UC-2: Deploy a Zero-Downtime Update | A system maintainer needs to deploy a new version of the AI model to improve response accuracy. They initiate a deployment through the continuous integration pipeline. The system seamlessly rolls out the update without any interruption in service to the end users. (RM1, RM2, RM4) |
| UC-3: Check Next Exam Date | A lecturer posts an update about an assignment, and a student prompts the assistant for information (ex. Next exam Date). (RS1, RS7, RS8, R3, RS10) |
| UC-4: View Personalized Dashboard | A Student signs into their school account through an SSO, and views their personalized dashboard that contains information about their upcoming events, deadlines, classes, etc. The System pulls that informaiton from connected systems and can also export infromation into the student's calendar. (RS2, RS3, RS7, RS13, R3) |
| UC-5: Proactive Assignment Notification | A student asks the assistant, "What assignments are due this week?". The system retrieves the student's personalized course schedule and assignment data from the LMS, then generates and presents a consolidated list of upcoming deadlines with direct links to the course materials. (RS1, RS2, R6) |
| UC-6: Onboard a New Data Source | The university adopts a new campus-wide calendar system. A system maintainer uses the configuration module to add the new system's API endpoint and authentication details, enabling the assistant to immediately begin synchronizing and responding to queries about new academic events. (RM5, RD1, RD2) |

<img width="603" height="489" alt="image" src="https://github.com/user-attachments/assets/37eac43e-d3fb-462f-9798-8e7ccf4c6306" />
