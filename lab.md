# Tổng Hợp Nội Dung 16 Bài Lab AWS Workshop

Tài liệu này tổng hợp và phân tích chi tiết các bài lab AWS cốt lõi, được chia thành 4 phần chính: **Quản trị & Bảo mật**, **Hạ tầng Mạng & Tính toán**, **Ứng dụng & Mở rộng**, và **Tổng hợp/Hỗ trợ**.

---

## Phần 1: Quản Trị Tài Khoản, Bảo Mật & Chi Phí

### 1. Thiết Lập Tài Khoản AWS

**URL:** https://000001.awsstudygroup.com/vi/  
**Tổng Quan:** Hướng dẫn thiết lập tài khoản AWS an toàn.

#### Mục Lục & Bài Lab Con

| Mục                                            | Nội Dung Chi Tiết                                                                                                     |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| 1. Tạo mới tài khoản AWS                       | Hướng dẫn tạo tài khoản, xác minh email, thông tin thanh toán (chọn gói Basic).                                       |
| 2. MFA cho Tài khoản AWS                       | Thiết lập MFA (Multi-Factor Authentication) cho tài khoản Root (sử dụng thiết bị ảo hoặc khóa U2F).                   |
| 3. Tạo Admin Group và Admin User               | Tạo IAM Group (Administrators) và gán Policy (AdministratorAccess), tạo IAM User và thêm vào nhóm theo Best Practice. |
| 4. Khám phá và cấu hình AWS Management Console | Thiết lập Region mặc định, tìm kiếm dịch vụ, cá nhân hóa Dashboard.                                                   |

### 2. Quản Trị Quyền Truy Cập Với AWS IAM

**URL:** https://000002.awsstudygroup.com/vi/  
**Tổng Quan:** Nghiên cứu sâu về các thành phần IAM và cơ chế IAM Role nâng cao.

#### Mục Lục & Bài Lab Con

| Mục                                     | Nội Dung Chi Tiết                                                                                         |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| 1. Giới thiệu                           | Định nghĩa IAM Group, IAM User, IAM Policy, IAM Role và nguyên tắc Đặc quyền tối thiểu (Least Privilege). |
| 2. Tạo IAM Group và IAM User            | Thực hành tạo cấu trúc quyền quản trị cơ bản.                                                             |
| 3. Tạo IAM Role và IAM User             | Tạo Role và User chỉ có quyền Console Access.                                                             |
| 4. Chuyển đổi IAM Role (Switching Role) | Thực hành sử dụng Switch Role để cấp quyền tạm thời/có giới hạn cho người dùng.                           |

### 3. Cấp Quyền Cho Ứng Dụng Truy Cập Dịch Vụ AWS Với IAM Role

**URL:** https://000048.awsstudygroup.com/vi/  
**Tổng Quan:** Phân biệt và áp dụng phương pháp cấp quyền an toàn cho ứng dụng trên EC2.

#### Mục Lục & Bài Lab Con

| Mục                   | Nội Dung Chi Tiết                                                                                                                             |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Sử dụng Access Key | Thực hành chống mẫu (Anti-pattern): Lấy Access Key/Secret Key và cấu hình thủ công trên EC2 (Giải thích rủi ro bảo mật).                      |
| 2. IAM Role trên EC2  | Thực hành Best Practice: Gán IAM Role trực tiếp cho EC2 Instance để tự động nhận quyền truy cập S3/dịch vụ khác mà không cần lưu khóa bí mật. |

### 4. Quản Lý Mức Chi Phí Với AWS Budget

**URL:** https://000007.awsstudygroup.com/vi/  
**Tổng Quan:** Thiết lập ngân sách và cảnh báo chi phí để quản lý chi tiêu chủ động.

#### Mục Lục & Bài Lab Con

