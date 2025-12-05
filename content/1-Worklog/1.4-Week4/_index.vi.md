---
title: "Nhật ký Tuần 4"
date: ""
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Mục tiêu Tuần 4

- Thiết kế và triển khai VPC infrastructure.
- Cấu hình networking components: subnets, route tables, gateways.
- Triển khai security groups và network ACLs.
- Hiểu AWS Support plans.

### Các nhiệm vụ trong tuần

| Ngày | Nhiệm vụ                                                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                      |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------- |
| 2    | - Xem xét AWS Support plans (Basic, Developer, Business, Enterprise).<br>- Hiểu SLA, response times, và support channels cho mỗi tier.                                                            | 30/09/2025   | 30/09/2025      | <https://aws.amazon.com/support/plans/> |
| 3    | - Thiết kế VPC với CIDR block 10.0.0.0/16.<br>- Tạo public và private subnets across multiple AZs.                                                                                                | 01/10/2025   | 01/10/2025      | <https://console.aws.amazon.com/vpc/>   |
| 4    | - Cấu hình Internet Gateway (IGW) cho public subnet access.<br>- Thiết lập NAT Gateway ở public subnet cho private subnet internet access.<br>- Tạo và cấu hình route tables cho traffic routing. | 02/10/2025   | 02/10/2025      | <https://console.aws.amazon.com/vpc/>   |
| 5    | - Tạo security groups với inbound/outbound rules.<br>- Triển khai Network ACLs cho stateless filtering.<br>- Test connectivity giữa public và private subnets.                                    | 03/10/2025   | 03/10/2025      | <https://console.aws.amazon.com/vpc/>   |
| 6    | - Bật VPC Flow Logs cho network traffic monitoring.<br>- Xem xét security best practices và compliance requirements.                                                                              | 04/10/2025   | 04/10/2025      | <https://console.aws.amazon.com/vpc/>   |

### Kết quả Tuần 4

- Thiết kế và triển khai multi-AZ VPC infrastructure.
- Cấu hình internet và NAT gateways cho controlled connectivity.
- Triển khai tiered security với Security Groups và Network ACLs.
- Bật network visibility với VPC Flow Logs.
- Hiểu AWS Support ecosystem và select appropriate support plan.

**Những điều quan trọng học được:**

- VPC design nên có multiple AZs cho high availability.
- Public/Private subnet separation critical cho security.
- Security Groups stateful; Network ACLs stateless.
- Regular security group reviews prevent overly permissive rules.
