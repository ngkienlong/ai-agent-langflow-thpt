# Dự án 3: Agent Tư vấn cá nhân hoá

**Số buổi:** 2 × 90 phút

---

## 1. Giới thiệu dự án

### Bối cảnh thực tế

Cô H là trưởng ban tuyển sinh của một trường THPT. Năm ngoái, cô phải ngồi đọc tay 20 đề án tuyển sinh PDF dài 50 trang mỗi cái, rồi copy-paste vào một bảng Excel để gửi cho phụ huynh. Mất 2 tuần. Năm nay, cô muốn AI giúp tự động hoá việc này.

Đồng thời, mỗi học sinh đến phòng tư vấn đều có hoàn cảnh khác nhau: bạn An thích Sinh học và muốn học bổng, bạn Bình giỏi Toán và code game, bạn Cường thích Văn và muốn du học. Một lời tư vấn chung chung "em nên học ngành hot" không giúp được ai. Mỗi em cần một bản tư vấn **riêng** — dựa trên chính sở thích, điểm mạnh, ràng buộc của em.

Trong dự án này, bạn sẽ làm 2 việc lớn:

1. **Buổi 6 — Hoàn thiện Agent Thông tin tuyển sinh:** Dạy agent trích xuất dữ liệu text thô từ AstraDB thành **bảng có cấu trúc 5 cột** (Trường, Ngành, Tổ hợp môn, Điểm chuẩn, Học bổng) trên Notion — dùng component **Structured Output**. Đây là bước "chuyển từ văn xuôi sang bảng biểu" mà cô H cần.

2. **Buổi 7 — Xây Agent Tư vấn:** Agent cuối cùng trong hệ thống 3 agent. Nó đọc bảng hỏi cá nhân hoá (từ Agent Chiến lược, dự án 1), tra cứu bảng tuyển sinh (từ Agent Thông tin, dự án 2), rồi đưa ra tư vấn **riêng cho từng học sinh**: 3-5 ngành phù hợp, 3-5 trường phù hợp, kế hoạch học tập cụ thể.

### Câu hỏi dẫn dắt

> "Làm thế nào để Agent Thông tin tuyển sinh tự lập bảng 5 cột đẹp trên Notion, và Agent Tư vấn đưa ra lời khuyên riêng cho từng học sinh dựa trên dữ liệu thật?"

### Mục tiêu học tập

**Buổi 6 — Structured Output: hoàn thiện Agent Thông tin:**

- [ ] Giải thích được sự khác biệt giữa dữ liệu **phi cấu trúc** và **có cấu trúc**, tại sao cần cả hai.
- [ ] Thiết kế schema JSON cho 5 trường cần trích xuất (Trường, Ngành, Tổ hợp môn, Điểm chuẩn tham khảo, Học bổng).
- [ ] Tạo 1 Notion database "Bảng tuyển sinh" với 5 cột phù hợp và chia sẻ với Notion Internal Integration.
- [ ] Xây dựng flow Agent: đọc Astra DB → Structured Output trích xuất → ghi từng hàng lên Notion qua Create Page.
- [ ] Hoàn thiện Agent Thông tin tuyển sinh phiên bản đầy đủ.
- [ ] Kiểm tra trên Notion: bảng có đủ dữ liệu, mỗi ngành 1 hàng với đầy đủ 5 cột.

**Buổi 7 — Agent Tư vấn & testing end-to-end:**

- [ ] Xây dựng Agent Tư vấn: đọc file bảng hỏi (nếu có) hoặc hỏi 8 câu (nếu không có).
- [ ] Tích hợp Agent Tư vấn với Astra DB (RAG) + Web Search + Notion bundle.
- [ ] Viết system prompt để Agent Tư vấn đưa ra đầu ra theo template: 3-5 ngành + 3-5 trường + kế hoạch học tập.
- [ ] Kiểm thử luồng end-to-end 3 agent với ít nhất 3 hồ sơ học sinh khác nhau.

### Sản phẩm đầu ra

**Cuối buổi 6:**

- 1 Notion database "Bảng tuyển sinh" với 5 cột đúng property type, đã share với Integration.
- Flow Agent hoàn thiện Agent Thông tin: chạy được trên ít nhất 3 trường, ghi nhiều hàng vào Notion database (mỗi ngành 1 hàng).

**Cuối buổi 7:**

- Flow Agent Tư vấn hoàn chỉnh với logic điều kiện "có file bảng hỏi → đọc file; không có → hỏi 8 câu", tích hợp Astra DB RAG + Web Search + Notion bundle.
- Agent Tư vấn output theo đúng template: 3-5 ngành phù hợp + 3-5 trường phù hợp + kế hoạch học tập cụ thể.
- Báo cáo test end-to-end với 3 hồ sơ HS khác biệt → 3 tư vấn khác nhau rõ rệt.

---

## 2. Kiến thức nền tảng

