---
title: "Worklog Tuần 4"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Hoàn thành các bài thực hành liên quan đến AWS CLI, di dời dữ liệu (DMS, SCT), VM Import/Export và tối ưu chi phí với Lambda.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Thực hành Lab 000011:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Cài đặt AWS CLI <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra tài nguyên qua CLI <br> &nbsp;&nbsp;&nbsp;&nbsp;+ AWS CLI với Amazon S3 <br> &nbsp;&nbsp;&nbsp;&nbsp;+ AWS CLI với Amazon SNS <br> &nbsp;&nbsp;&nbsp;&nbsp;+ AWS CLI với IAM <br> &nbsp;&nbsp;&nbsp;&nbsp;+ AWS CLI với VPC <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo EC2 sử dụng AWS CLI <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Khắc phục lỗi | 11/05/2026 | 11/05/2026 | [000011 - AWS CLI](https://000011.awsstudygroup.com/) |
| 3 | - **Thực hành Lab 000014:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Import máy ảo vào AWS <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Export EC2 Instance từ AWS | 12/05/2026 | 12/05/2026 | [000014 - VM Import/Export](https://000014.awsstudygroup.com/) |
| 4 | - **Thực hành Lab 000043:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Select your DMS source <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Chọn CSDL đích cho DMS <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Serverless replication <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Theo dõi DMS Migrations <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Xử lý sự cố với AWS DMS | 13/05/2026 | 14/05/2026 | [000043 - DMS & SCT](https://000043.awsstudygroup.com/) |
| 6 | - **Thực hành Lab 000022:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Tag cho Instance <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Role cho Lambda <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Lambda Function <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra kết quả| 15/05/2026 | 16/05/2026 | [000022 - EC2 Cost Opt](https://000022.awsstudygroup.com/) |

### Kết quả đạt được tuần 4:

* **Đối với Bài thực hành 000011 (AWS CLI):**
    * Biết cách cài đặt và cấu hình AWS CLI với các profile khác nhau.
    * Biết cách tương tác trực tiếp với các dịch vụ như S3, IAM, VPC, EC2 thông qua giao diện dòng lệnh.
* **Đối với Bài thực hành 000014 (VM Import/Export):**
    * Tìm hiểu quy trình chuyển dịch máy ảo (VM) từ môi trường on-premises vào Amazon EC2.
    * Biết cách sử dụng S3 làm kho lưu trữ trung gian trong quá trình import/export.
* **Đối với Bài thực hành 000043 (DMS & SCT):**
    * Biết cách sử dụng AWS Schema Conversion Tool (SCT) để chuyển đổi lược đồ CSDL.
    * Làm quen với AWS Database Migration Service (DMS) để di chuyển dữ liệu an toàn, giảm thiểu thời gian chết của ứng dụng.
* **Đối với Bài thực hành 000022 (Tối ưu chi phí với Lambda):**
    * Biết cách tạo Lambda Function để tự động hóa việc bật/tắt EC2 dựa trên lịch trình.
    * Tìm hiểu về phương pháp cam kết sử dụng Saving Plans để tối ưu chi phí cho các hệ thống chạy 24/7.