---
title: "Event 5"
date: "2025-11-29"
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
---

# Báo cáo sự kiện: “AWS Cloud Mastery Series #3 – AWS Well-Architected: Security Pillar Workshop”

### Mục tiêu sự kiện

- Giới thiệu AWS Cloud Club.
- Trụ cột 1: Identity & Access Management (IAM).
- Trụ cột 2: Phát hiện & Giám sát liên tục (Detection & Continuous Monitoring).
- Trụ cột 3: Bảo vệ hạ tầng (Infrastructure Protection).
- Trụ cột 4: Bảo vệ dữ liệu (Data Protection).
- Trụ cột 5: Ứng phó sự cố (Incident Response). [web:143]

### Diễn giả

- **Le Vu Xuan An** – AWS Cloud Club Captain, HCMUTE  
- **Tran Duc Anh** – AWS Cloud Club Captain, SGU  
- **Tran Doan Cong Ly** – AWS Cloud Club Captain, PTIT  
- **Danh Hoang Hieu Nghi** – AWS Cloud Club Captain, HUFLIT  

- **Huynh Hoang Long** – AWS Community Builder  
- **Dinh Le Hoang Anh** – AWS Community Builder  

- **Nguyen Tuan Thinh** – Cloud Engineer Trainee  
- **Nguyen Do Thanh Dat** – Cloud Engineer Trainee  

- **Van Hoang Kha** – Cloud Security Engineer, AWS Community Builder  

- **Thinh Lam** – FCJ Member  
- **Viet Nguyen** – FCJ Member  

- **Mendel Grabski (Long)** – Ex-Head of Security & DevOps, Cloud Security Solution Architect  
- **Tinh Truong** – Platform Engineer, TymeX, AWS Community Builder  

---

### Nội dung chính

## AWS Cloud Club

Phần mở đầu giới thiệu về AWS Cloud Club – cộng đồng do sinh viên dẫn dắt, tập trung vào học và thực hành cloud:

- Giúp sinh viên khám phá và rèn kỹ năng cloud qua sự kiện, lab, workshop.  
- Tạo cơ hội phát triển năng lực lãnh đạo kỹ thuật (Cloud Club Captain).  
- Kết nối với bạn bè, chuyên gia AWS và cộng đồng công nghệ rộng hơn.

Các Cloud Club tham gia FCJA:

- AWS Cloud Club HCMUTE  
- AWS Cloud Club SGU  
- AWS Cloud Club PTIT  
- AWS Cloud Club HUFLIT  

Lợi ích chính: học kỹ năng thực chiến, xây dựng cộng đồng, mở ra cơ hội nghề nghiệp qua networking, mentorship và trải nghiệm thực tế.

---

## Identity & Access Management (IAM)

IAM được trình bày như nền tảng của Security Pillar, kiểm soát “ai được làm gì” trong môi trường AWS. [web:143]

**Nhiệm vụ chính:**

- Quản lý danh tính (User, Group, Role) và quyền truy cập.  
- Thực thi xác thực (authentication) và phân quyền (authorization) trên nhiều account và workload.

**Best practice:**

- Áp dụng nguyên tắc **least privilege** ở mọi nơi. [web:145]  
- Không dùng access key của root lâu dài; xóa sau khi cấu hình ban đầu.  
- Hạn chế wildcard `"*"` trong Action/Resource; ưu tiên policy thu hẹp phạm vi.  
- Dùng **SSO** và AWS Organizations để quản lý truy cập đa account.

**Cơ chế kiểm soát nâng cao:**

- **Service Control Policies (SCP):**  
  - Ấn định “trần” quyền tối đa ở cấp Organization/OU.  
  - Chỉ hạn chế, không tự cấp quyền.

- **Permission boundary:**  
  - Xác định quyền tối đa mà user/role có thể nhận.  
  - Hữu ích khi cho dev tự tạo role nhưng vẫn phải nằm trong guardrail.

**So sánh MFA:**

| TOTP (Time-based OTP)                  | FIDO2                                     |
| :------------------------------------- | :---------------------------------------- |
| Dùng shared secret và mã 6 số         | Dùng mật mã khóa công khai                |
| Nhập mã thủ công                      | Thường chỉ cần chạm hoặc sinh trắc học    |
| Miễn phí (app authenticator)          | Có thể cần token phần cứng                |
| Dễ backup/khôi phục                   | Bảo mật rất cao, gần như không có recovery |

