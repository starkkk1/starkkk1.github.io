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

Dự án bàn giao bản demo AWS có thể tái tạo, gồm frontend responsive, API suy luận CPU, artifacts model lưu private và một alarm RAM vận hành. GitHub Actions phát hành image; quy trình thủ công có kiểm soát triển khai và rollback các phiên bản image và model tương thích. Phạm vi phù hợp học backend và MLOps, không cần gán nhãn serverless hoặc microservices.

Kịch bản dự toán dùng 240 giờ EC2 trong mười hai tuần, tổng ngân sách gộp 67,20 USD đã gồm dự phòng 20%. Ảnh tài khoản hiển thị 100 USD khả dụng; không giả định đã nhận thêm 100 USD. Cần kiểm tra dịch vụ được credit bù, hạn credit, giá Region và thuế trước khi chi tiêu. Đây là mức dự phòng lập kế hoạch, không phải báo giá AWS. [6–12]


### 2 Phát biểu vấn đề

Classifier được train local khó tái tạo trên host khác nếu weights, thứ tự lớp, preprocessing và dependencies được quản lý rời rạc. Chỉ truy cập LAN làm hạn chế demo; process còn chạy vẫn có thể lỗi dự đoán vì thiếu weights, CORS, queue quá tải hoặc thiếu quyền. Đồng bộ source code không tự cập nhật container đang chạy.

Dự án cần định danh release có phiên bản, quyền đọc artifacts tối thiểu, bằng chứng inference thật trên AWS, monitoring host có thể hành động và quy trình phục hồi rõ ràng. Prototype hiện có cung cấp các thành phần hoạt động, nhưng chưa xác lập high availability, accuracy thực địa hoặc deployment production tự động.


### 3 Kiến trúc giải pháp


#### Các dịch vụ AWS được sử dụng

| Dịch vụ | Vai trò | Lựa chọn |
| --- | --- | --- |
| Amazon EC2 | Chạy frontend và API | Một host x86_64 tại Sydney |
| Amazon S3 | Lưu checkpoint và class map | Artifacts private có phiên bản |
| Amazon CloudWatch | Log metrics dashboard và alarm RAM | Metrics Agent 60 giây; log 7 ngày |
| IAM and VPC | Danh tính và kiểm soát mạng | Instance role và giới hạn source IP |
| Amazon SNS | Gửi email cảnh báo | Một email subscription đã xác nhận |


#### Thiết kế thành phần

Browser truy cập Next.js tại port 3000 và FastAPI tại port 8000 trên cùng hostname EC2. FastAPI load checkpoint MobileNet một lần mỗi process và kiểm tra class map. Prediction và Grad-CAM++ dùng chung slot thực thi có giới hạn. S3 được truy cập lúc triển khai; use case hiện tại không yêu cầu lưu ảnh người dùng vào S3.

```text
GitHub Actions -> GHCR -> manual deployment to EC2
Private S3 artifacts -> host version directory -> read-only API mount
Browser -> Next.js frontend and FastAPI API
Container logs + Agent metrics -> CloudWatch -> RAM alarm -> SNS
```

Kiến trúc demo dùng public subnet với inbound bị giới hạn. S3 VPC endpoint, kết nối on-premises, mô phỏng DNS, load balancing và autoscaling nằm ngoài baseline. Có thể đánh giá phần mở rộng mạng private sau, nhưng không ghi nhận là tính năng đã triển khai.


### 4 Triển khai kỹ thuật

Build frontend bằng Dockerfile nhiều stage và Next.js standalone hiện có. Build API bằng Dockerfile.deploy để checkpoint nằm ngoài image. CI gate gồm Python tests, frontend build, backend build và smoke test container frontend. Phát hành hai image bằng tag full commit rồi ghi digest; kiểm thử fixture weights được phân biệt với nghiệm thu model thật.

