---
title: "Nhật ký tuần 10"
date: "2025-11-10"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10

* Thử nghiệm host website tĩnh bằng S3 và CloudFront.
* Thiết lập domain thật và luồng CI/CD cho dashboard viết bằng React.
* Khám phá mod âm thanh cho World of Tanks bằng Wwise.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | Xây một website test cơ bản để kiểm tra static hosting với S3 và CloudFront: <br> &emsp; + Tạo một S3 bucket cấu hình cho static website hosting. <br> &emsp; + Deploy một trang HTML/CSS đơn giản lên S3. <br> &emsp; + Tạo CloudFront distribution trỏ tới bucket để kiểm tra phân phối nội dung toàn cầu và caching. | 10/11/2025 | 10/11/2025 | Tài liệu AWS S3 & CloudFront |
| Ba | Mua domain trên Porkbun và kết nối vào website: <br> &emsp; + Đăng ký custom domain và cấu hình DNS record. <br> &emsp; + Trỏ domain về CloudFront distribution. <br> Bắt đầu hiện thực dashboard bằng React để sau này deploy lên S3/CloudFront. | 11/11/2025 | 11/11/2025 | Tài liệu Porkbun, tài liệu React |
| Tư | Đưa project React dashboard lên GitHub và thiết lập tự động deploy: <br> &emsp; + Push source code dashboard lên GitHub repository. <br> &emsp; + Cấu hình GitHub Actions workflow để build React app mỗi lần push. <br> &emsp; + Tự động upload build lên S3 và invalidate CloudFront cache sau mỗi lần deploy. | 12/11/2025 | 12/11/2025 | Tài liệu GitHub Actions, AWS CLI / SDK |
| Năm | Tìm hiểu cách mod âm thanh cho World of Tanks bằng Wwise: <br> &emsp; + Đọc kỹ hướng dẫn mod Wwise và tài liệu mod chính thức cho World of Tanks. <br> &emsp; + Nghiên cứu cấu trúc thư mục, sound bank và luồng sự kiện âm thanh cần thiết. <br> &emsp; + Ghi chú cách đóng gói và nạp mod âm thanh custom vào game. | 13/11/2025 | 13/11/2025 | [Hướng dẫn tạo Wwise mod](https://wotmod.mtm-gaming.org/resources/Wwise%20mods%20creation.pdf) <br><br> [Wwise project cho World of Tanks](https://wgmods.net/4894/) |
| Sáu | Hoàn thành và phát hành một sound mod cho World of Tanks bằng Wwise: <br> &emsp; + Chốt sound bank và cấu hình theo đúng hướng dẫn. <br> &emsp; + Test mod trong game để đảm bảo âm thanh load và phát đúng như mong muốn. <br> &emsp; + Đăng mod hoàn chỉnh lên WGMods: [Mod đã phát hành](https://wgmods.net/7398/). | 14/11/2025 | 14/11/2025 | – |

### Kết quả đạt được trong tuần 10

* Thiết lập thành công website tĩnh trên S3 + CloudFront và gắn được custom domain mua trên Porkbun.
* Xây dựng và tự động hóa luồng deploy cho React dashboard bằng GitHub Actions, S3 và CloudFront invalidation.
* Hoàn thiện và phát hành một sound mod cho World of Tanks sử dụng Wwise, bao gồm test trong game và đưa mod lên WGMods.
