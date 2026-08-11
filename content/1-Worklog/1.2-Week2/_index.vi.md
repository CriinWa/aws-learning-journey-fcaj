---
title: "Worklog Tuần 2"
date: 2026-07-05
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### Mục tiêu tuần 2:

* Nắm kiến thức nền tảng về EC2 và các thành phần đi kèm.
* Hiểu cách giám sát, mở rộng và sao lưu tài nguyên AWS.
* Làm quen với các dịch vụ cơ bản phục vụ vận hành hệ thống.
* Thao tác làm việc song song giữa giao diện Console và CLI để quản lý tài nguyên AWS hiệu quả.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                       | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                        |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2    | -Tìm hiểu Amazon EC2: instance type, AMI, EBS, snapshot, key pair và SSH Linux.                                                                                                                                                | 29/06/2026       | 29/06/2026         | [Introduction to Amazon EC2](https://000004.awsstudygroup.com/)                                                                                           |
| 3    | -**Thực hành:** CloudWatch, Giám sát tài nguyên và trạng thái hệ thống.<br />-**Thực hành:** Sử dụng quản lý Tag và Resource Group, theo dõi metric/log.                                        | 30/06/2026       | 30/06/2026         | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com)<br /><br />[Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com) |
| 4    | -Tìm hiểu cơ chế tự động mở rộng theo tải hệ thống.<br />-**Thực hành:** Triển khai Auto Scaling Group cho kiến trúc.<br />-Họp nhóm để thảo luận đề tài sẽ triển khai cho dự án thực tập. | 01/07/2026       | 01/07/2026         | [Deploying FCJ Management Application with Auto Scaling Group](https://000006.awsstudygroup.com/)                                                         |
| 5    | -Tìm hiểu AWS Lightsail.<br />-**Thực hành:** Thao tác EC2/S3/SNS/IAM/VPC qua AWS CLI.                                                                                                                                | 02/07/2026       | 02/07/2026         | [AMAZON LIGHTSAIL WORKSHOP](https://000045.awsstudygroup.com/)<br /><br />[Getting Started with the AWS CLI](https://000011.awsstudygroup.com/)            |
| 6    | -Tìm hiểu về AWS Backup to System, kiểm tra snapshot, restore và thông báo SNS.<br />-**Thực hành:** Quy trình sao lưu tài nguyên.                                                                            | 03/07/2026       | 03/07/2026         | [Deploy AWS Backup to the System](https://000013.awsstudygroup.com/)                                                                                      |

### Kết quả đạt được tuần 2:

* Hiểu tổng quan về AWS và các nhóm dịch vụ cốt lõi:

  * Compute (EC2, Auto Scaling, Lightsail)
  * Networking (VPC, Application Load Balancer, Internet Gateway)
  * Storage (EBS Snapshots)
  * Management & Monitoring (CloudWatch, AWS Backup, Resource Groups)
* Thiết lập và thao tác tự động hóa với AWS CLI

  * Sử dụng AWS CLI để thực hiện các thao tác quản lý cơ bản như: kiểm tra cấu hình tài khoản, tương tác với các dịch vụ S3, SNS, IAM, thiết lập hạ tầng mạng VPC và tạo mới máy chủ EC2.
  * Cài đặt và cấu hình thành công AWS CLI trên máy tính, bao gồm thiết lập: Access Key, Secret Key, Default Region và Output Format.
* Hiểu cách tổ chức tài nguyên và giám sát hệ thống

  * Thiết lập Launch Template và Auto Scaling Group kết hợp với Application Load Balancer (Target Group, ALB).
  * Sử dụng Amazon CloudWatch để theo dõi hiệu suất hệ thống: xem Metrics, quản lý Logs, thiết lập Alarms cảnh báo và tạo Dashboards trực quan.
  * Triển khai AWS Backup để tự động hóa việc sao lưu dữ liệu (như EBS, RDS), kiểm tra khả năng phục hồi (restore) và cấu hình AWS SNS để nhận thông báo.
