# Syllabus: Xây dựng AI Agent tư vấn tuyển sinh ĐH với Langflow

**Đối tượng:** Học sinh THPT
**Số buổi:** 8 buổi × 90 phút
**Mục tiêu cuối khoá:** Học sinh tự xây dựng một AI Agent trên Langflow Desktop có khả năng tra cứu và tư vấn tuyển sinh đại học Việt Nam. Agent sử dụng model Gemma 4 (free tier, API từ Google AI Studio) kết hợp các tool: tự động tìm kiếm thông tin tuyển sinh từ internet, lưu trữ vào AstraDB (vector database) để phục vụ RAG, đồng thời đẩy dữ liệu có cấu trúc lên Notion thông qua MCP (Model Context Protocol) để quản lý và tra cứu dạng bảng. Hỏi đáp trực tiếp trên Playground/Chat box của Langflow Desktop.

**Công cụ:**
- Langflow Desktop (cài trên máy)
- Model: Gemma 4 — free tier, API key từ Google AI Studio
- Vector Database: AstraDB (component có sẵn trên Langflow)
- Search: Web Search (component có sẵn trên Langflow)
- MCP: Notion MCP Server — để agent đẩy dữ liệu tuyển sinh lên Notion database
- Notion: workspace miễn phí để lưu trữ và quản lý dữ liệu tuyển sinh dạng bảng

**Cơ sở dữ liệu cần thu thập (đẩy lên Notion qua MCP):**

1. **Database "Trường Đại học"** — Tên trường, mã trường, loại hình (công lập/tư thục/quốc tế), địa chỉ, website, mô tả ngắn
2. **Database "Ngành học"** — Tên ngành, mã ngành, trường (liên kết), tổ hợp xét tuyển, phương thức xét tuyển, mô tả ngành, cơ hội nghề nghiệp
3. **Database "Điểm chuẩn"** — Trường + Ngành (liên kết), năm tuyển sinh, điểm chuẩn theo từng phương thức, chỉ tiêu tuyển sinh
4. **Database "Học bổng"** — Tên học bổng, trường (liên kết), đối tượng, giá trị học bổng, điều kiện xét tuyển, thời hạn nộp hồ sơ

---

## Tổng quan khoá học & Nhóm mục tiêu

