# Dự án 3: Agent Tư vấn cá nhân hoá

Khoá: Xây dựng hệ thống AI Agent tư vấn tuyển sinh với Langflow.

Cấp học: THPT.

Thời lượng: 2 buổi x 90 phút.

## Mô tả bài học

Trong bài học này, học sinh sẽ tìm hiểu kiến thức về **Structured Output** (trích xuất JSON theo schema), **Notion database có cấu trúc nhiều cột**, logic điều hướng agent theo điều kiện (có file / không có file), và **testing end-to-end** một hệ thống nhiều agent. Học sinh vận dụng để hoàn thiện **Agent Thông tin tuyển sinh** (lập bảng có cấu trúc 5 cột trên Notion) và xây dựng **Agent Tư vấn cá nhân hoá** — agent cuối cùng kết hợp dữ liệu của 2 agent trước để đưa ra tư vấn riêng cho từng học sinh.

## I. Yêu cầu cần đạt

### **1\. Kiến thức**

Thực hiện bài học này học sinh sẽ khám phá các kiến thức:

- Phân biệt dữ liệu **phi cấu trúc** (text thô) và **có cấu trúc** (bảng, JSON) — khi nào dùng cái nào.
- Component **Structured Output** — định nghĩa schema JSON và để LLM điền vào.
- Thiết kế schema JSON với 5 trường: Trường, Ngành, Tổ hợp môn, Điểm chuẩn tham khảo, Học bổng.
- Property types của Notion (Title, Text, Select, Number) — chọn đúng kiểu cột.
- Notion bundle component **Create Page** để ghi từng hàng vào Notion database.
- Component **File** để đọc file bảng hỏi HS đã trả lời.
- System prompt điều hướng theo điều kiện: "có file → đọc file; không có file → hỏi 8 câu".
- Tích hợp Agent Tư vấn với nhiều tool: **Astra DB RAG + Web Search + Notion bundle (Page Content Viewer / List Pages)**.
- Template output tư vấn: 3-5 ngành + 3-5 trường + kế hoạch học tập.
- **Testing end-to-end** — kiểm thử luồng 3 agent phối hợp với nhiều hồ sơ học sinh khác nhau.

### **2\. Năng lực**

Thực hiện bài học này học sinh sẽ rèn luyện và phát triển các năng lực với các biểu hiện:

*Năng lực đặc thù:*

- Tạo Notion database "Bảng tuyển sinh" với 5 cột đúng property type, chia sẻ với Integration.
- Xây dựng flow Agent trích xuất: `Astra DB → Structured Output → Create Page (Notion)` ghi nhiều hàng.
- Hoàn thiện Agent Thông tin tuyển sinh phiên bản đầy đủ (có cả tính năng lập bảng có cấu trúc).
- Xây dựng Agent Tư vấn với logic "có file → đọc; không có → hỏi 8 câu".
- Tích hợp Agent Tư vấn với Astra DB (RAG), Web Search, Notion bundle.
- Viết system prompt định hướng output theo template 3-5 ngành + 3-5 trường + kế hoạch.
- Thực hiện test end-to-end với ít nhất 3 hồ sơ HS khác biệt, đánh giá độ cá nhân hoá.

*Năng lực chung:*

- Giao tiếp, làm việc nhóm khi test hồ sơ giả định (một bạn đóng user, một bạn quan sát).
- Sáng tạo trong thiết kế template tư vấn.
- Giải quyết vấn đề khi debug luồng nhiều agent.
- Tư duy hệ thống — nhìn ra 3 agent phối hợp như thế nào.

### **3\. Phẩm chất**

Bài học này tạo điều kiện để học sinh phát triển những phẩm chất với các biểu hiện:

- **Trách nhiệm** khi thiết kế output tư vấn — hiểu rằng tư vấn sai có thể ảnh hưởng lớn đến người dùng.
- **Trung thực** khi đối chiếu output với dữ liệu thực tế, không "làm màu" kết quả.
- **Nhân ái** khi đặt mình vào vị trí HS nhận tư vấn — viết prompt để agent nói nhẹ nhàng, có giải thích.
- **Chăm chỉ** qua quá trình test end-to-end nhiều hồ sơ, sửa prompt nhiều lần.

## II. Sản phẩm

**Cuối buổi 1 (buổi 6 của khoá):**

- 1 Notion database **"Bảng tuyển sinh"** với 5 cột đúng property type (Title: Trường; Select: Ngành; Text: Tổ hợp môn; Number: Điểm chuẩn tham khảo; Text: Học bổng), đã share với Integration.
- Flow Agent **hoàn thiện Agent Thông tin**: `Astra DB (Retrieve) → Structured Output (schema 5 trường) → Create Page (Notion)` — chạy được trên ít nhất **3 trường**, ghi nhiều hàng vào Notion database (mỗi ngành 1 hàng).

