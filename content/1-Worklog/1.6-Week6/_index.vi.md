---
title: "Nhật ký Tuần 6"
date: ""
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---



### Mục tiêu Tuần 6

- Setup RDS cho application data persistence.
- Hiểu database backup và recovery.
- Cấu hình high availability với Multi-AZ và Read Replicas.
- Triển khai database security best practices.

### Các nhiệm vụ trong tuần

| Ngày | Nhiệm vụ                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                    |
| ---- | ------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ------------------------------------- |
| 2    | - Xem xét RDS fundamentals và database engines.<br>- Tạo RDS MySQL instance ở private subnet.          | 14/10/2025   | 14/10/2025      | <https://console.aws.amazon.com/rds/> |
| 3    | - Bật Multi-AZ cho automatic failover capability.<br>- Cấu hình automated backups với 7-day retention. | 15/10/2025   | 15/10/2025      | <https://console.aws.amazon.com/rds/> |
| 4    | - Tạo Read Replica cho load distribution.<br>- Test read/write distribution across instances.          | 16/10/2025   | 16/10/2025      | <https://console.aws.amazon.com/rds/> |
| 5    | - Cấu hình security group cho database access từ app tier.<br>- Test connectivity từ EC2 instances.    | 17/10/2025   | 17/10/2025      | <https://console.aws.amazon.com/rds/> |
| 6    | - Thực hành backup và restore testing.<br>- Monitor database performance và connection metrics.        | 18/10/2025   | 18/10/2025      | <https://console.aws.amazon.com/rds/> |

### Kết quả Tuần 6

- Setup RDS MySQL instance với Multi-AZ enabled thành công.
- Cấu hình automated backup strategy với appropriate retention.
- Triển khai Read Replica cho query load distribution.
- Bảo mật database với proper security group configuration.
- Test backup/restore procedures cho disaster recovery readiness.
- Verify connectivity và performance metrics.

**Những điều quan trọng học được:**

- Multi-AZ cung cấp high availability với automatic failover.
- Read Replicas scale read operations không impact primary writes.
- Automated backups cho phép point-in-time recovery.
- Database nên luôn ở private subnet cho security.
