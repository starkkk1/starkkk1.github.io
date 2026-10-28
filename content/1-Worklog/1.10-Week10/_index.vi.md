---
title: "Nhật ký công việc tuần 10"
date: 2026-10-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

# Nhật ký công việc tuần 10 - Thu thập metrics CloudWatch và cảnh báo RAM

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 10 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Thu thập các chỉ số về bộ nhớ (RAM) và ổ đĩa từ máy chủ EC2.
- Xây dựng giao diện giám sát tài nguyên tập trung (dashboard).
- Cấu hình một cảnh báo (alarm) RAM với hành động gửi thông báo qua SNS.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | So sánh sự khác nhau giữa metrics chuẩn của EC2 và host metrics do CloudWatch Agent thu thập. Xác định nhu cầu giám sát RAM và dung lượng đĩa. | Tuần 10 Ngày 1 | Tuần 10 Ngày 1 | Hướng dẫn CloudWatch Agent |
| 2 | Cài đặt và cấu hình CloudWatch Agent trên máy chủ EC2. Xuất dữ liệu bộ nhớ và ổ đĩa dưới namespace PlantDisease/Host với chu kỳ 60 giây. | Tuần 10 Ngày 2 | Tuần 10 Ngày 2 | Hướng dẫn CloudWatch Agent |
| 3 | Kiểm tra đồ thị CPU, bộ nhớ và đĩa cho đúng instance. Xem xét chiều metric (dimension), thống kê và chu kỳ thu thập trước khi tạo dashboard. | Tuần 10 Ngày 3 | Tuần 10 Ngày 3 | Hướng dẫn CloudWatch Agent |
| 4 | Cấu hình một alarm RAM: Mức sử dụng bộ nhớ trung bình trên 85% trong 3 chu kỳ 1 phút liên tiếp. Xem xét cách dữ liệu thiếu ảnh hưởng tới trạng thái alarm. | Tuần 10 Ngày 4 | Tuần 10 Ngày 4 | Hướng dẫn CloudWatch Alarms |
| 5 | Chuẩn bị hành động thông báo qua email bằng SNS và quy trình xác nhận đăng ký. Xem xét quy trình kiểm thử hạ ngưỡng có kiểm soát và khôi phục về ngưỡng vận hành thực tế. | Tuần 10 Ngày 5 | Tuần 10 Ngày 5 | Hướng dẫn CloudWatch Alarms |

### Kết quả đạt được:
- Xây dựng giao diện giám sát CloudWatch cho tài nguyên máy chủ EC2.
- Thiết lập một alarm RAM với ngưỡng và cửa sổ đánh giá rõ ràng.
- Ghi nhận quy trình thiết lập thông báo và kiểm thử cảnh báo có kiểm soát.