| # | Buổi | Dự án | Sản phẩm đầu ra | Nhóm mục tiêu | Kiến thức cốt lõi | Mục tiêu theo thang Bloom | Số buổi |
|---|------|-------|-----------------|---------------|---|---|---|
| 1 | 1 | AI đoán nghề nghiệp | Flow chatbot Chat Input → Gemma 4 → Chat Output, học sinh nhập sở thích → AI gợi ý nghề nghiệp | Làm quen AI Agent, Langflow & Kết nối LLM | Khái niệm AI Agent, LLM, prompt; cài đặt Langflow Desktop; giao diện Langflow (canvas, component, flow, playground); Google AI Studio, API key, Gemma 4, component Google Generative AI | - Giải thích được AI Agent là gì, khác chatbot thông thường ở điểm nào · - Nhận biết và gọi tên các thành phần chính trên giao diện Langflow Desktop · - Tạo API key trên Google AI Studio và kết nối thành công với Langflow qua component Google Generative AI · - Tạo flow đơn giản Input → Gemma 4 → Output, chạy thử trên Playground và nhận được phản hồi từ LLM | 1 |
| 2 | 2 | AI phỏng vấn hướng nghiệp | Flow chatbot "phỏng vấn hướng nghiệp" — AI hỏi sở thích, điểm mạnh, mục tiêu → gợi ý 3 ngành phù hợp kèm lý do | Prompt Engineering | System prompt, user prompt; kỹ thuật prompt (role, instruction, few-shot, constraint); component Prompt trên Langflow | - Viết system prompt để định hướng hành vi LLM (ví dụ: "Bạn là chuyên gia tư vấn tuyển sinh ĐH Việt Nam") · - So sánh kết quả trả lời khi thay đổi prompt (không system prompt vs. có system prompt vs. few-shot) · - Thiết kế prompt có constraint (trả lời bằng tiếng Việt, có dẫn nguồn, giới hạn độ dài) và đánh giá hiệu quả | 1 |
| 3 | 3 | Fact-checker AI: Tin đồn tuyển sinh | Flow Agent "fact-checker" — học sinh đưa phát biểu tuyển sinh → agent search web xác minh đúng/sai kèm bằng chứng | Tool & Agent: Web Search | Khái niệm tool trong AI Agent; component Agent, Web Search trên Langflow; cách agent quyết định khi nào dùng tool | - Giải thích được tại sao agent cần tool (LLM không có dữ liệu realtime) · - Xây dựng flow Agent + Web Search tool trên Langflow để agent tự tìm kiếm thông tin tuyển sinh từ internet · - Đánh giá được kết quả search của agent (đúng nguồn, đúng thông tin, có dẫn nguồn) | 1 |
| 4 | 4 | Thư viện tuyển sinh thông minh | Flow RAG — nhúng thông tin tuyển sinh 3-5 trường vào AstraDB, hỏi đáp + so sánh có RAG vs. không RAG | Vector Database & RAG cơ bản | Khái niệm embedding, vector database, RAG; component AstraDB, Embedding trên Langflow; tạo collection trên AstraDB | - Giải thích được tại sao cần vector database (LLM có giới hạn context, dữ liệu cần lưu trữ lâu dài) · - Tạo collection trên AstraDB và kết nối thành công với Langflow · - Xây dựng flow RAG cơ bản: nhúng tài liệu vào AstraDB → truy xuất → đưa vào LLM để trả lời | 1 |
| 5 | 5 | Robot thu thập tin tuyển sinh 24/7 | Flow agent nhận lệnh "Thu thập thông tin trường X" → search web → chunking → lưu AstraDB kèm metadata → hỏi lại để kiểm tra | Agent + RAG: Thu thập & lưu trữ dữ liệu tự động | Kết hợp Agent + Web Search + AstraDB; flow thu thập dữ liệu tự động; chunking, metadata | - Xây dựng flow agent tự động search web → xử lý kết quả → lưu vào AstraDB kèm metadata (tên trường, năm, ngành) · - Chạy agent thu thập dữ liệu điểm chuẩn của ít nhất 5 trường ĐH và kiểm tra dữ liệu đã lưu trong AstraDB · - Điều chỉnh prompt để agent thu thập đúng loại thông tin cần thiết | 1 |
| 6 | 6 | Từ AstraDB lên Notion — sắp xếp kho dữ liệu tuyển sinh | Flow agent đọc dữ liệu từ AstraDB (buổi 5) → trích xuất thành trường có cấu trúc → đẩy lên Notion database qua MCP | MCP & Notion: Quản lý dữ liệu có cấu trúc | Khái niệm MCP (Model Context Protocol); Notion MCP Server; Notion database; kết nối Langflow ↔ Notion qua MCP | - Giải thích được MCP là gì và tại sao cần đẩy dữ liệu lên Notion (dữ liệu có cấu trúc, dễ quản lý) · - Tạo Notion database với đúng cấu trúc (Trường ĐH, Ngành học, Điểm chuẩn, Học bổng) và kết nối với Langflow qua Notion MCP Server · - Xây dựng flow agent search web → đẩy dữ liệu có cấu trúc lên Notion database qua MCP | 1 |
| 7 | 7 | Agent tư vấn tuyển sinh — phiên bản hoàn chỉnh | Flow agent hoàn chỉnh trên Langflow — agent có thể: (1) tra cứu dữ liệu đã lưu trong AstraDB qua RAG, (2) search web nếu chưa có dữ liệu, (3) đọc/ghi Notion qua MCP. Trả lời được các câu hỏi như "So sánh điểm chuẩn ngành CNTT giữa Bách Khoa và KHTN 3 năm gần nhất", "Trường nào có học bổng toàn phần cho ngành Y?" | Tích hợp hoàn chỉnh: Agent tư vấn tuyển sinh | Tích hợp tất cả: Agent + Web Search + AstraDB (RAG) + Notion MCP; thiết kế flow hoàn chỉnh; tối ưu prompt | - Tích hợp tất cả thành phần thành 1 flow hoàn chỉnh: agent có thể search web, tra cứu AstraDB (RAG), đọc/ghi Notion qua MCP · - Agent trả lời được câu hỏi phức tạp (ví dụ: "So sánh điểm chuẩn ngành CNTT giữa Bách Khoa và KHTN 3 năm gần nhất") · - Tối ưu system prompt và cấu hình tool để agent trả lời chính xác, có dẫn nguồn | 1 |
| 8 | 8 | Ngày hội tư vấn tuyển sinh AI | Bảng test report (10+ câu hỏi → kết quả → cải tiến) + bài trình bày demo agent live, giải thích flow, thảo luận đạo đức AI | Thử nghiệm, cải tiến & Trình bày | Testing agent với nhiều kịch bản; debug flow; cải tiến prompt; trình bày sản phẩm; đạo đức AI | - Thử nghiệm agent với ít nhất 10 câu hỏi đa dạng và ghi nhận kết quả (đúng/sai/thiếu) · - Xác định và sửa được ít nhất 2 lỗi hoặc điểm yếu của agent · - Trình bày sản phẩm trước lớp: demo agent, giải thích kiến trúc flow, thảo luận về đạo đức AI (bias trong tư vấn, quyền riêng tư dữ liệu) | 1 |