**Rotate credential với Secrets Manager:**

- Dùng Lambda rotation theo 4 bước: `createSecret → setSecret → testSecret → finishSecret`.  
- Lên lịch tự động (ví dụ 7 ngày) và dùng EventBridge để điều phối.  
- Rotate không downtime, vô hiệu an toàn version cũ.

---

## Detection & Continuous Monitoring

Trụ cột thứ hai tập trung vào nhìn rõ hệ thống ở nhiều lớp và tự động hóa phản ứng.

**Multi-layer visibility:**

- **Management events:** API/console event trên toàn account (CloudTrail).  
- **Data events:** Log chi tiết truy cập S3 object, Lambda invoke.  
- **Network activity:** VPC Flow Logs cho traffic pattern, kết nối cho/không cho phép.  
- **Toàn tổ chức:** Gom log đa account/region về một nơi.

**Alert & automation với EventBridge:**

- CloudTrail đẩy event vào EventBridge để xử lý gần real-time.  
- Rule EventBridge route event đến Lambda, SNS, SQS, v.v.  
- Hỗ trợ cross-account, dùng một security account trung tâm.

**Detection-as-Code:**

- Dùng CloudTrail Lake hoặc log query được để định nghĩa rule phát hiện kiểu SQL.  
- Lưu rule trong VCS và deploy bằng CI/CD cho đồng nhất.  
- Dùng IaC để bật logging/detection cho toàn bộ account theo chuẩn chung.

---

## Amazon GuardDuty

GuardDuty được nhấn mạnh là dịch vụ threat detection managed, luôn chạy nền. [web:143]

**Ba nguồn dữ liệu chính:**

| Nguồn log         | Giám sát gì                         | Ví dụ finding                                      |
| :---------------- | :----------------------------------- | :------------------------------------------------ |
| CloudTrail events | Hành vi IAM, API call               | Tắt logging, tạo role quyền cao bất thường        |
| VPC Flow Logs     | Traffic vào/ra tài nguyên           | EC2 gửi dữ liệu ra máy chủ C2 điều khiển          |
| DNS logs          | DNS query trong môi trường          | Malware truy cập domain đáng ngờ/crypto-mining   |

**Gói bảo vệ mở rộng:**

- **S3 Protection:** Phát hiện pattern truy cập object bất thường, scan malware.  
- **EKS Protection:** Dùng audit log K8s để phát hiện thao tác bất thường.  
- **Malware Protection:** Scan EBS volume khi nghi ngờ bị compromise.  
- **RDS Protection:** Theo dõi login pattern, phát hiện brute-force.  
- **Lambda Protection:** Giám sát network Lambda để phát hiện exfiltration/beaconing.  
- **Runtime Monitoring:** Agent trên EC2/EKS/ECS Fargate quan sát process, file, syscall, privilege escalation.

**Chuẩn tuân thủ:**

GuardDuty + Security Hub hỗ trợ:

- AWS Foundational Security Best Practices.  
- CIS AWS Foundations Benchmark.  
- PCI DSS, NIST, và các chuẩn khác thông qua Security Hub.

**Compliance-as-Code:**

- Dùng CloudFormation hoặc IaC khác để bật GuardDuty, Security Hub, cấu hình chung.  
- Security Hub chạy check theo standard trên S3, EC2, RDS, v.v.  
- Gom finding về một central account phục vụ audit và cải tiến liên tục.

---

## Network Security Controls

Phần này gom lại các vector tấn công và control tương ứng:

- **Ingress:** DDoS, SQLi, port scan.  
- **Egress:** Data exfiltration, DNS tunneling.  
- **Lateral:** Di chuyển ngang sau khi một resource bị xâm nhập.

**Thành phần chính:**

- **Security Group (SG):**  
  - Stateful, gắn với ENI/instance.  
  - Chỉ có allow rule, còn lại bị chặn.

- **Network ACL (NACL):**  
  - Stateless, gắn với subnet.  
  - Rule có thứ tự, ALLOW hoặc DENY.  
  - Hữu ích cho lớp chặn rộng.

