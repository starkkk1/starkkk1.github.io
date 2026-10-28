---
title: "Chiến lược Triển khai"
date: 2026-10-28T10:00:00+07:00
weight: 40
chapter: false
pre: "<b>5.4. </b>"
---

# Chiến lược Triển khai

### 5.4.1 Quy trình Triển khai Tự động (Continuous Delivery)

Hệ thống triển khai được xây dựng dựa trên luồng tự động hóa tích hợp liên tục (CI) với **GitHub Actions** (`.github/workflows/ci.yml`). Mỗi khi có commit mới vào kho mã nguồn:
1. Chạy toàn bộ bộ kiểm thử tự động Python (linting, kiểm thử unit test cho tiền xử lý và schema dữ liệu).
2. Xây dựng gói mã nguồn giao diện Next.js, tối ưu hóa các tệp tĩnh và đảm bảo tương thích khi chạy với tài khoản không có quyền root.
3. Đóng gói container Docker nhiều giai đoạn (multi-stage) cho cả frontend và backend, tự động gắn thẻ và phát hành lên **GHCR (GitHub Container Registry)**.

#### Ghi nhận Định danh Bản phát hành

Để đảm bảo tính tái lập và tránh các vấn đề phát sinh do sử dụng thẻ mutable (như `:latest`), dự án lưu trữ chính xác mã băm commit và image digest:

| Thành phần | Định danh Phiên bản | Mã băm Ghi nhận Thực tế |
|---|---|---|
| **Git Commit** | `ff6b75bf3a625db124cc6449f52fa224bf76631e` | Bản phát hành chính thức |
| **Backend API Image** | `ghcr.io/starkkk1/plant-disease-api` | `sha256:f61be1570a834f1819614ad041f79e6114e00a0503852a121a564725cad4c27c` |
| **Frontend Image** | `ghcr.io/starkkk1/plant-disease-frontend` | `sha256:ec211d13241680f208b29f8282c7fb12a19735e9048d1f8dab9b6a27499a7f85` |

---

### 5.4.2 Cấu hình Môi trường Chạy Backend

Container backend chạy ứng dụng FastAPI trên nền Python 3.12 với PyTorch CPU. Máy chủ gắn thư mục `checkpoints/v1` đã tải từ S3 vào container theo chế độ **chỉ đọc (read-only)** tại đường dẫn `/app/artifacts`.

Khởi chạy container backend bằng câu lệnh sau:

```bash
docker run -d \
  --name plant-disease-api \
  --restart unless-stopped \
  -p 8000:8000 \
  -v /home/ec2-user/checkpoints/v1:/app/artifacts:ro \
  -e MODEL_CHECKPOINT=/app/artifacts/model.pth \
  -e MODEL_CLASS_MAP=/app/artifacts/class_to_idx.json \
  -e MODEL_NAME=mobilenetv3_small_100 \
  -e MODEL_THREADS=2 \
  -e CORS_ORIGINS="http://localhost:3000,http://$(curl -s http://checkip.amazonaws.com):3000" \
  --log-driver awslogs \
  --log-opt awslogs-region=ap-southeast-2 \
  --log-opt awslogs-group=/plant-disease/backend \
  --log-opt awslogs-create-group=true \
  ghcr.io/starkkk1/plant-disease-api@sha256:f61be1570a834f1819614ad041f79e6114e00a0503852a121a564725cad4c27c
```

> [!NOTE]
> - `MODEL_THREADS=2` giới hạn số luồng tính toán song song của PyTorch ở mức 2 core CPU, ngăn không cho các tác vụ suy luận chiếm dụng toàn bộ tài nguyên máy chủ.
> - Mô hình chỉ được nạp vào bộ nhớ một lần duy nhất khi tiến trình khởi động. Nếu tệp mô hình bị thiếu hoặc sai lệch, endpoint `/health` sẽ trả về trạng thái `unavailable` và chặn mọi yêu cầu suy luận với mã lỗi 503.

---

### 5.4.3 Cấu hình Giao diện Frontend và Mạng

Frontend được đóng gói theo mô hình **Next.js Standalone** chạy trên Node.js 20. Container thực thi bằng tài khoản người dùng bảo mật không có quyền root (`nextjs`) và phục vụ tại cổng `3000`.

Khởi chạy container frontend:

```bash
docker run -d \
  --name plant-disease-frontend \
  --restart unless-stopped \
  -p 3000:3000 \
  -e NODE_ENV=production \
  -e PORT=3000 \
  --log-driver awslogs \
  --log-opt awslogs-region=ap-southeast-2 \
  --log-opt awslogs-group=/plant-disease/frontend \
  --log-opt awslogs-create-group=true \
  ghcr.io/starkkk1/plant-disease-frontend@sha256:ec211d13241680f208b29f8282c7fb12a19735e9048d1f8dab9b6a27499a7f85
```

---

### 5.4.4 Xác minh Hoạt động của Ứng dụng

Kiểm tra trạng thái của các container và gửi yêu cầu suy luận thử nghiệm với tệp ảnh mẫu:

```bash
# 1. Kiểm tra trạng thái container đang chạy
sudo docker ps

# 2. Kiểm tra Health của Backend
curl --fail http://127.0.0.1:8000/health

# 3. Kiểm tra Header phản hồi của Frontend
curl -I http://127.0.0.1:3000

# 4. Gửi yêu cầu suy luận trực tiếp với ảnh mẫu
curl --fail-with-body \
  -F "file=@frontend/tests/fixtures/tomato-leaf.jpg;type=image/jpeg" \
  -F "top_k=3" \
  http://127.0.0.1:8000/predict
```

#### Bảng Kết quả Kiểm định Thực tế

Các kết quả đo đạc thực tế trên máy ảo Sydney được ghi nhận như sau:

| Hạng mục Kiểm tra | Lệnh / Endpoint | Kết quả Ghi nhận | Trạng thái |
|---|---|---|---|
| **Toàn vẹn Tệp Trọng số** | `sha256sum` | Cả hai tệp `model.pth` và `class_to_idx.json` trùng khớp | **ĐẠT** |
| **Trạng thái Backend** | `GET /health` | `{"status": "ok", "api_version": "1.0", "is_mock": false}` | **ĐẠT** |
| **Trạng thái Frontend** | `GET /` | `HTTP/1.1 200 OK` | **ĐẠT** |
| **Phiên bản Mô hình** | `POST /predict` | `model_version: mobilenetv3_small_100:05e72798a741` | **ĐẠT** |
| **Thời gian Suy luận** | Phản hồi API | **29.23 ms** thời gian tính toán trên CPU | **ĐẠT** |
| **Lớp Dự đoán Cao nhất** | Ảnh lá mẫu | **`Tomato___Bacterial_spot`** với độ tin cậy **0.7058** | **ĐẠT** |
| **Giao diện Đa nền tảng** | Máy tính & Điện thoại | Truy cập giao diện web mượt mà, phản hồi đầy đủ | **ĐẠT** |

![Kết quả Kiểm tra API Suy luận Trực tiếp](/images/5-Workshop/plant-disease/inference-live-fixed.png?width=750px)

Tệp ảnh mẫu được sử dụng để kiểm thử chẩn đoán bệnh (`tomato-leaf.jpg`):

![Mẫu Ảnh Lá Cà chua Kiểm thử](/images/5-Workshop/plant-disease/tomato-leaf.jpg?width=300px)
