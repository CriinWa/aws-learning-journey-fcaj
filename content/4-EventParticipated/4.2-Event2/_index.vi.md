---
title: "Event 2"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---
# Bài thu hoạch: FCAJ — Agentic AI Build Week

### Tổng quan sự kiện

**FCAJ — Agentic AI Build Week** là buổi tổng kết và chia sẻ kinh nghiệm từ cuộc thi *Agentic AI Build Week*. Đây là sân chơi thực chiến, nơi kỹ sư và sinh viên công nghệ lập nhóm, làm việc dưới áp lực thời gian từ 24 đến 48 giờ để xây dựng các sản phẩm AI Agent giải quyết bài toán thực tế của doanh nghiệp.

Các đội thi hoàn thiện sản phẩm mẫu (*demo*) và trực tiếp trình bày (*pitching*) giải pháp trước hội đồng giám khảo. Sự kiện cho thấy AI Agent có thể được ứng dụng trong thương mại, phân tích dữ liệu, thiết kế kiến trúc, điều phối vận hành và tài chính.

### Mục đích của sự kiện

- Khuyến khích người trẻ và những người làm công nghệ bước ra khỏi vùng an toàn, kết hợp kiến thức lý thuyết với trải nghiệm thực hành (*hands-on experience*).
- Phát triển kỹ năng làm việc nhóm, chia sẻ kiến thức, tư duy sản phẩm và hiểu biết về cách tiếp cận nhà đầu tư.
- Khẳng định vai trò của thế hệ kỹ sư mới trong việc ứng dụng AI và công nghệ đám mây để đổi mới cách làm việc và tăng tốc sáng tạo.

### Danh sách diễn giả và đội thi

- **Ông Nguyễn Gia Hưng** — Head of Solution Architects tại AWS Việt Nam.
- **Ông Joseph Barazota** — Head of Technology tại Asian.
- **Các đội thi xuất sắc:** One Team, Signal Scout, Plan, 3K và Six Pillars.

### Nội dung nổi bật

#### Các giải pháp AI Agent từ đội thi

- **One Team — Giải Nhất:** Xây dựng chatbot AI đặt món KFC trên Zalo và WhatsApp. Khách hàng không cần cài ứng dụng hoặc tạo tài khoản; AI Agent sử dụng AWS AgentCore để duy trì ngữ cảnh hội thoại, hỗ trợ xử lý đơn hàng tự nhiên và giảm rủi ro chốt sai đơn do *hallucination*.
- **Signal Scout — Giải Nhì:** Ứng dụng kiến trúc **multi-agent** để tự động thu thập dữ liệu, phân tích báo cáo tài chính và cấu trúc của đối thủ, từ đó đưa ra gợi ý và dự báo ROI cho đội ngũ chiến lược.
- **Plan:** Phát triển giải pháp AI-native giúp Solution Architect tạo sơ đồ kiến trúc trên Draw.io, ước tính chi phí và sinh Infrastructure as Code bằng Terraform từ yêu cầu ngôn ngữ tự nhiên.
- **3K:** Kết hợp Computer Vision với YOLO và GenAI để theo dõi luồng người tại sân bay hoặc siêu thị theo thời gian thực, phát hiện khu vực ùn tắc và đề xuất phương án điều phối nhân sự.
- **Six Pillars:** Xây dựng quy trình điều tra chống rửa tiền (*Adaptive AML Workflow*) cho ngân hàng bằng kiến trúc multi-agent. Các Agent chuyên biệt hỗ trợ kiểm tra KYC, dòng tiền và yếu tố pháp lý, giúp rút ngắn thời gian xử lý cảnh báo gian lận từ nhiều giờ xuống chỉ còn vài phút.

#### Tư duy xây dựng sản phẩm

- Cần xác định **pain point** trước khi lựa chọn công nghệ. Giá trị của sản phẩm nằm ở vấn đề mà nó giải quyết, đối tượng sử dụng và khả năng vận hành kinh doanh.
- Trong thời gian ngắn, **quản lý phạm vi** (*scope management*) là yếu tố then chốt: chọn đúng tính năng lõi của MVP và tránh đưa quá nhiều tính năng khiến sản phẩm không thể hoàn thiện.
- Ý tưởng chỉ có giá trị khi được thực thi. Một sản phẩm demo vận hành được là bằng chứng rõ ràng nhất cho tính khả thi của giải pháp.

#### Kiến trúc AI Agent và vận hành hệ thống

