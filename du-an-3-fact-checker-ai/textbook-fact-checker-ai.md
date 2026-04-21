# Dự án 3: Fact-checker AI — Tin đồn tuyển sinh đúng hay sai?

**Số buổi:** 1 × 90 phút

---

## 1. Giới thiệu dự án

### Bối cảnh thực tế

Mùa tuyển sinh, mạng xã hội tràn ngập thông tin: "Năm nay Bách Khoa tăng điểm chuẩn 3 điểm!", "Trường X bỏ xét tuyển bằng học bạ!", "Ngành Y dược không còn hot nữa!"... Nhưng bao nhiêu trong số đó là thật? Bao nhiêu là tin đồn?

Ở Dự án 1 và 2, em đã xây dựng chatbot có thể trò chuyện và hỏi ngược. Nhưng chatbot đó có một hạn chế lớn: **nó chỉ biết những gì đã được huấn luyện**. Nếu em hỏi "Điểm chuẩn Bách Khoa năm 2025 là bao nhiêu?", AI sẽ không biết — hoặc tệ hơn, nó sẽ **bịa** một con số (hallucination).

Giải pháp? Cho AI một **công cụ tìm kiếm web** — để nó tự search Google khi cần thông tin mới. Đây chính là lúc chatbot đơn giản tiến hoá thành **AI Agent** thực thụ!

### Câu hỏi dẫn dắt

"Trên mạng có nhiều thông tin tuyển sinh — làm sao để AI giúp mình kiểm chứng?"

### Mục tiêu học tập

- [ ] Giải thích được tại sao agent cần tool (LLM không có dữ liệu realtime, có thể hallucinate)
- [ ] Xây dựng flow Agent + Web Search tool trên Langflow để agent tự tìm kiếm thông tin tuyển sinh từ internet
- [ ] Đánh giá được kết quả search của agent (đúng nguồn, đúng thông tin, có dẫn nguồn)
- [ ] Phân biệt được flow thông thường (Input → LLM → Output) và flow Agent (Agent tự quyết định dùng tool)

### Sản phẩm đầu ra

Flow Agent "fact-checker" trên Langflow — học sinh đưa ra các phát biểu về tuyển sinh (đúng/sai), agent tự search web để xác minh và trả lời kèm bằng chứng từ nguồn.

---

## 2. Kiến thức nền tảng

### 2.1 Tại sao LLM cần Tool?

LLM (như Gemma 4 em đã dùng ở Dự án 1-2) rất giỏi ngôn ngữ, nhưng có 3 hạn chế lớn:

**1. Knowledge cutoff (Kiến thức bị đóng băng):**
LLM được huấn luyện đến một thời điểm nhất định. Sau đó, nó không biết gì mới. Ví dụ: nếu Gemma 4 được huấn luyện đến tháng 3/2025, nó sẽ không biết điểm chuẩn tuyển sinh năm 2025 (công bố tháng 8/2025).

**2. Hallucination (Bịa thông tin):**
Khi không biết câu trả lời, LLM có xu hướng "bịa" — tạo ra thông tin nghe có vẻ hợp lý nhưng hoàn toàn sai. Ví dụ: em hỏi "Điểm chuẩn ngành CNTT Bách Khoa 2025?", LLM có thể trả lời "27.5 điểm" — một con số bịa ra.

**3. Không có dữ liệu realtime:**
LLM không kết nối internet. Nó không thể kiểm tra thông tin mới nhất, giá cả, thời tiết, hay tin tức.

**Giải pháp: Cho LLM sử dụng Tool!**

```
TRƯỚC (Dự án 1-2):
Học sinh hỏi → LLM trả lời từ kiến thức cũ → Có thể sai

SAU (Dự án 3):
Học sinh hỏi → Agent suy nghĩ "Mình cần thông tin mới" 
             → Agent dùng Web Search tìm kiếm
             → Agent đọc kết quả search
             → Agent trả lời dựa trên thông tin tìm được + dẫn nguồn
```

