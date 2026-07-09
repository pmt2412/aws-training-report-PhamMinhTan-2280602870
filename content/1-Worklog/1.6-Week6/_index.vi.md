---
title: "Worklog Tuần 6"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Bàn luận lên kế hoạch, phân chia công việc chi tiết giữa các thành viên trong nhóm.
* Bắt tay vào xây dựng hệ thống backend (kiến trúc Microservices) của project.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --- | --- | --- |
| 2 | - Họp nhóm: Bàn luận lên kế hoạch, phân chia công việc. <br> - Khởi tạo tài liệu kế hoạch triển khai. | 25/05/2026 | 25/05/2026 |
| 3 | - **Auth-service:** Thiết lập kiến trúc, logic chính. <br> - **Bảo mật:** Cài đặt mã hóa mật khẩu và cơ chế sinh/xác minh JWT Token. | 26/05/2026 | 26/05/2026 |
| 4 | - **API Xác thực:** Viết các endpoint Đăng ký, Đăng nhập, quản lý người dùng, đổi mật khẩu. <br> - **Schemas:** Thiết lập Pydantic model validate I/O. | 27/05/2026 | 27/05/2026 |
| 5 | - **Database (CRUD):** Viết hàm tạo người dùng, liên kết công ty (tenant) và phân quyền. | 28/05/2026 | 28/05/2026 |
| 6 | - **Hỗ trợ Frontend:** Cập nhật giao diện Frontend. <br> - **Hỗ trợ Backend:** Review và fix các file core/schema bên `hr-service` và `tenant-service`. | 29/05/2026 | 29/05/2026 |
| 7/CN | - Phối hợp nhóm: Tích hợp hệ thống, kiểm tra chéo các service (Auth, HR, Tenant) và kiến trúc tổng thể. | 30/05/2026 | 31/05/2026 |

### Kết quả đạt được tuần 6:

* Hoàn thành việc bàn luận lên kế hoạch và phân chia công việc cụ thể giữa các thành viên trong nhóm.
* **Chi tiết công việc cá nhân hoàn thành:**
    * **Xây dựng auth-service:** Thiết lập thành công kiến trúc và logic cốt lõi cho service xác thực.
    * **Xử lý bảo mật (Security):** Cài đặt mã hóa mật khẩu và cơ chế sinh/xác minh JWT Token.
    * **Xây dựng API xác thực:** Hoàn thiện các endpoint quan trọng như Đăng ký (Register), Đăng nhập (Login), quản lý thông tin người dùng, đổi mật khẩu và mời người dùng mới vào hệ thống.
    * **Tương tác Cơ sở dữ liệu (CRUD):** Viết các hàm tương tác database để tạo người dùng mới, liên kết người dùng với từng công ty (tenant) riêng biệt và phân quyền người dùng.
    * **Định nghĩa dữ liệu (Schemas):** Thiết lập các Pydantic model để validate dữ liệu đầu vào/đầu ra cho các API.
    * **Hỗ trợ Frontend:** Cập nhật giao diện Frontend kết nối với API.
    * **Hỗ trợ Backend:** Review và sửa lỗi đồng bộ các file core/schema bên `hr-service` và `tenant-service` để đảm bảo hệ thống tích hợp trơn tru.
### Hình ảnh minh họa

![API 1](/images/1-Worklog/api1.jpg)
![API 2](/images/1-Worklog/api2.jpg)
![API 3](/images/1-Worklog/api3.jpg)    