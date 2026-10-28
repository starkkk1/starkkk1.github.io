---
title: "5.2 Điều kiện chuẩn bị"
date: 2026-10-28T10:00:00+07:00
weight: 20
chapter: false
pre: "<b>5.2. </b>"
---

# Điều kiện chuẩn bị & Thiết lập Môi trường

Trước khi triển khai ứng dụng Nhận diện Bệnh lá Cây trồng lên Amazon Web Services, cần chuẩn bị đầy đủ tài khoản AWS, quyền hạn IAM, cấu hình mạng VPC và cài đặt các công cụ cần thiết.

---

### Ma trận Điều kiện & Yêu cầu Kỹ thuật

Bảng dưới đây tổng hợp các yêu cầu cấu hình thực tế được ghi nhận trong quá trình triển khai:

| Nhóm Yêu cầu | Cấu hình Tiêu chuẩn | Mục đích & Chi tiết Kỹ thuật |
|---|---|---|
| **Tài khoản AWS** | Tài khoản AWS Cá nhân | Đảm bảo môi trường thực hành độc lập, kiểm soát chi phí tối ưu |
| **Khu vực AWS (Region)** | Sydney (`ap-southeast-2`) | Độ trễ thấp, hỗ trợ đầy đủ các dịch vụ EC2, S3, CloudWatch và SNS |
| **Máy chủ Ảo (EC2)** | Amazon EC2 (Amazon Linux 2023 x86_64) | Máy chủ chạy Docker Engine, AWS CLI v2 để điều phối container |
| **Mạng & VPC** | Public Subnet (`0.0.0.0/0` -> Internet Gateway) | Cung cấp đường truyền tải gói phần mềm và phục vụ web; gán Public IPv4 |
| **Bảo mật Cổng vào** | Cổng `22` (SSH), Cổng `3000` (Frontend), Cổng `8000` (API) | Giới hạn duy nhất tới địa chỉ IP công cộng của người vận hành (`/32`) |
| **Lưu trữ Trọng số** | Amazon S3 (Bucket Riêng tư - Private) | Lưu tệp checkpoint mô hình dưới tiền tố `mobilenet-distilled-v1/` |
| **IAM Instance Role** | EC2 Instance Profile | Cấp quyền `s3:GetObject` trên tệp mô hình và `CloudWatchAgentServerPolicy` |
| **Quyền Truy vấn Log** | `logs:FilterLogEvents` | Cho phép người vận hành truy vấn log container qua AWS CLI |
| **Tính toàn vẹn Model** | Tệp `model.pth` và `class_to_idx.json` | Kiểm tra tính toàn vẹn bằng mã băm mật mã học SHA256 |

> [!WARNING]
> Tuyệt đối KHÔNG mở cổng `0.0.0.0/0` cho SSH (cổng 22) hoặc cổng API. Luôn giới hạn IP nguồn vào địa chỉ IP tĩnh của bạn để ngăn chặn nguy cơ xâm nhập trái phép.

---

### Bước 1: Cấu hình AWS CLI & Xác thực Danh tính

Đảm bảo máy trạm vận hành đã cài đặt **AWS CLI v2** và đăng nhập tài khoản có quyền quản trị cần thiết:

![Cấu hình AWS CLI](/images/5-Workshop/5.2-Prerequisite/aws_cli_configure.png?width=700px)

Kiểm tra danh tính đang đăng nhập và đặt khu vực mặc định là Sydney:

```bash
# Kiểm tra danh tính người dùng AWS
aws sts get-caller-identity

# Đặt region mặc định là ap-southeast-2 (Sydney)
export AWS_DEFAULT_REGION="ap-southeast-2"
aws configure set default.region ap-southeast-2
```

---

### Bước 2: Thiết lập Security Group cho EC2

Tạo một Security Group có tên `plant-disease-sg` trong VPC mặc định với các quy tắc Inbound sau:

| Loại | Giao thức | Dải cổng | Nguồn IP | Mục đích |
|---|---|---|---|---|
| SSH | TCP | `22` | `<IP_CỦA_BẠN>/32` | Truy cập dòng lệnh quản trị qua SSH |
| Custom TCP | TCP | `3000` | `<IP_CỦA_BẠN>/32` | Giao diện Web Next.js Frontend |
| Custom TCP | TCP | `8000` | `<IP_CỦA_BẠN>/32` | Dịch vụ API suy luận FastAPI Backend |

---

### Bước 3: Cấu hình IAM Instance Role & Phân quyền

Gắn một IAM Role vào máy chủ EC2 để các container và tập lệnh nội bộ có thể truy cập Amazon S3 và CloudWatch **mà không cần lưu trữ AWS Access Key hay Secret Key** trên máy ảo.

Gắn các chính sách phân quyền sau vào IAM Role:
1. **AmazonCloudWatchAgentServerPolicy** (Chính sách do AWS quản lý): Cho phép CloudWatch Agent gửi metrics bộ nhớ, đĩa và tạo log stream.
2. **PlantDiseaseS3Policy** (Chính sách nội tuyến): Giới hạn quyền đọc nghiêm ngặt chỉ trong thư mục tệp mô hình:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ModelArtifactsDownload",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::plant-disease-model-artifacts-ap-southeast-2",
        "arn:aws:s3:::plant-disease-model-artifacts-ap-southeast-2/mobilenet-distilled-v1/*"
      ]
    },
    {
      "Sid": "AllowOperatorLogInspection",
      "Effect": "Allow",
      "Action": [
        "logs:FilterLogEvents",
        "logs:GetLogEvents",
        "logs:DescribeLogStreams"
      ],
      "Resource": "arn:aws:logs:ap-southeast-2:*:log-group:/plant-disease/*"
    }
  ]
}
```

![Chính sách IAM](/images/5-Workshop/5.4-Deployment-strategy/iam_audit_role_trust_policy.png?width=700px)

---

### Bước 4: Khởi tạo Phần mềm trên Máy chủ EC2

Sau khi SSH vào máy ảo Amazon Linux 2023, tiến hành cài đặt Docker Engine và CloudWatch Agent:

```bash
# Cập nhật các gói phần mềm trên Amazon Linux 2023
sudo dnf update -y

# Cài đặt Docker và Git
sudo dnf install -y docker git
sudo systemctl enable --now docker
sudo usermod -aG docker ec2-user

# Cài đặt CloudWatch Agent
sudo dnf install -y amazon-cloudwatch-agent

# Kiểm tra các phiên bản
docker --version
aws --version
```
