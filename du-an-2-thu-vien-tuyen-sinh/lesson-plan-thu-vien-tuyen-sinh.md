# Dự án 2: Thư viện tuyển sinh thông minh

Khoá: Xây dựng hệ thống AI Agent tư vấn tuyển sinh với Langflow.

Cấp học: THPT.

Thời lượng: 3 buổi x 90 phút.

## Mô tả bài học

Trong bài học này, học sinh sẽ tìm hiểu kiến thức về **tool Web Search**, **embedding**, **vector database (AstraDB)**, **RAG (Retrieval-Augmented Generation)** và **chiến lược fallback giữa RAG với Web Search**, rồi vận dụng để xây dựng dự án **Agent Thông tin tuyển sinh** — một "thư viện sống" biết tra cứu dữ liệu tuyển sinh đại học, kết hợp kiến thức nội bộ (RAG) và tin mới trên web.

## I. Yêu cầu cần đạt

### **1\. Kiến thức**

Thực hiện bài học này học sinh sẽ khám phá các kiến thức:

- Khái niệm **tool** trong AI Agent (mở rộng từ Notion bundle ở buổi 2 sang Web Search).
- Component **Web Search** (Google News) và cách gắn làm tool cho Agent.
- Khái niệm **embedding** — biến text thành dãy số để máy hiểu "nghĩa".
- Khái niệm **vector database** — kho lưu các dãy số và tìm kiếm theo nghĩa.
- Component **File**, **Split Text**, **Astra DB** (với vectorize integration).
- Chunking, metadata khi lưu dữ liệu vào vector database.
- Template **Vector Store RAG** có sẵn trong Langflow.
- Tham số **Number of Search Results** (top-k) và ảnh hưởng của nó.
- Chiến lược **fallback "ưu tiên RAG → xin phép user search web"**.
- Đánh giá độ tin cậy của nguồn web.

### **2\. Năng lực**

Thực hiện bài học này học sinh sẽ rèn luyện và phát triển các năng lực với các biểu hiện:

*Năng lực đặc thù:*

- Xây dựng flow Agent + Web Search: `Chat Input → Agent (Language Model + Web Search tool) → Chat Output`.
- Viết system prompt để agent biết khi nào cần search web và đánh giá nguồn.
- Giải thích được embedding là gì và tại sao cần vector database.
- Tạo database + collection trên AstraDB Serverless có bật vectorize integration.
- Xây dựng flow ingest thủ công: `File → Split Text → Astra DB`.
- Mở rộng sang flow ingest tự động qua Agent + Web Search.
- Xây dựng flow RAG cơ bản và so sánh chất lượng trả lời có/không RAG.
- Điều chỉnh top-k và quan sát ảnh hưởng đến kết quả.
- Viết system prompt cho chiến lược fallback RAG → Web Search.
- Đánh giá agent có chọn đúng nguồn uy tín và biết khi nào dùng công cụ nào.

*Năng lực chung:*

- Giao tiếp, làm việc nhóm qua hoạt động chia nhóm phân trường để ingest dữ liệu.
- Sáng tạo qua việc thiết kế system prompt và chiến lược fallback.
- Giải quyết vấn đề khi debug flow không chạy đúng.

### **3\. Phẩm chất**

Bài học này tạo điều kiện để học sinh phát triển những phẩm chất với các biểu hiện:

- **Trung thực** khi đối chiếu nguồn thông tin, dẫn nguồn rõ ràng trong output của agent.
- **Trách nhiệm** khi xử lý dữ liệu tuyển sinh — hiểu rằng thông tin sai lệch có thể ảnh hưởng đến quyết định quan trọng của người dùng.
- **Chăm chỉ** qua quá trình chuẩn bị dữ liệu nhiều trường/ngành, ingest từng bước.

## II. Sản phẩm

**Cuối buổi 3 (1/3):**

- Flow **Agent + Web Search** chạy trên Playground. Agent trả lời được ít nhất 5 câu hỏi tuyển sinh kèm dẫn nguồn từ web.
- Bảng so sánh: 3 câu hỏi được trả lời bằng Agent **có** Web Search vs. **không** Web Search — ghi rõ khác biệt về độ mới, độ đầy đủ, dẫn nguồn.

**Cuối buổi 4 (2/3):**

- Collection `tuyen_sinh_dai_hoc` trên AstraDB có bật vectorize integration.
- Flow **ingest thủ công** (`File → Split Text → Astra DB`) đã nạp dữ liệu của **3 trường** (mỗi nhóm/HS 1 trường).
- Flow **ingest tự động qua Agent** (`Agent + Web Search + Astra DB tool`) nạp thêm dữ liệu của **ít nhất 2 trường** khác.
- Tổng: **5+ trường** trong AstraDB với metadata đầy đủ (tên trường, năm, loại thông tin).

**Cuối buổi 5 (3/3):**

- Flow **RAG cơ bản** (`Chat Input → Astra DB → Parser → Prompt Template → Language Model → Chat Output`) — thử nghiệm top-k = 2, 4, 8 và ghi chú ảnh hưởng.
- Flow **Agent + 2 tool** (Astra DB RAG + Web Search) với chiến lược fallback "ưu tiên RAG → xin phép user search web".
- Bảng test **5 câu pha trộn** (có câu RAG trả lời được, có câu cần Web Search) — đánh giá agent có chọn đúng tool không.

## III. Thiết bị dạy học và học liệu

Các nguyên vật liệu cần chuẩn bị cho một học sinh:

| Nguyên vật liệu | Đơn vị | Số lượng |
| :-- | :-: | :-: |
| Sổ/note ghi chú so sánh prompt, kết quả test | Cuốn | 1 |
| File PDF/text tuyển sinh của 1 trường ĐH (do HS/nhóm tự chuẩn bị) | File | 1-3 |

Các công cụ và thiết bị cần chuẩn bị cho một nhóm:

| Công cụ và thiết bị | Đơn vị | Số lượng |
| :-- | :-: | :-: |
| Máy tính có internet, đã cài Langflow Desktop | Cái | 1 |
| Tài khoản Google + API key Gemma 4 (đã có từ buổi 1) | Tài khoản | 1 |
| Tài khoản Notion + Integration Token (đã có từ buổi 2) | Tài khoản | 1 |
| Tài khoản DataStax (AstraDB Serverless free tier) — đăng ký trước buổi 4 | Tài khoản | 1 |

Các tài liệu, phiếu học tập:

