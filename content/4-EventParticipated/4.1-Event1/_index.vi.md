---
title: "Event 1"
date: "2025-10-03"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Báo cáo sự kiện: “AI-Driven Development Life Cycle: Reimagining Software Engineering”

### Mục tiêu sự kiện

- Thảo luận về cách Generative AI đang thay đổi quy trình phát triển phần mềm hiện đại.
- Giới thiệu mô hình AI-Driven Development Life Cycle (AI-DLC) và các ý tưởng chính.
- Demo thực tế Kiro và Amazon Q Developer trong các tình huống phát triển phần mềm.

### Diễn giả

- **Toan Huynh** – Specialist SA, PACE  
- **My Nguyen** – Sr. Prototyping Architect, Amazon Web Services – ASEAN  

### Nội dung chính

- Chủ đề trung tâm của buổi chia sẻ là AI-DLC – một mô hình trong đó AI hỗ trợ điều phối vòng đời phát triển (lên kế hoạch, phân rã công việc, gợi ý kiến trúc), trong khi kỹ sư vẫn là người chịu trách nhiệm cuối cùng cho việc review, ra quyết định và quản trị.

**– AI-DLC – Ý tưởng cốt lõi:**  
AI-DLC đặt con người ở trung tâm: AI đóng vai trò cộng tác viên, giúp tăng năng suất lập trình viên, rút ngắn vòng lặp phát triển từ hàng tuần/tháng xuống còn vài ngày hoặc vài giờ.

**– Quy trình AI-DLC:**  
Luồng làm việc mang tính lặp, xen kẽ giữa bước của AI (đề xuất kế hoạch, triển khai thay đổi, trả lời câu hỏi) và bước của con người (làm rõ yêu cầu, duyệt kế hoạch), đảm bảo AI chỉ thực thi sau khi hướng đi đã được con người xác nhận.

**– Các giai đoạn AI-DLC:**  
Vòng đời được chia thành ba pha – Inception, Construction và Operation – mỗi pha bổ sung thêm ngữ cảnh cho pha tiếp theo:

- **Inception:** Thu thập bối cảnh, làm rõ ý định thông qua User Story, tổ chức các Units of Work.
- **Construction:** Thực hiện Domain Modeling, sinh và kiểm thử code, bổ sung thành phần kiến trúc, deploy bằng IaC và test tự động.
- **Operation:** Chạy hệ thống ở môi trường production, xử lý sự cố và vận hành runtime.

**– Những “nỗi đau” mà AI-DLC cố gắng giải quyết:**

- **Scaling AI development:** Các công cụ code AI hiện tại dễ “đuối” khi hệ thống trở nên phức tạp hơn.
- **Thiếu khả năng kiểm soát:** Khó giám sát và điều phối nhiều AI agent một cách có cấu trúc.
- **Chất lượng code:** Khó giữ chất lượng production khi đi từ PoC lên hệ thống thật nếu thiếu khung làm việc rõ ràng.

## Phần chuyên sâu: Kiro – AI IDE từ prototype đến production

- Kiro là một IDE định hướng AI, được xây dựng quanh ý tưởng AI-DLC và nhấn mạnh cách tiếp cận “spec-driven development” (phát triển dựa trên đặc tả).

**– Spec-driven Development:**  
Từ một prompt cấp cao (ví dụ: “xây một app chat kiểu Slack”), Kiro tạo ra các artifact có cấu trúc như:

- requirements.md (yêu cầu),
- design.md (kiến trúc và thiết kế),
- tasks.md (danh sách công việc).

Cách tiếp cận này chuyển workflow từ kiểu “code theo cảm hứng” sang quy trình có đặc tả cụ thể, dễ truy vết. Developer làm việc với các file spec như nguồn sự thật chính.

**– Agentic Workflows trong Kiro:**  
Các agent của Kiro thực thi dựa trên spec, còn developer vẫn là người điều khiển:

- **Implementation Plan:** Kiro sinh ra một kế hoạch triển khai cụ thể với các task/subtask (ví dụ: “thêm API đăng ký/đăng nhập”, “triển khai middleware JWT”), và map từng task về requirement ban đầu để dễ đối chiếu.
- **Agent Hooks:** Hook kích hoạt agent khi có sự kiện như “save file”, tự động hóa các việc nền như sinh tài liệu, viết unit test, tinh chỉnh hiệu năng.

### Các ý chính rút ra

**– AI cho sản phẩm “gần mức production”:**  
Bằng cách sinh thiết kế chi tiết (data flow, API contract…) và test trước khi viết code, Kiro giúp output do AI sinh ra gần với chất lượng production hơn, thay vì chỉ là prototype dùng tạm.

**– Kiểm soát thông qua artifact:**  
Developer điều khiển hệ thống chủ yếu bằng việc chỉnh sửa và phê duyệt các artifact – yêu cầu, thiết kế, kế hoạch công việc – thay vì ngồi viết từng dòng triển khai, trong khi AI agent chịu trách nhiệm phần thực thi.

### Liên hệ với công việc bản thân

- **Sử dụng Amazon Q Developer / công cụ tương tự:** Có thể áp dụng AI coding assistant vào bài tập và project cá nhân để xử lý những phần lặp lại/boilerplate, tập trung thời gian cho phần ý tưởng và kiến trúc.
- **Tập trung vào phần giá trị cao:** Chuyển các việc lặt vặt cho AI, dành thời gian cho Domain Modeling và Architectural Design – những phần vẫn cần tư duy và quyết định của con người trong pha Construction.  

### Cảm nhận về sự kiện

Buổi **AI-Driven Development Life Cycle: Reimagining Software Engineering** mang lại một bức tranh rất rõ về hướng đi tương lai của ngành phát triển phần mềm. Generative AI được trình bày không chỉ như một công cụ hỗ trợ viết code, mà còn như “động cơ” có thể điều phối một phần lớn vòng đời phát triển. Nội dung đi từ lý thuyết AI-DLC cho đến demo trực tiếp với Amazon Q Developer và Kiro. Phần demo Kiro ấn tượng nhất: từ một prompt ngắn có thể mở rộng thành một kế hoạch phát triển đầy đủ, có thể chạy và kiểm tra được ngay trong IDE.

#### Bài học rút ra

- Những vấn đề về scale, kiểm soát hành vi AI và chất lượng code cho thấy một quy trình có cấu trúc, có con người kiểm chứng như AI-DLC là rất thực tế và cần thiết.

#### Một vài hình ảnh sự kiện

![Group picture during the event](/images/4-Event/TheBois-AIDLC.jpg)
