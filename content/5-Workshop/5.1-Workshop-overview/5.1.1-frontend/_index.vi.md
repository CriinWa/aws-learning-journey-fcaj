---
title: "Frontend"
date: 2026-08-11
weight: 1
chapter: false
pre: " <b> 5.1.1 </b> "
---
# Frontend

Ứng dụng sử dụng **Streamlit** làm framework giao diện người dùng. Streamlit cho phép xây dựng nhanh các ứng dụng web tương tác bằng Python, phù hợp với các ứng dụng AI/ML và chatbot.

## Cấu trúc thư mục
```text
Frontend / UI
├── .streamlit/
│   └── config.toml          # Cấu hình theme & server của Streamlit
│
├── assets/
│   └── style.css            # CSS tùy chỉnh cho giao diện
│
└── views/
    ├── login.py             # Trang đăng nhập
    ├── register.py          # Trang đăng ký
    ├── chatbot.py           # Giao diện chat chính (User)
    └── admin.py             # Admin Dashboard
```

## Giải thích chi tiết từng file

### `.streamlit/config.toml`

File cấu hình giao diện Streamlit:

- `theme.primaryColor = "#0F4C81"`: Màu chính là xanh navy.
- `theme.backgroundColor = "#F8FAFC"`: Nền chính sáng, dịu mắt.
- `theme.secondaryBackgroundColor = "#FFFFFF"`: Nền phụ trắng.
- `theme.textColor = "#1E293B"`: Màu chữ xanh xám đậm.
- `theme.font = "sans serif"`: Font không chân.
- `server.headless = true`: Chạy không cửa sổ GUI riêng.
- `server.port = 8501`: Chạy ở cổng 8501.

Đây là file thuần cấu hình, quyết định màu sắc và cách app chạy, không chứa logic UI.

### `assets/style.css`

File CSS tùy chỉnh giao diện chi tiết:

- **Biến màu:** định nghĩa bộ màu dùng chung (primary, hover, nền, text, viền).
- **Layout & Typography:** tăng cỡ chữ, line-height cho văn bản pháp luật dễ đọc.
- **Form & Input:** form login/register dạng card trắng, input có viền rõ, hiệu ứng hover/focus, placeholder rõ nét.
- **Button:** nút to hơn, font đậm, bo góc 12px, nút primary nổi bật.
- **Sidebar:** nền trắng, có đường viền ngăn cách, nút logout màu đỏ nổi bật.
- **Chat bubbles:** tin nhắn user bên phải (xanh), assistant bên trái (trắng/xám), có bóng nhẹ, bo góc.
- **Gợi ý câu hỏi (suggestion chips):** hiển thị dạng chip nằm ngang, có scroll nếu quá nhiều.

### `views/login.py`

**Trang đăng nhập**:

- Hiển thị form đăng nhập với hai trường: tên đăng nhập và mật khẩu.
- Khi người dùng gửi form, hệ thống kiểm tra thông tin đăng nhập với cơ sở dữ liệu.
- Nếu thông tin chính xác và tài khoản đang hoạt động, người dùng được đăng nhập và chuyển vào ứng dụng.
- Nếu thông tin sai hoặc tài khoản đã bị vô hiệu hóa, hệ thống hiển thị thông báo lỗi tương ứng.
- Có liên kết chuyển sang trang đăng ký cho người chưa có tài khoản.

![1786494545703](image/_index.vi/1786494545703.png)

### `views/register.py`

**Trang đăng ký**:

- Hiển thị form đăng ký với các trường: tên đăng nhập, email, mật khẩu và xác nhận mật khẩu.
- Hệ thống kiểm tra tính hợp lệ của dữ liệu nhập vào:
  - Tên đăng nhập phải có độ dài tối thiểu, không chứa khoảng trắng và không trùng với tài khoản đã có.
  - Email phải đúng định dạng.
  - Mật khẩu phải đáp ứng yêu cầu về độ dài và độ phức tạp (có chữ hoa và số).
  - Mật khẩu xác nhận phải khớp với mật khẩu đã nhập.
- Nếu dữ liệu hợp lệ, tài khoản mới được tạo và người dùng được chuyển về trang đăng nhập với thông báo thành công.
- Có liên kết quay lại trang đăng nhập cho người đã có tài khoản.

![1786494483070](image/_index.vi/1786494483070.png)

### `views/chatbot.py`

**Giao diện chat chính cho người dùng**:

- **Quản lý phiên làm việc**: Người dùng có thể tạo phiên chat mới, chọn phiên cũ để tiếp tục, hoặc xóa phiên không cần thiết.
- **Gửi câu hỏi**: Người dùng nhập câu hỏi, hệ thống chuyển đến backend xử lý và nhận về câu trả lời kèm theo nguồn tham khảo.
- **Hiển thị kết quả**: Câu trả lời được hiển thị dạng hội thoại, có phân biệt tin nhắn của người dùng và trợ lý. Nguồn tham khảo được hiển thị dưới dạng mở rộng để người dùng xem chi tiết.
- **Đánh giá**: Người dùng có thể like/dislike câu trả lời để gửi phản hồi, giúp cải thiện chất lượng hệ thống.
- **Gợi ý câu hỏi**: Hiển thị các câu hỏi mẫu để người dùng tham khảo và sử dụng nhanh.
- **Kết nối backend**: Mọi câu hỏi đều được gửi đến backend RAG qua API và nhận câu trả lời thực tế, không sử dụng dữ liệu mẫu.

![1786494928599](image/_index.vi/1786494928599.png)

### `views/admin.py`

**Giao diện quản trị hệ thống cho Admin**:

- **Dashboard**: Hiển thị các chỉ số tổng quan như số lượng người dùng, câu hỏi, phản hồi, tỷ lệ hài lòng kèm biểu đồ xu hướng sử dụng.

![1786495178642](image/_index.vi/1786495178642.png)

- **Quản lý người dùng**: Cho phép tìm kiếm, lọc, thêm mới, vô hiệu hóa hoặc khôi phục tài khoản người dùng. Hỗ trợ xuất danh sách ra file CSV.

![1786495224552](image/_index.vi/1786495224552.png)

- **Lịch sử & Logs**: Xem lịch sử các câu hỏi và phản hồi của người dùng, hỗ trợ lọc và xuất dữ liệu.

![1786495255170](image/_index.vi/1786495255170.png)

- **Cài đặt hệ thống**: Cho phép điều chỉnh các tham số vận hành của chatbot (số lượng đoạn văn bản lấy ra, độ sáng tạo, số token trả về, lựa chọn mô hình AI).

![1786495298545](image/_index.vi/1786495298545.png)