> 💡 **Mẹo:** Hãy nghĩ về tool như "tay chân" của AI. LLM là "bộ não" — nó suy nghĩ giỏi nhưng không thể tự tìm kiếm internet. Tool Web Search là "đôi tay" giúp nó lên mạng tìm thông tin.

### 2.2 Tool trong AI Agent

Theo kiến thức về AI Agent, **Tool** là một trong 4 thành phần chính (Persona, Memory, Tools, Model). Tool là các hàm hoặc tài nguyên bên ngoài mà agent có thể sử dụng để tương tác với môi trường.

**Vai trò của Tool:**
- Mở rộng khả năng của agent vượt ra ngoài kiến thức có sẵn
- Cho phép agent truy cập thông tin realtime
- Cho phép agent thao tác dữ liệu (lưu, đọc, cập nhật)
- Cho phép agent điều khiển hệ thống bên ngoài

**Các tool em sẽ dùng trong khoá học:**

| Tool | Dự án | Chức năng |
|------|-------|-----------|
| **Web Search** | Dự án 3 (hôm nay) | Tìm kiếm thông tin trên internet |
| **AstraDB** | Dự án 4-5 | Lưu trữ và truy xuất dữ liệu từ vector database |
| **Notion MCP** | Dự án 6-7 | Đọc/ghi dữ liệu có cấu trúc trên Notion |

**Cập nhật bảng 4 thành phần AI Agent:**

| Thành phần | Dự án 1 | Dự án 2 | Dự án 3 |
|-----------|---------|---------|---------|
| Model | ✅ Gemma 4 | ✅ Gemma 4 | ✅ Gemma 4 |
| Persona | ❌ | ✅ System prompt | ✅ System prompt |
| Memory | ❌ | ❌ | ❌ (sẽ thêm sau) |
| Tools | ❌ | ❌ | ✅ Web Search |

### 2.3 Component Agent trên Langflow — khác gì flow thông thường?

Đây là sự khác biệt quan trọng nhất trong dự án này:

**Flow thông thường (Dự án 1-2):**
```
Chat Input → Prompt → LLM → Chat Output
```
- Dữ liệu chảy theo **một chiều cố định**
- LLM luôn trả lời trực tiếp, không có lựa chọn
- Giống dây chuyền sản xuất: nguyên liệu vào → sản phẩm ra

**Flow Agent (Dự án 3):**
```
Chat Input → Agent (LLM + Tools) → Chat Output
                ↕
           Web Search tool
```
- Agent **tự quyết định** có cần dùng tool hay không
- Nếu câu hỏi đơn giản → Agent trả lời trực tiếp
- Nếu cần thông tin mới → Agent gọi Web Search → đọc kết quả → trả lời
- Giống chuyên gia: nhận câu hỏi → suy nghĩ → quyết định cần tra cứu không → trả lời

**Ví dụ cụ thể:**

```
Câu hỏi 1: "AI là gì?"
→ Agent suy nghĩ: "Mình biết câu này" → Trả lời trực tiếp (KHÔNG search)

Câu hỏi 2: "Điểm chuẩn Bách Khoa 2025 là bao nhiêu?"
→ Agent suy nghĩ: "Đây là thông tin mới, mình cần kiểm tra"
→ Agent gọi Web Search: "điểm chuẩn Bách Khoa 2025"
→ Agent đọc kết quả search
→ Agent trả lời: "Theo [nguồn], điểm chuẩn ngành CNTT Bách Khoa 2025 là..."
```

### 2.4 Component Web Search trên Langflow

**Web Search** là component có sẵn trên Langflow, cho phép agent tìm kiếm thông tin trên internet. Khi agent quyết định cần search, nó sẽ:

1. Tạo câu truy vấn tìm kiếm (search query) phù hợp
2. Gửi truy vấn đến công cụ tìm kiếm
3. Nhận kết quả (tiêu đề, đoạn trích, URL)
4. Phân tích kết quả và tổng hợp câu trả lời

