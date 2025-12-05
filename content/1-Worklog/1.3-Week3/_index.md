---
title: "Worklog Week 3"
date: ""
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---



### Week 3 Objectives

- Understand IAM security best practices and credential handling.
- Learn about AWS Budgets for cost control.
- Practice least privilege access patterns.

### Tasks for this week

| Day | Task                                                                                                                                                                   | Start Date | Completion Date | Reference                                         |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------- |
| 2   | - Studied IAM access keys vs temporary credentials.<br>- Learned about EC2 instance roles and metadata service.                                                        | 23/09/2025 | 23/09/2025      | <https://docs.aws.amazon.com/iam/>                |
| 3   | - Created IAM roles for EC2 instances.<br>- Attached policies to roles with least privilege principle.<br>- Tested role-based access via metadata endpoint.            | 24/09/2025 | 24/09/2025      | <https://console.aws.amazon.com/iam/>             |
| 4   | - Set up AWS Budgets for monthly cost monitoring.<br>- Created alert thresholds at 80% and 100% of $50 limit.<br>- Reviewed free tier usage patterns and cost drivers. | 25/09/2025 | 25/09/2025      | <https://console.aws.amazon.com/billing/budgets/> |
| 5   | - Configured budget alerts via email notifications.<br>- Learned about cost optimization strategies and Reserved Instances.                                            | 26/09/2025 | 26/09/2025      | <https://console.aws.amazon.com/ce/>              |
| 6   | - Practiced applying permissions policies to users and roles.<br>- Verified least privilege enforcement with test scenarios.                                           | 27/09/2025 | 27/09/2025      | <https://console.aws.amazon.com/iam/>             |

### Week 3 Outcomes

- Transitioned from hardcoded access keys to IAM role-based approach.
- Successfully configured EC2 instance roles for secure access.
- Established AWS Budgets and cost monitoring infrastructure.
- Implemented least privilege access patterns across IAM users and roles.
- Gained understanding of cost control mechanisms and free tier limits.

**IAM security practices implemented:**

- IAM Roles are preferred over access keys for applications and EC2 instances.
- Metadata service provides temporary credentials automatically.
- Role-based access control ensures better security and auditability.
- Temporary credentials auto-rotate for enhanced security.

**Cost monitoring and budget setup:**

- AWS Budgets configured with alerts at 80% and 100% thresholds.
- Email notifications track spending patterns in real-time.
- Regular budget reviews prevent unexpected charges.
- Cost monitoring enables early detection of resource waste.
