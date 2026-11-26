---
title: "Các bài blogs đã đăng"
date: 2026-10-21
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

# Các bài Blogs kỹ thuật đã chia sẻ

Trong thời gian tham gia chương trình thực tập First Cloud AI Journey, em đã đọc, phân tích và chia sẻ lại các bài tóm tắt kỹ thuật từ ba bài viết kiến trúc và Cloud/AI của AWS tới cộng đồng [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj). Mỗi bài viết đều đi sâu vào các thành phần dịch vụ AWS, cơ chế tối ưu chi phí và những lưu ý thực tế cần cân nhắc khi áp dụng.

### [Blog 1 — Tự động hóa mã hóa y khoa với Step Functions và Amazon Bedrock AgentCore](3.1-Blog1/)
Phân tích kiến trúc kết hợp **AWS Step Functions** (điều phối tất định) với AI Agent chạy trên **Amazon Bedrock AgentCore** (sử dụng công cụ MCP và **Amazon Aurora PostgreSQL + pgvector**) để tự động hóa mã hóa y khoa, đồng thời vẫn giữ dấu vết kiểm toán và cơ chế chuyển người duyệt theo điểm tự tin.

---

### [Blog 2 — Nâng cấp backend game dùng AI với các tính năng mới của Amazon DynamoDB](3.2-Blog2/)
Tổng hợp bốn cải tiến của **Amazon DynamoDB** phục vụ lưu trạng thái cho backend game tích hợp AI gồm **Multi-Region Strong Consistency (MRSC)**, **Zero-ETL với Amazon SageMaker Lakehouse**, **LangGraph DynamoDBSaver** và **giảm giá thông lượng**, kèm kiểm chứng các giới hạn kỹ thuật và số liệu chi phí.

---

### [Blog 3 — Giải quyết tín hiệu bảo mật nhanh hơn với Aurora Agentic SOC của Arctic Wolf](3.3-Blog3/)
Phân tích giải pháp **Aurora Agentic SOC** của Arctic Wolf trên AWS Marketplace với đường ống nạp dữ liệu hướng sự kiện (**Security Hub CSPM &rarr; EventBridge &rarr; Data Firehose &rarr; S3 &rarr; Lambda**) và cách phân công hợp lý giữa AI agent (phân loại, điều tra khối lượng lớn) và chuyên gia bảo mật con người.
