---
title: "Event 2"
date: "2025-11-15"
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Báo cáo sự kiện: “AWS Cloud Mastery Series #1 – AI/ML/GenAI on AWS”

### Mục tiêu sự kiện

- Giới thiệu tổng quan về các khả năng AI/ML/GenAI quan trọng trên AWS.
- Làm rõ các “building block” thực tế như Amazon Bedrock, các dịch vụ AI dựng sẵn và AgentCore cho bài toán thực tế.

### Diễn giả

- **Lam Tuan Kiet** – Sr DevOps Engineer, FPT Software  
- **Danh Hoang Hieu Nghi** – AI Engineer, Renova Cloud  
- **Dinh Le Hoang Anh** – Cloud Engineer Trainee, First Cloud AI Journey  
- **Van Hoang Kha** – Cloud Security Engineer, AWS Community Builder  

### Nội dung chính

#### Generative AI với Amazon Bedrock

**– Foundation Models:**  
Amazon Bedrock cung cấp nhiều Foundation Model (FM) qua một API được quản lý, cho phép chọn model từ nhiều nhà cung cấp và gắn vào các bài toán như chat, search, tóm tắt… mà không cần tự quản hạ tầng model.

**– Prompt Engineering:**  
Phần trình bày đi qua các pattern prompt thực tế để kiểm soát hành vi model:

- **Zero-shot:** hỏi trực tiếp chỉ với yêu cầu, không kèm ví dụ.  
- **Few-shot:** thêm một vài ví dụ mẫu để dẫn model về đúng format và style mong muốn.  
- **Chain-of-thought:** yêu cầu model suy luận từng bước, thể hiện lập luận trung gian trước khi cho đáp án cuối.

**– Retrieval-Augmented Generation (RAG):**  
RAG kết hợp bước tìm kiếm với sinh văn bản để model trả lời dựa trên dữ liệu của riêng bạn, không chỉ dựa trên dữ liệu pretrain.

- **R – Retrieval:** lấy các đoạn nội dung liên quan từ knowledge base hoặc vector store.  
- **A – Augmented:** đính kèm nội dung thu được vào prompt làm ngữ cảnh bổ sung.  
- **G – Generation:** để model sinh câu trả lời dựa trên ngữ cảnh đó.  

Use case phổ biến: chatbot hiểu domain, Q&A trên tài liệu nội bộ, semantic search, tóm tắt nội dung gần real-time.

**– Amazon Titan Embeddings:**  
Titan Embeddings chuyển text thành vector số hóa mang nghĩa ngữ nghĩa, phục vụ similarity search chính xác hơn, RAG, gợi ý và clustering trên hơn 100 ngôn ngữ.

**– Các dịch vụ AI dựng sẵn trên AWS:**  
Sự kiện cũng điểm qua nhóm dịch vụ AI managed, ẩn bớt độ phức tạp ML sau API đơn giản:

- **Amazon Rekognition** – phân tích ảnh/video (label, khuôn mặt, moderation…).  
- **Amazon Translate** – tự động nhận diện ngôn ngữ và dịch.  
- **Amazon Textract** – trích xuất text và layout từ tài liệu.  
- **Amazon Transcribe** – chuyển giọng nói thành text (caption, phân tích cuộc gọi).  
- **Amazon Polly** – chuyển text thành giọng nói tự nhiên.  
- **Amazon Comprehend** – NLP: entity, sentiment, key phrase.  
- **Amazon Kendra** – enterprise search trên nhiều nguồn dữ liệu.  
- **Amazon Lookout services** – phát hiện bất thường trong metric, dữ liệu thiết bị, hình ảnh.  
- **Amazon Personalize** – gợi ý cá nhân hóa.

**– Demo – AMZPhoto:**  
Demo ứng dụng sử dụng Bedrock và nhận diện ảnh để nhận diện khuôn mặt trong ảnh upload, gắn với metadata đã lưu, minh họa pipeline end-to-end từ ingest đến inference.

#### Amazon Bedrock AgentCore

**– Tổng quan:**  
AgentCore là nền tảng quản lý agent AI ở mức production, lo phần runtime, security, observability… để đội ngũ tập trung vào logic của agent.

