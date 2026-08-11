---
title: "Worklog Tuần 4"
date: 2026-07-19
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu tuần 4:

- Lập kế hoạch Sprint Agile dự án.
- Xây dựng backend cơ bản và tìm hiểu Docker/ CI-CD
- Triển khai các dịch vụ hạ tầng cho dự án với Multi AZ

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                              |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 2    | -Vẽ kiến trúc dự án triển khai<br />-**Thực hành:** Cấu hình IAM policy/role để ứng dụng truy cập AWS                                                                           | 13/07/2026       | 13/07/2026         | [Granting authorization to access AWS services](https://000048.awsstudygroup.com/)<br /><br />[Limitation of user rights with IAM Permission](https://000030.awsstudygroup.com/) |
| 3    | -Lập kế hoạch Agile Sprint<br />-**Thực hành:** Sử dụng Tag và Resource Groups để quản lý tài nguyên                                                                             | 14/07/2026       | 14/07/2026         | [Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/)                                                                                            |
| 4    | -Học và nghiên cứu về Docker và CI/CD<br />-Chuẩn bị CloudFormation cho IAM, Role, Policy<br />-**Thực hành:** Quản lý bảo mật về tài khoản và dự án với AWS Security Hub | 15/07/2026       | 15/07/2026         | [AWS Security Hub](https://000018.awsstudygroup.com/)[Docker Build Document ](https://docs.docker.com/build/ci)[AWS CloudFormation](https://000037.awsstudygroup.com/)            |
| 5    | -Tạo RDS MySQL, thiết kế Database Schema<br />-Học và nghiên cứu về cách triển khai dự án với docker và quy trình CI/CD                                                                | 16/07/2026       | 16/07/2026         | [Docker Build Document](https://docs.docker.com/build/ci)                                                                                                                       |
| 6    | <br />-**Thực hành:** Mã hóa dữ liệu với AWS KMS, kiểm tra mã hóa và truy suất dữ liệu bằng Amazon Athena<br />-Hoàn thiện cấu hình dự án                                 | 17/07/2026       | 17/07/2026         | [Encrypt at rest with AWS KMS](https://000033.awsstudygroup.com/)                                                                                                               |

### Kết quả đạt được tuần 4:

- Lập kế hoạch tiến độ nhóm cần hoàn thành công việc:

  - **Sprint 1 – Foundation & Infrastructure:** Lập kế hoạch và xây dựng nền tảng AWS cho dự án, gồm VPC Multi-AZ, public/private subnet, Internet Gateway, NAT Gateway và route table.
  - **Sprint 2 – CI/CD Pipeline & Deployment:** Lên kế hoạch triển khai pipeline GitHub Actions, ECS Fargate, Application Load Balancer, health check và cấu hình ứng dụng Django/React trên AWS.
  - **Sprint 3 – Scaling, Monitoring & Go-Live:** Lên kế hoạch hoàn thiện ECS Auto Scaling, CloudFront, CloudWatch, cảnh báo chi phí, kiểm thử end-to-end và tài liệu triển khai.
  - **Sprint 4 – Testing & Documentation:** Kiểm thử toàn bộ luồng chính của ứng dụng, khắc phục lỗi và hoàn thiện tài liệu triển khai, kiến trúc và báo cáo dự án.
- Quản lý & Phân loại tài nguyên:

  - Sử dụng Tags để đánh dấu, phân loại tài nguyên ứng dụng AWS Resource Groups để gom nhóm dựa trên Tags, giúp tự động hóa và quản lý đồng loạt số lượng lớn tài nguyên cùng lúc.
  - Cấu hình AWS CloudTrail để ghi nhận toàn bộ lịch sử thao tác trong hệ thống và kết hợp Amazon Athena để truy vấn, phân tích dữ liệu log

  Bảo mật:

  - Triển khai IAM Permission Boundary để thiết lập ranh giới quyền hạn tối đa cho User/Group, giúp quản lý các vấn đề bảo mật.
  - AWS Security Hub nhằm tổng hợp tự động các cảnh báo bảo mật từ nhiều dịch vụ (như GuardDuty, Inspector, Macie) về một bảng điều khiển duy nhất và tự động kiểm tra.
  - Thực hành bảo mật dữ liệu bằng cách cấu hình mã hóa dữ liệu ở trạng thái lưu trữ (Encrypt at rest) trên S3 thông qua dịch vụ AWS KMS.
- Ứng dụng & Container hóa:

  - Sử dụng Docker để tạo các môi trường chạy ứng dụng (container) độc lập, hạn chế các lỗi phát sinh do khác biệt môi trường giữa máy cá nhân và máy chủ.
  - Tích hợp Docker vào quy trình Continuous Integration (CI), giúp quá trình kiểm thử và tích hợp mã nguồn (code) diễn ra nhất quán và tự động.
