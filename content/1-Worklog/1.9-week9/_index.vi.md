---
title: "Nhật ký Tuần 9"
date: ""
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---



### Mục tiêu Tuần 9

- Khởi tạo **Program Service** sử dụng Java 25 và Spring Boot.
- Thiết kế và triển khai schema cơ sở dữ liệu PostgreSQL cho Programs và Quizzes.
- Triển khai logic Bài đánh giá ban đầu (Initial Assessment) và Gợi ý gói (Recommendation).

### Các công việc thực hiện trong tuần

| Ngày | Công việc                                                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                    |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------------------------- |
| 2    | - **Thiết lập:** Khởi tạo dự án Spring Boot với Java 25.<br>- **Thiết kế DB:** Thiết kế ERD cho các bảng `Users`, `Programs`, `QuizTemplates`, và `QuizResults`.                        | 03/11/2025   | 03/11/2025      | <https://start.spring.io/>                                            |
| 3    | - **Triển khai:** Code `MeQuizController` để xử lý bài quiz đầu vào của người dùng.<br>- **Logic:** Viết logic phân loại mức độ nghiện (Nhẹ, Trung bình, Nặng) dựa trên điểm số quiz.   | 04/11/2025   | 04/11/2025      | —                                                                     |
| 4    | - **Cơ sở dữ liệu:** Seed dữ liệu `ProgramTemplates` (Gói Free/Trả phí) vào PostgreSQL.<br>- **Tính năng:** Phát triển Engine Gợi ý để đề xuất 3 gói phù hợp dựa trên level người dùng. | 05/11/2025   | 05/11/2025      | <https://docs.spring.io/spring-data/jpa/docs/current/reference/html/> |
| 5    | - **API:** Xây dựng API cho Đăng ký Lộ trình (Enrollment).<br>- **Bảo mật:** Tích hợp JWT authentication filter để bảo mật API.                                                         | 06/11/2025   | 06/11/2025      | <https://spring.io/guides/gs/securing-web/>                           |
| 6    | - **Kiểm thử:** Test API bằng Postman cho các luồng Đánh giá và Đăng ký.<br>- **Rà soát:** Code review đảm bảo sử dụng đúng các tính năng mới của Java 25.                              | 07/11/2025   | 07/11/2025      | —                                                                     |

### Kết quả đạt được Tuần 9

- Thiết lập thành công kiến trúc nền tảng cho **Program Service**.
- Hoàn thành triển khai **Database Schema** trên PostgreSQL.
- Hệ thống **Đánh giá & Gợi ý** đã hoạt động, cho phép phân loại người dùng và đề xuất lộ trình.
- Luồng **Đăng ký người dùng** đã hoạt động, liên kết người dùng với gói lộ trình đã chọn.

**Bài học rút ra:**

- **Java 25:** Áp dụng các tính năng ngôn ngữ mới giúp code gọn gàng và hiệu quả hơn.
- **Ánh xạ nghiệp vụ:** Chuyển đổi các tiêu chuẩn y tế phức tạp (mức độ nghiện) thành logic code điều kiện.
- **Chiến lược Seeding:** Tầm quan trọng của việc khởi tạo dữ liệu mẫu cho các template tĩnh như Programs.