| Đường dẫn tài liệu, phiếu học tập, video,... |
| :-- |
| Tài liệu Langflow Web Search: `https://docs.langflow.org/components-data#web-search` |
| Tài liệu Langflow AstraDB: `https://docs.langflow.org/components-vector-stores#astra-db` |
| Trang đăng ký AstraDB: `https://astra.datastax.com` |
| Tài liệu Langflow Vector Store RAG template: `https://docs.langflow.org/starter-projects-vector-store-rag` |
| File PDF tuyển sinh mẫu (GV chuẩn bị): Đề án tuyển sinh 2024 của 3 trường: ĐH Bách Khoa Hà Nội, ĐH KHTN TP.HCM, ĐH Kinh tế Quốc dân |
| Phiếu so sánh Agent có/không Web Search (buổi 3) |
| Phiếu ghi chú top-k + chất lượng RAG (buổi 5) |
| Phiếu test 5 câu pha trộn (buổi 5) |
| Rubric quá trình EDP (dùng chung cả 3 buổi) |

## IV. Tiến trình dạy học


## BUỔI 1 (buổi 3 của khoá) — Web Search: tool thứ hai cho Agent

### **1\. Xác định vấn đề (15 phút)**

#### **1.1. Mục tiêu**

- HS nhận ra giới hạn của một agent chỉ có kiến thức sẵn có trong LLM: không biết tin mới, không dẫn nguồn được.
- Xác định được nhiệm vụ buổi học: gắn thêm **Web Search** làm tool thứ hai cho Agent, biến agent từ "kể chuyện theo trí nhớ" thành "tra cứu có dẫn nguồn".
- Đặt được câu hỏi làm rõ: khi nào agent nên search web? Làm sao đánh giá nguồn tin?

#### **1.2. Tổ chức thực hiện**

**1.2.1. Khởi động — Thử thách "AI biết gì, không biết gì?" (5 phút)**

*Chiến lược dẫn dắt: Sai lầm có chủ đích + Câu hỏi dự đoán → Kiểm chứng.*

- GV mở Langflow, bật flow Agent Chiến lược của một HS (hoặc flow chatbot đơn giản Chat Input → Language Model → Chat Output).
- GV hỏi cả lớp: "Các em dự đoán xem agent của chúng ta trả lời được những câu nào sau đây?"
    - Câu A: "AI Agent là gì?"
    - Câu B: "Điểm chuẩn ngành CNTT ĐH Bách Khoa Hà Nội năm 2024 là bao nhiêu?"
    - Câu C: "Hôm qua có tin gì mới về kỳ thi đánh giá năng lực?"
- GV gọi 3 HS xung phong, mỗi em gõ 1 câu vào Playground, cả lớp quan sát.
- Kết quả dự đoán được: câu A trả lời tốt, câu B thường trả lời chung chung/sai/từ chối, câu C nhất định không biết (vì cut-off date của model).
- GV chốt: "Agent của chúng ta đang như một học sinh giỏi nhưng **bị nhốt trong phòng không có internet**. Chúng ta cần cho nó một cái... gì đó?"

**1.2.2. Đặt vấn đề & câu hỏi dẫn dắt (10 phút)**

- GV mời HS nêu ý tưởng. Dẫn dắt đến từ khoá **tool** và **Web Search**.
- GV treo/chiếu câu hỏi dẫn dắt của cả dự án 2 lên bảng:

  > "Làm thế nào để Agent Thông tin tuyển sinh trả lời chính xác, cập nhật và **có dẫn nguồn** cho mọi câu hỏi tuyển sinh đại học?"

- GV giới thiệu lộ trình 3 buổi:
    - Buổi 3 (hôm nay): Gắn **Web Search** — agent có "internet".
    - Buổi 4: Tạo **thư viện riêng trên AstraDB** — agent có "trí nhớ lâu dài".
    - Buổi 5: Kết hợp cả hai với chiến lược thông minh.
- GV nhấn mạnh quy trình EDP: hôm nay đi **Xác định vấn đề → Tìm hiểu kiến thức → Lên ý tưởng → Làm prototype → Thử nghiệm → Trình bày mini** trọn vẹn cho nhiệm vụ Web Search.
- GV chia nhóm 2 HS/nhóm (hoặc cá nhân nếu đủ máy). Phát phiếu so sánh Agent có/không Web Search.

### **2\. Nghiên cứu kiến thức nền (20 phút)**

#### **2.1. Mục tiêu**

- Hiểu lại khái niệm **tool** trong AI Agent (củng cố từ buổi 2 với Notion, mở rộng sang Web Search).
- Biết component **Web Search** trong Langflow, cách nó hoạt động (Google News / DuckDuckGo / RSS), rate limit.
- Biết các tiêu chí đánh giá nguồn web uy tín cho lĩnh vực tuyển sinh.

#### **2.2. Tổ chức thực hiện**

**2.2.1. Tìm hiểu Tool và Web Search component (10 phút)**

*Chiến lược dẫn dắt: Phép tương tự + So sánh đối chiếu.*

- GV dùng phép tương tự: "Ở buổi 2, agent có tool Notion — giống như có **tay** để ghi lên sổ. Hôm nay agent có thêm tool Web Search — giống như có **mắt** để nhìn ra internet."
- GV mở Langflow → Sidebar → tìm component **Web Search** (trong nhóm Data/Core).
- GV vừa thao tác vừa giải thích các tham số chính:
    - **Query**: từ khoá tìm kiếm (agent tự điền khi chạy).
    - **Provider / Source**: DuckDuckGo, Google News, RSS feed.
    - **Max Results**: số kết quả trả về.
    - Lưu ý: Web Search có rate limit — không gọi liên tục quá nhiều lần.
- GV đưa ra 1 bảng so sánh Notion tool vs. Web Search tool (viết trên bảng hoặc chiếu slide):

    | Tiêu chí | Notion tool (Create Page) | Web Search tool |
    | :-- | :-- | :-- |
    | Mục đích | Ghi dữ liệu ra ngoài | Đọc dữ liệu từ ngoài |
    | Input agent cần cung cấp | title, content | query (từ khoá) |
    | Output trả về | page_id | Danh sách bài viết + link |
    | Cần token/key? | Có (integration token) | Không (cho DuckDuckGo) |

- GV hỏi HS: "Theo em, vì sao cùng là 'tool' nhưng một cái cần token, một cái không?" → Dẫn dắt tới khái niệm API công khai và API cần xác thực.

**2.2.2. Đánh giá nguồn web uy tín — hoạt động nhanh (10 phút)**

*Chiến lược dẫn dắt: Thí nghiệm nhanh + Phân tích.*

- GV chiếu 5 URL (đã chuẩn bị trước), yêu cầu các nhóm trong 3 phút xếp hạng từ **uy tín nhất → kém uy tín nhất** khi tra cứu thông tin tuyển sinh. Ví dụ:
    1. `https://moet.gov.vn` (Bộ Giáo dục)
    2. `https://hust.edu.vn` (ĐH Bách Khoa Hà Nội — chính thức)
    3. `https://vnexpress.net/tuyen-sinh`
    4. `https://hocmai.vn/forum/...` (diễn đàn học sinh)
    5. `https://facebook.com/nhom-luyen-thi-2024` (group Facebook)