### 2.1 Dữ liệu phi cấu trúc vs. có cấu trúc — hai dạng bổ sung nhau

Ở dự án 2, bạn đã lưu dữ liệu tuyển sinh vào AstraDB dưới dạng **text thô** (phi cấu trúc). Agent trả lời câu hỏi tốt, nhưng nếu cô H muốn một **bảng tổng hợp** gửi phụ huynh thì sao?

Cùng một nội dung, hai dạng khác nhau:

**Dạng A — Phi cấu trúc (text thô):**

> "Trường ĐH Bách Khoa Hà Nội năm 2024 tuyển sinh ngành Công nghệ Thông tin với tổ hợp A00, A01. Điểm chuẩn năm 2024 là 28.3. Trường có học bổng toàn phần cho top 10% thí sinh trúng tuyển..."

**Dạng B — Có cấu trúc (bảng):**

| Trường | Ngành | Tổ hợp | Điểm chuẩn | Học bổng |
|---|---|---|---|---|
| ĐH Bách Khoa HN | Công nghệ Thông tin | A00, A01 | 28.3 | Học bổng toàn phần top 10% |

Dạng A dễ cho AI đọc và trả lời câu hỏi. Dạng B dễ cho **người** đọc nhanh, so sánh, lọc. Hai dạng bổ sung nhau — và hôm nay bạn sẽ dạy agent **chuyển** từ A sang B.

### 2.2 Structured Output — "khuôn mẫu" cho LLM

Bình thường khi bạn hỏi LLM "Cho tôi thông tin tuyển sinh ngành CNTT", nó trả lời mỗi lần một khác: lúc thì văn xuôi, lúc thì bullet, lúc thì thiếu trường này thừa trường kia. Giống như bạn nói "Cho tôi 1 bát phở" — mỗi quán mỗi khác.

Nhưng nếu bạn nói: "Cho tôi 1 bát phở gồm: 300g bánh, 100g thịt, 50g rau thơm, 1 viên chanh" — đó là **cấu trúc cụ thể**. LLM cũng vậy: không có schema → output mỗi lần một khác; có schema → luôn ra đúng khuôn mẫu.

**Structured Output** là component trong Langflow cho phép bạn định nghĩa một **schema JSON** — khuôn mẫu cố định — và LLM sẽ trả về đúng format đó.

Schema JSON cho dự án này:

```json
{
  "truong": "string — tên đầy đủ của trường",
  "nganh": "string — tên ngành học",
  "to_hop_mon": "string — danh sách tổ hợp xét tuyển, ví dụ 'A00, A01'",
  "diem_chuan_tham_khao": "number — điểm chuẩn năm gần nhất, ví dụ 27.5",
  "hoc_bong": "string — mô tả ngắn về học bổng, nếu không có ghi 'Không có'"
}
```

Khi bật **Tool Mode**, agent sẽ tự gọi Structured Output mỗi khi cần trích xuất dữ liệu theo schema.

### 2.3 Notion property types — chọn đúng kiểu cho từng cột

Notion database có nhiều kiểu cột (property type). Chọn đúng kiểu giúp tận dụng filter/sort/view:

| Cột | Property type | Lý do |
|---|---|---|
| **Trường** | **Title** | Bắt buộc có 1 cột Title mỗi database — dùng làm "tên hàng", định danh |
| **Ngành** | **Select** | Nhiều ngành lặp lại → Select gọn, dễ lọc |
| **Tổ hợp môn** | **Rich Text** | Tự do nhập nhiều tổ hợp, ngăn bằng dấu phẩy |
| **Điểm chuẩn tham khảo** | **Number** | Số — để sort/filter theo giá trị (ví dụ: lọc ngành < 25 điểm) |
| **Học bổng** | **Rich Text** | Mô tả dài, tự do |

> 💡 **Mẹo:** Chọn đúng kiểu **ngay từ đầu**. Nếu chọn Rich Text cho mọi cột, bảng hoạt động nhưng mất hầu hết tính năng filter/sort — giống như dùng Excel mà gõ số dưới dạng text.

### 2.4 Create Page — ghi vào database (khác với ghi vào parent page)

Ở dự án 1, bạn dùng **Create Page** để tạo page con bên trong parent page "Bảng hỏi học sinh". Lần này, bạn dùng **Create Page** để ghi **hàng mới vào Notion database** — mỗi hàng là 1 ngành của 1 trường.

Sự khác biệt:

| Dự án 1 | Dự án 3 (buổi 6) |
|---|---|
| Parent type: **Page** | Parent type: **Database** |
| Input: title + content (text dài) | Input: 5 property values (Trường, Ngành, Tổ hợp, Điểm, Học bổng) |
| Tạo page con chứa text | Tạo hàng mới trong bảng |

Khi cấu hình Create Page cho database, bạn cần dán **Database ID** (thay vì Parent Page ID).

