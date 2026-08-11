---
title: "Proposal"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# NeonFoodMap – Phần mềm thuyết minh tự động Phố ẩm thực Vĩnh Khánh

## 1. Tổng quan dự án

Dự án xây dựng nền tảng thuyết minh tự động dành cho du khách tại **Phố ẩm thực Vĩnh Khánh, Quận 4, TP. Hồ Chí Minh**. Hệ thống hỗ trợ khám phá các điểm ẩm thực và văn hóa qua nội dung thuyết minh đa phương tiện, được kích hoạt theo vị trí địa lý hoặc mã QR.

| Tiêu chí              | Giá trị                                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------------------------ |
| Loại dự án           | Nền tảng thuyết minh tự động và khám phá du lịch số                                               |
| Khu vực triển khai    | Phố ẩm thực Vĩnh Khánh, Quận 4, TP. Hồ Chí Minh                                                      |
| Đối tượng sử dụng | Du khách, đối tác kinh doanh địa phương và quản trị viên                                         |
| Công nghệ ứng dụng  | Frontend trên S3/CloudFront<br />Backend container trên Amazon ECS Fargate, Amazon RDS MySQL và Amazon S3 |
| Hạ tầng AWS           | VPC triển khai trên hai Availability Zone, ECS Auto Scaling và RDS Multi-AZ                               |
| Vận hành              | CI/CD với Docker, GitHub Actions, Amazon ECR, CloudWatch và Amazon SNS                                     |

### Bối cảnh chung về dự án

Trình bày giải pháp triển khai hệ thống NeonFoodMap trên nền tảng Amazon Web Services (AWS) theo kiến trúc Cloud-Native, đáp ứng yêu cầu về khả năng mở rộng, tính sẵn sàng cao, bảo mật và tự động hóa phát hành phần mềm. Mục tiêu là xây dựng hạ tầng có thể tái sử dụng, hỗ trợ triển khai lặp lại và chuẩn hóa quy trình vận hành DevOps cho môi trường Production.

NeonFoodMap là website bản đồ ẩm thực, cho phép người dùng tìm kiếm, khám phá và đánh giá địa điểm ăn uống theo thời gian thực. Hệ thống tích hợp tìm kiếm điểm địa lý (POI), định vị GPS, hiển thị lộ trình, đánh giá địa điểm và Text-to-Speech để phát nội dung mô tả, từ đó nâng cao trải nghiệm khám phá ẩm thực. Đặc điểm xử lý dữ liệu gần thời gian thực và phục vụ nhiều người dùng đồng thời đòi hỏi hạ tầng linh hoạt, có tính sẵn sàng cao và dễ bảo trì.

Giải pháp đề xuất sử dụng Docker và Amazon ECS Fargate; GitHub, GitHub Actions và OpenID Connect (OIDC) để tự động hóa quy trình Build–Test–Deploy; Amazon ECR để lưu trữ Docker image; Amazon RDS trong Private Subnet để bảo vệ dữ liệu; Amazon S3 cho tài nguyên tĩnh và Amazon CloudWatch để giám sát. Kiến trúc này thiết lập một quy trình triển khai thống nhất, an toàn và có thể mở rộng cho các giai đoạn phát triển tiếp theo.

## 2. Mục tiêu

### 2.1 Mục tiêu dự án

| # | Mục tiêu                                                                                        | Chỉ số đánh giá                                                                   |
| - | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| 1 | Cung cấp ứng dụng thuyết minh tự động theo vị trí hoặc QR code.                         | Người dùng có thể truy cập POI và phát nội dung đa phương tiện.           |
| 2 | Quản lý tập trung POI, nội dung âm thanh, hình ảnh, thực đơn và thông tin đối tác. | Dữ liệu được quản lý qua API và giao diện quản trị.                         |
| 3 | Triển khai hệ thống AWS có khả năng mở rộng, bảo mật và giám sát.                    | ECS Auto Scaling, RDS Multi-AZ, private subnet, CloudWatch và SNS được cấu hình. |
| 4 | Tự động hóa quy trình phát hành.                                                           | Image được build, đẩy lên ECR và triển khai ECS qua GitHub Actions.            |

