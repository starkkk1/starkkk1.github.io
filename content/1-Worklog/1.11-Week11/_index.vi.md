---
title: "Nhật ký công việc tuần 11"
date: 2026-10-16
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

# Nhật ký công việc tuần 11 - Bảo mật truy cập công khai cho ứng dụng EC2

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 11 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Kết nối tên miền hiện có tới địa chỉ công khai của EC2.
- Kích hoạt truy cập HTTPS trong khi vẫn duy trì lớp bảo vệ tên miền.
- Giảm thiểu các cổng mở không cần thiết thông qua Security Groups.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Rà soát luồng DNS cho tên miền caycachua.id.vn và xác định nơi quản lý bản ghi. Phân biệt giữa nhà đăng ký tên miền và đơn vị cung cấp DNS có thẩm quyền. | Tuần 11 Ngày 1 | Tuần 11 Ngày 1 | Hướng dẫn SSL và TLS từ iNET |
| 2 | Trỏ bản ghi A gốc về địa chỉ public IPv4 hiện tại của EC2. Tìm hiểu lý do tại sao DNS có bảo vệ có thể trả về địa chỉ proxy thay vì địa chỉ gốc của máy chủ. | Tuần 11 Ngày 2 | Tuần 11 Ngày 2 | Hướng dẫn SSL và TLS từ iNET |
| 3 | Rà soát yêu cầu truy cập HTTP và HTTPS trên EC2. Mở cổng 80 và 443 trong khi vẫn giới hạn cổng SSH cho địa chỉ IP của người vận hành. | Tuần 11 Ngày 3 | Tuần 11 Ngày 3 | Hướng dẫn bắt đầu với Amazon EC2 |
| 4 | Rà soát chứng chỉ tên miền và kết nối HTTPS đến EC2. Phân biệt chứng chỉ cho người truy cập với chứng chỉ gốc và chế độ Full Strict. | Tuần 11 Ngày 4 | Tuần 11 Ngày 4 | Hướng dẫn SSL và TLS từ iNET |
| 5 | Xóa bỏ các quy tắc mở cổng ứng dụng trực tiếp không cần thiết sau khi đã truy cập thành công qua tên miền. Xác minh bản demo mô hình vẫn truy cập tốt qua tên miền bảo vệ. | Tuần 11 Ngày 5 | Tuần 11 Ngày 5 | Hướng dẫn bắt đầu với Amazon EC2 |

### Kết quả đạt được:
- Kết nối thành công tên miền của dự án tới bản demo chạy trên EC2.
- Thiết lập truy cập HTTPS và kiểm tra việc xác thực chứng chỉ tại máy chủ gốc.
- Giảm thiểu bề mặt tấn công công khai bằng cách loại bỏ các quy tắc cổng ứng dụng thừa.
