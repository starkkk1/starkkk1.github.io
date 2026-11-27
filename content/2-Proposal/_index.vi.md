---
title: "Đề xuất dự án"
date: 2026-10-16
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Đề xuất dự án nhận diện bệnh lá cây trên AWS

## Kế hoạch triển khai và xác minh trong 12 tuần

Ngày 16 tháng 10 năm 2026  |  Sydney  |  ap-southeast-2

Đề xuất chương trình triển khai và xác minh kéo dài mười hai tuần cho ứng dụng phân loại bệnh lá cà chua bằng container trên EC2, S3 và CloudWatch. Kế hoạch kế thừa prototype hiện có và hoàn thiện deployment, bằng chứng nghiệm thu cùng vận hành.


### 1 Tóm tắt điều hành

Dự án bàn giao bản demo AWS có thể tái tạo, gồm frontend responsive, API suy luận CPU, artifacts model lưu private và khả năng quan sát cơ bản qua CloudWatch. GitHub Actions phát hành image; quy trình thủ công có kiểm soát triển khai và rollback các phiên bản image và model tương thích. Phạm vi phù hợp học backend và MLOps, không cần gán nhãn serverless hoặc microservices.

Ngân sách dự án theo cấu hình demo AWS tải thấp trong file tổng quan: **50 USD mỗi tháng trước thuế**, tương đương **150 USD cho ba kỳ thanh toán tháng** trong thời gian dự án mười hai tuần. Với 200 USD AWS credit đủ điều kiện, cấu hình này được chi trả trong bốn tháng. Khả năng sử dụng và hạn credit phụ thuộc vào tài khoản AWS. [6–12]


### 2 Phát biểu vấn đề

Classifier được train local khó tái tạo trên host khác nếu weights, thứ tự lớp, preprocessing và dependencies được quản lý rời rạc. Chỉ truy cập LAN làm hạn chế demo; process còn chạy vẫn có thể lỗi dự đoán vì thiếu weights, CORS, queue quá tải hoặc thiếu quyền. Đồng bộ source code không tự cập nhật container đang chạy.

Dự án cần định danh release có phiên bản, quyền đọc artifacts tối thiểu, bằng chứng inference thật trên AWS, monitoring host có thể hành động và quy trình phục hồi rõ ràng. Prototype hiện có cung cấp các thành phần hoạt động, nhưng chưa xác lập high availability, accuracy thực địa hoặc deployment production tự động.


### 3 Kiến trúc giải pháp


#### Các dịch vụ AWS được sử dụng

| Dịch vụ | Vai trò | Lựa chọn |
| --- | --- | --- |
| Amazon EC2 | Chạy frontend và API | Một host x86_64 tại Sydney |
| Amazon S3 | Lưu checkpoint và class map | Artifacts private có phiên bản |
| Amazon CloudWatch | Log container, metrics máy chủ và dashboard | Metrics Agent 60 giây; log 7 ngày |
| IAM and VPC | Danh tính và kiểm soát mạng | Instance role và giới hạn source IP |


#### Thiết kế thành phần

Browser truy cập Next.js tại port 3000 và FastAPI tại port 8000 trên cùng hostname EC2. FastAPI load checkpoint MobileNet một lần mỗi process và kiểm tra class map. Prediction và Grad-CAM++ dùng chung slot thực thi có giới hạn. S3 được truy cập lúc triển khai; use case hiện tại không yêu cầu lưu ảnh người dùng vào S3.

```text
GitHub Actions -> GHCR -> manual deployment to EC2
Private S3 artifacts -> host version directory -> read-only API mount
Browser -> Next.js frontend and FastAPI API
Container logs + Agent metrics -> CloudWatch dashboard
```

Kiến trúc demo dùng public subnet với inbound bị giới hạn. S3 VPC endpoint, kết nối on-premises, mô phỏng DNS, load balancing và autoscaling nằm ngoài baseline. Có thể đánh giá phần mở rộng mạng private sau, nhưng không ghi nhận là tính năng đã triển khai.


### 4 Triển khai kỹ thuật

Build frontend bằng Dockerfile nhiều stage và Next.js standalone hiện có. Build API bằng Dockerfile.deploy để checkpoint nằm ngoài image. CI gate gồm Python tests, frontend build, backend build và smoke test container frontend. Phát hành hai image bằng tag full commit rồi ghi digest; kiểm thử fixture weights được phân biệt với nghiệm thu model thật.

