---
title: "Worklog Tuần 7"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Xây dựng và hoàn thiện các chức năng trọng yếu trên giao diện Frontend của project.
* Tìm hiểu chuyên sâu về bảo mật và quản lý định danh trên AWS thông qua việc hoàn thành các bài thực hành: 000012, 000030, 000044, 000018, 000026, 000033.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --- | --- | --- |
| 2 | - **Thực hành Lab 000012:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Truy cập AWS CLI <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Sử dụng Time-based access control <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Customer Managed Policies <br> &nbsp;&nbsp;&nbsp;&nbsp;+ IAM Identity Center Identity Store APIs <br> - **Thực hành Lab 000030:** Giới hạn quyền của user với IAM Permission Boundary. | 01/06/2026 | 01/06/2026 |
| 3 | - **Thực hành Lab 000044:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo IAM Group <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo IAM User <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình Role Condition <br> - **Frontend:** Code logic đăng nhập Multi-Tenant (nhập Subdomain) và cơ chế Refresh Token tự động. | 02/06/2026 | 02/06/2026 |
| 4 | - **Thực hành Lab 000018:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Các tiêu chuẩn bảo mật <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Kích hoạt Security Hub <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Điểm từng bộ tiêu chuẩn <br> - **Frontend:** Tích hợp đa ngôn ngữ (Anh/Việt) trực tiếp trên giao diện. | 03/06/2026 | 03/06/2026 |
| 5 | - **Thực hành Lab 000026:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Sử dụng AWS WAF <br> - **Frontend:** Viết logic kết nối API để hiển thị chức năng Quản lý nhân sự, Chấm công, Nghỉ phép và Quản trị thành viên. | 04/06/2026 | 04/06/2026 |
| 6 | - **Thực hành Lab 000033:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Key Management Service <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Amazon S3 <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo AWS CloudTrail và Amazon Athena <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm thử và chia sẻ dữ liệu mã hóa trên S3 | 05/06/2026 | 05/06/2026 |
| 7/CN | - **Frontend:** Làm giao diện theo dõi sức khỏe (Health Check) của các microservices | 06/06/2026 | 07/06/2026 |

### Kết quả đạt được tuần 7:

* **Đối với tiến độ xây dựng Frontend:**
    * Hoàn thiện thành công logic đăng nhập Multi-Tenant cho phép người dùng nhập Subdomain, kết hợp với cơ chế Refresh Token tự động để duy trì phiên đăng nhập an toàn.
    * Đã tích hợp tính năng đa ngôn ngữ (Anh/Việt) trực tiếp trên giao diện, nâng cao trải nghiệm người dùng.
    * Xây dựng xong logic kết nối API, hiển thị mượt mà các module quan trọng: Quản lý nhân sự, Chấm công, Nghỉ phép và Quản trị thành viên.
    * Thiết kế và hoàn thiện giao diện Health Check để theo dõi tình trạng hoạt động (sức khỏe) của các microservices trong hệ thống.
* **Đối với các bài thực hành AWS (Bảo mật & Định danh):**
    * **Lab 000012 (IAM Identity Center):** Tìm hiểu cách quản lý định danh nhân viên tập trung trên nhiều tài khoản AWS theo nguyên tắc least privilege.
    * **Lab 000030 (IAM Permission Boundary):** Biết cách thiết lập Permission Boundary để giới hạn quyền tối đa của user/group, ngăn chặn rủi ro leo thang đặc quyền (privilege escalation).
    * **Lab 000044 (IAM Role & Condition):** Thực hành tạo Role và tăng cường bảo mật bằng cách cấu hình các điều kiện giới hạn truy cập theo địa chỉ IP và thời gian.
    * **Lab 000018 (AWS Security Hub):** Làm quen với trang tổng quan giám sát bảo mật, nhận cảnh báo ưu tiên cao và kiểm tra trạng thái tuân thủ bảo mật tự động.
    * **Lab 000026 (AWS WAF):** Nắm được cách sử dụng tường lửa ứng dụng web để bảo vệ hệ thống khỏi các lỗ hổng phổ biến như SQL Injection hay Cross Site Scripting (XSS).
    * **Lab 000033 (AWS KMS):** Thực hành mã hóa dữ liệu S3 Object ở trạng thái lưu trữ bằng Key Management Service (KMS), ghi log với CloudTrail và truy xuất bằng Athena.

### Hình ảnh minh họa

![Giao diện đăng nhập Multi-Tenant](/images/1-Worklog/login.png)
![Giao diện đa ngôn ngữ - Tiếng Việt](/images/1-Worklog/VIE.png) ![Giao diện đa ngôn ngữ - Tiếng Anh](/images/1-Worklog/eng.png)
![Giao diện Quản lý nhân viên](/images/1-Worklog/staff.png)
![Giao diện Health Check hệ thống](/images/1-Worklog/healthcheck.png)