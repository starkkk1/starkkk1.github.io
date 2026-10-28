---
title: "Nhật ký công việc tuần 4"
date: 2026-10-16
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

# Nhật ký công việc tuần 04 - Quản lý danh tính và quyền truy cập AWS (IAM)

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 04 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Nắm rõ người dùng, vai trò (role), chính sách (policy) và thông tin xác thực tạm thời.
- Chuẩn bị quyền truy cập từ EC2 đến artifacts mô hình mà không cần nhúng access key.
- Áp dụng nguyên tắc đặc quyền tối thiểu (least privilege) cho quyền lưu trữ và giám sát.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Rà soát các danh tính IAM, policy actions và ARN tài nguyên. Phân tách quyền của người vận hành với quyền của workload EC2. | Tuần 04 Ngày 1 | Tuần 04 Ngày 1 | Thực hành tốt nhất cho AWS IAM |
| 2 | Rà soát bảo vệ tài khoản root và xác thực đa yếu tố (MFA). Xác định quy trình truy cập hàng ngày tránh sử dụng root credentials cho các thao tác dự án. | Tuần 04 Ngày 2 | Tuần 04 Ngày 2 | Thực hành tốt nhất cho AWS IAM |
| 3 | Chuẩn bị service role và instance profile cho EC2. Rà soát trust policy cho phép EC2 nhận temporary credentials từ AWS. | Tuần 04 Ngày 3 | Tuần 04 Ngày 3 | Thực hành tốt nhất cho AWS IAM |
| 4 | Giới hạn quyền đọc S3 đúng prefix chứa artifacts mô hình cần thiết. Tách quyền upload artifacts khỏi quyền tải về trên EC2. | Tuần 04 Ngày 4 | Tuần 04 Ngày 4 | Thực hành tốt nhất cho AWS IAM |
| 5 | Rà soát quyền CloudWatch để xuất metrics, gửi log và đọc log. Ghi nhận lý do logs:FilterLogEvents tách biệt với quyền gửi log thông thường. | Tuần 04 Ngày 5 | Tuần 04 Ngày 5 | Hướng dẫn CloudWatch Logs |

### Kết quả đạt được:
- Chuẩn bị thiết kế IAM role cho phép EC2 truy cập an toàn vào tài nguyên AWS.
- Xác định quyền truy cập artifacts S3 giới hạn và quyền giám sát phù hợp.
- Làm rõ sự khác biệt giữa thông tin xác thực của workload và quyền của người vận hành.
