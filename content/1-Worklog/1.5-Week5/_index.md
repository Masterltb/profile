---
title: "Worklog Week 5"
date: ""
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---



### Week 5 Objectives

- Launch and manage EC2 instances.
- Create custom AMI for reusable deployments.
- Deploy applications on EC2.
- Understand EC2 access recovery and management.

### Tasks for this week

| Day | Task                                                                                                                                                                    | Start Date | Completion Date | Reference                                         |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------- |
| 2   | - Launched EC2 instance (t2.micro) in public subnet.<br>- Generated and downloaded key pair for SSH access.<br>- Configured security group with SSH, HTTP, HTTPS ports. | 07/10/2025 | 07/10/2025      | <https://console.aws.amazon.com/ec2/>             |
| 3   | - Installed packages on EC2 instance.<br>- Created EBS snapshot of root volume.<br>- Built custom AMI from snapshot for faster future deployments.                      | 08/10/2025 | 08/10/2025      | <https://console.aws.amazon.com/ec2/>             |
| 4   | - Deployed Node.js application on EC2.<br>- Set up systemd service for application auto-start.<br>- Configured logging and monitored application health.                | 09/10/2025 | 09/10/2025      | <https://console.aws.amazon.com/ec2/>             |
| 5   | - Learned Session Manager as SSH alternative.<br>- Created IAM role with SSM permissions.<br>- Practiced access recovery without key pair.                              | 10/10/2025 | 10/10/2025      | <https://console.aws.amazon.com/systems-manager/> |
| 6   | - Created Lightsail instance as alternative to EC2.<br>- Deployed WordPress using Lightsail blueprint.<br>- Compared Lightsail vs EC2 pricing and use cases.            | 11/10/2025 | 11/10/2025      | <https://lightsail.aws.amazon.com/>               |

### Week 5 Outcomes

- Successfully launched and configured EC2 instances.
- Created reusable custom AMI for consistent deployments.
- Deployed working Node.js application with auto-restart capability.
- Learned Session Manager for enhanced security and auditability.
- Understood Lightsail as viable alternative for simpler workloads.
- Applied least privilege IAM policies for EC2 access control.

**Key Learnings:**

- Custom AMI saves deployment time and ensures consistency.
- Session Manager provides better security audit trail than SSH key pairs.
- Lightsail offers simpler, fixed-price alternative for appropriate workloads.
- Proper IAM roles eliminate need for credential management on instances.
