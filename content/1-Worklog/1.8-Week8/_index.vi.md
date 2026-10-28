---
title: "Nhật ký công việc tuần 8"
date: 2026-10-16
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

# Nhật ký công việc tuần 08 - Kết nối Amazon EC2 với Amazon S3

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 08 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Truy xuất bộ artifacts mô hình thông qua IAM role của EC2.
- Xử lý các sự cố về thông tin xác thực và quyền truy cập đối tượng.
- Xác minh việc thực thi mô hình thật trong môi trường cloud.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Xác nhận instance profile đã được gắn vào EC2. Kiểm tra danh tính AWS đang hoạt động và Region mà không cần đặt access key vĩnh viễn trên instance. | Tuần 08 Ngày 1 | Tuần 08 Ngày 1 | Thực hành tốt nhất cho AWS IAM |
| 2 | Tải model.pth và class_to_idx.json từ prefix mobilenet-distilled-v1/. Sử dụng chính xác object key và lưu vào thư mục checkpoint riêng. | Tuần 08 Ngày 2 | Tuần 08 Ngày 2 | Hướng dẫn sử dụng Amazon S3 |
| 3 | Rà soát các lỗi thiếu thông tin xác thực và lỗi S3 access denied. Kiểm tra assumed role, object key và phạm vi tài nguyên trong policy trước khi sửa đổi quyền. | Tuần 08 Ngày 3 | Tuần 08 Ngày 3 | Thực hành tốt nhất cho AWS IAM |
| 4 | Tính toán mã băm SHA256 trên EC2 và đối chiếu với manifest artifacts gốc. Giữ checkpoint và file ánh xạ nhãn luôn đi đôi với nhau. | Tuần 08 Ngày 4 | Tuần 08 Ngày 4 | Hướng dẫn sử dụng Amazon S3 |
| 5 | Tải mô hình đã xác minh lên EC2 và chạy thử nghiệm dự đoán ảnh lá cà chua. Rà soát các nhãn lớp trả về và ghi nhận phiên bản mô hình. | Tuần 08 Ngày 5 | Tuần 08 Ngày 5 | Hướng dẫn bắt đầu với Amazon EC2 |

### Kết quả đạt được:
- Kết nối EC2 với lưu trữ mô hình S3 riêng tư thông qua quyền theo vai trò (role-based access).
- Xác minh tính toàn vẹn của checkpoint và ánh xạ lớp tương ứng trên EC2.
- Chạy thành công dự đoán mẫu bằng mô hình thật trên môi trường cloud.
