---
title: "Bản đề xuất"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SaaS HR Multi-Tenant Microservices
## Giải pháp AWS Cloud hợp nhất cho quản lý nhân sự đa doanh nghiệp quy mô Enterprise

### 1. Tóm tắt điều hành
SaaS HR Multi-Tenant Microservices là nền tảng quản lý nhân sự đa doanh nghiệp (SaaS) quy mô Enterprise được thiết kế nhằm nâng cao hiệu quả quản trị nhân sự và tối ưu hóa chi phí vận hành cho các tổ chức. Hệ thống hỗ trợ tối đa hàng nghìn doanh nghiệp (tenants) hoạt động độc lập trên cùng một nền tảng nhờ mô hình phân tách dữ liệu logic nghiêm ngặt (Pool Model). Giải pháp tận dụng các dịch vụ Cloud của AWS (ECS Fargate, RDS MySQL, SQS, Cognito) kết hợp xác thực bất đối xứng qua Nginx API Gateway để cung cấp trải nghiệm giám sát sức khỏe hệ thống, quản lý nhân viên, chấm công và nghỉ phép theo thời gian thực một cách bảo mật, an toàn và tiết kiệm chi phí.

### 2. Tuyên bố vấn đề
*Vấn đề hiện tại*
Các hệ thống quản lý nhân sự truyền thống thường được triển khai riêng lẻ cho từng công ty, dẫn đến lãng phí tài nguyên máy chủ và tốn nhiều công sức bảo trì. Đối với các giải pháp dùng chung (multi-tenant), thách thức lớn nhất là đảm bảo cô lập dữ liệu tuyệt đối giữa các doanh nghiệp khách hàng (ngăn ngừa rò rỉ thông tin chéo) mà vẫn duy trì hiệu năng cao khi lượng truy cập tăng đột biến, đồng thời kiểm soát tốt chi phí hạ tầng.

*Giải pháp*
Dự án phát triển hệ thống SaaS HR Multi-Tenant trên kiến trúc Microservices phân rã thành 3 dịch vụ cốt lõi: `auth-service` (xác thực), `tenant-service` (quản lý doanh nghiệp) và `hr-service` (nghiệp vụ nhân sự). Hệ thống thực thi phân tách dữ liệu logic (Pool Model) trên 3 cơ sở dữ liệu độc lập của RDS MySQL (`auth_db`, `tenant_db`, `hr_db`). Sử dụng Amazon Cognito làm nhà cung cấp danh tính tập trung phối hợp ký mã token JWT RS256, cho phép các dịch vụ tự xác thực token không trạng thái (stateless) cực kỳ nhanh chóng. Giao diện frontend ReactJS (Vite) được lưu trữ trên S3 và phân phối qua CloudFront CDN toàn cầu. Sự kiện bất đồng bộ giữa các dịch vụ được tách biệt qua Amazon SQS để đảm bảo hệ thống không bị nghẽn mạch.

*Lợi ích và hoàn vốn đầu tư (ROI)*
- **Tối ưu chi phí hạ tầng**: Bằng cách gộp 3 database của các microservices vào cùng một instance RDS MySQL Multi-AZ và sử dụng ECS Fargate serverless, chi phí vận hành hạ tầng trên AWS ước tính tối ưu chỉ khoảng 115 - 155 USD/tháng (chạy 24/7), có thể tắt/hủy toàn bộ hạ tầng (destroy) khi không dùng để đưa chi phí về gần 0 USD.
- **Tính sẵn sàng cao & Tự động sao lưu**: Hệ thống phân bổ đa vùng sẵn sàng (Multi-AZ) giúp giảm thiểu downtime, tự động phục hồi lỗi và hỗ trợ tự động backup dữ liệu định kỳ 7 ngày.
- **Tiết kiệm thời gian triển khai**: Doanh nghiệp khách hàng mới có thể đăng ký sử dụng ngay lập tức thông qua subdomain riêng biệt mà không cần bất kỳ bước cài đặt hay cấu hình máy chủ vật lý nào. Thời gian hoàn vốn đầu tư cho hệ thống ước tính từ 3–6 tháng nhờ tối ưu hiệu quả làm việc của nhân sự và giảm chi phí vận hành IT.

### 3. Kiến trúc giải pháp
Nền tảng áp dụng kiến trúc 3-tier kết hợp Microservices trên AWS để quản lý và vận hành ứng dụng. Toàn bộ các tài nguyên tính toán được triển khai bên trong một VPC trải trên 2 Availability Zone (AZ) nhằm tăng cường khả năng chịu lỗi.

![Sơ đồ kiến trúc AWS - SaaS HR Multi-Tenant](/images/2-Proposal/architecture.png)

