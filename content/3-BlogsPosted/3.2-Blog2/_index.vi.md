---
title: "Blog 2"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# AMAZON RDS AUTOMATION ON SCHEDULE

Xin chào mọi người, mình đang tìm hiểu thêm về cách tối ưu chi phí trên AWS và có đọc bài hướng dẫn Automatically stop and start an Amazon RDS DB instance using AWS Systems Manager Maintenance Windows trong AWS Prescriptive Guidance.

Bài viết chia sẻ cách **tự động bật và tắt Amazon RDS** theo lịch, chẳng hạn chỉ khởi động database trong giờ làm việc và tắt vào buổi tối hoặc cuối tuần. Cách này phù hợp với môi trường development, testing hoặc staging không cần hoạt động 24/7.

Ban đầu mình nghĩ muốn làm việc này sẽ phải dùng EventBridge kết hợp với Lambda và tự viết code gọi API của RDS. Tuy nhiên, AWS Systems Manager đã có sẵn hai Automation runbook là `AWS-StartRdsInstance` và `AWS-StopRdsInstance`, nên gần như không cần viết thêm logic riêng.

Các điểm chính cần nắm:

- Tạo hai Maintenance Windows cho lịch bật và lịch tắt.
- Sử dụng cron expression để xác định thời gian chạy.
- Gắn tag cho các RDS instance cần áp dụng.
- Gom các instance vào Resource Group.
- Dùng Automation runbook để tự động start hoặc stop database.

Điểm mình thấy hay là khi có nhiều RDS instance, mình chỉ cần gắn cùng một tag cho chúng thay vì cấu hình thủ công từng database. Systems Manager sẽ thực hiện task trên toàn bộ tài nguyên trong nhóm.

Một vài điều mình rút ra được:

- Không phải tài nguyên nào cũng cần chạy 24/7, đặc biệt là môi trường development và testing.
- Trước khi viết Lambda để tự động hóa, nên kiểm tra xem AWS đã có sẵn runbook phù hợp không.
- IAM role chỉ nên được cấp quyền start và stop đúng những RDS instance cần thiết.
- RDS chỉ có thể bị dừng liên tục tối đa 7 ngày; sau đó AWS sẽ tự khởi động lại để thực hiện bảo trì.

Project của mình hiện vẫn để database chạy liên tục và chỉ tắt khi không sử dụng, nên bài viết này giúp mình biết thêm một cách đơn giản để tối ưu chi phí mà không cần xây dựng quá nhiều thành phần.

{{< event-image src="images/3-Blog/Blog2.png" alt="Blog2" >}}

## Tài liệu tham khảo

[docs.aws.amazon.com/prescriptive-guidance/latest/patterns/automatically-stop-and-start-an-amazon-rds-db-instance-using-aws-systems-manager-maintenance-windows.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/automatically-stop-and-start-an-amazon-rds-db-instance-using-aws-systems-manager-maintenance-windows.html)


## Đường dẫn đăng bài

[Link bài đăng tại AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2230171591081134/)
