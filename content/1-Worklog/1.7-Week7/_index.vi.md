---
title: "Nhật ký công việc tuần 7"
date: 2026-10-16
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# Nhật ký công việc tuần 07 - Khởi tạo và kết nối máy chủ Amazon EC2

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 07 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Khởi tạo một instance EC2 phù hợp cho việc suy luận mô hình bằng CPU.
- Chuẩn bị ổ đĩa lưu trữ và thiết lập truy cập quản trị an toàn.
- Nắm rõ các kiểm tra trạng thái (status check) và vòng đời của instance.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Rà soát các loại instance x86_64 hiện có, yêu cầu bộ nhớ và tính khả dụng theo Region. Chọn tùy chọn phù hợp trong phạm vi ngân sách dự án. | Tuần 07 Ngày 1 | Tuần 07 Ngày 1 | Hướng dẫn bắt đầu với Amazon EC2 |
| 2 | Khởi chạy instance Amazon Linux 2023 với ổ lưu trữ EBS. Gắn subnet đã chọn, Security Group và EC2 instance profile. | Tuần 07 Ngày 2 | Tuần 07 Ngày 2 | Hướng dẫn bắt đầu với Amazon EC2 |
| 3 | Chuẩn bị SSH key trên Windows và phân quyền bảo vệ cho file private key. Kết nối bằng đúng tài khoản người dùng và địa chỉ public IPv4 hiện tại. | Tuần 07 Ngày 3 | Tuần 07 Ngày 3 | Hướng dẫn bắt đầu với Amazon EC2 |
| 4 | Kiểm tra thông tin hệ điều hành, RAM trống, dung lượng CPU và dung lượng ổ đĩa. Xác nhận instance status check đạt trạng thái tốt trước khi chuẩn bị file mô hình. | Tuần 07 Ngày 4 | Tuần 07 Ngày 4 | Hướng dẫn bắt đầu với Amazon EC2 |
| 5 | Nắm rõ các thao tác Start, Stop, Reboot và Terminate. Ghi nhận tài nguyên nào được giữ lại sau khi Stop và lý do địa chỉ public IPv4 có thể thay đổi sau khi Start. | Tuần 07 Ngày 5 | Tuần 07 Ngày 5 | Hướng dẫn dừng và khởi động EC2 |

### Kết quả đạt được:
- Chuẩn bị môi trường tính toán EC2 cho suy luận mô hình.
- Thiết lập truy cập SSH an toàn và đánh giá dung lượng tài nguyên máy chủ.
- Ghi nhận các thao tác vòng đời máy chủ cùng tác động tới lưu trữ và địa chỉ IP.