### 2.2 Giá trị mang lại

- **Nâng cao trải nghiệm du khách:** Cung cấp nội dung thuyết minh linh hoạt, đa phương tiện và dễ tiếp cận.
- **Hỗ trợ hoạt động địa phương:** Tạo kênh số để đối tác giới thiệu thực đơn, ưu đãi và thông tin dịch vụ.
- **Vận hành tin cậy:** Tách biệt các lớp ứng dụng, dữ liệu và mạng; hỗ trợ giám sát và cảnh báo tập trung.
- **Sẵn sàng mở rộng:** Kiến trúc container và hạ tầng đa Availability Zone có thể đáp ứng lượng truy cập tăng lên.

## 3. Vấn đề giải quyết

**Vấn đề 1 — Chi phí nhân lực cao:** Việc thuyết minh, hướng dẫn và cập nhật nội dung theo cách thủ công cần nhiều nhân sự, khó duy trì liên tục và khó đáp ứng nhu cầu đa ngôn ngữ của du khách. Các hộ kinh doanh nhỏ cũng khó đầu tư riêng cho nhân sự giới thiệu, truyền thông và hỗ trợ khách hàng.

**Vấn đề 2 — Chi phí và độ phức tạp của hạ tầng:** Một hệ thống phục vụ nhiều người dùng cần có khả năng mở rộng, lưu trữ media, sao lưu dữ liệu, giám sát và bảo mật. Nếu triển khai không phù hợp, chi phí vận hành hạ tầng có thể tăng cao hoặc hệ thống không đáp ứng tốt trong thời điểm có lượng truy cập lớn.

**Vấn đề 3 — Quản lý nội dung phân tán:** Thông tin về POI, bài thuyết minh, hình ảnh, âm thanh, thực đơn và ưu đãi thường được quản lý rời rạc. Điều này gây khó khăn cho việc cập nhật đồng bộ, kiểm soát chất lượng nội dung và duy trì trải nghiệm nhất quán cho du khách.

### 3.1 Phạm vi chức năng

- Hiển thị bản đồ và danh sách POI tại Phố ẩm thực Vĩnh Khánh.
- Kích hoạt bài thuyết minh bằng geofencing hoặc quét QR code; hỗ trợ phát âm thanh và hiển thị hình ảnh liên quan.
- Hỗ trợ nội dung đa ngôn ngữ, lưu lịch sử trải nghiệm và đồng bộ dữ liệu khi có kết nối.
- Cung cấp giao diện cho đối tác cập nhật thực đơn, ưu đãi và theo dõi thông tin cơ bản về lượt tiếp cận.
- Cung cấp giao diện quản trị để quản lý POI, nội dung, người dùng và theo dõi tình trạng hệ thống.

## 4. Kiến trúc triển khai trên AWS

Hệ thống được triển khai trên **Amazon Web Services (AWS)** theo kiến trúc Multi-tier Architecture và định hướng theo **AWS Well-Architected Framework**. Toàn bộ hạ tầng chạy trong **Amazon VPC (10.0.0.0/16)** tại Region **ap-southeast-1 (Singapore)**, trải rộng trên hai Availability Zone để tăng khả năng sẵn sàng và chịu lỗi.

Hệ thống phân phối Frontend qua **Amazon S3** và  **CloudFront** , điều hướng API request qua **Application Load Balancer** tới backend **Amazon ECS Fargate** và lưu trữ trên  **Amazon RDS MySQL** . **GitHub Actions, ECR, CloudWatch** và **SNS** thực hiện CI/CD, giám sát và cảnh báo tự động

### 4.1 Sơ đồ kiến trúc

#### Kiến trúc tổng thể

