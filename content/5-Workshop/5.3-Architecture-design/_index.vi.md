---
title: "Kiến trúc & Thiết kế"
date: 2026-10-28T10:00:00+07:00
weight: 30
chapter: false
pre: "<b>5.3. </b>"
---

# Kiến trúc & Thiết kế

### Tổng quan Kiến trúc Toàn diện Hệ thống

Hệ thống **Plant Disease Detection** kết hợp quy trình phát triển và vận hành học máy (MLOps) tiên tiến với hạ tầng điện toán đám mây tối ưu trên nền tảng Amazon Web Services (AWS). Được thiết kế để phục vụ chẩn đoán bệnh học thời gian thực trên lá cà chua có khả năng giải thích quyết định (Explainable AI), kiến trúc hệ thống tại khu vực **Sydney (`ap-southeast-2`)** được phân tách thành 4 phân hệ cốt lõi cùng quy trình phân phối liên tục (CI/CD) và giám sát vận hành:

1. **Phân hệ Lưu trữ Trọng số & Tách rời Artifacts (Amazon S3 &rarr; Máy chủ EC2):** Tách biệt các tệp trọng số mạng nơ-ron nhị phân khỏi container image, đảm bảo tính toàn vẹn mật mã học SHA256 tại thời điểm chạy.
2. **Quy trình Phân phối Liên tục & Đóng gói Container (GitHub Actions &rarr; GHCR &rarr; EC2):** Tự động kiểm thử và phát hành container image frontend cùng API lên GHCR; kỹ sư vận hành triển khai phiên bản lên EC2 theo quy trình thủ công.
3. **Phân hệ Thực thi Web & Suy luận Trí tuệ Nhân tạo (Docker Stack trên EC2):** Chạy giao diện người dùng tương tác Next.js và dịch vụ suy luận FastAPI PyTorch CPU tích hợp công nghệ giải thích Grad-CAM++.
4. **Phân hệ Vận hành, An ninh & Giám sát Hệ thống:** Áp dụng nguyên tắc đặc quyền tối thiểu với IAM Role, Security Group khóa địa chỉ IP client, thu thập log với CloudWatch Logs, đo đạc RAM tùy biến với CloudWatch Agent và kích hoạt cảnh báo email qua Amazon SNS.

![Kiến trúc Toàn diện Hệ thống Nhận diện Bệnh Cây trồng](/images/5-Workshop/5.3-Architecture-design/system_architecture.svg?width=100%)

---

### Bảng Mô tả 18 Bước Vận hành Hệ thống Chi tiết

Chuỗi tương tác xuyên suốt 18 bước được đánh số trên sơ đồ kiến trúc như sau:

