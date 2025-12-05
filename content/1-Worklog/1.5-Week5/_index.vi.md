---
title: "Nhật ký Tuần 5"
date: ""
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---



### Mục tiêu Tuần 5

- Launch và manage EC2 instances.
- Tạo custom AMI cho reusable deployments.
- Deploy applications trên EC2.
- Hiểu EC2 access recovery và management.

### Các nhiệm vụ trong tuần

| Ngày | Nhiệm vụ                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ------------------------------------------------- |
| 2    | - Launch EC2 instance (t2.micro) ở public subnet.<br>- Tạo và tải key pair cho SSH access.<br>- Cấu hình security group với SSH, HTTP, HTTPS ports.    | 07/10/2025   | 07/10/2025      | <https://console.aws.amazon.com/ec2/>             |
| 3    | - Install packages trên EC2.<br>- Tạo EBS snapshot của root volume.<br>- Build custom AMI từ snapshot cho faster future deployments.                   | 08/10/2025   | 08/10/2025      | <https://console.aws.amazon.com/ec2/>             |
| 4    | - Deploy Node.js application trên EC2.<br>- Thiết lập systemd service cho application auto-start.<br>- Cấu hình logging và monitor application health. | 09/10/2025   | 09/10/2025      | <https://console.aws.amazon.com/ec2/>             |
| 5    | - Học Session Manager như SSH alternative.<br>- Tạo IAM role với SSM permissions.<br>- Thực hành access recovery không key pair.                       | 10/10/2025   | 10/10/2025      | <https://console.aws.amazon.com/systems-manager/> |
| 6    | - Tạo Lightsail instance như alternative EC2.<br>- Deploy WordPress dùng Lightsail blueprint.<br>- So sánh Lightsail vs EC2 pricing và use cases.      | 11/10/2025   | 11/10/2025      | <https://lightsail.aws.amazon.com/>               |

### Kết quả Tuần 5

- Launch và cấu hình EC2 instances thành công.
- Tạo reusable custom AMI cho consistent deployments.
- Deploy Node.js application với auto-restart capability.
- Học Session Manager cho enhanced security và auditability.
- Hiểu Lightsail như viable alternative cho simpler workloads.
- Áp dụng least privilege IAM policies cho EC2 access control.

**Những điều quan trọng học được:**

- Custom AMI tiết kiệm thời gian deploy và ensure consistency.
- Session Manager cung cấp better security audit trail hơn SSH key pairs.
- Lightsail cung cấp simpler, fixed-price alternative cho appropriate workloads.
- Proper IAM roles eliminate cần credential management trên instances.
