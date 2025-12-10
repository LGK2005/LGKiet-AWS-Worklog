---
title: "Event 4"
date: "2025-11-19"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Báo cáo sự kiện: “Secure Your Applications: AWS Perimeter Protection Workshop”

### Mục tiêu sự kiện

- Giới thiệu Amazon CloudFront như nền tảng cho bảo vệ vành đai (perimeter protection).
- Giải thích AWS WAF và các mô hình bảo vệ ở tầng ứng dụng.
- Thực hành: Tối ưu một web app bằng CloudFront.
- Thực hành: Bảo vệ web app internet-facing bằng WAF.

### Diễn giả

- **Nguyen Gia Hung** – Head of Solution Architect  
- **Julian Ju** – Senior Edge Services Specialist Solutions Architect  
- **Kevin Lim** – Senior Edge Services Specialist GTM  

---

### Nội dung chính

## Amazon CloudFront

### Nhu cầu bảo mật và vị trí CloudFront

Các nhóm khách hàng khác nhau cần mức độ bảo vệ khác nhau:

- **Chủ website nhỏ:** Cần lớp bảo vệ đơn giản, chi phí thấp.  
- **Doanh nghiệp tầm trung:** Cần che chắn trước DDoS, bot và traffic độc hại.  
- **Doanh nghiệp lớn/đang scale:** Cần cấu hình nâng cao như origin failover, origin offload và tích hợp chặt với WAF, Shield.

CloudFront được trình bày như lớp edge nền tảng cho cả ba nhóm: giá dễ dự đoán, phủ toàn cầu, tích hợp sâu với các dịch vụ bảo mật của AWS.

### CloudFront Flat-Rate Pricing

CloudFront hiện có các gói trả phí trọn gói, gom CDN, WAF, DDoS protection, DNS và storage vào một mức phí/tháng:

- **Gói:**
  - Free: 0 USD/tháng  
  - Pro: 15 USD/tháng  
  - Business: 200 USD/tháng  
  - Premium: 1000 USD/tháng  

- **Ví dụ mức sử dụng:**
  - Free: 1M request + 100 GB data  
  - Pro: 10M request + 50 TB data  
  - Business: 125M request + 50 TB data  
  - Premium: 500M request + 50 TB data  

Khi vượt “soft limit”, CloudFront không tính phí vượt ngay mà có thể giảm hiệu năng và cảnh báo để bạn nâng gói hoặc tối ưu.

### Bảo vệ vành đai với CloudFront

- **Phòng thủ phân tán:** Tấn công được “hấp thụ” tại edge gần attacker thay vì đánh thẳng vào origin.  
- **Tích hợp AWS Shield Advanced:** Có thêm visibility cho DDoS layer hạ tầng và được SRT hỗ trợ 24/7.  
- **Giảm tấn công volumetric:** STN proxy và global routing giúp giảm SYN flood và các tấn công volume lớn ngay trên đường đi.

---

### Tối ưu chi phí với CloudFront

- **Giảm/miễn phí egress:** Data từ nhiều dịch vụ AWS về CloudFront thường rẻ hơn hoặc miễn phí so với egress trực tiếp ra internet.  
- **Nén HTTP:** Nén nội dung có thể giảm kích thước payload rất nhiều, tăng tốc và giảm băng thông.  
- **Tối ưu TLS:**
  - Tự động cấp và gia hạn chứng chỉ (ACM).  
  - HTTPS miễn phí với policy bảo mật sẵn cho cipher/TLS.  
  - Hỗ trợ TLS hiện đại (TLS 1.3, ECDSA, chuẩn “sẵn sàng hậu lượng tử”).  
  - Redirect HTTP → HTTPS ngay tại edge.

- **Mutual TLS (sắp có):** Hỗ trợ xác thực 2 chiều bằng client certificate tại edge.  
- **Origin cloaking:**
  - **Origin Access Control (OAC):** Ký request tới S3, Lambda Function URL… bằng credential ngắn hạn, chặn truy cập trực tiếp.  
  - **Custom origin:** Chỉ cho phép IP CloudFront và yêu cầu secret header.