### 2.5 Component File — đọc file bảng hỏi

Đây là kiến thức cho **buổi 7**.

Component **File** trong Langflow đọc nội dung file (PDF, TXT, Markdown) mà user đính kèm. Trong dự án này, Agent Tư vấn dùng File để đọc bảng hỏi cá nhân hoá mà HS đã trả lời (từ Notion, export ra file).

Khi bật **Tool Mode**, agent tự gọi File khi user đính kèm file — không cần bạn cấu hình cố định.

### 2.6 Notion Page Content Viewer — đọc page Notion trực tiếp

Ngoài cách đọc file, Agent Tư vấn còn có thể đọc **trực tiếp page Notion** qua component **Page Content Viewer** (trong Notion bundle). User chỉ cần gửi link Notion, agent tự lấy page_id và đọc nội dung.

So sánh 2 cách đọc bảng hỏi:

| Cách | Cách dùng | Ưu điểm | Nhược điểm |
|---|---|---|---|
| **File** | User download page Notion → đính kèm file | Đơn giản, không cần kết nối Notion | User phải download thủ công |
| **Page Content Viewer** | Agent đọc trực tiếp page Notion qua Integration | Tự động, không cần file | Phải biết page_id, cần quyền share |

Dự án này dùng **cả 2** cho linh hoạt.

### 2.7 Điều hướng theo điều kiện trong Agent Instructions

Agent Tư vấn cần xử lý **2 tình huống**:

- **Tình huống 1:** HS đã có bảng hỏi (gửi link Notion hoặc file) → đọc và tư vấn ngay.
- **Tình huống 2:** HS chưa có bảng hỏi → hỏi nhanh 8 câu rồi tư vấn.

Bạn viết logic này trong Agent Instructions bằng cấu trúc **NẾU ... THÌ ... NGƯỢC LẠI** — giống pseudocode:

```
NẾU user gửi link Notion hoặc file:
  → Đọc nội dung → chuyển sang BƯỚC TƯ VẤN.

NẾU user nói "chưa có":
  → Hỏi lần lượt 8 câu → chuyển sang BƯỚC TƯ VẤN.
```

Từ khoá **NẾU / NGƯỢC LẠI / BƯỚC** giúp LLM đọc và làm đúng logic. Viết prompt với cấu trúc rõ như code pseudocode — đừng viết văn xuôi dài dòng.

### 2.8 Template output tư vấn — chuẩn hoá kết quả

Để tư vấn ổn định và dễ so sánh giữa các HS, bạn định nghĩa một **template output cố định**:

```
## Hồ sơ tóm tắt
[3-5 câu tóm lại đặc điểm HS: lớp, khối, thế mạnh, sở thích, ràng buộc]

## 3-5 ngành phù hợp
1. [Tên ngành] — lý do phù hợp với HS này.
2. ...

## 3-5 trường phù hợp
| Trường | Ngành cụ thể | Điểm chuẩn tham khảo | Ghi chú |
| ... | ... | ... | ... |

## Kế hoạch học tập gợi ý
- Môn cần tập trung: ...
- Phương thức xét tuyển nên chọn: ...
- Học bổng có thể nhắm đến: ...

## Dẫn nguồn
- Nguồn RAG: [ghi rõ trường, năm]
- Nguồn Web (nếu có): [link]
```

Template cố định = tư vấn ổn định = dễ so sánh 3 HS. HS nào cũng ra 4 section, nhưng **nội dung khác nhau** tuỳ hồ sơ.

### 2.9 Testing end-to-end — kiểm thử toàn bộ hệ thống

**End-to-end testing** là kiểm thử luồng hoàn chỉnh từ đầu đến cuối. Trong dự án này:

1. Agent Chiến lược (dự án 1) → tạo bảng hỏi trên Notion.
2. Agent Thông tin (dự án 2-3) → lưu dữ liệu AstraDB + lập bảng Notion.
3. Agent Tư vấn (dự án 3) → đọc bảng hỏi + tra cứu dữ liệu → tư vấn cá nhân hoá.

Bạn test bằng cách chạy Agent Tư vấn với **3 hồ sơ HS khác biệt** và kiểm tra:

- Agent chọn đúng nhánh điều hướng (có file / không file)?
- 3 tư vấn có **khác nhau rõ rệt** không?
- Trường/ngành gợi ý có phù hợp với hồ sơ HS không?
- Có dẫn nguồn không?

Nếu 3 tư vấn giống nhau → prompt chưa đủ mạnh về cá nhân hoá → cần sửa.


---

## 3. Hướng dẫn thực hành

### 3.1 Vật tư và công cụ cần chuẩn bị