{{< event-image src="images/2-Proposal/platform_architecture.jpg" alt="Kiến trúc tổng thể nền tảng trên AWS" >}}

Kiến trúc triển khai được xây dựng trên hai Availability Zone để cải thiện tính sẵn sàng:

- **Phân phối frontend:** Nội dung tĩnh được lưu trên **Amazon S3 Static Website** và phân phối qua **Amazon CloudFront** để tăng tốc độ truy cập cho người dùng.
- **Xử lý API:** CloudFront chuyển các yêu cầu API đến ALB. ALB định tuyến lưu lượng đến các ECS Fargate task trong private subnet và Auto Scaling Group phân bổ trên hai Availability Zone.
- **Cơ sở dữ liệu:** **Amazon RDS MySQL** triển khai Multi-AZ, gồm primary database và standby database đồng bộ nhằm nâng cao khả năng chịu lỗi.
- **CI/CD:** Đẩy mã nguồn lên GitHub. **GitHub Actions** dùng OIDC để xác thực với **AWS STS**, build container image và push lên **Amazon ECR**; ECS sau đó pull image và triển khai phiên bản mới.
- **Bảo mật và hạ tầng:** **AWS IAM** quản lý quyền truy cập, **AWS Secrets Manager** lưu trữ thông tin nhạy cảm, và **AWS CloudFormation** chuẩn hóa việc cung cấp và thay đổi hạ tầng.
- **Quan sát hệ thống:** **Amazon CloudWatch** thu thập log và metric; log có thể được lưu trữ lâu dài trên S3. **Amazon SNS** gửi cảnh báo đến email.

#### Kiến trúc kết nối dịch vụ

{{< event-image src="images/2-Proposal/edge_architecture.jpg" alt="Kiến trúc biên và kết nối dịch vụ trên AWS" >}}

- Người dùng truy cập ứng dụng qua **Internet Gateway** và **Application Load Balancer (ALB)**.
- Frontend và backend được đóng gói thành container, vận hành bằng **Amazon ECS Fargate** trong ECS Cluster.
- **AWS Cloud Map** được sử dụng để **quản lý Service Discovery** giữa các Container trong ECS Cluster, giúp các dịch vụ giao tiếp nội bộ mà không cần phải cập nhật lại IP khi cập nhật Task Revision mới.

### 4.2 Các thành phần kiến trúc

| Tầng                  | Dịch vụ                                    | Vai trò                                                                                      |
| ---------------------- | -------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Edge                   | Amazon CloudFront                            | Phân phối nội dung từ S3 và chuyển các yêu cầu API đến Application Load Balancer. |
| Frontend               | Amazon S3 Static Website                     | Lưu trữ và cung cấp tài nguyên của giao diện ứng dụng.                              |
| Compute                | Application Load Balancer                    | Nhận yêu cầu API, thực hiện health check và phân phối truy cập đến ECS service.    |
| Compute                | Amazon ECS Fargate                           | Chạy các container backend; mở rộng theo nhu cầu với Auto Scaling.                     |
| Service discovery      | AWS Cloud Map và Amazon Route 53            | Hỗ trợ dịch vụ nội bộ giao giữa các thành phần trong ECS Cluster.                   |
| CI/CD                  | GitHub Actions, AWS STS và Amazon ECR       | Xác thực OIDC, build container image, lưu image và triển khai phiên bản mới cho ECS.  |
| Data                   | Amazon RDS MySQL                             | Lưu trữ dữ liệu nghiệp vụ.                                                              |
| Mạng                  | Amazon VPC, Internet Gateway và NAT Gateway | Tách public/private subnet; cung cấp kết nối Internet.                                    |
| Security               | AWS IAM và AWS Secrets Manager              | Phân quyền IAM role và bảo vệ thông tin.                                                |
| Infrastructure as Code | AWS CloudFormation                           | Chuẩn hóa việc khởi tạo và thay đổi hạ tầng.                                        |
| Observability          | Amazon CloudWatch, Amazon SNS và S3 Logs    | Thu thập log, metric, tạo alert và lưu trữ log.                                         |

