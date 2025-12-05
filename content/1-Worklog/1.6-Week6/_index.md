---
title: "Worklog Week 6"
date: ""
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---



### Week 6 Objectives

- Set up RDS for application data persistence.
- Understand database backup and recovery.
- Configure high availability with Multi-AZ and Read Replicas.
- Implement database security best practices.

### Tasks for this week

| Day | Task                                                                                                          | Start Date | Completion Date | Reference                             |
| --- | ------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------- |
| 2   | - Reviewed RDS fundamentals and database engines.<br>- Created RDS MySQL instance in private subnet.          | 14/10/2025 | 14/10/2025      | <https://console.aws.amazon.com/rds/> |
| 3   | - Enabled Multi-AZ for automatic failover capability.<br>- Configured automated backups with 7-day retention. | 15/10/2025 | 15/10/2025      | <https://console.aws.amazon.com/rds/> |
| 4   | - Created Read Replica for load distribution.<br>- Tested read/write distribution across instances.           | 16/10/2025 | 16/10/2025      | <https://console.aws.amazon.com/rds/> |
| 5   | - Configured security group for database access from app tier.<br>- Tested connectivity from EC2 instances.   | 17/10/2025 | 17/10/2025      | <https://console.aws.amazon.com/rds/> |
| 6   | - Performed backup and restore testing.<br>- Monitored database performance and connection metrics.           | 18/10/2025 | 18/10/2025      | <https://console.aws.amazon.com/rds/> |

### Week 6 Outcomes

- Successfully set up RDS MySQL instance with Multi-AZ enabled.
- Configured automated backup strategy with appropriate retention.
- Implemented Read Replica for query load distribution.
- Secured database with proper security group configuration.
- Tested backup/restore procedures for disaster recovery readiness.
- Verified connectivity and performance metrics.

**Key Learnings:**

- Multi-AZ provides high availability with automatic failover.
- Read Replicas scale read operations without impacting primary writes.
- Automated backups enable point-in-time recovery.
- Database should always reside in private subnet for security.