**Cuối buổi 2 (buổi 7 của khoá):**

- Flow **Agent Tư vấn** hoàn chỉnh với logic điều kiện "có file bảng hỏi → đọc file; không có → hỏi 8 câu", tích hợp Astra DB RAG + Web Search + Notion bundle (Page Content Viewer / List Pages).
- Agent Tư vấn output theo đúng template: **3-5 ngành phù hợp + 3-5 trường phù hợp + kế hoạch học tập** cụ thể.
- Báo cáo test **end-to-end với 3 hồ sơ HS khác biệt** → 3 tư vấn khác nhau rõ rệt (phiếu so sánh điền xong).

## III. Thiết bị dạy học và học liệu

Các nguyên vật liệu cần chuẩn bị cho một học sinh:

| Nguyên vật liệu | Đơn vị | Số lượng |
| :-- | :-: | :-: |
| File/link bảng hỏi cá nhân hoá đã trả lời (từ Notion, buổi 2) — ít nhất 1 bảng | File | 1-3 |
| Sổ/note ghi chú test end-to-end | Cuốn | 1 |

Các công cụ và thiết bị cần chuẩn bị cho một nhóm:

| Công cụ và thiết bị | Đơn vị | Số lượng |
| :-- | :-: | :-: |
| Máy tính có internet, đã cài Langflow Desktop | Cái | 1 |
| API key Gemma 4 (đã có) | Tài khoản | 1 |
| AstraDB đã có dữ liệu 5+ trường từ dự án 2 | Tài khoản | 1 |
| Notion + Integration Token (đã có) | Tài khoản | 1 |

Các tài liệu, phiếu học tập:

| Đường dẫn tài liệu, phiếu học tập, video,... |
| :-- |
| Tài liệu Langflow Structured Output: `https://docs.langflow.org/components-processing#structured-output` |
| Tài liệu Langflow Notion bundle — Create Page: `https://docs.langflow.org/bundles-notion#create-page` |
| Tài liệu Langflow Notion bundle — Page Content Viewer / List Pages: `https://docs.langflow.org/bundles-notion` |
| 3 hồ sơ HS mẫu để test end-to-end (GV chuẩn bị — dán vào phiếu) |
| Phiếu so sánh 3 tư vấn end-to-end (buổi 7) |
| Rubric đánh giá Agent Tư vấn (độ cá nhân hoá + độ chính xác + chất lượng trình bày) |

## IV. Tiến trình dạy học


## BUỔI 1 (buổi 6 của khoá) — Structured Output: hoàn thiện Agent Thông tin tuyển sinh

### **1\. Xác định vấn đề (15 phút)**

#### **1.1. Mục tiêu**

- HS nhận ra giới hạn của dữ liệu **phi cấu trúc** (text thô trong AstraDB): đủ để agent trả lời câu hỏi, nhưng không thể gửi cho phụ huynh/HS xem nhanh.
- Xác định nhiệm vụ buổi 6: thiết kế 1 **bảng có cấu trúc 5 cột** trên Notion (Trường, Ngành, Tổ hợp môn, Điểm chuẩn, Học bổng), dùng Agent + Structured Output để trích xuất tự động.
- Hiểu bối cảnh: "ban tuyển sinh của trường cần 1 bảng tổng hợp dễ đọc" → bài toán thực tế.

#### **1.2. Tổ chức thực hiện**

**1.2.1. Khởi động — Case study "Ban tuyển sinh của trường A" (5 phút)**

*Chiến lược dẫn dắt: Case study / Tình huống thực tế.*

- GV chiếu/kể tình huống:

  > "Cô H là trưởng ban tuyển sinh của trường THPT. Năm ngoái, cô phải ngồi đọc tay 20 đề án tuyển sinh PDF dài 50 trang mỗi cái, rồi copy-paste vào một bảng Excel để gửi cho phụ huynh. Mất 2 tuần. Năm nay cô hỏi em: 'AI giúp cô tự động được không?'. Em trả lời thế nào?"

- GV đặt câu hỏi mở cho cả lớp trong 2 phút:
    - Agent Thông tin tuyển sinh của các em (dự án 2) có giúp được cô H không?
    - Nếu có, nó giúp ra sao? Nếu không đủ, còn thiếu gì?
- Dẫn dắt: Agent đang trả lời tốt câu hỏi lẻ, nhưng **không tự lập bảng** được. Đây là nhiệm vụ của hôm nay.

**1.2.2. Phi cấu trúc vs. có cấu trúc — 2 dạng dữ liệu (5 phút)**

*Chiến lược dẫn dắt: So sánh đối chiếu.*