- Các nhóm xếp hạng và giải thích ngắn (1 câu cho mỗi URL).
- GV chốt tiêu chí đánh giá nguồn:
    1. Nguồn chính thức (website trường, Bộ GD&ĐT) > báo chí chính thống > diễn đàn > mạng xã hội.
    2. Có ngày đăng rõ ràng, không quá cũ.
    3. Có tác giả / cơ quan đứng tên.
    4. Đối chiếu được với ít nhất 1 nguồn khác.
- GV nhấn mạnh: trong system prompt lát nữa, ta sẽ nhắc agent **ưu tiên nguồn chính thức** và **luôn dẫn link**.

### **3\. Đề xuất, lựa chọn giải pháp và lên kế hoạch (10 phút)**

#### **3.1. Mục tiêu**

- Phác thảo được flow Agent + Web Search trên giấy/note.
- Lên được danh sách 5 câu hỏi tuyển sinh để test agent.
- Phân công nhiệm vụ trong nhóm (ai gõ, ai ghi chú, ai đóng vai user test).

#### **3.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Checklist + Brainstorm có cấu trúc.*

- GV phát/chiếu checklist 4 việc cần làm:
    1. Mở Langflow → tạo flow mới `Agent + Web Search`.
    2. Cấu hình Agent (Model = Gemma 4, API key đã có).
    3. Bật Tool Mode cho Web Search, viết Tool Description.
    4. Viết Agent Instructions nhấn mạnh: khi nào search, trả lời phải dẫn nguồn.
- GV yêu cầu mỗi nhóm vẽ sơ đồ flow vào note (khung: `Chat Input → Agent ← Web Search → Chat Output`).
- Mỗi nhóm brainstorm **5 câu hỏi test** — ít nhất 2 câu cần thông tin mới (tin 2025-2026), 2 câu tra cứu cụ thể (điểm chuẩn, học phí), 1 câu tổng hợp (so sánh, liệt kê).

### **4\. Chế tạo mẫu, thử nghiệm và đánh giá (35 phút)**

#### **4.1. Mục tiêu**

- Xây được flow `Chat Input → Agent (Language Model + Web Search tool) → Chat Output`.
- Agent trả lời được 5 câu hỏi tuyển sinh kèm dẫn nguồn.
- So sánh được kết quả có/không Web Search.

#### **4.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Làm mẫu từng bước (Modeling) + Thử thách kỹ thuật có scaffold.*

**Bước 1 (5 phút) — GV làm mẫu nhanh:** GV vừa thao tác vừa nói to suy nghĩ trên 1 máy chiếu:

- Tạo flow mới `Blank Flow` → kéo vào `Chat Input`, `Agent`, `Chat Output`, `Web Search`.
- Cấu hình Agent: Provider Google, Model gemma hoặc gemini flash, dán API key, Temperature mặc định.
- Click **Web Search** → bật **Tool Mode**.
- Viết **Tool Name**: `web_search_tuyen_sinh`.
- Viết **Tool Description** mẫu:
    ```
    Tìm kiếm trên web các thông tin tuyển sinh đại học Việt Nam mới nhất (điểm chuẩn, phương thức xét tuyển, học bổng, tin tuyển sinh năm hiện tại). Input: query (chuỗi từ khoá tiếng Việt). Output: danh sách các bài viết kèm link nguồn. Dùng tool này khi user hỏi về thông tin thời sự hoặc cụ thể mà em không biết chắc chắn.
    ```
- Nối dây: Chat Input → Agent (input), Web Search (Tool output) → Agent (Tools port), Agent (Message output) → Chat Output.
- Dán **Agent Instructions** mẫu (chiếu lên màn hình để HS copy):

    ```
    Bạn là "Agent Thông tin tuyển sinh" — trợ lý tra cứu thông tin tuyển sinh đại học Việt Nam.

    NGUYÊN TẮC:
    1. Với câu hỏi về thông tin cập nhật (điểm chuẩn, học phí, lịch tuyển sinh, tin mới), LUÔN dùng tool web_search_tuyen_sinh để tra cứu thay vì trả lời từ trí nhớ.
    2. Ưu tiên nguồn chính thức: website trường (.edu.vn), Bộ GD&ĐT (moet.gov.vn), báo chính thống (vnexpress, tuoitre, thanhnien).
    3. LUÔN dẫn link nguồn ở cuối câu trả lời, format: [Tên nguồn](URL).
    4. Nếu kết quả web không đủ/mâu thuẫn, nói rõ "Tôi tìm được X nhưng chưa chắc chắn" thay vì bịa.
    5. Trả lời ngắn gọn, có cấu trúc (bullet/bảng khi cần), tiếng Việt, xưng "em/thầy cô" phù hợp.
    ```

**Bước 2 (15 phút) — HS làm theo, GV đi vòng hỗ trợ:**

- HS/nhóm tự thao tác lại các bước trên. GV đi vòng kiểm tra:
    - Checkpoint 1: flow có đủ 4 component và nối đúng chưa?
    - Checkpoint 2: Tool Mode đã bật? Tool Description viết rõ chưa?
- HS nào xong trước có thể chuyển sang Bước 3.

**Bước 3 (10 phút) — Test và so sánh:**

- HS mở Playground, chạy **từng câu trong 5 câu đã brainstorm**. Ghi vào **phiếu so sánh**:

    | Câu hỏi | Có Web Search (trả lời + link) | Không Web Search (chỉ LLM) | Nhận xét |
    | :-- | :-- | :-- | :-- |
    | ... | ... | ... | ... |

- Để test "không Web Search", HS tạm **xóa dây nối** Web Search → Agent, chạy lại câu đó, ghi lại. Rồi nối dây lại.
- Tiêu chí nhận xét: độ mới, độ cụ thể, có dẫn nguồn không, có "bịa" không.

**Bước 4 (5 phút) — Phân hoá & mở rộng nhanh:**

- **HS nhanh:** thử thay đổi **Provider** của Web Search (DuckDuckGo ↔ Google News), quan sát khác biệt.
- **HS cần hỗ trợ:** GV đi đến từng nhóm, hỗ trợ theo checklist debug sẽ nhắc ở phần Tổng kết.

### **5\. Chia sẻ, thảo luận và điều chỉnh (5 phút)**

#### **5.1. Mục tiêu**

- Chia sẻ nhanh phát hiện thú vị giữa Agent có/không Web Search.
- Nhận diện vấn đề chung cả lớp gặp phải (rate limit, nguồn không uy tín, agent không gọi tool...).

#### **5.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Gallery nhanh + Two Stars and a Wish mini.*