- **Bảo vệ nội dung:**
  - Signed URL/cookie ngăn hotlink và copy link chia sẻ bừa bãi.

- **Caching & tính sẵn sàng:**
  - Cache bằng TTL để giảm tải origin.  
  - Serve stale content khi origin chậm hoặc tạm thời lỗi.  
  - Origin failover sang origin dự phòng.  
  - Trang lỗi custom và cache error hợp lý.

---

### Tăng hiệu năng với CloudFront

- **Multi-layer caching:** Dùng edge location, Regional Edge Cache, Origin Shield để gom request và tăng cache hit.  
- **Tối ưu kết nối:**
  - Kết nối multiplexed để tải nhiều asset song song.  
  - Kết nối persist tới origin để tránh bắt tay TCP nhiều lần.  
- **Hạ tầng mạng AWS:** Traffic giữa edge và origin đi trên backbone của AWS, giảm độ trễ và tránh nghẽn internet công cộng.  
- **Logic tại edge:**
  - CloudFront Functions, Lambda@Edge cho redirect, rewrite URL, A/B test, routing theo geo/device.  
  - Có thể làm rate limit, mock API, health check, xử lý lỗi ngay gần user.

---

### Use case phổ biến của CloudFront

- **Static website & asset:** Hit cache cao, tăng tốc độ và giảm chi phí origin.  
- **Full website delivery:** Kết hợp performance, security, HA cho cả nội dung tĩnh và động.  
- **API acceleration:** Tái dùng kết nối lâu dài, cache hợp lý để giảm latency.  
- **Streaming media:** Phục vụ số lượng lớn người xem với độ trễ thấp và chi phí tốt.  
- **Large file download:** Tận dụng range request và edge cache cho file lớn.

### Best practice với CloudFront

- Theo dõi end-to-end: user metrics, internet path và backend.  
- Tối đa hóa caching (chuẩn hóa cache key, cache response động khi an toàn, cache error).  
- Dùng WAF chặn traffic xấu, rate limit, lọc API.  
- Đưa logic nhẹ ra edge (CORS, redirect, header/cookie).  
- Kết hợp Route 53 failover với origin group để tăng độ bền.

---

## AWS WAF & Application Protection

### Mối đe dọa và tác động

Nhóm threat chính:

- DDoS và cạn kiệt tài nguyên.  
- Tấn công lỗ hổng ứng dụng (XSS, SQLi, path traversal...).  
- Bot độc hại, traffic lạm dụng.

Hậu quả: rò rỉ dữ liệu, lộ credential, spam/abuse, downtime, tăng chi phí, mất uy tín.

### Bot traffic tăng mạnh

Lượng bot (bao gồm bot dùng AI, scraper…) tăng rõ rệt, khiến việc phát hiện và chặn phức tạp hơn, cần cơ chế tinh vi hơn (behavioral, fingerprint…).

### Route 53 trong bảo vệ vành đai

- Hệ thống DNS phân tán toàn cầu.  
- Sẵn sàng cao, chống DDoS tốt.  
- Thường là điểm vào đầu tiên cùng với CloudFront + WAF.

### Bảo vệ hạ tầng với AWS Shield

**Tại edge:**

- SYN proxy, packet validation.  
- Lọc và “rửa” tấn công volumetric.  
- Điều phối routing tránh path không khỏe.

**Tại “border”:**

- Lọc traffic, phát hiện bất thường ở mức resource.  
- Phát hiện theo health và mitigation nhắm vào resource được bảo vệ.

### Bảo vệ tầng ứng dụng với AWS WAF

- Thường đứng trước CloudFront hoặc ALB.  
- Phát hiện HTTP flood, pattern xấu, IP đáng ngờ.  
- Có thêm Bot Control và Fraud Control add-on.

### Shield Advanced incident response

- Phát hành metrics và alarm để kích hoạt quy trình xử lý sự cố.  
- Truy cập SRT 24/7 để escalation.  
- Xác định vector tấn công, nguồn chính và gợi ý phương án giảm thiểu.

### Khái niệm cấu hình WAF