*Dịch vụ AWS sử dụng*
- **Amazon CloudFront**: CDN phân phối frontend tĩnh và là điểm truy cập duy nhất cho toàn bộ hệ thống.
- **Amazon S3**: Lưu trữ mã nguồn build tĩnh của ứng dụng ReactJS (truy cập bảo mật qua OAC).
- **Application Load Balancer (ALB)**: Nhận traffic từ CloudFront để định tuyến các request `/api/v1/*` vào cụm ECS Fargate.
- **Amazon ECS Fargate**: Chạy các container microservices (`auth`, `tenant`, `hr`) không trạng thái ở cả 2 AZ.
- **Amazon RDS MySQL**: Hệ quản trị cơ sở dữ liệu Multi-AZ (Primary & Standby) chạy ở private subnets cách ly hoàn toàn với internet.
- **Amazon SQS**: Quản lý hàng đợi tin nhắn phục vụ giao tiếp bất đồng bộ giữa `tenant-service` và `hr-service`.
- **Amazon Cognito**: User Pools quản lý tài khoản người dùng và cấp phát JWT chứa định danh tenant ID.
- **AWS NAT Gateway, Route 53, ACM, CloudWatch, SNS, SSM Parameter Store**.

*Thiết kế thành phần*
- **Giao diện Client (Frontend)**: ReactJS (Vite) xử lý định tuyến phía client, tự động nhận diện tenant qua subdomain và lưu trữ token xác thực.
- **Cổng API & Phân tải**: ALB định tuyến động tới các target groups tương ứng trong ECS. Security Groups thiết lập theo mô hình Zero-Trust chỉ cho phép ALB kết nối tới ECS Fargate, và ECS Fargate kết nối tới RDS MySQL.
- **Tầng Dịch vụ & Xử lý nghiệp vụ**: Các microservices FastAPI xử lý logic độc lập. Token JWT được verify không trạng thái tại mỗi service thông qua khóa công khai (Public Key) nhận từ Cognito mà không cần gọi lại service xác thực.
- **Tầng Cơ sở dữ liệu & Tách biệt**: Dữ liệu lưu trữ tập trung tại RDS MySQL. Bảng `employees` và `departments` thực thi phân tách logic qua trường dữ liệu bắt buộc `tenant_id` và được tối ưu hóa chỉ mục tìm kiếm nhằm tăng tốc độ truy vấn và cô lập dữ liệu.

### 4. Triển khai kỹ thuật
*Các giai đoạn triển khai*
Dự án được triển khai song song giữa phần lập trình phát triển và thiết lập hạ tầng Cloud qua 4 giai đoạn chính:
1. *Thiết kế hạ tầng mạng & Bảo mật*: Thiết lập VPC, phân chia 6 subnets cách ly cho 3 tầng (Public, App Private, Data Private). Cấu hình Route Table và Security Groups.
2. *Phát triển mã nguồn local*: Lập trình 3 microservices FastAPI, xây dựng giao diện ReactJS, cài đặt Nginx làm API Gateway local và tích hợp cơ chế JWT RS256.
3. *Tích hợp và kiểm định an toàn dữ liệu*: Thực hiện kiểm thử cô lập dữ liệu giữa các tenant, tích hợp hệ thống hàng đợi tin nhắn bất đồng bộ qua Redis/SQS.
4. *Triển khai Cloud & Tối ưu hóa*: Đóng gói container Docker (multi-stage build), đẩy lên ECR, cấu hình và khởi chạy các dịch vụ trên AWS ECS Fargate, ALB, RDS Multi-AZ và Cognito, thiết lập hệ thống giám sát CloudWatch Logs.

*Yêu cầu kỹ thuật*
- **Trạm máy ảo phát triển local**: Cài đặt Docker Desktop để khởi chạy đồng bộ MySQL, Nginx Gateway, và 3 microservices. Python 3.10+ cho các API dịch vụ và Node.js v18+ phục vụ React frontend.
- **Nền tảng Cloud AWS**: Yêu cầu tài khoản AWS hoạt động với đầy đủ quyền khởi tạo VPC, ECS Fargate, RDS MySQL, Cognito User Pools, SQS, CloudFront, S3. Cần nắm vững kỹ thuật đóng gói Docker tối ưu và cơ chế truyền nhận khóa bất đối xứng RS256.

