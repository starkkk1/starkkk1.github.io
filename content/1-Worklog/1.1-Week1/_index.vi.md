---
title: "Nhật ký công việc tuần 1"
date: 2026-10-16
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

# Nhật ký công việc tuần 01 - Chuẩn bị dữ liệu

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 01 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Chuẩn bị ảnh lá cà chua và các nhãn lớp nhất quán.
- Tạo các phân vùng huấn luyện (training), kiểm định (validation) và kiểm thử (test) có thể tái tạo.
- Xác định các vấn đề về chất lượng dữ liệu trước khi huấn luyện.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Rà soát tập dữ liệu lá cà chua, các lớp được hỗ trợ và phạm vi dự án. Ghi nhận nguồn và mục đích của từng phân vùng dữ liệu. | Tuần 01 Ngày 1 | Tuần 01 Ngày 1 | Tài liệu tập dữ liệu và mô hình dự án |
| 2 | Kiểm tra khả năng đọc ảnh, định dạng file và các thư mục lớp. Xác định các file bị hỏng và nhãn không nhất quán để loại trừ hoặc chỉnh sửa. | Tuần 01 Ngày 2 | Tuần 01 Ngày 2 | Tài liệu tập dữ liệu và mô hình dự án |
| 3 | Rà soát độ cân bằng lớp và các ảnh trùng lặp. Giữ các mẫu liên quan hoặc trùng lặp trong cùng một phân vùng để giảm rò rỉ dữ liệu (data leakage). | Tuần 01 Ngày 3 | Tuần 01 Ngày 3 | Tài liệu tập dữ liệu và mô hình dự án |
| 4 | Chuẩn bị phân chia tập train, validation và test. Xác định kích thước ảnh, chuẩn hóa và augmentation cho tập train trong khi giữ nhất quán tiền xử lý đánh giá. | Tuần 01 Ngày 4 | Tuần 01 Ngày 4 | Tài liệu tập dữ liệu và mô hình dự án |
| 5 | Tạo ánh xạ chỉ số lớp (class index mapping) và bản tóm tắt dữ liệu. Xem lại các ảnh mẫu từ từng phân vùng và ghi nhận các giới hạn chất lượng còn lại. | Tuần 01 Ngày 5 | Tuần 01 Ngày 5 | Tài liệu tập dữ liệu và mô hình dự án |

### Kết quả đạt được:
- Tổ chức tập dữ liệu lá cà chua có nhãn với các phân vùng đánh giá riêng biệt.
- Chuẩn bị tiền xử lý nhất quán và bộ ánh xạ chỉ số lớp có thể tái sử dụng.
- Ghi nhận các kiểm tra chất lượng dữ liệu và các nguồn thiên lệch tiềm ẩn.