**– Vấn đề AgentCore giải quyết:**

- Chạy và scale code của agent trên runtime chuyên biệt, có bảo mật.  
- Cung cấp bộ nhớ ngắn hạn và dài hạn để agent “học” từ tương tác cũ.  
- Tích hợp với hệ thống định danh để kiểm soát quyền truy cập công cụ/dữ liệu.  
- Kết nối agent với tool và API (qua Gateway, MCP…) cho các workflow phức tạp.  
- Cung cấp observability và cơ chế đánh giá để theo dõi chất lượng và trace quyết định của agent.

**– Các nhóm dịch vụ lõi:**

- **Foundational services:** Runtime, Identity, Gateway, Policy – phụ trách thực thi an toàn, truy cập tool và guardrail.  
- **Tools & memory layer:** Memory, Browser tool, Code Interpreter và các tích hợp mở rộng khả năng agent.  
- **Secure deployment at scale:** Serverless Runtime và Identity hỗ trợ multi-tenant, kịch bản enterprise.  
- **Operational insights:** Observability và Evaluations cung cấp trace, metrics và kiểm tra chất lượng liên tục.

**– Framework xây agent:**  
AgentCore tương thích với nhiều framework: CrewAI, Google ADK, LangGraph/LangChain, LlamaIndex, OpenAI Agents SDK, Strands Agents SDK… giúp tránh bị lock-in.

### Những điểm rút ra

- **Bedrock là trung tâm GenAI:** Bedrock gom các FM và công cụ vào một chỗ, giúp thử nghiệm và triển khai tính năng GenAI mà không phải tự dựng hạ tầng model.  
- **Tùy biến qua prompt và dữ liệu:** Zero-shot, few-shot, chain-of-thought và RAG cho phép “bẻ lái” model theo domain riêng với ít hoặc không cần fine-tuning.  
- **Embeddings là “viên gạch” nền tảng:** Titan Embeddings và các embedding model khác là lõi cho semantic search và RAG, biến text tự do thành vector dễ index và truy vấn.  
- **Dịch vụ dựng sẵn cho use case phổ biến:** Rekognition, Textract, Comprehend, Transcribe… giúp giải quyết bài toán hình ảnh, tài liệu, text analytics mà không cần tự train model.  
- **AgentCore cho agent production:** AgentCore lấp khoảng trống từ PoC agent lên production bằng cách cung cấp runtime, memory, identity và quan sát vận hành ngay từ đầu.

### Ứng dụng vào công việc

- Lên kế hoạch sử dụng FM trên Bedrock và Titan Embeddings cho các project sắp tới, đặc biệt là search và tính năng kiểu RAG trong tool nội bộ.  
- Đánh giá xem các dịch vụ dựng sẵn (Rekognition, Textract, Comprehend…) có thể thay thế script tự viết ở khâu xử lý dữ liệu nào để giảm công bảo trì.  
- Cân nhắc AgentCore cho các thành phần “agentic” trong kiến trúc tương lai, nhất là những chỗ cần dùng tool an toàn và cần quan sát chi tiết.

### Trải nghiệm sự kiện

- Các diễn giả trình bày rõ ràng, nhiều ví dụ gắn trực tiếp với dịch vụ AWS nên khá dễ liên hệ giữa ý tưởng tổng quan và cách triển khai thực tế.  
- Trong phần Q&A, một bạn trong team nêu bài toán kiến trúc: SNS topic đang nhận burst hơn 1000 GuardDuty alert, có nguy cơ xử lý không kịp. Gợi ý từ diễn giả là chèn thêm SQS queue giữa SNS và consumer để buffer backlog, đảm bảo không mất sự kiện.  
- Kết thúc sự kiện, nhóm lọt top 10 trong bài Kahoot và có dịp chụp hình chung với diễn giả.  
- Tụi mình còn lập “liên minh” nhỏ **“Mèo Cam Đeo Khăn”** gồm thành viên từ “The Ballers” và “Vinhomies”, tạo thêm cảm giác vui và gắn kết cộng đồng.

#### Một vài hình ảnh sự kiện

![Mèo Cam Đeo Khăn group](/images/4-Event/Meocamdeokhan.jpg)