| Bước # | Phân hệ / Ranh giới | Thành phần Khởi phát | Thành phần Tiếp nhận | Giao thức / Cơ chế | Mô tả Kỹ thuật Chi tiết |
|:---:|---|---|---|---|---|
| **❶** | Quản lý Mã nguồn | Kỹ sư Phát triển | GitHub Repository | `git push` | Kỹ sư đẩy mã nguồn và cấu hình đã qua kiểm thử lên nhánh chính `master`. |
| **❷** | Tích hợp Liên tục | GitHub Repository | GitHub Actions | Webhook Trigger | Sự kiện push tự động kích hoạt workflow CI định nghĩa tại `.github/workflows/ci.yml`. |
| **❸** | Đóng gói Container | GitHub Actions | GitHub Container Registry (GHCR) | `docker push` | CI xây dựng Docker image đa giai đoạn và đẩy lên GHCR kèm mã băm digest bất biến. |
| **❹** | Lưu trữ Artifacts | Pipeline Huấn luyện | Amazon S3 Bucket | HTTPS S3 Upload | Quá trình chưng cất tri thức ngoại tuyến lưu trữ tệp checkpoint `model.pth` và `class_to_idx.json`. |
| **❺** | Tiếp nhận Trọng số | Amazon S3 Bucket | Thư mục Máy chủ EC2 | `aws s3 cp` | Máy chủ EC2 tải tệp trọng số từ S3 thông qua quyền hạn từ IAM Instance Profile. |
| **❻** | Kiểm định Toàn vẹn | Thư mục Máy chủ EC2 | Script Kiểm thử Host | `sha256sum` | Kiểm tra mã băm SHA256 của tệp tải về so với bản kiểm định gốc trước khi nạp. |
| **❼** | Cách ly Container | Thư mục Máy chủ EC2 | Container FastAPI | Docker `:ro` Volume | Gắn kết thư mục `checkpoints/v1` ở chế độ chỉ đọc vào `/app/artifacts` của container. |
| **❽** | Triển khai Image | GHCR | Docker Engine (EC2) | `docker pull` | Máy chủ EC2 kéo image từ GHCR theo mã digest hash bất biến (`sha256:...`). |
| **❾** | Truy cập Người dùng | Nông dân & Nhà nông học | Security Group (VPC) | HTTP (`:3000`) | Thiết bị người dùng truy cập web frontend qua cổng 3000 được mở giới hạn IP. |
| **❿** | Tải ảnh Bệnh học | Trình duyệt Client | Container Next.js | HTTP Multipart | Người dùng chụp hoặc tải tệp ảnh lá cà chua (`tomato-leaf.jpg`) lên giao diện web. |
| **⓫** | Chuyển tiếp Suy luận | Container Next.js | Container FastAPI | HTTP POST (`:8000`) | Giao diện web chuyển tiếp yêu cầu chẩn đoán đến API nội bộ qua endpoint `/predict`. |
| **⓬** | Suy luận Nơ-ron | Engine Suy luận FastAPI | MobileNetV3-Small Runtime | PyTorch CPU (2 Cores) | Mô hình thực thi lan truyền tiến phân loại 11 bệnh học với độ trễ cực nhanh **29.23 ms**. |
| **⓭** | Trích xuất Giải thích | PyTorch Backend | Engine Grad-CAM++ | Gradient Feature Maps | Tính toán trọng số gradient tại lớp tích chập cuối để tạo bản đồ nhiệt vùng hoại tử. |
| **⓮** | Trả kết quả Chẩn đoán | Backend FastAPI | Giao diện Next.js | JSON + PNG Overlay | Phản hồi danh sách xác suất bệnh cùng ảnh heatmap phủ lớp trong suốt cho nông dân. |
| **⓯** | Ghi log Tập trung | Docker Engine | Amazon CloudWatch Logs | Docker `awslogs` Driver | Chuyển luồng log stdout/stderr dạng JSON vào log group `/plant-disease/backend` và `/plant-disease/frontend`. |
| **⓰** | Đo đạc Tài nguyên Host | CloudWatch Agent | CloudWatch Metrics | Custom Metric Daemon | Daemon trên host đọc thông số RAM (`mem_used_percent`) và ổ đĩa theo chu kỳ 60 giây. |
| **⓱** | Đánh giá Ngưỡng Tràn | CloudWatch Metrics | CloudWatch RAM Alarm | Cảnh báo Vi phạm Ngưỡng | Kích hoạt trạng thái báo động khi tỷ lệ sử dụng RAM vượt quá 85% trong 3 phút liên tiếp. |
| **⓲** | Phát thông báo Sự cố | CloudWatch RAM Alarm | Amazon SNS Topic | Email Subscription | Amazon SNS lập tức gửi thư cảnh báo khẩn cấp đến hộp thư điện tử của kỹ sư vận hành. |

---

### Phân tích Chuyên sâu 4 Phân hệ Kiến trúc

#### Phân hệ 1: Nguyên tắc Tách rời Trọng số Mô hình khỏi Mã nguồn
Trong môi trường MLOps chuẩn doanh nghiệp, việc đóng gói trực tiếp các tệp trọng số nhị phân nặng hàng trăm megabyte vào Docker image gây ra nhiều hệ lụy: container image bị phình to dung lượng, chu kỳ build/push kéo dài, và mỗi khi cập nhật phiên bản mô hình lại buộc phải build lại toàn bộ ứng dụng.

