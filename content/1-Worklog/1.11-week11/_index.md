---
title: "Worklog Week 11"
date: ""
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---



### Week 11 Objectives

- Implement **Slip vs Relapse** logic differentiation.
- Develop **Weekly Assessment** scheduler (7-day cycle).
- Build **Admin APIs** for CRUD operations on Quiz Templates.

### Tasks for this week

| Day | Task                                                                                                                                                                | Start Date | Completion Date | Reference                                                                                         |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------- |
| 2   | - **Feature:** Implemented **Slip** logic: User reports a slip -> Allowed recovery immediately without reset.<br>- **Feature:** Implemented **Relapse** logic: User reports relapse -> Immediate hard reset to 0. | 17/11/2025 | 17/11/2025      | —                                                                                                 |
| 3   | - **Scheduler:** Configured Spring Scheduler (`@Scheduled`) to trigger **Weekly Assessments**.<br>- **Logic:** Assigns a progress-check quiz every 7 days to active users. | 18/11/2025 | 18/11/2025      | <https://spring.io/guides/gs/scheduling-tasks/>                                                   |
| 4   | - **Admin API:** Built REST endpoints for Admin to Create/Read/Update/Delete **Quiz Templates**.<br>- **Validation:** Added validation logic to ensure quizzes have valid questions and scoring keys. | 19/11/2025 | 19/11/2025      | —                                                                                                 |
| 5   | - **Optimization:** Optimized the reset counter logic (Max 3 recoveries).<br>- **Logic:** Ensured the 4th recovery attempt triggers a hard reset regardless of quiz result. | 20/11/2025 | 20/11/2025      | —                                                                                                 |
| 6   | - **Integration:** Connected Weekly Quiz results to the User Profile to track long-term progress.<br>- **Refactor:** Refactored the `QuizService` to handle both "Recovery" and "Weekly" quiz types. | 21/11/2025 | 21/11/2025      | —                                                                                                 |

### Week 11 Outcomes

- **Slip vs Relapse** logic is fully implemented, strictly enforcing the program's recovery rules.
- **Weekly Assessment** system is automated, ensuring continuous monitoring of user progress.
- **Admin Module** for Quiz Management is complete, allowing dynamic content updates without code changes.
- **Recovery Cap** (Max 3 times) is successfully enforced, preventing system abuse.

**Key Learnings:**

- **Scheduling:** Using Spring Scheduler for periodic tasks vs external cron jobs.
- **Polymorphism in Services:** Designing `QuizService` to handle different quiz contexts (Recovery vs Weekly) efficiently.
- **Hard vs Soft Deletes:** Handling "Relapse" (Hard Reset) vs "Smoke Event" (Soft Reset) requires clear state definitions.