| Mục                                     | Nội Dung Chi Tiết                                                              |
| --------------------------------------- | ------------------------------------------------------------------------------ |
| 1. Giới thiệu AWS Budget                | Định nghĩa và phân loại 4 loại ngân sách.                                      |
| 2. Tạo Cost Budget                      | Thiết lập ngân sách dựa trên tổng chi phí thực tế và cảnh báo.                 |
| 3. Tạo Usage Budget                     | Thiết lập ngân sách dựa trên mức sử dụng dịch vụ cụ thể (ví dụ: giờ chạy EC2). |
| 4. Tạo RI Budget & Savings Plans Budget | Thiết lập ngân sách để theo dõi mức độ cam kết sử dụng dịch vụ.                |

### 5. Yêu Cầu Hỗ Trợ Với AWS Support

**URL:** https://000009.awsstudygroup.com/vi/  
**Tổng Quan:** Tìm hiểu về dịch vụ hỗ trợ khách hàng và quy trình tạo yêu cầu hỗ trợ.

#### Mục Lục & Bài Lab Con

| Mục                       | Nội Dung Chi Tiết                                                                             |
| ------------------------- | --------------------------------------------------------------------------------------------- |
| 1. Các gói hỗ trợ của AWS | Phân tích Basic, Developer, Business, Enterprise về thời gian phản hồi và phạm vi hỗ trợ.     |
| 2. Quản lý Yêu cầu Hỗ trợ | Thực hành khởi tạo một Support Case và lựa chọn Mức độ nghiêm trọng (Severity Level) phù hợp. |

---

## Phần 2: Hạ Tầng Mạng Và Tính Toán

### 6. Triển Khai Hạ Tầng Mạng Với Amazon VPC

**URL:** https://000003.awsstudygroup.com/vi/  
**Tổng Quan:** Thiết lập môi trường mạng riêng ảo, nền tảng cho mọi dịch vụ AWS.

#### Mục Lục & Bài Lab Con

| Mục                          | Nội Dung Chi Tiết                                                                                                                         |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Tường lửa trong VPC       | Phân biệt và cấu hình Security Group (SG) và Network ACLs (NACLs).                                                                        |
| 2. Các bước chuẩn bị         | Tạo VPC, Subnets (Public/Private), Internet Gateway (IGW), NAT Gateway và Route Table. Kích hoạt VPC Flow Logs.                           |
| 3. Cấu hình Site to Site VPN | Lab nâng cao: Thiết lập kết nối Hybrid Cloud giữa VPC và môi trường On-premise mô phỏng bằng Virtual Private Gateway và Customer Gateway. |

### 7. Bắt Đầu Và Triển Khai Ứng Dụng Trên Amazon EC2

**URL:** https://000004.awsstudygroup.com/vi/  
**Tổng Quan:** Khởi tạo và quản lý máy chủ ảo EC2, triển khai ứng dụng mẫu trên nhiều hệ điều hành.

#### Mục Lục & Bài Lab Con

| Mục                                     | Nội Dung Chi Tiết                                                                                        |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| 1. Khởi tạo Windows/Linux instance      | Tạo và kết nối (RDP/SSH) đến các Instance.                                                               |
| 2. Amazon EC2 cơ bản                    | Tạo EC2 Snapshot, tạo Custom AMI, truy cập khi mất Key Pair (sử dụng SSM).                               |
| 3. Triển khai ứng dụng Node.js          | Cài đặt môi trường và triển khai ứng dụng web mẫu trên cả Amazon Linux và Windows.                       |
| 4. Giới hạn sử dụng tài nguyên bằng IAM | Sử dụng IAM Policy để giới hạn người dùng sử dụng EC2 theo Region, Instance Type, hoặc Địa chỉ IP nguồn. |

### 8. Tối Ưu Chi Phí Tính Toán Với Amazon Lightsail

**URL:** https://000045.awsstudygroup.com/vi/  
**Tổng Quan:** Sử dụng Lightsail để triển khai ứng dụng với chi phí cố định, đơn giản hóa việc quản lý.

#### Mục Lục & Bài Lab Con

| Mục                                         | Nội Dung Chi Tiết                                                                             |
| ------------------------------------------- | --------------------------------------------------------------------------------------------- |
| 1. Triển khai Lightsail Database & Instance | Triển khai các ứng dụng (Wordpress, Prestashop, Akaunting) bằng Blueprints của Lightsail.     |
| 2. Bảo mật ứng dụng                         | Cấu hình Firewall và Networking cơ bản.                                                       |
| 3. Tạo Snapshot & Dịch chuyển               | Tạo bản sao lưu (Snapshot) và dịch chuyển sang Instance lớn hơn (Scaling Up) khi cần mở rộng. |
| 4. Tạo cảnh báo                             | Thiết lập các cảnh báo (Alerts) để theo dõi trạng thái hệ thống.                              |