### 4.3 AWS Well-Architected Framework

| Trụ cột              | Giải pháp áp dụng                                                           |
| ---------------------- | ------------------------------------------------------------------------------- |
| Operational Excellence | GitHub Actions CI/CD, CloudFormation, CloudWatch.                               |
| Security               | IAM Least Privilege, Secrets Manager, KMS, Private Subnets                      |
| Reliability            | Application Load Balancer, ECS Auto Scaling, RDS Multi-AZ, VPC Endpoint for S3. |
| Performance Efficiency | CloudFront, ECS Fargate AutoScaling, RDS Optimization.                         |
| Cost Optimization      | ECS Fargate Auto Scaling, S3 Lifecycle.                                         |
| Sustainability         | Scale theo nhu cầu, tắt môi trường dev ngoài giờ                        |

## 5. Timeline

| Giai đoạn                               | Thời gian theo Worklog | Nội dung triển khai                                                                                                                                                                                                        |
| ----------------------------------------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Nền tảng và thiết kế              | Tuần 1–3              | Thiết lập tài khoản AWS, IAM, Budgets và kiến thức mạng; nghiên cứu EC2, S3, RDS, CloudWatch, Auto Scaling, Backup; hoàn thiện kiến trúc dự án và lựa chọn dịch vụ AWS.                                 |
| 2. Phát triển và chuẩn bị hạ tầng  | Tuần 4                 | Lập kế hoạch Agile; xây dựng backend cơ bản, thiết kế cơ sở dữ liệu RDS MySQL, chuẩn bị Dockerfile, CloudFormation, IAM role/policy và cấu hình bảo mật cần thiết.                                     |
| 3. Container hóa và triển khai staging | Tuần 5–6              | Hoàn thiện frontend, backend và API; build image, đẩy image lên ECR; triển khai ECS Fargate, ALB, RDS, Auto Scaling; cấu hình CloudWatch, AWS Budgets, SNS và kiểm thử trên môi trường staging.              |
| 4. Tự động hóa triển khai            | Tuần 7                 | Rà soát Dockerfile/Docker Compose; thiết lập GitHub Actions với OIDC, tự động build, push image lên ECR, cập nhật ECS task definition và theo dõi ECS rollout.                                                  |
| 5. Phân phối nội dung và hoàn thiện | Tuần 7–8              | Triển khai CloudFront cho frontend tĩnh, kiểm tra DNS và khả năng truy cập; rà soát chi phí, bảo mật, hệ thống cảnh báo; kiểm thử tổng thể, hoàn thiện tài liệu, sơ đồ kiến trúc và báo cáo. |

## 6. Ước tính ngân sách

### 6.1 Chi phí sử dụng và chi phí tối đa dự kiến

| Dịch vụ                          | Cấu hình theo kiến trúc hiện tại                                                                                                 |                                                   Chi phí/tháng | Chi phí ước tính tối đa/tháng |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------: | -----------------------------------: |
| Amazon ECS (Fargate)               | Chạy backend container trên ECS; Production dùng 2 tasks trên 2 AZ với Auto Scaling, ví dụ 0,5 vCPU và 1 GB RAM cho mỗi task. |                                                  $9,86 | ~$20–35 |                                      |
| Amazon RDS MySQL                   | Production dùng Multi-AZ: Primary ở AZ A và Standby ở AZ B.                                                                        |                                                     $11,78 | ~$50 |                                      |
| NAT Gateway, ALB và Amazon VPC    | Hai NAT Gateway và ALB cho Production; dashboard hiện gộp một phần chi phí vào**EC2 – Other** và **VPC**.         |                                                 $32,80 | ~$82–84 |                                      |
| Amazon CloudFront                  | Phân phối static web từ S3 và định tuyến API; giả định khoảng 100 GB truyền dữ liệu.                                     | $0.00 (Free Tier cho 1 TB) |           $0.00 (Free Tier cho 1 TB) |                                      |
| Amazon S3                          | Lưu static web, media và logs; giả định khoảng 50 GB.                                                                            |                          ~$2 |                                ~$2 |                                      |
| Amazon CloudWatch và SNS          | Flow Logs, container logs, metrics, alarms và gửi email cảnh báo.                                                                  |                                                    $5,61 | ~$5–6 |                                      |
| AWS Secrets Manager và Amazon ECR | Lưu secrets và container images.                                                                                                     |                          ~$2 |                                ~$3 |                                      |
| **Tổng chi phí/tháng**    |                                                                                                                                        |                           **$64,05** | **~$166–184** |                                      |

