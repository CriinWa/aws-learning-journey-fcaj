---
title: "Blog 1"
date: 2026-08-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# KIRO POWERS

Trong quá trình vibe coding, anh em FCAJ chắc hẳn từng gặp các trường hợp sau:

- Mỗi dự án mới lại phải tốn công cấu hình lại từ đầu.
- Team khó đồng bộ thiết lập, dẫn đến code thiếu nhất quán.
- Nạp tất cả tool cùng lúc gây context bloat, làm lãng phí token và giảm độ tập trung của AI.

Kiro Powers ra đời để giải quyết điểm nghẽn này. Thay vì cấu hình thủ công mỗi khi mở dự án mới, chúng ta có thể đóng gói công cụ, hướng dẫn và tự động hóa thành một đơn vị duy nhất, rồi chia sẻ cho cả team. Chỉ cần cài một lần, mọi người đều dùng chung một thiết lập.

Một Power là tập hợp các thành phần:

- File `POWER.md` chứa tài liệu và từ khóa kích hoạt.
- Các MCP server cung cấp công cụ thực thi.
- Steering files định nghĩa quy trình và quy chuẩn.
- Hooks để tự động hóa theo sự kiện.

Điểm khác biệt quan trọng là Power **không tải sẵn**. Nó chỉ được kích hoạt khi câu lệnh chứa từ khóa phù hợp. Nhờ vậy, agent chỉ nhận đúng kiến thức và công cụ cần thiết, tránh tình trạng context bloat.

Mỗi người đều có thể tạo Power riêng bằng công cụ Build a Power, hoặc import từ GitHub và thư mục local qua Add Custom Power. Sau khi hoàn thiện, chỉ cần đẩy lên Git repository là cả team có thể cài đặt đồng bộ.

## Ví dụ nhanh: Power Zapier

Chỉ cần cài Power “Zapier” trong Kiro Powers panel, agent đã có thể kết nối và tự động hóa hàng nghìn ứng dụng bên ngoài.

Power này chứa `POWER.md` với các từ khóa như `zapier`, `automation`, `webhook`, `youtube`, `discord`, kèm MCP Zapier và steering hướng dẫn workflow tích hợp dữ liệu. Từ đó, chỉ cần chat “Tự động lấy video mới nhất từ kênh YouTube Y gửi qua kênh ứng dụng X...” hoặc dán link kịch bản Zapier là Power tự động kích hoạt. Agent sẽ lấy context kết nối, map dữ liệu, sinh cấu trúc thông báo và xử lý tự động thay vì code thủ công.

Kiro Powers không chỉ là tiện ích; đây là cách biến chuyên môn thành module có thể tái sử dụng và chia sẻ.

{{< event-image src="images/3-Blog/Blog1.jpg" alt="Blog1" >}}

## Tài liệu tham khảo

1. [Kiro Powers documentation](https://kiro.dev/docs/powers/)
2. [Introducing Kiro Powers](https://kiro.dev/blog/introducing-powers/)
3. [Video giới thiệu Kiro Powers](https://youtu.be/kEOmuVyqfMU?si=p9iFGMNMUK9rbYAp)

## Đường dẫn đăng bài

[Link bài đăng tại AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2232309990867294/)