- GV mời 2-3 nhóm chia sẻ nhanh (mỗi nhóm 1 phút): 1 phát hiện thú vị + 1 vấn đề gặp phải.
- GV tổng kết các mẫu trả lời:
    - "Agent có Web Search trả lời chi tiết hơn và dẫn nguồn được" (điểm cộng).
    - "Agent đôi khi không chịu search, tự trả lời bừa" → cần viết Tool Description và Agent Instructions kỹ hơn.
    - "Agent dùng nguồn Facebook/Forum" → cần nhấn mạnh ưu tiên nguồn chính thức trong prompt.

### **6\. Tổng kết (5 phút)**

*Chiến lược dẫn dắt: Exit Ticket + Cliffhanger.*

- GV đặt **Exit Ticket** — mỗi HS viết 1 câu vào note/giấy:

  > "Hôm nay agent của em trở nên 'thông minh hơn' vì có thêm tool gì? Và tool đó giúp giải quyết vấn đề gì mà LLM một mình không làm được?"

- GV đúc kết:
    - Agent = LLM + tool. Hôm nay thêm tool thứ 2: **Web Search**.
    - Web Search giúp agent lấy tin **mới** và **dẫn nguồn**.
    - Nhưng Web Search có nhược điểm: **chậm**, **phụ thuộc internet**, **nguồn có thể không uy tín**, **rate limit**.
- **Cliffhanger cho buổi sau:**

  > "Nếu em có 1000 trang PDF tuyển sinh của 50 trường, em có muốn mỗi lần hỏi là agent phải đọc lại tất cả không? Chắc chắn không. Vậy làm sao để agent 'nhớ' được 1000 trang đó và tra cứu trong 1 giây? Đó là câu chuyện của buổi sau: **Vector Database**."

