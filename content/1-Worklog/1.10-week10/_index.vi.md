---
title: "Nhật ký Tuần 10"
date: ""
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---



### Mục tiêu Tuần 10

- Triển khai tính năng cốt lõi **Nhiệm vụ Hàng ngày** (Daily Routine - 4 bước/ngày).
- Xây dựng hệ thống **Theo dõi Chuỗi** (Streak Tracking - Chuỗi hiện tại, Chuỗi tốt nhất).
- Phát triển logic xử lý **Sự kiện Hút thuốc** (Smoke Events - Reset chờ khôi phục).

### Các công việc thực hiện trong tuần

| Ngày | Công việc                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo          |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------- |
| 2    | - **Tính năng:** Thiết kế entity `StepTemplate`.<br>- **Logic:** Triển khai logic sinh ra 4 bước nhiệm vụ khác nhau mỗi ngày cho người dùng đã đăng ký.                                           | 10/11/2025   | 10/11/2025      | —                           |
| 3    | - **API:** Xây dựng API cho phép người dùng đánh dấu hoàn thành các bước.<br>- **Logic:** Triển khai **Streak Service** để tự động tăng chuỗi khi hoàn thành đủ 4 bước.                           | 11/11/2025   | 11/11/2025      | —                           |
| 4    | - **Tính năng:** Phát triển trigger **Sự kiện Hút thuốc**.<br>- **Logic:** Triển khai logic "Soft Reset": Khi có sự kiện hút thuốc, chuỗi bị tạm dừng (Trạng thái: PENDING_RECOVERY).             | 12/11/2025   | 12/11/2025      | —                           |
| 5    | - **Logic Khôi phục:** Triển khai logic gán "Bài Quiz Khôi phục".<br>- **Validate:** Nếu người dùng vượt qua quiz (Tối đa 3 lần), khôi phục chuỗi; ngược lại, reset về 0.                         | 13/11/2025   | 13/11/2025      | —                           |
| 6    | - **Kiểm thử:** Unit test logic Tính toán Streak bao phủ các trường hợp biên (qua đêm, múi giờ).<br>- **Tích hợp:** Xác minh luồng từ Nhiệm vụ Hàng ngày -> Cập nhật Streak -> Sự kiện Hút thuốc. | 14/11/2025   | 14/11/2025      | <https://junit.org/junit5/> |

### Kết quả đạt được Tuần 10

- Engine **Nhiệm vụ Hàng ngày** đã hoạt động, thúc đẩy tương tác người dùng.
- **Hệ thống Streak** theo dõi chính xác tính nhất quán và cập nhật "Best Streak".
- **Cơ chế Khôi phục** xử lý thành công "Sự kiện Hút thuốc", cho phép người dùng cơ hội khôi phục chuỗi (giới hạn 3 lần) thông qua bài quiz.

**Bài học rút ra:**

- **Quản lý Trạng thái:** Quản lý các trạng thái phức tạp của người dùng (Active, Pending Recovery, Reset) đòi hỏi logic máy trạng thái (state machine) vững chắc.
- **Logic Thời gian:** Xử lý reset "hàng ngày" cần cẩn trọng với thời gian server so với múi giờ người dùng.
- **Tính toàn vẹn giao dịch:** Đảm bảo cập nhật streak và hoàn thành nhiệm vụ diễn ra đồng bộ (atomically).
