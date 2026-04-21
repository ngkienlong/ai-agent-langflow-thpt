# Dự án 1: AI đoán nghề nghiệp

**Số buổi:** 1 × 90 phút

---

## 1. Giới thiệu dự án

### Bối cảnh thực tế

Mỗi năm, hàng triệu học sinh THPT đứng trước câu hỏi lớn: "Mình nên chọn nghề gì?". Có bạn thích vẽ nhưng không biết ngành nào liên quan. Có bạn giỏi Toán nhưng phân vân giữa Kỹ sư phần mềm và Tài chính. Trước đây, các em phải tự tìm hiểu hoặc nhờ người lớn tư vấn. Nhưng giờ đây, AI có thể giúp — chỉ cần kể về sở thích, AI sẽ gợi ý những nghề nghiệp phù hợp.

Trong dự án này, em sẽ xây dựng một chatbot AI đầu tiên trên Langflow — một công cụ kéo-thả trực quan. Chatbot này sẽ lắng nghe sở thích của em và gợi ý nghề nghiệp phù hợp. Đây là bước đầu tiên trên hành trình xây dựng một AI Agent tư vấn tuyển sinh hoàn chỉnh!

### Câu hỏi dẫn dắt

"Liệu AI có thể gợi ý nghề nghiệp phù hợp với sở thích của em không?"

### Mục tiêu học tập

- [ ] Giải thích được AI Agent là gì, khác chatbot thông thường và bot ở điểm nào
- [ ] Nhận biết và gọi tên các thành phần chính trên giao diện Langflow Desktop (canvas, component, flow, playground)
- [ ] Tạo API key trên Google AI Studio và kết nối thành công với Langflow qua component Google Generative AI
- [ ] Tạo flow đơn giản Chat Input → Gemma 4 → Chat Output, chạy thử trên Playground và nhận được phản hồi từ LLM

### Sản phẩm đầu ra

Flow chatbot trên Langflow kết nối **Chat Input → Gemma 4 → Chat Output** — học sinh nhập sở thích, AI gợi ý nghề nghiệp phù hợp. Chạy trực tiếp trên Playground.

---

## 2. Kiến thức nền tảng

### 2.1 AI Agent là gì?

**AI Agent** là một hệ thống phần mềm sử dụng trí tuệ nhân tạo để **tự chủ** theo đuổi mục tiêu và hoàn thành nhiệm vụ thay cho người dùng. Khác với chatbot thông thường chỉ trả lời câu hỏi, AI Agent có khả năng **suy luận, lập kế hoạch, ghi nhớ** và **ra quyết định** một cách độc lập.

**6 đặc điểm chính của AI Agent:**

1. **Suy luận (Reasoning):** Phân tích thông tin, nhận diện quy luật, đưa ra kết luận dựa trên bằng chứng. Ví dụ: Agent phân tích sở thích "thích vẽ, thích kể chuyện" → suy luận ra em phù hợp với ngành Thiết kế đồ hoạ hoặc Truyền thông.

2. **Hành động (Acting):** Thực hiện hành động cụ thể — gửi tin nhắn, tìm kiếm web, cập nhật dữ liệu. Ví dụ: Agent tự search Google để tìm điểm chuẩn mới nhất.

3. **Quan sát (Observing):** Thu thập thông tin từ môi trường để hiểu ngữ cảnh. Ví dụ: Agent đọc câu hỏi của em và hiểu em đang hỏi về ngành CNTT.

4. **Lập kế hoạch (Planning):** Xác định các bước cần làm để đạt mục tiêu. Ví dụ: Agent lên kế hoạch — trước tiên tìm thông tin trường, sau đó so sánh điểm chuẩn, cuối cùng đưa ra gợi ý.

5. **Cộng tác (Collaborating):** Làm việc với con người hoặc agent khác. Ví dụ: Agent hỏi ngược lại em để hiểu rõ hơn trước khi tư vấn.

6. **Tự cải tiến (Self-refining):** Học từ kinh nghiệm, ngày càng trả lời tốt hơn.

**Bảng so sánh AI Agent, AI Assistant và Bot:**