| STT | Vật tư / Công cụ | Số lượng | Ghi chú |
|-----|-------------------|----------|---------|
| 1 | Máy tính có internet, đã cài Langflow Desktop | 1/nhóm | Dùng cả 2 buổi |
| 2 | API key Gemma 4 (Gemini API) | 1/HS | Đã có từ buổi 1 |
| 3 | AstraDB đã có dữ liệu 5+ trường từ dự án 2 | 1/HS | Token + endpoint đã lưu |
| 4 | Notion + Integration Token | 1/HS | Đã có từ buổi 2 |
| 5 | File/link bảng hỏi cá nhân hoá đã trả lời (từ Notion, buổi 2) | 1-3/HS | Buổi 7 — mang theo |
| 6 | Sổ/note ghi chú test end-to-end | 1/HS | Giấy hoặc Notion |

### 3.2 Sơ đồ các flow sẽ xây

**Flow buổi 6 — Agent Thông tin + Structured Output + Create Page (Notion DB):**

```
              [Astra DB RAG (Tool)] ──┐
                                      │
              [Web Search (Tool)] ────┤
                                      ▼
[Chat Input] → [Agent] ─────────→ [Chat Output]
                                      ▲
              [Structured Output] ────┤
              (Tool)                  │
              [Create Page] ──────────┘
              (Notion DB, Tool)
```

**Flow buổi 7 — Agent Tư vấn:**

```
              [Astra DB RAG (Tool)] ──┐
                                      │
              [Web Search (Tool)] ────┤
                                      ▼
[Chat Input] → [Agent Tư vấn] ──→ [Chat Output]
                                      ▲
              [File (Tool)] ──────────┤
                                      │
              [Page Content Viewer] ──┘
              (Notion bundle, Tool)
```

### 3.3 Hướng dẫn từng bước

#### ━━━ BUỔI 6 (buổi 6 của khoá) — Structured Output ━━━

**Bước 1: Tạo Notion database "Bảng tuyển sinh"**

1. Mở Notion workspace → tạo page mới `Bảng tuyển sinh`.
2. Trong page, gõ `/database` → chọn **Table - Inline** (hoặc Full page).
3. Đặt tên database: `Bảng tuyển sinh`.
4. Thiết lập 5 cột:
    - Cột 1: `Trường` — property type: **Title** (mặc định — rename từ "Name").
    - Cột 2: `Ngành` — thêm cột mới → type **Select**.
    - Cột 3: `Tổ hợp môn` — type **Text**.
    - Cột 4: `Điểm chuẩn tham khảo` — type **Number**.
    - Cột 5: `Học bổng` — type **Text**.
5. **Share database với Integration**: click "..." ở góc phải trên → **Connections** → chọn integration đã tạo ở buổi 2 → Confirm.
6. Lấy **Database ID** từ URL: `https://notion.so/workspace/...?v=<VIEW_ID>` — phần 32 ký tự hex trước `?v=` chính là database ID. Copy lưu lại.

Kết quả mong đợi: database có 5 cột đúng type, đã share integration, có database ID.

**Bước 2: Mở lại flow Agent Thông tin từ buổi 5**

- Mở flow `Agent_Thong_Tin_Tuyen_Sinh` từ buổi 5 (hoặc clone ra để giữ bản gốc). Đặt tên `Agent_Thong_Tin_Full`.
- Flow hiện có: Agent + 2 tool (Astra DB RAG + Web Search).

**Bước 3: Thêm Structured Output ở Tool Mode**

1. Kéo **Structured Output** (Processing) vào canvas.
2. Cấu hình:
    - **Schema** (dán JSON):

```json
{
  "truong": "string — tên đầy đủ",
  "nganh": "string — tên ngành",
  "to_hop_mon": "string — các tổ hợp, ví dụ 'A00, A01'",
  "diem_chuan_tham_khao": "number — điểm chuẩn năm gần nhất, ví dụ 27.5",
  "hoc_bong": "string — mô tả ngắn về học bổng, nếu không có ghi 'Không có'"
}
```

3. Bật **Tool Mode**.
    - **Tool Name:** `extract_tuyen_sinh_fields`
    - **Tool Description:**

```
Trích xuất 5 trường có cấu trúc (trường, ngành, tổ hợp, điểm chuẩn, học bổng) từ một đoạn text thô về tuyển sinh. Input: text_thoi. Output: JSON đúng schema. Dùng tool này khi cần chuyển text thô thành dữ liệu có cấu trúc.
```

**Bước 4: Thêm Create Page (cho database) ở Tool Mode**

1. Kéo thêm 1 **Create Page** (Notion bundle) vào canvas — cái mới, khác với Create Page buổi 2.
2. Cấu hình:
    - **Notion Secret:** Integration Token.
    - **Parent type:** Database (chọn đúng mode Database, không phải Page).
    - **Database ID:** dán database ID ở Bước 1.
3. Bật **Tool Mode**.
    - **Tool Name:** `them_hang_bang_tuyen_sinh`
    - **Tool Description:**

