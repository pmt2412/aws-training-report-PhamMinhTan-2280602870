---
title: "Blog 3"
date: 2026-06-26
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# VPC LINK TRONG TÍCH HỢP RIÊNG TƯ (PRIVATE INTEGRATIONS) CỦA AMAZON API GATEWAY

VPC link là một tài nguyên trong Amazon API Gateway cho phép kết nối các route của API đến các tài nguyên nội bộ, riêng tư nằm bên trong một mạng VPC. Bài viết đi sâu vào các công nghệ cốt lõi đứng sau VPC Link (như AWS Hyperplane và AWS PrivateLink) và so sánh sự khác biệt cơ bản về mặt kiến trúc giữa VPC Link dành cho REST API và HTTP API.

Các công nghệ nền tảng:

* **AWS Hyperplane:** Nền tảng ảo hóa mạng nội bộ của AWS, hỗ trợ kết nối và định tuyến giữa các VPC khác nhau. Hyperplane sử dụng VPC-to-VPC NAT thay vì PrivateLink.
* **AWS PrivateLink:** Công nghệ cho phép truy cập các dịch vụ AWS một cách riêng tư trong mạng nội bộ của AWS mà không cần đi qua Internet công cộng. Traffic được dẫn qua các Interface VPC Endpoint (phía người dùng) và VPC Endpoint Service (phía nhà cung cấp).

Phân biệt Private API và Private Integration (Tích hợp riêng tư):

* **Private API:** Nghĩa là Endpoint của API Gateway chỉ có thể truy cập được từ bên trong VPC (hoặc qua Direct Connect/VPN). Hiện tại chỉ có REST API hỗ trợ cấu hình Private API.
* **Private Integration:** Nghĩa là hệ thống backend (phía sau API Gateway) nằm trong VPC và không công khai ra Internet. API Gateway sử dụng VPC Link để kết nối tới backend này một cách an toàn.

So sánh VPC Link cho REST API và HTTP API:

* **Công nghệ nền tảng:** Điểm khác biệt đầu tiên nằm ở công nghệ nền tảng. VPC Link của REST API dựa trên AWS PrivateLink, trong khi HTTP API sử dụng hệ thống dịch mã địa chỉ mạng VPC-to-VPC NAT của AWS Hyperplane. Sự khác biệt này dẫn đến những thay đổi lớn về kiến trúc bên dưới.
* **Tài nguyên và khả năng tích hợp:** HTTP API linh hoạt hơn hẳn khi không cần VPC Endpoint Service mà có thể kết nối trực tiếp đến ALB, NLB hoặc các dịch vụ qua AWS Cloud Map; một VPC Link duy nhất cũng có thể liên kết với nhiều backend. Ngược lại, REST API bắt buộc phải có VPC Endpoint Service, kết nối qua một NLB duy nhất và việc cấu hình đa backend phức tạp hơn nhiều.
* **Tính năng và địa chỉ IP:** HTTP API tận dụng được sức mạnh Layer 7 của ALB (như định tuyến nâng cao, xác thực) và hoạt động theo cơ chế tạo tunnel bằng các ENI. Trong khi đó, REST API bị hạn chế hơn ở Layer 7 do đi qua NLB (Layer 4) và hệ thống backend phía sau sẽ nhận diện IP nguồn là IP riêng của chính NLB đó.

Hiểu rõ sự khác biệt giữa hai loại VPC Link giúp các kỹ sư kiến trúc đưa ra quyết định chính xác khi thiết kế hệ thống. Nếu bạn cần một giải pháp API tiết kiệm chi phí, tốc độ cao hơn và tích hợp linh hoạt với ALB hoặc container, VPC Link cho HTTP API là sự lựa chọn tối ưu. Nếu bạn yêu cầu các tính năng chuyên sâu riêng biệt của REST API, bạn sẽ cần cấu hình VPC Link qua cơ chế PrivateLink với NLB.

![Hình Ảnh 1](/images/3-BlogsPosted/blog3.1.jpg)
![Hình Ảnh 2](/images/3-BlogsPosted/blog3.2.jpg)
![Hình Ảnh 3](/images/3-BlogsPosted/blog3.3.jpg)

- **Link bài viết:** [Bài viết trên AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/posts/2194084638023163)
- **Link blog:** [Understanding VPC links in Amazon API Gateway private integrations](https://aws.amazon.com/vi/blogs/compute/understanding-vpc-links-in-amazon-api-gateway-private-integrations/?fbclid=IwY2xjawS3BcdleHRuA2FlbQIxMABicmlkETFWanVNbWhjdjRGR0g4NEFxc3J0YwZhcHBfaWQQMjIyMDM5MTc4ODIwMDg5MgABHnd0siHru9JZp4Q6bMUgKNmulHBR7GCnCVUQsPz38_-8DjQKWSt0fGK1uS9s_aem_8iC52_t_hIPs-EySNErBaA)