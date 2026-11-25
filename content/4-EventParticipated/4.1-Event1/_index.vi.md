---
title: "Event 1"
date: 2026-10-24
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Sáng Thứ Bảy trên tầng 26 Bitexco: Một buổi sáng, năm phiên chia sẻ và vài thói quen cần thay đổi

> **Sự kiện:** First Cloud AI Journey  
> **Thời gian:** 09:00 – 12:00, Thứ Bảy ngày 24/10/2026  
> **Địa điểm:** Tầng 26, Tòa nhà Bitexco Financial Tower, Thành phố Hồ Chí Minh  
> **Vai trò:** Người tham dự

Sáng thứ Bảy ngày 24/10, em lên tầng 26 tòa nhà Bitexco Financial Tower tham dự buổi meetup **"First Cloud AI Journey"**. Chỉ gói gọn trong ba tiếng (từ 9:00 đến 12:00 trưa), sự kiện có tới năm phiên chia sẻ liên tiếp về Cloud và AI Agents do các Tech Lead và kỹ sư đang làm việc thực tế trình bày. Với tư cách sinh viên, em đến chủ yếu để lắng nghe và cảm nhận không khí ngành. Bầu không khí khá thoải mái, nhưng cứ hễ đến phần demo trực tiếp hay hỏi đáp (Q&A) là cả phòng lại im phăng phắc vì ai cũng tập trung ghi chép.

---

### Những Điểm Đáng Nhớ Nhất

- **Màn Demo Voice AI:** Phiên mở đầu trình bày về việc xây dựng tổng đài thoại tiếng Việt trên AWS. Diễn giả nhấn mạnh yếu tố độ trễ (latency) — chỉ cần trễ một tích tắc là cuộc trò chuyện sẽ mất tự nhiên ngay. Điểm nhấn là màn demo trực tiếp khi AI gọi điện thật để thực hiện cuộc phỏng vấn sàng lọc ban đầu cho vị trí cloud engineer bằng tiếng Việt. Nghe hệ thống đối thoại trôi chảy như vậy cho thấy công nghệ này đã bước ra khỏi slide thuyết trình để vào vận hành thực tế.
- **Hai Phiên Về Bảo Mật:** Phiên đầu tiên tập trung vào chia sẻ tài nguyên liên tài khoản (cross-account) trong một AWS Organization, nhấn mạnh việc phân định ranh giới quyền hạn chặt chẽ để không người dùng nào vượt quá phạm vi được phép. Phiên thứ hai đề cập đến việc đưa bảo mật hạ tầng lên sớm hơn trong quy trình (shift-left): quét Infrastructure-as-Code (IaC) ngay tại bước pull request và triển khai một agent đóng vai người dùng đối kháng để dò tìm lỗ hổng trên môi trường staging trước khi đưa lên production. Cả hai đều có chung một thông điệp: phát hiện rủi ro càng sớm, chi phí khắc phục càng thấp.
- **Đưa Agents Lên Môi Trường Production:** Phiên chia sẻ về **Amazon Bedrock AgentCore** đi qua cách vận hành agent trong môi trường production và quản lý bộ nhớ của agent — tách bạch bộ nhớ ngắn hạn và dài hạn để agent giữ được đúng ngữ cảnh người dùng cần thiết.
- **Kiro Crew và Phiên Q&A Cuối Chương Trình:** Đây là phiên em thích nhất, tập trung vào việc chạy các AI Agent liên tục để hỗ trợ review code và kiểm tra hệ thống. Phần Q&A cuối buổi mang tính thực chiến cao nhất: các diễn giả chia sẻ cách xử lý khi tràn cửa sổ ngữ cảnh (context window), theo dõi lượng token tiêu thụ theo từng thành phần, cắt tỉa bộ nhớ cũ để tránh phình chi phí và thiết lập các hàng rào bảo vệ (guardrails) để AI không vô tình xóa hạ tầng hoặc làm rò rỉ dữ liệu nhạy cảm.

---

### 5 Thói Quen Em Muốn Thay Đổi Sau Buổi Sáng Hôm Đó

1. **Agent càng tự chủ, guardrails càng phải chặt:** Xuyên suốt mọi bài nói — từ phân quyền IAM đến guardrails — những người làm hệ thống nghiêm túc luôn hỏi *"Nếu sai sót xảy ra thì sao?"* trước khi hỏi *"Nó làm được gì?"*. Với dự án trợ lý lập trình nhỏ của mình, em sẽ rà soát toàn bộ access key, đảm bảo không để bất kỳ secret nào nằm trong repository và giới hạn quyền của công cụ ở mức tối thiểu (least privilege).
2. **Ngữ cảnh (context) và bộ nhớ là tài nguyên đắt đỏ:** Trước đây em thường dán nguyên khối dữ liệu lớn vào prompt vì nghĩ càng nhiều thông tin càng tốt. Nghe phần Q&A giúp em nhận ra ngữ cảnh phình to vừa tốn tiền vừa dễ làm mô hình lạc hướng. Từ nay em sẽ chia nhỏ tác vụ, mở phiên mới cho từng đầu việc và chỉ đưa vào những gì thực sự liên quan.
3. **Đưa kiểm tra bảo mật lên đầu quy trình:** Nhìn thấy cách bắt lỗi ngay ở bước review code khiến em xem lại thói quen làm cho chạy được trước rồi mới tính chuyện bảo mật sau. Em muốn rèn thói quen rà soát cấu hình hạ tầng và danh sách phân quyền trước mỗi lần triển khai — kể cả với các bản dựng thử nghiệm.
4. **Học từ những người đang xây dựng hệ thống thật:** Nghe các kỹ sư chia sẻ về những điểm nghẽn thực tế giúp em thấy khoảng cách giữa bài tập trên lớp và hệ thống production lớn đến mức nào. Em dự định chọn một chủ đề từ buổi meetup — chẳng hạn xây dựng một agent gọn nhẹ có logging đầy đủ — và ghi chép lại quá trình tự tìm hiểu.
5. **Chủ động hơn trong việc kết nối cộng đồng:** Em vẫn còn hơi ngại bắt chuyện với người lạ, nhưng buổi sáng hôm đó nhắc em nhớ rằng cộng đồng chỉ mang lại giá trị khi mình thực sự bước vào trao đổi. Lần tham dự tới, mục tiêu của em là đặt ít nhất một câu hỏi trong phần Q&A và tự giới thiệu bản thân với hai người bạn mới.

---

### Tổng Kết & Hành Động Tiếp Theo

Em rời Bitexco với cái đầu hơi quá tải theo hướng tích cực nhất. Điều đọng lại không phải là một công cụ cụ thể nào, mà là tư duy kỹ thuật: làm cho AI chạy được mới chỉ là bước một; giữ cho nó an toàn, tiết kiệm chi phí và nằm trong tầm kiểm soát mới là công việc thực sự.

Tuần này, em bắt đầu với hai việc cụ thể: rà soát phân quyền và secrets trong dự án hiện tại, đồng thời tập quản lý ngữ cảnh gọn gàng hơn mỗi khi làm việc với AI. Nếu bạn cũng đang bắt đầu hành trình cloud của mình, hãy thử tham gia một buổi meetup như thế này — ba tiếng sáng thứ Bảy mang lại nhiều giá trị hơn em từng nghĩ rất nhiều.