- Với bài toán phức tạp, không nên giao toàn bộ trách nhiệm cho một AI Agent vì dễ dẫn đến quá tải ngữ cảnh và *hallucination*.
- Nên phân tách thành các **Agent chuyên biệt**, ví dụ Agent thu thập dữ liệu, Agent phân tích và Agent đánh giá; sau đó dùng một Agent điều phối để quản lý luồng công việc.
- Cần tính toán chi phí vận hành ngay từ đầu. Việc linh hoạt chuyển từ API bên thứ ba sang dịch vụ AWS phù hợp có thể giảm đáng kể chi phí cho giải pháp.
- Với các lĩnh vực nhạy cảm như tài chính hoặc điều phối đám đông, cần thiết kế quy trình **human-in-the-loop**: AI tổng hợp chứng cứ và đề xuất, còn con người là người phê duyệt quyết định cuối cùng.

### Những gì học được

#### Tư duy thiết kế

- Luôn bắt đầu từ nhu cầu của người dùng và bài toán doanh nghiệp, thay vì chỉ tập trung vào việc sử dụng công nghệ mới.
- Biết cân bằng giữa tính năng, thời gian và chất lượng để hoàn thành MVP đúng hạn.
- Ưu tiên xây dựng sản phẩm có thể trình diễn, kiểm chứng và nhận phản hồi thực tế.

#### Kiến trúc kỹ thuật

- Hiểu cách phân chia vai trò trong kiến trúc **multi-agent** để giảm tải ngữ cảnh và tăng khả năng kiểm soát kết quả.
- Nhận thức được tầm quan trọng của tối ưu chi phí đám mây khi lựa chọn dịch vụ và mô hình triển khai.
- Áp dụng **human-in-the-loop** và các cơ chế kiểm soát đầu ra cho những quyết định có rủi ro cao.

### Ứng dụng vào công việc

- Trước khi triển khai dự án, xác định rõ khách hàng mục tiêu, vấn đề cần giải quyết và chi phí vận hành dự kiến.
- Áp dụng AI để tự động hóa các bước thu thập thông tin, rà soát tài liệu và cảnh báo; đồng thời thiết lập **guardrails** để kiểm chứng độ tin cậy của đầu ra.
- Khi thiết kế hệ thống AI, chia nhỏ quy trình thành các tác vụ có thể kiểm soát và duy trì bước phê duyệt của con người đối với các kết quả quan trọng.

### Trải nghiệm trong sự kiện

#### Học hỏi từ các chuyên gia

Chia sẻ của ông Joseph Barazota giúp tôi hiểu rõ hơn sự thay đổi trong tư duy hệ thống. Nếu trước đây đội ngũ kỹ thuật thường hạn chế thay đổi để duy trì ổn định, thì trong thời đại AI, năng lực thử nghiệm và triển khai nhanh có thể tạo lợi thế lớn. Người trẻ cần chủ động thử thách các giới hạn cũ và tích lũy trải nghiệm thực tế.

#### Góc nhìn thực tế về phát triển sản phẩm

Hackathon cho thấy việc xây dựng sản phẩm luôn đi kèm áp lực về thời gian, mâu thuẫn trong nhóm và các sự cố kỹ thuật bất ngờ, như hết token AI, mất kết nối Wi-Fi khi demo hoặc vô tình đưa tệp `.env` lên GitHub. Những tình huống này đòi hỏi sự cẩn thận, ghi chép quy trình và tinh thần phối hợp vì mục tiêu chung.

Khi trình bày giải pháp, đội thi cần giải thích được chi phí, lý do chọn dịch vụ và các đánh đổi trong kiến trúc. Điều này nhắc nhở tôi rằng cần hiểu sâu hệ thống mình xây dựng, không chỉ dừng ở việc sản phẩm hoạt động.

#### Bài học rút ra

- **“Done beats perfect”**: hoàn thành một sản phẩm có thể sử dụng quan trọng hơn việc theo đuổi sự hoàn hảo nhưng không thể triển khai.
- Kỹ năng công nghệ có thể học liên tục, nhưng trải nghiệm vượt qua áp lực cùng đội nhóm và trực tiếp sửa lỗi là những giá trị giúp trưởng thành nhanh.
- Cần duy trì tinh thần **học tập suốt đời** vì công nghệ và phương pháp làm việc luôn thay đổi.

#### Một số hình ảnh khi tham gia sự kiện

<div style="display: flex; gap: 12px; justify-content: space-between; align-items: center; margin: 15px 0;">
	{{< event-image src="/images/4-EventParticipated/event2-1.jpg" alt="Hình ảnh sự kiện 2 - 1" >}}
	{{< event-image src="/images/4-EventParticipated/event2-2.jpg" alt="Hình ảnh sự kiện 2 - 2" >}}
	{{< event-image src="/images/4-EventParticipated/event2-3.jpg" alt="Hình ảnh sự kiện 2 - 3" >}}
</div>
