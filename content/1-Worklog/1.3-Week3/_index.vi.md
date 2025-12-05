---
title: "Nhật ký Tuần 3"
date: ""
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---



### Mục tiêu Tuần 3

- Hiểu IAM security best practices và credential handling.
- Tìm hiểu AWS Budgets để kiểm soát chi phí.
- Thực hành least privilege access patterns.

### Các nhiệm vụ trong tuần

| Ngày | Nhiệm vụ                                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------- |
| 2    | - Học IAM access keys vs temporary credentials.<br>- Tìm hiểu EC2 instance roles và metadata service.                                                                | 23/09/2025   | 23/09/2025      | <https://docs.aws.amazon.com/iam/>                |
| 3    | - Tạo IAM roles cho EC2 instances.<br>- Gán policies với least privilege principle.<br>- Test role-based access via metadata endpoint.                               | 24/09/2025   | 24/09/2025      | <https://console.aws.amazon.com/iam/>             |
| 4    | - Thiết lập AWS Budgets để monitor chi phí hàng tháng.<br>- Tạo alert thresholds ở 80% và 100% của $50 limit.<br>- Xem xét free tier usage patterns và cost drivers. | 25/09/2025   | 25/09/2025      | <https://console.aws.amazon.com/billing/budgets/> |
| 5    | - Cấu hình budget alerts via email notifications.<br>- Tìm hiểu cost optimization strategies và Reserved Instances.                                                  | 26/09/2025   | 26/09/2025      | <https://console.aws.amazon.com/ce/>              |
| 6    | - Thực hành apply permissions policies cho users và roles.<br>- Verify least privilege enforcement với test scenarios.                                               | 27/09/2025   | 27/09/2025      | <https://console.aws.amazon.com/iam/>             |

### Kết quả Tuần 3

- Chuyển từ hardcoded access keys sang IAM role-based approach.
- Cấu hình EC2 instance roles để secure access.
- Thiết lập AWS Budgets và cost monitoring infrastructure.
- Triển khai least privilege access patterns.
- Hiểu cost control mechanisms và free tier limits.

**Các thực hành bảo mật IAM:**

- IAM Roles ưu tiên hơn access keys cho applications và EC2 instances.
- Metadata service cung cấp temporary credentials tự động.
- Role-based access control đảm bảo security tốt hơn và auditability.
- Temporary credentials auto-rotate cho enhanced security.

**Cost monitoring và budget setup:**

- AWS Budgets được cấu hình với alerts ở 80% và 100% thresholds.
- Email notifications theo dõi spending patterns real-time.
- Budget reviews thường xuyên prevent unexpected charges.
- Cost monitoring cho early detection của resource waste.
