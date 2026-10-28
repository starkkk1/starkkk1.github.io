---
title: "Nhật ký công việc tuần 9"
date: 2026-10-16
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

# Nhật ký công việc tuần 09 - Quản lý nhật ký với Amazon CloudWatch Logs

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 09 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Thu thập nhật ký vận hành từ workload chạy trên EC2.
- Tổ chức các nhóm nhật ký (log group) và rà soát cài đặt thời gian lưu giữ (retention).
- Khắc phục sự cố truy cập vào log trên cloud.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Tìm hiểu về CloudWatch log groups, log streams và log events. Xác định các sự kiện khởi tạo mô hình và suy luận hữu ích cho việc chẩn đoán lỗi. | Tuần 09 Ngày 1 | Tuần 09 Ngày 1 | Hướng dẫn CloudWatch Logs |
| 2 | Cấu hình workload gửi dữ liệu nhật ký vận hành về CloudWatch Logs. Sử dụng log group và log stream có tên gọi rõ ràng cho môi trường dự án. | Tuần 09 Ngày 2 | Tuần 09 Ngày 2 | Hướng dẫn CloudWatch Logs |
| 3 | Rà soát quyền gửi log trên EC2 role. Phân biệt giữa quyền đẩy log vào hệ thống và quyền tìm kiếm, đọc các sự kiện log đã lưu. | Tuần 09 Ngày 3 | Tuần 09 Ngày 3 | Thực hành tốt nhất cho AWS IAM |
| 4 | Kiểm tra các thông điệp khởi tạo và log suy luận mẫu. Sử dụng khoảng thời gian và bộ lọc để định vị sự cố mà không để lộ credentials hoặc hình ảnh riêng tư. | Tuần 09 Ngày 4 | Tuần 09 Ngày 4 | Hướng dẫn CloudWatch Logs |
| 5 | Rà soát mục tiêu lưu giữ 7 ngày và chi phí lưu trữ log. Ghi nhận quyền logs:FilterLogEvents giới hạn cần thiết để tìm kiếm log từ giao diện dòng lệnh. | Tuần 09 Ngày 5 | Tuần 09 Ngày 5 | Hướng dẫn CloudWatch Logs |

### Kết quả đạt được:
- Tổ chức dữ liệu log vận hành trong CloudWatch cho workload trên EC2.
- Xây dựng phương pháp lặp lại được để tra cứu các sự kiện log trên cloud.
- Rà soát thời gian lưu giữ log và các quyền cần thiết để đọc dữ liệu log.
