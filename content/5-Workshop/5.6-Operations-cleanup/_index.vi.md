---
title: "Vận hành & Thu hồi"
date: 2026-10-28T10:00:00+07:00
weight: 60
chapter: false
pre: "<b>5.6. </b>"
---

# Vận hành & Thu hồi

### Quy trình Vận hành Tiết kiệm Chi phí Hằng ngày

Trong môi trường học tập, nghiên cứu hoặc thử nghiệm (PoC), việc duy trì máy chủ EC2 hoạt động 24/7 sẽ gây phát sinh chi phí tính theo giờ không cần thiết. Cần xây dựng quy trình tạm dừng và kích hoạt lại hệ thống một cách có kỷ luật:

#### 1. Tạm dừng Ứng dụng Giữa các Buổi Thực hành
Khi kết thúc phiên làm việc trong ngày, dừng các container và tạm dừng máy ảo EC2:

```bash
# Trên máy chủ EC2 qua SSH: Dừng các container ứng dụng một cách an toàn
sudo docker stop plant-disease-frontend plant-disease-api

# Từ máy trạm hoặc AWS CloudShell: Tạm dừng máy ảo EC2
aws ec2 stop-instances \
  --instance-ids "$INSTANCE_ID" \
  --region ap-southeast-2
```

> [!NOTE]
> Thao tác tạm dừng (stop) máy ảo EC2 sẽ ngắt hoàn toàn việc tính phí năng lực tính toán CPU/RAM. Ổ đĩa Amazon EBS gắn kèm, các tệp log và đối tượng lưu trên S3 vẫn được giữ nguyên trạng nhưng tiếp tục tính phí lưu trữ với mức chi phí rất thấp.

#### 2. Kích hoạt Lại Hệ thống
Khi bắt đầu buổi làm việc tiếp theo, khởi động lại máy ảo và kích hoạt các dịch vụ theo đúng trình tự phụ thuộc:

```bash
# 1. Khởi động lại máy ảo EC2
aws ec2 start-instances --instance-ids "$INSTANCE_ID" --region ap-southeast-2

# 2. Kết nối lại SSH (Lưu ý: Địa chỉ Public IPv4 có thể thay đổi nếu không dùng Elastic IP)
ssh -i key.pem ec2-user@<IP_MỚI>

# 3. Khởi chạy Backend trước và đợi hệ thống sẵn sàng
sudo docker start plant-disease-api
curl --retry 5 --retry-connrefused http://127.0.0.1:8000/health

# 4. Khởi chạy Frontend
sudo docker start plant-disease-frontend
```

> [!IMPORTANT]
> Nếu máy ảo EC2 không được gán một địa chỉ Elastic IP (EIP) tĩnh, mỗi lần stop/start máy ảo sẽ được AWS cấp một **địa chỉ Public IPv4 mới**. Cần cập nhật lại câu lệnh SSH, địa chỉ mở web trên trình duyệt và cấu hình biến `CORS_ORIGINS` của backend cho phù hợp.

---

### Xóa Tài nguyên Demo và Rà soát Chi phí Còn lại

Khi kết thúc workshop, xóa các tài nguyên được tạo riêng cho bản demo, sau đó kiểm tra AWS Billing để xem còn phát sinh phí hay không. Checklist này không xóa tài nguyên khác trong tài khoản và không bảo đảm tổng hóa đơn của tài khoản bằng 0.

```mermaid
graph TD
    Step1["1. Terminate máy EC2 của bản demo"]
    Step2["2. Xóa Log Group và dashboard CloudWatch"]
    Step3["3. Làm rỗng và xóa bucket model riêng cho dự án"]
    Step4["4. Giải phóng Elastic IP của demo nếu có"]
    Step5["5. Kiểm tra volume, snapshot và tài nguyên còn tính phí"]
    
    Step1 --> Step2
    Step2 --> Step3
    Step3 --> Step4
    Step4 --> Step5
```

#### Các Lệnh Dọn dẹp Tài nguyên Thực tế:

```bash
# 1. Xóa máy ảo EC2
aws ec2 terminate-instances \
  --instance-ids "$INSTANCE_ID" \
  --region ap-southeast-2

# 2. Xóa các nhóm CloudWatch Log Groups
aws logs delete-log-group \
  --log-group-name /plant-disease/backend \
  --region ap-southeast-2

aws logs delete-log-group \
  --log-group-name /plant-disease/frontend \
  --region ap-southeast-2

aws cloudwatch delete-dashboards \
  --dashboard-names plant-disease-demo \
  --region ap-southeast-2

# 3. Làm rỗng và xóa Bucket S3
# Lệnh này xóa các object hiện tại. Nếu bật versioning, hãy xóa các
# phiên bản object cũ trong S3 Console trước khi xóa bucket.
aws s3 rm "s3://$MODEL_BUCKET" --recursive
aws s3api delete-bucket --bucket "$MODEL_BUCKET" --region ap-southeast-2

# 4. Giải phóng địa chỉ Elastic IP (nếu có)
aws ec2 release-address --allocation-id "$EIP_ALLOC_ID" --region ap-southeast-2

# 5. Liệt kê EBS volume khả dụng và snapshot của tài khoản để rà soát.
# Chỉ xóa tài nguyên đã xác nhận thuộc bản demo; không xóa hàng loạt.
aws ec2 describe-volumes --filters Name=status,Values=available --region ap-southeast-2
aws ec2 describe-snapshots --owner-ids self --region ap-southeast-2
```

> [!WARNING]
> Xác nhận EC2, bucket S3 và Elastic IP thuộc bản demo trước khi xóa. `aws s3 rm --recursive` không xóa các phiên bản object cũ trong bucket bật versioning. Sau khi dọn dẹp, rà soát EBS volume, snapshot và kiểm tra Billing để tìm tài nguyên vẫn còn tính phí.

---

### Tài liệu Tham khảo

Các quyết định kiến trúc, tiêu chuẩn đo lường và quy trình vận hành trong workshop được căn cứ trên các tài liệu kỹ thuật chính thức:

1. **[1] Kho mã nguồn Dự án & Bản phát hành:** [starkkk1/plant-disease-detection](https://github.com/starkkk1/plant-disease-detection) — Mã nguồn đầy đủ của pipeline huấn luyện, FastAPI backend, Next.js frontend và các tệp cấu hình triển khai.
2. **[2] Vòng đời Bật/Tắt Amazon EC2:** [Tài liệu AWS về Vòng đời Máy ảo](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html) — Chi tiết về bảo toàn dữ liệu EBS và thay đổi địa chỉ IP công cộng.
3. **[3] Amazon S3 HeadObject và Phân quyền Tối thiểu:** [Thực hành An toàn trên Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html) — Phương pháp phân quyền chặt chẽ cho tác vụ tải tệp trọng số mô hình.
4. **[4] Hướng dẫn Cài đặt CloudWatch Agent:** [Tài liệu Hướng dẫn CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) — Thu thập chỉ số phần cứng bộ nhớ RAM và tỷ lệ sử dụng đĩa.
5. **[5] Giám sát vận hành bằng CloudWatch:** [Tài liệu CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) — Tài liệu tham khảo về các khả năng giám sát của CloudWatch.
