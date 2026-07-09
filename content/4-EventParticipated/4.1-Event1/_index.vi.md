---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch “FCAJ Community Day” (23/05)

### Mục Đích Của Sự Kiện

- Cập nhật các xu hướng, tư duy mới nhất về Trí tuệ nhân tạo (LLMs, Prompting, Multi-Agent).
- Tìm hiểu chuyên sâu và thực hành tối ưu hóa các dịch vụ của AWS (Amazon CloudFront, Amazon Quick).
- Chia sẻ kinh nghiệm thực chiến từ các cuộc thi Hackathon và quá trình xây dựng sản phẩm thực tế.
- Nhận định về nhu cầu thị trường hiện nay và định hướng tạo ra giá trị cho bản thân.

### Danh Sách Diễn Giả

- **Tinh Truong** - Chia sẻ về tầm quan trọng của Context trong AI.
- **Anh Pham** - Giới thiệu Friendly AI Assistant với Amazon Quick.
- **Thinh Nguyen** - Tối ưu hóa và bảo mật với Amazon CloudFront.
- **Team VIB** - Chia sẻ hành trình 36 giờ tại LotusHacks.
- **Duc Dao** - Bàn về tính phi quyết định (Non-Determinism) của LLM.
- **Vy Lam** - Hệ thống Multi-Agent cấp doanh nghiệp (Enterprise-Grade).
- **Anh Hưng** - Chia sẻ về nhu cầu thị trường và định hướng cá nhân.

### Nội Dung Nổi Bật

#### 1. Context Is Everything & Tư Duy Prompting (Tinh Truong & Anh Hưng)
- Để tạo ra giá trị bản thân trong thị trường, cần biết cách sử dụng AI hiệu quả. AI sẽ thất bại nếu thiếu context (ngữ cảnh). 
- Giới thiệu khái niệm **Second AI Brain** (sử dụng công cụ Obsidian).
- **Nguyên tắc viết prompt chuẩn:** 
  - Chỉ đưa cho AI ngữ cảnh nó thực sự cần, không cung cấp dư thừa những gì AI đã biết, không gom bừa bãi mọi thứ trên Internet vào prompt.
  - Đừng mô tả quá rõ ràng những thứ hiển nhiên. Thay vào đó, hãy chỉ định rõ **vấn đề gì cần giải quyết tiếp theo**.

#### 2. Friendly AI Assistant với Amazon Quick (Anh Pham)
- AWS cung cấp nền tảng kết nối hệ sinh thái để user tự build sản phẩm.
- Các tính năng nổi bật: **Quick Chat Agent** (khám phá dữ liệu, phân tích insight), **Quick Flows** (tạo workflow thông minh bằng ngôn ngữ tự nhiên - no coding), **Quick Spaces** (không gian làm việc nhóm), **Quick Sight** (tạo dashboard từ raw data).
- **Demo thực tế:** Dùng AI phân tích insight từ file Excel; tạo Agent tự động tóm tắt nội dung cuộc họp và gửi thông tin đến mọi người qua email.

#### 3. Amazon CloudFront as Your Foundation (Thinh Nguyen)
- Nhóm người dùng mục tiêu đa dạng từ chủ website nhỏ đến doanh nghiệp lớn với cơ chế tính tiền mới (dễ dàng estimate, fixed-price CDN).
- **Khả năng bảo mật (Security Capabilities):** Hỗ trợ HTTPS, Mutual TLS, Origin Cloaking (tạo đường hầm VPC từ CloudFront vào Origin), Viewer access control (block theo khu vực), Content protection với Signed URL (ngăn copy-paste link).
- **Kiến trúc & Hiệu suất:** Multi-layer caching, HTTP/3 (QUIC/UDP) multiplexing, duy trì kết nối TCP (Persistent connections to origins) để tránh việc phải bắt tay TCP (handshake) nhiều lần.
- Tính năng điều hướng thông minh với **CloudFront Functions**.