```
Thêm 1 hàng mới vào Notion database "Bảng tuyển sinh". Input là 5 giá trị tương ứng với 5 cột: truong (Title), nganh (Select), to_hop_mon (Text), diem_chuan_tham_khao (Number), hoc_bong (Text). Dùng SAU KHI đã có dữ liệu JSON từ extract_tuyen_sinh_fields.
```

**Bước 5: Cập nhật Agent Instructions**

Thêm phần trích xuất vào Agent Instructions (giữ nguyên phần fallback RAG → Web Search từ buổi 5):

```
[Giữ nguyên toàn bộ Instructions fallback RAG → Web Search từ buổi 5]

---
CHỨC NĂNG BỔ SUNG: Trích xuất bảng có cấu trúc
---
Khi user yêu cầu "thu thập", "trích xuất", "lập bảng", "ghi vào bảng tuyển sinh" cho một/nhiều trường:

1. Gọi rag_tuyen_sinh để lấy text thô về trường đó.
2. Với mỗi NGÀNH xuất hiện trong text, GỌI extract_tuyen_sinh_fields để trích xuất 5 trường.
3. NGAY SAU ĐÓ, gọi them_hang_bang_tuyen_sinh để ghi hàng vào Notion.
4. Lặp cho đến khi hết các ngành. Cuối cùng báo cáo user: "Đã thêm N ngành của trường X vào bảng".
5. Nếu trường nào không có trong RAG, xử lý theo chiến lược fallback (hỏi user có search web không).
```

**Bước 6: Nối các component**

- `extract_tuyen_sinh_fields` (Tool) → Agent (Tools port).
- `them_hang_bang_tuyen_sinh` (Tool) → Agent (Tools port).
- Agent giữ nguyên 2 tool cũ (RAG + Web Search).
- Tổng: Agent có **4 tool**.

**Bước 7: Test với 3 trường**

Mở Playground. Chạy lần lượt 3 prompt:

1. "Thu thập thông tin các ngành của trường ĐH Bách Khoa Hà Nội vào bảng tuyển sinh."
2. "Tiếp tục với ĐH KHTN TPHCM."
3. "Tiếp tục với ĐH Kinh tế Quốc dân."

Sau mỗi prompt, mở Notion → refresh database "Bảng tuyển sinh" → xác nhận có thêm hàng mới.

Kết quả mong đợi: ≥ 9 hàng (3 trường × ~3 ngành mỗi trường), mỗi hàng đủ 5 cột.

#### ━━━ BUỔI 7 (buổi 7 của khoá) — Agent Tư vấn ━━━

**Bước 8: Tạo flow Agent Tư vấn**

1. New Flow `Blank Flow` → tên `Agent_Tu_Van`.
2. Kéo vào canvas: `Chat Input`, `Agent`, `Chat Output`, `Astra DB` (Tool), `Web Search` (Tool), `File` (Tool), `Page Content Viewer` (Notion bundle, Tool).

**Bước 9: Cấu hình Agent**

- **Model Provider:** Google.
- **Model Name:** Gemma hoặc Gemini Flash.
- **API Key:** dán API key Gemma 4.

**Bước 10: Cấu hình 4 tool**

**Tool 1 — Astra DB RAG (Tool Mode, Retrieve):**

- Collection: `tuyen_sinh_dai_hoc`, token + endpoint.
- Number of Search Results: `4`.
- **Tool Name:** `rag_tuyen_sinh`
- **Tool Description:** "Tra cứu dữ liệu tuyển sinh nội bộ — trường, ngành, điểm chuẩn, học bổng. Dùng khi cần dẫn chứng cụ thể."

**Tool 2 — Web Search (Tool Mode):**

- **Tool Name:** `web_search_tuyen_sinh`
- **Tool Description:** "Tìm thông tin tuyển sinh mới trên web chính thống. CHỈ DÙNG SAU KHI rag_tuyen_sinh không đủ VÀ user đồng ý."

**Tool 3 — File (Tool Mode):**

- **Tool Name:** `doc_file_ban_hoi`
- **Tool Description:** "Đọc nội dung file bảng hỏi HS (PDF/TXT/Markdown) mà user đính kèm. Dùng khi user gửi file chứa câu trả lời bảng hỏi cá nhân hoá."

**Tool 4 — Page Content Viewer (Notion bundle, Tool Mode):**

- **Notion Secret:** Integration Token.
- **Tool Name:** `doc_page_notion_ban_hoi`
- **Tool Description:** "Đọc nội dung một page Notion khi user cung cấp page_id hoặc link. Dùng khi user chia sẻ link Notion thay vì file."

**Bước 11: Viết Agent Instructions**

Dán vào ô Agent Instructions:

```
Bạn là "Agent Tư vấn tuyển sinh" — trợ lý đồng hành chọn ngành/trường cá nhân hoá cho HS THPT Việt Nam.

CÂU HỎI ĐẦU TIÊN (bắt buộc, khi user bắt đầu chat):
"Chào em! Em đã hoàn thành bảng hỏi cá nhân hoá từ Agent Chiến lược chưa?
- NẾU RỒI, em gửi cho anh/chị LINK Notion hoặc ĐÍNH KÈM FILE bảng hỏi nhé.
- NẾU CHƯA, anh/chị sẽ hỏi nhanh 8 câu cơ bản để hiểu em."

ĐIỀU HƯỚNG:

1) NẾU user gửi LINK Notion (dạng https://notion.so/...):
   - Gọi tool doc_page_notion_ban_hoi với page_id/URL.
   - Đọc nội dung → chuyển sang BƯỚC TƯ VẤN.

2) NẾU user đính kèm FILE (PDF/TXT/Markdown):
   - Gọi tool doc_file_ban_hoi để đọc.
   - Chuyển sang BƯỚC TƯ VẤN.

3) NẾU user nói "chưa có" / không gửi:
   - Hỏi lần lượt 8 câu (MỖI LẦN 1 CÂU, chờ user trả lời):
     1. Em đang học lớp mấy? Khối nào?
     2. Em muốn học ở khu vực nào? (Hà Nội / TP.HCM / không giới hạn)
     3. Môn nào em thích nhất?
     4. Môn nào em tự tin nhất?
     5. Em có sở thích hoặc hoạt động ngoại khoá gì?
     6. Em đã có dự tính học ngành/lĩnh vực gì?
     7. Điểm trung bình / điểm thi thử khoảng bao nhiêu?
     8. Có yếu tố nào quan trọng khác không? (học bổng, học phí, ký túc xá...)
   - Sau câu 8 → chuyển sang BƯỚC TƯ VẤN.

BƯỚC TƯ VẤN:
- Dựa trên thông tin HS đã thu thập, gọi rag_tuyen_sinh để tra cứu ngành/trường/điểm chuẩn/học bổng PHÙ HỢP.
- Nếu RAG thiếu → xin phép user search web (giống Agent Thông tin buổi 5).
- OUTPUT theo ĐÚNG TEMPLATE sau:

## Hồ sơ tóm tắt
[3-5 câu tóm lại đặc điểm HS]

## 3-5 ngành phù hợp
1. [Ngành] — lý do phù hợp với HS này.
...

## 3-5 trường phù hợp
| Trường | Ngành cụ thể | Điểm chuẩn tham khảo | Ghi chú |
| ... | ... | ... | ... |

## Kế hoạch học tập gợi ý
- Môn cần tập trung: ...
- Phương thức xét tuyển nên chọn: ...
- Học bổng có thể nhắm đến: ...

## Dẫn nguồn
- Nguồn RAG: [ghi rõ trường, năm]
- Nguồn Web (nếu có): [link]

NGUYÊN TẮC:
- KHÔNG bịa điểm chuẩn/ngành. Nếu không có dữ liệu, nói thẳng.
- Tư vấn phải DỰA TRÊN hồ sơ cụ thể của HS, không nói chung chung.
- KHI tư vấn, BẮT BUỘC tham chiếu TRỰC TIẾP đến ÍT NHẤT 3 chi tiết cụ thể trong hồ sơ HS (ví dụ: "vì em tham gia CLB môi trường", "vì em đã code game Unity") ở phần 'lý do phù hợp'.
- Ngôn ngữ: nhẹ nhàng, khích lệ, xưng "anh/chị" với HS.
```

**Bước 12: Nối các component**

- 4 tool → Agent (Tools port).
- `Chat Input → Agent → Chat Output`.

**Bước 13: Test end-to-end với 3 hồ sơ**

Chuẩn bị 3 hồ sơ khác biệt:

**Hồ sơ A (có link Notion — dùng Page Content Viewer):**

- Paste link Notion page bảng hỏi của HS A (từ buổi 2 — khối B, thích Sinh học, CLB môi trường).
- Quan sát agent gọi `doc_page_notion_ban_hoi`, đọc, rồi tư vấn.

**Hồ sơ B (có file PDF — dùng File):**

- Đính kèm file PDF bảng hỏi của HS B (khối A, thích Toán, code game Unity).
- Quan sát agent gọi `doc_file_ban_hoi`.

**Hồ sơ C (không có — agent hỏi 8 câu):**

- Bắt đầu chat với câu "chưa có bảng hỏi".
- Agent phải hỏi 8 câu. Trả lời theo hồ sơ: khối D, thích Văn, viết blog, muốn du học.

Ghi kết quả vào phiếu so sánh:

| Tiêu chí | HS A (Link Notion) | HS B (File PDF) | HS C (Hỏi 8 câu) |
|---|---|---|---|
| Agent chọn đúng nhánh điều hướng? | | | |
| 3-5 ngành có khác biệt rõ rệt giữa 3 HS? | | | |
| Trường gợi ý có phù hợp khối/vùng HS? | | | |
| Kế hoạch học tập có cụ thể? | | | |
| Có dẫn nguồn? | | | |