### 5. Lộ trình & Mốc triển khai
- **Sprint 1: Khởi tạo hạ tầng & Cài đặt API Gateway (Tuần 1)**: Cấu trúc thư mục dự án, chạy di chuyển cơ sở dữ liệu mẫu (Alembic). Thiết lập Nginx gateway local và khung React Vite cho login/dashboard.
- **Sprint 2: Core Microservices & Liên kết DB (Tuần 2)**: Xây dựng hoàn chỉnh `auth-service` (xác thực RS256), `tenant-service` (quản lý gói đăng ký), và `hr-service` với các schema dữ liệu cốt lõi hỗ trợ cô lập logic bằng `tenant_id`.
- **Sprint 3: Tích hợp Hệ thống & Định tuyến Subdomain (Tuần 3)**: Kết nối giao diện ReactJS với API Gateway, cấu hình Axios interceptor tự động đính kèm token, xử lý phân quyền người dùng và giao diện quản lý nhân sự trực quan.
- **Sprint 4: Đóng gói Container, Bảo mật & Triển khai Cloud (Tuần 4)**: Đóng gói Docker production siêu nhẹ (multi-stage), quét bảo mật (Trivy), thiết lập hạ tầng AWS và kiểm thử hệ thống cuối cùng bằng một lệnh duy nhất.

### 6. Ước tính ngân sách
Bảng dự toán chi phí vận hành trên AWS (Vùng Singapore, chạy 24/7):

| Dịch vụ AWS | Cấu hình đề xuất | Ước tính / Tháng |
| :--- | :--- | :---: |
| **NAT Gateway** | 1 cổng NAT dùng chung ở public subnet | **$43 – $50** |
| **RDS MySQL Multi-AZ** | Instance `db.t4g.micro` x2, 20 GB gp3 storage & backup | **$30 – $38** |
| **Application Load Balancer** | 1 ALB phục vụ chia tải + ~1 LCU | **$22 – $26** |
| **ECS Fargate** | 4–6 tasks chạy các dịch vụ (0.25 vCPU / 0.5 GB) | **$12 – $18** |
| **Data Transfer / CloudWatch** | Logs lưu 7 ngày, NAT data transfer, inter-AZ | **$5 – $12** |
| **Route 53 + CloudFront + S3 + ECR** | Tên miền, CDN, Hosting tĩnh React, Container registry | **$3 – $8** |
| **Cognito / SQS / SNS / ACM** | Nằm trong định mức miễn phí (Free Tier) | **$0** |
| **Tổng cộng / Tháng** | | **≈ $115 – $155 (~$130)** |

> [!NOTE]
> Để tối ưu hóa chi phí học tập và thử nghiệm, toàn bộ tài nguyên có thể được tạo và dọn dẹp (destroy) nhanh chóng bằng script CDK/Terraform khi không chạy demo.

### 7. Đánh giá rủi ro
*Ma trận rủi ro*
- **Rò rỉ dữ liệu chéo giữa các tenant**: Ảnh hưởng: Rất nghiêm trọng; Xác suất: Thấp.
- **Quá tải API Gateway hoặc Microservice**: Ảnh hưởng: Cao; Xác suất: Trung bình.
- **Vượt ngân sách AWS**: Ảnh hưởng: Trung bình; Xác suất: Trung bình.

*Chiến lược giảm thiểu*
- **Ngăn rò rỉ dữ liệu**: Thiết lập cơ chế kiểm tra và tự động inject điều kiện `tenant_id` từ claims của token JWT đã verify vào mọi câu lệnh truy vấn database thông qua Dependency Injection trong FastAPI.
- **Chống quá tải**: Cấu hình cơ chế tự động mở rộng (Auto Scaling) của ECS Fargate dựa trên CPU/Memory và phân tách tải bất đồng bộ qua hàng đợi SQS cho các luồng xử lý nặng.
- **Quản lý chi phí**: Kích hoạt AWS Budgets cảnh báo qua email (SNS) khi chi phí dự kiến vượt quá 100 USD hoặc 130 USD.

*Kế hoạch dự phòng*
- Sử dụng công cụ tự động dọn dẹp các tài nguyên không sử dụng trên AWS (Orphaned volumes, unused NAT gateways, stale Elastic IPs).
- Lưu trữ các bản backup dữ liệu RDS ở định dạng nén an toàn và tách biệt để phục hồi khi có sự cố thảm họa.

### 8. Kết quả kỳ vọng
- **Cải tiến kỹ thuật**: Hệ thống đa khách hàng chạy mượt mà, phân tách dữ liệu tuyệt đối giữa các doanh nghiệp, tự động định tuyến theo subdomain, xác thực bảo mật chuẩn Enterprise (RS256 JWT).
- **Giá trị dài hạn**: Xây dựng được bộ tài liệu thiết kế và mã nguồn kiến trúc microservices mẫu chất lượng cao, dễ dàng tái sử dụng cho các dự án SaaS tiếp theo.