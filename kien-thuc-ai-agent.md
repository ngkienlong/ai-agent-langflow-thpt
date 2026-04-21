# Cơ sở tri thức: AI Agent

> Nguồn tham khảo chính: [What are AI agents? — Google Cloud](https://cloud.google.com/discover/what-are-ai-agents)
> Nội dung được diễn giải lại phù hợp với ngữ cảnh khoá học THPT, không sao chép nguyên văn.

---

## 1. AI Agent là gì?

AI Agent là một hệ thống phần mềm sử dụng trí tuệ nhân tạo để **tự chủ** theo đuổi mục tiêu và hoàn thành nhiệm vụ thay cho người dùng. Agent có khả năng **suy luận, lập kế hoạch, ghi nhớ** và có mức độ tự chủ nhất định để **ra quyết định, học hỏi và thích nghi**.

Khả năng này phần lớn nhờ vào các mô hình nền tảng AI (foundation model) và AI tạo sinh (generative AI) — có thể xử lý đồng thời nhiều loại thông tin: văn bản, giọng nói, hình ảnh, video, code...

---

## 2. Các đặc điểm chính của AI Agent

### 2.1. Suy luận (Reasoning)
Quá trình nhận thức cốt lõi — sử dụng logic và thông tin có sẵn để rút ra kết luận, suy diễn và giải quyết vấn đề. Agent có khả năng suy luận mạnh có thể phân tích dữ liệu, nhận diện pattern, và đưa ra quyết định dựa trên bằng chứng và ngữ cảnh.

### 2.2. Hành động (Acting)
Khả năng thực hiện hành động dựa trên quyết định, kế hoạch hoặc input bên ngoài. Đây là yếu tố then chốt để agent tương tác với môi trường và đạt mục tiêu. Hành động có thể là hành động số (gửi tin nhắn, cập nhật dữ liệu, kích hoạt quy trình khác).

### 2.3. Quan sát (Observing)
Thu thập thông tin về môi trường hoặc tình huống thông qua nhận thức hoặc cảm biến. Đây là yếu tố cần thiết để agent hiểu ngữ cảnh và đưa ra quyết định đúng đắn.

### 2.4. Lập kế hoạch (Planning)
Phát triển kế hoạch chiến lược để đạt mục tiêu. Agent có khả năng lập kế hoạch có thể xác định các bước cần thiết, đánh giá hành động tiềm năng, và chọn phương án tốt nhất. Thường bao gồm dự đoán trạng thái tương lai và xem xét trở ngại tiềm ẩn.

### 2.5. Cộng tác (Collaborating)
Làm việc hiệu quả với người khác — dù là con người hay agent AI khác — để đạt mục tiêu chung. Cộng tác đòi hỏi giao tiếp, phối hợp, và khả năng hiểu quan điểm của người khác.

### 2.6. Tự cải tiến (Self-refining)
Khả năng tự cải thiện và thích nghi. Agent có thể học từ kinh nghiệm, điều chỉnh hành vi dựa trên phản hồi, và liên tục nâng cao hiệu suất theo thời gian.

---

## 3. Phân biệt AI Agent, AI Assistant và Bot

Đây là sự khác biệt quan trọng cần nắm rõ:

### Bảng so sánh

| Tiêu chí | AI Agent | AI Assistant | Bot |
|----------|----------|-------------|-----|
| **Mục đích** | Tự chủ và chủ động thực hiện nhiệm vụ | Hỗ trợ người dùng thực hiện nhiệm vụ | Tự động hoá tác vụ đơn giản hoặc hội thoại |
| **Khả năng** | Thực hiện hành động phức tạp, nhiều bước; học hỏi và thích nghi; ra quyết định độc lập | Phản hồi yêu cầu; cung cấp thông tin; hoàn thành tác vụ đơn giản; gợi ý nhưng người dùng quyết định | Tuân theo quy tắc định sẵn; khả năng học hạn chế; tương tác cơ bản |
| **Tương tác** | Chủ động; hướng mục tiêu | Phản ứng; phản hồi yêu cầu người dùng | Phản ứng; phản hồi trigger hoặc lệnh |

### Ba khác biệt then chốt

1. **Mức độ tự chủ (Autonomy):**
   - **AI Agent:** Tự chủ cao nhất — có thể hoạt động và ra quyết định độc lập để đạt mục tiêu
   - **AI Assistant:** Ít tự chủ hơn — cần input và chỉ đạo từ người dùng
   - **Bot:** Ít tự chủ nhất — thường tuân theo quy tắc lập trình sẵn

2. **Độ phức tạp (Complexity):**
   - **AI Agent:** Xử lý được tác vụ và workflow phức tạp
   - **AI Assistant & Bot:** Phù hợp hơn cho tác vụ và tương tác đơn giản

3. **Khả năng học (Learning):**
   - **AI Agent:** Thường sử dụng machine learning để thích nghi và cải thiện theo thời gian
   - **AI Assistant:** Có thể có một số khả năng học
   - **Bot:** Thường có khả năng học hạn chế hoặc không có

---

## 4. AI Agent hoạt động như thế nào?

Mỗi agent được định nghĩa bởi 4 thành phần chính:

### 4.1. Persona (Nhân cách)
Agent có vai trò, tính cách và phong cách giao tiếp được định nghĩa rõ ràng, bao gồm hướng dẫn cụ thể và mô tả các tool có sẵn. Persona giúp agent duy trì tính nhất quán trong hành vi.

**Ví dụ trong khoá học:** System prompt định nghĩa agent là "chuyên gia tư vấn tuyển sinh ĐH Việt Nam, trả lời bằng tiếng Việt, luôn dẫn nguồn".

### 4.2. Memory (Bộ nhớ)
Agent được trang bị các loại bộ nhớ:
- **Short-term memory (bộ nhớ ngắn hạn):** Cho tương tác hiện tại — nhớ ngữ cảnh cuộc hội thoại đang diễn ra
- **Long-term memory (bộ nhớ dài hạn):** Cho dữ liệu lịch sử và hội thoại trước đó
- **Episodic memory (bộ nhớ sự kiện):** Cho các tương tác trong quá khứ
- **Consensus memory (bộ nhớ đồng thuận):** Cho thông tin chia sẻ giữa các agent

Agent có thể duy trì ngữ cảnh, học từ kinh nghiệm, và cải thiện hiệu suất bằng cách nhớ lại các tương tác trước.

**Ví dụ trong khoá học:** Component Memory trên Langflow giúp chatbot nhớ ngữ cảnh hội thoại.

### 4.3. Tools (Công cụ)
Tool là các hàm hoặc tài nguyên bên ngoài mà agent có thể sử dụng để tương tác với môi trường và mở rộng khả năng. Tool cho phép agent thực hiện tác vụ phức tạp bằng cách truy cập thông tin, thao tác dữ liệu, hoặc điều khiển hệ thống bên ngoài.

**Ví dụ trong khoá học:**
- **Web Search:** Agent tìm kiếm thông tin tuyển sinh trên internet
- **AstraDB:** Agent lưu trữ và truy xuất dữ liệu từ vector database
- **Notion MCP:** Agent đọc/ghi dữ liệu có cấu trúc trên Notion

### 4.4. Model (Mô hình)
Large Language Model (LLM) đóng vai trò nền tảng — là "bộ não" của agent, cho phép agent xử lý và tạo ngôn ngữ, trong khi các thành phần khác hỗ trợ suy luận và hành động.

**Ví dụ trong khoá học:** Gemma 4 (qua Google AI Studio) là LLM nền tảng cho agent.

---

## 5. Phân loại AI Agent

### 5.1. Theo cách tương tác

| Loại | Mô tả | Ví dụ |
|------|--------|-------|
| **Interactive partners (Surface agents)** | Tương tác trực tiếp với người dùng — hỗ trợ tác vụ như dịch vụ khách hàng, giáo dục, y tế. Được kích hoạt bởi câu hỏi/yêu cầu của người dùng. | Agent tư vấn tuyển sinh trong khoá học này |
| **Autonomous background processes (Background agents)** | Hoạt động ngầm — tự động hoá tác vụ thường xuyên, phân tích dữ liệu, tối ưu quy trình. Ít hoặc không tương tác với người. | Agent tự động thu thập điểm chuẩn hàng ngày |

### 5.2. Theo số lượng agent

| Loại | Mô tả |
|------|--------|
| **Single agent** | Hoạt động độc lập để đạt mục tiêu cụ thể. Sử dụng tool và tài nguyên bên ngoài. Phù hợp cho tác vụ rõ ràng không cần cộng tác. Chỉ dùng 1 foundation model. |
| **Multi-agent** | Nhiều agent cộng tác hoặc cạnh tranh để đạt mục tiêu chung hoặc riêng. Tận dụng khả năng đa dạng của từng agent. Mỗi agent có thể dùng foundation model khác nhau. |

**Trong khoá học này:** Học sinh xây dựng **single agent** với Gemma 4 làm foundation model.

---

## 6. Lợi ích của AI Agent

### Hiệu quả và năng suất
- **Tăng output:** Agent chia nhỏ tác vụ như các chuyên gia, hoàn thành nhiều việc hơn
- **Thực thi đồng thời:** Agent có thể làm nhiều việc cùng lúc
- **Tự động hoá:** Agent xử lý tác vụ lặp lại, giải phóng con người cho công việc sáng tạo

### Ra quyết định tốt hơn
- **Cộng tác:** Agent làm việc cùng nhau, thảo luận ý tưởng, học từ nhau
- **Thích nghi:** Agent điều chỉnh kế hoạch khi tình huống thay đổi
- **Suy luận vững chắc:** Qua thảo luận và phản hồi, agent tinh chỉnh suy luận và tránh lỗi

### Khả năng mở rộng
- **Giải quyết vấn đề phức tạp:** Agent kết hợp điểm mạnh để xử lý vấn đề thực tế
- **Giao tiếp ngôn ngữ tự nhiên:** Agent hiểu và sử dụng ngôn ngữ con người
- **Sử dụng tool:** Agent tương tác với thế giới bên ngoài qua tool
- **Học và tự cải tiến:** Agent học từ kinh nghiệm và ngày càng tốt hơn

---

## 7. Thách thức khi sử dụng AI Agent

### 7.1. Thiếu trí tuệ cảm xúc
AI agent có thể gặp khó khăn với cảm xúc con người tinh tế. Các tác vụ như trị liệu, công tác xã hội, hoặc giải quyết xung đột đòi hỏi mức độ thấu hiểu cảm xúc mà AI hiện tại chưa đạt được.

### 7.2. Vấn đề đạo đức
AI agent có thể ra quyết định dựa trên dữ liệu, nhưng thiếu la bàn đạo đức và phán đoán cần thiết cho các tình huống phức tạp về mặt đạo đức (pháp luật, y tế, tư pháp).

### 7.3. Môi trường không thể dự đoán
AI agent có thể gặp khó khăn trong môi trường vật lý biến đổi cao, nơi cần thích nghi thời gian thực và kỹ năng vận động phức tạp.

### 7.4. Tốn tài nguyên
Phát triển và triển khai AI agent phức tạp có thể tốn kém về mặt tính toán, có thể không phù hợp cho dự án nhỏ hoặc tổ chức có ngân sách hạn chế.

**Liên hệ khoá học:** Ở buổi 8, học sinh thảo luận về đạo đức AI — đặc biệt bias trong tư vấn tuyển sinh (agent có thể thiên vị trường/ngành nào đó) và quyền riêng tư dữ liệu.

---

## 8. Ứng dụng thực tế của AI Agent

| Loại agent | Mô tả | Ví dụ |
|-----------|--------|-------|
| **Customer agents** | Trải nghiệm khách hàng cá nhân hoá — hiểu nhu cầu, trả lời câu hỏi, giải quyết vấn đề, gợi ý sản phẩm | Chatbot tư vấn tuyển sinh (dự án cuối khoá) |
| **Employee agents** | Tăng năng suất — tự động hoá quy trình, quản lý tác vụ lặp, trả lời câu hỏi nhân viên | Agent hỗ trợ giáo viên quản lý lớp |
| **Creative agents** | Hỗ trợ thiết kế và sáng tạo — tạo nội dung, hình ảnh, ý tưởng | Agent tạo poster tuyển sinh |
| **Data agents** | Phân tích dữ liệu phức tạp — tìm insight, đảm bảo tính chính xác | Agent phân tích xu hướng điểm chuẩn |
| **Code agents** | Tăng tốc phát triển phần mềm — tạo code, hỗ trợ lập trình | GitHub Copilot, Kiro |
| **Security agents** | Tăng cường bảo mật — phát hiện và phản ứng tấn công | Agent giám sát hệ thống |

---

## 9. Ánh xạ kiến thức vào khoá học

| Khái niệm AI Agent | Buổi áp dụng | Cách học sinh trải nghiệm |
|--------------------|--------------|-----------------------------|
| Agent là gì, khác bot/assistant | Buổi 1 | So sánh chatbot đơn giản (buổi 1) với agent có tool (buổi 3+) |
| Persona (System prompt) | Buổi 2 | Viết system prompt cho agent phỏng vấn hướng nghiệp |
| Tools | Buổi 3, 5, 6 | Web Search (buổi 3), AstraDB (buổi 5), Notion MCP (buổi 6) |
| Memory | Buổi 1-2 | Component Memory trên Langflow |
| Model (LLM) | Buổi 1 | Kết nối Gemma 4 qua Google AI Studio |
| Reasoning + Acting | Buổi 3 | Agent tự quyết định khi nào search web |
| Planning | Buổi 7 | Agent chọn tool phù hợp (RAG trước, web search nếu thiếu) |
| Self-refining | Buổi 8 | Test, debug, cải tiến agent |
| Đạo đức AI | Buổi 8 | Thảo luận bias, quyền riêng tư |
| Single agent | Toàn khoá | Học sinh xây dựng 1 agent với Gemma 4 |
| Interactive partner | Toàn khoá | Agent tư vấn tuyển sinh tương tác trực tiếp với người dùng |
