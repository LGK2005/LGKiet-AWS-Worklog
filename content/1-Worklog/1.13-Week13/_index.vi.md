---
title: "Nhật ký tuần 13"
date: "2025-12-01"
weight: 13
chapter: false
pre: " <b> 1.13. </b> "
---

### Mục tiêu tuần 13
Hoàn thành project và nộp bài.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | Tiếp tục viết AWS CDK cho backend và lớp edge của dashboard: <br> &emsp; + Mở rộng CDK app hiện có để khai báo các Lambda dùng cho API của dashboard. <br> &emsp; + Thêm cấu hình CloudFront distribution để phục vụ dashboard và điều hướng các đường dẫn API. <br> &emsp; + Định nghĩa tài nguyên API Gateway và integration với Lambda bằng CDK. | 01/12/2025 | 01/12/2025 | [CDK Lambda + API tutorial](https://docs.aws.amazon.com/lambda/latest/dg/lambda-cdk-tutorial.html) |
| Ba | Thêm Amazon Cognito vào kiến trúc để hỗ trợ đăng nhập cho dashboard: <br> &emsp; + Thiết kế vị trí của Cognito user pool giữa front-end và API Gateway. <br> &emsp; + Ôn lại luồng authentication và cách token được dùng để bảo vệ API. <br> &emsp; + Cập nhật sơ đồ kiến trúc để bổ sung Cognito và ghi chú cho phần triển khai sau này. | 02/12/2025 | 02/12/2025 | [Amazon Cognito user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools.html) |
| Tư | Kiểm thử deploy CDK và debug lỗi: <br> &emsp; + Chạy `cdk synth` và `cdk deploy` cho các stack liên quan đến dashboard. <br> &emsp; + Sửa lỗi logic và phân quyền (IAM policy, thiếu biến môi trường, sai reference, v.v.). <br> &emsp; + Deploy lại nhiều lần cho đến khi CloudFront, API Gateway và Lambda hoạt động đúng như mong đợi. | 03/12/2025 | 03/12/2025 | – |
| Năm | Gộp các stack của các thành viên và test deploy tổng thể: <br> &emsp; + Kéo các CDK stack của những thành viên khác trong organization. <br> &emsp; + Tích hợp các stack đó (ETL, IR workflow, thành phần bảo mật) với stack dashboard. <br> &emsp; + Deploy ứng dụng tổng thể và kiểm tra các stack chạy cùng nhau mà không bị xung đột. | 04/12/2025 | 04/12/2025 | – |
| Sáu | Việc gia đình. <br> Không ghi nhận thêm công việc project trong ngày. | 05/12/2025 | 05/12/2025 | – |

### Kết quả đạt được trong tuần 13

* Mở rộng codebase CDK để mô tả đầy đủ các thành phần Lambda, CloudFront và API Gateway phục vụ cho đường đi của dashboard từ đầu đến cuối.
* Thiết kế và ghi lại kiến trúc tích hợp Amazon Cognito để hỗ trợ đăng nhập và bảo vệ truy cập vào dashboard.
* Deploy và debug thành công các stack CDK, sau đó gộp nhiều stack của cả nhóm thành một ứng dụng có thể triển khai đồng bộ.
* Cân đối được thời gian giữa việc hoàn thiện project và xử lý công việc gia đình vào cuối tuần.
