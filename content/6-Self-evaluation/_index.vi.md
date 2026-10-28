---
title: "Tự đánh giá"
date: 2026-10-16
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

# Tự đánh giá


### 1 Năng lực đã thể hiện

Quá trình thực hiện thể hiện việc tách phát hành image khỏi deployment, xác minh tính toàn vẹn artifacts, dùng IAM role, cấu hình Docker runtime và tích hợp CloudWatch. Các lỗi thực tế gồm ACL SSH key trên Windows, thiếu credentials từ instance role, sai prefix hoặc quyền object S3 và thiếu quyền đọc log đã được xử lý. Đây là năng lực vận hành backend; chưa phải bằng chứng của nền tảng production được quản lý hoàn chỉnh.


### 2 Bài học xử lý lỗi

| Triệu chứng | Nguyên nhân và cách xử lý |
| --- | --- |
| UNPROTECTED PRIVATE KEY FILE | Giới hạn ACL key trên Windows; dùng đúng key pair EC2 |
| Unable to locate credentials | Gắn instance role; kiểm tra danh tính STS |
| 403 HeadObject | Kiểm tra đúng S3 key và prefix trong quyền GetObject |
| AccessDenied FilterLogEvents | Thêm quyền đọc giới hạn cho log ứng dụng |
| Mở trang nhưng không nhận diện | Kiểm tra port API và CORS_ORIGINS; tạo lại container khi đổi cấu hình |


### 3 Giới hạn và kiểm tra tiếp theo

Một EC2 là một điểm lỗi duy nhất. Demo HTTP chưa có xác thực ứng dụng hoặc load balancer. Alarm RAM không chứng minh availability endpoint hay độ chính xác model. Inference và explanation dùng chung một slot mỗi backend process nên tải đồng thời có thể sinh service_busy hoặc timeout. Mẫu 29,23 ms không phải phép đo throughput hoặc percentile.

Các kiểm tra nghiệm thu tiếp theo là nhận diện và giải thích trong browser trên AWS, xác nhận email SNS và ngưỡng khôi phục 85%, diễn tập rollback có ghi nhận và phép đo latency lặp lại được. Độ chính xác thực địa cần tập dữ liệu gán nhãn độc lập; workshop không công bố một tỷ lệ accuracy chưa được đo.


### 4 Tiêu chí tự đánh giá kết quả thực tập

| STT | Tiêu chí | Mô tả | Tốt | Khá | Trung bình |
| --- | --- | --- | --- | --- | --- |
| 1 | **Kiến thức và kỹ năng chuyên môn** | Hiểu biết về ngành, áp dụng kiến thức vào thực tế, kỹ năng sử dụng công cụ, chất lượng công việc | ✅ | ☐ | ☐ |
| 2 | **Khả năng học hỏi** | Tiếp thu kiến thức mới, học hỏi nhanh | ✅ | ☐ | ☐ |
| 3 | **Chủ động** | Tự tìm hiểu, nhận nhiệm vụ mà không chờ chỉ dẫn | ✅ | ☐ | ☐ |
| 4 | **Tinh thần trách nhiệm** | Hoàn thành công việc đúng hạn, đảm bảo chất lượng | ✅ | ☐ | ☐ |
| 5 | **Kỷ luật** | Tuân thủ giờ giấc, nội quy, quy trình làm việc | ✅ | ☐ | ☐ |
| 6 | **Tính cầu tiến** | Sẵn sàng nhận feedback và cải thiện bản thân | ✅ | ☐ | ☐ |
| 7 | **Giao tiếp** | Trình bày ý tưởng, báo cáo công việc rõ ràng | ✅ | ☐ | ☐ |
| 8 | **Hợp tác nhóm** | Làm việc hiệu quả với đồng nghiệp, tham gia nhóm | ✅ | ☐ | ☐ |
| 9 | **Ứng xử chuyên nghiệp** | Tôn trọng đồng nghiệp, đối tác, môi trường làm việc | ✅ | ☐ | ☐ |
| 10 | **Tư duy giải quyết vấn đề** | Nhận diện vấn đề, đề xuất giải pháp, sáng tạo | ✅ | ☐ | ☐ |
| 11 | **Đóng góp vào dự án/tổ chức** | Hiệu quả công việc, sáng kiến cải tiến, ghi nhận từ team | ✅ | ☐ | ☐ |
| 12 | **Tổng thể** | Đánh giá chung về toàn bộ quá trình thực tập | ✅ | ☐ | ☐ |