- **Transit Gateway SG referencing:**  
  - Cho phép tham chiếu SG qua các VPC gắn TGW, đơn giản hóa quản lý rule.  

- **Route 53 Resolver:**  
  - Điều phối DNS cho private hosted zone, DNS nội bộ và internet, hỗ trợ mô hình DNS tập trung.

- **AWS Network Firewall:**  
  - DPI, egress filter, segmentation.  
  - Rule theo domain/protocol, tích hợp threat intel để block tự động.

---

## Data Protection & Governance

**Mã hóa với KMS:**

- Dữ liệu dùng data key, data key được bọc bởi CMK.  
- Policy và condition key của KMS kiểm soát ai/khung cảnh nào được encrypt/decrypt.

**Quản lý chứng chỉ (ACM):**

- Cấp chứng chỉ public miễn phí cho nhiều endpoint tích hợp AWS.  
- Tự động gia hạn, thường 60 ngày trước khi hết hạn.  
- Khuyến nghị DNS validation để tự động và ổn định.

**Quản lý secrets:**

- Secrets Manager giúp loại bỏ secret hard-code trong code/pipeline.  
- Hỗ trợ rotation tự động qua Lambda.

**Bảo mật ở mức dịch vụ:**

- **S3 & DynamoDB:**  
  - Bắt buộc HTTPS, có thể ép `aws:SecureTransport = true` trong policy.  
- **RDS:**  
  - Yêu cầu client trust AWS root CA.  
  - Có thể ép SSL/TLS (ví dụ `rds.force_ssl=1` cho Postgres).

---

## Incident Response & Prevention

**Best practice phòng ngừa:**

- Ưu tiên credential tạm thời (STS, role) thay vì key dài hạn.  
- Không mở S3/public endpoint thừa.  
- Đặt workload nhạy cảm trong private subnet, có entry point kiểm soát.  
- Chuẩn hóa IaC cho mọi môi trường để rebuild/audit dễ hơn.  
- Dùng “double gate” cho thay đổi nhạy cảm: review code + phê duyệt pipeline.

**Vòng đời ứng phó sự cố:**

1. **Chuẩn bị:** Viết runbook, tập dượt, phân vai.  
2. **Phát hiện & phân tích:** Dùng GuardDuty, Security Hub, log để xác định sự cố.  
3. **Cô lập:** Cắt network, rotate credential, giới hạn quyền.  
4. **Xử lý & khôi phục:** Loại bỏ root cause, rebuild hoặc restore từ bản sạch.  
5. **Hậu kiểm:** Họp rút kinh nghiệm, cập nhật runbook, tăng cường kiểm soát.

---

### Trải nghiệm sự kiện

- Workshop này cực kỳ “trúng trọng tâm” với project Automated Incident Response & Forensics của nhóm, nhất là phần GuardDuty, detection pipeline và pattern xử lý sự cố. [web:143]

- **Hỏi:** Project dựa vào GuardDuty để trigger automation, nhưng độ trễ tới ~5 phút từ lúc sự việc xảy ra tới khi có finding. Có cách nào giảm rõ rệt không?  
  **Trả lời:** Độ trễ ~5 phút là bình thường vì GuardDuty cần gom và phân tích nhiều tín hiệu để đưa ra kết luận đáng tin cậy. Để phản ứng sớm hơn, có thể kết hợp thêm công cụ bên thứ ba hoặc xây detection bổ sung dựa trên CloudTrail/log khác, sau đó dùng GuardDuty để enrich khi finding đến. [web:143]

- Sau buổi workshop, anh Mendel Grabski dành thêm thời gian trao đổi về project, gợi ý hướng đi và cách tích hợp, rất hữu ích cho bước tiếp theo.

#### Một vài hình ảnh sự kiện

![All Attendee Picture](/images/4-Event/Event6AttendeePic.jpg)  
_Picture of all attendees_

![Group Picture With Speaker Mendel Grabski and Speaker Van Hoang Kha](/images/4-Event/Event6PicturewithSpeakers.jpg)  
_Group picture with speakers Mendel Grabski and Van Hoang Kha_
