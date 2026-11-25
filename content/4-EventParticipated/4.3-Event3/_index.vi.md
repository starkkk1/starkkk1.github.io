---
title: "Event 3"
date: 2026-11-24
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Những Điều Em Rút Ra Khi Nghe Tiến sĩ Werner Vogels Chia Sẻ

> **Sự kiện:** Navigating the Future of Cloud & AI in Vietnam (Keynote của Dr. Werner Vogels)  
> **Thời gian:** Năm 2026  
> **Địa điểm:** Thành phố Hồ Chí Minh, Việt Nam  
> **Vai trò:** Người tham dự

Em đã tham dự sự kiện **"Navigating the Future of Cloud & AI in Vietnam"** để lắng nghe **Tiến sĩ Werner Vogels** (CTO của Amazon) chia sẻ về tương lai của ngành kỹ thuật phần mềm. Là một sinh viên vừa làm bài tập trên lớp vừa phát triển các dự án cá nhân, câu hỏi *"Liệu AI có thay thế lập trình viên không?"* đối với em là một trăn trở rất thực tế. Nhìn quanh khán phòng, ai cũng chăm chú ghi chép — và mỗi lần diễn giả nhắc đến AI, cả hội trường lại im ắng hẳn xuống.

---

### Những Điều Đọng Lại Sâu Sắc Nhất

- **Sự Cố Ngày Mua Sắm Cao Điểm:** Ông kể lại câu chuyện từ những ngày đầu của Amazon khi cơ sở dữ liệu quan hệ bị quá tải trong mùa mua sắm lễ hội, gây sập hệ thống suốt cả ngày và thiệt hại nặng nề. Điều nổi bật nhất là thái độ xử lý: khi nhà cung cấp phần mềm chỉ ra rằng Amazon đã đẩy sản phẩm vượt quá xa giới hạn thiết kế, Vogels thẳng thắn thừa nhận họ đúng. Thay vì chắp vá tạm bợ, đội ngũ kỹ sư đã lùi lại để đo đạc cách hệ thống thực sự được sử dụng. Họ nhận ra phần lớn truy vấn chỉ là tra cứu key-value đơn giản mà không cần đến một cơ sở dữ liệu quan hệ nặng nề. Phát hiện đó đã dẫn đến sự ra đời của **Dynamo**, và sau này là **Amazon DynamoDB**.
- **Kỷ Luật Vận Hành (Operational Discipline):** Theo Vogels, chỉ dựa vào giá trị trung bình là cách dễ nhất để tự đánh lừa mình, vì con số trung bình che lấp trải nghiệm tệ hại của những người dùng ở hai đầu cực. Ông nhấn mạnh tư duy *"mọi thứ đều có thể hỏng bất cứ lúc nào"* (*everything fails all the time*) và giải thích cách Amazon chủ động diễn tập bằng việc cố tình tắt một phần hạ tầng đang chạy thật để kiểm chứng khả năng phục hồi của hệ thống.
- **Hình Mẫu "Renaissance Developer":** Đây là phần em tâm đắc nhất. Lập trình viên sẽ không bị lỗi thời nếu phát triển bản thân xoay quanh năm phẩm chất: **tính tò mò, tư duy hệ thống, tinh thần làm chủ (ownership), kiến thức rộng kết hợp chuyên môn sâu, và kỹ năng giao tiếp**. Ông ví AI như một trình biên dịch ngôn ngữ tự nhiên — cực kỳ nhanh nhưng dễ sai sót, đồng nghĩa đầu ra của nó luôn cần con người kiểm chứng.

---

### 5 Bài Học Em Áp Dụng Vào Công Việc Của Mình

1. **AI viết code, nhưng chính mình chịu trách nhiệm:** Trước đây em từng copy code do AI sinh ra, chạy thử nhanh rồi đẩy thẳng vào dự án nếu thấy chạy ổn. Sau buổi nói chuyện này, em đặt ra nguyên tắc cứng: không bao giờ commit một dòng code nào mà bản thân không giải thích được. Trước khi chấp nhận code từ AI, em cần kiểm tra các trường hợp biên (edge cases), lỗ hổng bảo mật và điểm nghẽn hiệu năng. Nếu chưa hiểu nó, em không thể nhận đó là sản phẩm của mình.
2. **Nhìn toàn bộ hệ thống, không chỉ các tính năng rời rạc:** Phần lớn bài tập ở trường chỉ tập trung vào từng module tách biệt. Sắp tới, em sẽ lấy một dự án cá nhân, vẽ lại toàn bộ luồng dữ liệu từ giao diện xuống cơ sở dữ liệu và tự hỏi: *điều gì xảy ra nếu thành phần đơn lẻ này chết?* Các sự cố nghiêm trọng gần như luôn bắt nguồn từ những giả định chưa từng được kiểm chứng.
3. **Hiểu rõ workload thực tế trước khi chọn công nghệ:** Em thường chọn công cụ chỉ vì chúng đang phổ biến hoặc theo xu hướng. Câu chuyện của Amazon nhắc em phải bắt đầu từ mô hình sử dụng thực tế. Từ nay, trước khi chọn cơ sở dữ liệu hay framework, em sẽ liệt kê rõ các dạng truy vấn và xử lý dữ liệu mà ứng dụng thực sự cần.
4. **Coi trọng kỹ năng giao tiếp:** Em từng xem giao tiếp là kỹ năng phụ, nhưng giờ em thấy nó giúp kỹ sư bóc tách đúng vấn đề cốt lõi đằng sau những yêu cầu mơ hồ và giải thích được các đánh đổi kỹ thuật cho những người không làm chuyên môn. Em sẽ rèn luyện cách viết tài liệu ngắn gọn cho công việc của mình và chủ động thuyết trình nhiều hơn trong các dự án nhóm.
5. **Cảnh giác với câu *"trước giờ tụi mình vẫn làm thế"*:** Tiến sĩ Werner Vogels khép lại bằng lời cảnh báo nổi tiếng của Grace Hopper về câu nói nguy hiểm nhất trong ngôn ngữ. Với em, đó là lời nhắc phải định kỳ rà soát và làm mới cách mình học tập cũng như viết code.

---

### Tổng Kết & Hành Động Tiếp Theo

Em bước ra khỏi khán phòng với tâm trạng nhẹ nhõm hơn nhiều so với nỗi lo ban đầu. Điều đọng lại không phải là một lời trấn an sáo rỗng kiểu "đừng lo về AI", mà là cái nhìn rõ ràng về những gì AI vẫn chưa thể thay thế: **khả năng phán đoán chuẩn xác, tư duy hệ thống, tinh thần chịu trách nhiệm và niềm tự hào nghề nghiệp**. Đó chính xác là những kỹ năng em cần mài giũa bắt đầu từ hôm nay.

Bước đầu tiên trong tuần này của em là vẽ lại sơ đồ kiến trúc cho dự án hiện tại và rà soát từng đoạn code trước đây em từng để AI viết giúp. Nếu bạn cũng đang ở giai đoạn đầu trên hành trình kỹ sư phần mềm như em, hy vọng những ghi chép này sẽ hữu ích.
