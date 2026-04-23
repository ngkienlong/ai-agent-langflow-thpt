# Dự án 2: Thư viện tuyển sinh thông minh

**Số buổi:** 3 × 90 phút

---

## 1. Giới thiệu dự án

### Bối cảnh thực tế

Mỗi mùa tuyển sinh, hàng triệu học sinh THPT Việt Nam lên mạng tìm thông tin: "Điểm chuẩn ngành CNTT ĐH Bách Khoa năm nay bao nhiêu?", "ĐH Kinh tế Quốc dân có học bổng gì?", "Kỳ thi đánh giá năng lực ĐHQG mở đăng ký khi nào?". Mỗi câu hỏi đòi hỏi tra cứu nhiều nguồn — website trường, Bộ GD&ĐT, báo chí — rồi đối chiếu, tổng hợp. Mất hàng giờ cho vài câu hỏi đơn giản.

Ở dự án 1, bạn đã xây Agent Chiến lược — một AI biết hỏi và sinh bảng hỏi cá nhân hoá. Nhưng agent đó chỉ "nói chuyện" dựa trên kiến thức sẵn có trong LLM. Nó không biết tin mới, không tra cứu được dữ liệu cụ thể, và nếu hỏi "điểm chuẩn năm 2025", nó sẽ bịa hoặc từ chối.

Trong dự án này, bạn sẽ xây **Agent Thông tin tuyển sinh** — một "thư viện sống" có 3 khả năng:

1. **Tra cứu web** (buổi 3): agent có "mắt" nhìn ra internet, tìm tin tuyển sinh mới nhất kèm dẫn nguồn.
2. **Thư viện riêng** (buổi 4): agent có "trí nhớ dài hạn" — một vector database (AstraDB) chứa sẵn dữ liệu tuyển sinh của nhiều trường, tra cứu theo nghĩa chứ không chỉ khớp từ khoá.
3. **Chiến lược thông minh** (buổi 5): agent biết **ưu tiên thư viện riêng trước**, chỉ chạy ra internet khi thư viện không đủ — và luôn **hỏi xin phép** trước khi search web.

Đây là agent thứ hai trong hệ thống 3 agent tư vấn tuyển sinh mà bạn đang xây dựng xuyên suốt khoá học.

### Câu hỏi dẫn dắt

> "Làm thế nào để Agent Thông tin tuyển sinh trả lời chính xác, cập nhật và **có dẫn nguồn** cho mọi câu hỏi tuyển sinh đại học?"

### Mục tiêu học tập

**Buổi 3 — Web Search: tool thứ hai cho Agent:**

- [ ] Củng cố khái niệm **tool** trong AI Agent: agent dùng Web Search để lấy dữ liệu realtime.
- [ ] Xây dựng flow `Agent + Web Search`: Chat Input → Agent (Language Model + Web Search tool) → Chat Output.
- [ ] Viết system prompt để agent biết khi nào cần search web.
- [ ] Agent trả lời được ít nhất 5 câu hỏi tuyển sinh kèm dẫn nguồn từ web.
- [ ] Đánh giá agent có chọn đúng nguồn uy tín không.

**Buổi 4 — Vector Database & Embedding: ingest dữ liệu vào AstraDB:**

- [ ] Giải thích được **embedding** là gì (biến text thành dãy số đại diện cho nghĩa).
- [ ] Giải thích được tại sao cần **vector database** (lưu dữ liệu lâu dài, tìm kiếm theo nghĩa, giảm hallucination).
- [ ] Tạo database + collection trên AstraDB Serverless có bật vectorize integration.
- [ ] Xây dựng flow ingest thủ công: File → Split Text → Astra DB.
- [ ] Mở rộng sang flow ingest tự động qua Agent.
- [ ] Kiểm tra dữ liệu đã nhúng thành công.

**Buổi 5 — RAG + chiến lược fallback:**

- [ ] Xây dựng flow RAG cơ bản.
- [ ] So sánh chất lượng trả lời có RAG vs. không RAG.
- [ ] Điều chỉnh Number of Search Results (top-k) và quan sát ảnh hưởng.
- [ ] Chuyển flow RAG sang flow Agent: gắn cùng Web Search.
- [ ] Viết system prompt cho chiến lược fallback.
- [ ] Đánh giá agent có biết lúc nào dùng RAG, lúc nào hỏi user, lúc nào search web không.

### Sản phẩm đầu ra

**Cuối buổi 3:**

- Flow Agent + Web Search chạy trên Playground. Agent trả lời được ít nhất 5 câu hỏi tuyển sinh kèm dẫn nguồn.
- Bảng so sánh: 3 câu hỏi được trả lời bằng Agent có Web Search vs. không Web Search.

**Cuối buổi 4:**

- Collection `tuyen_sinh_dai_hoc` trên AstraDB có bật vectorize.
- Flow ingest thủ công đã nạp dữ liệu của 3 trường.
- Flow ingest tự động qua Agent nạp thêm ít nhất 2 trường.
- Tổng: 5+ trường trong AstraDB với metadata đầy đủ.

**Cuối buổi 5:**

- Flow RAG cơ bản — thử nghiệm top-k = 2, 4, 8.
- Flow Agent + 2 tool (Astra DB RAG + Web Search) với chiến lược fallback "ưu tiên RAG → xin phép user search web".
- Bảng test 5 câu pha trộn — đánh giá agent có chọn đúng tool không.

---

## 2. Kiến thức nền tảng

### 2.1 Tool — "cánh tay nối dài" của Agent (ôn tập và mở rộng)

Ở dự án 1, bạn đã biết: **Agent = LLM + tool**. Agent Chiến lược có 2 tool Notion (Create Page, Add Content to Page) — giống như có **tay** để ghi lên sổ.

