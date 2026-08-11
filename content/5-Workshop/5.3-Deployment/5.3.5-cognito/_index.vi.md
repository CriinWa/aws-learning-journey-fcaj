---
title: "Cognito — Auth và RBAC"
date: 2026-08-11
weight: 7
chapter: false
pre: " <b> 5.3.7. </b> "
---
Hệ thống sử dụng Amazon Cognito để quản lý tài khoản người dùng và phân quyền truy cập các chức năng của hệ thống trong môi trường production.

## Tổng quan Sơ đồ Kiến trúc

![1786475234181](image/_index.vi/1786475234181.png)

## Phân quyền người dùng

| Nhóm            | Quyền                                                                     |
| ---------------- | -------------------------------------------------------------------------- |
| **users**  | Sử dụng chức năng chatbot và các chức năng dành cho người dùng |
| **admins** | Quản lý user, dashboard admin, toàn quyền editor                       |

Các chức năng quản lý người dùng Cognito được thực hiện thông qua module
`src/services/cognito_admin.py`
Module này hỗ trợ các thao tác quản trị như:

- Liệt kê danh sách người dùng.
- Kích hoạt người dùng.
- Vô hiệu hóa người dùng.
- Gán quyền quản trị cho người dùng.

Module xác thực `src/api/auth.py` đảm nhiệm:

- Xác thực người dùng: Kiểm tra thông tin đăng nhập qua Cognito User Pool.
- Lấy thông tin user: Trích xuất username, email, và các groups từ Cognito.
- Enforce permissions: Kiểm tra quyền truy cập require_roles decorator.

### Cấu hình Amazon Cognito

Đăng nhập vào AWS Management Console
Trên thanh tìm kiếm, nhập Cognito và chọn dịch vụ Cognito.

![1786471067587](image/_index.vi/1786471067587.png)

Chọn `Get started for free in less than five minutes` để bắt đầu

![1786471125057](image/_index.vi/1786471125057.png)

Tại mục Define your application

* **Type d'application (Loại ứng dụng):**
  * Chọn Single-page application (SPA) nếu làm ứng dụng web bằng  **React, Vue, Angular, Next.js (Client-side)** .
  * Chọn Traditional Web Application nếu dùng **Node.js/Express, Python/Django, Java, PHP** render từ phía server.
* **Name your application:** Đổi thành tên ứng dụng.

![1786471315889](image/_index.vi/1786471315889.png)

Tại mục Configuration of options

* **Options for login credentials:**
  * Tích chọn **E-mail** (cho phép người dùng đăng nhập bằng Email).
* **Auto-inscription:**
  * Giữ tích chọn **Activate self-registration** để cho phép người dùng tự đăng ký tài khoản.
* **Attributes required for registration:**
  * Nhấn vào dropdown **Select attributes** và chọn các thông tin bắt buộc khi đăng ký (ví dụ: `email`, `name`).

![1786471477131](image/_index.vi/1786471477131.png)

Tại mục Add a return URL

* Nếu định dùng màn hình đăng nhập sẵn của AWS (Hosted UI), nhập URL sau khi đăng nhập thành công.
* Nếu tự dựng giao diện đăng nhập riêng (dùng SDK/Amplify), có thể để trống mục này.

Sau khi điền xong, nhấn nút **Create a user directory** ở góc dưới bên phải để khởi tạo User Pool.

![1786471579317](image/_index.vi/1786471579317.png)

Hệ thống sử dụng Amazon Cognito User Pool để quản lý tài khoản. Các thông tin cấu hình cần thiết được khai báo trong file môi trường `.env`, ví dụ:
`COGNITO_USER_POOL_ID=...`
`OGNITO_APP_CLIENT_ID=...`
`AUTH_DISABLED=false`

Trong môi trường production, cơ chế xác thực được bật để đảm bảo các chức năng quản trị không bị truy cập trái phép.

### Các chức năng quản trị

Tài khoản admins có quyền thực hiện các chức năng quản trị như:

| Chức năng                       | Mục đích                                  |
| --------------------------------- | -------------------------------------------- |
| **Quản lý người dùng** | Xem và quản lý tài khoản                |
| **Enable/Disable user**     | Kích hoạt hoặc vô hiệu hóa tài khoản |
| **Phân quyền**            | Quản lý quyền quản trị                  |
| **Quản lý tài liệu**    | Upload và quản lý dữ liệu pháp luật   |
| **Dashboard Admin**         | Theo dõi và quản lý hệ thống           |
