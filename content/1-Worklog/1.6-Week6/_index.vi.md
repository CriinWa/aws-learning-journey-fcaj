---
title: "Worklog Tuần 6"
date: 2026-08-02
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu tuần 6:

* Triển khai staging trên ECS Fargate
* Cấu hình giám sát và kiểm thử hệ thống khi chạy.
* Triển khai hạ tầng mạng và triển khai ứng dụng container lên ECS với ALB

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                           |
| ---- | ------------------------------------------------------------------------------- | ---------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| 2    | -Triển khai ECS Services Backend và Frontend, Auto Scaling và CloudWatch   | 27/07/2026       | 27/07/2026         | [Amazon Elastic Container Service Document](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-tutorials.html)  |
| 3    | -Cấu hình AWS Budget, SNS. Kiểm thử các chức năng, giao diện dự án    | 28/07/2026       | 28/07/2026         | [Monolithic app with Docker, ECS and AWS Fargate](https://000067.awsstudygroup.com/)                                         |
| 4    | -Thiết lập CloudWatch Logs/metrics, theo dõi lỗi và kiểm thử tích hợp. | 29/07/2026       | 31/07/2026         | [CloudWatch Workshop](https://000008.awsstudygroup.com)                                                                      |
| 5    | -Kiểm tra log, health check, kết nối ECS và ALB, sửa lỗi phát sinh.      | 30/07/2026       | 31/07/2026         |                                                                                                                             |
| 6    | - Kiểm thử chức năng, tích hợp và thống kê các lỗi cần xử lý.     | 30/07/2026       | 31/07/2026         |                                                                                                                             |

### Kết quả đạt được tuần 6:

* Triển khai ứng dụng trên Amazon ECS Fargate:
  * Triển khai thành công ECS Service cho Frontend và Backend bằng AWS Fargate. Cấu hình Task Definition, tài nguyên, biến môi trường và cổng kết nối phù hợp cho các container.
  * Tích hợp ECS Service với ALB và Target Group phân phối lưu lượng đến các container của ứng dụng.
  * Kiểm tra kết nối giữa Frontend, Backend và các dịch vụ trong hệ thống.
* Cấu hình khả năng mở rộng và kiểm tra trạng thái hệ thống:
  * Tìm hiểu và cấu hình Auto Scaling cho ECS Service dựa trên các chỉ số sử dụng CPU và bộ nhớ.
  * Thiết lập Health Check trên Application Load Balancer để theo dõi trạng thái hoạt động của các ECS Task.
  * Kiểm tra quá trình khởi tạo, thay thế và khôi phục container khi Task gặp lỗi hoặc không vượt qua Health Check.
* Giám sát và quản lý nhật ký bằng Amazon CloudWatch:
  * Kiểm tra log phát hiện các lỗi liên quan đến cấu hình biến môi trường, kết nối mạng, cổng dịch vụ.
  * Thực hiện sửa lỗi và điều chỉnh cấu hình dựa trên thông tin thu thập được từ ECS, ALB và CloudWatch.
* Quản lý chi phí và cảnh báo:
  * Cấu hình AWS Budget để theo dõi chi phí sử dụng tài nguyên trong quá trình triển khai và vận hành hệ thống.
  * Thiết lập Amazon SNS để nhận thông báo khi chi phí đạt đến các ngưỡng đã cấu hình.
* Kiểm thử hệ thống:
  * Thực hiện kiểm thử giao diện, chức năng và khả năng tích hợp giữa Frontend và Backend.
  * Kiểm tra luồng xử lý dữ liệu, phản hồi của API, khả năng truy cập ứng dụng thông qua ALB và trạng thái hoạt động của các container.
  * Thống kê các lỗi phát sinh trong quá trình triển khai và kiểm thử cấu hình, kết nối, Health Check và quyền truy cập tài nguyên AWS.
