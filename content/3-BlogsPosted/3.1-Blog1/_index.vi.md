---
title: "Blog 1"
date: 2026-08-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# TÁI ĐỊNH HÌNH CÁCH LƯU TRỮ BINARY ASSETS TRONG GAME VỚI LORE

## Nỗi đau của ngành Game Development

Trong quá trình phát triển game, các nhóm phát triển phải commit hàng trăm tài sản nhị phân (binary assets) có dung lượng lớn mỗi ngày. Các hệ thống quản lý phiên bản (Version Control Systems) truyền thống như Git không được thiết kế để xử lý việc này. Khi một file binary bị sửa đổi, hệ thống cũ sẽ lưu toàn bộ file đó thành một phiên bản mới, bất kể số lượng byte thay đổi là bao nhiêu. Điều này tạo ra lượng dữ liệu lưu trữ thừa thãi khổng lồ, một studio 50 người có thể tích lũy tới hàng Petabytes trong một chu kỳ sản xuất, dẫn đến chi phí hàng tháng tăng vọt.

Để giải quyết bài toán này, Epic Games đã tạo ra Lore - một hệ thống quản lý phiên bản mã nguồn mở với một cách tiếp cận hoàn toàn khác biệt.

## 2. Lore làm điều đó khác biệt như thế nào?

Thay vì coi các file binary là những khối dữ liệu mờ nhạt (opaque blobs), Lore chia nhỏ mỗi file nhị phân thành các phân mảnh (fragments) có kích thước thay đổi, được định danh bằng mã băm mật mã (cryptographic hash).

- Nếu bạn sửa một file texture 200MB, Lore sẽ chỉ lưu trữ những phân mảnh chứa các byte bị thay đổi, chứ không nhân bản toàn bộ file.
- Tính kinh tế thay đổi từ tuyến tính sang "dưới tuyến tính" (sub-linear). Càng về sau của dự án, tỷ lệ trùng lặp phân mảnh càng cao, giúp tiết kiệm dung lượng.
- Nếu cùng một phân mảnh xuất hiện trong 100 texture khác nhau, nó chỉ được lưu đúng 1 lần (deduplication).
- Việc tạo nhánh (branching) với hàng chục ngàn assets không thay đổi sẽ không tốn thêm dung lượng lưu trữ nào, khiến cho việc rẽ nhánh gần như hoàn toàn miễn phí.

Tuyệt vời hơn, người dùng cuối (Artists, Devs) không cần thay đổi thói quen. Mọi người vẫn dùng quy trình quen thuộc: Check Out, Edit, Commit; client của Lore sẽ tự động xử lý việc phân mảnh ngầm bên dưới.

## 3. Kiến trúc của Lore trên AWS

Để triển khai Lore mạnh mẽ nhất, AWS và Epic đã xây dựng một kiến trúc tham chiếu hoàn chỉnh. Dữ liệu sẽ chảy qua các thành phần sau:

- Edge pods (Amazon EC2): Chạy trên các instance C8gd. Client kết nối qua giao thức QUIC (một giao thức UDP giúp tối ưu đường truyền và chống mất gói tin). Pod có sẵn hàng terabytes NVMe cục bộ để làm cache.
- Write tier (Amazon ECS): Đảm nhận tính bền vững của dữ liệu. Khi có dữ liệu đẩy lên (push), Edge pods gửi phân mảnh mới tới Write tier để loại bỏ trùng lặp (deduplicate) và lưu vào S3.
- Durable storage (Amazon S3): Lưu trữ mọi phân mảnh độc nhất (unique). Các phân mảnh này là bất biến (immutable), chỉ ghi 1 lần và đọc nhiều lần.
- Metadata và Locks (Amazon DynamoDB): Xử lý metadata của file, con trỏ nhánh và khóa (locks). DynamoDB mang lại tốc độ đọc tính bằng mili-giây, cực kỳ quan trọng khi có 50 người cùng tranh giành quyền khóa độc quyền (exclusive locks) một file.
- Service discovery (AWS Cloud Map): Giúp Edge pods tìm thấy Write tier qua DNS nội bộ. Khi hạ tầng cập nhật, DNS update trong vài giây, không làm gián đoạn team.

## 4. Ứng dụng thực tế và góc nhìn cá nhân

Sau khi đọc bài viết này, mình nhận thấy Lore không chỉ đơn thuần là công cụ lưu trữ, mà nó thay đổi tư duy làm game:

- Việc "rẽ nhánh miễn phí" giúp các team tự do thử nghiệm các tính năng mới mà không sợ tốn tiền lưu trữ.
- Các studio có nhiều dự án có thể chia sẻ các phân mảnh với nhau, từ đó xây dựng các thư viện asset dùng chung ở quy mô lớn.

Là một người đang tìm hiểu về AWS, kiến trúc của Lore là một case-study hoàn hảo về việc kết hợp đúng dịch vụ cho đúng mục đích: dùng S3 để lưu trữ rẻ/bền, DynamoDB để đọc/ghi metadata siêu tốc, và EC2 NVMe để caching.

## 5. Kết luận & triển khai

Hiện tại, mã nguồn của Lore đã có trên GitHub và AWS cũng đã public một module Terraform mã nguồn mở (terraform-aws-lore) để tự động hóa toàn bộ việc cấu hình từ mạng, tính toán, lưu trữ đến xác thực.

Trong tương lai, kiến trúc này hứa hẹn sẽ còn mở rộng với khả năng triển khai multi-region và tích hợp sâu hơn vào hệ sinh thái Unreal Engine. Nếu anh em nào đang làm DevOps cho Game Studio thì chắc chắn không nên bỏ qua giải pháp này!

**Nguồn tham khảo:** https://aws.amazon.com/vi/blogs/gametech/how-lore-rethinks-binary-asset-storage-on-aws/