- GV chiếu 2 ví dụ cùng nội dung, 2 dạng khác nhau:

    **Dạng A (phi cấu trúc):**
    > "Trường ĐH Bách Khoa Hà Nội năm 2024 tuyển sinh ngành Công nghệ Thông tin với tổ hợp A00, A01. Điểm chuẩn năm 2024 là 28.3. Trường có học bổng toàn phần cho top 10% thí sinh trúng tuyển..."

    **Dạng B (có cấu trúc):**

    | Trường | Ngành | Tổ hợp | Điểm chuẩn | Học bổng |
    | :-- | :-- | :-- | :-: | :-- |
    | ĐH Bách Khoa HN | Công nghệ Thông tin | A00, A01 | 28.3 | Học bổng toàn phần top 10% |

- GV hỏi: "Dạng nào dễ **đọc nhanh**? Dạng nào dễ **tìm kiếm/so sánh** giữa nhiều trường? Dạng nào **AI dễ sinh**, dạng nào **người đọc thích hơn**?"
- Chốt: hai dạng bổ sung nhau. Hôm nay ta sẽ **chuyển** dạng A (AstraDB) → dạng B (Notion).

**1.2.3. Câu hỏi dẫn dắt buổi (5 phút)**

- GV treo câu hỏi dẫn dắt:

  > "Làm thế nào để Agent Thông tin tuyển sinh **tự lập bảng 5 cột đẹp** trên Notion, thay vì chỉ trả lời văn xuôi khi được hỏi?"

- GV chốt lộ trình hôm nay:
    1. Thiết kế schema JSON 5 trường.
    2. Tạo Notion database với 5 cột đúng kiểu.
    3. Xây flow Agent + Structured Output + Create Page.
    4. Test trên 3 trường, xác nhận bảng lên đúng.

### **2\. Nghiên cứu kiến thức nền (20 phút)**

#### **2.1. Mục tiêu**

- Biết component **Structured Output** và cách định nghĩa schema.
- Biết các **property types** chính của Notion database (Title, Text, Select, Multi-select, Number).
- Biết component **Create Page** dùng ở chế độ "ghi vào database" (thay vì ghi vào parent page như buổi 2).

#### **2.2. Tổ chức thực hiện**

**2.2.1. Structured Output — khuôn mẫu cho LLM (10 phút)**

*Chiến lược dẫn dắt: Phép tương tự + Demo.*

- GV dùng phép tương tự: "Bình thường em nói 'Cho tôi 1 bát phở', người bán đưa phở — nhưng 'bát phở' mỗi chỗ mỗi khác (nhiều rau, ít rau, bánh to bánh nhỏ). Nếu mình nói 'Cho tôi 1 bát phở gồm: 300g bánh, 100g thịt, 50g rau thơm, 1 viên chanh' — đó là **cấu trúc cụ thể**. LLM cũng vậy: không có schema → trả lời mỗi lần một khác; có schema → luôn ra đúng khuôn mẫu."
- GV chiếu schema JSON mẫu (làm chung với lớp):

    ```json
    {
      "truong": "string — tên đầy đủ của trường",
      "nganh": "string — tên ngành học",
      "to_hop_mon": "string — danh sách tổ hợp xét tuyển, ví dụ 'A00, A01'",
      "diem_chuan_tham_khao": "number — điểm chuẩn năm gần nhất",
      "hoc_bong": "string — mô tả ngắn về học bổng (nếu có)"
    }
    ```

- GV thao tác nhanh trong Langflow: tìm component **Structured Output** → chỉ vào ô "Output Schema" → giải thích "ở đây dán JSON schema, LLM sẽ trả về đúng format này".

**2.2.2. Notion property types — chọn đúng kiểu cho từng cột (10 phút)**

*Chiến lược dẫn dắt: Trò chơi xếp thẻ.*

- GV chuẩn bị trước 5 thẻ giấy: "Title", "Rich Text", "Select", "Multi-select", "Number".
- GV đọc tên 5 cột, mời HS xung phong ghép thẻ:
    - "Trường" → **Title** (mỗi hàng cần 1 cột title duy nhất để làm định danh).
    - "Ngành" → **Select** hoặc **Rich Text** (nhiều ngành lặp lại → Select gọn hơn).
    - "Tổ hợp môn" → **Rich Text** (tự do nhập nhiều tổ hợp, ngăn bằng dấu phẩy).
    - "Điểm chuẩn tham khảo" → **Number** (để lọc/sắp xếp được).
    - "Học bổng" → **Rich Text** (mô tả dài).
- GV giải thích ngắn:
    - **Title** (bắt buộc có 1 cột title mỗi database): dùng làm "tên hàng".
    - **Select**: chọn từ danh sách cố định, dễ lọc.
    - **Rich Text**: text tự do, phù hợp cho mô tả.
    - **Number**: số, để sort/filter theo giá trị.
- GV chốt: "Chọn đúng kiểu **ngay từ đầu** giúp Notion database tận dụng được filter/sort/view — nếu chọn Rich Text cho mọi cột, bảng hoạt động nhưng mất hầu hết tính năng."

