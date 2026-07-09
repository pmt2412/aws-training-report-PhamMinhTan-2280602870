---
title: "Worklog Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Thiết lập hệ thống bảo mật thông tin cấu hình và quản lý danh tính người dùng.
* Xây dựng hạ tầng xử lý thông điệp bất đồng bộ và hệ thống giám sát, cảnh báo tự động trên AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --- | --- | --- |
| 2 | - **Bảo mật cấu hình:** Thiết lập quản lý bảo mật thông tin cấu hình (secrets) bằng AWS Systems Manager (SSM) Parameter Store. | 22/06/2026 | 22/06/2026 |
| 3 | - **Xác thực người dùng:** Triển khai và cấu hình hệ thống xác thực và quản lý danh tính người dùng sử dụng AWS Cognito. | 23/06/2026 | 23/06/2026 |
| 4 | - **Giao tiếp Service:** Xây dựng hệ thống xử lý thông điệp bất đồng bộ giữa các service với Amazon SQS (bao gồm thiết lập Dead-Letter Queue). | 24/06/2026 | 24/06/2026 |
| 5 | - **Hệ thống thông báo:** Cấu hình hệ thống thông báo sự kiện và cảnh báo qua email sử dụng Amazon SNS Topic. | 25/06/2026 | 25/06/2026 |
| 6 | - **Giám sát tài nguyên:** Cấu hình hệ thống giám sát tài nguyên với Amazon CloudWatch (Alarms). <br>- **Viết bài blog:** <br>&emsp;+ "VPC Link trong tích hợp riêng tư (Private Integrations) của Amazon API Gateway" | 26/06/2026 | 26/06/2026 |

### Kết quả đạt được tuần 10:

* **Về bảo mật và xác thực:**
    * Đã thiết lập thành công AWS SSM Parameter Store để lưu trữ và quản lý an toàn các thông tin cấu hình nhạy cảm.
    * Triển khai hoàn thiện AWS Cognito, thiết lập thành công luồng xác thực và quản lý danh tính cho người dùng của hệ thống.
* **Về xử lý thông điệp bất đồng bộ:**
    * Xây dựng thành công hàng đợi Amazon SQS giúp các microservices giao tiếp bất đồng bộ, đi kèm với Dead-Letter Queue để xử lý các message bị lỗi.
* **Về giám sát và cảnh báo:**
    * Cấu hình thành công Amazon SNS Topic để gửi các thông báo sự kiện và cảnh báo quan trọng qua email.
    * Tích hợp thành công Amazon CloudWatch Alarms để tự động giám sát sức khỏe của các tài nguyên hệ thống và kích hoạt thông báo khi cần.
* **Về hoạt động viết blog:**
    * Hoàn thành viết và đăng tải bài blog: *"VPC Link trong tích hợp riêng tư (Private Integrations) của Amazon API Gateway"*.