---
title: "Nhật ký công việc tuần 2"
date: 2026-10-16
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# Nhật ký công việc tuần 02 - Huấn luyện mô hình và chuẩn bị artifacts

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 02 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Huấn luyện và đánh giá mô hình phân loại lá cà chua gọn nhẹ.
- Chuẩn bị các file mô hình phù hợp để suy luận bằng CPU trên Amazon EC2.
- Ghi nhận định danh mô hình mà không dựa vào các khẳng định độ chính xác chưa được chứng minh.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Rà soát cấu hình MobileNetV3 Small và khởi tạo môi trường huấn luyện. Kiểm tra kích thước đầu ra của mô hình khớp với ánh xạ chỉ số lớp. | Tuần 02 Ngày 1 | Tuần 02 Ngày 1 | Tài liệu tập dữ liệu và mô hình dự án |
| 2 | Chạy thử nghiệm huấn luyện ban đầu. Rà soát loss huấn luyện và kiểm định để phát hiện lỗi cấu hình và dấu hiệu quá khớp (overfitting). | Tuần 02 Ngày 2 | Tuần 02 Ngày 2 | Tài liệu tập dữ liệu và mô hình dự án |
| 3 | Rà soát mô hình học sinh (student model) gọn nhẹ và quy trình chưng cất tri thức (knowledge distillation). So sánh các checkpoint ứng viên trên cùng phân vùng kiểm định và tiền xử lý. | Tuần 02 Ngày 3 | Tuần 02 Ngày 3 | Tài liệu tập dữ liệu và mô hình dự án |
| 4 | Đánh giá checkpoint được chọn trên tập ảnh kiểm thử độc lập. Kiểm tra lỗi theo từng lớp và xác minh suy luận CPU với ánh xạ lớp tương ứng. | Tuần 02 Ngày 4 | Tuần 02 Ngày 4 | Tài liệu tập dữ liệu và mô hình dự án |
| 5 | Xuất checkpoint đã chọn thành model.pth và ánh xạ nhãn thành class_to_idx.json. Tính mã băm SHA256 cho việc kiểm tra toàn vẹn trên cloud sau này. | Tuần 02 Ngày 5 | Tuần 02 Ngày 5 | Tài liệu tập dữ liệu và mô hình dự án |

### Kết quả đạt được:
- Chuẩn bị checkpoint mô hình gọn nhẹ và ánh xạ lớp tương thích.
- Rà soát hành vi mô hình và khả năng suy luận CPU trước khi triển khai lên cloud.
- Tạo các artifacts có định danh rõ ràng để lưu trữ và truy xuất trên Amazon S3.