Em không cần cấu hình gì phức tạp — chỉ cần kéo component Web Search vào canvas và kết nối với Agent.

### 2.5 Cách Agent quyết định khi nào dùng Tool — ReAct

Agent sử dụng phương pháp **ReAct** (Reasoning + Acting) để quyết định:

```
1. REASONING (Suy luận): Agent đọc câu hỏi và suy nghĩ
   → "Câu hỏi này cần thông tin mới nhất không?"
   → "Mình có đủ kiến thức để trả lời không?"

2. ACTING (Hành động): Dựa trên suy luận, agent quyết định
   → Nếu đủ kiến thức → Trả lời trực tiếp
   → Nếu cần thông tin mới → Gọi Web Search

3. OBSERVING (Quan sát): Agent đọc kết quả từ tool
   → "Kết quả search có trả lời được câu hỏi không?"
   → "Cần search thêm không?"

4. Lặp lại nếu cần → Cuối cùng tổng hợp và trả lời
```

Đây chính là đặc điểm **Suy luận (Reasoning)** và **Hành động (Acting)** — 2 trong 6 đặc điểm của AI Agent mà em đã học ở Dự án 1.

> ⚠️ **Lưu ý:** Agent không phải lúc nào cũng quyết định đúng. Đôi khi nó search khi không cần, hoặc không search khi cần. Prompt tốt sẽ giúp agent quyết định chính xác hơn.

---

## 3. Hướng dẫn thực hành

### 3.1 Vật tư và công cụ cần chuẩn bị

| STT | Vật tư / Công cụ | Số lượng | Ghi chú |
|-----|-------------------|----------|---------|
| 1 | Máy tính (laptop/PC) có kết nối internet | 1/học sinh hoặc 1/nhóm | Đã cài Langflow Desktop |
| 2 | Langflow Desktop | Đã cài | Phiên bản có component Agent |
| 3 | API key Google AI Studio | Đã tạo | Dùng lại key từ Dự án 1 |
| 4 | Danh sách 5 phát biểu tuyển sinh | Chuẩn bị sẵn | Mix đúng/sai (xem Bước 5) |

### 3.2 Hướng dẫn từng bước

**Bước 1: Tạo flow mới với component Agent**

Lần này em sẽ KHÔNG dùng flow thông thường (Input → LLM → Output). Thay vào đó:

1. Mở Langflow Desktop, tạo **New Flow**
2. Đặt tên: "Fact-checker AI"
3. Ở thanh bên trái, tìm mục **Agents**
4. Kéo component **Agent** vào canvas

Component Agent khác với Google Generative AI ở chỗ: nó có thêm các cổng kết nối cho **Tools** — nơi em sẽ gắn Web Search.

**Bước 2: Kết nối Google Generative AI làm model cho Agent**

1. Kéo component **Google Generative AI** vào canvas
2. Cấu hình API key và chọn model Gemma 4 (như Dự án 1)
3. Kết nối Google Generative AI vào cổng **Model** của Agent

Đây là "bộ não" cho Agent — Agent sẽ dùng Gemma 4 để suy luận và tạo câu trả lời.

**Bước 3: Thêm Web Search tool cho Agent**

1. Ở thanh bên trái, tìm mục **Tools**
2. Kéo component **Web Search** (hoặc **Search API**) vào canvas
3. Kết nối Web Search vào cổng **Tools** của Agent

Giờ Agent đã có "đôi tay" — nó có thể tự search web khi cần!

4. Thêm component **Chat Input** và **Chat Output**
5. Kết nối: Chat Input → Agent → Chat Output

**Flow hoàn chỉnh:**

```
[Chat Input] ──→ [Agent] ──→ [Chat Output]
                    ↑
        [Google Generative AI]  (Model — bộ não)
                    ↑
           [Web Search]  (Tool — đôi tay)
```

**Bước 4: Viết system prompt cho fact-checker**

