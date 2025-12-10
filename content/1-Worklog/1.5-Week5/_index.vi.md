---
title: "Nhật ký tuần 5"
date: "2025-10-06"
weight: 05
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5

* Tiếp tục hoàn thiện và lên kế hoạch chi tiết cho workshop proposal.
* Khám phá tự động hóa workflow với n8n và thử xây bot Messenger cơ bản.
* Học về Auto Scaling và giám sát hệ thống (CloudWatch) cho workload trên AWS.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | Tìm hiểu n8n như một công cụ tự động hóa workflow (self-hosted, tương tự Zapier). <br> Học các khái niệm chính: workflow, trigger, node, credential, execution. <br> Triển khai n8n cục bộ bằng Docker (docker-compose cơ bản) và tạo vài workflow test đơn giản (HTTP trigger, timer, xử lý dữ liệu cơ bản). | 06/10/2025 | 06/10/2025 | [Tài liệu n8n](https://docs.n8n.io/) |
| Ba | Làm theo video hướng dẫn để xây một bot Messenger dùng n8n và Facebook: <br> &emsp; + Tạo Facebook App và Fanpage cho bot. <br> &emsp; + Cấu hình webhook và token để Messenger gửi tin nhắn vào n8n. <br> &emsp; + Xây flow trả lời đơn giản trong n8n để xử lý tin nhắn đến và phản hồi lại. <br> &emsp; + Test đầy đủ luồng bot từ Messenger trên điện thoại/máy tính. | 07/10/2025 | 07/10/2025 | [Video hướng dẫn bot Messenger](https://youtu.be/ieFnSN2Gvs4) |
| Tư | Dịch 3 bài blog được giao phục vụ tài liệu và nội dung workshop, tập trung giữ đúng thuật ngữ kỹ thuật và văn phong. | 08/10/2025 | 08/10/2025 | [Blog1](https://docs.google.com/document/d/1ge1HDyGWudRqSg6LUp2y-jgBTH0y7eFBcZLS2q93OK8/edit?usp=sharing) <br> [Blog2](https://docs.google.com/document/d/11Nvdi9t93mp6l4Trngs8DZFN1Cj6UJLeBS4FChl-Hic/edit?usp=sharing) <br> [Blog3](https://docs.google.com/document/d/1CmH2ggwue55AxPlKajLlMa3d5sBV_1vgqRxyIRLuD_E/edit?usp=sharing) |
| Năm | Tham gia workshop “FCJ Management with Auto Scaling Group”: <br> &emsp; + Ôn lại kiến trúc tổng thể và các yêu cầu trước khi deploy ứng dụng FCJ Management. <br> &emsp; + Học cách tạo Launch Template dựa trên AMI sẵn có của FCJ Management. <br> &emsp; + Tìm hiểu cấu hình Elastic Load Balancer để phân phối traffic cho các instance ứng dụng. <br> &emsp; + Học các bước tạo và cấu hình Auto Scaling Group, thiết lập scaling policy và cách kiểm thử scale/failover. | 09/10/2025 | 09/10/2025 | [FCJ Management với Auto Scaling Group](https://000006.awsstudygroup.com/vi/) |
| Sáu | Tham gia AWS CloudWatch Workshop: <br> &emsp; + Tìm hiểu mục đích chính của Amazon CloudWatch trong việc giám sát metrics, logs và events trên tài nguyên và ứng dụng AWS. <br> &emsp; + Xem lại các bước chuẩn bị để bật CloudWatch trong account. <br> &emsp; + Học CloudWatch Metrics hoạt động như thế nào, các loại metric mặc định và custom có thể thu thập. <br> &emsp; + Tìm hiểu CloudWatch Logs và cách ứng dụng gửi log tập trung để phân tích. <br> &emsp; + Học cách tạo CloudWatch Alarm và Dashboard, cũng như các bước dọn lab sau khi hoàn thành. | 10/10/2025 | 10/10/2025 | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com/vi/) |

### Kết quả đạt được trong tuần 5

* Nắm được các khái niệm cơ bản của n8n, triển khai được bằng Docker và kết nối thành công với Facebook Messenger để tạo một bot đơn giản.
* Dịch xong 3 bài blog kỹ thuật, vừa củng cố kiến thức nội dung, vừa cải thiện kỹ năng viết tài liệu cho workshop.
* Hiểu rõ quy trình deploy ứng dụng FCJ Management với Launch Template, Load Balancer và Auto Scaling Group trên AWS.
* Xây dựng nền tảng vững chắc về Amazon CloudWatch, bao gồm metrics, logs, alarm và dashboard để giám sát hệ thống trên AWS.