Kiến trúc **Plant Disease Detection** tách biệt hoàn toàn giữa mã nguồn và tệp mô hình:
1. **Container Image Nhẹ và Tối giản:** Image Docker chỉ đóng gói mã logic ứng dụng, thư viện phụ thuộc (FastAPI, PyTorch CPU, Next.js) và tài nguyên tĩnh.
2. **Lưu trữ Chuyên biệt trên Đám mây:** Tệp checkpoint PyTorch (`model.pth`) và từ điển nhãn (`class_to_idx.json`) nằm trong S3 bucket riêng tư (`plant-disease-model-artifacts-ap-southeast-2`) dưới tiền tố `mobilenet-distilled-v1/`.
3. **Tiếp nhận An toàn khi Triển khai:** Khi máy chủ khởi chạy, instance profile IAM cấp quyền tải tệp, kiểm tra mã băm SHA256 và gắn kết ổ đĩa chỉ đọc (**read-only volume** `/app/artifacts:ro`) vào container FastAPI.

#### Phân hệ 2: Tích hợp Liên tục & Phân phối Bất biến
Mọi thao tác đẩy commit lên GitHub đều kích hoạt quy trình CI/CD tự động:
- **Bộ Kiểm thử PyTest Tự động:** Kiểm định tính đúng đắn của logic tiền xử lý tensor, giải mã ảnh PIL và kiểm tra định dạng dữ liệu đầu ra JSON.
- **Tối ưu hóa Next.js Standalone:** Biên dịch TypeScript và đóng gói máy chủ Node.js 20 độc lập với kích thước nhỏ gọn.
- **Gắn mã Digest Cố định:** Các image đẩy lên GitHub Container Registry (GHCR) được kéo về EC2 theo mã băm digest bất biến (`sha256:f61be157...` và `sha256:ec211d13...`), loại bỏ hoàn toàn rủi ro sai lệch do ghi đè tag `:latest`.

#### Phân hệ 3: Môi trường Container Thực thi Web & Suy luận AI
Phân hệ tính toán chạy trên một máy chủ ảo EC2 Amazon Linux 2023 với hai container độc lập:
- **Next.js Standalone Frontend (`:3000`):** Chạy dưới tài khoản không có quyền root `nextjs` (UID 1001), phục vụ giao diện người dùng đáp ứng đa nền tảng (Mobile/Desktop).
- **FastAPI Backend (`:8000`):** Dịch vụ Python 3.12 giới hạn số luồng xử lý PyTorch CPU ở mức 2 core (`MODEL_THREADS=2`), ngăn chặn tình trạng chiếm dụng cạn kiệt tài nguyên máy chủ khi lưu lượng tăng đột biến.
- **MobileNetV3-Small Chưng cất Tri thức:** Giữ lại hơn 96% độ chính xác của mô hình giáo viên ConvNeXt-Tiny nhưng giảm dung lượng tới 94.7% (chỉ còn ~6 MiB) và đạt tốc độ suy luận CPU ấn tượng **29.23 ms**.
- **Công nghệ Giải thích Grad-CAM++:** Phân tích gradient lớp tích chập cuối cùng để sinh ra bản đồ nhiệt trực quan hóa, giúp người dùng nhận biết chính xác mô hình tập trung vào vết hoại tử hay viền lá úa.

#### Phân hệ 4: Vận hành, An ninh Phân quyền Tối thiểu & Giám sát
- **IAM Instance Profile:** Cấp quyền chính xác `s3:GetObject` trên tiền tố tệp trọng số và policy `CloudWatchAgentServerPolicy`, loại bỏ hoàn toàn việc lưu trữ Access Key tĩnh trên máy ảo.
- **Amazon VPC & Security Groups:** Tường lửa bảo vệ chỉ cho phép truy cập các cổng `22` (SSH), `3000` (Web UI) và `8000` (API) từ duy nhất địa chỉ IPv4 của kỹ sư vận hành (`<OPERATOR_IP>/32`).
- **Amazon CloudWatch Logs:** Thu thập toàn bộ luồng log stdout/stderr với chính sách lưu trữ **7 ngày**, cân đối giữa nhu cầu khắc phục sự cố và tối ưu chi phí lưu trữ.
- **CloudWatch Agent & Alarm:** Thu thập mức sử dụng RAM thực tế (`mem_used_percent`) trong namespace `PlantDisease/Host`. Chuông báo động kích hoạt khi RAM **vượt quá 85% trong 3 phút liên tục**, gửi email cảnh báo qua **Amazon SNS**.

