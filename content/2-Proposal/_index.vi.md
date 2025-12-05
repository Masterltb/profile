---
title: "Bản đề xuất"
date: "2025-01-01"
weight: 2
chapter: true
pre: " <b> 2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Đề xuất Hệ thống Hỗ trợ Cai thuốc lá

## 1. Tóm tắt điều hành

Đề xuất này phác thảo thiết kế và triển khai **Nền tảng Hỗ trợ Cai thuốc lá** dựa trên đám mây, giúp người dùng bỏ thuốc thông qua theo dõi dữ liệu, phân tích hành vi, huấn luyện viên AI và sự tham gia của cộng đồng.

Hệ thống tích hợp cơ sở hạ tầng backend hiện đại, có khả năng mở rộng được triển khai trên **AWS Cloud**, đảm bảo tính khả dụng cao, bảo mật và trải nghiệm người dùng liền mạch. Mục tiêu là cung cấp một hành trình thông minh, được cá nhân hóa để người dùng theo dõi, lập kế hoạch và đạt được mục tiêu cai thuốc lá—đồng thời cung cấp cho quản trị viên và chuyên gia y tế các công cụ để hỗ trợ họ.

---

## 2. Mục tiêu hệ thống

- **Kế hoạch cá nhân hóa:** Giúp người dùng xây dựng và tuân theo lộ trình cai thuốc riêng biệt.
- **Theo dõi thời gian thực:** Ghi nhận hành vi hút thuốc và tiến triển sức khỏe tức thì.
- **Huấn luyện viên AI:** Cung cấp hướng dẫn, nhắc nhở và phản hồi tạo động lực tự động.
- **Gắn kết cộng đồng:** Cho phép tương tác xã hội và khích lệ giữa các thành viên.
- **Độ tin cậy:** Cung cấp hạ tầng cloud-native an toàn, dễ mở rộng.

---

## 3. Các tính năng chính

### Tính năng hướng người dùng

- **Trang chủ & Knowledge Hub:** Giới thiệu nền tảng, chia sẻ câu chuyện thành công và blog giáo dục.
- **Thành viên & Thanh toán:** Đăng ký, chọn gói (Free/Premium) và thanh toán an toàn qua bên thứ 3.
- **Theo dõi hút thuốc:** Ghi nhật ký tiêu thụ, tần suất, chi phí và các yếu tố kích thích (triggers).
- **Lập kế hoạch & Phân nhánh:** Kế hoạch động (lý do, giai đoạn). Nếu tái nghiện, hệ thống tự động điều chỉnh lộ trình phục hồi.
- **Theo dõi tiến độ:** Biểu đồ trực quan về số ngày không khói thuốc, tiền tiết kiệm và cải thiện sức khỏe.
- **Thông báo động lực:** Nhắc nhở hàng ngày/tuần qua App, Email hoặc SMS.
- **Thành tích (Gamification):** Nhận và chia sẻ huy hiệu (ví dụ: “1 ngày không khói thuốc”, “Tiết kiệm 100K”).
- **Huấn luyện viên AI:** Hướng dẫn cá nhân hóa với rào chắn an toàn và khả năng chuyển giao cho người thật.
- **Tương tác chuyên gia:** Chat hoặc gọi Video trực tiếp với huấn luyện viên sức khỏe.
- **Quản lý hồ sơ:** Chỉnh sửa mục tiêu, thông báo và quyền riêng tư.

### Tính năng Quản trị & Vận hành

- **Dashboard phân tích:** Theo dõi chỉ số tương tác và dữ liệu sức khỏe cộng đồng.
- **Cổng thông tin Huấn luyện viên:** Phân công hồ sơ, phân loại rủi ro và quản lý tư vấn.
- **Quản lý giá cước:** Định nghĩa các gói phí, quyền lợi và khuyến mãi.
- **Quản lý nội dung:** Quản lý bài viết, thông điệp động lực và tài liệu giáo dục.
- **Quản lý phản hồi:** Theo dõi mức độ hài lòng và chất lượng nội dung.

---

## 4. Kiến trúc hệ thống (AWS Cloud)

Hệ thống tận dụng các dịch vụ AWS được quản lý để đảm bảo hiệu năng và bảo mật.

![Sơ đồ kiến trúc nền tảng](/images/2-Proposal/arch.drawio.png)

