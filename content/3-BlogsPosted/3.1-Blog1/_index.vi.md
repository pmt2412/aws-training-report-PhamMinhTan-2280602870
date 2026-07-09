---
title: "Blog 1"
date: 2026-06-19
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# CUNG CẤP THÔNG TIN XÁC THỰC CƠ SỞ DỮ LIỆU BẢO MẬT CHO CÁC HÀM LAMBDA BẰNG AWS SECRETS MANAGER

Sử dụng AWS Secrets Manager giúp bảo mật credentials của cơ sở dữ liệu và truyền chúng cho các hàm AWS Lambda khi kết nối với Amazon RDS (MySQL). Giải pháp này giúp loại bỏ việc hardcode mật khẩu trong mã nguồn hoặc truyền qua các biến môi trường, từ đó bảo vệ cơ sở dữ liệu backend một cách an toàn hơn. Ngoài ra, tính năng tự động rotate mật khẩu định kỳ giúp giảm thiểu rủi ro bảo mật.

Mô hình hoạt động của giải pháp bao gồm:

* Client gửi Request đến RESTful API được lưu trữ trên AWS API Gateway.
* API Gateway thực thi hàm AWS Lambda tương ứng.
* Hàm Lambda gọi API của AWS Secrets Manager để lấy thông tin đăng nhập cơ sở dữ liệu (username/password).
* Hàm Lambda sử dụng thông tin đó để kết nối, truy vấn cơ sở dữ liệu Amazon RDS (MySQL) và trả lại kết quả.

Quy trình triển khai thông qua CloudFormation

Giải pháp sử dụng một template AWS CloudFormation để tự động hóa việc khởi tạo:

* Một cơ sở dữ liệu RDS MySQL (loại instance db.t3.micro).
* Hai hàm Lambda: Một hàm (LambdaRDSCFNInit) dùng để tạo bảng Employees và thêm dữ liệu mẫu ngay sau khi khởi tạo stack; một hàm (LambdaRDSTest) dùng để truy vấn đếm số lượng nhân viên.
* Một RESTful API Gateway với phương thức GET.
* Một tài nguyên Secret trong Secrets Manager với mật khẩu được tạo ngẫu nhiên.

Điểm cốt lõi về mặt kỹ thuật:

* **Tham chiếu động (Dynamic References):** CloudFormation sử dụng tính năng tham chiếu động để lấy mật khẩu từ Secrets Manager khi tạo RDS instance. Điều này đảm bảo CloudFormation không ghi log hoặc lưu lại mật khẩu dưới dạng văn bản thuần túy (plain text).
* **Tự động xoay vòng mật khẩu (Automatic Rotation):** Cấu hình tài nguyên AWS SecretsManager RotationSchedule phối hợp với một hàm Lambda xoay vòng để tự động thay đổi mật khẩu cơ sở dữ liệu RDS sau mỗi 30 ngày.

Việc kết hợp Lambda với AWS Secrets Manager giúp tự động quản lý vòng đời của các thông tin nhạy cảm, giảm chi phí vận hành hạ tầng bảo mật riêng và nâng cao đáng kể mức độ an toàn cho các ứng dụng Serverless.

![Hình Ảnh](/images/3-BlogsPosted/blog1.1.jpg)

- **Link bài viết:** [Bài viết trên AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/posts/2187144322050528)
- **Link blog:** [How to securely provide database credentials to Lambda functions by using AWS Secrets Manager](https://aws.amazon.com/vi/blogs/security/how-to-securely-provide-database-credentials-to-lambda-functions-by-using-aws-secrets-manager/?fbclid=IwY2xjawS3AUNleHRuA2FlbQIxMABicmlkETFWanVNbWhjdjRGR0g4NEFxc3J0YwZhcHBfaWQQMjIyMDM5MTc4ODIwMDg5MgABHu4APzf301MYy7P9_61k2xiY8s_uPcppPQp_j0D1ebu-DsVcDHbhTa6vYkDd_aem_n-kVGqUxYdH_v_p1Btu0RA)