- GV nhắc chuẩn bị buổi 4:
    - Mỗi HS/nhóm **chuẩn bị trước 1 file PDF hoặc 1 trang text** về đề án tuyển sinh của 1 trường ĐH em quan tâm (download từ website trường, ít nhất 5-10 trang).
    - **Đăng ký tài khoản AstraDB free tier tại [astra.datastax.com](https://astra.datastax.com)** trước khi vào lớp — khuyến khích làm ở nhà để tiết kiệm thời gian.

**Checklist debug buổi 3 (GV phát cho HS dán vào note):**

| Triệu chứng | Nguyên nhân | Giải pháp |
| :-- | :-- | :-- |
| Agent không gọi Web Search, trả lời từ trí nhớ | Tool Description mơ hồ | Thêm "LUÔN dùng tool khi câu hỏi liên quan tin mới/cụ thể" |
| Web Search báo lỗi rate limit | Gọi quá nhiều lần | Đợi 1-2 phút, giảm Max Results, hoặc đổi Provider |
| Agent trả lời không có link | Agent Instructions chưa yêu cầu | Thêm "LUÔN dẫn link nguồn ở cuối" |
| Agent trích nguồn Facebook/Forum | Chưa nhấn mạnh nguồn uy tín | Thêm danh sách domain ưu tiên vào Instructions |

## HẾT BUỔI 1 (buổi 3 của khoá)


## BUỔI 2 (buổi 4 của khoá) — Vector Database & Embedding: ingest dữ liệu vào AstraDB

HS ổn định, chào hỏi GV, kiểm tra máy và tài khoản AstraDB.

### **1\. Ôn tập (10 phút)**

*Chiến lược dẫn dắt: Think-Pair-Share + Kết nối bài trước.*

- GV chiếu slide tóm tắt buổi 3: "Agent + Web Search — cái được, cái mất".
- **Think (1 phút):** Mỗi HS tự trả lời vào note 2 câu:
    1. Web Search giúp agent cái gì?
    2. Web Search có nhược điểm gì đáng lo?
- **Pair (2 phút):** HS chia sẻ với bạn cùng nhóm.
- **Share (4 phút):** GV gọi 2-3 nhóm trả lời. Mục tiêu dẫn về các hạn chế: chậm, không ổn định, không kiểm soát được nguồn, phụ thuộc internet.
- GV nêu tình huống mới: *"Ban tuyển sinh của trường mình muốn xây một 'thư viện riêng' chứa sẵn dữ liệu tuyển sinh của 50+ trường đại học. Mỗi lần HS hỏi, agent tra thư viện này trước — nhanh, chắc chắn, không phụ thuộc internet. Cấu trúc nào phù hợp nhất?"*
- HS thảo luận ngắn 5 phút theo cấu trúc: "Công việc hôm nay là gì?" → **xây thư viện riêng** trên AstraDB.

### **2\. Nghiên cứu kiến thức nền (25 phút)**

#### **2.1. Mục tiêu**

- Giải thích được **embedding** là gì bằng ngôn ngữ bình dân.
- Giải thích được tại sao cần **vector database** và nó khác database thường ở điểm gì.
- Hiểu vai trò của **chunking** (Split Text) và **metadata**.

#### **2.2. Tổ chức thực hiện**

**2.2.1. Embedding — biến text thành dãy số "biết nghĩa" (10 phút)**

*Chiến lược dẫn dắt: Trò chơi mô phỏng + Phép tương tự.*

- **Trò chơi mô phỏng "toạ độ nghĩa":** GV vẽ 2 trục trên bảng: trục X = "vui ↔ buồn", trục Y = "nhanh ↔ chậm".
- GV đọc 6 từ, mời HS xung phong lên dán giấy nhớ ở toạ độ phù hợp:
    - "Cười" (vui, nhanh)
    - "Khóc" (buồn, chậm)
    - "Nhảy" (vui, nhanh)
    - "Thở dài" (buồn, chậm)
    - "Mỉm cười" (vui, chậm)
    - "Bực bội" (buồn, nhanh)
- Cả lớp quan sát: các từ "cùng nghĩa" tụ lại gần nhau.
- GV chốt:

  > "Embedding là việc **biến một từ/câu/đoạn văn thành một dãy số** — mỗi số là một 'toạ độ' trong một không gian nghĩa. Máy tính dùng khoảng vài trăm đến vài nghìn toạ độ (thay vì 2 như mình vừa làm) để biểu diễn nghĩa chi tiết."

- Ví dụ: `"điểm chuẩn ngành CNTT ĐH Bách Khoa"` → `[0.12, -0.34, 0.56, ..., 0.78]` (768 số).
- Phép tương tự: "Giống như mỗi người có một địa chỉ nhà — những người sống gần nhau thì hay chơi với nhau. Những câu 'sống gần' nhau trong không gian nghĩa thì có nghĩa tương tự nhau."

**2.2.2. Vector database — thư viện tìm kiếm theo nghĩa (8 phút)**

*Chiến lược dẫn dắt: So sánh đối chiếu.*

- GV vẽ 2 cột trên bảng:

    | Database thường (Excel, SQL) | Vector database (AstraDB) |
    | :-- | :-- |
    | Lưu text y nguyên | Lưu text + embedding (dãy số) |
    | Tìm kiếm **khớp từ khoá** | Tìm kiếm **theo nghĩa** |
    | "điểm chuẩn" ≠ "điểm đầu vào" | "điểm chuẩn" ≈ "điểm đầu vào" (vì gần nhau về nghĩa) |
    | Dùng cho: danh sách HS, lịch | Dùng cho: tìm kiếm tài liệu, RAG |

- GV nêu lợi ích cho dự án: "Khi HS hỏi 'học phí ngành CNTT là bao nhiêu?', agent có thể tìm ra đoạn ghi 'Mức học phí đào tạo Công nghệ Thông tin' — dù 2 câu không khớp từ khoá hoàn hảo."
- GV giới thiệu **AstraDB Serverless** với **vectorize integration**: AstraDB tự lo embedding, mình chỉ việc đưa text vào. Đỡ phải cấu hình embedding model riêng.

**2.2.3. Chunking & metadata — vì sao cần (7 phút)**

*Chiến lược dẫn dắt: Câu hỏi mở kích thích tò mò.*

- GV hỏi: "Nếu các em có 1 file PDF 50 trang, nhét cả 50 trang vào làm 1 embedding thì tốt hay xấu?"
- HS thảo luận nhanh 1-2 phút. GV dẫn:
    - Embedding 1 cục lớn = dãy số trung bình của cả file → mất nghĩa chi tiết.
    - Giải pháp: **Split Text (chunking)** — chia file thành nhiều đoạn nhỏ (ví dụ mỗi đoạn 500-1000 ký tự), embedding từng đoạn.
    - Mỗi chunk lưu kèm **metadata**: `{ trường: "HUST", năm: 2024, loại: "đề án" }` → sau này filter được.
- GV giới thiệu tham số **Chunk Size** và **Chunk Overlap** trong component Split Text.

### **3\. Đề xuất, lựa chọn giải pháp và lên kế hoạch (10 phút)**

#### **3.1. Mục tiêu**

- Thiết kế được sơ đồ 2 flow: flow ingest thủ công và flow ingest tự động.
- Phân công: mỗi nhóm 1 trường để ingest, đảm bảo đủ 3 trường ingest thủ công.
- Lên kế hoạch metadata thống nhất cho cả lớp.

#### **3.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Checklist + Thống nhất cấu trúc chung.*

- GV trình chiếu 2 sơ đồ flow:

    **Flow ingest thủ công:**
    ```
    [File] → [Split Text] → [Astra DB (mode: Ingest)]
    ```

    **Flow ingest tự động (nâng cao, cuối buổi):**
    ```
                      [Web Search] ──┐
                                     ▼ (tool)
    [Chat Input] → [Agent] ─────────→ [Chat Output]
                                     ▲
                                     │ (tool)
                      [Astra DB (Ingest mode)] ──┘
    ```

- **Thống nhất metadata cả lớp** (GV chiếu, HS ghi vào note):
    ```
    {
      "truong": "HUST" | "HCMUT" | "NEU" | ...,
      "nam": 2024,
      "loai": "de_an_tuyen_sinh" | "hoc_bong" | "diem_chuan"
    }
    ```
- GV chia nhóm:
    - **3 nhóm ingest thủ công**: mỗi nhóm 1 file PDF 1 trường (ĐH Bách Khoa HN, ĐH KHTN TPHCM, ĐH Kinh tế Quốc dân — GV cung cấp file nếu nhóm chưa có).
    - **Các nhóm còn lại**: chuẩn bị sẵn file khác để lát ingest tự động.
- Mỗi nhóm vẽ sơ đồ flow mình sẽ xây vào note (1 phút).

### **4\. Chế tạo mẫu, thử nghiệm và đánh giá (40 phút)**

#### **4.1. Mục tiêu**

- Tạo được database + collection trên AstraDB có vectorize integration.
- Xây và chạy được flow ingest thủ công cho 1 file.
- Xác thực dữ liệu đã nhúng thành công (kiểm tra trên AstraDB Console).
- (Nâng cao) Mở rộng sang flow ingest tự động qua Agent.

#### **4.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Hướng dẫn từng bước (Guided Lab) + Phân hoá theo tốc độ.*

**Bước 1 (10 phút) — Tạo database + collection trên AstraDB:**

GV làm mẫu nhanh trên máy chiếu, HS làm theo:

1. Truy cập [astra.datastax.com](https://astra.datastax.com), đăng nhập.
2. Click **Create Database** → chọn **Serverless (Vector)** → tên database: `langflow_tuyensinh` → Region gần nhất → Create. Chờ 2-3 phút.
3. Khi database **Active**, mở → tab **Data Explorer** → **Create Collection**:
    - Name: `tuyen_sinh_dai_hoc`.
    - **Bật "Vector-enabled collection"**.
    - Embedding Provider: chọn **NVIDIA** hoặc **OpenAI** tuỳ list có sẵn free (thường NVIDIA NV-Embed-QA là free).
    - Dimensions: giữ mặc định của model đã chọn.
    - Create.
4. Lấy **Application Token** và **API Endpoint**: tab **Settings** → **Generate Token** (role Database Administrator). Copy token (`AstraCS:...`) và Endpoint (`https://....apps.astra.datastax.com`). Lưu 2 giá trị này vào note an toàn.

> ⚠️ **Checkpoint 1:** HS phải có được 4 thứ trước khi sang bước 2: `collection name = tuyen_sinh_dai_hoc`, database active, token, endpoint. GV đi vòng kiểm tra.

**Bước 2 (15 phút) — Xây flow ingest thủ công và chạy:**

1. Trong Langflow, tạo **flow mới** — `Blank Flow`, đặt tên `Ingest_AstraDB`.
2. Kéo vào canvas: **File** (Data), **Split Text** (Processing), **Astra DB** (Vector Stores).
3. Cấu hình **File**: upload file PDF/text đã chuẩn bị.
4. Cấu hình **Split Text**: Chunk Size = `800`, Chunk Overlap = `100`.
5. Cấu hình **Astra DB**:
    - **Astra DB Application Token**: dán token đã lấy.
    - **API Endpoint**: dán endpoint.
    - **Collection Name**: `tuyen_sinh_dai_hoc`.
    - **Mode**: chọn mode nhập dữ liệu (Ingest/Write — tuỳ UI).
    - **Metadata** (nếu có ô metadata hoặc qua Prompt Template): `{"truong": "HUST", "nam": 2024, "loai": "de_an_tuyen_sinh"}`.
6. Nối: `File → Split Text → Astra DB`.
7. Nhấn **Run** trên component Astra DB (hoặc Play toàn flow).

> ⚠️ **Checkpoint 2:** HS phải thấy thông báo "Inserted N documents" (hoặc tương tự). Nếu lỗi:
> - Lỗi token → kiểm tra token có role đúng không.
> - Lỗi endpoint → copy lại từ AstraDB console.
> - Lỗi embedding → kiểm tra collection có bật vectorize.

**Bước 3 (5 phút) — Xác thực dữ liệu đã ingest:**

- HS mở lại AstraDB Console → Data Explorer → collection `tuyen_sinh_dai_hoc` → thấy N rows mới được chèn.
- Click 1 row → xem field `content` (text) và `$vector` (dãy số — embedding).
- Chia sẻ nhanh với bạn trong nhóm: "Mỗi chunk có bao nhiêu ký tự? Embedding có bao nhiêu số?"

**Bước 4 (10 phút) — Mở rộng: flow ingest tự động qua Agent (phân hoá):**

- **HS làm nhanh (mở rộng A):**
    - Tạo flow mới: `Chat Input → Agent → Chat Output`, gắn 2 tool là **Web Search** (Tool Mode) và **Astra DB** (Tool Mode — mode Ingest).
    - Agent Instructions mẫu: "Khi user nói 'thu thập thông tin trường X', hãy (1) dùng Web Search tìm đề án tuyển sinh, điểm chuẩn, học bổng của trường X; (2) với mỗi kết quả phù hợp, dùng tool Astra DB ingest để lưu text vào collection `tuyen_sinh_dai_hoc` kèm metadata `{truong, nam, loai}`."
    - Test với 1-2 trường khác.
- **HS cần hỗ trợ:**
    - GV đi vòng hỗ trợ debug bước 2 cho xong. Mục tiêu tối thiểu: ingest thủ công thành công 1 trường.
    - Nếu còn thời gian, làm thêm 1 trường nữa bằng flow thủ công (thay file, thay metadata).

### **5\. Chia sẻ, thảo luận và điều chỉnh (3 phút)**

#### **5.1. Mục tiêu**

- Các nhóm báo nhanh: đã ingest được bao nhiêu trường, tổng số chunk.
- Ghi nhận vấn đề chung.

#### **5.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Gallery Walk nhanh.*

- GV chuẩn bị 1 bảng chung trên bảng đen (hoặc Google Sheet chung):

    | Nhóm | Trường ingest | Số chunk | Ghi chú |
    | :-- | :-- | :-- | :-- |

- Mỗi nhóm lên điền 1 dòng. GV tổng: bao nhiêu trường, đã đủ 5+ chưa? Nếu thiếu → giao cho buổi tới.

### **6\. Tổng kết (2 phút)**

*Chiến lược dẫn dắt: Cliffhanger.*

- GV đúc kết: "Hôm nay chúng ta đã xây được **thư viện riêng** — 5+ trường đã có trên AstraDB. Nhưng thư viện chỉ có ích khi agent biết **tra cứu** nó. Buổi sau, ta sẽ làm điều đó bằng kỹ thuật có tên **RAG** — Retrieval-Augmented Generation."
- **Cliffhanger:** "Và quan trọng hơn: làm sao để agent **biết lúc nào dùng thư viện riêng, lúc nào chạy ra internet tìm**? Đó là nghệ thuật của **chiến lược fallback** mà mình sẽ thiết kế."
- GV nhắc HS giữ nguyên database & collection — buổi sau sẽ tiếp tục dùng.

## HẾT BUỔI 2 (buổi 4 của khoá)


## BUỔI 3 (buổi 5 của khoá) — RAG + chiến lược fallback

HS ổn định, chào hỏi GV, kiểm tra collection AstraDB vẫn còn dữ liệu.

### **1\. Ôn tập (10 phút)**

*Chiến lược dẫn dắt: Câu hỏi phản biện + So sánh giải pháp.*

- GV mở Playground của **Agent + Web Search** (từ buổi 3), chạy 1 câu: "Điểm chuẩn ngành CNTT ĐH Bách Khoa Hà Nội 2024?"
- GV hỏi cả lớp phản biện nhanh: "Agent này trả lời đúng không? Nếu đúng thì đến từ đâu? Nếu ngày mai không có internet, agent còn trả lời được không?"
- Dẫn về vấn đề:
    - Web Search: nhanh về **tin mới**, nhưng chậm / không ổn định / không kiểm soát được nguồn.
    - Data đã ingest ở buổi 4 vào AstraDB: **chắc chắn, nhanh, kiểm soát được** — nhưng agent chưa biết tra cứu.
- Chốt nhiệm vụ 3 phần của buổi 5:
    1. **Xây flow RAG cơ bản** (không Agent, chỉ chain đơn giản) — để hiểu cơ chế.
    2. Thử **điều chỉnh top-k** → quan sát chất lượng trả lời thay đổi thế nào.
    3. **Nâng cấp thành Agent + 2 tool** (Astra DB RAG + Web Search) với **chiến lược fallback**.

### **2\. Nghiên cứu kiến thức nền (20 phút)**

#### **2.1. Mục tiêu**

- Hiểu nguyên lý **RAG**: Retrieval + Augmented + Generation.
- Biết vai trò của component **Parser**, **Prompt Template** với biến `{context}` và `{question}`.
- Biết tham số **Number of Search Results (top-k)** và ý nghĩa của nó.
- Biết thiết kế chiến lược **fallback** bằng system prompt.

#### **2.2. Tổ chức thực hiện**

**2.2.1. RAG — nguyên lý 3 bước (10 phút)**

*Chiến lược dẫn dắt: Trò chơi nhập vai (Role Play).*

- GV mời 3 HS lên làm trò chơi "Mô phỏng RAG":
    - **HS A** — đóng vai **User**: đọc to 1 câu hỏi (ví dụ: "Ngành CNTT ĐH Bách Khoa HN tuyển theo tổ hợp gì?").
    - **HS B** — đóng vai **Retriever (AstraDB)**: cầm sẵn 4 tờ giấy, mỗi tờ là 1 đoạn text (chunk). Chọn 2 tờ **liên quan nhất** trao cho HS C.
    - **HS C** — đóng vai **LLM**: đọc câu hỏi của A + nội dung 2 tờ giấy của B, rồi trả lời to cho cả lớp.
- GV giải thích song song ý nghĩa từng bước:

    | Bước | Trong trò chơi | Trong Langflow |
    | :-- | :-- | :-- |
    | 1. **R**etrieval | HS B chọn tờ giấy liên quan | Astra DB component tìm top-k chunks gần câu hỏi nhất |
    | 2. **A**ugmented | HS C nhận **câu hỏi + 2 tờ giấy** | Prompt Template ghép `{context}` + `{question}` |
    | 3. **G**eneration | HS C trả lời | Language Model sinh câu trả lời |

- GV chốt: "RAG = cho LLM đọc **tài liệu liên quan** **trước** khi trả lời. Agent/LLM không phải nhớ hết — chỉ cần biết **tìm** rồi **đọc**."

**2.2.2. Prompt Template với `{context}` và `{question}` (5 phút)**

*Chiến lược dẫn dắt: Demo + Phân tích.*

- GV chiếu template mẫu:

    ```
    Dựa CHỈ trên thông tin sau đây:
    ---
    {context}
    ---
    Hãy trả lời câu hỏi: {question}

    NẾU thông tin không đủ để trả lời, hãy nói rõ "Không tìm thấy thông tin trong dữ liệu".
    KHÔNG bịa đặt thông tin ngoài {context}.
    ```

- GV hỏi: "Tại sao cần dặn LLM 'KHÔNG bịa'? Có gì khác so với việc chỉ truyền `{context}` mà không nói gì?"
- Dẫn về khái niệm **hallucination** — LLM đôi khi "bịa" thông tin nghe có lý. Prompt tốt giúp giảm hallucination.

**2.2.3. Top-k và chiến lược fallback (5 phút)**

*Chiến lược dẫn dắt: Câu hỏi dự đoán → kiểm chứng.*

- GV hỏi: "Nếu top-k = 1, chỉ lấy 1 chunk — có đủ không? Nếu top-k = 20, có vấn đề gì không?"
- Dẫn:
    - **Top-k nhỏ (1-2):** nhanh, nhưng có thể thiếu context → trả lời thiếu.
    - **Top-k lớn (10+):** nhiều thông tin, nhưng nhiễu, tốn token, có thể gây nhầm lẫn.
    - **Giá trị gợi ý:** 3-5 cho hầu hết use case. Trong dự án này dùng **top-k = 4** làm mặc định, rồi thử 2 và 8.
- GV giới thiệu **chiến lược fallback**:

    ```
    BƯỚC 1: Thử tìm câu trả lời trong Astra DB (RAG).
    BƯỚC 2: Đánh giá — câu trả lời từ RAG có đủ/đầy đủ không?
    BƯỚC 3a: Nếu ĐỦ → trả lời user, kèm dẫn nguồn từ RAG.
    BƯỚC 3b: Nếu KHÔNG đủ → BÁO cho user, HỎI: "Em có muốn em tìm trên các trang web uy tín không?"
    BƯỚC 4: Nếu user đồng ý → dùng Web Search.
    ```

- Nhấn: việc **hỏi xin phép user** trước khi search web là một pattern tốt — **human-in-the-loop**.

### **3\. Đề xuất, lựa chọn giải pháp và lên kế hoạch (5 phút)**

#### **3.1. Mục tiêu**

- Phác thảo 2 flow cần xây: RAG cơ bản và Agent + 2 tool.
- Soạn trước **5 câu test** theo tỉ lệ: 2 câu RAG trả lời được, 2 câu cần Web Search, 1 câu tổng hợp.

#### **3.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Brainstorm có cấu trúc.*

- Mỗi nhóm viết 5 câu vào note, gắn nhãn dự kiến (RAG / Web / Hỗn hợp):

    | # | Câu hỏi | Dự kiến | Kết quả thực tế (điền sau khi test) |
    | :-: | :-- | :-- | :-- |
    | 1 | ... | RAG | |
    | 2 | ... | RAG | |
    | 3 | ... | Web | |
    | 4 | ... | Web | |
    | 5 | ... | Hỗn hợp | |

- GV nhắc: câu "RAG" phải liên quan đến trường đã ingest. Câu "Web" nên là tin mới (2025-2026) hoặc trường chưa có trong AstraDB.

### **4\. Chế tạo mẫu, thử nghiệm và đánh giá (45 phút)**

#### **4.1. Mục tiêu**

- Xây được flow RAG cơ bản từ template.
- Thử nghiệm top-k = 2, 4, 8 và so sánh kết quả.
- Nâng cấp thành flow Agent + 2 tool với chiến lược fallback.
- Test 5 câu và đánh giá độ chính xác.

#### **4.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Scaffold từ template + Thử thách kỹ thuật + So sánh giải pháp.*

**Bước 1 (10 phút) — Dùng template Vector Store RAG có sẵn:**

1. Trong Langflow, New Flow → chọn **Template** → tìm **Vector Store RAG** → Open.
2. Quan sát cấu trúc có sẵn: `Chat Input → [Astra DB retriever] → [Parser] → [Prompt Template] → [Language Model] → Chat Output`. Có 2 nhánh: 1 nhánh cho retrieve, 1 nhánh cho câu hỏi.
3. Cấu hình lại **Astra DB** component: dùng collection `tuyen_sinh_dai_hoc` đã có (token, endpoint từ buổi 4). **Mode**: Retrieve. **Number of Search Results**: `4`.
4. Cấu hình **Language Model**: Provider Google, Model Gemma/Gemini, API key.
5. Chạy thử với 1 câu từ danh sách "dự kiến RAG" trong phiếu. Xác nhận agent trả lời dùng dữ liệu AstraDB.

> ⚠️ **Checkpoint 1:** Nếu agent trả lời "Không tìm thấy thông tin" → kiểm tra lại collection có đúng dữ liệu từ buổi 4 không.

**Bước 2 (8 phút) — Thử nghiệm top-k:**

- Giữ nguyên 1 câu hỏi. Lần lượt chạy với `top-k = 2 → 4 → 8`. Ghi vào **phiếu ghi chú top-k**:

    | Top-k | Độ dài trả lời | Mức độ đầy đủ | Có thừa thông tin lạc đề không? |
    | :-: | :-- | :-- | :-- |
    | 2 | ... | ... | ... |
    | 4 | ... | ... | ... |
    | 8 | ... | ... | ... |

- Chia sẻ trong nhóm: top-k nào tốt nhất cho câu hỏi này? Top-k có phải "càng cao càng tốt" không?

**Bước 3 (20 phút) — Nâng cấp thành Agent + 2 tool với chiến lược fallback:**

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
4. Cấu hình **Web Search** ở **Tool Mode** (giống buổi 3):
    - **Tool Name**: `web_search_tuyen_sinh`.
    - **Tool Description**: "Tìm thông tin tuyển sinh mới trên web chính thống. CHỈ DÙNG SAU KHI rag_tuyen_sinh không có đủ thông tin VÀ user đã đồng ý search web."
5. Dán **Agent Instructions** (chiến lược fallback):

    ```
    Bạn là "Agent Thông tin tuyển sinh".

    QUY TRÌNH BẮT BUỘC cho MỌI câu hỏi tuyển sinh:
    1. GỌI TRƯỚC tool `rag_tuyen_sinh` để tra cứu cơ sở dữ liệu nội bộ.
    2. Đọc kết quả. Nếu ĐỦ thông tin → trả lời user kèm dẫn nguồn nội bộ (tên trường, năm).
    3. Nếu KHÔNG đủ → KHÔNG tự động search web. Thay vào đó, nói với user:
       "Em không tìm thấy đủ thông tin về [X] trong cơ sở dữ liệu nội bộ. Em có muốn anh/chị tìm kiếm trên các trang web uy tín (website trường, Bộ GD&ĐT, báo chính thống) không?"
    4. CHỈ KHI user trả lời đồng ý (yes/có/ok/...) → mới gọi tool `web_search_tuyen_sinh`.
    5. Luôn dẫn link nguồn khi dùng Web Search. Ưu tiên domain .edu.vn, .gov.vn, báo chính thống.
    6. Nếu cả RAG và Web đều không có → thành thật nói "Em không có thông tin về việc này".

    KHÔNG BAO GIỜ bịa điểm chuẩn, học phí, tên ngành không có trong nguồn.
    ```

6. Nối: `Chat Input → Agent`, `Astra DB (Tool)` + `Web Search (Tool)` → Agent (Tools port), `Agent → Chat Output`.

**Bước 4 (7 phút) — Test 5 câu và đánh giá:**

- Chạy Playground với lần lượt 5 câu đã brainstorm. Quan sát:
    - Agent có gọi `rag_tuyen_sinh` TRƯỚC không?
    - Khi RAG không đủ, agent có HỎI user không? Hay tự ý chạy Web Search?
    - Khi user đồng ý, agent có chuyển sang Web Search đúng cách?
- Điền cột "Kết quả thực tế" vào phiếu test. So sánh với dự kiến.

**Bước 5 — Phân hoá nhanh:**

- **HS nhanh (mở rộng):** thử yêu cầu Agent trích xuất ra bảng Markdown so sánh 3 trường. Hoặc thêm 1 câu hỏi đa bước ("Liệt kê các ngành có điểm chuẩn dưới 24 ở 3 trường đã ingest").
- **HS cần hỗ trợ:** GV đi vòng, sửa Tool Description hoặc Agent Instructions nếu agent không làm đúng quy trình.

### **5\. Chia sẻ, thảo luận và điều chỉnh (7 phút)**

#### **5.1. Mục tiêu**

- Các nhóm trình bày phát hiện: RAG có ích khi nào, fallback vận hành ra sao.
- Nhận xét chéo, nêu ý cải tiến prompt.

#### **5.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Gallery Walk mini + Two Stars and a Wish.*

- Mỗi nhóm viết lên bảng (hoặc note chung):
    - **⭐ 2 điều làm tốt** (ví dụ: "Agent chọn đúng RAG trước 5/5 câu", "Fallback hoạt động mượt")
    - **💭 1 điều muốn cải thiện** (ví dụ: "Agent đôi khi trả lời sai tên ngành")
- GV tổng hợp vấn đề phổ biến:
    - Agent không gọi RAG → thường là Tool Description chưa nói "DÙNG TRƯỚC TIÊN".
    - Agent tự ý search web mà không hỏi → thiếu dòng "KHÔNG TỰ ĐỘNG" trong Instructions.
    - RAG trả lời thiếu → tăng top-k, hoặc kiểm tra dữ liệu đã ingest.
- Mời 1-2 nhóm demo 30 giây 1 câu hỏi "fallback đẹp" (agent chủ động hỏi xin phép).

### **6\. Tổng kết (3 phút)**

*Chiến lược dẫn dắt: 3-2-1 Reflection + Cliffhanger.*

- **3-2-1** viết nhanh ra note:
    - **3** điều em học được hôm nay (ví dụ: "RAG là gì", "top-k ảnh hưởng ra sao", "fallback cần prompt kỹ").
    - **2** điều em thấy thú vị (ví dụ: "agent biết hỏi xin phép", "embedding là dãy số").
    - **1** câu hỏi em vẫn thắc mắc (GV thu, trả lời ở đầu buổi sau).
- GV đúc kết dự án 2 đã xong:

  > "Sau 3 buổi, chúng ta đã có một **Agent Thông tin tuyển sinh** đầy đủ: có thư viện riêng (AstraDB), có internet (Web Search), có bộ não biết chọn công cụ đúng lúc (chiến lược fallback). Đây là **một trong 3 agent** của hệ thống cuối khoá."

- **Cliffhanger cho dự án 3:**

  > "Nhưng agent của chúng ta hiện tại **trả lời chung chung**: ai hỏi cũng giống ai. Buổi sau ta sẽ kết hợp dữ liệu của Agent Thông tin với **bảng hỏi cá nhân hoá** từ Agent Chiến lược (buổi 1-2), để tạo ra **Agent Tư vấn** cho từng học sinh khác nhau — 3 HS = 3 tư vấn khác nhau."

- GV nhắc buổi 6: mang lại **file/link bảng hỏi cá nhân hoá** (từ Notion, buổi 2) cho buổi 7.

**Checklist debug buổi 5 (GV phát):**

| Triệu chứng | Nguyên nhân | Giải pháp |
| :-- | :-- | :-- |
| RAG báo "không tìm thấy" dù có dữ liệu | Collection name sai, hoặc dữ liệu chưa ingest | Quay lại AstraDB Console kiểm tra rows |
| Agent không gọi RAG | Tool Description yếu | Thêm "DÙNG TRƯỚC TIÊN" và ví dụ |
| Agent tự ý Web Search | Thiếu ràng buộc fallback | Thêm "KHÔNG TỰ ĐỘNG" vào Instructions |
| Trả lời lẫn lộn nguồn | Không yêu cầu dẫn nguồn | Thêm "LUÔN ghi rõ nguồn: [RAG nội bộ] hay [Web: URL]" |
| Top-k quá lớn làm loãng | Giảm về 3-5 | Chỉnh Number of Search Results |

## HẾT BUỔI 3 (buổi 5 của khoá)


## Thông tin tài liệu

| Được soạn ngày: | 23/04/2026 | Bởi: | Kiro AI |
| :-- | :-- | :-- | :-- |
| Được review ngày: |  | Bởi: |  |
| Được hiệu chỉnh ngày: |  | Bởi: |  |

## Phân bổ các hoạt động

| Nội dung | Chương trình 3 buổi | | Chương trình rút gọn 2 buổi (gợi ý) | |
| :-- | :-: | :-: | :-: | :-: |
| | **Buổi** | **Thời lượng (phút)** | **Buổi** | **Thời lượng (phút)** |
| Web Search — tool thứ hai cho Agent | 1 (buổi 3) | 90 | — | — |
| Vector Database & Embedding — ingest AstraDB | 2 (buổi 4) | 90 | 1 | 90 (gộp ingest rút gọn) |
| RAG cơ bản + chiến lược fallback | 3 (buổi 5) | 90 | 2 | 90 (bỏ ingest tự động) |

