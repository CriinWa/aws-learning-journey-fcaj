---
title: "Worklog Tuần 7"
date: 2026-08-09
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu tuần 7:

* Hoàn thiện CI/CD với GitHub Actions
* Tự động hóa build/deploy image
* Kiểm thử hoạt động của hệ thống sau mỗi lần cập nhật
* Triển khai hệ thống với CloudFront CDN

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                 |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| 2    | -Rà soát Dockerfile, Docker Compose, ECR, thống nhất cấu hình ứng dụng<br />- Triển khai CLoudFront CDN cho ứng dụng                           | 03/08/2026       | 03/08/2026         | [Amazon CloudFront Document](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)                 |
| 3    | -Tạo workflow GitHub Actions checkout, build, kiểm tra mã nguồn.<br />-Cấu hình xác thực AWS, cập nhật ECS task definition, push image lên ECR | 04/08/2026       | 04/08/2026         | [Configuration OIDC with AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws) |
| 4    | -Kiểm thử luồng commit -> build -> deploy<br />-Theo dõi ECS rollout và CloudWatch Logs, sửa các lỗi phát sinh                                   | 05/08/2026       | 05/08/2026         | -                                                                                                                                 |
| 5    | -Sửa các lỗi trong ứng dụng khi chạy các luồng dữ liệu                                                                                          | 06/08/2026       | 06/08/2026         | -                                                                                                                                 |
| 6    | -Cập nhật tài liệu và chỉnh sửa hoàn thiện kiến trúc nhóm                                                                                     | 07/08/2026       | 07/08/2026         | -                                                                                                                                 |

### Kết quả đạt được tuần 7:

* Cấu hình triển khai:

  * Rà soát và đồng bộ Dockerfile, Docker Compose, biến môi trường và cấu hình Amazon ECR.
  * Điều chỉnh ECS Task Definition để phù hợp với quy trình triển khai tự động.
* Xây dựng quy trình CI/CD:

  * Tạo workflow GitHub Actions để tự động kiểm tra mã nguồn, build và đẩy container image lên Amazon ECR.
  * Tự động cập nhật ECS Task Definition và triển khai phiên bản mới lên ECS Service sau mỗi lần cập nhật mã nguồn.
* Kiểm thử và giám sát hệ thống:

  * Kiểm thử toàn bộ luồng commit, build, push image và deploy lên ECS.
  * Theo dõi quá trình ECS Rollout, CloudWatch Logs và Health Check của Application Load Balancer.
  * Phát hiện và khắc phục các lỗi liên quan đến cấu hình, quyền IAM, container và luồng dữ liệu của ứng dụng.
* Hoàn thiện tài liệu:

  * Cập nhật tài liệu triển khai, kiểm thử hệ thống.
  * Hoàn thiện sơ đồ kiến trúc, bổ sung GitHub Actions, Amazon ECR, Amazon ECS, CloudFront.