### **3\. Đề xuất, lựa chọn giải pháp và lên kế hoạch (10 phút)**

#### **3.1. Mục tiêu**

- Vẽ được sơ đồ flow hoàn chỉnh cho nhiệm vụ trích xuất.
- Chia công việc: chọn 3 trường (đã ingest ở dự án 2) để test.

#### **3.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Checklist + Vẽ sơ đồ.*

- GV chiếu sơ đồ flow chuẩn:

    ```
    [Chat Input (prompt: "Thu thập thông tin các ngành của trường HUST")]
           │
           ▼
    [Agent] ─── tool 1 ──→ [Astra DB (Retrieve)] ── trả về text thô
       │   ─── tool 2 ──→ [Structured Output] ── trả về JSON 5 trường
       │   ─── tool 3 ──→ [Create Page (Notion DB)] ── ghi 1 hàng
       ▼
    [Chat Output]
    ```

- GV giải thích: agent sẽ **lặp** — với mỗi ngành trong text thô, gọi Structured Output → Create Page. Nhiều ngành = nhiều lần gọi.
- Mỗi nhóm vẽ sơ đồ vào note và chọn **3 trường** trong số 5+ trường đã ingest dự án 2.

### **4\. Chế tạo mẫu, thử nghiệm và đánh giá (40 phút)**

#### **4.1. Mục tiêu**

- Tạo được Notion database "Bảng tuyển sinh" đủ 5 cột đúng type, đã share Integration.
- Xây flow Agent hoàn chỉnh có Structured Output + Create Page.
- Trích xuất được dữ liệu của 3 trường → bảng có ≥ 9 hàng (3 trường × trung bình 3 ngành).

#### **4.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Làm mẫu từng bước (Modeling) + Phân hoá.*

**Bước 1 (10 phút) — Tạo Notion database "Bảng tuyển sinh":**

GV làm mẫu, HS làm theo song song:

1. Mở Notion workspace → tạo page mới `Bảng tuyển sinh` bên trong parent page tuỳ ý.
2. Trong page, gõ `/database` → chọn **Table - Inline** (hoặc Full page).
3. Đặt tên database: `Bảng tuyển sinh`.
4. Thiết lập 5 cột:
    - Cột 1: `Trường` — property type: **Title** (mặc định — rename từ "Name").
    - Cột 2: `Ngành` — thêm cột mới → type **Select**.
    - Cột 3: `Tổ hợp môn` — type **Text**.
    - Cột 4: `Điểm chuẩn tham khảo` — type **Number**.
    - Cột 5: `Học bổng` — type **Text**.
5. **Share database với Integration**: click "..." ở góc phải trên → **Connections** → chọn integration `Langflow Agent Chien Luoc` (hoặc tên em đã đặt) → Confirm.
6. Lấy **Database ID** từ URL: `https://notion.so/workspace/...?v=<VIEW_ID>` — phần 32 ký tự hex trước `?v=` chính là database ID. Copy lưu lại.

> ⚠️ **Checkpoint 1:** HS có (a) 5 cột đúng type, (b) database đã share integration, (c) database ID đã copy. GV đi vòng kiểm tra.

**Bước 2 (15 phút) — Xây flow Agent + Structured Output + Create Page:**

1. Trong Langflow, mở lại flow **Agent Thông tin** từ buổi 5 (hoặc clone ra để giữ bản gốc). Đặt tên `Agent_Thong_Tin_Full`.
2. Kéo thêm vào canvas: **Structured Output**, thêm 1 **Create Page** (cấu hình khác với Create Page của buổi 2 — lần này ghi vào database).
3. Cấu hình **Structured Output**:
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
    - Bật **Tool Mode**, Tool Name: `extract_tuyen_sinh_fields`.
    - Tool Description: "Trích xuất 5 trường có cấu trúc (trường, ngành, tổ hợp, điểm chuẩn, học bổng) từ một đoạn text thô về tuyển sinh. Input: text_thoi. Output: JSON đúng schema."
4. Cấu hình **Create Page** (cái mới cho database):
    - Notion Secret: Integration Token.
    - **Parent type**: Database (chọn đúng mode Database, không phải Page).
    - **Database ID**: dán database ID ở Bước 1.
    - Bật **Tool Mode**, Tool Name: `them_hang_bang_tuyen_sinh`.
    - Tool Description:
        ```
        Thêm 1 hàng mới vào Notion database "Bảng tuyển sinh". Input là 5 giá trị tương ứng với 5 cột: truong (Title), nganh (Select), to_hop_mon (Text), diem_chuan_tham_khao (Number), hoc_bong (Text). Dùng SAU KHI đã có dữ liệu JSON từ extract_tuyen_sinh_fields.
        ```
5. Cập nhật **Agent Instructions** thêm phần trích xuất:

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