---

### 5.3.1 Chuẩn bị Danh tính và Quyền hạn Tải Tệp

Trước khi thực hiện tải tệp, cần đảm bảo máy chủ EC2 đã được gắn IAM Role phù hợp và có thể kết nối với dịch vụ S3 trong khu vực:

```bash
# Xác thực danh tính IAM Role của máy ảo EC2
aws sts get-caller-identity

# Đặt biến môi trường làm việc khu vực Sydney
export AWS_DEFAULT_REGION="ap-southeast-2"
read -r -p "Nhập tên S3 bucket lưu model: " MODEL_BUCKET

# Tạo thư mục lưu trữ trọng số trên máy chủ
mkdir -p checkpoints/v1
```

> [!NOTE]
> Bản demo hiện tại truy cập dịch vụ S3 qua bảng định tuyến internet thông thường của public subnet ra Internet Gateway. Đối với môi trường doanh nghiệp có subnet riêng tư (private subnet), người quản trị nên tạo thêm một **Amazon S3 Gateway VPC Endpoint** để lưu lượng tải tệp đi hoàn toàn trên mạng nội bộ của AWS với chi phí đường truyền bằng 0.

---

### 5.3.2 Tải về và Kiểm tra Tính Toàn vẹn của Artifacts

Chạy các lệnh sau để tải tệp checkpoint mô hình và từ điển phân lớp từ S3, sau đó tiến hành kiểm tra mã băm mật mã học SHA256:

```bash
# Tải tệp mô hình và tệp ánh xạ nhãn
aws s3 cp "s3://$MODEL_BUCKET/mobilenet-distilled-v1/model.pth" checkpoints/v1/model.pth
aws s3 cp "s3://$MODEL_BUCKET/mobilenet-distilled-v1/class_to_idx.json" checkpoints/v1/class_to_idx.json

# Xác thực mã băm SHA256
sha256sum checkpoints/v1/model.pth checkpoints/v1/class_to_idx.json
```

#### Bảng Đối soát Mã băm Ghi nhận Thực tế

Các tệp tải về phải khớp tuyệt đối với mã băm đã được kiểm định từ quá trình huấn luyện và đánh giá ngoại tuyến:

| Tên Tệp | Mã băm SHA256 Tiêu chuẩn | Dung lượng | Trạng thái Kiểm định |
|---|---|---|---|
| `model.pth` | `05e72798a74193499f17c4c8267c1436e8bc65cc1e8b8037245664a17644efa5` | `~6.0 MiB` | **TRÙNG KHỚP** |
| `class_to_idx.json` | `ff5c73e7a7e3e495593fa664b75ebc73de4c9a425f1d847351ae60bc1fbaa786` | `~1.2 KiB` | **TRÙNG KHỚP** |

> [!IMPORTANT]
> Sai khác mã băm SHA256 đồng nghĩa tệp tải về bị lỗi, phiên bản không đồng bộ hoặc đã bị chỉnh sửa bất thường. Backend FastAPI sẽ từ chối khởi động và trả về mã lỗi 503 nếu trọng số không hợp lệ.

---

### So sánh Hình ảnh Giải thích Quyết định Mô hình

Trọng số MobileNetV3-Small sau khi chưng cất tri thức tái hiện xuất sắc khả năng chú ý vùng bệnh học tương tự như mô hình giáo viên ConvNeXt-Tiny:

| Mô hình Giáo viên (ConvNeXt-Tiny) | Mô hình Học sinh (MobileNetV3-Small Chưng cất) |
|---|---|
| ![ConvNeXt Overlay](/images/5-Workshop/plant-disease/explain-convnext_tiny-overlay.png?width=350px) | ![MobileNetV3 Overlay](/images/5-Workshop/plant-disease/explain-mobilenetv3_small_100_distilled-overlay.png?width=350px) |
| Vùng bao phủ rộng, nhận diện đặc trưng toàn diện | Tập trung chính xác vào rìa hoại tử và vùng tổn thương cục bộ |
