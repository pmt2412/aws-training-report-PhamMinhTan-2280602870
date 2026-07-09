---
title: "Worklog Tuần 1"
date: 2026-04-20
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Tham dự buổi kickoff, làm quen và nắm bắt lộ trình của chương trình.
* Đọc và nắm vững các nội quy, quy định chung của đơn vị thực tập, cũng như quy trình làm việc offline và online.
* Thiết lập tài khoản AWS và hoàn thành các nhiệm vụ nhận thêm AWS credits.
* Nắm vững các khái niệm cơ bản và thực hành quản trị quyền truy cập an toàn với AWS IAM.
* Hiểu và thực hành xây dựng kiến trúc mạng cơ bản trên AWS với Amazon VPC và thiết lập AWS Site-to-Site VPN.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 6 (tuần trước) | - Tham dự buổi kickoff | 17/04/2026 | 17/04/2026 | |
| 2 | - Xem các video hướng dẫn cơ bản trên YouTube để làm quen <br> - Đọc Notion Group description (TP.HCM) và Nội quy (hcm-rules.awsfcaj.com) | 20/04/2026 | 20/04/2026 | [Youtube Playlist AWS](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) <br> [Nội quy HCM](https://hcm-rules.awsfcaj.com/1-regulations/) <br> [Group Info](https://app.notion.com/p/Group-description-TP-HCM-347df829a730809a8f63d39505644917) |
| 3 | - Tạo tài khoản AWS <br> - Hoàn thành 5 nhiệm vụ để nhận thêm 100 AWS credits | 21/04/2026 | 21/04/2026 | [Hướng dẫn nhận 200 credit](https://000001.awsstudygroup.com/vi/3-chi%E1%BA%BFn-l%C6%B0%E1%BB%A3c-nh%E1%BA%ADn-%C4%91%E1%BB%A7-200-credit/) <br> [Chi tiết 5 nhiệm vụ](https://000001.awsstudygroup.com/vi/4-h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn-chi-ti%E1%BA%BFt-5-nhi%E1%BB%87m-v%E1%BB%A5-ki%E1%BA%BFm-ti%E1%BB%81n/) |
| 4 | - **Thực hành Lab 000002 (IAM):** <br>&emsp; + Tìm hiểu tổng quan IAM <br>&emsp; + Tạo IAM Group và IAM User <br>&emsp; + Phân quyền với IAM Policy <br>&emsp; + Tạo và chuyển đổi IAM Role | 22/04/2026 | 23/04/2026 | [000002 - AWS IAM](https://000002.awsstudygroup.com/vi/) |
| 5 | - **Thực hành Lab 000003 (VPC & VPN):** <br>&emsp; + Giới thiệu Amazon VPC <br>&emsp; + Network Security với Security Groups và Network ACLs <br>&emsp; + Chuẩn bị môi trường & Triển khai EC2 Instance | 24/04/2026 | 25/04/2026 | [000003 - AWS VPC & VPN](https://000003.awsstudygroup.com/vi/) |
| 6 | - **Thực hành Lab 000003 (Tiếp tục):** <br>&emsp; + Thiết lập AWS Site-to-Site VPN <br>&emsp; + Dọn dẹp tài nguyên (Clean up) | 26/04/2026 | 26/04/2026 | [000003 - AWS VPC & VPN](https://000003.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 1:

* Đã tham dự buổi kickoff, nắm rõ quy định và lộ trình triển khai của chương trình.
* Đã đọc và nắm vững các nội quy chung của chương trình bao gồm: quy định về chuyên cần và điểm danh, quy định về thời gian, quy định về trang phục và tác phong, quy định về ra vào văn phòng, quy định về ăn uống và sử dụng đồ dùng tại văn phòng, cũng như quy định về việc sử dụng nhà vệ sinh.
* Nắm rõ quy trình và yêu cầu khi đăng ký lên văn phòng làm việc offline tại Tầng 26, Tòa nhà Bitexco (số 2 Hải Triều, phường Bến Nghé, Quận 1, TP. Hồ Chí Minh) và các quy định ứng xử chuẩn mực trong group chat.
* Cập nhật và nắm được các thông tin chính thức phục vụ việc xác nhận thực tập: Công ty TNHH Amazon Web Services Viet Nam, vị trí thực tập Workforce Bootcamp - First Cloud AI Journey.
* Khởi tạo thành công tài khoản AWS và hoàn thiện 5 nhiệm vụ thực hành để nhận thêm 100 AWS credits.
* Hoàn thành việc theo dõi playlist video hướng dẫn trên YouTube để xây dựng nền tảng kiến thức AWS cơ bản.
* **Đối với Bài thực hành 000002 (Quản trị quyền truy cập với IAM):**
  * Hiểu rõ cơ chế Xác thực (Authentication) và Ủy quyền (Authorization) tập trung của AWS.
  * Nắm vững và thực hành tạo lập 4 thành phần cốt lõi: IAM Users, IAM Groups, IAM Policies, và IAM Roles.
  * Nắm được phương pháp phân quyền bảo mật tuân thủ nguyên tắc đặc quyền tối thiểu (principle of least privilege).
* **Đối với Bài thực hành 000003 (Amazon VPC và AWS Site-to-Site VPN):**
  * Triển khai thành công kiến trúc mạng ảo Amazon VPC bám sát tiêu chuẩn AWS Well-Architected Framework.
  * Thiết lập và cấu hình các lớp bảo mật mạng quan trọng bằng Security Groups và Network ACLs.
  * Cấu hình thành công môi trường máy chủ EC2 Instance và thiết lập kết nối an toàn AWS Site-to-Site VPN giữa môi trường giả lập on-premise và AWS.