6. Nối:
    - `extract_tuyen_sinh_fields` (Tool) → Agent.
    - `them_hang_bang_tuyen_sinh` (Tool) → Agent.
    - Agent giữ nguyên 2 tool cũ (RAG + Web Search).

> ⚠️ **Checkpoint 2:** GV đi vòng kiểm tra: tất cả 4 tool đều ở Tool Mode, database ID đã dán đúng, schema không có lỗi JSON.

**Bước 3 (10 phút) — Test với 3 trường:**

- Playground. Chạy lần lượt 3 prompt:
    1. "Thu thập thông tin các ngành của trường ĐH Bách Khoa Hà Nội vào bảng tuyển sinh."
    2. "Tiếp tục với ĐH KHTN TPHCM."
    3. "Tiếp tục với ĐH Kinh tế Quốc dân."
- Sau mỗi prompt, mở Notion → refresh database "Bảng tuyển sinh" → xác nhận có thêm hàng mới.
- Kết quả mong đợi: ≥ 9 hàng (3 trường × ~3 ngành mỗi trường).

**Bước 4 (5 phút) — Phân hoá:**

- **HS nhanh:**
    - Thêm cột thứ 6 vào database: `Năm` (Number), thêm vào schema, thử trích xuất thêm.
    - Hoặc: thử yêu cầu agent trích xuất cho **tất cả 5 trường** đã ingest.
- **HS cần hỗ trợ:**
    - Sai JSON schema → GV sửa chung 1 lượt trên màn chiếu.
    - Create Page báo "database not found" → kiểm tra lại: đã share integration chưa? Database ID đúng chưa?

### **5\. Chia sẻ, thảo luận và điều chỉnh (3 phút)**

#### **5.1. Mục tiêu**

- Chia sẻ nhanh kết quả: số hàng đã tạo, trường nào có khó khăn.
- Nhận xét chéo về chất lượng trích xuất (điểm chuẩn có đúng không, ngành có sót không).

#### **5.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Gallery nhanh.*

- GV mở screen share Notion database của 2-3 nhóm lên, cả lớp quan sát 30 giây mỗi bảng.
- Hỏi nhanh: "Có sai sót nào rõ ràng không? Ngành nào bị sót/sai điểm?"
- Ghi nhận vấn đề để buổi sau sửa (nếu cần quay lại ingest thêm).

### **6\. Tổng kết (2 phút)**

*Chiến lược dẫn dắt: Cliffhanger + Tóm tắt hệ thống.*

- GV tổng kết:

  > "Hôm nay chúng ta đã **hoàn thiện Agent Thông tin tuyển sinh** — phiên bản đầy đủ, có cả tính năng trích xuất thành bảng có cấu trúc. 2 trong 3 agent của hệ thống đã xong (Agent Chiến lược ở dự án 1, Agent Thông tin ở dự án 2-3)."

- **Cliffhanger:**

  > "Buổi sau: agent cuối cùng — **Agent Tư vấn** — sẽ **kết hợp cả 2 agent trước**: đọc bảng hỏi cá nhân hoá (do Agent Chiến lược tạo ra), tra cứu bảng tuyển sinh (do Agent Thông tin lập), rồi đưa ra **tư vấn riêng** cho mỗi HS. 3 agent phối hợp cuối cùng thành 1 hệ thống. Hãy mang theo link bảng hỏi từ Notion (buổi 2) để sẵn sàng test."

## HẾT BUỔI 1 (buổi 6 của khoá)


## BUỔI 2 (buổi 7 của khoá) — Agent Tư vấn & testing end-to-end

HS ổn định, chào hỏi GV, kiểm tra Notion database "Bảng tuyển sinh" của buổi 6.

### **1\. Ôn tập (10 phút)**

*Chiến lược dẫn dắt: Think-Pair-Share + Kết nối bài trước.*

- GV mở một Notion "Bảng tuyển sinh" của nhóm đại diện → cả lớp cùng nhìn, xác nhận data đã có.
- **Think (1 phút):** Mỗi HS viết nhanh vào note:
    - "Đến giờ hệ thống của em có 2 agent: Agent Chiến lược, Agent Thông tin. Mỗi agent tạo ra **dữ liệu gì** trên Notion?"
- **Pair (2 phút):** chia sẻ bạn cùng bàn.
- **Share (5 phút):** GV gọi 2-3 HS trả lời. Vẽ sơ đồ lên bảng:

    ```
    Agent Chiến lược ──→ Notion: "Bảng hỏi học sinh" (mỗi HS 1 page)
    Agent Thông tin ──→ Notion: "Bảng tuyển sinh" (mỗi ngành 1 hàng)
                     ──→ AstraDB: text thô 5+ trường
    ```