| Tiêu chí | AI Agent | AI Assistant | Bot |
|----------|----------|-------------|-----|
| **Mục đích** | Tự chủ thực hiện nhiệm vụ phức tạp | Hỗ trợ người dùng khi được yêu cầu | Tự động hoá tác vụ đơn giản |
| **Mức độ tự chủ** | Cao — ra quyết định độc lập | Trung bình — cần chỉ đạo từ người dùng | Thấp — tuân theo quy tắc lập trình sẵn |
| **Khả năng học** | Học và thích nghi theo thời gian | Có thể học một phần | Hạn chế hoặc không có |
| **Tương tác** | Chủ động, hướng mục tiêu | Phản hồi yêu cầu | Phản hồi trigger/lệnh |
| **Ví dụ** | Agent tư vấn tuyển sinh (dự án cuối khoá) | Siri, Google Assistant | Chatbot FAQ trên website |

> 💡 **Mẹo:** Cách dễ nhớ — **Bot** như máy bán nước tự động (chỉ làm đúng 1 việc), **Assistant** như thư ký (làm theo yêu cầu), **Agent** như chuyên gia tư vấn (tự tìm hiểu, tự đề xuất, tự hành động).

Trong khoá học này, em sẽ bắt đầu từ một chatbot đơn giản (dự án 1) và dần dần nâng cấp nó thành một AI Agent thực thụ có khả năng tìm kiếm web, lưu trữ dữ liệu, và tư vấn tuyển sinh chuyên nghiệp.

### 2.2 LLM — Large Language Model là gì?

**LLM (Large Language Model)** — Mô hình ngôn ngữ lớn — là "bộ não" của AI Agent. Đây là một chương trình máy tính được huấn luyện trên hàng tỷ đoạn văn bản (sách, báo, website, Wikipedia...) để hiểu và tạo ra ngôn ngữ tự nhiên.

**Cách LLM hoạt động (đơn giản hoá):**

1. **Huấn luyện:** LLM đọc hàng tỷ đoạn văn bản và học cách dự đoán từ tiếp theo. Ví dụ: sau cụm "Hà Nội là thủ đô của...", LLM học được rằng từ tiếp theo có xác suất cao là "Việt Nam".

2. **Suy luận:** Khi em đặt câu hỏi, LLM phân tích câu hỏi và tạo ra câu trả lời bằng cách dự đoán từng từ một, dựa trên những gì đã học.

**Ví dụ trực quan:**

```
Em hỏi: "Mình thích vẽ và thiết kế, nên học ngành gì?"

LLM suy nghĩ (đơn giản hoá):
- "thích vẽ" → liên quan đến nghệ thuật, sáng tạo
- "thiết kế" → liên quan đến thiết kế đồ hoạ, kiến trúc, thời trang
- Kết hợp → gợi ý: Thiết kế đồ hoạ, Kiến trúc, Thiết kế thời trang

LLM trả lời: "Với sở thích vẽ và thiết kế, bạn có thể phù hợp với
ngành Thiết kế đồ hoạ, Kiến trúc, hoặc Thiết kế thời trang..."
```

**Gemma 4** là LLM mà chúng ta sẽ sử dụng trong khoá học. Đây là model của Google, có thể dùng miễn phí qua Google AI Studio.

> ⚠️ **Lưu ý:** LLM rất giỏi ngôn ngữ nhưng có hạn chế: kiến thức bị "đóng băng" tại thời điểm huấn luyện (knowledge cutoff), không biết thông tin mới nhất, và đôi khi "bịa" thông tin (hallucination). Đây là lý do ở các dự án sau, chúng ta sẽ bổ sung thêm tool cho agent!

### 2.3 Langflow Desktop là gì?

**Langflow** là một công cụ xây dựng ứng dụng AI theo kiểu **kéo-thả trực quan** (visual builder). Thay vì viết code phức tạp, em chỉ cần kéo các khối (component) vào canvas và nối chúng lại với nhau.

**4 khái niệm quan trọng trên Langflow:**

