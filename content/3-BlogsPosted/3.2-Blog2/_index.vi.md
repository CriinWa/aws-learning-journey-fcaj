---
title: "Blog 2"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# NGĂN CHẶN RÒ RỈ DỮ LIỆU (DATA EXFILTRATION) TRÊN AWS VỚI EGRESS CONTROLS

## 1. Mở đầu: Lỗ hổng chết người thường bị bỏ quên

Khi thiết lập bảo mật trên môi trường Cloud, các team thường dồn toàn bộ sự chú ý vào luồng truy cập đi vào (Ingress) như WAF, Security Group hay các chính sách phân quyền. Trong khi đó, luồng truy cập đi ra ngoài (Egress) lại thường bị bỏ ngỏ theo mặc định để tránh làm lỗi các ứng dụng phụ thuộc.

Điều này cực kỳ nguy hiểm. AWS đã lấy ví dụ về lỗ hổng React2Shell (CVE-2025-55182): ngay khi lỗ hổng này được công bố, hacker đã lập tức nhắm vào các máy chủ chưa được vá lỗi để chiếm quyền thực thi mã từ xa. Khi đã xâm nhập thành công, chúng thiết lập kết nối Command-and-Control và âm thầm tuồn dữ liệu ra ngoài. Nếu không có Egress Controls, luồng dữ liệu này sẽ đi qua trót lọt mà tổ chức không hề hay biết.

## 2. Kỷ nguyên AI Agentic: Rủi ro rò rỉ dữ liệu tăng cấp số nhân

Bài blog của AWS đặc biệt nhấn mạnh rằng Egress Control không chỉ dành cho các ứng dụng truyền thống mà còn sống còn đối với AI Agent.

Theo chuẩn OWASP cho ứng dụng AI, có 2 rủi ro lớn:

- Agent Goal Hijack (ASI01): Kẻ tấn công dùng kỹ thuật thao túng (prompt injection) để thay đổi mục tiêu của AI Agent, sai khiến nó âm thầm gửi dữ liệu ra ngoài.
- Unexpected Code Execution (ASI05): AI Agent bị lừa để sinh ra và chạy các đoạn mã độc lập kết nối ngược (reverse shell) hoặc truyền dữ liệu nhạy cảm ra các endpoint bên ngoài.

Vì các AI Agent thường xuyên phải gọi API bên ngoài để hoạt động, chúng trở thành mục tiêu giá trị cao và luồng mạng đi ra của chúng cần được kiểm soát gắt gao như bất kỳ hệ thống nào khác.

## 3. Phân tích kiến trúc bảo mật Hub-and-Spoke trên AWS

Để giải quyết vấn đề này, AWS đề xuất một kiến trúc bảo mật nhiều lớp. Dữ liệu và các lớp bảo mật được phân bổ như sau:

- VPC Layer: Các ứng dụng (workload) nằm trong Spoke VPC. Chúng ưu tiên kết nối với các dịch vụ AWS qua VPC Endpoints để giữ dữ liệu không chạy ra ngoài Internet. Mọi luồng traffic muốn ra Internet sẽ được Transit Gateway gom lại và đẩy qua Network Firewall để kiểm duyệt.
- IAM Layer: Sử dụng Data perimeter SCPs và RCPs để thiết lập hàng rào bảo vệ ở cấp độ API.
- Detection Layer: Các dịch vụ như GuardDuty, Security Hub và IAM Access Analyzer liên tục rà quét và phát hiện bất thường.
- Integration Layer: Khi phát hiện sự cố, EventBridge sẽ kích hoạt Lambda để xử lý tự động và gửi thông báo qua SNS.
- Observability Layer: Toàn bộ log được tập trung về CloudWatch để giám sát.

## 4. Nhóm giải pháp phòng thủ chủ động

Đây là các rào chắn giúp chặn đứng dữ liệu ngay trước khi nó kịp thoát ra ngoài:

- AWS Network Firewall: Tường lửa này cung cấp khả năng kiểm tra sâu gói tin từ Layer 3 đến Layer 7. Nó có thể chặn các tên miền trái phép, lọc IP/Port, sử dụng Suricata rules để phát hiện mẫu tấn công (IDS/IPS), và đặc biệt là có khả năng giải mã TLS để bắt quả tang các luồng dữ liệu rò rỉ bị mã hóa ẩn bên trong giao thức HTTPS.
- Amazon Route 53 Resolver DNS Firewall: Hacker rất hay dùng kỹ thuật DNS Tunneling (nhét dữ liệu vào các truy vấn DNS) vì DNS thường ít bị kiểm duyệt. DNS Firewall giúp chặn các truy vấn đến tên miền độc hại, nhận diện mã độc tạo tên miền tự động (DGA) bằng AI/Machine Learning ngay từ khâu phân giải tên miền.
- Data Perimeters (hàng rào dữ liệu): Sử dụng Service Control Policies (SCPs) ở cấp độ tổ chức để cấm tạo các resource lách luật, kết hợp với Resource Control Policies (RCPs) và VPC Endpoint Policies để đảm bảo chỉ những định danh (identity) thuộc tổ chức của bạn mới được phép gọi API rút dữ liệu.

## 5. Nhóm giải pháp phát hiện

Nếu các lớp phòng thủ chủ động bị vượt qua, tổ chức cần các "camera an ninh" để phát hiện:

- Amazon GuardDuty: Phát hiện các hành vi bất thường như nỗ lực rò rỉ dữ liệu qua DNS (Trojan:EC2/DNSDataExfiltration) hoặc các lệnh gọi API từ các IP nằm trong danh sách đen (Exfiltration:S3/MaliciousIPCaller).
- IAM Access Analyzer: Sử dụng công nghệ suy luận tự động để liên tục rà soát xem có tài nguyên nào (ví dụ: S3 bucket) đang bị mở public hoặc chia sẻ nhầm cho các account bên ngoài hay không.
- AWS Security Hub: Gom tất cả các cảnh báo từ GuardDuty, IAM Access Analyzer, Inspector, Macie về một nơi, giúp đánh giá toàn diện nguy cơ rò rỉ.

## 6. Lộ trình triển khai thực tế cho doanh nghiệp

AWS khuyên anh em không nên làm tất cả cùng lúc mà chia thành 3 giai đoạn:

- Giai đoạn 1 - Quick wins: Bật ngay Route 53 DNS Firewall và Amazon GuardDuty để có baseline giám sát và vá lỗ hổng DNS.
- Giai đoạn 2 - Foundational: Triển khai Data Perimeters (SCPs, RCPs) và đưa AWS Network Firewall vào hệ thống Transit Gateway.
- Giai đoạn 3 - Efficient: Bật IAM Access Analyzer, tích hợp EventBridge + Lambda để tự động hóa việc chặn IP/rule khi có sự cố, và theo dõi tập trung qua Security Hub.

## 7. Bài học rút ra

Qua bài viết này, mình nhận thấy tư duy bảo mật Cloud đã thay đổi. Chúng ta không chỉ khóa cửa trước (Ingress) mà phải giám sát cực kỳ chặt chẽ cửa sau (Egress). Đặc biệt, khi tích hợp AI (LLM, AI Agents) vào hệ thống, việc kiểm soát chúng gọi API ra ngoài là bắt buộc để tránh trở thành nạn nhân của Prompt Injection tuồn dữ liệu công ty.

Nguồn tham khảo: https://aws.amazon.com/vi/blogs/security/prevent-data-exfiltration-aws-egress-controls-for-cloud-workloads/