Tạo instance profile với GetObject giới hạn đúng prefix và quyền gửi CloudWatch. Tải artifacts từ mobilenet-distilled-v1, đối chiếu SHA256 rồi mount chỉ đọc. Publish port API và frontend, đặt CORS_ORIGINS khớp origin trang và dùng restart unless-stopped. Tạo lại container khi đổi image hoặc biến môi trường.

Xác minh readiness, dự đoán ảnh thật, schema response và model identity. Sau đó kiểm tra upload cùng Grad-CAM++ trên desktop và điện thoại. Đo latency API với số request và concurrency được khai báo thay vì xem một request là p95. Chỉ đánh giá macro F1 và recall từng lớp khi có dữ liệu gán nhãn độc lập; không cam kết accuracy chẩn đoán nếu chưa đánh giá.

Dùng CloudWatch Logs cho hai container và CloudWatch Agent để thu thập metrics bộ nhớ cùng đĩa, lưu log trong bảy ngày. Theo dõi dashboard máy chủ trong quá trình vận hành. Ghi nhận diễn tập rollback và EC2 stop/start, gồm cập nhật public IP và CORS.


### 5 Tiến độ và các mốc bàn giao

Mỗi tuần phân bổ 20 giờ làm việc, tổng cộng 240 giờ. Lịch đề xuất chạy từ ngày 7/9 đến 27/11/2026; ngày làm việc cuối là 27/11, trước hạn 30/11. Đây là kế hoạch, không phải ghi lại ngày công đã thực hiện. Chi phí AWS áp dụng cấu hình theo tháng trong bảng ngân sách dưới đây, không tính từ số giờ làm việc dự kiến.

| Tuần và ngày dự kiến | Công việc | Bằng chứng |
| --- | --- | --- |
| 1 · 7–11/9 | Phạm vi và tiêu chí nghiệm thu | Chốt phạm vi kiến trúc |
| 2 · 14–18/9 | Audit model preprocessing và nhãn | Manifest artifacts và hash |
| 3 · 21–25/9 | Hoàn thiện validation và lỗi API | Hợp đồng API và tests |
| 4 · 28/9–2/10 | Kiểm tra frontend và giải thích | Luồng browser local được xác minh |
| 5 · 5–9/10 | Đóng gói container demo | Hai image qua smoke tests |
| 6 · 12–16/10 | Hoàn thiện CI và phát hành GHCR | Digest release được phát hành |
| 7 · 19–23/10 | Chuẩn bị IAM mạng và ngân sách | Role EC2 và kết nối giới hạn |
| 8 · 26–30/10 | Lưu và tải artifacts từ S3 | Hash tải về khớp |
| 9 · 2–6/11 | Triển khai EC2 và inference thật | Nghiệm thu desktop và điện thoại |
| 10 · 9–13/11 | Cấu hình khả năng quan sát CloudWatch | Kiểm tra log group, metrics máy chủ và dashboard |
| 11 · 16–20/11 | Diễn tập rollback và đo latency | Bằng chứng phục hồi và hiệu năng |
| 12 · 23–27/11 | Hoàn thiện báo cáo demo và cleanup | Bàn giao và rà soát chi phí |


### 6 Ngân sách

Ngân sách vận hành cho bản demo tải thấp tại Sydney (`ap-southeast-2`) là **50 USD mỗi tháng trước thuế**. Mức này bao gồm một máy EC2 `t3.medium` chạy 730 giờ mỗi tháng, ổ gp3 20 GiB, một địa chỉ IPv4 public, artifacts model dung lượng nhỏ trên S3 và mức sử dụng thấp cho monitoring. Phân bổ theo tháng:

| Dịch vụ / mức sử dụng | Chi phí tháng |
|---|---:|
| EC2 Linux `t3.medium`, 730 giờ | $38.54 |
| EBS gp3, 20 GiB | $1.60 |
| Public IPv4, 730 giờ | $3.65 |
| Checkpoint mô hình và class map trên S3 | $0.01 |
| CloudWatch logs, metrics máy chủ và dashboard | $0.00 |
| Truyền dữ liệu Internet trong hạn mức dùng chung 100 GB/tháng | $0.00 |
| Dự phòng vận hành | $6.20 |
| **Tổng mỗi tháng** | **$50.00** |

