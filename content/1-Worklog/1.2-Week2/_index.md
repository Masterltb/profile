---
title: "Worklog Week 2"
date: ""
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---



### Week 2 Objectives

- Set up AWS account and IAM fundamentals.
- Enable MFA and create IAM users.
- Understand access control and credential management.

### Tasks for this week

| Day | Task                                                                                                                                                          | Start Date | Completion Date | Reference                                 |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Created AWS Free Tier account with root user.<br>- Set up billing alerts and free tier monitoring.                                                          | 16/09/2025 | 16/09/2025      | <https://aws.amazon.com/>                 |
| 3   | - Enabled MFA (Multi-Factor Authentication) on root account.<br>- Downloaded and saved backup codes securely.                                                 | 17/09/2025 | 17/09/2025      | <https://console.aws.amazon.com/iam/>     |
| 4   | - Created first IAM user for daily work.<br>- Attached AdministratorAccess policy to test user.<br>- Generated access keys and tested AWS CLI connection.     | 18/09/2025 | 18/09/2025      | <https://console.aws.amazon.com/iam/>     |
| 5   | - Explored AWS Management Console navigation.<br>- Learned about service regions and availability zones.<br>- Reviewed billing dashboard and free tier usage. | 19/09/2025 | 19/09/2025      | <https://console.aws.amazon.com/billing/> |
| 6   | - Studied IAM core components: Users, Groups, Roles, Policies.<br>- Created IAM group and added users to group.                                               | 20/09/2025 | 20/09/2025      | <https://console.aws.amazon.com/iam/>     |

### Week 2 Outcomes

- Successfully created and secured AWS Free Tier account with MFA enabled.
- Understood root user vs IAM user best practices.
- Created first IAM user with programmatic access (AWS CLI).
- Gained foundational knowledge of IAM access control model.
- Practiced with AWS Management Console and CLI for basic operations.
- Set up billing alerts to monitor free tier usage and prevent unexpected charges.

**Key Learnings:**

- Root user should only be used for account recovery (MFA + backup codes critical).
- IAM users provide granular access control and audit trail.
- Access keys should be rotated regularly for security.
- Always use least privilege principle when assigning permissions.