| Khái niệm | Giải thích | Ví dụ |
|-----------|------------|-------|
| **Canvas** | Không gian làm việc chính — nơi em kéo thả và kết nối các component | Giống bảng vẽ trong Paint |
| **Component** | Các khối chức năng — mỗi khối làm một việc cụ thể | Chat Input, Google Generative AI, Chat Output |
| **Flow** | Một chuỗi các component được kết nối với nhau tạo thành một quy trình hoàn chỉnh | Input → LLM → Output = 1 flow chatbot |
| **Playground** | Nơi chạy thử flow — em chat trực tiếp với AI tại đây | Giống cửa sổ chat của ChatGPT |

**Tại sao dùng Langflow?**
- Không cần viết code — phù hợp cho người mới bắt đầu
- Nhìn thấy trực quan cách dữ liệu chảy từ component này sang component khác
- Có sẵn nhiều component cho AI (LLM, Agent, Web Search, Database...)
- Chạy trên máy tính cá nhân (Desktop version)

### 2.4 Google AI Studio & API Key

**Google AI Studio** là nền tảng của Google cho phép em sử dụng các model AI (như Gemma 4) thông qua API.

**API Key là gì?**

API Key giống như "chìa khoá" để Langflow giao tiếp với Google AI Studio. Khi em tạo API key, Google cấp cho em một chuỗi ký tự bí mật (ví dụ: `AIzaSyB...xyz`). Langflow dùng key này để gửi câu hỏi đến Gemma 4 và nhận câu trả lời.

```
Langflow gửi: "Mình thích vẽ, nên học ngành gì?"
    ↓ (qua API key)
Google AI Studio → Gemma 4 xử lý
    ↓ (qua API key)
Langflow nhận: "Bạn có thể phù hợp với ngành Thiết kế đồ hoạ..."
```

**Gemma 4** là model AI của Google, có free tier (miễn phí trong giới hạn sử dụng nhất định) — hoàn toàn đủ cho khoá học này.

> ⚠️ **Lưu ý:** API key là thông tin bí mật. Không chia sẻ API key với người khác và không đăng lên mạng xã hội. Nếu bị lộ, người khác có thể dùng key của em và hết quota miễn phí.

---

## 3. Hướng dẫn thực hành

### 3.1 Vật tư và công cụ cần chuẩn bị

