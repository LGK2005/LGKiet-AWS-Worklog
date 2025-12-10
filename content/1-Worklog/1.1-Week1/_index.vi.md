---
title: "Nhật ký tuần 1"
date: "2025-09-09"
weight: 01
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1

* Kết nối và làm quen với các anh chị, mentor trong FCJ.
* Tìm hiểu cảm giác và môi trường làm việc văn phòng.
* Học những kiến thức cơ bản về AWS, giao diện Console và CLI.
* Học cách sử dụng AWS Pricing Calculator để ước tính chi phí.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | - Đọc nội quy thực tập. <br> - Tạo tài khoản AWS mới. <br> - Tìm hiểu tổng quan AWS là gì. <br> - Hoàn thành Module 1 Lab 1 (tạo tài khoản AWS và quản lý user groups). <br> - Hoàn thành Module 1 Lab 7 (tạo budget để quản lý chi phí sử dụng dịch vụ). <br> - Lab 7-3 (Usage Budget) không thực hiện được do lỗi dropdown Usage Type không hiển thị dữ liệu. <br> - Hoàn thành Module 1 Lab 9 (tìm hiểu AWS Support, các gói hỗ trợ, lợi ích và cách tạo yêu cầu hỗ trợ). | 08/09/2025 | 08/09/2025 | [Tạo tài khoản AWS mới](https://000001.awsstudygroup.com/1-create-new-aws-account/) <br><br> [Bật MFA cho tài khoản AWS](https://000001.awsstudygroup.com/2-mfa-setup-for-aws-user-root/) <br><br> [Tạo nhóm Admin và user Admin](https://000001.awsstudygroup.com/3-create-admin-user-and-group/) <br><br> [Xác thực tài khoản AWS](https://000001.awsstudygroup.com/4-verify-new-account/) <br><br> [Khám phá AWS Management Console](https://000001.awsstudygroup.com/5-explore-and-configure-the-aws-management-console/) <br><br> [Tạo và quản lý case hỗ trợ](https://000001.awsstudygroup.com/6-support-cases/) |
| Ba | - Tìm hiểu tổng quan về AWS Budgets và tầm quan trọng của quản lý chi phí trên AWS. <br> - Học 4 loại budget chính: Cost Budget, Usage Budget, RI Budget, Savings Plans Budget. <br> - Thực hành tạo Cost Budget để nhận cảnh báo khi chi phí vượt ngưỡng. <br> - Thực hành tạo Usage Budget để theo dõi mức sử dụng của một số dịch vụ (ví dụ: giờ chạy EC2). <br> - Phân biệt RI Budget và Savings Plans Budget, hiểu cách chúng giúp tối ưu chi phí dài hạn. <br> - Ôn lại best practices khi đặt ngưỡng cảnh báo và cấu hình người nhận thông báo. | 09/09/2025 | 09/09/2025 | [Bắt đầu với AWS Budget](https://000007.awsstudygroup.com/vi/) <br><br> [Tạo Cost Budget](https://000007.awsstudygroup.com/vi/1-cost-budgets/) <br><br> [Tạo Usage Budget](https://000007.awsstudygroup.com/vi/2-usage-budget/) <br><br> [Tạo RI Budget](https://000007.awsstudygroup.com/vi/3-reservation-budget/) <br><br> [Tạo Savings Plans Budget](https://000007.awsstudygroup.com/vi/4-saving-plans-budget/) |
| Tư | - Tìm hiểu tổng quan về AWS Identity and Access Management (IAM). <br> - Học các khái niệm cốt lõi: IAM User, IAM Group, IAM Role, IAM Policy. <br> - Thực hành tạo IAM Group và IAM User cho quyền admin. <br> - Gán và kiểm thử IAM Policy theo nguyên tắc least privilege (ít quyền nhất cần thiết). <br> - Tạo IAM Role để cấp quyền tạm thời thay vì dùng credential dài hạn. <br> - Thực hành switch role giữa các tài khoản và kiểm tra quyền truy cập. <br> - Ôn lại các best practices bảo mật với IAM như dùng CloudTrail để giám sát và dọn dẹp user/role không còn dùng. | 10/09/2025 | 10/09/2025 | [Giới thiệu IAM](https://000002.awsstudygroup.com/vi/1-introduction/) <br><br> [Tạo IAM Group và IAM User](https://000002.awsstudygroup.com/vi/2-create-admin-user-and-group/) <br><br> [Tạo IAM Role](https://000002.awsstudygroup.com/vi/3-aws-role/) <br><br> [Chuyển đổi IAM Role](https://000002.awsstudygroup.com/vi/4-switch-roles/) |
| Năm | - Tìm hiểu tổng quan Amazon Virtual Private Cloud (VPC) và vai trò của VPC trong kiến trúc mạng trên AWS. <br> - Học các thành phần chính của VPC: Subnet, Route Table, Internet Gateway, NAT Gateway, VPN Gateway. <br> - Tìm hiểu bảo mật mạng với Security Group và Network ACL. <br> - Thực hành triển khai EC2 vào public và private subnet. <br> - Học và thực hành thiết lập kết nối Site-to-Site VPN giữa on-premises và AWS VPC. <br> - Ôn lại best practices trong việc chia subnet, phân tầng mạng và giám sát lưu lượng với VPC Flow Logs. | 11/09/2025 | 11/09/2025 | [Giới thiệu Amazon VPC](https://000003.awsstudygroup.com/vi/1-introduce/) <br><br> [Network Security với Security Group và NACL](https://000003.awsstudygroup.com/vi/2-firewallinvpc/) <br><br> [Triển khai EC2 Instance](https://000003.awsstudygroup.com/vi/4-createec2server/) <br><br> [Thiết lập AWS Site-to-Site VPN](https://000003.awsstudygroup.com/vi/5-vpnsitetosite/) |
| Sáu | - Tìm hiểu tổng quan về Amazon Elastic Compute Cloud (Amazon EC2) và các use case phổ biến. <br> - Ôn lại các yêu cầu trước khi dùng EC2: key pair, security group, cấu hình mạng. <br> - Thực hành tạo EC2 Windows và kết nối qua RDP. <br> - Thực hành tạo EC2 Linux và kết nối qua SSH. <br> - Thử các thao tác EC2 cơ bản: start, stop, reboot, đổi instance type, quản lý volume. <br> - Triển khai ứng dụng mẫu AWS User Management trên cả EC2 Linux và Windows bằng Node.js. <br> - Ôn lại cách quản lý chi phí và sử dụng EC2, dọn dẹp tài nguyên sau khi lab để tránh phát sinh thêm chi phí. | 12/09/2025 | 12/09/2025 | [Giới thiệu Amazon EC2](https://000004.awsstudygroup.com/vi/1-introduce/) <br><br> [Các bước chuẩn bị](https://000004.awsstudygroup.com/vi/2-prerequiste/) <br><br> [Khởi tạo Windows instance](https://000004.awsstudygroup.com/vi/3-launchwindowsinstance/) <br><br> [Khởi tạo Linux instance](https://000004.awsstudygroup.com/vi/4-launchlinuxinstance/) <br><br> [Amazon EC2 cơ bản](https://000004.awsstudygroup.com/vi/5-amazonec2basic/) <br><br> [Triển khai ứng dụng Node.js trên EC2](https://000004.awsstudygroup.com/vi/6-awsfcjmanagement-linux/) |

### Kết quả đạt được trong tuần 1

* Xây dựng và bảo vệ thành công một tài khoản AWS mới, bao gồm bật MFA, tạo user/admin IAM và làm quen với quy trình hỗ trợ của AWS.
* Nắm được nền tảng quản lý chi phí trên AWS thông qua việc tạo và kiểm thử nhiều loại AWS Budgets với cảnh báo qua email.
* Củng cố kiến thức về bảo mật và quản trị truy cập với IAM (user, group, role, policy) theo nguyên tắc least privilege.
* Thiết kế và vận hành được mạng cơ bản trên AWS với Amazon VPC, gồm public/private subnet, security group, NACL và kết nối Site-to-Site VPN.
* Triển khai và quản lý được EC2 Windows và Linux, biết cách kết nối RDP/SSH và thực hiện các thao tác vận hành thường gặp.
* Triển khai thành công ứng dụng quản lý người dùng mẫu trên EC2 và luyện thói quen dọn dẹp tài nguyên sau lab để kiểm soát chi phí.
