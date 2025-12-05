---
title: "Worklog Week 12"
date: ""
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---



### Week 12 Objectives

- Conduct comprehensive **Unit Testing** & **Integration Testing**.
- Optimize **PostgreSQL Queries** for performance.
- Finalize documentation and prepare for deployment handover.

### Tasks for this week

| Day | Task                                                                                                                                                                | Start Date | Completion Date | Reference                                                                                         |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------- |
| 2   | - **Testing:** Wrote **JUnit/Mockito** tests for critical business logic (Streak Calculation, Reset limit enforcement).<br>- **Goal:** Achieved >80% code coverage for `StreakService`. | 24/11/2025 | 24/11/2025      | <https://site.mockito.org/>                                                                       |
| 3   | - **Testing:** Performed **Integration Tests** using `@SpringBootTest` to verify the full flow: Smoke Event -> Quiz -> Streak Restore.<br>- **Validation:** Verified database consistency after transaction rollbacks. | 25/11/2025 | 25/11/2025      | —                                                                                                 |
| 4   | - **Optimization:** Analyzed slow queries using `EXPLAIN ANALYZE` in PostgreSQL.<br>- **Action:** Added indexes to `user_id` and `created_at` columns in the `DailyRoutine` table to speed up streak calculation. | 26/11/2025 | 26/11/2025      | <https://www.postgresql.org/docs/current/using-explain.html>                                      |
| 5   | - **Documentation:** Generated API documentation using **Swagger/OpenAPI**.<br>- **Reporting:** Finalized the internship report summarizing the backend architecture and logic flows. | 27/11/2025 | 27/11/2025      | <https://swagger.io/>                                                                             |
| 6   | - **Review:** Final code cleanup and refactoring.<br>- **Presentation:** Presented the completed `program-service` backend logic to the mentor.                                     | 28/11/2025 | 28/11/2025      | —                                                                                                 |

### Week 12 Outcomes

- **High Quality Code:** Verified core logic stability with extensive Unit and Integration tests.
- **Performance Optimized:** Improved database query performance for user stats retrieval.
- **Documentation:** Delivered clear API documentation for Frontend integration.
- **Completion:** Successfully built and delivered the backend for the **Smoking Cessation Support Platform**.

**Overall Internship Reflection:**
Over the last 4 weeks, I have architected and implemented the core backend for the "Program Service" from scratch using Java 25 and Spring Boot. I successfully translated complex domain requirements (Addiction levels, Recovery logic) into robust, testable code.