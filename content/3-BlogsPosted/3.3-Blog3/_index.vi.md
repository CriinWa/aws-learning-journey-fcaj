---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
# Kiến Thức Nền Tảng – Bệ Phóng Vững Chắc Để Chinh Phục Điện Toán Đám Mây

Xin chào mọi người!

Mình là một người yêu thích lĩnh vực Backend, Cloud Computing và luôn tin rằng việc học công nghệ không chỉ dừng lại ở việc biết sử dụng công cụ, mà quan trọng hơn là hiểu được những nguyên lý cốt lõi phía sau.

Trong thời gian tìm hiểu và thực hành với AWS, mình nhận ra rằng hành trình học Cloud của bản thân diễn ra nhẹ nhàng hơn rất nhiều so với những gì mình từng hình dung. Không phải vì AWS đơn giản, cũng không phải vì mình có khả năng ghi nhớ hàng trăm dịch vụ, mà bởi trước đó mình đã có cơ hội tiếp cận các kiến thức nền tảng như System Design, Networking, API, Cơ sở dữ liệu và các nguyên lý xây dựng hệ thống.

Chính những kiến thức ấy đã trở thành "chiếc bản đồ" giúp mình kết nối các dịch vụ AWS với những bài toán thực tế thay vì học chúng như những khái niệm rời rạc.

Thông qua bài viết này, mình muốn chia sẻ một góc nhìn cá nhân: kiến thức nền tảng chính là bệ phóng vững chắc giúp chúng ta học Cloud nhanh hơn, hiểu sâu hơn và ứng dụng hiệu quả hơn.

Hy vọng rằng bài viết sẽ mang đến cho bạn thêm một góc nhìn mới nếu đang bắt đầu hành trình chinh phục điện toán đám mây.

Trong vài năm trở lại đây, điện toán đám mây (Cloud Computing) đã trở thành một trong những kỹ năng quan trọng đối với lập trình viên, kỹ sư hệ thống và kỹ sư dữ liệu. Tuy nhiên, khi bắt đầu tìm hiểu AWS, mình đã từng cảm thấy choáng ngợp trước hàng trăm dịch vụ với những cái tên hoàn toàn mới: EC2, VPC, IAM, ECS, Lambda, CloudFront, Route 53,...

Mình cũng đặt câu hỏi: "Liệu mình có phải học thuộc tất cả những dịch vụ này không?" Sau cùng, câu trả lời của mình là không.

Điều giúp mình tiếp cận AWS nhanh hơn không phải là khả năng ghi nhớ tên dịch vụ, mà chính là những kiến thức nền tảng tôi đã học trước đó: System Design, API, Networking, Cơ sở dữ liệu, Hệ điều hành và các nguyên lý xây dựng hệ thống. Việc ánh xạ từng kiến thức nền tảng vào các use case của từng dịch vụ giúp mình học tập và áp dụng vào kiến trúc dễ dàng hơn bao giờ hết.

## AWS Không dạy bạn một khái niệm mới

Sau một thời gian học và thực hành, mình nhận ra rằng phần lớn dịch vụ trên AWS đều là cách hiện thực hóa những nguyên lý đã tồn tại từ lâu.

- AWS không tạo ra khái niệm máy chủ. Họ cung cấp EC2.
- AWS không phát minh ra mạng máy tính. Họ xây dựng VPC, Route Table, Internet Gateway, NAT Gateway, Security Group và Network ACL để chúng ta quản lý mạng trên môi trường đám mây.
- AWS không tạo ra cơ sở dữ liệu. Họ cung cấp RDS, Aurora, DynamoDB để vận hành các hệ quản trị cơ sở dữ liệu theo nhiều mô hình khác nhau.
- AWS không phát minh ra HTTP hay API. Họ cung cấp API Gateway, Load Balancer và nhiều dịch vụ khác để giúp API hoạt động ổn định ở quy mô lớn.
- Điều đó có nghĩa là nếu đã hiểu bản chất của một hệ thống truyền thống, việc học AWS không còn là học một công nghệ hoàn toàn mới, mà là ánh xạ những kiến thức nền tảng sang cách AWS hiện thực hóa chúng.

## Khi hiểu nguyên lý, mọi dịch vụ đều có ý nghĩa

Trước khi học AWS, mình đã dành thời gian tìm hiểu về kiến trúc hệ thống, giao thức mạng và cách các API hoạt động. Nhờ vậy, khi nhìn thấy một dịch vụ mới, mình không còn tự hỏi:

> "Dịch vụ này dùng để làm gì?"

Thay vào đó, mình đặt câu hỏi:

> "Trong kiến trúc hệ thống truyền thống, vấn đề nào cần được giải quyết? AWS đang giải quyết vấn đề đó bằng dịch vụ nào?"

Chỉ một thay đổi nhỏ trong cách tư duy đã giúp quá trình học trở nên trực quan hơn rất nhiều.

Ví dụ:

- Hiểu Load Balancer trước thì Application Load Balancer không còn là khái niệm xa lạ.
- Hiểu Subnet và Routing trước thì VPC trở thành một mô hình mạng quen thuộc.
- Hiểu Reverse Proxy thì CloudFront và API Gateway dễ hình dung hơn.
- Hiểu Docker thì ECS hay ECR chỉ là bước mở rộng để quản lý container trên hạ tầng đám mây.
- Hiểu Database Replication thì Multi-AZ và Read Replica không còn là những thuật ngữ khó nhớ.

Mình không học từng dịch vụ một cách độc lập. Mình học bằng cách kết nối chúng với những kiến thức mình đã có.

## Cloud không thay thế kiến thức nền tảng

Cloud không thay thế kiến thức nền tảng; cloud chỉ là môi trường mới để vận dụng chúng.

- Một kỹ sư không hiểu mạng máy tính sẽ rất khó cấu hình VPC một cách chính xác.
- Một người chưa từng thiết kế API sẽ khó lựa chọn kiến trúc phù hợp giữa API Gateway, Load Balancer và Lambda.
- Một người chưa hiểu về tính sẵn sàng (High Availability) sẽ khó lý giải vì sao cần Multi-AZ, Auto Scaling hay Load Balancer.
- Một người chưa hiểu nguyên lý bảo mật sẽ chỉ biết cấp quyền IAM theo hướng "cho chạy được", thay vì theo nguyên tắc đặc quyền tối thiểu.

Cloud giúp việc triển khai hệ thống nhanh hơn, nhưng không thể thay thế tư duy kỹ thuật.

## Học Cloud là học cách giải quyết vấn đề

Điều khiến mình hứng thú nhất khi học AWS không phải là số lượng dịch vụ, mà là cách mỗi dịch vụ được sinh ra để giải quyết một bài toán thực tế.

Mỗi lần tìm hiểu một dịch vụ, mình luôn bắt đầu bằng hai câu hỏi:

- Trước khi có dịch vụ này, người ta giải quyết bài toán như thế nào?
- AWS đã làm gì để đơn giản hóa hoặc tối ưu cách giải quyết đó?

Khi trả lời được hai câu hỏi này, mình không còn học theo kiểu ghi nhớ tên gọi hay thao tác trên giao diện quản trị. Thay vào đó, mình hiểu được lý do dịch vụ tồn tại, biết khi nào nên sử dụng và khi nào không nên sử dụng. Đó là sự khác biệt giữa việc học công cụ và học tư duy.

## Kiến thức nền tảng là khoản đầu tư sinh lời lâu dài

Cloud sẽ thay đổi. Tên dịch vụ có thể thay đổi. Giao diện quản trị có thể thay đổi. Một số công nghệ rồi sẽ được thay thế.

Nhưng những nguyên lý của mạng máy tính, hệ điều hành, cơ sở dữ liệu, API, bảo mật và thiết kế hệ thống vẫn sẽ tồn tại trong nhiều năm tới.

Đó là lý do mình luôn tin rằng đầu tư vào kiến thức nền tảng là khoản đầu tư có giá trị lâu dài nhất đối với một kỹ sư phần mềm. Nó không chỉ giúp học AWS nhanh hơn, mà còn giúp tiếp cận bất kỳ nền tảng điện toán đám mây nào khác với cùng một tư duy.

## Lời kết

Nếu bạn đang bắt đầu hành trình học Cloud và cảm thấy choáng ngợp bởi hàng trăm dịch vụ, lời khuyên của mình là hãy tạm dừng việc cố gắng ghi nhớ tất cả. Thay vào đó, hãy quay về những kiến thức nền tảng.

Hãy hiểu cách một hệ thống hoạt động. Hãy hiểu vì sao cần mạng, API, cơ sở dữ liệu, cân bằng tải, bảo mật và kiến trúc hệ thống.

Khi đã nắm vững những viên gạch đầu tiên ấy, AWS sẽ không còn là một tập hợp những dịch vụ rời rạc. Nó sẽ trở thành một hệ sinh thái mà mỗi dịch vụ đều có vị trí, mục đích và ý nghĩa rõ ràng.

{{< event-image src="images/3-Blog/Blog3.jpg" alt="Blog3" >}}

## Tài liệu tham khảo

1. [What is Cloud Computing? - AWS](https://aws.amazon.com/what-is-cloud-computing/)
2. [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/wellarchitected-framework.html)
3. [Amazon EC2](https://docs.aws.amazon.com/ec2/)
4. [Amazon VPC](https://docs.aws.amazon.com/vpc/)
5. [AWS Identity and Access Management (IAM)](https://docs.aws.amazon.com/iam/)
6. [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/)
7. [Amazon RDS](https://docs.aws.amazon.com/rds/)
8. [Amazon DynamoDB](https://docs.aws.amazon.com/dynamodb/)

## Đường dẫn đăng bài

[Link bài đăng tại AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2233901344041492/)