### 9. Sử Dụng Cloud IDE Trên Trình Duyệt Với AWS Cloud9

**URL:** https://000049.awsstudygroup.com/vi/  
**Tổng Quan:** Sử dụng môi trường phát triển tích hợp (IDE) trên nền web.

#### Mục Lục & Bài Lab Con

| Mục                           | Nội Dung Chi Tiết                                                                                |
| ----------------------------- | ------------------------------------------------------------------------------------------------ |
| 1. Tạo Cloud9 instance        | Hướng dẫn tạo môi trường Cloud9 mới và chọn loại instance backend.                               |
| 2. Tính năng cơ bản & AWS CLI | Thao tác trên Terminal, chỉnh sửa file, và thử nghiệm AWS CLI đã được cấu hình sẵn trong Cloud9. |

### 10. Sử Dụng AWS CLI Trên Các Amazon EC2

**URL:** https://000011.awsstudygroup.com/vi/  
**Tổng Quan:** Thực hành quản lý tài nguyên AWS thông qua cửa sổ lệnh.

#### Mục Lục & Bài Lab Con

| Mục                              | Nội Dung Chi Tiết                                                                            |
| -------------------------------- | -------------------------------------------------------------------------------------------- |
| 1. Cài đặt và Cấu hình AWS CLI   | Hướng dẫn cài đặt, sử dụng lệnh aws configure để thiết lập Access Key, Secret Key và Region. |
| 2. Profile trong AWS CLI         | Hướng dẫn tạo và sử dụng nhiều Profile khác nhau.                                            |
| 3. AWS CLI với S3, IAM, VPC, EC2 | Thực hành các lệnh tạo/quản lý tài nguyên trên các dịch vụ chính.                            |

### 11. Thiết Lập Hệ Thống DNS Hybrid Với Route 53 Resolver

**URL:** https://000010.awsstudygroup.com/vi/  
**Tổng Quan:** Xây dựng kiến trúc DNS lai (Hybrid DNS) giữa AWS Cloud và On-premise.

#### Mục Lục & Bài Lab Con

| Mục                             | Nội Dung Chi Tiết                                                                            |
| ------------------------------- | -------------------------------------------------------------------------------------------- |
| 1. Giới thiệu Route 53 Resolver | Định nghĩa Outbound Endpoints, Inbound Endpoints, và Resolver Rules.                         |
| 2. Thiết lập DNS                | Tạo Inbound/Outbound Endpoint và Resolver Rules để chuyển tiếp truy vấn giữa hai môi trường. |
| 3. Thử nghiệm kết quả           | Kiểm tra phân giải tên miền hai chiều.                                                       |

---

## Phần 3: Ứng Dụng, Mở Rộng Và Giám Sát

### 12. Hosting Static Website Với Amazon S3

**URL:** https://000057.awsstudygroup.com/vi/  
**Tổng Quan:** Sử dụng S3 để lưu trữ và phân phối trang web tĩnh.

#### Mục Lục & Bài Lab Con

| Mục                                       | Nội Dung Chi Tiết                                                              |
| ----------------------------------------- | ------------------------------------------------------------------------------ |
| 1. Bật tính năng Static Website           | Cấu hình S3 Bucket thành Endpoint tĩnh.                                        |
| 2. Cấu hình Block Public Access           | Quản lý và tùy chỉnh bảo mật Block Public Access và Bucket Policy.             |
| 3. Tăng tốc Static Website với Cloudfront | Nâng cao: Tích hợp Amazon CloudFront (CDN) để tăng tốc độ truy cập và bảo mật. |
| 4. Tính năng Bucket Versioning            | Bật Versioning để bảo vệ dữ liệu khỏi việc xóa/ghi đè.                         |

