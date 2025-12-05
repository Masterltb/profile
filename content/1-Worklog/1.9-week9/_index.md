---
title: "Worklog Week 9"
date: ""
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---



### Week 9 Objectives

- Initialize the **Program Service** utilizing Java 25 and Spring Boot.
- Design and implement the PostgreSQL database schema for Programs and Quizzes.
- Implement the Initial Assessment Quiz and Package Recommendation logic.

### Tasks for this week

| Day | Task                                                                                                                                                                                            | Start Date | Completion Date | Reference                                                             |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | --------------------------------------------------------------------- |
| 2   | - **Setup:** Initialized Spring Boot project with Java 25.<br>- **DB Design:** Designed ERD for `Users`, `Programs`, `QuizTemplates`, and `QuizResults` tables.                                 | 03/11/2025 | 03/11/2025      | <https://start.spring.io/>                                            |
| 3   | - **Implementation:** Implemented `MeQuizController` to handle the initial user quiz.<br>- **Logic:** Coded the logic to classify addiction levels (Light, Medium, Heavy) based on quiz scores. | 04/11/2025 | 04/11/2025      | —                                                                     |
| 4   | - **Database:** Seeded initial `ProgramTemplates` (Free/Paid tiers) into PostgreSQL.<br>- **Feature:** Developed the Recommendation Engine to suggest 3 suitable packages based on user level.  | 05/11/2025 | 05/11/2025      | <https://docs.spring.io/spring-data/jpa/docs/current/reference/html/> |
| 5   | - **API:** Built APIs for User Enrollment (Register for a Program).<br>- **Security:** Integrated JWT authentication filter for secure API access.                                              | 06/11/2025 | 06/11/2025      | <https://spring.io/guides/gs/securing-web/>                           |
| 6   | - **Testing:** Performed API testing using Postman for Assessment and Enrollment flows.<br>- **Review:** Code review ensuring Java 25 features are utilized correctly.                          | 07/11/2025 | 07/11/2025      | —                                                                     |

### Week 9 Outcomes

- Successfully established the foundational **Program Service** architecture.
- Completed the **Database Schema** implementation in PostgreSQL.
- Functional **Assessment & Recommendation** system allowing users to be classified and receive program suggestions.
- **User Enrollment** flow is operational, linking users to their chosen program tier.

**Key Learnings:**

- **Java 25:** Applied new language features for cleaner and more efficient code.
- **Business Logic Mapping:** Translating complex medical criteria (addiction levels) into conditional code logic.
- **Seeding Strategy:** Importance of initial data seeding for static templates like Programs.