Tạo instance profile với GetObject giới hạn đúng prefix và quyền gửi CloudWatch. Tải artifacts từ mobilenet-distilled-v1, đối chiếu SHA256 rồi mount chỉ đọc. Publish port API và frontend, đặt CORS_ORIGINS khớp origin trang và dùng restart unless-stopped. Tạo lại container khi đổi image hoặc biến môi trường.

Xác minh readiness, dự đoán ảnh thật, schema response và model identity. Sau đó kiểm tra upload cùng Grad-CAM++ trên desktop và điện thoại. Đo latency API với số request và concurrency được khai báo thay vì xem một request là p95. Chỉ đánh giá macro F1 và recall từng lớp khi có dữ liệu gán nhãn độc lập; không cam kết accuracy chẩn đoán nếu chưa đánh giá.

Dùng CloudWatch Logs cho hai container và Agent metrics cho RAM cùng đĩa. Giữ log bảy ngày, cấu hình một alarm RAM Average lớn hơn 85% trong ba datapoint một phút. Xác nhận SNS bằng kiểm thử ngưỡng có kiểm soát rồi khôi phục 85. Ghi nhận diễn tập rollback và EC2 stop/start, gồm cập nhật public IP và CORS.


### 5 Tiến độ và các mốc bàn giao

Mỗi tuần phân bổ 20 giờ làm việc, tổng cộng 240 giờ. Đây là kế hoạch đề xuất mười hai tuần, không phải ghi lại ngày công đã thực hiện. Runtime EC2 được dự toán riêng ở tổng 240 giờ; không bắt buộc trùng từng giờ làm việc.

| Tuần | Công việc | Bằng chứng |
| --- | --- | --- |
| 1 20 h | Phạm vi và tiêu chí nghiệm thu | Chốt phạm vi kiến trúc |
| 2 20 h | Audit model preprocessing và nhãn | Manifest artifacts và hash |
| 3 20 h | Hoàn thiện validation và lỗi API | Hợp đồng API và tests |
| 4 20 h | Kiểm tra frontend và giải thích | Luồng browser local được xác minh |
| 5 20 h | Đóng gói container production | Hai image qua smoke tests |
| 6 20 h | Hoàn thiện CI và phát hành GHCR | Digest release được phát hành |
| 7 20 h | Chuẩn bị IAM mạng và ngân sách | Role EC2 và kết nối giới hạn |
| 8 20 h | Lưu và tải artifacts từ S3 | Hash tải về khớp |
| 9 20 h | Triển khai EC2 và inference thật | Nghiệm thu desktop và điện thoại |
| 10 20 h | Cấu hình CloudWatch và alarm RAM | Kiểm tra metrics log và email |
| 11 20 h | Diễn tập rollback và đo latency | Bằng chứng phục hồi và hiệu năng |
| 12 20 h | Hoàn thiện báo cáo demo và cleanup | Bàn giao và rà soát chi phí |


### 6 Ước tính ngân sách

Dự toán bằng USD trước thuế, credit và ưu đãi tại Sydney. Giả định một c7i-flex.large hoặc loại x86_64 được duyệt sau khi kiểm tra availability và giá. Chưa xác minh loại instance hiện có. Mức EC2 0,12 USD mỗi giờ và gp3 0,12 USD mỗi GiB tháng là giả định dự phòng làm tròn, không khẳng định là giá niêm yết Sydney. Tính lại nếu loại được chọn vượt mức dự toán. IPv4 dùng mức công bố 0,005 USD mỗi giờ. [6–8]

| Hạng mục | Cơ sở | USD |
| --- | --- | --- |
| EC2 | 240 h × $0.12 | $28.80 |
| Public IPv4 | 240 h × $0.005 | $1.20 |
| EBS gp3 | 30 GiB × 3 tháng × $0.12 | $10.80 |
| Metrics dashboard alarm | Dự phòng $3/tháng × 3 | $9.00 |
| Log và truy vấn | Dự phòng $1/tháng × 3 | $3.00 |
| Lưu trữ phiên bản và request | dự phòng dự án | $1.00 |
| SNS email | dự phòng dự án | $0.20 |
| Truyền dữ liệu | dự phòng tải thấp | $2.00 |
| Tạm tính |  | $56.00 |
| Dự phòng | 20% × $56.00 | $11.20 |
| Tổng cộng |  | $67.20 |