### 6.2 Chiến lược tối ưu chi phí

- Cấu hình **AWS Budgets** và cảnh báo qua SNS ở các ngưỡng 50%, 80% và 100% ngân sách tháng.
- Theo dõi chi phí NAT Gateway, ECS Fargate, RDS và CloudWatch là các nhóm chi phí chính.
- Chỉ duy trì số lượng ECS task cần thiết; sử dụng Auto Scaling để tránh cấp phát tài nguyên nhàn rỗi.
- Xóa hoặc dừng các tài nguyên không còn sử dụng trong môi trường staging sau khi hoàn tất kiểm thử.
- Sử dụng CloudFront cache cho static web và media để giảm lưu lượng đến origin; cân nhắc S3 Lifecycle khi dung lượng log hoặc media tăng lên.

## 7. Đánh giá rủi ro

### 7.1 Ma trận rủi ro

| Rủi ro                                             | Khả năng  | Ảnh hưởng |
| --------------------------------------------------- | ----------- | ------------ |
| Chi phí AWS vượt dự báo                        | Trung bình | Trung bình  |
| ECS task hoặc container gặp lỗi                  | Trung bình | Trung bình  |
| Sự cố cơ sở dữ liệu                           | Thấp       | Cao          |
| Lộ thông tin nhạy cảm                           | Thấp       | Rất cao     |
| Lưu lượng tăng đột biến                      | Trung bình | Trung bình  |
| Log hoặc cảnh báo không đầy đủ              | Trung bình | Trung bình  |
| Lỗi trong quá trình triển khai phiên bản mới | Trung bình | Trung bình  |

### 7.2 Kế hoạch dự phòng và ứng phó

- Xử lý cảnh báo chi phí ngay khi chạm ngưỡng ngân sách; xác định dịch vụ phát sinh và dừng hoặc điều chỉnh tài nguyên không cần thiết.
- Khi API hoặc container lỗi, kiểm tra CloudWatch Logs, trạng thái ALB health check và ECS task definition trước khi rollback hoặc triển khai bản sửa.
- Khi có sự cố dữ liệu, ưu tiên bảo vệ dữ liệu, đánh giá ảnh hưởng và thực hiện khôi phục theo quy trình backup/restore đã kiểm thử.
- Khi phát hiện dấu hiệu lộ thông tin xác thực, thu hồi hoặc xoay vòng secret, kiểm tra IAM permissions và rà soát lịch sử triển khai.

## 8. Kết quả kỳ vọng

* **Cải tiến kỹ thuật:** Số hóa việc thuyết minh và quản lý POI, thay thế quy trình cung cấp thông tin thủ công bằng nền tảng đa phương tiện có thể giám sát, mở rộng và triển khai tự động trên AWS.
* **Giá trị dài hạn:** Hình thành nền tảng nội dung và dữ liệu có thể tái sử dụng cho các khu vực du lịch khác; đồng thời tạo cơ sở để mở rộng phân tích hành vi người dùng, nội dung đa ngôn ngữ và hợp tác với các hộ kinh doanh địa phương trong tương lai.

### Tài liệu tham khảo

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)
- [AWS Documentation](https://docs.aws.amazon.com/)
