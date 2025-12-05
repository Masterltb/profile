---
title: "Worklog Week 10"
date: ""
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---



### Week 10 Objectives

- Implement the **Daily Routine** core feature (4 steps/day).
- Build the **Streak Tracking** system (Current Streak, Best Streak).
- Develop logic for **Smoke Events** handling (Reset pending recovery).

### Tasks for this week

| Day | Task                                                                                                                                                                | Start Date | Completion Date | Reference                                                                                         |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------- |
| 2   | - **Feature:** Designed `DailyRoutine` entity.<br>- **Logic:** Implemented logic to generate 4 distinct steps daily for enrolled users.                              | 10/11/2025 | 10/11/2025      | —                                                                                                 |
| 3   | - **API:** Built APIs for users to mark daily steps as completed.<br>- **Logic:** Implemented **Streak Service** to auto-increment streak upon completing all 4 steps. | 11/11/2025 | 11/11/2025      | —                                                                                                 |
| 4   | - **Feature:** Developed the **Smoke Event** trigger.<br>- **Logic:** Implemented "Soft Reset" logic: When a smoke event occurs, streak is paused (Status: PENDING_RECOVERY). | 12/11/2025 | 12/11/2025      | —                                                                                                 |
| 5   | - **Recovery Logic:** Implemented the logic to assign a "Recovery Quiz".<br>- **Validation:** If user passes the quiz (Max 3 attempts), restore the streak; otherwise, reset to 0. | 13/11/2025 | 13/11/2025      | —                                                                                                 |
| 6   | - **Testing:** Unit tested the Streak Calculation logic covering edge cases (midnight reset, timezone issues).<br>- **Integration:** Verified the flow from Daily Routine -> Streak Update -> Smoke Event. | 14/11/2025 | 14/11/2025      | <https://junit.org/junit5/>                                                                       |

### Week 10 Outcomes

- **Daily Routine** engine is operational, driving user engagement.
- **Streak System** accurately tracks user consistency and handles "Best Streak" updates.
- **Recovery Mechanism** successfully handles "Smoke Events", allowing users a chance to recover their streak (limited to 3 times) via quizzes.

**Key Learnings:**

- **State Management:** Managing complex user states (Active, Pending Recovery, Reset) requires robust state machine logic.
- **Time Sensitive Logic:** Handling "daily" resets requires careful handling of server time vs user timezone.
- **Transactional Integrity:** Ensuring streak updates and task completions happen atomically.