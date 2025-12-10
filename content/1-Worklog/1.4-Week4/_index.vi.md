---
title: "Nhật ký tuần 4"
date: "2025-09-29"
weight: 04
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4

- Hoàn thành Module 6 (Amazon RDS).
- Bắt đầu phác thảo proposal cho workshop.
- “Đổi gió” với một công nghệ ngoài AWS (Three.js) để tránh bị quá tải.
- Làm quen với Three.js và 3D trên trình duyệt.

### Công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --------- | ------------ | ---------------- | ------------------- |
| Hai | Module 6 – Amazon RDS (Phần 1): <br> &emsp; + Ôn lại các khái niệm cơ bản về cơ sở dữ liệu quan hệ (RDBMS) trong bối cảnh dịch vụ managed trên AWS. <br> &emsp; + Tìm hiểu tổng quan và lợi ích của Amazon RDS so với tự quản lý database trên EC2 (backup, cập nhật bản vá, scale, Multi-AZ, read replica được quản lý). <br> &emsp; + Tìm hiểu các engine được hỗ trợ trên RDS (Aurora, MySQL, MariaDB, PostgreSQL, Oracle, SQL Server). <br> &emsp; + So sánh khi nào nên dùng RDS thay vì DynamoDB, Redshift, Neptune, ElastiCache hoặc S3 cho từng loại workload. | 29/09/2025 | 29/09/2025 | [Amazon RDS – Giới thiệu](https://000005.awsstudygroup.com/vi/1-introduce/) |
| Ba | Module 6 – Amazon RDS (Phần 2): <br> &emsp; + Tìm hiểu các tính năng quản lý của RDS: automated backup, snapshot, maintenance window, cập nhật phần mềm, thông báo sự kiện qua SNS. <br> &emsp; + Học về mã hóa dữ liệu at rest và in transit với KMS và SSL, cách tạo DB instance được mã hóa từ snapshot. <br> &emsp; + Ôn lại thiết kế mạng cho RDS với DB subnet group và VPC (đặt DB trong private subnet, đa AZ). <br> &emsp; + Tìm hiểu các thành phần chính trong chi phí RDS: giờ chạy instance, storage, IOPS, dung lượng backup, Multi-AZ. | 30/09/2025 | 30/09/2025 | [Amazon RDS – Quản lý & Bảo mật](https://000005.awsstudygroup.com/vi/1-introduce/) |
| Tư | Module 6 – Amazon RDS (Phần 3): <br> &emsp; + Học về khía cạnh hiệu năng: các loại storage (gp2, Provisioned IOPS, magnetic) và use case tương ứng. <br> &emsp; + Tìm hiểu cơ chế hoạt động của Multi-AZ, các kịch bản failover thường gặp và cách cập nhật endpoint DNS khi failover. <br> &emsp; + Ôn lại Read Replica để scale đọc, và các mô hình HA/DR giữa AZ/Region. <br> &emsp; + Đọc thêm về snapshot, khôi phục và các lựa chọn migration với DMS và Schema Conversion Tool (SCT). | 01/10/2025 | 01/10/2025 | [Amazon RDS – Hiệu năng, Multi-AZ & Read Replica](https://000005.awsstudygroup.com/vi/1-introduce/) |
| Năm | Học Three.js: <br> &emsp; + Tìm hiểu Three.js là gì và cách nó dựng đồ họa 3D trong trình duyệt dựa trên WebGL. <br> &emsp; + Học các khái niệm cốt lõi: Scene, Camera, Renderer, Mesh, Geometry, Material và ánh sáng cơ bản. <br> &emsp; + Làm theo ví dụ cơ bản để dựng một vật thể 3D quay đơn giản và thử điều khiển camera. <br> &emsp; + Ghi chú lại các ý tưởng dùng Three.js để làm visualization/interactive cho project sau này. | 02/10/2025 | 02/10/2025 | [Three.js Documentation](https://threejs.org/) |
| Sáu | Tham gia sự kiện “AI-Driven Development Life Cycle: Reimagining Software Engineering”. | 03/10/2025 | 04/10/2025 | – |

### Kết quả đạt được trong tuần 4

* Xây dựng được nền tảng lý thuyết vững chắc về Amazon RDS: engine, bảo mật, mạng, Multi-AZ và mô hình Read Replica.
* Hiểu rõ hơn vị trí của RDS so với các dịch vụ dữ liệu khác trên AWS như DynamoDB, Redshift, Neptune, ElastiCache và S3.
* “Đổi gió” khỏi nội dung thuần AWS bằng cách học Three.js, nắm được các khái niệm 3D cơ bản và thử nghiệm scene đơn giản cho ý tưởng project sau này.
* Duy trì nhịp học và mở rộng góc nhìn về AI/software engineering thông qua việc tham dự một sự kiện về AI-Driven Development.