Hôm nay agent có thêm tool **Web Search** — giống như có **mắt** để nhìn ra internet. Và ở buổi 4-5, agent sẽ có thêm tool **Astra DB** — giống như có **trí nhớ dài hạn** để lưu và tra cứu thư viện riêng.

Mỗi tool trong Langflow đều có 3 thứ quan trọng:

| Thành phần | Vai trò | Ví dụ |
|---|---|---|
| **Tool Name** | Tên ngắn để agent gọi | `web_search_tuyen_sinh` |
| **Tool Description** | Mô tả cho agent biết tool làm gì và khi nào dùng | "Tìm kiếm thông tin tuyển sinh mới trên web. Dùng khi cần tin cập nhật." |
| **Tool Mode** | Toggle bật trong inspection panel để biến component thành tool | Bật = agent tự gọi; Tắt = chạy cố định |

Bạn đã thấy ở dự án 1: Tool Description viết mơ hồ → agent gọi sai tool. Nguyên tắc này vẫn đúng cho mọi tool mới.

### 2.2 Web Search — cho agent "nhìn ra internet"

**Web Search** là component có sẵn trong Langflow (nhóm Data/Core). Khi bật Tool Mode, agent có thể tự gọi nó để tìm kiếm trên web.

Các tham số chính:

| Tham số | Ý nghĩa | Giá trị gợi ý |
|---|---|---|
| **Query** | Từ khoá tìm kiếm (agent tự điền khi chạy) | — |
| **Provider / Source** | Nguồn tìm kiếm | DuckDuckGo, Google News, RSS feed |
| **Max Results** | Số kết quả trả về | 3-5 |

So sánh Web Search với Notion tool đã học:

| Tiêu chí | Notion tool (Create Page) | Web Search tool |
|---|---|---|
| Mục đích | **Ghi** dữ liệu ra ngoài | **Đọc** dữ liệu từ ngoài |
| Input agent cần cung cấp | title, content | query (từ khoá) |
| Output trả về | page_id | Danh sách bài viết + link |
| Cần token/key? | Có (integration token) | Không (cho DuckDuckGo) |

> 💡 **Tại sao Notion cần token mà Web Search không?** Notion là workspace riêng tư — cần xác thực để truy cập. Web Search dùng DuckDuckGo — công cụ tìm kiếm công khai, ai cũng dùng được.

> ⚠️ **Rate limit:** Web Search có giới hạn số lần gọi trong một khoảng thời gian. Nếu gọi liên tục quá nhiều, sẽ bị chặn tạm thời. Giải pháp: đợi 1-2 phút, giảm Max Results, hoặc đổi Provider.

### 2.3 Đánh giá nguồn web — không phải link nào cũng đáng tin

Khi agent search web, nó trả về nhiều link. Nhưng không phải nguồn nào cũng uy tín. Trong lĩnh vực tuyển sinh, thứ tự ưu tiên:

1. **Nguồn chính thức** — website trường (`.edu.vn`), Bộ GD&ĐT (`moet.gov.vn`): đáng tin nhất.
2. **Báo chí chính thống** — VnExpress, Tuổi Trẻ, Thanh Niên: tin cậy, nhưng đôi khi có sai sót nhỏ.
3. **Diễn đàn giáo dục** — Hocmai, Vietjack: hữu ích nhưng cần đối chiếu.
4. **Mạng xã hội** — Facebook, TikTok: ít tin cậy nhất, thông tin hay bị sai lệch.

Tiêu chí đánh giá nhanh:

- Có **ngày đăng** rõ ràng, không quá cũ?
- Có **tác giả / cơ quan** đứng tên?
- Có thể **đối chiếu** với ít nhất 1 nguồn khác?

Trong system prompt, bạn sẽ nhắc agent **ưu tiên nguồn chính thức** và **luôn dẫn link**.

### 2.4 Embedding — biến text thành dãy số "biết nghĩa"

Đây là kiến thức cho **buổi 4**.

Hãy tưởng tượng bạn vẽ 2 trục trên bảng: trục X = "vui ↔ buồn", trục Y = "nhanh ↔ chậm". Bạn đặt các từ lên toạ độ:

- "Cười" → (vui, nhanh) → toạ độ (0.8, 0.7)
- "Khóc" → (buồn, chậm) → toạ độ (-0.8, -0.6)
- "Nhảy" → (vui, nhanh) → toạ độ (0.7, 0.9)

Các từ "cùng nghĩa" tụ lại gần nhau trên bản đồ.

**Embedding** là việc biến một từ/câu/đoạn văn thành **một dãy số** — mỗi số là một "toạ độ" trong một không gian nghĩa. Máy tính dùng khoảng vài trăm đến vài nghìn toạ độ (thay vì 2 như ví dụ trên) để biểu diễn nghĩa chi tiết.

Ví dụ: `"điểm chuẩn ngành CNTT ĐH Bách Khoa"` → `[0.12, -0.34, 0.56, ..., 0.78]` (768 số).

> 💡 **Phép tương tự:** Giống như mỗi người có một địa chỉ nhà — những người sống gần nhau thì hay chơi với nhau. Những câu "sống gần" nhau trong không gian nghĩa thì có nghĩa tương tự nhau.

Tại sao embedding quan trọng? Vì nó cho phép máy tính **tìm kiếm theo nghĩa**, không chỉ khớp từ khoá. Khi bạn hỏi "học phí ngành CNTT", máy có thể tìm ra đoạn ghi "Mức phí đào tạo Công nghệ Thông tin" — dù 2 câu không có từ nào giống nhau.

### 2.5 Vector Database — thư viện tìm kiếm theo nghĩa