Mười hai tuần tương đương 84 ngày; lưu trữ và monitoring cố định được làm tròn lên ba tháng. Khoản CloudWatch và S3 là hạn mức dự phòng, không phải phép tính chính xác theo đơn giá, và không dựa vào miễn phí Free Tier. Khoản truyền dữ liệu chỉ phù hợp demo nhỏ. Chưa gồm thuế, mua domain, load balancer, NAT Gateway, GPU, training model và VPC endpoint bổ sung. [7–10]

Với cùng giả định, để EC2 và public IPv4 chạy đủ 2.016 giờ trong mười hai tuần làm tạm tính tăng lên 278,00 USD, tổng có dự phòng là 333,60 USD. Vì vậy, stop máy giữa các buổi là yêu cầu quan trọng của kế hoạch giới hạn credit. EBS và tài nguyên monitoring giữ lại vẫn có chi phí khi instance đã dừng. [2,6–9]

Chỉ dùng số credit 100 USD đã thấy cho đến khi khoản bổ sung thực sự được cấp. Dự toán gộp còn 32,80 USD dư địa trước chênh lệch giá thực và thuế. Đặt cảnh báo ngân sách tháng và rà soát sử dụng gộp hằng tuần với credit được loại khỏi góc nhìn chi phí. Thông báo ngân sách không phải cơ chế chặn chi tiêu tức thời. [11–12]


### 7 Đánh giá rủi ro

| Rủi ro | Khả năng và tác động | Giảm thiểu và người phụ trách |
| --- | --- | --- |
| Cạn credit | Vừa / Cao | Người vận hành stop EC2 và rà soát chi phí tuần |
| Giới hạn loại máy hoặc capacity | Vừa / Vừa | Kiểm tra loại x86_64 phù hợp và tính lại dự toán |
| Sai model hoặc thứ tự lớp | Vừa / Cao | Phụ trách ML xác minh hash map và preprocessing |
| CORS hoặc public IP thay đổi | Cao / Vừa | Tạo lại backend với origin đúng |
| Lộ quyền hoặc credentials | Vừa / Cao | Dùng instance role S3 private và SSH giới hạn |
| Host lỗi hoặc quá tải | Vừa / Cao | Đo queue tải và diễn tập phục hồi |
| Email alarm không đến | Vừa / Vừa | Xác nhận SNS và kiểm thử ngưỡng |
| Ảnh thực địa khác dữ liệu train | Cao / Cao | Lấy mẫu có nhãn và giới hạn kết luận |

Bản demo ban đầu chấp nhận downtime của một host và truy cập HTTP giới hạn theo IP đã duyệt. Mở production công khai cần đánh giá bảo mật và availability riêng, gồm HTTPS và xác thực. Alarm RAM là alarm vận hành duy nhất trong phạm vi và không bao phủ availability dịch vụ hoặc chất lượng ML.


### 8 Kết quả kỳ vọng

| Kết quả | Bằng chứng nghiệm thu |
| --- | --- |
| Release có thể tái tạo | Hai digest image cùng hash checkpoint map và cấu hình |
| Ứng dụng hoạt động trên AWS | Health predict thành công; xác minh nhận diện và giải thích trên browser |
| Tải artifacts có kiểm soát | GetObject S3 private bằng instance role và hash khớp |
| Khả năng quan sát | Log dashboard host một alarm RAM và email SNS nhận được |
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

[5] Action của CloudWatch alarm

[6] Giá EC2 On Demand

[7] Giá EBS

[8] Giá public IPv4

[9] Giá CloudWatch

[10] Giá S3

[11] AWS credit

[12] Ngân sách AWS
