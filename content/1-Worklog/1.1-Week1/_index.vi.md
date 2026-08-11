---
title: "Worklog Tuần 1"
date: 2026-06-28
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
### Mục tiêu tuần 1:

* Làm quen với môi trường làm việc và các công cụ được sử dụng trong chương trình FCAJ.
* Tìm hiểu các nguyên tắc cơ bản về bảo mật tài khoản AWS, quản lý chi phí và môi trường phát triển.
* Nắm được các khái niệm nền tảng về mạng AWS và thực hành các bài lab về mạng.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                             |
| ---- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 2    | -Làm quen chương trình FCAJ; tạo AWS Free Tier, bật MFA, tìm hiểu và cấu hình AWS Budgets.                        | 22/06/2026       | 22/06/2026         | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br /><br />[AWS Free Tier](https://000001.awsstudygroup.com/)                |
| 3    | -Tìm hiểu về cấu trúc mạng VPC, Subnet, Route Table, Internet Gateway và Security Group.                              | 23/06/2026       | 23/06/2026         | [Cost Management with AWS Budget](https://000007.awsstudygroup.com/)                                                                           |
| 4    | -**Thực hành:** Tạo cấu hình mạng với VPC, Site-to-Site VPN, kiểm tra các routing.                          | 24/06/2026       | 24/06/2026         | [Amazon VPC and AWS Site-to-Site VPN](https://000003.awsstudygroup.com/)                                                                       |
| 5    | -**Thực hành:** Tạo cấu hình mạng VPC Peering, cấu hình Security Group, NACL.                                  | 25/06/2026       | 25/06/2026         | [Setting up VPC Peering](https://000019.awsstudygroup.com/)                                                                                    |
| 6    | -**Thực hành:** Tìm hiểu kết nối mạng lai với Transit Gateway, Hybrid DNS/Route 53. Thực hành lệnh AWS CLI. | 25/06/2026       | 26/06/2026         | [Set up Hybrid DNS with Route 53](https://000010.awsstudygroup.com/)<br /><br />[Set up AWS Transit Gateway](https://000020.awsstudygroup.com/) |

### Kết quả đạt được tuần 1:

* Đã hiểu cách khởi tạo và bảo vệ tài khoản AWS bằng MFA và IAM User. Đã tạo và cấu hình thành công tài khoản AWS Free Tier. Biết cách thiết lập AWS Budgets để kiểm soát chi phí phát sinh.
* Làm quen với giao diện AWS Console: Nắm bắt giao diện AWS Management Console, biết cách tìm kiếm, truy cập và sử dụng các dịch vụ trực tiếp trên nền tảng web.
* Nắm được các khái niệm cốt lõi về mạng AWS như VPC, Subnet, Security Group, NACL. Hiểu vai trò của Transit Gateway và Hybrid DNS trong kiến trúc mạng lai.
* Sử dụng AWS CLI trên Kiro để thực hiện các thao tác cơ bản như:

  * Kiểm tra thông tin tài khoản & cấu hình
  * Tạo và quản lý key pair
  * Kiểm tra thông tin dịch vụ đang chạy