**Vector database** là kho lưu trữ các embedding (dãy số) và cho phép tìm kiếm theo nghĩa.

So sánh với database thường:

| Database thường (Excel, SQL) | Vector database (AstraDB) |
|---|---|
| Lưu text y nguyên | Lưu text + embedding (dãy số) |
| Tìm kiếm **khớp từ khoá** | Tìm kiếm **theo nghĩa** |
| "điểm chuẩn" ≠ "điểm đầu vào" | "điểm chuẩn" ≈ "điểm đầu vào" (gần nhau về nghĩa) |
| Dùng cho: danh sách HS, lịch | Dùng cho: tìm kiếm tài liệu, RAG |

Trong dự án này, bạn dùng **AstraDB Serverless** với **vectorize integration** — AstraDB tự lo embedding, bạn chỉ việc đưa text vào. Không cần cấu hình embedding model riêng.

### 2.6 Chunking — chia nhỏ để embedding chính xác

Nếu bạn có 1 file PDF 50 trang, nhét cả 50 trang vào làm 1 embedding thì sao? Embedding sẽ là "trung bình" của cả file → mất nghĩa chi tiết. Giống như bạn tóm tắt cả cuốn sách bằng 1 câu — chắc chắn thiếu rất nhiều.

Giải pháp: **Split Text (chunking)** — chia file thành nhiều đoạn nhỏ (ví dụ mỗi đoạn 500-1000 ký tự), embedding từng đoạn riêng.

Hai tham số quan trọng trong component **Split Text**:

| Tham số | Ý nghĩa | Giá trị gợi ý |
|---|---|---|
| **Chunk Size** | Số ký tự tối đa mỗi đoạn | 800 |
| **Chunk Overlap** | Số ký tự chồng lấn giữa 2 đoạn liên tiếp | 100 |

Tại sao cần **Chunk Overlap**? Vì khi cắt, có thể cắt giữa một câu quan trọng. Overlap giúp đoạn sau "nhớ" phần cuối đoạn trước, tránh mất ngữ cảnh.

Mỗi chunk lưu kèm **metadata** — thông tin phụ giúp lọc sau này:

```json
{
  "truong": "HUST",
  "nam": 2024,
  "loai": "de_an_tuyen_sinh"
}
```

### 2.7 RAG — Retrieval-Augmented Generation

Đây là kiến thức cho **buổi 5**.

**RAG** là kỹ thuật cho LLM đọc **tài liệu liên quan trước** khi trả lời. Gồm 3 bước:

| Bước | Tên | Ý nghĩa | Trong Langflow |
|---|---|---|---|
| 1 | **R**etrieval (Truy xuất) | Tìm các đoạn text liên quan nhất trong vector database | Astra DB component tìm top-k chunks gần câu hỏi nhất |
| 2 | **A**ugmented (Bổ sung) | Ghép câu hỏi + đoạn text tìm được thành 1 prompt | Prompt Template ghép `{context}` + `{question}` |
| 3 | **G**eneration (Sinh) | LLM đọc prompt đã bổ sung và sinh câu trả lời | Language Model sinh câu trả lời |

Hãy tưởng tượng: bạn hỏi "Ngành CNTT ĐH Bách Khoa tuyển tổ hợp gì?". Thay vì LLM tự bịa, RAG sẽ:

1. Tìm trong AstraDB 4 đoạn text liên quan nhất (ví dụ: đoạn về đề án tuyển sinh HUST).
2. Ghép 4 đoạn đó + câu hỏi thành 1 prompt: "Dựa trên thông tin sau: [4 đoạn]. Hãy trả lời: Ngành CNTT ĐH Bách Khoa tuyển tổ hợp gì?"
3. LLM đọc và trả lời dựa trên dữ liệu thật — không bịa.

> 💡 **Cách nhớ:** RAG = cho LLM "đọc bài" trước khi "trả bài". Agent/LLM không phải nhớ hết — chỉ cần biết **tìm** rồi **đọc**.

### 2.8 Prompt Template với `{context}` và `{question}`

Trong flow RAG, Prompt Template đóng vai trò "ghép" context (đoạn text tìm được) với câu hỏi. Template mẫu:

```
Dựa CHỈ trên thông tin sau đây:
---
{context}
---
Hãy trả lời câu hỏi: {question}

NẾU thông tin không đủ để trả lời, hãy nói rõ "Không tìm thấy thông tin trong dữ liệu".
KHÔNG bịa đặt thông tin ngoài {context}.
```

Tại sao cần dặn LLM "KHÔNG bịa"? Vì LLM có xu hướng **hallucination** — "bịa" thông tin nghe có lý nhưng không đúng. Prompt tốt giúp giảm hallucination đáng kể.

### 2.9 Top-k — lấy bao nhiêu đoạn text?

Tham số **Number of Search Results (top-k)** quyết định AstraDB trả về bao nhiêu chunk liên quan nhất.

| Top-k | Ưu điểm | Nhược điểm |
|---|---|---|
| Nhỏ (1-2) | Nhanh, ít nhiễu | Có thể thiếu context → trả lời thiếu |
| Vừa (3-5) | Cân bằng tốt | Phù hợp hầu hết use case |
| Lớn (10+) | Nhiều thông tin | Nhiễu, tốn token, có thể gây nhầm lẫn |

Giá trị gợi ý: **top-k = 4** làm mặc định. Bạn sẽ thử nghiệm 2, 4, 8 ở buổi 5 để thấy sự khác biệt.

### 2.10 Chiến lược fallback — ưu tiên RAG, dự phòng Web Search

Khi agent có cả 2 tool (RAG + Web Search), cần một **chiến lược** để biết dùng tool nào trước:

```
BƯỚC 1: Thử tìm câu trả lời trong Astra DB (RAG).
BƯỚC 2: Đánh giá — câu trả lời từ RAG có đủ/đầy đủ không?
BƯỚC 3a: Nếu ĐỦ → trả lời user, kèm dẫn nguồn từ RAG.
BƯỚC 3b: Nếu KHÔNG đủ → BÁO cho user, HỎI: "Em có muốn tìm trên web uy tín không?"
BƯỚC 4: Nếu user đồng ý → dùng Web Search.
```

Việc **hỏi xin phép user** trước khi search web là pattern **human-in-the-loop** — rất hay dùng khi agent cần làm hành động có rủi ro (search web có thể trả về nguồn không uy tín).

> 💡 **Tại sao ưu tiên RAG?** Vì RAG dùng dữ liệu bạn đã kiểm soát (đã ingest vào AstraDB), nhanh, không phụ thuộc internet, không bị rate limit. Web Search là "dự phòng" cho những gì RAG chưa có.

### 2.11 Component Parser — chuyển đổi định dạng

Trong flow RAG, giữa Astra DB (trả về object phức tạp) và Prompt Template (cần text thuần), cần component **Parser** để chuyển đổi. Parser lấy output của Astra DB và trích xuất phần text content, sẵn sàng để ghép vào `{context}`.

### 2.12 AstraDB — tạo database và collection

**AstraDB Serverless** là dịch vụ vector database miễn phí (free tier) của DataStax. Bạn cần:

1. **Database** — container lớn chứa nhiều collection.
2. **Collection** — "bảng" chứa các document (chunk text + embedding). Trong dự án này: `tuyen_sinh_dai_hoc`.
3. **Vectorize integration** — tính năng tự động embedding khi bạn đưa text vào. Bạn chỉ cần chọn embedding provider (ví dụ NVIDIA NV-Embed-QA) khi tạo collection.
4. **Application Token** — "chìa khoá" xác thực, tương tự API key.
5. **API Endpoint** — địa chỉ URL để Langflow kết nối.

> ⚠️ **Application Token cũng là dữ liệu nhạy cảm** — giữ bí mật, không đăng lên mạng.

### 2.13 Hai chế độ của Astra DB component trong Langflow

Component **Astra DB** trong Langflow có 2 chế độ:

| Chế độ | Mục đích | Dùng khi nào |
|---|---|---|
| **Ingest (Write)** | Đưa text vào collection (lưu + embedding) | Buổi 4 — nạp dữ liệu tuyển sinh |
| **Retrieve (Read)** | Tìm kiếm top-k chunks liên quan nhất | Buổi 5 — RAG tra cứu |

Cùng 1 component, chỉ đổi mode. Cấu hình chung (token, endpoint, collection name) giữ nguyên.


---

## 3. Hướng dẫn thực hành

### 3.1 Vật tư và công cụ cần chuẩn bị