---

## Vật tư và công cụ cần thiết cho toàn khoá học

**Phần cứng:**
- Máy tính cá nhân (laptop/PC) có kết nối internet | 1/học sinh hoặc 1/nhóm

**Phần mềm:**
- Langflow Desktop (cài trên máy, miễn phí) — [https://langflow.org](https://langflow.org)
- Google AI Studio (tạo API key Gemma 4, free tier) — [https://aistudio.google.com](https://aistudio.google.com)
- AstraDB (vector database, free tier) — [https://astra.datastax.com](https://astra.datastax.com)
- Notion (workspace miễn phí) — [https://notion.so](https://notion.so)
- Notion MCP Server (kết nối Langflow ↔ Notion)
- Trình duyệt web (Chrome/Firefox)

**Tài khoản cần tạo trước khoá học:**
- Google Account (cho Google AI Studio)
- DataStax Account (cho AstraDB)
- Notion Account

---

## Đánh giá chung

**Rubric chung** (áp dụng cho tất cả dự án):

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Quy trình EDP | Không thực hiện đủ các bước EDP hoặc bỏ qua bước quan trọng | Thực hiện đủ các bước EDP nhưng chuyển tiếp chưa rõ ràng, chưa quay lại cải tiến | Thực hiện đủ các bước EDP có logic, có ít nhất 1 lần quay lại cải tiến sau thử nghiệm | Thực hiện EDP trọn vẹn, quay lại cải tiến nhiều lần, ghi nhận quá trình rõ ràng, rút ra bài học |
| Làm việc nhóm | Không phân vai hoặc chỉ 1 người làm, các thành viên không tương tác | Có phân vai nhưng chưa đều, một số thành viên ít đóng góp | Phân vai rõ ràng, các thành viên đều đóng góp, có trao đổi và hỗ trợ nhau | Phân vai hiệu quả, các thành viên chủ động, có thảo luận sâu, giải quyết bất đồng tích cực |
| Trình bày | Không trình bày hoặc trình bày không rõ ràng, không demo | Trình bày cơ bản, có demo nhưng chưa giải thích được cách hoạt động | Trình bày rõ ràng, demo thành công, giải thích được flow và các component chính | Trình bày chuyên nghiệp, demo mượt mà, giải thích sâu, trả lời Q&A tốt, có phản biện |

**Peer feedback** (đặc biệt dự án 7 và 8): Mẫu "Điều mình thích — Điều mình thắc mắc — Gợi ý cải tiến"

---

## Dự án 1: AI đoán nghề nghiệp — 1 buổi

**Câu hỏi dẫn dắt:** "Liệu AI có thể gợi ý nghề nghiệp phù hợp với sở thích của em không?"

**Mục tiêu (AI):**
- Giải thích được AI Agent là gì, khác chatbot thông thường ở điểm nào
- Cài đặt Langflow Desktop, tạo API key trên Google AI Studio, kết nối Gemma 4
- Xây dựng flow chatbot "tư vấn nghề nghiệp" trên Langflow và chạy thử trên Playground

**Sản phẩm đầu ra:** Flow chatbot trên Langflow kết nối Chat Input → Gemma 4 → Chat Output — học sinh nhập sở thích, AI gợi ý nghề nghiệp phù hợp. Chạy trực tiếp trên Playground.

**Phần cứng:** Máy tính cá nhân (laptop/PC) | 1/học sinh hoặc 1/nhóm
**Phần mềm:** Langflow Desktop, Google AI Studio (tạo API key), trình duyệt web

**Phân hoá:**
- Yêu cầu cơ bản: Flow hoạt động, AI gợi ý được nghề nghiệp từ sở thích của học sinh
- Yêu cầu nâng cao: Thử với nhiều bạn khác nhau, so sánh kết quả, đánh giá xem AI có thiên vị không

**Rubric dự án:**

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Sản phẩm | Flow không chạy được hoặc LLM không phản hồi | Flow chạy được, AI trả lời nhưng chưa liên quan đến sở thích đã nhập | Flow chạy tốt, AI gợi ý nghề nghiệp phù hợp với sở thích, câu trả lời rõ ràng | Flow chạy tốt, AI gợi ý nghề nghiệp chính xác + giải thích lý do + thử với nhiều bạn và so sánh kết quả |
| Code (Flow) | Không tạo được flow hoặc thiếu component chính | Có đủ 3 component (Input, LLM, Output) nhưng kết nối chưa đúng hoặc chưa cấu hình API key | Flow đúng cấu trúc, API key kết nối thành công, các component kết nối chính xác | Flow hoàn chỉnh + thêm component Memory hoặc tuỳ chỉnh nâng cao khác |

---

## Dự án 2: AI phỏng vấn hướng nghiệp — 1 buổi

**Câu hỏi dẫn dắt:** "Làm sao để AI biết hỏi đúng câu hỏi giúp em chọn ngành phù hợp?"

**Mục tiêu (AI):**
- Viết prompt để AI chủ động hỏi ngược lại học sinh (thay vì chỉ trả lời)
- Sử dụng kỹ thuật role + instruction + constraint trong prompt
- So sánh kết quả khi thay đổi prompt (không system prompt vs. có role vs. few-shot)
- Đánh giá hiệu quả prompt qua chất lượng cuộc hội thoại

**Sản phẩm đầu ra:** Flow chatbot "phỏng vấn hướng nghiệp" trên Langflow — AI hỏi học sinh về sở thích, điểm mạnh, mục tiêu, sau đó gợi ý 3 ngành phù hợp kèm lý do.

**Phần cứng:** Máy tính cá nhân (laptop/PC) | 1/học sinh hoặc 1/nhóm
**Phần mềm:** Langflow Desktop, Google AI Studio

**Phân hoá:**
- Yêu cầu cơ bản: AI hỏi được ít nhất 3 câu hỏi hướng nghiệp và gợi ý ngành phù hợp
- Yêu cầu nâng cao: AI hỏi có logic (câu sau dựa trên câu trả lời trước), gợi ý ngành kèm trường cụ thể ở Việt Nam

**Rubric dự án:**

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Sản phẩm | AI không hỏi ngược hoặc chỉ trả lời chung chung, không gợi ý ngành | AI hỏi được 1-2 câu nhưng câu hỏi chưa liên quan, gợi ý ngành chưa sát sở thích | AI hỏi ít nhất 3 câu hướng nghiệp có logic, gợi ý 3 ngành phù hợp kèm lý do | AI hỏi có chiều sâu (câu sau dựa trên câu trước), gợi ý ngành + trường cụ thể + giải thích tại sao phù hợp |
| Code (Prompt) | Không có system prompt hoặc prompt không thay đổi hành vi AI | Có system prompt nhưng AI vẫn chỉ trả lời thay vì hỏi ngược, hoặc prompt quá đơn giản | System prompt rõ ràng với role + instruction + constraint, AI hỏi ngược đúng như thiết kế | System prompt tối ưu với few-shot example, constraint chặt chẽ, so sánh được ít nhất 2 phiên bản prompt và giải thích phiên bản nào tốt hơn |

---

## Dự án 3: Fact-checker AI: Tin đồn tuyển sinh đúng hay sai? — 1 buổi

**Câu hỏi dẫn dắt:** "Trên mạng có nhiều thông tin tuyển sinh — làm sao để AI giúp mình kiểm chứng?"

**Mục tiêu (AI):**
- Giải thích được tại sao agent cần tool (LLM không có dữ liệu realtime)
- Xây dựng flow Agent + Web Search trên Langflow
- Agent kiểm chứng các "tin đồn tuyển sinh" bằng cách search web và đối chiếu
- Đánh giá được kết quả search của agent (đúng nguồn, đúng thông tin, có dẫn nguồn)

**Sản phẩm đầu ra:** Flow Agent "fact-checker" trên Langflow — học sinh đưa ra các phát biểu về tuyển sinh (đúng/sai), agent search web để xác minh và trả lời kèm bằng chứng từ nguồn.

**Phần cứng:** Máy tính cá nhân (laptop/PC) | 1/học sinh hoặc 1/nhóm
**Phần mềm:** Langflow Desktop, Google AI Studio

**Phân hoá:**
- Yêu cầu cơ bản: Agent kiểm chứng được 3 phát biểu tuyển sinh, có dẫn nguồn
- Yêu cầu nâng cao: Agent phân tích mức độ tin cậy của nguồn (trang chính thức vs. diễn đàn), giải thích tại sao nguồn này đáng tin hơn nguồn kia

**Rubric dự án:**

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Sản phẩm | Agent không search được hoặc trả lời không liên quan đến phát biểu cần kiểm chứng | Agent search được nhưng chỉ kiểm chứng 1-2 phát biểu, thiếu dẫn nguồn | Agent kiểm chứng được ít nhất 3 phát biểu, trả lời đúng/sai kèm bằng chứng và dẫn nguồn | Agent kiểm chứng 3+ phát biểu, phân tích mức độ tin cậy của nguồn, chỉ ra sự khác biệt giữa nguồn chính thức và không chính thức |
| Code (Flow) | Không tạo được flow Agent hoặc thiếu Web Search tool | Có flow Agent + Web Search nhưng agent không tự quyết định khi nào search (phải ép thủ công) | Flow Agent + Web Search hoạt động tốt, agent tự quyết định search khi cần, prompt rõ ràng | Flow hoàn chỉnh, agent search chính xác, prompt có instruction yêu cầu dẫn nguồn + đánh giá độ tin cậy |

---

## Dự án 4: Thư viện tuyển sinh thông minh — 1 buổi

**Câu hỏi dẫn dắt:** "Nếu có một thư viện chứa tất cả thông tin tuyển sinh, làm sao AI tìm đúng thông tin em cần trong vài giây?"

**Mục tiêu (AI):**
- Giải thích được tại sao cần vector database (tìm kiếm theo nghĩa, không phải từ khoá)
- Tạo AstraDB collection, kết nối với Langflow, nhúng nhiều đoạn thông tin tuyển sinh
- Xây dựng flow RAG cơ bản và so sánh kết quả có RAG vs. không có RAG

**Sản phẩm đầu ra:** Flow RAG "thư viện tuyển sinh" trên Langflow — nhúng thông tin tuyển sinh của 3-5 trường ĐH (giáo viên chuẩn bị sẵn file text/PDF) vào AstraDB, hỏi đáp và so sánh chất lượng câu trả lời khi có RAG vs. không có RAG.

**Phần cứng:** Máy tính cá nhân (laptop/PC) | 1/học sinh hoặc 1/nhóm
**Phần mềm:** Langflow Desktop, Google AI Studio, AstraDB (đăng ký free tier)

**Phân hoá:**
- Yêu cầu cơ bản: Nhúng thông tin 3 trường vào AstraDB, hỏi 3 câu và AI trả lời đúng từ dữ liệu
- Yêu cầu nâng cao: So sánh kết quả có RAG vs. không RAG với cùng câu hỏi, ghi nhận và giải thích sự khác biệt

**Rubric dự án:**

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Sản phẩm | Không nhúng được dữ liệu hoặc AI không trả lời được từ AstraDB | Nhúng được dữ liệu 1-2 trường, AI trả lời nhưng đôi khi không chính xác hoặc thiếu thông tin | Nhúng dữ liệu 3+ trường, AI trả lời chính xác từ dữ liệu đã nhúng cho ít nhất 3 câu hỏi | Nhúng 3+ trường, AI trả lời chính xác + so sánh rõ ràng kết quả có RAG vs. không RAG, giải thích được sự khác biệt |
| Code (Flow) | Không tạo được flow RAG hoặc thiếu component chính (Embedding, AstraDB, Retriever) | Có đủ component nhưng kết nối chưa đúng hoặc AstraDB chưa nhận dữ liệu | Flow RAG hoàn chỉnh: Embedding → AstraDB → Retriever → LLM, dữ liệu nhúng thành công, truy xuất đúng | Flow RAG tối ưu, có thêm component Text Splitter để chia nhỏ tài liệu, metadata rõ ràng |

---

## Dự án 5: Robot thu thập tin tuyển sinh 24/7 — 1 buổi

**Câu hỏi dẫn dắt:** "Mùa tuyển sinh thông tin thay đổi liên tục — làm sao để AI tự cập nhật mà mình không cần làm gì?"

**Mục tiêu (AI):**
- Kết hợp Agent + Web Search + AstraDB thành flow thu thập dữ liệu tự động
- Hiểu chunking và metadata khi lưu dữ liệu vào vector database
- Agent thu thập thông tin tuyển sinh mới nhất và lưu có tổ chức
- Điều chỉnh prompt để agent thu thập đúng loại thông tin cần thiết

**Sản phẩm đầu ra:** Flow agent "robot thu thập" trên Langflow — nhận lệnh "Thu thập thông tin tuyển sinh trường X" → search web → chia nhỏ kết quả (chunking) → lưu vào AstraDB kèm metadata → xác nhận đã lưu. Sau đó hỏi lại để kiểm tra dữ liệu đã thu thập.

**Phần cứng:** Máy tính cá nhân (laptop/PC) | 1/học sinh hoặc 1/nhóm
**Phần mềm:** Langflow Desktop, Google AI Studio, AstraDB

**Phân hoá:**
- Yêu cầu cơ bản: Agent thu thập và lưu thông tin tuyển sinh 5 trường vào AstraDB, hỏi lại được
- Yêu cầu nâng cao: Agent phát hiện thông tin trùng lặp, chỉ lưu thông tin mới; metadata chi tiết (tên trường, ngành, năm, phương thức)

**Rubric dự án:**

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Sản phẩm | Agent không search được hoặc không lưu được dữ liệu vào AstraDB | Agent search và lưu được dữ liệu 1-2 trường nhưng thiếu metadata hoặc dữ liệu không chính xác | Agent thu thập dữ liệu 5+ trường, lưu vào AstraDB kèm metadata, hỏi lại trả lời đúng | Agent thu thập 5+ trường, metadata chi tiết, phát hiện trùng lặp, dữ liệu có tổ chức rõ ràng |
| Code (Flow) | Không kết nối được Agent với Web Search và AstraDB | Có flow nhưng agent chỉ search hoặc chỉ lưu, chưa kết hợp được cả hai trong cùng flow | Flow hoàn chỉnh: Agent → Web Search → chunking → Embedding → AstraDB, prompt hướng dẫn agent thu thập đúng thông tin | Flow tối ưu, prompt có instruction chi tiết về format dữ liệu cần thu thập, chunking hợp lý, metadata phong phú |

---

## Dự án 6: Từ AstraDB lên Notion — sắp xếp kho dữ liệu tuyển sinh — 1 buổi

**Câu hỏi dẫn dắt:** "Dữ liệu tuyển sinh đã nằm trong AstraDB rồi — làm sao để AI tự sắp xếp lại thành bảng gọn gàng trên Notion?"

**Mục tiêu (AI):**
- Giải thích được MCP là gì và tại sao cần dữ liệu có cấu trúc (AstraDB lưu text thô cho RAG, Notion lưu dạng bảng có cột rõ ràng cho tra cứu chính xác)
- Tạo Notion database với đúng cấu trúc, kết nối Langflow qua Notion MCP Server
- Agent đọc dữ liệu từ AstraDB (đã thu thập ở buổi 5) → dùng LLM trích xuất thành các trường có cấu trúc → đẩy lên Notion database qua MCP

**Sản phẩm đầu ra:** Flow agent trên Langflow — đọc dữ liệu tuyển sinh đã lưu trong AstraDB → trích xuất thành các trường có cấu trúc (tên trường, ngành, điểm chuẩn, năm) → tạo entry trên Notion database qua MCP. Kiểm tra trực tiếp trên Notion thấy dữ liệu đã được sắp xếp thành bảng.

**Phần cứng:** Máy tính cá nhân (laptop/PC) | 1/học sinh hoặc 1/nhóm
**Phần mềm:** Langflow Desktop, Google AI Studio, AstraDB (dữ liệu từ buổi 5), Notion (workspace miễn phí), Notion MCP Server

**Phân hoá:**
- Yêu cầu cơ bản: Agent đọc AstraDB và đẩy được thông tin 3 trường lên 1 Notion database (Điểm chuẩn)
- Yêu cầu nâng cao: Đẩy lên 2+ database Notion (Trường ĐH + Điểm chuẩn) với dữ liệu liên kết giữa các bảng

**Rubric dự án:**

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Sản phẩm | Không kết nối được Notion hoặc không đẩy được dữ liệu lên | Đẩy được dữ liệu 1-2 trường lên Notion nhưng thiếu trường hoặc sai cấu trúc | Đẩy dữ liệu 3+ trường lên Notion database đúng cấu trúc, dữ liệu chính xác so với AstraDB | Đẩy 3+ trường lên 2+ database Notion có liên kết, dữ liệu nhất quán, Notion có view lọc/sắp xếp hữu ích |
| Code (Flow) | Không tạo được flow kết nối AstraDB → Notion hoặc thiếu MCP Server | Có flow nhưng agent chỉ đọc AstraDB hoặc chỉ ghi Notion, chưa kết hợp được | Flow hoàn chỉnh: Agent đọc AstraDB → LLM trích xuất cấu trúc → ghi Notion qua MCP, prompt rõ ràng | Flow tối ưu, prompt có instruction chi tiết về format trích xuất, xử lý được nhiều loại dữ liệu (điểm chuẩn, học bổng, ngành) |

---

## Dự án 7: Agent tư vấn tuyển sinh — phiên bản hoàn chỉnh — 1 buổi

**Câu hỏi dẫn dắt:** "Giờ mình đã có tất cả mảnh ghép — làm sao ráp lại thành một agent tư vấn tuyển sinh thực sự?"

**Mục tiêu (AI):**
- Tích hợp tất cả thành phần đã học (Agent + Web Search + AstraDB/RAG + Notion MCP) thành 1 flow hoàn chỉnh
- Agent trả lời được câu hỏi phức tạp về tuyển sinh (so sánh, tổng hợp, gợi ý)
- Tối ưu system prompt và cấu hình tool để agent trả lời chính xác, có dẫn nguồn

**Sản phẩm đầu ra:** Flow agent hoàn chỉnh trên Langflow — agent có thể: (1) tra cứu dữ liệu đã lưu trong AstraDB qua RAG, (2) search web nếu chưa có dữ liệu, (3) đọc/ghi Notion qua MCP. Trả lời được các câu hỏi như "So sánh điểm chuẩn ngành CNTT giữa Bách Khoa và KHTN 3 năm gần nhất", "Trường nào có học bổng toàn phần cho ngành Y?"

**Phần cứng:** Máy tính cá nhân (laptop/PC) | 1/học sinh hoặc 1/nhóm
**Phần mềm:** Langflow Desktop, Google AI Studio, AstraDB, Notion, Notion MCP Server

**Phân hoá:**
- Yêu cầu cơ bản: Flow tích hợp hoạt động, agent trả lời được 5 câu hỏi tuyển sinh từ nhiều nguồn (AstraDB + web)
- Yêu cầu nâng cao: Agent tự quyết định dùng tool nào (RAG trước, web search nếu thiếu), trả lời có dẫn nguồn, cập nhật Notion khi có thông tin mới

**Rubric dự án:**

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Sản phẩm | Flow không hoạt động hoặc agent chỉ dùng được 1 tool | Agent dùng được 2 tool nhưng trả lời chưa chính xác hoặc thiếu thông tin, trả lời được 2-3 câu hỏi | Agent dùng đủ 3 tool (RAG + Web Search + Notion MCP), trả lời chính xác 5+ câu hỏi tuyển sinh kèm dẫn nguồn | Agent trả lời 5+ câu hỏi phức tạp (so sánh, tổng hợp), tự quyết định tool phù hợp, cập nhật Notion khi có thông tin mới |
| Code (Flow) | Không tích hợp được các component hoặc flow bị lỗi kết nối | Có flow nhưng các tool hoạt động rời rạc, agent chưa biết khi nào dùng tool nào | Flow tích hợp hoàn chỉnh, agent có system prompt rõ ràng hướng dẫn khi nào dùng RAG / web search / Notion | Flow tối ưu, system prompt có chiến lược rõ (ưu tiên RAG → fallback web search → ghi Notion), xử lý được edge case (không tìm thấy, dữ liệu mâu thuẫn) |

---

## Dự án 8: Ngày hội tư vấn tuyển sinh AI — 1 buổi

**Câu hỏi dẫn dắt:** "Agent của em đã sẵn sàng tư vấn cho người thật chưa? Hãy thử nghiệm và trình bày!"

**Mục tiêu (AI):**
- Thử nghiệm agent với ít nhất 10 câu hỏi đa dạng (câu đơn giản, câu so sánh, câu bẫy), ghi nhận kết quả
- Xác định và sửa được ít nhất 2 lỗi hoặc điểm yếu của agent (trả lời sai, thiếu nguồn, không tìm được thông tin)
- Trình bày sản phẩm trước lớp: demo agent live, giải thích kiến trúc flow, thảo luận về đạo đức AI (bias trong tư vấn, quyền riêng tư dữ liệu)

**Sản phẩm đầu ra:** (1) Bảng test report: 10+ câu hỏi → kết quả agent → đánh giá đúng/sai/thiếu → cải tiến đã thực hiện. (2) Bài trình bày trước lớp: demo agent live, giải thích flow, thảo luận về bias và quyền riêng tư trong AI tư vấn.

**Phần cứng:** Máy tính cá nhân (laptop/PC) | 1/học sinh hoặc 1/nhóm
**Phần mềm:** Langflow Desktop, Google AI Studio, AstraDB, Notion, Notion MCP Server

**Phân hoá:**
- Yêu cầu cơ bản: Test 10 câu, sửa 2 lỗi, trình bày demo + giải thích flow
- Yêu cầu nâng cao: Test với người ngoài nhóm (bạn khác lớp, giáo viên), phân tích bias trong câu trả lời, đề xuất cải tiến dài hạn

**Rubric dự án:**

| Tiêu chí | Chưa đạt (1) | Đạt (2) | Tốt (3) | Xuất sắc (4) |
|----------|---------------|---------|---------|---------------|
| Sản phẩm | Test dưới 5 câu hoặc không ghi nhận kết quả, không sửa lỗi | Test 5-9 câu, ghi nhận kết quả nhưng chỉ sửa 1 lỗi hoặc sửa chưa hiệu quả | Test 10+ câu, ghi nhận đầy đủ, sửa 2+ lỗi có hiệu quả, agent cải thiện rõ rệt | Test 10+ câu với người ngoài nhóm, phân tích pattern lỗi, sửa 3+ lỗi, đề xuất cải tiến dài hạn |
| Code (Cải tiến) | Không thay đổi gì sau khi test hoặc thay đổi làm agent tệ hơn | Điều chỉnh prompt nhưng chưa giải quyết được lỗi chính | Điều chỉnh prompt + cấu hình tool hiệu quả, agent trả lời chính xác hơn sau cải tiến | Cải tiến có hệ thống (prompt + tool + dữ liệu), đo lường trước/sau, giải thích tại sao cải tiến hiệu quả |
| Trình bày & Đạo đức AI | Không trình bày hoặc chỉ demo không giải thích | Demo agent nhưng không giải thích flow hoặc không đề cập đạo đức AI | Demo agent + giải thích kiến trúc flow rõ ràng + thảo luận ít nhất 1 vấn đề đạo đức AI | Demo chuyên nghiệp + giải thích flow chi tiết + thảo luận sâu về bias, quyền riêng tư, sử dụng AI có trách nhiệm + Q&A |

---