- **Rule và Rule Group:** kết hợp managed + custom rule.  
- **Managed rules:** bộ rule có sẵn (ví dụ OWASP Top 10).  
- **Custom rules:** tinh chỉnh theo path/header/pattern của ứng dụng.  
- **COUNT trước, BLOCK sau:** ban đầu chỉ đếm để quan sát, tránh false positive, rồi mới chuyển sang chặn.  
- **Label:** gắn nhãn cho request để dùng cho logic phức tạp hơn.  
- **Scope-down:** thu hẹp phạm vi rule để giảm chi phí và tránh match ngoài ý muốn.

### Web ACL / Protection Pack

- Web ACL là cấu hình WAF tổng thể: rule, rule group, default action.  
- Gắn vào CloudFront, ALB, API Gateway.  
- Có logging và sampling để xem log và fine-tune.

### WAF rules và DDoS ở tầng ứng dụng

- Rule đọc IP, header, URI, body rồi quyết định Allow/Block/Count hoặc trả về custom response.  
- Rate-based rule giới hạn request theo IP (hoặc key khác), tự động bóp traffic lạm dụng.  
- Dùng rule để giảm thiểu HTTP flood ở tầng ứng dụng.

### WAF label & loại bot

- **Labels:** do managed/custom rule thêm, đánh dấu geo, IP reputation, bot/fraud category, match…  
- **Common bots:** bot tự khai báo (search engine, social, library), có thể nhận diện qua header, TLS fingerprint, IP reputation.  
- **Evasive bots:** scraper, tool login/brute force, scanner… cố bắt chước người dùng thực; cần client interrogation, phân tích hành vi theo session.

---

## Lab CloudFront: Perimeter Protection

### So sánh S3 static origin vs S3 sau CloudFront

Trong lab, so sánh direct S3 website và S3 sau CloudFront:

- CloudFront phục vụ nội dung cache nhanh hơn nhiều.  
- Edge location + backbone AWS cho latency tốt hơn đường internet công cộng.  
- Nén ở edge giúp giảm kích thước, tăng tốc thêm một bước.

---

## Lab AWS WAF: “Strengthen Your Web Application Defenses”

Trong lab WAF, nhóm cấu hình và kiểm thử rule để chặn:

- XSS.  
- SQL Injection.  
- Path traversal.  
- Mẫu abuse server-side.  
- Bot phổ thông và bot “lẩn tránh”.  
- API misuse.  
- Một bài test “bí ẩn” dùng header mã hóa, đòi hỏi rule custom để chặn.

---

### Trải nghiệm sự kiện

Workshop diễn ra rất đúng thời điểm với nhóm, vì ngay tối hôm trước bọn mình vừa bắt đầu cấu hình CloudFront. Mô hình flat-rate bao gồm WAF đặc biệt phù hợp với bài toán hiện tại vì giúp đơn giản hóa cả ước tính chi phí lẫn cấu hình bảo mật.

Các phần lab khá vui và thực tế, và anh Julian hỗ trợ cực kỳ nhiệt tình, thường xuyên đi vòng để xem và giúp debug khi có lỗi cấu hình. Sau workshop, nhóm cũng tranh thủ hỏi thêm:

- **Hỏi:** WAF có thể chặn bot, nhưng với AI agent kiểu Gemini dùng trình duyệt thật (browser integration) thì sao?  
  **Trả lời:** Dù là Gemini hay agent khác, traffic cuối cùng vẫn là HTTP/API và thường có thể nhận diện/chặn ở tầng request. Một số agent (ví dụ vài công cụ assist dựa trên Claude) giống hệt người dùng thật và không nhất thiết gây hại, nên không phải lúc nào cũng cần chặn gắt.

Sự kiện kết thúc với phần chụp ảnh kỷ niệm và quiz Kahoot; việc nhìn thấy dashboard WAF báo “chặn hết các threat trong lab” tạo cảm giác khá “đã”, giống như thấy cả pipeline bảo vệ hoạt động trơn tru từ đầu đến cuối.

#### Một vài hình ảnh sự kiện

![Attendee](/images/4-Event/Event5AttendeePic.jpg)  
_Hình chụp nhóm tham dự workshop_
