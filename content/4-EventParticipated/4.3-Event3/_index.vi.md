---
title: "Event 3"
date: "2025-11-17"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Báo cáo sự kiện: “AWS Cloud Mastery Series #2 – DevOps on AWS”

### Mục tiêu sự kiện

- Giới thiệu các dịch vụ DevOps cốt lõi trên AWS và các pipeline CI/CD.
- Giải thích khái niệm Infrastructure as Code (IaC) và các công cụ phổ biến.
- Trình bày các lựa chọn container hóa trên AWS.
- Minh họa cách giám sát và quan sát hệ thống (monitoring & observability) bằng dịch vụ AWS.

### Diễn giả

- **Truong Quang Tinh** – AWS Community Builder, Platform Engineer – TymeX  
- **Bao Huynh** – AWS Community Builder  
- **Nguyen Khanh Phuc Thinh** – AWS Community Builder  
- **Tran Dai Vi** – AWS Community Builder  
- **Huynh Hoang Long** – AWS Community Builder  
- **Pham Hoang Quy** – AWS Community Builder  
- **Nghiem Le** – AWS Community Builder  
- **Dinh Le Hoang Anh** – Cloud Engineer Trainee, First Cloud AI Journey  

---

## DevOps Mindset

**– Văn hóa (Culture):**  
Nhấn mạnh hợp tác giữa các team, tự động hóa cao, học tập liên tục và ra quyết định dựa trên số liệu đo được.

**– Vai trò DevOps:**  
Các vai trò thường gặp trong tổ chức DevOps hiện đại gồm: DevOps Engineer, Cloud Engineer, Platform Engineer, Site Reliability Engineer (SRE)… tập trung vào độ tin cậy, tốc độ triển khai và vận hành hiệu quả.

**– Chỉ số thành công:**

- Triển khai ổn định, ít lỗi.  
- Tăng tốc độ deliver, giảm lead time cho mỗi thay đổi.  
- Hệ thống ổn định, chịu lỗi tốt.  
- Trải nghiệm người dùng cuối mượt hơn.  
- Chứng minh được giá trị kinh doanh từ đầu tư công nghệ.

