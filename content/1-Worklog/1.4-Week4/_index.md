---
title: "Worklog Week 4"
date: ""
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Week 4 Objectives

- Understand AWS Support tiers and when to use each one.
- Learn VPC fundamentals and network design patterns.
- Set up a secure network infrastructure with Security Groups and NACLs.
- Practice hands-on VPC creation, subnet configuration, and traffic management.

### Tasks for this week

| Day | Task                                                                                                                                                                                                     | Start Date | Completion Date | Reference                               |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | --------------------------------------- |
| 2   | - Reviewed AWS Support plans (Basic, Developer, Business, Enterprise).<br>- Understood SLA, response times, and support channels for each tier.                                                          | 30/09/2025 | 30/09/2025      | <https://aws.amazon.com/support/plans/> |
| 3   | - Designed VPC with CIDR block 10.0.0.0/16.<br>- Created public and private subnets across multiple AZs.                                                                                                 | 01/10/2025 | 01/10/2025      | <https://console.aws.amazon.com/vpc/>   |
| 4   | - Configured Internet Gateway (IGW) for public subnet access.<br>- Set up NAT Gateway in public subnet for private subnet internet access.<br>- Created and configured route tables for traffic routing. | 02/10/2025 | 02/10/2025      | <https://console.aws.amazon.com/vpc/>   |
| 5   | - Created security groups with inbound/outbound rules.<br>- Implemented Network ACLs for stateless filtering.<br>- Tested connectivity between public and private subnets.                               | 03/10/2025 | 03/10/2025      | <https://console.aws.amazon.com/vpc/>   |
| 6   | - Enabled VPC Flow Logs for network traffic monitoring.<br>- Reviewed security best practices and compliance requirements.                                                                               | 04/10/2025 | 04/10/2025      | <https://console.aws.amazon.com/vpc/>   |

### Week 4 Outcomes

- Successfully designed and implemented multi-AZ VPC infrastructure.
- Configured internet and NAT gateways for controlled connectivity.
- Implemented tiered security with Security Groups and Network ACLs.
- Enabled network visibility with VPC Flow Logs.
- Understood AWS Support ecosystem and selected appropriate support plan.

**Key Learnings:**

- VPC design should include multiple AZs for high availability.
- Public/Private subnet separation is critical for security.
- Security Groups are stateful; Network ACLs are stateless.
- Regular security group reviews prevent overly permissive rules.
