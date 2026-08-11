---
title: "Event 3"
date: 2026-08-08
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
---
# Bài thu hoạch: AWS FCAJ Agent Forge – Deepdive (Ngày 2)

### Tổng quan sự kiện

AWS FCAJ Agent Forge – Deepdive (Ngày 2) là buổi học thứ hai trong chuỗi workshop chuyên sâu do cộng đồng First Cloud AI Journey (FCAJ) phối hợp tổ chức. Chương trình thuộc cấp độ nâng cao (L300 – Advanced), được xây dựng dựa trên tài liệu và kinh nghiệm thực tế của các kỹ sư AWS.

Buổi học tập trung vào việc phát triển một AI Agent từ mức cơ bản thành hệ thống Agentic AI có khả năng vận hành trong môi trường doanh nghiệp. Các chủ đề chính gồm quản lý bộ nhớ, đánh giá chất lượng phản hồi, giám sát hệ thống và tối ưu hiệu suất.

### Diễn giả và người hướng dẫn

- **Anh Hiếu:** Co-head cộng đồng FCAJ, Solution Architect tại AWS Việt Nam.
- **Anh Hải Anh:** Cloud Consultant tại Chiase Pacific, trực tiếp hướng dẫn phần thực hành lab.

### Mục đích của sự kiện

- Hiểu cách quản lý trạng thái hội thoại dài hạn bằng Short-term Memory và Long-term Memory.
- Tiếp cận mô hình Observability dựa trên chuẩn OpenTelemetry, gồm Logs, Traces và Metrics.
- Tìm hiểu cơ chế Evaluation tự động nhằm đánh giá chất lượng phản hồi, giảm rủi ro ảo giác và lỗi lập luận của Agent.
- Nhận thức được yêu cầu về bảo mật, chi phí và khả năng vận hành ổn định khi đưa Agent vào môi trường production.

### Workshop

Chuỗi Agent Forge Deepdive được triển khai trong ba ngày, theo lộ trình từ kiến thức nền tảng đến thực hành triển khai Agent với Amazon Bedrock AgentCore.

1. **Ngày 1 (01/08): AgentCore Foundations** — kiến trúc tổng quan, Runtime, Gateway và Identity.
2. **Ngày 2 (08/08): Memory, Evaluations, Observability & Optimization** — quản lý bộ nhớ, đánh giá Agent, giám sát và tối ưu hiệu suất.
3. **Ngày 3 (15/08): DevOps, Policies & Production Best Practices** — DevOps, policies, bảo mật và các thực hành production.

### Nội dung nổi bật

#### Quản lý bộ nhớ của Agent

Agent Memory giúp Agent vượt qua giới hạn của Context Window, duy trì ngữ cảnh hội thoại và cá nhân hóa trải nghiệm của người dùng.

**Short-term Memory** là bộ nhớ ngắn hạn, lưu trữ đồng bộ toàn bộ lịch sử hội thoại dưới dạng các tin nhắn thô. Nhờ đó, Agent có thể hiểu mạch trao đổi hiện tại và phản hồi nhất quán. Hệ thống cũng hỗ trợ cơ chế rẽ nhánh (branching), tương tự như cách Git tạo nhánh trong quá trình phát triển phần mềm.

**Long-term Memory** là bộ nhớ dài hạn, hoạt động bất đồng bộ. Hệ thống trích xuất những thông tin quan trọng từ hội thoại và lưu dưới dạng vector để có thể truy xuất trong các phiên sau. Bốn chiến lược lưu trữ chính gồm:

- **Summary:** tóm tắt và nén nội dung hội thoại.
- **User Preference:** lưu trữ sở thích của người dùng.
- **Semantic:** lưu trữ tri thức chuyên ngành.
- **Episodic:** lưu lại các quyết định hoặc sự kiện đã diễn ra.

**Namespace** được sử dụng như một cấu trúc thư mục phân cấp để cô lập dữ liệu theo strategy, actor hoặc session. Khi kết hợp semantic search và similarity ranking, Agent có thể tìm đúng thông tin cần thiết, giảm lượng token sử dụng và cải thiện thời gian phản hồi.

#### Khả năng quan sát hệ thống

Workshop nhấn mạnh nguyên tắc: *“You cannot fix what you cannot see”* — không thể khắc phục vấn đề nếu không quan sát được vấn đề đó. Hệ thống Observability sử dụng chuẩn OpenTelemetry để thu thập ba nhóm dữ liệu chính:

- **Logs:** ghi lại chi tiết về request, lỗi kết nối, lỗi hệ thống hoặc log từ terminal.
- **Traces:** theo dõi toàn bộ hành trình của một request, từ khi người dùng gửi prompt đến khi Agent trả về phản hồi, bao gồm các tool call.
- **Metrics:** đo lường các chỉ số như mức tiêu thụ token, tỷ lệ lỗi và độ trễ phản hồi.

Những dữ liệu này giúp đội ngũ phát triển xác định nguyên nhân gây chậm trễ, tối ưu chi phí token và cải thiện trải nghiệm người dùng.

#### Hệ thống đánh giá Agent

Một rủi ro phổ biến của AI Agent là hiện tượng *hallucination*, tức đưa ra thông tin không chính xác nhưng thể hiện như sự thật. Để hạn chế rủi ro này, hệ thống cung cấp 13 evaluator tích hợp sẵn, chẳng hạn như *correctness* và *helpfulness*.

Các evaluator được áp dụng ở ba cấp độ:

- **Session level:** đánh giá kết quả của toàn bộ phiên làm việc.
- **Trace level:** đánh giá độ chính xác của phản hồi.
- **Span level:** đánh giá từng bước xử lý, chẳng hạn như việc gọi tool hoặc truyền tham số.

Hệ thống hỗ trợ hai hình thức đánh giá. **On-demand** phù hợp với giai đoạn phát triển và thử nghiệm; **Online** được sử dụng để theo dõi chất lượng Agent theo thời gian thực trong môi trường production. Kết quả đánh giá tự động vẫn cần được chuyên gia lĩnh vực kiểm chứng để bảo đảm tính chính xác.

### Những gì học được

#### Kiến thức chuyên môn

- Hiểu rõ sự khác biệt giữa Short-term Memory và Long-term Memory, đặc biệt là cơ chế xử lý đồng bộ và bất đồng bộ.
- Nắm được ba trụ cột của Observability là Logs, Traces và Metrics, cùng vai trò của chuẩn OpenTelemetry trong việc theo dõi sức khỏe hệ thống.
- Hiểu cách các evaluator tự động đánh giá phản hồi của Agent theo tiêu chí chuẩn hóa thay vì dựa hoàn toàn vào cảm nhận chủ quan.
- Biết thêm về Cedar Policy và cơ chế sandbox, qua đó nhận thức rõ vai trò của bảo mật khi Agent thực hiện tác vụ hoặc thử nghiệm mã nguồn.

#### Bài học kinh nghiệm

Điểm em ấn tượng nhất là tư duy **“6/94”**. Trong một hệ thống Agentic AI thực tế, mô hình AI chỉ chiếm một phần nhỏ; phần lớn công việc thuộc về kỹ nghệ phần mềm, như xây dựng bộ nhớ, quản lý định danh, bảo mật gateway, giám sát, đánh giá và tối ưu hệ thống.

Bên cạnh đó, hạ tầng hoạt động ổn định chưa chắc đã mang lại trải nghiệm tốt cho người dùng. Dù máy chủ không gặp sự cố, Agent vẫn có thể phản hồi chậm hoặc đưa ra kết quả chưa phù hợp. Vì vậy, cần theo dõi độ trễ và chất lượng phản hồi ở cả cấp độ hạ tầng, ứng dụng và mô hình AI.

Cuối cùng, khi xây dựng Agent cần tuân thủ nguyên tắc **đặc quyền tối thiểu** (*Least Privilege*). Việc áp dụng Cedar Policy và sandbox giúp giới hạn quyền truy cập, giảm nguy cơ Agent thực hiện hành động không mong muốn hoặc ảnh hưởng đến tài nguyên hệ thống.

### Trải nghiệm trong workshop

Buổi workshop giúp em có cái nhìn thực tế hơn về những yếu tố cần thiết để vận hành một AI Agent trong môi trường doanh nghiệp. Bên cạnh mô hình ngôn ngữ, hệ thống cần có cơ chế lưu trữ tri thức, giám sát, đánh giá chất lượng và bảo mật chặt chẽ.

Nội dung của Ngày 2 là nền tảng quan trọng để em tiếp tục tìm hiểu về cách triển khai Agent an toàn, tối ưu và có khả năng mở rộng trong thực tế.

#### Một số hình ảnh khi tham gia sự kiện