- **Đặt vấn đề buổi 7 (2 phút):**

  > "Bạn An — HS lớp 12 — đã trả lời bảng hỏi của Agent Chiến lược trên Notion. Làm sao để Agent Tư vấn **đọc được bảng hỏi của An, tra được bảng tuyển sinh**, rồi đưa ra 3-5 ngành + 3-5 trường phù hợp với chính An (khác với bạn Bình, bạn Cường)? Đó là việc hôm nay."

- Cliff mở rộng: "Và cái khó là: agent phải biết **2 tình huống**: (1) An có file bảng hỏi sẵn; (2) Bạn Y không có — phải hỏi nhanh. Làm sao 1 agent xử lý cả 2 tình huống?"

### **2\. Nghiên cứu kiến thức nền (15 phút)**

#### **2.1. Mục tiêu**

- Biết component **File** để đọc file bảng hỏi (PDF/TXT download từ Notion, hoặc paste link).
- Biết Notion bundle component **Page Content Viewer** / **List Pages** để đọc page Notion trực tiếp.
- Hiểu pattern **"điều hướng theo điều kiện"** trong system prompt.
- Biết template output tư vấn (3-5 ngành + 3-5 trường + kế hoạch học tập).

#### **2.2. Tổ chức thực hiện**

**2.2.1. Làm sao để agent đọc bảng hỏi? — 2 cách (7 phút)**

*Chiến lược dẫn dắt: So sánh giải pháp.*

- GV chiếu bảng so sánh 2 cách:

    | Cách | Cách dùng | Ưu điểm | Nhược điểm |
    | :-- | :-- | :-- | :-- |
    | **A. Component File** | User download page Notion dưới dạng PDF/Markdown → đính kèm vào flow | Đơn giản, không cần kết nối Notion | User phải download thủ công |
    | **B. Notion Page Content Viewer** | Agent đọc trực tiếp page Notion qua Integration | Tự động, không cần file | Phải biết page_id, cần quyền share |

- GV chốt: dự án này dùng **cả 2** cho linh hoạt. Ưu tiên cách B khi có sẵn link Notion, fallback cách A khi user upload file rời.

**2.2.2. Điều hướng theo điều kiện trong Agent Instructions (4 phút)**

*Chiến lược dẫn dắt: Làm mẫu (Modeling).*

- GV chiếu prompt mẫu có khối điều kiện `NẾU ... THÌ ... NGƯỢC LẠI`:

    ```
    KHI bắt đầu cuộc hội thoại, luôn hỏi user CÂU ĐẦU TIÊN:
    "Em đã làm bảng hỏi cá nhân hoá từ Agent Chiến lược chưa? Nếu có, em chia sẻ link Notion hoặc đính kèm file cho anh/chị."

    NẾU user gửi link Notion hoặc file:
    - Dùng tool đọc_ban_hoi để lấy nội dung.
    - Phân tích câu trả lời, sau đó sang bước Tư vấn.

    NẾU user nói "chưa có" hoặc không gửi:
    - Hỏi lần lượt 8 CÂU MỞ ĐẦU (giống Agent Chiến lược):
      1. Em đang học lớp mấy, khối nào?
      2. ...
    - Sau khi đủ 8 câu, sang bước Tư vấn.

    BƯỚC TƯ VẤN (sau khi có đủ thông tin HS):
    - Gọi rag_tuyen_sinh + them_hang_bang_tuyen_sinh (nếu cần) để có dữ liệu.
    - Đưa ra 3-5 ngành + 3-5 trường + kế hoạch học tập theo template.
    ```

- GV nhấn mạnh: **từ khoá NẾU / NGƯỢC LẠI / BƯỚC** giúp LLM đọc và làm đúng logic. Viết prompt với cấu trúc rõ như code pseudocode.

**2.2.3. Template output tư vấn (4 phút)**

- GV chiếu template chuẩn (HS copy vào prompt lát nữa):

    ```
    ## Hồ sơ tóm tắt
    [3-5 câu tóm lại đặc điểm HS: lớp, khối, thế mạnh, sở thích, ràng buộc]

    ## 3-5 ngành phù hợp
    1. [Tên ngành] — lý do phù hợp với HS.
    2. ...

    ## 3-5 trường phù hợp
    | Trường | Ngành cụ thể | Điểm chuẩn tham khảo | Ghi chú |
    | :-- | :-- | :-: | :-- |
    | ... | ... | ... | ... |

    ## Kế hoạch học tập gợi ý
    - Môn cần tập trung: ...
    - Phương thức xét tuyển nên chọn: ...
    - Học bổng có thể nhắm đến: ...

    ## Dẫn nguồn
    - Nguồn RAG: ...
    - Nguồn Web (nếu có): [link]
    ```

- GV chốt: "Template cố định = tư vấn ổn định = dễ so sánh 3 HS. HS nào cũng ra 4 section, nhưng **nội dung khác nhau** tuỳ hồ sơ."

### **3\. Đề xuất, lựa chọn giải pháp và lên kế hoạch (5 phút)**