| NÊN (DO)                       | KHÔNG NÊN (DON'T)                 |
| :---------------------------- | :---------------------------------|
| Bắt đầu từ nền tảng cơ bản    | Mắc kẹt mãi trong “tutorial hell” |
| Học bằng cách tự xây project  | Copy-paste mù quáng               |
| Ghi chép/tài liệu hóa đầy đủ  | So sánh bản thân với người khác   |
| Làm chủ từng thứ một          | Bỏ cuộc sau vài lần thất bại      |
| Rèn luyện kỹ năng mềm         |                                   |

**– Continuous Integration (CI):**  
Dev liên tục merge code vào nhánh chung (main), với build + test tự động để mỗi commit luôn ở trạng thái có thể release, tạo tiền đề cho Continuous Delivery/Deployment.

---

## Infrastructure as Code (IaC)

**– Lợi ích:**  
IaC giúp tự động hóa, lặp lại được và dễ scale khi quản lý hạ tầng. Hạ tầng có thể version-control, tạo/xóa môi trường theo nhu cầu, và các thay đổi được review như code thay vì thao tác ClickOps thủ công.

### AWS CloudFormation

Dịch vụ IaC “chính chủ” của AWS, dùng file JSON/YAML để mô tả và tạo toàn bộ stack tài nguyên AWS một cách dự đoán được và dễ audit.

**– Stack:**  
Stack là tập hợp tài nguyên AWS được định nghĩa trong một template. Các thao tác create, update, delete đều thực hiện ở cấp độ stack, giúp quản lý môi trường phức tạp dễ hơn.

**– CloudFormation Template:**  
File khai báo (YAML/JSON) mô tả tài nguyên, cấu hình và quan hệ giữa chúng, đóng vai trò “bản vẽ” có thể tái sử dụng cho các môi trường dev/test/prod.

**– Cách hoạt động (tổng quan):**

1. Viết template mô tả hạ tầng mong muốn.  
2. Lưu local hoặc trên S3, tạo stack từ template đó.  
3. CloudFormation tự tạo/cập nhật/xóa tài nguyên để khớp với template.

**– Drift Detection:**  
Cho phép phát hiện tài nguyên bị chỉnh sửa ngoài IaC. Từ đó team có thể đồng bộ lại template với trạng thái thực tế hoặc rollback những thay đổi trái phép.

### AWS Cloud Development Kit (CDK)

Framework open-source cho phép định nghĩa CloudFormation stack bằng ngôn ngữ lập trình thực (TypeScript/JavaScript, Python, Java, C#/.NET, Go).

**– Construct:**  
Construct là “viên gạch” trong CDK, đại diện cho một hoặc nhiều tài nguyên kèm cấu hình. Có 3 cấp:

- **L1:** Low-level, map 1:1 với resource CloudFormation, kiểm soát chi tiết nhất.  
- **L2:** API cấp cao, đóng gói best practice và default hợp lý.  
- **L3:** Pattern sẵn (opinionated), ghép nhiều resource thành kiến trúc hoàn chỉnh (ví dụ: API Gateway + Lambda + DynamoDB).

### AWS Amplify

Nền tảng tập trung vào build và deploy web/mobile app. Amplify dùng CloudFormation phía sau để dựng backend và hosting, nhưng cung cấp trải nghiệm thân thiện hơn qua CLI và console.

### Terraform

Công cụ IaC đa cloud (AWS, Azure, GCP…) dùng HCL (HashiCorp Configuration Language).

**– Thế mạnh:**

- Hỗ trợ multi-cloud với ngôn ngữ và quy trình thống nhất.  
- Có “state” để nắm rõ chênh lệch giữa trạng thái hiện tại và định nghĩa trong code.

### Cách chọn công cụ IaC?

**– Cần cân nhắc:**

- Hạ tầng xài 1 cloud (AWS-only) hay đa cloud?  
- Vai trò của bạn thiên về dev ứng dụng hay platform/ops?  
- Ecosystem (doc, ví dụ, module chính thức, support) quanh công cụ đó trên cloud bạn dùng.

---

## Dịch vụ Container trên AWS

### Dockerfile

Dockerfile mô tả cách build image: base image, dependency, step build, env, entrypoint… giúp app chạy nhất quán giữa các môi trường.

**– Image:**  
Image là blueprint bất biến, build từ Dockerfile theo kiểu layered. Container chạy từ image đó ở dev/stage/prod.

**– Workflow:**  
Viết Dockerfile → build image → chạy container local hoặc trên AWS → push image lên registry (ECR/Docker Hub).

### Amazon ECR

Registry container private, được quản lý toàn phần trên AWS.

**– Tính năng chính:**

- Quét image tìm lỗ hổng bảo mật.  
- Immutable tag tránh overwrite nhầm.  
- Lifecycle policy dọn image cũ.  
- Mã hóa & tích hợp IAM để kiểm soát truy cập.

### Container Orchestration

Orchestrator quản lý scheduling, scaling, lifecycle container:

- Restart container lỗi.  
- Scale in/out theo traffic/metrics.  
- Cân bằng tải và phân bố workload hiệu quả.

### Kubernetes (K8s)

Nền tảng orchestrator mã nguồn mở, tự động deploy, scale và self-heal ứng dụng container.

**– Thành phần chính:**

- **Control Plane (Master):** quản lý trạng thái cluster, scheduling, API.  
- **Worker Node:** chạy workload trong các pod.  
- **Pod:** đơn vị deploy nhỏ nhất, thường chứa 1–vài container gắn chặt với nhau.  
- **Service:** endpoint ổn định + load balancing cho nhóm pod.

### ECS vs EKS

| Tiêu chí                  | Amazon ECS                                           | Amazon EKS                                                  |
| :------------------------ | :--------------------------------------------------- | :---------------------------------------------------------- |
| **Công nghệ lõi**         | Orchestrator container “nhà trồng” trên AWS         | Kubernetes được AWS quản lý (chuẩn open-source)            |
| **Độ phức tạp**           | Đơn giản, dễ vận hành                                | Linh hoạt hơn nhưng phức tạp hơn                           |
| **Kiến thức yêu cầu**     | Không cần biết Kubernetes                            | Cần nắm khái niệm K8s (pod, deployment, service, …)        |
| **Tích hợp với AWS**      | Tích hợp sâu với dịch vụ AWS                         | Tích hợp theo chuẩn K8s, dễ dùng thêm tool từ ecosystem    |
| **Use case / Lợi ích**    | Deploy nhanh, ít gánh nặng vận hành                  | Phù hợp khi cần tính năng K8s, multi-cluster, portability  |
| **Ecosystem / Community** | Tool & cộng đồng xoay quanh ECS                     | Cộng đồng và hệ sinh thái Kubernetes rất lớn               |
| **Tóm tắt**               | Phù hợp khi muốn nền tảng container AWS-managed     | Phù hợp khi cần full power của Kubernetes                  |

### App Runner

Dịch vụ fully-managed để deploy nhanh web app/API chạy container từ source hoặc registry, phù hợp team nhỏ hoặc workload muốn “ít đụng vào hạ tầng” nhất.

---

## Monitoring & Observability

### CloudWatch

- Dịch vụ trung tâm để giám sát tài nguyên và ứng dụng AWS gần real-time.  
- Cung cấp metrics, logs, alarm, dashboard để cải thiện độ tin cậy và nhìn rõ chi phí.  
- Tích hợp với Auto Scaling, EventBridge… để tự động phản ứng theo sự kiện.

**– CloudWatch Metrics:**  
Lưu lại số liệu hiệu năng/trạng thái từ dịch vụ AWS và workload on-prem (thông qua agent), có thể dùng làm trigger cho alarm, scaling policy, DevOps workflow.

### AWS X-Ray

**– Distributed Tracing:**  
X-Ray trace request đi qua nhiều microservice, vẽ sơ đồ call graph và độ trễ, giúp tìm ra bottleneck hoặc điểm lỗi trong kiến trúc phức tạp.

**– Performance Insight:**  
Hỗ trợ phân tích nguyên nhân gốc rễ của lỗi và vấn đề hiệu năng, cho thấy request tốn thời gian ở đâu, lỗi xảy ra tại service nào.

---

### Trải nghiệm sự kiện

Sự kiện này đặc biệt hữu ích cho project hiện tại vì trùng với định hướng của nhóm: chuyển từ ClickOps sang IaC với AWS CDK, giúp hạ tầng dễ bảo trì, tái sử dụng và làm việc nhóm tốt hơn. Phần CloudWatch và X-Ray cũng hỗ trợ cho yêu cầu monitoring và quan sát dữ liệu mà nhóm đang cần.

Các diễn giả cũng giải đáp một số câu hỏi của team:

- **Hỏi:** Project hiện tại đang dựng bằng ClickOps, muốn chuyển sang CDK thì có tool nào scan hạ tầng hiện tại và tự sinh CDK/CloudFormation không?  
  **Trả lời:** Hiện chưa có công cụ nào làm việc này một cách đáng tin cậy cho toàn bộ môi trường. Cách tốt nhất là dựng lại hạ tầng bằng IaC và dần dần đồng bộ môi trường thực với code.

- **Hỏi:** X-Ray nhìn khá giống CloudTrail, khác nhau chỗ nào?  
  **Trả lời:** X-Ray tập trung vào trace request ứng dụng và call giữa service để phân tích hiệu năng và debug. CloudTrail thì chuyên về audit API call và hoạt động của user/role trong tài khoản AWS.

- **Hỏi:** Project nhóm đang dựng quanh GuardDuty findings, có gợi ý nào để mô phỏng finding ổn định cho demo không?  
  **Trả lời:** Có thể dùng port scanning để tạo finding, hoặc cấu hình thêm custom threat list (IP/domain độc hại) để tạo cảnh báo khi có hoạt động liên quan.

Đây cũng là lần đầu một số diễn giả trình bày các chủ đề này:

- Phần DevOps và IaC được trình bày rõ ràng, mạch lạc và dễ theo dõi.  
- Phần Monitoring & Observability tuy có chút hồi hộp nhưng vẫn mang lại nhiều thông tin thực tế và áp dụng được.

#### Một vài hình ảnh sự kiện

![Group picture during the event taken by speaker Tran Dai Vi](/images/4-Event/CM2GroupPic.jpg)