| STT | Vật tư / Công cụ | Số lượng | Ghi chú |
|-----|-------------------|----------|---------|
| 1 | Máy tính có internet, đã cài Langflow Desktop | 1/nhóm | Dùng cả 3 buổi |
| 2 | API key Gemma 4 (Gemini API) | 1/HS | Đã có từ buổi 1 |
| 3 | Tài khoản Notion + Integration Token | 1/HS | Đã có từ buổi 2 |
| 4 | Tài khoản DataStax (AstraDB Serverless free tier) | 1/HS | Đăng ký trước buổi 4 tại [astra.datastax.com](https://astra.datastax.com) |
| 5 | File PDF/text tuyển sinh của 1 trường ĐH | 1-3/nhóm | HS tự chuẩn bị hoặc GV phát |
| 6 | Sổ/note ghi chú so sánh | 1/HS | Giấy hoặc Notion |

### 3.2 Sơ đồ các flow sẽ xây

**Flow buổi 3 — Agent + Web Search:**

```
                  [Web Search] ──┐
                                 ▼ (tool)
[Chat Input] → [Agent] → [Chat Output]
```

**Flow buổi 4 — Ingest thủ công:**

```
[File] → [Split Text] → [Astra DB (Ingest)]
```

**Flow buổi 4 nâng cao — Ingest tự động qua Agent:**

```
                  [Web Search] ──┐
                                 ▼ (tool)
[Chat Input] → [Agent] → [Chat Output]
                                 ▲
                                 │ (tool)
              [Astra DB (Ingest)] ──┘
```

**Flow buổi 5 — RAG cơ bản (không Agent):**

```
[Chat Input] → [Astra DB (Retrieve)] → [Parser] → [Prompt Template] → [Language Model] → [Chat Output]
```

**Flow buổi 5 — Agent + 2 tool (RAG + Web Search):**

```
              [Astra DB RAG] ──┐
                               ▼ (tool)
[Chat Input] → [Agent] → [Chat Output]
                               ▲
                               │ (tool)
              [Web Search] ──┘
```

### 3.3 Hướng dẫn từng bước

#### ━━━ BUỔI 3 (buổi 3 của khoá) — Web Search ━━━

**Bước 1: Tạo flow mới Agent + Web Search**

- Mở Langflow Desktop → **+ New Flow** → **Blank Flow**.
- Kéo từ Sidebar vào canvas: **Chat Input** (Inputs), **Agent** (Agents), **Chat Output** (Outputs), **Web Search** (Data/Core).

**Bước 2: Cấu hình Agent**

- Click **Agent** → mở Component inspection panel.
- **Model Provider:** Google.
- **Model Name:** Gemma hoặc Gemini Flash.
- **API Key:** dán API key Gemma 4 (đã có từ buổi 1).

**Bước 3: Cấu hình Web Search ở Tool Mode**

- Click **Web Search** → bật **Tool Mode** (toggle trong inspection panel).
- Điền:
    - **Tool Name:** `web_search_tuyen_sinh`
    - **Tool Description:**

```
Tìm kiếm trên web các thông tin tuyển sinh đại học Việt Nam mới nhất (điểm chuẩn, phương thức xét tuyển, học bổng, tin tuyển sinh năm hiện tại). Input: query (chuỗi từ khoá tiếng Việt). Output: danh sách các bài viết kèm link nguồn. Dùng tool này khi user hỏi về thông tin thời sự hoặc cụ thể mà em không biết chắc chắn.
```

**Bước 4: Viết Agent Instructions**

Dán vào ô **Agent Instructions** của component Agent:

```
Bạn là "Agent Thông tin tuyển sinh" — trợ lý tra cứu thông tin tuyển sinh đại học Việt Nam.

NGUYÊN TẮC:
1. Với câu hỏi về thông tin cập nhật (điểm chuẩn, học phí, lịch tuyển sinh, tin mới), LUÔN dùng tool web_search_tuyen_sinh để tra cứu thay vì trả lời từ trí nhớ.
2. Ưu tiên nguồn chính thức: website trường (.edu.vn), Bộ GD&ĐT (moet.gov.vn), báo chính thống (vnexpress, tuoitre, thanhnien).
3. LUÔN dẫn link nguồn ở cuối câu trả lời, format: [Tên nguồn](URL).
4. Nếu kết quả web không đủ/mâu thuẫn, nói rõ "Tôi tìm được X nhưng chưa chắc chắn" thay vì bịa.
5. Trả lời ngắn gọn, có cấu trúc (bullet/bảng khi cần), tiếng Việt, xưng "em/thầy cô" phù hợp.
```

**Bước 5: Nối các component**

- `Chat Input` → `Agent` (input port).
- `Web Search` (Tool output) → `Agent` (Tools port).
- `Agent` (Message output) → `Chat Output`.

**Bước 6: Test với 5 câu hỏi tuyển sinh**

Mở **Playground**, chạy lần lượt 5 câu. Gợi ý:

1. "Điểm chuẩn ngành CNTT ĐH Bách Khoa Hà Nội năm 2025 là bao nhiêu?"
2. "ĐH Kinh tế Quốc dân có những phương thức xét tuyển nào?"
3. "Học phí ngành Y đa khoa ĐH Y Hà Nội năm nay bao nhiêu?"
4. "Có tin gì mới về kỳ thi đánh giá năng lực ĐHQG?"
5. "So sánh điểm chuẩn ngành Luật giữa ĐH Luật Hà Nội và Khoa Luật ĐHQG."

Ghi kết quả vào phiếu so sánh.

**Bước 7: So sánh có/không Web Search**

Để test "không Web Search": tạm **xoá dây nối** Web Search → Agent, chạy lại 3 câu, ghi lại. Rồi nối dây lại.

Tiêu chí nhận xét: độ mới, độ cụ thể, có dẫn nguồn không, có "bịa" không.

Kết quả mong đợi: Agent có Web Search trả lời chi tiết hơn, dẫn nguồn được, có thông tin cập nhật. Agent không có Web Search trả lời chung chung hoặc từ chối.

#### ━━━ BUỔI 4 (buổi 4 của khoá) — Vector Database & Embedding ━━━

**Bước 8: Tạo database + collection trên AstraDB**

1. Truy cập [astra.datastax.com](https://astra.datastax.com), đăng nhập.
2. Click **Create Database** → chọn **Serverless (Vector)** → tên database: `langflow_tuyensinh` → Region gần nhất → Create. Chờ 2-3 phút.
3. Khi database **Active**, mở → tab **Data Explorer** → **Create Collection**:
    - Name: `tuyen_sinh_dai_hoc`.
    - **Bật "Vector-enabled collection"**.
    - Embedding Provider: chọn **NVIDIA** hoặc **OpenAI** tuỳ list có sẵn free (thường NVIDIA NV-Embed-QA là free).
    - Dimensions: giữ mặc định của model đã chọn.
    - Create.
4. Lấy **Application Token** và **API Endpoint**: tab **Settings** → **Generate Token** (role Database Administrator). Copy token (`AstraCS:...`) và Endpoint (`https://....apps.astra.datastax.com`). Lưu 2 giá trị này vào note an toàn.

Kết quả mong đợi: collection `tuyen_sinh_dai_hoc` hiện trong Data Explorer, database ở trạng thái Active.

**Bước 9: Xây flow ingest thủ công**

1. Trong Langflow, tạo **flow mới** — `Blank Flow`, đặt tên `Ingest_AstraDB`.
2. Kéo vào canvas: **File** (Data), **Split Text** (Processing), **Astra DB** (Vector Stores).
3. Cấu hình **File**: upload file PDF/text tuyển sinh đã chuẩn bị.
4. Cấu hình **Split Text**: Chunk Size = `800`, Chunk Overlap = `100`.
5. Cấu hình **Astra DB**:
    - **Astra DB Application Token**: dán token đã lấy.
    - **API Endpoint**: dán endpoint.
    - **Collection Name**: `tuyen_sinh_dai_hoc`.
    - **Mode**: Ingest/Write.
    - **Metadata** (nếu có ô metadata): `{"truong": "HUST", "nam": 2024, "loai": "de_an_tuyen_sinh"}` — thay tên trường phù hợp.
6. Nối: `File → Split Text → Astra DB`.
7. Nhấn **Run** trên component Astra DB (hoặc Play toàn flow).

Kết quả mong đợi: thông báo "Inserted N documents" (N tuỳ độ dài file, thường 10-50 chunks).

**Bước 10: Xác thực dữ liệu đã ingest**

- Mở AstraDB Console → Data Explorer → collection `tuyen_sinh_dai_hoc` → thấy N rows mới.
- Click 1 row → xem field `content` (text gốc) và `$vector` (dãy số — embedding).
- Kiểm tra: mỗi chunk có bao nhiêu ký tự? Embedding có bao nhiêu số?

**Bước 11: Lặp lại cho các trường khác**

- Thay file mới (trường khác), thay metadata (`truong`, `loai`).
- Chạy lại flow. Mục tiêu: **3 trường** ingest thủ công.

**Bước 12 (Nâng cao): Flow ingest tự động qua Agent**

Dành cho HS làm nhanh:

1. Tạo flow mới: `Chat Input → Agent → Chat Output`, gắn 2 tool: **Web Search** (Tool Mode) và **Astra DB** (Tool Mode — mode Ingest).
2. Agent Instructions mẫu:

```
Khi user nói "thu thập thông tin trường X":
1. Dùng Web Search tìm đề án tuyển sinh, điểm chuẩn, học bổng của trường X.
2. Với mỗi kết quả phù hợp, dùng tool Astra DB ingest để lưu text vào collection tuyen_sinh_dai_hoc kèm metadata {truong, nam, loai}.
3. Báo cáo: "Đã lưu N đoạn thông tin về trường X."
```

3. Test với 1-2 trường khác. Mục tiêu: tổng **5+ trường** trong AstraDB.

#### ━━━ BUỔI 5 (buổi 5 của khoá) — RAG + Chiến lược Fallback ━━━

**Bước 13: Dùng template Vector Store RAG có sẵn**

1. Trong Langflow, New Flow → chọn **Template** → tìm **Vector Store RAG** → Open.
2. Quan sát cấu trúc có sẵn: `Chat Input → [Astra DB retriever] → [Parser] → [Prompt Template] → [Language Model] → Chat Output`.
3. Cấu hình **Astra DB** component: collection `tuyen_sinh_dai_hoc`, token, endpoint. **Mode**: Retrieve. **Number of Search Results**: `4`.
4. Cấu hình **Language Model**: Provider Google, Model Gemma/Gemini, API key.
5. Chạy thử với 1 câu hỏi về trường đã ingest. Ví dụ: "Ngành CNTT ĐH Bách Khoa tuyển tổ hợp gì?"

Kết quả mong đợi: agent trả lời dựa trên dữ liệu AstraDB, không bịa.

**Bước 14: Thử nghiệm top-k**

Giữ nguyên 1 câu hỏi. Lần lượt chạy với `top-k = 2 → 4 → 8`. Ghi vào phiếu:

| Top-k | Độ dài trả lời | Mức độ đầy đủ | Có thừa thông tin lạc đề không? |
|---|---|---|---|
| 2 | ... | ... | ... |
| 4 | ... | ... | ... |
| 8 | ... | ... | ... |

Nhận xét: top-k nào tốt nhất cho câu hỏi này? Top-k có phải "càng cao càng tốt" không?

**Bước 15: Nâng cấp thành Agent + 2 tool với chiến lược fallback**

1. New Flow mới `Blank Flow` → tên: `Agent_Thong_Tin_Tuyen_Sinh`.
2. Kéo vào: `Chat Input`, `Agent`, `Chat Output`, `Astra DB`, `Web Search`.
3. Cấu hình **Astra DB** ở **Tool Mode** (mode Retrieve):
    - Collection: `tuyen_sinh_dai_hoc`, token + endpoint.
    - Number of Search Results: `4`.
    - **Tool Name**: `rag_tuyen_sinh`.
    - **Tool Description**:

```
Tra cứu cơ sở dữ liệu nội bộ về tuyển sinh đại học Việt Nam (đề án tuyển sinh, điểm chuẩn các năm, học bổng của các trường đã được team lưu sẵn). Input: query (chuỗi tiếng Việt). Output: các đoạn text liên quan nhất từ cơ sở dữ liệu. DÙNG TOOL NÀY TRƯỚC TIÊN cho mọi câu hỏi tuyển sinh.
```

4. Cấu hình **Web Search** ở **Tool Mode**:
    - **Tool Name**: `web_search_tuyen_sinh`.
    - **Tool Description**: "Tìm thông tin tuyển sinh mới trên web chính thống. CHỈ DÙNG SAU KHI rag_tuyen_sinh không có đủ thông tin VÀ user đã đồng ý search web."

5. Dán **Agent Instructions** (chiến lược fallback):

```
Bạn là "Agent Thông tin tuyển sinh".

QUY TRÌNH BẮT BUỘC cho MỌI câu hỏi tuyển sinh:
1. GỌI TRƯỚC tool rag_tuyen_sinh để tra cứu cơ sở dữ liệu nội bộ.
2. Đọc kết quả. Nếu ĐỦ thông tin → trả lời user kèm dẫn nguồn nội bộ (tên trường, năm).
3. Nếu KHÔNG đủ → KHÔNG tự động search web. Thay vào đó, nói với user:
   "Em không tìm thấy đủ thông tin về [X] trong cơ sở dữ liệu nội bộ. Em có muốn anh/chị tìm kiếm trên các trang web uy tín (website trường, Bộ GD&ĐT, báo chính thống) không?"
4. CHỈ KHI user trả lời đồng ý (yes/có/ok/...) → mới gọi tool web_search_tuyen_sinh.
5. Luôn dẫn link nguồn khi dùng Web Search. Ưu tiên domain .edu.vn, .gov.vn, báo chính thống.
6. Nếu cả RAG và Web đều không có → thành thật nói "Em không có thông tin về việc này".

KHÔNG BAO GIỜ bịa điểm chuẩn, học phí, tên ngành không có trong nguồn.
```

6. Nối: `Chat Input → Agent`, `Astra DB (Tool)` + `Web Search (Tool)` → Agent (Tools port), `Agent → Chat Output`.

**Bước 16: Test 5 câu pha trộn**

Chuẩn bị 5 câu: 2 câu RAG trả lời được, 2 câu cần Web Search, 1 câu tổng hợp. Chạy lần lượt và quan sát:

- Agent có gọi `rag_tuyen_sinh` TRƯỚC không?
- Khi RAG không đủ, agent có HỎI user không? Hay tự ý chạy Web Search?
- Khi user đồng ý, agent có chuyển sang Web Search đúng cách?

Ghi kết quả vào phiếu test:

| # | Câu hỏi | Dự kiến (RAG/Web/Hỗn hợp) | Kết quả thực tế | Agent chọn đúng tool? |
|---|---|---|---|---|
| 1 | ... | RAG | ... | Có/Không |
| 2 | ... | RAG | ... | Có/Không |
| 3 | ... | Web | ... | Có/Không |
| 4 | ... | Web | ... | Có/Không |
| 5 | ... | Hỗn hợp | ... | Có/Không |

**Checklist debug cả 3 buổi:**

| Triệu chứng | Nguyên nhân | Giải pháp |
|---|---|---|
| Agent không gọi Web Search, trả lời từ trí nhớ | Tool Description mơ hồ | Thêm "LUÔN dùng tool khi câu hỏi liên quan tin mới/cụ thể" |
| Web Search báo lỗi rate limit | Gọi quá nhiều lần | Đợi 1-2 phút, giảm Max Results, hoặc đổi Provider |
| Agent trả lời không có link | Agent Instructions chưa yêu cầu | Thêm "LUÔN dẫn link nguồn ở cuối" |
| Agent trích nguồn Facebook/Forum | Chưa nhấn mạnh nguồn uy tín | Thêm danh sách domain ưu tiên vào Instructions |
| Ingest báo lỗi token | Token sai hoặc role không đủ | Copy lại token từ AstraDB Console, chọn role Database Administrator |
| Ingest báo lỗi endpoint | Endpoint sai | Copy lại từ AstraDB Console |
| Ingest báo lỗi embedding | Collection chưa bật vectorize | Tạo lại collection có bật "Vector-enabled" |
| RAG báo "không tìm thấy" dù có dữ liệu | Collection name sai, hoặc dữ liệu chưa ingest | Quay lại AstraDB Console kiểm tra rows |
| Agent không gọi RAG | Tool Description yếu | Thêm "DÙNG TRƯỚC TIÊN" và ví dụ |
| Agent tự ý Web Search mà không hỏi | Thiếu ràng buộc fallback | Thêm "KHÔNG TỰ ĐỘNG" vào Instructions |
| Top-k quá lớn làm loãng | Giảm về 3-5 | Chỉnh Number of Search Results |


---

## 4. Nâng cao

**Mở rộng A — Đổi Provider Web Search**

Thử thay đổi Provider của Web Search (DuckDuckGo ↔ Google News ↔ RSS feed). Quan sát: nguồn nào cho kết quả tuyển sinh tốt hơn? Google News thường có tin mới hơn, DuckDuckGo cho kết quả đa dạng hơn.

**Mở rộng B — Metadata nâng cao khi ingest**

Thêm metadata chi tiết hơn cho mỗi chunk: `{"truong": "HUST", "nam": 2024, "loai": "de_an_tuyen_sinh", "nganh": "CNTT", "phuong_thuc": "diem_thi"}`. Metadata càng chi tiết, sau này filter càng chính xác.

**Mở rộng C — So sánh RAG vs. Web Search trên cùng câu hỏi**

Chọn 1 câu hỏi mà cả RAG và Web Search đều trả lời được (ví dụ: "Điểm chuẩn ngành CNTT ĐH Bách Khoa 2024"). Chạy 2 lần: 1 lần chỉ RAG, 1 lần chỉ Web Search. So sánh: nguồn nào chính xác hơn? Nguồn nào nhanh hơn? Nguồn nào dẫn nguồn tốt hơn?

**Mở rộng D — Agent trích xuất bảng Markdown**

Yêu cầu Agent trích xuất thông tin từ RAG thành bảng Markdown so sánh 3 trường. Ví dụ prompt: "Liệt kê các ngành có điểm chuẩn dưới 24 ở 3 trường đã ingest, trình bày dạng bảng." Đây là bước đệm cho dự án 3 (Structured Output).

**Mở rộng E — Câu hỏi đa bước**

Thử câu hỏi phức tạp đòi hỏi agent gọi tool nhiều lần: "Tìm 3 trường ở Hà Nội có ngành CNTT, so sánh điểm chuẩn, rồi gợi ý trường nào phù hợp cho HS có 25 điểm." Agent cần gọi RAG nhiều lần + tổng hợp.

---

## 5. Câu hỏi ôn tập

1. **(Nhớ)** Kể tên 3 Provider mà component Web Search trong Langflow hỗ trợ.
2. **(Nhớ)** RAG là viết tắt của gì? Giải thích ngắn gọn ý nghĩa từng chữ cái.
3. **(Hiểu)** Embedding là gì? Giải thích bằng ngôn ngữ bình dân (không dùng thuật ngữ kỹ thuật) cho một bạn chưa biết gì về AI.
4. **(Hiểu)** Tại sao cần **chunking** (Split Text) trước khi đưa file vào vector database? Nếu không chunk mà nhét cả file 50 trang làm 1 embedding thì hậu quả là gì?
5. **(Hiểu)** Phân biệt **database thường** (Excel, SQL) và **vector database** (AstraDB). Cho 1 ví dụ câu hỏi mà vector database trả lời được nhưng database thường không.
6. **(Áp dụng)** Bạn muốn agent ưu tiên nguồn chính thức khi search web. Hãy viết 2 câu thêm vào Agent Instructions để đạt mục tiêu này.
7. **(Áp dụng)** Bạn có 1 file PDF 100 trang về đề án tuyển sinh. Chunk Size = 800, Chunk Overlap = 100. Ước tính file sẽ tạo ra khoảng bao nhiêu chunks? (Gợi ý: 1 trang PDF ≈ 2000-3000 ký tự.)
8. **(Phân tích)** Khi top-k = 2, agent trả lời "Ngành CNTT ĐH Bách Khoa tuyển tổ hợp A00, A01". Khi top-k = 8, agent trả lời thêm "...và cả A02 cho chương trình tiên tiến". Phân tích: tại sao top-k lớn hơn lại cho thêm thông tin? Có rủi ro gì khi top-k quá lớn?
9. **(Đánh giá)** Bạn A xây agent chỉ có Web Search (không RAG). Bạn B xây agent chỉ có RAG (không Web Search). Bạn C xây agent có cả hai với chiến lược fallback. Đánh giá ưu/nhược điểm của mỗi cách. Cách nào phù hợp nhất cho "thư viện tuyển sinh" và vì sao?
10. **(Sáng tạo)** Ngoài tuyển sinh, bạn có thể dùng pattern "RAG + Web Search fallback" cho lĩnh vực nào khác? Nêu 1 ý tưởng, mô tả dữ liệu sẽ ingest vào AstraDB và loại câu hỏi agent sẽ trả lời.

---

## 6. Thuật ngữ

| Thuật ngữ | Giải thích |
|---|---|
| **Tool** | "Cánh tay nối dài" cho agent: component thực hiện hành động (search web, đọc/ghi database, gọi API). Agent tự quyết định khi nào gọi tool. |
| **Web Search** | Component Langflow cho phép agent tìm kiếm trên internet. Hỗ trợ DuckDuckGo, Google News, RSS. |
| **Rate limit** | Giới hạn số lần gọi API trong một khoảng thời gian. Vượt quá sẽ bị chặn tạm thời. |
| **Embedding** | Quá trình biến text thành dãy số (vector) đại diện cho nghĩa. Các câu có nghĩa tương tự sẽ có embedding gần nhau. |
| **Vector** | Dãy số đại diện cho nghĩa của một đoạn text. Ví dụ: [0.12, -0.34, 0.56, ...] với vài trăm đến vài nghìn số. |
| **Vector database** | Cơ sở dữ liệu lưu trữ text kèm embedding, cho phép tìm kiếm theo nghĩa thay vì khớp từ khoá. |
| **AstraDB Serverless** | Dịch vụ vector database miễn phí (free tier) của DataStax. Hỗ trợ vectorize integration (tự embedding). |
| **Collection** | "Bảng" trong AstraDB chứa các document (chunk text + embedding). Tương đương table trong SQL. |
| **Vectorize integration** | Tính năng của AstraDB tự động tạo embedding khi bạn đưa text vào. Không cần cấu hình embedding model riêng. |
| **Application Token** | "Chìa khoá" xác thực để Langflow kết nối AstraDB. Bắt đầu bằng `AstraCS:...`. Giữ bí mật. |
| **API Endpoint** | Địa chỉ URL để Langflow kết nối đến database AstraDB cụ thể. |
| **Chunking (Split Text)** | Chia file dài thành nhiều đoạn nhỏ (chunk) trước khi embedding. Giúp embedding chính xác hơn. |
| **Chunk Size** | Số ký tự tối đa mỗi đoạn khi chunking. Giá trị gợi ý: 800. |
| **Chunk Overlap** | Số ký tự chồng lấn giữa 2 đoạn liên tiếp. Giúp tránh mất ngữ cảnh khi cắt. Giá trị gợi ý: 100. |
| **Metadata** | Thông tin phụ gắn kèm mỗi chunk (tên trường, năm, loại). Giúp lọc kết quả sau này. |
| **Ingest** | Quá trình đưa dữ liệu vào vector database (chunking → embedding → lưu). |
| **RAG** (Retrieval-Augmented Generation) | Kỹ thuật cho LLM đọc tài liệu liên quan trước khi trả lời. Gồm 3 bước: Retrieval → Augmented → Generation. |
| **Retrieval** | Bước 1 của RAG: tìm các chunk liên quan nhất trong vector database dựa trên câu hỏi. |
| **Top-k** (Number of Search Results) | Số chunk liên quan nhất mà vector database trả về. Giá trị gợi ý: 4. |
| **Parser** | Component Langflow chuyển đổi output phức tạp của Astra DB thành text thuần để ghép vào Prompt Template. |
| **Prompt Template** | Component chứa prompt có biến `{context}` và `{question}`. Biến được thay bằng dữ liệu thực khi chạy. |
| **Hallucination** | Hiện tượng LLM "bịa" thông tin nghe có lý nhưng không đúng. RAG + prompt tốt giúp giảm hallucination. |
| **Fallback** | Chiến lược dự phòng: khi công cụ chính (RAG) không đủ, chuyển sang công cụ phụ (Web Search). |
| **Human-in-the-loop** | Pattern thiết kế: agent hỏi xin phép user trước khi thực hiện hành động có rủi ro (ví dụ: search web). |
| **Nguồn uy tín** | Website chính thức (.edu.vn, .gov.vn), báo chính thống. Ưu tiên hơn diễn đàn và mạng xã hội. |