#### **3.1. Mục tiêu**

- Phác thảo sơ đồ flow Agent Tư vấn.
- Chuẩn bị 3 hồ sơ test (hoặc nhận 3 hồ sơ GV phát).

#### **3.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Checklist.*

- GV chiếu sơ đồ Agent Tư vấn:

    ```
                  [Astra DB RAG (Tool)] ──┐
                                          │
                  [Web Search (Tool)] ────┤
                                          ▼
    [Chat Input] → [Agent Tư vấn] ─────→ [Chat Output]
                                          ▲
                  [File (Tool)] ──────────┤
                                          │
                  [Page Content Viewer] ──┘
                  (Notion bundle, Tool)
    ```

- GV phát/chiếu **3 hồ sơ HS test end-to-end**:

    - **Hồ sơ 1 (có bảng hỏi đã trả lời — dùng cách B):** Link Notion đến page bảng hỏi của HS A (từ buổi 2 — khối B, thích Sinh học, CLB môi trường).
    - **Hồ sơ 2 (có file PDF — dùng cách A):** File bảng hỏi của HS B (khối A, thích Toán, code game Unity).
    - **Hồ sơ 3 (không có — agent phải hỏi 8 câu):** GV đóng vai HS C (khối D, thích Văn, viết blog).

- Mỗi nhóm phân vai: 1 người đóng user, 1 người quan sát agent + ghi chú.

### **4\. Chế tạo mẫu, thử nghiệm và đánh giá (50 phút)**

#### **4.1. Mục tiêu**

- Xây được flow Agent Tư vấn hoàn chỉnh với cả 4 tool.
- Test thành công với 3 hồ sơ khác biệt → 3 output tư vấn khác nhau.
- Đánh giá độ cá nhân hoá qua phiếu so sánh.

#### **4.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Scaffold từ flow đã có + Testing end-to-end.*

**Bước 1 (15 phút) — Xây flow Agent Tư vấn:**

1. New Flow `Blank Flow` → tên `Agent_Tu_Van`.
2. Kéo vào canvas: `Chat Input`, `Agent`, `Chat Output`, `Astra DB` (Tool), `Web Search` (Tool), `File` (Tool), `Page Content Viewer` (Notion bundle, Tool).
3. Cấu hình **Agent**: Provider Google, Model Gemma/Gemini, API key.
4. Cấu hình **Astra DB** (Tool Mode, Retrieve):
    - Token, endpoint, collection `tuyen_sinh_dai_hoc`.
    - Tool Name: `rag_tuyen_sinh`.
    - Tool Description: "Tra cứu dữ liệu tuyển sinh nội bộ — trường, ngành, điểm chuẩn, học bổng. Dùng khi cần dẫn chứng cụ thể."
5. Cấu hình **Web Search** (Tool Mode, giống các buổi trước, chỉ fallback).
6. Cấu hình **File** (Tool Mode):
    - Tool Name: `doc_file_ban_hoi`.
    - Tool Description: "Đọc nội dung file bảng hỏi HS (PDF/TXT/Markdown) mà user đính kèm. Dùng khi user gửi file chứa câu trả lời 8 câu mở đầu và bảng hỏi chuyên sâu."
7. Cấu hình **Page Content Viewer** (Notion bundle, Tool Mode):
    - Notion Secret: Integration Token.
    - Tool Name: `doc_page_notion_ban_hoi`.
    - Tool Description: "Đọc nội dung một page Notion khi user cung cấp page_id hoặc link. Dùng khi user chia sẻ link Notion thay vì file."
