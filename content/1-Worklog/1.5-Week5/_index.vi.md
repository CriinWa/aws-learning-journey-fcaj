---
title: "Worklog Tuần 5"
date: 2026-07-26
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu tuần 5:

- Hoàn thiện frontend/backend và các API endpoint cơ bản
- Cài đặt cấu hình Dockerfile, container hóa ứng dụng và quản lý image trên Amazon ECR
- Thực hành triển khai hạ tầng mạng và triển khai ứng dụng container lên ECS với ALB

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                           |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2    | -**Thực hành:** Route 53 trong Hybrid DNS và rà soát DNS cần cho hệ thống                                                  | 20/07/2026       | 20/07/2026         | [Set up Hybrid DNS with Route 53](https://000010.awsstudygroup.com/)                                                                                                         |
| 3    | -**Thực hành:** Triển khai ứng dụng lên Docker và sử dụng các dịch vụ EC2, RDS, ECR                                    | 21/07/2026       | 21/07/2026         | [Deploy Application on Docker](https://000015.awsstudygroup.com/)                                                                                                            |
| 4    | -Xây dựng API endpoint, cấu hình backend và tìm hiểu cấu hình ALB                                                                | 22/07/2026       | 22/07/2026         | [Security groups for your Application Load Balancer Document](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-update-security-groups.html) |
| 5    | -**Thực hành:** Triển khai ứng dụng image với ECS<br />-Xây dựng các API endpoint và sửa lỗi, hoàn thiện giao diện | 23/07/2026       | 23/07/2026         | [Deploy applications on Amazon Elastic Container Service](https://000016.awsstudygroup.com/)                                                                                 |
| 6    | -Kiểm thử Docker và ECR                                                                                                                | 24/07/2026       | 24/07/2026         | [Amazon Elastic Container Registry Document](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)                                                        |

### Kết quả đạt được tuần 5:

* Mạng và tên miền DNS:

  * Hiểu tổng quan về kiến trúc Hybrid DNS và cách tích hợp hệ thống phân giải tên miền On-premises với môi trường AWS thông qua Amazon Route 53.
  * Thực hành cấu hình Route 53 Resolver, bao gồm Inbound Endpoint, Outbound Endpoint và Resolver Rules để chuyển tiếp DNS giữa các môi trường.
* Phát triển dự án:

  * Xây dựng và hoàn thiện các API endpoint cho dịch vụ backend.
  * Xử lý các lỗi phát sinh trong quá trình gửi, nhận và hiển thị dữ liệu giữa Frontend và Backend.
  * Rà soát cấu hình Backend, biến môi trường và kết nối cơ sở dữ liệu nhằm chuẩn bị cho quá trình container hóa và triển khai.
* Container hóa ứng dụng:

  * Xây dựng Dockerfile Frontend và Backend để đóng gói mã nguồn.
  * Thực hiện build và chạy container trên môi trường cục bộ, kiểm tra cổng kết nối, biến môi trường và khả năng giao tiếp các container.
  * Kiểm tra và sửa các lỗi liên quan đến Dockerfile, thư viện phụ thuộc, cấu hình ứng dụng và quá trình khởi động container.
  * Kiểm tra quyền truy cập và xác thực giữa Docker, Amazon ECR và các dịch vụ AWS.'
* Triển khai ứng dụng trên Amazon ECS:

  * Hiểu quy trình chuyển từ việc chạy container Docker thủ công trên Amazon EC2 sang sử dụng dịch vụ quản lý container Amazon ECS.
  * Thực hành khởi tạo ECS Cluster và xây dựng Task Definition cho các container Frontend và Backend.
  * Tìm hiểu và thực hành cấu hình ECS Service, ALB, Listener và Target Group để định tuyến lưu lượng đến ứng dụng.
  * Xử lý các lỗi liên quan đến biến môi trường, cổng container, Security Group, quyền IAM quá trình image từ Amazon ECR.