Trong component Agent, tìm ô **System Prompt** (hoặc **Agent Instructions**) và viết:

```
## Vai trò
Bạn là chuyên gia kiểm chứng thông tin tuyển sinh đại học Việt Nam 
(fact-checker). Nhiệm vụ của bạn là xác minh các phát biểu về tuyển 
sinh là ĐÚNG hay SAI.

## Quy trình làm việc
Khi nhận được một phát biểu cần kiểm chứng:
1. Phân tích phát biểu — xác định thông tin cần kiểm tra
2. Tìm kiếm trên web để tìm nguồn đáng tin cậy
3. Đối chiếu thông tin từ nguồn với phát biểu
4. Đưa ra kết luận: ĐÚNG, SAI, hoặc CHƯA XÁC MINH ĐƯỢC

## Định dạng trả lời
📋 **Phát biểu:** [nhắc lại phát biểu]
🔍 **Kết quả kiểm tra:** ĐÚNG / SAI / CHƯA XÁC MINH ĐƯỢC
📝 **Giải thích:** [giải thích ngắn gọn]
🔗 **Nguồn:** [dẫn link hoặc tên nguồn]

## Ràng buộc
- Luôn tìm kiếm web trước khi trả lời — KHÔNG trả lời từ kiến thức cũ
- Ưu tiên nguồn chính thức: website trường ĐH, Bộ GD&ĐT, báo uy tín
- Nếu không tìm được nguồn đáng tin cậy, trả lời "CHƯA XÁC MINH ĐƯỢC"
- Trả lời bằng tiếng Việt
```

**Bước 5: Chuẩn bị 5 phát biểu tuyển sinh (mix đúng/sai)**

Dưới đây là 5 phát biểu mẫu. Em có thể thay đổi cho phù hợp với thời điểm hiện tại:

```
1. "Đại học Bách Khoa TP.HCM có ngành Khoa học Máy tính."
   (Có thể đúng — cần kiểm tra tên ngành chính xác)

2. "Điểm chuẩn ngành Y đa khoa luôn trên 27 điểm."
   (Cần kiểm tra — phụ thuộc trường và năm)

3. "Đại học FPT không xét tuyển bằng điểm thi THPT."
   (Cần kiểm tra — FPT có nhiều phương thức xét tuyển)

4. "Ngành Trí tuệ Nhân tạo chỉ có ở 2 trường đại học tại Việt Nam."
   (Có thể sai — nhiều trường đã mở ngành AI)

5. "Học bổng toàn phần tại Đại học Vinuni trị giá 100% học phí."
   (Cần kiểm tra thông tin cụ thể)
```

> 💡 **Mẹo:** Em nên tự nghĩ thêm 2-3 phát biểu liên quan đến trường hoặc ngành em quan tâm. Điều này vừa giúp test agent, vừa giúp em tìm hiểu thông tin tuyển sinh thực tế!

**Bước 6: Test agent — agent có tự search không?**

1. Mở Playground
2. Gõ phát biểu đầu tiên: "Đại học Bách Khoa TP.HCM có ngành Khoa học Máy tính."
3. Quan sát:
   - Agent có tự gọi Web Search không? (Nhìn log hoặc indicator trên flow)
   - Kết quả trả lời có dẫn nguồn không?
   - Kết luận ĐÚNG/SAI có chính xác không?
4. Lặp lại với 4 phát biểu còn lại
5. Ghi nhận kết quả vào bảng:

```
| # | Phát biểu | Agent search? | Kết luận | Có nguồn? | Chính xác? |
|---|-----------|---------------|----------|-----------|------------|
| 1 | ...       | Có/Không      | Đúng/Sai | Có/Không  | Có/Không   |
| 2 | ...       | ...           | ...      | ...       | ...        |
| 3 | ...       | ...           | ...      | ...       | ...        |
| 4 | ...       | ...           | ...      | ...       | ...        |
| 5 | ...       | ...           | ...      | ...       | ...        |
```

