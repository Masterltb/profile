---
title: "Nhật ký Tuần 2"
date: ""
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---



### Mục tiêu Tuần 2

- Thiết lập tài khoản AWS và IAM fundamentals.
- Bật MFA và tạo IAM users.
- Hiểu access control và credential management.

### Các nhiệm vụ trong tuần

| Ngày | Nhiệm vụ                                                                                                                                                 | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2    | - Tạo tài khoản AWS Free Tier với root user.<br>- Thiết lập billing alerts và free tier monitoring.                                                      | 16/09/2025   | 16/09/2025      | <https://aws.amazon.com/>                 |
| 3    | - Bật MFA (Multi-Factor Authentication) trên root account.<br>- Tải và lưu backup codes an toàn.                                                         | 17/09/2025   | 17/09/2025      | <https://console.aws.amazon.com/iam/>     |
| 4    | - Tạo IAM user đầu tiên để sử dụng hàng ngày.<br>- Gán AdministratorAccess policy cho test user.<br>- Tạo access keys và test AWS CLI connection.        | 18/09/2025   | 18/09/2025      | <https://console.aws.amazon.com/iam/>     |
| 5    | - Khám phá AWS Management Console navigation.<br>- Tìm hiểu về service regions và availability zones.<br>- Xem xét billing dashboard và free tier usage. | 19/09/2025   | 19/09/2025      | <https://console.aws.amazon.com/billing/> |
| 6    | - Học IAM core components: Users, Groups, Roles, Policies.<br>- Tạo IAM group và thêm users vào group.                                                   | 20/09/2025   | 20/09/2025      | <https://console.aws.amazon.com/iam/>     |

### Kết quả Tuần 2

- Tạo thành công AWS Free Tier account với MFA enabled.
- Hiểu root user vs IAM user best practices.
- Tạo IAM user đầu tiên với programmatic access (AWS CLI).
- Kiến thức nền tảng về IAM access control model.
- Thực hành AWS Management Console và CLI cho basic operations.
- Thiết lập billing alerts để monitor free tier usage.

**Những điều quan trọng học được:**

- Root user chỉ dùng cho account recovery (MFA + backup codes rất quan trọng).
- IAM users cung cấp granular access control và audit trail.
- Access keys nên rotate thường xuyên cho security.
- Luôn dùng least privilege principle khi assign permissions.
