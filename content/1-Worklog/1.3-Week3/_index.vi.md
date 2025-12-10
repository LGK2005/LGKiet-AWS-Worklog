---
title: "Nhật ký tuần 3"
date: "2025-09-22"
weight: 03
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3

* Tiếp tục hồi phục sức khỏe nhưng vẫn cố gắng duy trì tiến độ học khi có thể.
* Bắt đầu tìm hiểu Amazon S3 và cách host website tĩnh.
* Ôn lại IAM, Organizations và các best practices về bảo mật khi tình trạng ổn hơn.
* Trao đổi ý tưởng project với các bạn trong nhóm khi có thời gian và sức.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | Vẫn đang trong giai đoạn hồi phục sau phẫu thuật. <br> Đi bệnh viện rửa vết thương và thay băng. <br> Chủ yếu nghỉ ngơi tại nhà, chưa thể ngồi hoặc dùng máy tính lâu, không làm bài lab hay học gì thêm. | 22/09/2025 | 22/09/2025 | – |
| Ba | Tiếp tục đi bệnh viện thay băng và kiểm tra vết thương. <br> Mức độ đau vẫn còn cao, phần lớn thời gian phải nằm nghỉ. <br> Chỉ xem lướt ghi chú trên điện thoại, chưa làm lab thực hành. | 23/09/2025 | 23/09/2025 | – |
| Tư | Ngày cuối của lịch thay băng dày đặc tại bệnh viện. <br> Bác sĩ cho phép bắt đầu ngồi dậy trong thời gian ngắn, nhưng vẫn chưa đủ sức để tập trung học lâu. <br> Dành thời gian suy nghĩ về phần S3 sẽ học và một vài ý tưởng cho project, nhưng chưa làm lab thực tế. | 24/09/2025 | 24/09/2025 | – |
| Năm | Sức khỏe khá hơn, có thể ngồi bàn học trong thời gian ngắn, vẫn phải đi thay băng hằng ngày nhưng bắt đầu học lại. <br> Bắt đầu “Khởi đầu với Amazon S3”: <br> &emsp; + Tìm hiểu khái niệm cơ bản về Amazon S3, cách lưu trữ dạng object, độ bền dữ liệu và các use case phổ biến. <br> &emsp; + Xem lại các yêu cầu trước khi làm việc với S3 và tạo một S3 bucket cho lab. <br> &emsp; + Bật chế độ static website hosting cho S3 bucket và upload các file website ban đầu. <br> &emsp; + Cấu hình Block Public Access để mở public có kiểm soát cho website. | 25/09/2025 | 25/09/2025 | [Giới thiệu Amazon S3](https://000057.awsstudygroup.com/vi/1-introduce/) <br><br> [Chuẩn bị](https://000057.awsstudygroup.com/vi/2-prerequiste/) <br><br> [Bật Static Website](https://000057.awsstudygroup.com/vi/3-staticwebsite/) <br><br> [Cấu hình Block Public Access](https://000057.awsstudygroup.com/vi/4-blockpublicaccess/) |
| Sáu | Vẫn phải thay băng mỗi ngày nhưng đã ngồi được lâu hơn, tiếp tục làm lab về S3. <br> Tiếp tục “Khởi đầu với Amazon S3”: <br> &emsp; + Cấu hình quyền public ở mức object để website có thể phục vụ nội dung. <br> &emsp; + Test truy cập website tĩnh trên trình duyệt để kiểm tra cấu hình. <br> &emsp; + Tìm hiểu lý thuyết về tăng tốc website tĩnh với CloudFront (chưa triển khai đầy đủ do sức khỏe/thời gian). <br> &emsp; + Thực hành Bucket Versioning, di chuyển object giữa thư mục/bucket và replicate object sang region khác. <br> &emsp; + Ôn lại các bước dọn dẹp tài nguyên và best practices về bảo mật, chi phí với S3. | 26/09/2025 | 26/09/2025 | [Cấu hình Public Object](https://000057.awsstudygroup.com/vi/5-publicobject/) <br><br> [Kiểm tra Website](https://000057.awsstudygroup.com/vi/6-testwebsite/) <br><br> [Tăng tốc với CloudFront](https://000057.awsstudygroup.com/vi/7-cloudfront/) <br><br> [Bucket Versioning](https://000057.awsstudygroup.com/vi/8-versioning/) <br><br> [Di chuyển & sao chép Object](https://000057.awsstudygroup.com/vi/9-moveobject/) <br><br> [S3 Cross-Region Replication](https://000057.awsstudygroup.com/vi/10-s3ccr/) <br><br> [Cleanup & Best Practices](https://000057.awsstudygroup.com/vi/11-cleanup/) |

### Kết quả đạt được trong tuần 3

* Cân bằng khá tốt giữa việc hồi phục sức khỏe và học tập bằng cách bắt đầu học lại từ từ từ thứ Năm trở đi.
* Nắm được nền tảng về Amazon S3, bao gồm bucket, object, static website hosting và cách quản lý quyền truy cập public.
* Triển khai thành công một website tĩnh đơn giản trên S3, kiểm tra truy cập và tìm hiểu thêm về việc tăng tốc với CloudFront.
* Thực hành các tính năng như Bucket Versioning, di chuyển object, replicate cross-region và dọn dẹp tài nguyên theo đúng best practices.
