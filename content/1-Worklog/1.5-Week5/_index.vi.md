---
title: "Nhật ký công việc tuần 5"
date: 2026-10-16
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

# Nhật ký công việc tuần 05 - Lưu trữ artifacts mô hình trên Amazon S3

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 05 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Lưu trữ mô hình và ánh xạ nhãn đã chọn trong S3 bucket riêng tư (private).
- Sử dụng object prefix nhất quán và xác minh tính toàn vẹn của artifacts.
- Rà soát quyền lưu trữ và cấu trúc tổ chức bản phát hành mô hình.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Tạo S3 bucket cho dự án trong Region đã chọn. Kiểm tra cài đặt Block Public Access và mã hóa trước khi tải lên bất kỳ artifact nào. | Tuần 05 Ngày 1 | Tuần 05 Ngày 1 | Hướng dẫn sử dụng Amazon S3 |
| 2 | Cấu trúc bản phát hành mô hình dưới prefix mobilenet-distilled-v1/. Giữ checkpoint và class mapping cùng nhau như một bộ artifact tương thích. | Tuần 05 Ngày 2 | Tuần 05 Ngày 2 | Hướng dẫn sử dụng Amazon S3 |
| 3 | Tải lên model.pth và class_to_idx.json. Rà soát tên đối tượng, kích thước và metadata để đảm bảo đúng file dự định. | Tuần 05 Ngày 3 | Tuần 05 Ngày 3 | Hướng dẫn sử dụng Amazon S3 |
| 4 | Đối chiếu tên artifact đã lưu với resource prefix dự định cấp trong IAM policy. Xem xét cách xử lý khi sai key hoặc thiếu quyền gây lỗi truy cập. | Tuần 05 Ngày 4 | Tuần 05 Ngày 4 | Thực hành tốt nhất cho AWS IAM |
| 5 | Tải về bộ artifact để kiểm tra tính toàn vẹn và so sánh mã băm SHA256. Tìm hiểu tính năng versioning như một tùy chọn để lưu giữ các artifacts mô hình phiên bản trước. | Tuần 05 Ngày 5 | Tuần 05 Ngày 5 | Hướng dẫn sử dụng Amazon S3 |

### Kết quả đạt được:
- Tổ chức lưu trữ artifacts mô hình trong Amazon S3 riêng tư.
- Thiết lập prefix nhất quán cho bản phát hành mô hình đã chọn.
- Chuẩn bị quy trình đối chiếu checksum để xác minh tính toàn vẹn của artifacts tải về.
