---
title: "Nhật ký công việc tuần 6"
date: 2026-10-16
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

# Nhật ký công việc tuần 06 - Amazon VPC và kết nối mạng

**Dự án:** Nhận diện bệnh lá cà chua trên AWS  
**Giai đoạn:** Tuần 06 ngày làm việc 1 đến 5

### Mục tiêu tuần:
- Nắm rõ luồng mạng giữa Internet, EC2 và S3.
- Chọn subnet phù hợp và rà soát định tuyến mạng.
- Giới hạn truy cập chiều vào (inbound) bằng Security Groups.

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Rà soát các khái niệm VPC, subnet, route table và Internet Gateway. Vẽ sơ đồ luồng mạng cần thiết để tiếp cận instance EC2 của dự án. | Tuần 06 Ngày 1 | Tuần 06 Ngày 1 | Hướng dẫn sử dụng Amazon VPC |
| 2 | Kiểm tra cấu hình VPC và subnet hiện có. Tái sử dụng một public subnet phù hợp sau khi kiểm tra bảng định tuyến và cấu hình public IPv4. | Tuần 06 Ngày 2 | Tuần 06 Ngày 2 | Hướng dẫn sử dụng Amazon VPC |
| 3 | Rà soát kết nối chiều ra (outbound) để lấy artifacts mô hình từ S3. So sánh tuyến đường Internet hiện tại với giải pháp tùy chọn qua S3 gateway endpoint. | Tuần 06 Ngày 3 | Tuần 06 Ngày 3 | Hướng dẫn sử dụng Amazon S3 |
| 4 | Chuẩn bị các quy tắc Security Group cho truy cập quản trị. Giới hạn cổng SSH 22 cho IP công khai của người vận hành và phân biệt quy tắc inbound với outbound. | Tuần 06 Ngày 4 | Tuần 06 Ngày 4 | Hướng dẫn bắt đầu với Amazon EC2 |
| 5 | Rà soát các quy tắc truy cập web dự kiến và các bước xử lý lỗi kết nối cơ bản. Ghi nhận mối quan hệ giữa địa chỉ instance, bảng định tuyến và Security Group. | Tuần 06 Ngày 5 | Tuần 06 Ngày 5 | Hướng dẫn sử dụng Amazon VPC |

### Kết quả đạt được:
- Vẽ sơ đồ định tuyến VPC cần thiết cho bản demo trên cloud.
- Chọn subnet phù hợp và thiết lập giới hạn truy cập SSH an toàn.
- Ghi nhận trình tự khắc phục sự cố mạng và định hướng cải tiến với S3 endpoint.
