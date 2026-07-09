---
title: "Worklog Tuần 2"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Hoàn thành việc xem các video hướng dẫn trên YouTube.
* Hoàn thành các bài thực hành từ 000004 đến 000008 và bài 000010 để mở rộng kiến thức về các dịch vụ quan trọng của AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Xem video hướng dẫn youtube <br> - **Thực hành Lab 000004:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Khởi tạo Windows instance <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Khởi tạo Linux instance <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Amazon EC2 cơ bản <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai ứng dụng Node.js trên Amazon Linux <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Ứng dụng Node.js trên Amazon EC2 Windows <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Giới hạn sử dụng tài nguyên bằng dịch vụ IAM | 27/04/2026 | 27/04/2026 | [000004 - Amazon EC2](https://000004.awsstudygroup.com/) |
| 3 | - **Thực hành Lab 000005:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo EC2 instance <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo RDS database instance <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai ứng dụng <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Backup và restore | 28/04/2026 | 28/04/2026 | [000005 - Amazon RDS](https://000005.awsstudygroup.com/) |
| 4 | - **Thực hành Lab 000006:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Launch Template <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập Load Balancer <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra kết quả <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Auto Scaling Group <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm thử các giải pháp | 29/04/2026 | 29/04/2026 | [000006 - ASG & ELB](https://000006.awsstudygroup.com/) |
| 5 | - **Thực hành Lab 000007:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Budget <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Cost Budget <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Usage Budget <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo RI Budget <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Saving Plans Budget | 30/04/2026 | 30/04/2026 | [000007 - AWS Budget](https://000007.awsstudygroup.com/) |
| 6 | - **Thực hành Lab 000008:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ CloudWatch Metric <br> &nbsp;&nbsp;&nbsp;&nbsp;+ CloudWatch Logs <br> &nbsp;&nbsp;&nbsp;&nbsp;+ CloudWatch Alarms <br> &nbsp;&nbsp;&nbsp;&nbsp;+ CloudWatch Dashboards | 01/05/2026 | 01/05/2026 | [000008 - CloudWatch](https://000008.awsstudygroup.com/) |
| 7/CN | - **Thực hành Lab 000010:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Kết nối đến RDGW <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai Microsoft AD <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập DNS | 02/05/2026 | 03/05/2026 | [000010 - Route 53](https://000010.awsstudygroup.com/) |

### Kết quả đạt được tuần 2:

* Tiếp tục xem các video hướng dẫn trên YouTube.
* **Đối với Bài thực hành 000004 (Amazon EC2):**
    * Tìm hiểu các tính năng cơ bản của dịch vụ máy chủ ảo cốt lõi AWS.
    * Biết cách lựa chọn Instance Types, AMI, Backup và Key Pair.
* **Đối với Bài thực hành 000005 (Amazon RDS):**
    * Hiểu cách triển khai cơ sở dữ liệu quan hệ cho các yêu cầu lưu trữ dữ liệu có cấu trúc (OLTP).
    * Biết cách thiết lập tính sẵn sàng cao với Multi-AZ và mở rộng hiệu suất đọc bằng Read Replicas.
* **Đối với Bài thực hành 000006 (ASG & ELB):**
    * Thực hành triển khai ứng dụng FCJ Management.
    * Bước đầu làm quen với việc sử dụng Amazon EC2 Auto Scaling Group để mở rộng linh hoạt theo nhu cầu.
    * Làm quen với Elastic Load Balancing để phân phối tải đến Application Tier.
* **Đối với Bài thực hành 000007 (AWS Budget):**
    * Tìm hiểu 4 loại ngân sách (Cost Budget, Usage Budget, RI Budget, Savings Plans Budget) để theo dõi chi phí trên tài khoản AWS.
* **Đối với Bài thực hành 000008 (AWS CloudWatch):**
    * Biết cách thu thập dữ liệu hiệu năng thông qua CloudWatch Metrics và Logs từ ứng dụng chạy trên EC2.
    * Thực hành thiết lập các cảnh báo (Alarms) và tạo bảng điều khiển (Dashboard) đơn giản để giám sát tài nguyên.
* **Đối với Bài thực hành 000010 (Route 53 Resolver):**
    * Hiểu khái niệm về kiến trúc DNS hybrid.
    * Làm quen với cách sử dụng Outbound Endpoints, Inbound Endpoints và Resolver Rules để tích hợp giữa on-premise DNS và dịch vụ Amazon Route 53.