8. Dán **Agent Instructions** (chuẩn):

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
    - Ngôn ngữ: nhẹ nhàng, khích lệ, xưng "anh/chị" với HS.
    ```

9. Nối 4 tool vào Agent, `Chat Input → Agent → Chat Output`.

> ⚠️ **Checkpoint 1:** Flow có đủ 4 tool ở Tool Mode, 4 Tool Description đều khác nhau và rõ ràng. GV đi vòng kiểm tra.

**Bước 2 (25 phút) — Test end-to-end với 3 hồ sơ:**

Mỗi nhóm chạy cả 3 hồ sơ. Sử dụng **phiếu so sánh**:

| Tiêu chí | HS A (Link Notion) | HS B (File PDF) | HS C (Hỏi 8 câu) |
| :-- | :-- | :-- | :-- |
| Agent chọn đúng nhánh điều hướng? | | | |
| 3-5 ngành có khác biệt rõ rệt giữa 3 HS không? | | | |
| Trường được gợi ý có phù hợp khối/vùng HS? | | | |
| Kế hoạch học tập có cụ thể không? | | | |
| Có dẫn nguồn không? | | | |

- **Hồ sơ A:** Paste link Notion page bảng hỏi của HS A vào Playground. Quan sát agent gọi `doc_page_notion_ban_hoi`, đọc, rồi tư vấn. Ghi phiếu.
- **Hồ sơ B:** Đính kèm file PDF (hoặc dán text bảng hỏi) của HS B. Quan sát agent gọi `doc_file_ban_hoi`. Ghi phiếu.
- **Hồ sơ C:** Bắt đầu chat với câu "chưa có bảng hỏi". Agent phải hỏi 8 câu. HS trả lời đúng theo hồ sơ C (khối D, Văn, blog...). Sau câu 8, agent tư vấn. Ghi phiếu.

**Bước 3 (10 phút) — So sánh độ cá nhân hoá + sửa prompt nếu cần:**

- Các nhóm so sánh 3 output:
    - Có phân biệt khối B / A / D trong lựa chọn trường không?
    - Có nhắc đến đặc điểm riêng (CLB môi trường / code game / viết blog) trong phần "lý do phù hợp" không?
    - Có trường nào bị lặp không phù hợp?
- Nếu output **quá giống nhau giữa 3 HS** → sửa Agent Instructions thêm ràng buộc:

    ```
    KHI tư vấn, BẮT BUỘC tham chiếu TRỰC TIẾP đến ÍT NHẤT 3 chi tiết cụ thể trong hồ sơ HS (ví dụ: "vì em tham gia CLB môi trường", "vì em đã code game Unity") ở phần 'lý do phù hợp'.
    ```

- Chạy lại 1 hồ sơ để xác nhận prompt sửa hoạt động.

### **5\. Chia sẻ, thảo luận và điều chỉnh (7 phút)**

#### **5.1. Mục tiêu**

- Mỗi nhóm trình bày 1 output tư vấn mà nhóm thấy "đắt" nhất.
- Peer feedback theo mẫu Two Stars and a Wish.

#### **5.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Gallery Walk + Two Stars and a Wish.*

- GV yêu cầu mỗi nhóm chọn **1 trong 3 output tư vấn** mà nhóm thấy thuyết phục nhất, chiếu lên màn chính trong 30 giây.
- Cả lớp (1-2 nhóm khác) nhận xét nhanh theo mẫu:
    - ⭐ "Tôi thích ... vì ...".
    - ⭐ "Tôi thích ... vì ...".
    - 💭 "Tôi sẽ cải thiện ... bằng cách ...".
- GV tổng hợp: "Đa số tư vấn đang đạt chuẩn nào? Điểm yếu chung là gì?"

### **6\. Tổng kết (3 phút)**

*Chiến lược dẫn dắt: Exit Ticket + Cliffhanger dẫn sang buổi 8.*

- **Exit Ticket** — HS viết vào note 1 câu:

  > "Hôm nay em đã xây xong hệ thống 3 agent. Nếu phải giới thiệu trong 1 phút cho bố mẹ, em sẽ nói hệ thống này **làm được việc gì** và **khác với ChatGPT thường ở chỗ nào**?"

- GV đúc kết:

  > "Chúng ta đã hoàn thành **toàn bộ 3 agent** — Chiến lược, Thông tin, Tư vấn — phối hợp thành 1 hệ thống thực sự. Việc còn lại: **luyện tập demo và phản biện**."

- **Cliffhanger sang buổi 8:**

  > "Buổi cuối cùng là một **Mini Hackathon**: mỗi nhóm sẽ **bốc thăm 1 trong 5 hồ sơ bí mật** mà GV đã chuẩn bị — chưa từng thấy trước đó. Các em demo live Agent Tư vấn với hồ sơ đó trước cả lớp. Cả lớp sẽ nhận xét. Và cuối buổi, chúng ta sẽ bàn một chủ đề **nhạy cảm**: khi AI tư vấn sai, ai chịu trách nhiệm? AI có định kiến khi gợi ý ngành cho con trai / con gái không? Dữ liệu HS trên Notion có được bảo mật? Hẹn các em buổi 8."

- GV nhắc chuẩn bị: luyện demo flow Agent Tư vấn ở nhà, đảm bảo API key + AstraDB + Notion còn hoạt động. Chuẩn bị ngắn **tên flow / icon nhóm** cho Hackathon.

## Thông tin tài liệu

| Được soạn ngày: | 23/04/2026 | Bởi: | Kiro AI |
| :-- | :-- | :-- | :-- |
| Được review ngày: |  | Bởi: |  |
| Được hiệu chỉnh ngày: |  | Bởi: |  |

## Phân bổ các hoạt động

| Nội dung | Chương trình 2 buổi | |
| :-- | :-: | :-: |
| | **Buổi** | **Thời lượng (phút)** |
| Structured Output + hoàn thiện Agent Thông tin (lập bảng Notion 5 cột) | 1 (buổi 6) | 90 |
| Agent Tư vấn + testing end-to-end 3 hồ sơ | 2 (buổi 7) | 90 |