### Lớp Frontend (Giao diện)
- **Amazon S3:** Lưu trữ trang web tĩnh và frontend ứng dụng (React).
- **Amazon CloudFront:** Phân phối nội dung toàn cầu và xử lý mã hóa SSL/TLS.

### Xác thực & Ủy quyền
- **Amazon Cognito:** Quản lý định danh, đăng nhập và bảo mật quyền truy cập.

### Lớp Ứng dụng
- **AWS Lambda:** Xử lý serverless cho webhook thanh toán và tác vụ nền.
- **Coach Chat/Video:** Cho phép giao tiếp thời gian thực.
- **Thanh toán:** Tích hợp bảo mật qua webhook; khóa lưu trong **AWS Secrets Manager**.
- **Network Load Balancer (NLB):** Phân phối tải đến các dịch vụ backend.
- **EC2 Instances (Private Subnet):** Chạy các microservices cốt lõi (User, Cessation, Social, AI).

### Lớp Dữ liệu
- **PostgreSQL (trên EC2):** Lưu trữ dữ liệu quan hệ (người dùng, kế hoạch, giao dịch).
- **MongoDB (trên EC2):** Lưu trữ dữ liệu xã hội và nội dung phi cấu trúc.
- **Amazon S3:** Lưu trữ tài sản media và sao lưu cơ sở dữ liệu.

### Quy trình DevOps
- **CodePipeline:** Tự động hóa quy trình build và deploy.
- **Amazon ECR:** Lưu trữ container image.
- **VPC Endpoint:** Đảm bảo giao tiếp nội bộ an toàn.

---

## 5. Bảo mật và Tuân thủ

- **Mã hóa:** TLS cho đường truyền; AES-256 cho dữ liệu lưu trữ.
- **Kiểm soát truy cập:** Chính sách IAM chi tiết cho từng vai trò hệ thống.
- **An ninh mạng:** Backend nằm trong Private Subnet; giao tiếp qua VPC Endpoints.
- **Thanh toán:** Tuân thủ PCI-DSS (không lưu dữ liệu thẻ trực tiếp).
- **Quyền riêng tư:** Quản lý chặt chẽ sự đồng ý và vòng đời dữ liệu người dùng.

---

## 6. Khả năng mở rộng và Hiệu suất

- **Auto Scaling:** Tự động điều chỉnh dung lượng EC2 và Lambda theo nhu cầu.
- **Caching:** CloudFront cache nội dung tĩnh để tăng tốc độ truy cập.
- **Microservices:** Thiết kế tách biệt cho phép mở rộng độc lập từng tính năng.
- **Event-Driven:** EventBridge tách biệt các quy trình thông báo và xử lý nền.

---

## 7. Ước tính chi phí

### Các khoản MIỄN PHÍ ($0.00)
- **Lambda Functions (x2):** Trong giới hạn 1 triệu requests/tháng (Free Tier).
- **S3 Buckets (x2):** Trong giới hạn 5GB Standard (Free Tier).
- **Amazon ECR:** Trong giới hạn 500MB/tháng.
- **CloudFront:** Trong giới hạn 1TB truyền dữ liệu.
- **EC2 Instance (x1):** 750 giờ/tháng (t4g.micro).
- **Cognito:** Miễn phí cho 50.000 người dùng đầu tiên (MAU).

### Chi phí ước tính (Trả phí)
- **Compute (Microservices):** 3x EC2 instances (t4g.small) ≈ **$45.00**
- **Networking (Load Balancing):** 1x Network Load Balancer ≈ **$18.00**
- **Security (Private Access):** 1x VPC Endpoint ≈ **$5.00**

**Tổng chi phí ước tính hàng tháng: ~$68.00**

---

## 8. Kết quả mong đợi

- **Tỷ lệ thành công:** Tăng tỷ lệ cai thuốc nhờ kế hoạch có cấu trúc và giám sát.
- **Sự gắn kết:** Tăng động lực người dùng thông qua game hóa và hỗ trợ cộng đồng.
- **Khả năng mở rộng:** Nền tảng vững chắc hỗ trợ lượng lớn người dùng tăng trưởng.
- **Bảo mật:** Cơ sở hạ tầng tuân thủ, bảo vệ dữ liệu sức khỏe nhạy cảm.
