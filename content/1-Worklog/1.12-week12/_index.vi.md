---
title: "Nhật ký Tuần 12"
date: ""
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---



### Mục tiêu Tuần 12

- Thực hiện **Unit Testing** & **Integration Testing** toàn diện.
- Tối ưu hóa **PostgreSQL Queries** để tăng hiệu năng.
- Hoàn thiện tài liệu và chuẩn bị bàn giao deployment.

### Các công việc thực hiện trong tuần

| Ngày | Công việc                                                                                                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                           |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------ |
| 2    | - **Kiểm thử:** Viết **API test** test cho các logic nghiệp vụ quan trọng (Tính toán Streak, Giới hạn Reset).<br>- **Mục tiêu:** Đạt tiêu chí `StreakService`.                                                                      | 24/11/2025   | 24/11/2025      | <https://site.mockito.org/>                                  |
| 3    | - **Kiểm thử:** Thực hiện **Integration Tests** sử dụng `@SpringBootTest` để xác minh luồng đầy đủ: Sự kiện hút thuốc -> Quiz -> Khôi phục Streak.<br>- **Xác nhận:** Kiểm tra tính nhất quán dữ liệu sau khi rollback transaction. | 25/11/2025   | 25/11/2025      | —                                                            |
| 4    | - **Tối ưu hóa:** Phân tích các truy vấn chậm bằng `EXPLAIN ANALYZE` trong PostgreSQL.<br>- **Hành động:** Thêm index cho cột `user_id` và `created_at` trong bảng `DailyRoutine` để tăng tốc tính toán streak.                     | 26/11/2025   | 26/11/2025      | <https://www.postgresql.org/docs/current/using-explain.html> |
| 5    | - **Tài liệu:** Tạo tài liệu API sử dụng **Swagger/OpenAPI/API Document**.<br>- **Báo cáo:** Hoàn thiện báo cáo thực tập tóm tắt kiến trúc backend và các luồng xử lý logic.                                                        | 27/11/2025   | 27/11/2025      | <https://swagger.io/>                                        |
| 6    | - **Rà soát:** Dọn dẹp code lần cuối và refactor.<br>- **Thuyết trình:** Chuẩn bị trình bày logic backend `program-service` mentor.                                                                                                 | 28/11/2025   | 28/11/2025      | —                                                            |

### Kết quả đạt được Tuần 12

- **Code chất lượng cao:** Xác nhận tính ổn định của logic cốt lõi thông qua các bài test Unit và Integration mở rộng.
- **Tối ưu hiệu năng:** Cải thiện hiệu năng truy vấn cơ sở dữ liệu cho việc truy xuất thống kê người dùng.
- **Tài liệu:** Bàn giao tài liệu API rõ ràng phục vụ tích hợp Frontend.
- **Hoàn thành:** Xây dựng và bàn giao thành công backend cho **Nền tảng Hỗ trợ Cai thuốc lá**.

**Tổng kết kỳ thực tập:**
Trong 4 tuần cuối, tôi đã thiết kế kiến trúc và triển khai backend cốt lõi cho "Program Service" từ con số 0 sử dụng Java 25 và Spring Boot. Tôi đã chuyển đổi thành công các yêu cầu nghiệp vụ phức tạp (Mức độ nghiện, Logic khôi phục) thành mã nguồn, dễ kiểm thử.