**Bước 7: Cải tiến prompt để agent dẫn nguồn tốt hơn**

Dựa trên kết quả test, cải tiến prompt. Ví dụ nếu agent không dẫn nguồn đầy đủ, thêm instruction cụ thể hơn:

```
## Ràng buộc bổ sung
- Với MỖI kết luận, PHẢI dẫn ít nhất 1 nguồn cụ thể (tên website + URL)
- Phân biệt rõ: nguồn chính thức (website trường, Bộ GD&ĐT) vs. 
  nguồn không chính thức (diễn đàn, blog cá nhân)
- Nếu thông tin từ nhiều nguồn mâu thuẫn nhau, nêu rõ sự mâu thuẫn
```

Test lại và so sánh kết quả trước/sau cải tiến.

---

## 4. Nâng cao

**Agent đánh giá mức độ tin cậy của nguồn**

Thêm vào system prompt yêu cầu agent phân tích độ tin cậy:

```
## Đánh giá nguồn
Với mỗi nguồn tìm được, đánh giá mức độ tin cậy:
- ⭐⭐⭐ Rất tin cậy: Website chính thức của trường ĐH, Bộ GD&ĐT
- ⭐⭐ Tin cậy: Báo chí uy tín (VnExpress, Tuổi Trẻ, Thanh Niên)
- ⭐ Tham khảo: Diễn đàn, blog, mạng xã hội

Giải thích cho học sinh tại sao nguồn này đáng tin hoặc không đáng tin.
```

Thử test với các phát biểu khó kiểm chứng hơn — ví dụ tin đồn trên mạng xã hội. Quan sát xem agent có phân biệt được nguồn chính thức và không chính thức không.

---

## 5. Câu hỏi ôn tập

1. **Nêu 3 hạn chế của LLM khiến nó cần tool.** Cho ví dụ cụ thể cho mỗi hạn chế.

2. **Flow Agent khác flow thông thường (Input → LLM → Output) ở điểm nào?** Vẽ sơ đồ đơn giản cho cả hai.

3. **ReAct là gì?** Mô tả quá trình agent suy luận và hành động khi nhận câu hỏi "Điểm chuẩn ngành CNTT Bách Khoa 2025 là bao nhiêu?"

4. **Tại sao agent cần dẫn nguồn khi trả lời?** Nếu agent trả lời mà không dẫn nguồn, em có nên tin không? Tại sao?

5. **Đến dự án này, AI Agent của em đã có những thành phần nào trong 4 thành phần (Persona, Memory, Tools, Model)?** Thành phần nào còn thiếu?

---

## 6. Thuật ngữ

| Thuật ngữ | Tiếng Anh | Giải thích |
|-----------|-----------|------------|
| Tool | Tool | Công cụ bên ngoài mà agent sử dụng để mở rộng khả năng (search web, truy cập database...) |
| Agent | Agent | Hệ thống AI có khả năng tự quyết định dùng tool nào, khi nào, để hoàn thành nhiệm vụ |
| Web Search | Web Search | Tool tìm kiếm thông tin trên internet |
| ReAct | Reasoning + Acting | Phương pháp agent suy luận trước, hành động sau, quan sát kết quả, lặp lại nếu cần |
| Knowledge cutoff | Knowledge cutoff | Thời điểm cuối cùng LLM được cập nhật kiến thức |
| Hallucination | Hallucination | Hiện tượng LLM tạo ra thông tin sai nhưng nghe có vẻ hợp lý |
| Fact-check | Fact-check | Kiểm chứng thông tin — xác minh một phát biểu là đúng hay sai dựa trên bằng chứng |
| Nguồn chính thức | Official source | Nguồn thông tin đáng tin cậy: website trường ĐH, cơ quan nhà nước, báo uy tín |
| Search query | Search query | Câu truy vấn tìm kiếm mà agent tạo ra để search web |
| Realtime | Realtime | Thông tin thời gian thực, cập nhật liên tục |
