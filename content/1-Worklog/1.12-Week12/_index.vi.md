---
title: "Nhật ký tuần 12"
date: "2025-11-24"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu tuần 12

* Tối ưu truy vấn Athena và hiệu năng dashboard.
* Cải thiện UI/UX và quy trình deploy cho dashboard.
* Bắt đầu định nghĩa hạ tầng dashboard bằng AWS CDK.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | Tham gia GitHub organization của team thay vì tạo org mới, và sắp xếp lại các repository cho dashboard và backend. <br> Dọn lại cấu trúc repo và phân quyền để dễ phối hợp làm việc về sau. | 24/11/2025 | 24/11/2025 | – |
| Ba | Áp dụng partition projection cho bảng Athena để tối ưu truy vấn: <br> &emsp; + Xem lại các partition hiện có và định nghĩa bảng dùng cho dữ liệu cost/performance. <br> &emsp; + Cấu hình các thuộc tính partition projection trên bảng Athena để giảm số lần truy vấn metadata. <br> &emsp; + Test truy vấn trước và sau khi bật partition projection để kiểm tra tốc độ planning và lượng dữ liệu quét được cải thiện. | 25/11/2025 | 25/11/2025 | [Athena partition projection](https://docs.aws.amazon.com/athena/latest/ug/partition-projection.html) |
| Tư | Cải thiện UI cho dashboard: <br> &emsp; + Tinh chỉnh layout cho các panel chính (threats, trạng thái IR workflow, cost metrics). <br> &emsp; + Chỉnh lại màu sắc, font chữ, khoảng cách để giao diện dễ nhìn và dễ đọc hơn. <br> &emsp; + Mài giũa lại các biểu đồ/thẻ thông tin để dữ liệu dễ scan hơn trong lúc demo. | 26/11/2025 | 26/11/2025 | – |
| Năm | Chuyển repo dashboard từ GitHub sang GitLab và thiết lập GitLab CI: <br> &emsp; + Import project React dashboard hiện có vào GitLab. <br> &emsp; + Tạo file `.gitlab-ci.yml` để build dự án và deploy file tĩnh lên S3. <br> &emsp; + Thêm bước invalidate CloudFront sau mỗi lần deploy thành công. | 27/11/2025 | 27/11/2025 | [GitLab CI/CD deploy lên S3 & CloudFront](https://about.gitlab.com/blog/how-to-deploy-react-to-amazon-s3/) |
| Sáu | Bắt đầu viết AWS CDK cho dashboard stack: <br> &emsp; + Tạo app và stack CDK mới dành riêng cho dashboard. <br> &emsp; + Khai báo các resource S3 để host dashboard tĩnh. <br> &emsp; + Thêm các IAM policy cơ bản phục vụ việc deploy và truy cập bucket (sau này sẽ mở rộng). | 28/11/2025 | 30/11/2025 | [AWS CDK Developer Guide](https://docs.aws.amazon.com/cdk/v2/guide/home.html) |

### Kết quả đạt được trong tuần 12

* Tổ chức lại cách làm việc với mã nguồn bằng cách tham gia GitHub org sẵn có và sắp xếp lại repo cho project.
* Cải thiện hiệu năng Athena bằng partition projection, giúp giảm overhead phần planning và lượng dữ liệu bị scan.
* Nâng cấp trải nghiệm dashboard (UI/UX) và chuyển pipeline deploy từ GitHub sang GitLab CI với S3 + CloudFront.
* Khởi tạo stack CDK cho dashboard, bắt đầu từ S3 và IAM để từng bước chuyển sang hạ tầng-as-code hoàn chỉnh.