| STT | Vật tư / Công cụ | Số lượng | Ghi chú |
|-----|-------------------|----------|---------|
| 1 | Máy tính (laptop/PC) có kết nối internet | 1/học sinh hoặc 1/nhóm | Windows, macOS hoặc Linux |
| 2 | Langflow Desktop | Cài sẵn | Tải tại [langflow.org](https://langflow.org) |
| 3 | Tài khoản Google | 1/học sinh | Để tạo API key trên Google AI Studio |
| 4 | Trình duyệt web (Chrome/Firefox) | 1 | Để truy cập Google AI Studio |

### 3.2 Hướng dẫn từng bước

**Bước 1: Cài đặt Langflow Desktop**

1. Mở trình duyệt, truy cập [https://langflow.org](https://langflow.org)
2. Nhấn nút **Download** và chọn phiên bản phù hợp với hệ điều hành (Windows / macOS / Linux)
3. Chạy file cài đặt vừa tải về, làm theo hướng dẫn trên màn hình
4. Sau khi cài xong, mở Langflow Desktop — giao diện chính sẽ hiện ra với canvas trống

> 💡 **Mẹo:** Nếu Langflow mở chậm lần đầu, hãy kiên nhẫn chờ 1-2 phút. Lần sau sẽ nhanh hơn.

**Bước 2: Tạo API key trên Google AI Studio**

1. Mở trình duyệt, truy cập [https://aistudio.google.com](https://aistudio.google.com)
2. Đăng nhập bằng tài khoản Google
3. Ở menu bên trái, tìm và nhấn **Get API key** (hoặc **API keys**)
4. Nhấn **Create API key** → chọn project (hoặc tạo project mới)
5. Google sẽ tạo ra một chuỗi ký tự dài — đây là API key của em
6. **Sao chép API key** và lưu vào một nơi an toàn (file text trên máy, KHÔNG đăng lên mạng)

> ⚠️ **Lưu ý:** Mỗi tài khoản Google có giới hạn số lần gọi API miễn phí mỗi ngày. Trong khoá học, lượng sử dụng hoàn toàn nằm trong giới hạn free tier.

**Bước 3: Tạo flow mới trên Langflow**

1. Mở Langflow Desktop
2. Nhấn **New Flow** (hoặc biểu tượng **+**) để tạo flow mới
3. Chọn **Blank Flow** (flow trống) để bắt đầu từ đầu
4. Đặt tên flow: "AI đoán nghề nghiệp"
5. Canvas trống hiện ra — đây là nơi em sẽ kéo thả các component

**Bước 4: Thêm component Chat Input**

1. Ở thanh bên trái, tìm mục **Inputs**
2. Kéo component **Chat Input** vào canvas
3. Component này đóng vai trò "tai nghe" — nhận tin nhắn từ em

Chat Input là nơi em gõ câu hỏi. Khi em gõ "Mình thích vẽ, nên học ngành gì?", Chat Input sẽ chuyển câu hỏi này sang component tiếp theo.

**Bước 5: Thêm component Google Generative AI**

1. Ở thanh bên trái, tìm mục **Models**
2. Kéo component **Google Generative AI** vào canvas (đặt bên phải Chat Input)
3. Cấu hình component:
   - **Google API Key:** Dán API key đã tạo ở Bước 2
   - **Model Name:** Chọn **gemma-4** (hoặc model Gemma mới nhất có sẵn)
4. Component này đóng vai trò "bộ não" — nhận câu hỏi, suy nghĩ, và tạo câu trả lời

> 💡 **Mẹo:** Nếu không thấy Gemma 4 trong danh sách model, hãy thử gõ "gemma" vào ô tìm kiếm. Nếu vẫn không có, chọn model Gemini mới nhất.

**Bước 6: Thêm component Chat Output**

1. Ở thanh bên trái, tìm mục **Outputs**
2. Kéo component **Chat Output** vào canvas (đặt bên phải Google Generative AI)
3. Component này đóng vai trò "loa" — hiển thị câu trả lời của AI cho em

**Bước 7: Kết nối các component**

Bây giờ em cần nối 3 component lại với nhau để dữ liệu chảy từ Input → LLM → Output:

1. **Nối Chat Input → Google Generative AI:**
   - Nhấn vào điểm nối (output) bên phải của Chat Input
   - Kéo đường nối đến điểm nối (input) bên trái của Google Generative AI
   - Nối cổng **Message** của Chat Input vào cổng **Input** của Google Generative AI

2. **Nối Google Generative AI → Chat Output:**
   - Nhấn vào điểm nối (output) bên phải của Google Generative AI
   - Kéo đường nối đến điểm nối (input) bên trái của Chat Output
   - Nối cổng **Text** của Google Generative AI vào cổng **Message** của Chat Output

Flow hoàn chỉnh sẽ trông như thế này:

```
[Chat Input] ──Message──→ [Google Generative AI] ──Text──→ [Chat Output]
     (tai)                      (bộ não)                      (loa)
```

**Bước 8: Chạy thử trên Playground**

1. Nhấn nút **Playground** (biểu tượng chat ở góc phải) để mở cửa sổ chat
2. Gõ thử: "Mình thích vẽ và thiết kế, nên học ngành gì?"
3. Chờ vài giây — AI sẽ trả lời gợi ý nghề nghiệp!
4. Thử thêm các câu hỏi khác:
   - "Mình giỏi Toán và thích công nghệ"
   - "Mình thích chăm sóc người khác và muốn giúp đỡ mọi người"
   - "Mình thích viết lách và kể chuyện"
5. Quan sát: AI trả lời có hợp lý không? Có gợi ý được nghề nghiệp phù hợp không?

> 💡 **Mẹo:** Thử hỏi AI bằng nhiều cách khác nhau cho cùng một sở thích. Ví dụ: "Mình thích vẽ" vs. "Mình đam mê hội hoạ và nghệ thuật thị giác". Quan sát xem câu trả lời có khác nhau không — đây là bước đầu để hiểu tầm quan trọng của cách đặt câu hỏi (prompt), sẽ học kỹ hơn ở Dự án 2.

---

## 4. Nâng cao

**Thêm component Memory để chatbot nhớ ngữ cảnh**

Hiện tại, chatbot của em "quên" ngay sau mỗi câu hỏi. Nếu em hỏi "Mình thích vẽ" rồi hỏi tiếp "Trường nào đào tạo ngành đó?", AI sẽ không biết "ngành đó" là ngành gì vì nó không nhớ câu trước.

Để khắc phục, em có thể thêm component **Memory**:

1. Ở thanh bên trái, tìm mục **Memories** (hoặc **Helpers**)
2. Kéo component **Memory** vào canvas
3. Kết nối Memory với Google Generative AI (nối vào cổng Memory/Context)
4. Chạy lại Playground — giờ AI sẽ nhớ ngữ cảnh cuộc hội thoại!

Thử test:
- Câu 1: "Mình thích vẽ và thiết kế"
- Câu 2: "Trường nào ở TP.HCM đào tạo ngành đó?"
- Nếu AI hiểu "ngành đó" = Thiết kế đồ hoạ → Memory hoạt động!

Memory chính là một trong 4 thành phần quan trọng của AI Agent (Persona, Memory, Tools, Model). Em vừa thêm "bộ nhớ" cho chatbot — giúp nó tiến gần hơn đến một AI Agent thực thụ!

---

## 5. Câu hỏi ôn tập

1. **AI Agent khác chatbot thông thường (bot) ở những điểm nào?** Hãy nêu ít nhất 3 điểm khác biệt.

2. **LLM là gì?** Giải thích bằng ngôn ngữ đơn giản và cho một ví dụ về cách LLM tạo ra câu trả lời.

3. **Trong flow em vừa xây dựng, 3 component (Chat Input, Google Generative AI, Chat Output) đóng vai trò gì?** Nếu thiếu một trong ba, chuyện gì sẽ xảy ra?

4. **Tại sao cần API key?** Điều gì xảy ra nếu em chia sẻ API key cho người khác?

5. **Hạn chế của chatbot hiện tại là gì?** Em nghĩ cần thêm gì để chatbot trở thành một AI Agent tư vấn tuyển sinh thực sự? (Gợi ý: nghĩ về 4 thành phần của AI Agent — Persona, Memory, Tools, Model)

---

## 6. Thuật ngữ

| Thuật ngữ | Tiếng Anh | Giải thích |
|-----------|-----------|------------|
| AI Agent | AI Agent | Hệ thống AI tự chủ, có khả năng suy luận, lập kế hoạch, hành động và học hỏi để hoàn thành nhiệm vụ |
| LLM | Large Language Model | Mô hình ngôn ngữ lớn — "bộ não" của AI, được huấn luyện trên hàng tỷ đoạn văn bản |
| Langflow | Langflow | Công cụ xây dựng ứng dụng AI bằng kéo-thả trực quan |
| Canvas | Canvas | Không gian làm việc chính trên Langflow, nơi kéo thả component |
| Component | Component | Khối chức năng trên Langflow, mỗi khối làm một việc cụ thể |
| Flow | Flow | Chuỗi các component được kết nối tạo thành quy trình hoàn chỉnh |
| Playground | Playground | Nơi chạy thử flow, chat trực tiếp với AI |
| API Key | API Key | "Chìa khoá" để phần mềm giao tiếp với dịch vụ AI bên ngoài |
| Google AI Studio | Google AI Studio | Nền tảng của Google để sử dụng các model AI qua API |
| Gemma 4 | Gemma 4 | Model AI của Google, có free tier, dùng làm LLM trong khoá học |
| Hallucination | Hallucination | Hiện tượng LLM "bịa" thông tin không chính xác |
| Knowledge cutoff | Knowledge cutoff | Thời điểm cuối cùng LLM được cập nhật kiến thức — sau đó LLM không biết thông tin mới |