**Bước 14: Đánh giá độ cá nhân hoá**

So sánh 3 output:

- Có phân biệt khối B / A / D trong lựa chọn trường không?
- Có nhắc đến đặc điểm riêng (CLB môi trường / code game / viết blog) trong phần "lý do phù hợp" không?
- Có trường nào bị lặp không phù hợp?

Nếu output **quá giống nhau giữa 3 HS** → sửa Agent Instructions thêm ràng buộc:

```
KHI tư vấn, BẮT BUỘC tham chiếu TRỰC TIẾP đến ÍT NHẤT 3 chi tiết cụ thể trong hồ sơ HS (ví dụ: "vì em tham gia CLB môi trường", "vì em đã code game Unity") ở phần 'lý do phù hợp'.
```

Chạy lại 1 hồ sơ để xác nhận prompt sửa hoạt động.

**Checklist debug cả 2 buổi:**

| Triệu chứng | Nguyên nhân | Giải pháp |
|---|---|---|
| Structured Output trả JSON sai format | Schema có lỗi cú pháp | Kiểm tra JSON hợp lệ (dấu ngoặc, dấu phẩy) |
| Create Page báo "database not found" | Chưa share integration hoặc Database ID sai | Quay lại Bước 1 kiểm tra |
| Agent không gọi extract_tuyen_sinh_fields | Tool Description mơ hồ | Viết rõ "Dùng khi cần chuyển text thô thành dữ liệu có cấu trúc" |
| Bảng Notion có hàng nhưng thiếu cột | Property type không khớp | Kiểm tra tên cột trong Notion khớp với schema |
| Agent Tư vấn không hỏi 8 câu khi user nói "chưa có" | Instructions thiếu nhánh điều kiện | Thêm khối NẾU user nói "chưa có" rõ ràng |
| Agent Tư vấn không đọc được link Notion | Page chưa share với integration | Share page bảng hỏi với integration |
| 3 tư vấn giống nhau | Prompt chưa yêu cầu cá nhân hoá | Thêm "BẮT BUỘC tham chiếu ÍT NHẤT 3 chi tiết cụ thể" |
| Agent bịa điểm chuẩn | Thiếu ràng buộc "KHÔNG bịa" | Thêm "KHÔNG BAO GIỜ bịa điểm chuẩn/ngành" |


---

## 4. Nâng cao

**Mở rộng A — Thêm cột thứ 6 vào bảng tuyển sinh**

Thêm cột `Năm` (Number) vào Notion database và cập nhật schema JSON thêm trường `"nam": "number — năm tuyển sinh"`. Chạy lại trích xuất — agent sẽ ghi thêm năm cho mỗi hàng. Hữu ích khi muốn so sánh điểm chuẩn qua các năm.

**Mở rộng B — Human-in-the-loop cho tư vấn**

Sửa Agent Instructions: "Sau khi sinh bản tư vấn, **trình bày cho user xem trước**. Hỏi 'Em có đồng ý với tư vấn này không? Có muốn điều chỉnh gì không?'. Chỉ khi user xác nhận mới kết thúc." Đây là pattern human-in-the-loop — cho HS quyền phản hồi trước khi nhận kết quả cuối.

**Mở rộng C — Agent Tư vấn song ngữ**

Yêu cầu Agent Tư vấn output bằng **cả tiếng Việt và tiếng Anh**. Phần tiếng Anh giúp HS có kế hoạch du học làm quen terminology (major, GPA, scholarship, admission method...).

**Mở rộng D — Trích xuất tất cả trường đã ingest**

Thay vì chạy từng trường, thử prompt: "Trích xuất thông tin tất cả các trường trong cơ sở dữ liệu vào bảng tuyển sinh." Agent cần gọi RAG nhiều lần, trích xuất nhiều ngành, ghi nhiều hàng. Đây là bài test stress cho flow.

**Mở rộng E — Kết hợp 3 agent trong 1 phiên**

Thử chạy tuần tự: Agent Chiến lược (tạo bảng hỏi) → HS trả lời → Agent Tư vấn (đọc bảng hỏi + tư vấn). Quan sát luồng end-to-end thực tế — từ câu hỏi đầu tiên đến bản tư vấn cuối cùng.

---

## 5. Câu hỏi ôn tập