#### 4. Hành trình 36 giờ tại LotusHacks (Team VIB)
- Quá trình từ "Zero to Idea", brainstorming và định hình vấn đề để xây dựng ứng dụng **UTMorpho** trong 36 giờ phát triển liên tục.
- Demo ứng dụng cho phép tương tác trực tiếp với các component để chỉnh sửa thông qua 3 AI agents, có khả năng xem lại lịch sử chỉnh sửa.

#### 5. Bản chất "Non-Determinism" của LLMs (Duc Dao)
- Cơ chế của LLM là xếp hạng điểm số cho các từ trong prompt để lựa chọn từ phù hợp nhất trong kho vocab của nó.
- **Thực tế:** Nhiều người lầm tưởng đặt `Temperature = 0` sẽ đảm bảo tính nhất quán (determinism). Tuy nhiên, các nhà cung cấp LLM hiện nay sử dụng các phương pháp tối ưu hóa suy luận (Inference optimization methods) để tối ưu chi phí (plan pricing), dẫn đến việc đầu ra vẫn có thể thay đổi.

#### 6. Enterprise-Grade Multi-Agent System (Vy Lam)
- **Case Study:** Đánh giá điểm tín dụng cho doanh nghiệp Startup (Startup Credit Scoring). Khó khăn nằm ở việc dữ liệu startup không khớp với cấu trúc hệ thống ngân hàng truyền thống.
- **Mô hình Multi-Agent:** Đóng vai trò như một hội đồng ảo với các chức danh chuyên biệt: Financial analyst, Market analyst, Team evaluator, Risk assessor, Compliance.
- **Lý do kiến trúc này hoạt động hiệu quả:** Specialized expertise (chuyên môn hóa), checks & balance (kiểm tra chéo), parallel processing (xử lý song song), auditability, tính mở rộng và chịu lỗi (fault tolerance). Hệ thống phải đảm bảo an toàn, tin cậy và tuân thủ (Guardrails & Compliance).

### Những Gì Học Được & Ứng Dụng

- **Thay đổi tư duy giao tiếp với AI:** Hiểu được sức mạnh của "Context". Trong công việc hàng ngày, tôi sẽ áp dụng nguyên tắc cung cấp bối cảnh vừa đủ, tránh nhiễu thông tin, và xây dựng "Second AI Brain" để lưu trữ, hệ thống hóa kiến thức cá nhân một cách thông minh.
- **Tối ưu hạ tầng mạng trên AWS:** Nắm bắt được các tính năng bảo mật nâng cao của CloudFront (Origin Cloaking, Signed URLs) và cơ chế persistent connections. Điều này rất hữu ích để thiết kế các kiến trúc web an toàn và phản hồi nhanh hơn.
- **Thiết kế hệ thống AI tiên tiến:** Nhận thức rõ giới hạn của một AI đơn lẻ (Single Agent) và hiểu được cách thiết kế mô hình Multi-Agent để giải quyết các bài toán phức tạp đòi hỏi kiểm tra chéo, đặc biệt trong các nghiệp vụ doanh nghiệp khắt khe.
- **Hiểu sâu về LLM:** Nhận thức được tính phi quyết định của LLM dù ở `Temperature = 0`, từ đó có chiến lược xử lý lỗi (mitigation strategies) phù hợp khi tích hợp AI vào ứng dụng thực tế.

#### Một số hình ảnh khi tham gia sự kiện
![Ảnh check-in sự kiện](/images/4-EventParticipated/checkin1.png)


![Ảnh tại sự kiện](/images/4-EventParticipated/event1.jpg)


> Tham gia FCAJ Community Day không chỉ mang lại cho tôi những cập nhật công nghệ mới nhất về AWS và GenAI, mà còn cung cấp những góc nhìn thực tế từ các bài toán doanh nghiệp, giúp tôi định hướng tốt hơn con đường phát triển chuyên môn sắp tới.