Dự án mười hai tuần dành ngân sách cho ba kỳ thanh toán tháng, tổng cộng **150 USD trước thuế**. AWS credit đủ điều kiện trị giá 200 USD chi trả được bốn tháng theo ngân sách này, còn 50 USD sau thời gian mười hai tuần. Khả năng đủ điều kiện, số dư và hạn sử dụng credit tùy thuộc tài khoản AWS; cần xác minh tại Billing Console. Hãy đặt cảnh báo ngân sách tháng và thường xuyên kiểm tra mức dùng gộp vì thông báo ngân sách không tự dừng tài nguyên. [6–12]

Ngân sách này dành cho một máy chủ và lưu lượng demo thấp. Chưa bao gồm thuế, huấn luyện GPU, load balancer, NAT Gateway, database, high availability, lưu lượng vượt hạn mức nêu trên, log hoặc metrics vượt mức, và phụ phí T3 CPU credit. Huấn luyện và nghiên cứu chạy trên máy cục bộ; GHCR và GitHub Actions nằm trong mức sử dụng đã bao gồm của tài khoản. Dừng EC2 giúp giảm phí compute, nhưng dung lượng EBS vẫn tính phí. Kiểm tra AWS Pricing Calculator nếu thay đổi Region hoặc cấu hình. [6–10]


### 7 Đánh giá rủi ro

| Rủi ro | Khả năng và tác động | Giảm thiểu và người phụ trách |
| --- | --- | --- |
| Cạn credit | Vừa / Cao | Người vận hành stop EC2 và rà soát chi phí tuần |
| Giới hạn loại máy hoặc capacity | Vừa / Vừa | Kiểm tra loại x86_64 phù hợp và tính lại dự toán |
| Sai model hoặc thứ tự lớp | Vừa / Cao | Phụ trách ML xác minh hash map và preprocessing |
| CORS hoặc public IP thay đổi | Cao / Vừa | Tạo lại backend với origin đúng |
| Lộ quyền hoặc credentials | Vừa / Cao | Dùng instance role S3 private và SSH giới hạn |
| Host lỗi hoặc quá tải | Vừa / Cao | Đo queue tải và diễn tập phục hồi |
| Ảnh thực địa khác dữ liệu train | Cao / Cao | Lấy mẫu có nhãn và giới hạn kết luận |

Bản demo ban đầu chấp nhận downtime của một host và truy cập HTTP giới hạn theo IP đã duyệt. Mở production công khai cần đánh giá bảo mật và availability riêng, gồm HTTPS và xác thực. Log và metrics CloudWatch hỗ trợ quan sát vận hành nhưng không chứng minh availability dịch vụ hay chất lượng ML.


### 8 Kết quả kỳ vọng

| Kết quả | Bằng chứng nghiệm thu |
| --- | --- |
| Release có thể tái tạo | Hai digest image cùng hash checkpoint map và cấu hình |
| Ứng dụng hoạt động trên AWS | Health predict thành công; xác minh nhận diện và giải thích trên browser |
| Tải artifacts có kiểm soát | GetObject S3 private bằng instance role và hash khớp |
| Khả năng quan sát | Log container, metrics máy chủ và dashboard được rà soát |
| Phục hồi có tài liệu | Diễn tập rollback và stop/start với thời gian ghi nhận |
| Hiệu năng được đo | Báo cáo nêu image phần cứng concurrency số mẫu và p95 |
| Chi phí được kiểm soát | Theo dõi chi phí gộp tuần và bằng chứng cleanup |

Thành công là deployment trình diễn được, có tài liệu và nằm trong ngân sách được duyệt. Điều đó không đồng nghĩa đã chứng minh dịch vụ chẩn đoán nông nghiệp, nền tảng không downtime hoặc đã hoàn thành lịch sử mười hai tuần. Mục tiêu accuracy và phục hồi chỉ được công bố kết quả sau khi đo.


### Tài liệu tham khảo

Tài liệu AWS và trang giá được truy cập ngày 16 tháng 10 năm 2026. Đơn giá dự phòng trong proposal là giả định lập ngân sách, không phải báo giá chính thức theo Region.

[1] Repository dự án và lịch sử phát hành

[2] Cơ chế dừng và khởi động EC2

[3] Quyền HeadObject S3 và phản hồi lỗi

[4] Cài đặt CloudWatch Agent

[5] Giám sát vận hành bằng CloudWatch

[6] Giá EC2 On Demand

[7] Giá EBS

[8] Giá public IPv4

[9] Giá CloudWatch

[10] Giá S3

[11] AWS credit

[12] Ngân sách AWS