### 13. Tạo Cơ Sở Dữ Liệu Trên Amazon RDS

**URL:** https://000005.awsstudygroup.com/vi/  
**Tổng Quan:** Triển khai và quản lý cơ sở dữ liệu quan hệ với khả năng Sẵn sàng cao (HA).

#### Mục Lục & Bài Lab Con

| Mục                          | Nội Dung Chi Tiết                                                                                                          |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 1. Tạo RDS database instance | Triển khai RDS Instance và cấu hình tùy chọn Multi-AZ (cho HA/DR).                                                         |
| 2. Multi-AZ và Read Replicas | Phân tích chi tiết và ứng dụng của Multi-AZ (sao chép đồng bộ) và Read Replicas (sao chép không đồng bộ, mở rộng tải đọc). |
| 3. Backup và Restore         | Thực hành tạo DB Snapshot và khôi phục cơ sở dữ liệu.                                                                      |

### 14. Tự Động Mở Rộng Qui Mô Ứng Dụng Với Amazon EC2 Autoscaling

**URL:** https://000006.awsstudygroup.com/vi/  
**Tổng Quan:** Triển khai khả năng mở rộng tự động cho ứng dụng web.

#### Mục Lục & Bài Lab Con

| Mục                               | Nội Dung Chi Tiết                                                                           |
| --------------------------------- | ------------------------------------------------------------------------------------------- |
| 1. Tạo Launch Template            | Tạo khuôn mẫu (template) cấu hình cho các EC2 Instance mới.                                 |
| 2. Thiết lập Load Balancer        | Tạo Application Load Balancer (ALB) và Target Group để phân phối tải.                       |
| 3. Tạo Auto Scaling Group         | Tạo ASG và liên kết với ALB.                                                                |
| 4. Kiểm thử các giải pháp Scaling | Thực hành và kiểm tra Manual, Scheduled, Dynamic (dựa trên metrics), và Predictive Scaling. |

### 15. Tạo Bảng Theo Dõi Hệ Thống Với Amazon Cloudwatch

**URL:** https://000008.awsstudygroup.com/vi/  
**Tổng Quan:** Giám sát hiệu suất và hoạt động của toàn bộ hệ thống AWS.

#### Mục Lục & Bài Lab Con

| Mục                      | Nội Dung Chi Tiết                                                                           |
| ------------------------ | ------------------------------------------------------------------------------------------- |
| 1. CloudWatch Metric     | Xem, tìm kiếm và thực hiện các phép toán trên Metrics của các dịch vụ.                      |
| 2. CloudWatch Logs       | Thu thập, lưu trữ và sử dụng Logs Insights để phân tích nhật ký.                            |
| 3. CloudWatch Alarms     | Thiết lập các cảnh báo (Alarms) dựa trên ngưỡng của Metrics và cấu hình hành động phản hồi. |
| 4. CloudWatch Dashboards | Tạo bảng điều khiển trực quan để theo dõi hiệu suất hệ thống.                               |

---

## Phần 4: Workshop Tổng Hợp

### 16. Highly Available Web Application Workshop (Wordpress)

**URL:** https://000021.awsstudygroup.com/vi/  
**Tổng Quan:** Bài lab tổng hợp cuối cùng: Triển khai ứng dụng Wordpress có Tính Sẵn Sàng Cao và Khả năng Mở rộng bằng cách kết hợp tất cả các dịch vụ đã học.

#### Mục Lục & Bài Lab Con

| Mục                           | Nội Dung Chi Tiết                                                                                       |
| ----------------------------- | ------------------------------------------------------------------------------------------------------- |
| 1. Cấu hình Multi-AZ          | Sử dụng RDS Multi-AZ cho Database và triển khai Web Server trên nhiều Availability Zone trong VPC.      |
| 2. Cài đặt và Tạo Autoscaling | Tạo Custom AMI từ Web Server đã cài Wordpress, sau đó cấu hình ASG và ELB để tự động co giãn.           |
| 3. Tích hợp Cloudfront        | Sử dụng Amazon CloudFront (CDN) để tăng tốc độ phân phối và bảo vệ ứng dụng web khỏi các cuộc tấn công. |
