---
title: "Nhật ký tuần 7"
date: "2025-10-20"
weight: 07
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7

* Học cách xây dựng website tĩnh serverless có SSL trên AWS (S3 + CloudFront + ACM + Route 53).
* Xây một front-end đơn giản gọi API Gateway và Lambda trong kiến trúc serverless.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | Workshop “Serverless – SSL S3 Static Website”: <br> &emsp; + Tìm hiểu kiến trúc tổng thể để host website tĩnh trên S3 với HTTPS, kết hợp ACM, Route 53 và CloudFront. <br> &emsp; + Ôn lại các bước chuẩn bị và yêu cầu trước khi làm workshop. <br> &emsp; + Học cách dùng ACM để cấp và quản lý chứng chỉ SSL/TLS cho CloudFront. <br> &emsp; + Ôn lại cách dùng Route 53 (hosted zone, custom domain) để trỏ traffic về CloudFront distribution. | 20/10/2025 | 20/10/2025 | [Serverless – SSL S3 Static Website](https://000082.awsstudygroup.com/vi/) |
| Ba | Serverless – Build Frontend to call API Gateway (Phần 1): <br> &emsp; + Xem kiến trúc tổng thể: front-end tĩnh trên S3/CloudFront gọi API Gateway, Lambda và DynamoDB. <br> &emsp; + Đọc phần giới thiệu và yêu cầu của workshop. <br> &emsp; + Triển khai ứng dụng front-end theo hướng dẫn. <br> &emsp; + Kiểm tra để đảm bảo front-end đã chạy ổn định và sẵn sàng kết nối với API. | 21/10/2025 | 21/10/2025 | [Serverless – Build Frontend to call API Gateway](https://000079.awsstudygroup.com/vi/1-introduce/) |
| Tư | Serverless – Build Frontend to call API Gateway (Phần 2): <br> &emsp; + Ôn lại cách Lambda function và bảng DynamoDB hoạt động phía sau API Gateway. <br> &emsp; + Cấu hình route và integration của API Gateway theo nội dung workshop. <br> &emsp; + Test API bằng Postman, sau đó test từ front-end để xác nhận luồng end-to-end hoạt động. <br> &emsp; + Ôn lại các bước cleanup để xóa API Gateway, Lambda và tài nguyên liên quan sau khi thử nghiệm. | 22/10/2025 | 22/10/2025 | [Serverless – Build Frontend to call API Gateway](https://000079.awsstudygroup.com/vi/) |
| Năm | Họp nhóm: ôn nhanh các dịch vụ AWS và trao đổi về thay đổi trong proposal. <br> Cập nhật kiến trúc AWS, bổ sung AWS Detective. <br> Chỉnh sửa proposal để đưa AWS Detective vào luồng xử lý và thêm plan dùng CDK sau workshop. <br> Cấu hình EventBridge để bắt GuardDuty findings, gửi email qua SNS cho các thành viên và kích hoạt một Lambda đơn giản. <br> Lên ý tưởng cho một trang dashboard đơn giản host trên S3, dùng API Gateway + Lambda + Athena để lấy dữ liệu forensics. | 23/10/2025 | 23/10/2025 | Tài liệu EventBridge, SNS, Lambda, Detective |
| Sáu | Chơi thử AWS Card Clash với các thành viên trong nhóm để ôn lại dịch vụ AWS và cách đặt chúng trong kiến trúc. <br> Ôn kiến thức AWS cho kỳ kiểm tra giữa kỳ bằng cách dùng LLM tạo bộ câu hỏi theo yêu cầu. | 24/10/2025 | 24/10/2025 | [AWS Card Clash](https://aws.amazon.com/training/digital/aws-card-clash/) |

### Kết quả đạt được trong tuần 7

* Học được cách host website tĩnh bảo mật với HTTPS trên S3 bằng cách kết hợp ACM, Route 53 và CloudFront.
* Hiểu rõ hơn về kiến trúc web serverless, nơi front-end tĩnh gọi API Gateway, Lambda và DynamoDB phía sau.
* Tiến thêm một bước với proposal và kiến trúc workshop khi bổ sung AWS Detective và tự động hóa xử lý sự kiện qua EventBridge, SNS và Lambda.
* Củng cố kiến thức dịch vụ AWS thông qua game AWS Card Clash và ôn tập có cấu trúc cho kỳ giữa kỳ.
