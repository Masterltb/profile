---
title: "Nhật ký Tuần 11"
date: ""
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---



### Mục tiêu Tuần 11

- Triển khai logic phân biệt **Lỡ (Slip)** và **Tái nghiện (Relapse)**.
- Phát triển bộ lập lịch **Đánh giá Hàng tuần** (Weekly Assessment - chu kỳ 7 ngày).
- Xây dựng **Admin APIs** cho các thao tác CRUD trên Quiz Templates.

### Các công việc thực hiện trong tuần

| Ngày | Công việc                                                                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                              |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------- |
| 2    | - **Tính năng:** Triển khai logic **Slip**: Báo cáo lỡ -> Cho phép khôi phục ngay, không reset.<br>- **Tính năng:** Triển khai logic **Relapse**: Báo cáo tái nghiện -> Hard reset về 0 ngay lập tức. | 17/11/2025   | 17/11/2025      | —                                               |
| 3    | - **Bổ sung Flyway:** Thêm Flyway các bảng còn thiếu , thêm thuộc tính (cột) .<br>- **Logic:** Cải thiện luồng , bổ sung các thuộc tính cần thiết cho đối tượng.                                      | 18/11/2025   | 18/11/2025      | <https://spring.io/guides/gs/scheduling-tasks/> |
| 4    | - **Admin API:** Xây dựng REST endpoints cho Admin thực hiện Create/Read/Update/Delete **Quiz Templates**.<br>- **Validate:** Thêm logic kiểm tra để đảm bảo quiz có câu hỏi và đáp án hợp lệ.        | 19/11/2025   | 19/11/2025      | —                                               |
| 5    | - **Tối ưu hóa:** Tối ưu logic bộ đếm reset (Tối đa 3 lần khôi phục).<br>- **Logic:** Đảm bảo lần khôi phục thứ 4 sẽ kích hoạt hard reset bất kể kết quả quiz.                                        | 20/11/2025   | 20/11/2025      | —                                               |
| 6    | - **Tích hợp:** Kết nối kết quả Quiz Hàng tuần vào Hồ sơ Người dùng để theo dõi tiến độ dài hạn.<br>- **Refactor:** Tái cấu trúc `QuizService` để xử lý cả hai loại quiz "Khôi phục" và "Hàng tuần".  | 21/11/2025   | 21/11/2025      | —                                               |

### Kết quả đạt được Tuần 11

- Logic **Slip vs Relapse** đã hoàn thiện, thực thi nghiêm ngặt các quy tắc khôi phục của chương trình.
- Hệ thống **Đánh giá Hàng tuần** được tự động hóa, đảm bảo giám sát liên tục tiến độ người dùng.
- **Module Admin** quản lý Quiz đã hoàn tất, cho phép cập nhật nội dung động mà không cần sửa code.
- **Giới hạn Khôi phục** (Tối đa 3 lần) được thực thi thành công, ngăn chặn lạm dụng hệ thống.

**Bài học rút ra:**

- **Lập lịch (Scheduling):** Sử dụng Spring Scheduler cho các tác vụ định kỳ so với external cron jobs.
- **Đa hình trong Service:** Thiết kế `QuizService` để xử lý các ngữ cảnh quiz khác nhau (Khôi phục vs Hàng tuần) một cách hiệu quả.
- **Hard vs Soft Deletes:** Xử lý "Relapse" (Hard Reset) khác biệt rõ ràng với "Smoke Event" (Soft Reset).
