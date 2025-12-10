---
title: "Nhật ký tuần 11"
date: "2025-09-09"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11

* Tinh chỉnh và kết nối các thành phần của project (dashboard, API, data layer).
* Chuẩn bị cho giai đoạn hạ tầng-as-code với AWS CDK.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | Tham gia sự kiện AWS Cloud Mastery Series #2 – DevOps on AWS. <br> Học thêm nhiều góc nhìn về cách dùng CloudFormation và CDK để quản lý hạ tầng. <br> Nhận được một số gợi ý từ mentor về chiến lược demo cho project và ghi lại cảm nhận, kinh nghiệm sau sự kiện. | 17/11/2025 | 17/11/2025 | [Event Summary and Experience](../../4-EventParticipated/4.3-Event3) |
| Ba | Thiết lập API Gateway và một Lambda đơn giản để thử gọi API từ dashboard: <br> &emsp; + Tạo REST API trong API Gateway với Lambda proxy integration. <br> &emsp; + Viết Lambda handler cơ bản trả về JSON test. <br> &emsp; + Kết nối React dashboard tới endpoint của API và kiểm tra luồng end-to-end. | 18/11/2025 | 18/11/2025 | [Lambda proxy integration trong API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html) |
| Tư | Cập nhật Lambda để truy vấn dữ liệu bằng Athena: <br> &emsp; + Cài một Lambda có nhiệm vụ gửi câu lệnh query lên Athena và chờ kết quả. <br> &emsp; + Cấu hình vị trí S3 lưu kết quả query của Athena. <br> &emsp; + Kiểm tra để đảm bảo Lambda trả được kết quả về dashboard thông qua API Gateway. | 19/11/2025 | 19/11/2025 | [Query Athena từ Lambda](https://www.youtube.com/watch?v=a_Og1t3ULOI) |
| Năm | Tinh chỉnh lại kiến trúc bên ngoài xung quanh dashboard: <br> &emsp; + Tạm thời loại Route 53 ra khỏi kiến trúc ở giai đoạn này, dùng CloudFront làm entry point chính. <br> &emsp; + Test lại toàn bộ luồng giữa CloudFront, API Gateway và Lambda. <br> &emsp; + Cập nhật sơ đồ kiến trúc và ghi chú để phản ánh đường đi mới, gọn hơn. | 20/11/2025 | 20/11/2025 | – |
| Sáu | Nghiên cứu AWS CDK để chuẩn bị chuyển kiến trúc sang hạ tầng-as-code: <br> &emsp; + Xem lại cách cài đặt và bootstrap dự án AWS CDK. <br> &emsp; + Học các khái niệm cơ bản: App, Stack, Construct và cách khai báo tài nguyên hạ tầng bằng code. <br> &emsp; + Tham khảo ví dụ khai báo API Gateway, Lambda, S3/CloudFront bằng CDK. | 21/11/2025 | 23/11/2025 | [AWS CDK GitHub](https://github.com/aws/aws-cdk) <br><br> [AWS CDK Developer Guide](https://docs.aws.amazon.com/cdk/v2/guide/home.html) |

### Kết quả đạt được trong tuần 11

* Hiểu rõ hơn về CDK và CloudFormation thông qua AWS Cloud Mastery Series #2, đồng thời thu thập được nhiều góp ý hữu ích cho phần demo project.
* Kết nối thành công React dashboard với REST API xây bằng API Gateway và Lambda, kiểm chứng được flow proxy integration và response JSON cơ bản.
* Xây dựng được Lambda truy vấn Athena và trả kết quả về dashboard, giúp mở đường cho các màn hình data-driven.
* Đơn giản hóa và xác nhận kiến trúc bên ngoài bằng việc test luồng CloudFront → API Gateway → Lambda (không dùng Route 53), và sẵn sàng đưa kiến trúc này vào AWS CDK.