1. **(Nhớ)** Kể tên 5 property types phổ biến trong Notion database.
2. **(Nhớ)** Component **Structured Output** trong Langflow cần input gì để hoạt động?
3. **(Hiểu)** Phân biệt dữ liệu **phi cấu trúc** và **có cấu trúc**. Cho 1 ví dụ cùng nội dung ở 2 dạng.
4. **(Hiểu)** Tại sao cần **schema JSON** khi dùng Structured Output? Nếu không có schema, LLM trả lời khác gì?
5. **(Hiểu)** Giải thích pattern "điều hướng theo điều kiện" trong Agent Instructions. Tại sao viết dạng pseudocode (NẾU/THÌ/NGƯỢC LẠI) tốt hơn viết văn xuôi?
6. **(Áp dụng)** Bạn muốn thêm cột "Phương thức xét tuyển" vào bảng tuyển sinh. Hãy: (a) chọn property type phù hợp, (b) thêm trường vào schema JSON, (c) cập nhật Tool Description của `extract_tuyen_sinh_fields`.
7. **(Áp dụng)** Viết 1 đoạn Agent Instructions cho Agent Tư vấn để xử lý tình huống: "User gửi link Notion nhưng page không có nội dung (rỗng)." Agent nên làm gì?
8. **(Phân tích)** Bạn A test Agent Tư vấn với 3 hồ sơ HS khác nhau nhưng cả 3 output đều gợi ý "ngành CNTT, trường Bách Khoa". Phân tích: vấn đề nằm ở đâu? Liệt kê 3 nguyên nhân có thể và cách sửa cho từng nguyên nhân.
9. **(Đánh giá)** So sánh 2 cách đọc bảng hỏi: File vs. Page Content Viewer. Trong trường hợp nào nên dùng cách nào? Nếu chỉ được chọn 1, bạn chọn cách nào và vì sao?
10. **(Sáng tạo)** Ngoài tư vấn tuyển sinh, hãy thiết kế 1 hệ thống 3 agent cho một lĩnh vực khác (ví dụ: tư vấn sức khoẻ, tư vấn du lịch, tư vấn nghề nghiệp). Mô tả ngắn: mỗi agent làm gì, dùng tool gì, dữ liệu lưu ở đâu.

---

## 6. Thuật ngữ

| Thuật ngữ | Giải thích |
|---|---|
| **Dữ liệu phi cấu trúc** | Text thô, văn xuôi — không có format cố định. Ví dụ: đoạn văn mô tả đề án tuyển sinh. Dễ cho AI đọc, khó cho người so sánh nhanh. |
| **Dữ liệu có cấu trúc** | Dữ liệu theo format cố định (bảng, JSON). Ví dụ: bảng 5 cột trên Notion. Dễ cho người đọc, lọc, so sánh. |
| **Structured Output** | Component Langflow cho phép định nghĩa schema JSON và LLM trả về đúng format đó. Giúp output ổn định, không mỗi lần một khác. |
| **Schema JSON** | Khuôn mẫu định nghĩa các trường dữ liệu cần trích xuất (tên, kiểu, mô tả). LLM đọc schema và điền giá trị vào. |
| **Property type** | Kiểu dữ liệu của một cột trong Notion database: Title, Text, Select, Multi-select, Number, Date... |
| **Title** | Property type bắt buộc trong Notion database — mỗi database phải có đúng 1 cột Title làm định danh hàng. |
| **Select** | Property type cho phép chọn 1 giá trị từ danh sách cố định. Dễ lọc, gọn. |
| **Rich Text** | Property type cho phép nhập text tự do, không giới hạn. |
| **Number** | Property type lưu số — cho phép sort, filter theo giá trị. |
| **Database ID** | Chuỗi 32 ký tự hex định danh một Notion database. Lấy từ URL. |
| **Page Content Viewer** | Notion bundle component đọc nội dung một page Notion qua Integration. Cần page_id và Notion Secret. |
| **File** | Component Langflow đọc nội dung file (PDF, TXT, Markdown) mà user đính kèm. |
| **Điều hướng theo điều kiện** | Pattern viết Agent Instructions dạng NẾU/THÌ/NGƯỢC LẠI để agent xử lý nhiều tình huống khác nhau. |
| **Template output** | Khuôn mẫu cố định cho output của agent (ví dụ: 4 section — Hồ sơ, Ngành, Trường, Kế hoạch). Giúp output ổn định, dễ so sánh. |
| **End-to-end testing** | Kiểm thử luồng hoàn chỉnh từ đầu đến cuối — chạy cả hệ thống (không chỉ 1 component) với dữ liệu thực. |
| **Cá nhân hoá (personalization)** | Output khác nhau cho mỗi người dùng, dựa trên dữ liệu riêng của họ. 3 HS khác nhau → 3 tư vấn khác nhau. |
| **Human-in-the-loop** | Pattern thiết kế: agent hỏi xác nhận user trước khi thực hiện hành động quan trọng hoặc trả kết quả cuối. |
| **Hallucination** | Hiện tượng LLM "bịa" thông tin nghe có lý nhưng không đúng. Giảm bằng RAG + prompt ràng buộc "KHÔNG bịa". |
| **Fallback** | Chiến lược dự phòng: khi công cụ chính (RAG) không đủ, chuyển sang công cụ phụ (Web Search) sau khi xin phép user. |