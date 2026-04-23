# AGENT CHIẾN LƯỢC ĐỊNH HƯỚNG CHỌN TRƯỜNG VÀ NGÀNH

Khoá: Xây dựng hệ thống AI Agent tư vấn tuyển sinh với Langflow

Cấp học: THPT

Thời lượng: 2 buổi x 90 phút

## Mô tả bài học

Trong bài học này, học sinh sẽ tìm hiểu kiến thức về **AI Agent vs Chatbot, Langflow, Prompt Engineering, Notion Integration và Notion bundle trong Langflow**, và vận dụng để thực hiện dự án **Agent Chiến lược định hướng chọn trường và ngành**. Dự án giải quyết vấn đề thực tế: ban tư vấn tuyển sinh của trường không đủ người để gặp riêng từng HS, cần AI hỗ trợ hỏi thu thập thông tin và sinh bảng hỏi cá nhân hoá giúp HS tự phân tích bản thân và mục tiêu.

## I. Yêu cầu cần đạt

### **1\. Kiến thức**

Thực hiện bài học này học sinh sẽ khám phá các kiến thức:

- Khái niệm **AI Agent** vs **Chatbot** — vai trò của **tool** trong agent.
- **Langflow Desktop** — giao diện (canvas, components, flow, Playground, component inspection panel), cách kéo thả và nối component.
- **Google AI Studio, API key, model Gemma 4** — cách lấy API key và cấu hình Model Provider trong Langflow Settings.
- Ba component cơ bản: **Chat Input**, **Language Model**, **Chat Output**.
- **Prompt Engineering** — phân biệt user prompt vs system message; 4 thành phần prompt (role, instruction, constraint, few-shot); component **Prompt Template** với biến động.
- **Notion** — page, parent page; **Notion Internal Integration** và integration token; chia sẻ page với integration.
- **Notion bundle** trong Langflow — component **Create Page**, **Add Content to Page**.
- **Tool Mode** — biến component thành tool cho Agent; Tool Name, Tool Description.
- Component **Agent** trong Langflow — LLM + Instructions + Tools + Chat Memory tích hợp sẵn.
- **Cá nhân hoá (personalization)** — output khác nhau cho mỗi người dùng dựa trên dữ liệu của họ.

### **2\. Năng lực**

Thực hiện bài học này học sinh sẽ rèn luyện và phát triển các năng lực với các biểu hiện:

*Năng lực đặc thù:*

- Giải thích được AI Agent là gì, khác chatbot thông thường ở điểm nào.
- Nhận biết và gọi tên các thành phần chính trên giao diện Langflow Desktop.
- Tạo API key trên Google AI Studio và cấu hình Model Provider trong Langflow Settings.
- Xây dựng flow `Chat Input → Language Model (Gemma 4) → Chat Output` và chạy thử trên Playground (bài tập nhỏ làm quen).
- Viết system message với role + instruction + constraint để định hướng hành vi LLM.
- So sánh kết quả khi thay đổi prompt (không system message vs. có system message vs. few-shot).
- Giải thích được tại sao agent cần tool để tương tác với công cụ bên ngoài (Notion).
- Tạo Notion Internal Integration, lấy integration token, chia sẻ page với integration.
- Sử dụng component Create Page và Add Content to Page (Notion bundle) ở Tool Mode làm tool cho Agent.
- Xây dựng flow Agent Chiến lược hoàn chỉnh: Chat Input → Agent (Language Model + Notion bundle tools) → Chat Output.
- Agent hỏi câu mở đầu → sinh bảng hỏi cá nhân hoá → tạo page/entry trên Notion qua Notion bundle.
- Đánh giá bảng hỏi cá nhân hoá thực sự khác nhau giữa 3 học sinh mẫu.

*Năng lực chung:*

- **Giao tiếp và hợp tác:** Brainstorm theo nhóm danh sách câu hỏi mở đầu, trình bày và lắng nghe nhóm khác, chốt danh sách chung.
- **Giải quyết vấn đề:** Debug flow khi agent không gọi tool, không ghi được Notion, hoặc bảng hỏi không cá nhân hoá.
- **Tự chủ và tự học:** Tự chọn danh sách câu hỏi mở đầu cuối cùng (dùng của GV, tự nghĩ, hoặc trộn), chủ động đối chiếu checklist khi gặp lỗi.

### **3\. Phẩm chất**

Bài học này tạo điều kiện để học sinh phát triển những phẩm chất với các biểu hiện:

- **Trách nhiệm:** Nhận thức được tầm quan trọng của việc bảo mật API key và Integration Token, không chia sẻ công khai; hiểu trách nhiệm khi xử lý dữ liệu cá nhân của người khác trên Notion.
- **Trung thực:** Trung thực khi test 3 hồ sơ học sinh — ghi nhận đúng các bảng hỏi tạo ra, không "chỉnh" prompt chỉ để bảng hỏi có vẻ khác nhau.
- **Chăm chỉ:** Kiên trì debug khi agent không gọi đúng tool, không bỏ cuộc khi gặp lỗi "page not found".

## II. Sản phẩm

**Cuối buổi 1:**

- Flow chatbot cá nhân trên Langflow (chủ đề HS tự chọn: chuyên gia giải mã giấc mơ, đầu bếp AI, dịch ngôn ngữ Gen Z...), chạy được trên Playground.
- 3 phiên bản prompt khác nhau đã test trên cùng một input, có ghi chú so sánh.
- Danh sách câu hỏi mở đầu (5-10 câu) cho Agent Chiến lược, do HS/nhóm tự chọn.

**Cuối buổi 2:**

- Flow **Agent Chiến lược** chạy trên Playground: agent hỏi các câu mở đầu đã chọn → sinh bảng hỏi cá nhân hoá 6-8 câu chuyên sâu → tạo page mới trên Notion kèm toàn bộ bảng hỏi.
- Test với **ít nhất 3 hồ sơ HS khác nhau** → 3 bảng hỏi trên Notion với nội dung khác nhau rõ rệt.

## III. Thiết bị dạy học và học liệu

Các nguyên vật liệu cần chuẩn bị cho một nhóm:

| Nguyên vật liệu                                | Đơn vị | Số lượng |
| :------------------------------------------------- | :-------: | :---------: |
| Sticky note (brainstorm câu hỏi mở đầu)       |  Xấp  |      1      |
| Bút dạ / bút màu                              |   Cái   |     2-3     |

Các công cụ và thiết bị cần chuẩn bị cho một nhóm:

| Công cụ và thiết bị                              | Đơn vị | Số lượng |
| :---------------------------------------------------- | :-------: | :---------: |
| Máy tính có internet (Langflow Desktop cài sẵn) |   Cái   |     1-2     |
| API key Gemma 4 (Gemini API) — mỗi HS 1 key        |   Cái   |      1      |
| Tài khoản Notion (free tier, HS đăng ký trước buổi 2) | Cái |      1      |
| Máy chiếu / TV để GV demo và so sánh bảng hỏi  |   Cái   |  1 (cho lớp)  |

Các tài liệu, phiếu học tập:

| Đường dẫn tài liệu, phiếu học tập, video,...                            |
| :----------------------------------------------------------------------------------- |
| Giáo trình dự án 1: `textbook-agent-chien-luoc.md`                                 |
| File `final-project-agent-tu-van-tuyen-sinh.md` (tham khảo ví dụ bảng hỏi cá nhân hoá) |
| Danh sách 8 câu hỏi mở đầu gợi ý của GV (in ra phát cho mỗi nhóm khi cần tham khảo) |
| 3 hồ sơ học sinh mẫu để test cuối buổi 2 (HS1 khối B thích Sinh, HS2 khối A thích Toán, HS3 khối D thích Văn) |
| Checklist debug lỗi thường gặp (in ra phát khi HS gặp khó khăn ở buổi 2)           |

## IV. Tiến trình dạy học

### **1\. Xác định vấn đề (15 phút)**

#### **1.1. Mục tiêu**

- Xác định được vấn đề thực tế: ban tư vấn tuyển sinh quá tải, không thể gặp riêng từng HS — cần AI hỗ trợ sinh bảng hỏi cá nhân hoá cho từng HS.
- Hình dung được sản phẩm cuối của dự án 2 buổi: Agent Chiến lược biết hỏi câu mở đầu rồi sinh bảng hỏi riêng cho mỗi HS và đẩy lên Notion.
- Đặt được các câu hỏi để làm rõ vấn đề.

#### **1.2. Tổ chức thực hiện**

**1.2.1. Khởi động (5 phút) — Case study: "Ban tư vấn tuyển sinh của trường"**

*Chiến lược dẫn dắt: Case study / Tình huống thực* (THPT ưu tiên chiến lược này).

GV mở đầu bằng một tình huống rất gần gũi với HS lớp 11-12:

> "Thầy/cô có 3 số liệu cho các em:
>
> 1. Trường mình có bao nhiêu HS khối 11-12 đang cần tư vấn tuyển sinh?
> 2. Ban tư vấn có bao nhiêu người?
> 3. Nếu mỗi HS cần 30 phút gặp riêng, cả ban tư vấn cần bao nhiêu giờ?"

GV cho HS tự nhẩm 30 giây, rồi hỏi:

> "Theo các em, ban tư vấn có gặp riêng được từng HS không? Thế các HS không được gặp thường làm gì?"

Gợi ý HS trả lời: tự tra Google, hỏi anh chị, dùng ChatGPT... Nhưng các công cụ này **hỏi ai cũng giống ai** — cùng 10 câu cứng nhắc cho HS mê Toán lẫn HS mê Văn.

GV chốt vấn đề:

> "Vấn đề là: **một bộ câu hỏi không thể đủ cho mọi người khác nhau**. Người tư vấn giỏi hỏi vài câu mở đầu, rồi *dựa vào câu trả lời* mà đặt câu hỏi chuyên sâu khác nhau. Câu hỏi cho em mê Sinh sẽ khác câu hỏi cho em mê Toán.
>
> Hôm nay, **các em sẽ xây một AI biết làm đúng việc đó**."

**1.2.2. Giao nhiệm vụ (10 phút) — Trình bày sản phẩm cuối 2 buổi**

*Chiến lược dẫn dắt: Driving Question + Ví dụ sản phẩm mẫu (Exemplar)*.

GV chiếu **câu hỏi dẫn dắt** (driving question) của dự án lên bảng:

> "Làm sao để AI biết hỏi một vài câu, rồi tự sinh ra một bảng hỏi dành riêng cho mỗi học sinh?"

GV mở file `final-project-agent-tu-van-tuyen-sinh.md` hoặc chiếu slide có **2 ví dụ bảng hỏi cá nhân hoá** — một cho HS thích Sinh học, một cho HS thích Toán. Để HS thấy rõ sự khác biệt.

GV giải thích lộ trình 2 buổi:

- **Buổi 1 (hôm nay):** làm quen Langflow qua một bài tập nhỏ — chatbot cá nhân (chủ đề tuỳ các em). Cuối buổi: brainstorm câu hỏi mở đầu cho Agent Chiến lược.
- **Buổi 2:** xây Agent Chiến lược hoàn chỉnh — hỏi các câu đã brainstorm → sinh bảng hỏi cá nhân hoá → lưu Notion. Test với 3 HS mẫu.

GV giao nhiệm vụ buổi 1 cụ thể:

> "Buổi hôm nay, mỗi em (hoặc nhóm 2) cần làm 2 việc:
>
> 1. Xây một chatbot cá nhân trên Langflow, chủ đề tuỳ chọn. Test 3 phiên bản prompt khác nhau.
> 2. Cùng nhóm, brainstorm 5-10 câu hỏi mở đầu mà Agent Chiến lược nên hỏi."

### **2\. Nghiên cứu kiến thức nền (30 phút)**

#### **2.1. Mục tiêu**

- Phân biệt được Chatbot vs Agent, giải thích vì sao agent cần tool.
- Nhận biết các vùng chính của Langflow Desktop (canvas, sidebar, inspection panel, Playground).
- Hiểu khái niệm API key và cách lấy từ Google AI Studio.
- Biết 3 component cơ bản (Chat Input, Language Model, Chat Output) và cách nối chúng.
- Hiểu khái niệm prompt, system message, và 4 thành phần prompt (role, instruction, constraint, few-shot).

#### **2.2. Tổ chức thực hiện**

**2.2.1. Tìm hiểu kiến thức 1: LLM, Chatbot, AI Agent (8 phút)**

*Chiến lược dẫn dắt: Phép tương tự (Analogy)*.

GV vẽ/chiếu 3 hình trên bảng:

- 🧠 (Bộ não) — LLM.
- 🧠 + 🗨️ (Bộ não + miệng) — Chatbot.
- 🧠 + 🗨️ + 🦾 (Bộ não + miệng + tay chân) — AI Agent.

GV giải thích bằng phép tương tự:

> "LLM là bộ não đơn thuần — biết suy nghĩ và sinh chữ, nhưng không tự làm được gì. Chatbot = LLM + giao diện chat, chỉ biết nói chuyện. Agent = LLM + tool, có tay chân để làm việc ngoài phần mềm."

Câu hỏi kiểm tra nhanh (câu hỏi đóng):

> "Các em cho thầy/cô 3 ví dụ: (A) Hỏi AI 'Thủ đô nước Pháp là gì?'. (B) Nhờ AI tìm điểm chuẩn 2024 của ĐHBK Hà Nội. (C) Nhờ AI tạo page mới trên Notion. Cái nào chatbot làm được, cái nào cần agent có tool?"

Đáp án mong đợi: A chatbot làm được (kiến thức sẵn); B và C cần agent có tool (Web Search / Notion).

GV kết: "Hôm nay buổi 1, các em xây **chatbot** (làm quen). Buổi 2, nâng cấp thành **agent thật sự** — gắn tool Notion."

**2.2.2. Tìm hiểu kiến thức 2: Giao diện Langflow (5 phút)**

*Chiến lược dẫn dắt: Demo trực quan*.

GV mở Langflow Desktop trên máy chiếu, chỉ từng vùng:

- **Sidebar** (trái): danh sách component. GV kéo 1 component bất kỳ ra canvas để minh hoạ.
- **Canvas** (giữa): nơi thả component và nối dây.
- **Component inspection panel** (phải, xuất hiện khi click component): chỉnh thông số.
- **Playground** (nút góc phải trên): chat thử với flow.
- **Settings** (icon avatar góc phải trên): cấu hình Model Providers.

Câu hỏi: "Nếu em muốn đổi model từ Gemma sang Gemini, em vào đâu?" Đáp: Settings → Model Providers.
> "Để nối 2 component, các em nhìn mỗi component có các **chấm tròn** (port) ở cạnh trái (input) và cạnh phải (output). Cách làm:
>
> 1. **Kéo component:** Click giữ component từ sidebar, thả vào canvas.
> 2. **Nối dây:** Rê chuột từ port output (phải) của component A → kéo đến port input (trái) của component B. Khi thấy đường nối sáng lên, thả chuột.
> 3. **Xoá nối:** Click vào đường nối → nhấn Delete hoặc Backspace.
> 4. **Di chuyển:** Click giữ component rồi kéo để sắp xếp gọn trên canvas."



**2.2.3. Tìm hiểu kiến thức 3: Ba component cơ bản + Prompt (12 phút)**

*Chiến lược dẫn dắt: So sánh đối chiếu*.

GV nối thử `Chat Input → Language Model → Chat Output` trên máy chiếu để HS thấy rõ port nào nối port nào.

Sau đó, GV tập trung vào **System Message** — phần quan trọng nhất của Language Model:

> "System message là 'nội quy' cho LLM. Cùng câu hỏi 'Hôm nay nên ăn gì?', nếu system message là 'Bạn là chuyên gia dinh dưỡng', output sẽ là salad. Nếu là 'Bạn là đầu bếp hài hước', output sẽ là bún bò Huế."

GV giới thiệu **4 thành phần của prompt**: **Role** (vai trò) + **Instruction** (nhiệm vụ) + **Constraint** (ràng buộc) + **Few-shot** (ví dụ mẫu). Chiếu bảng từ giáo trình (mục 2.6).

GV mời 1 HS thử viết 1 prompt nhanh trên bảng cho chủ đề "AI động viên khi buồn":

> "Em thử viết cho thầy/cô 1 system message có đủ role + instruction + constraint."

Cả lớp góp ý prompt đó — đây là bước chuẩn bị cho phần thực hành ngay sau.

**2.2.4. Tìm hiểu kiến thức 4: API key và Google AI Studio (5 phút)**

*Chiến lược dẫn dắt: Làm mẫu từng bước (Modeling)*.

GV nối kiến thức: "Các em đã hiểu flow 3 component và biết cách viết prompt. Còn một thứ cuối cùng cần trước khi thực hành — **API key** để Language Model gọi được đến Gemma 4."

GV demo nhanh trên máy chiếu:

1. Vào [aistudio.google.com](https://aistudio.google.com) → đăng nhập Google.
2. Click **Get API key** → **Create API key**.
3. Copy key (chuỗi `AIza...`).
4. Về Langflow → Settings → Model Providers → Google Generative AI → dán key → Save.
5. Enable model Gemma 4 (hoặc Gemini Flash).

GV nhấn mạnh:

> ⚠️ "API key là thứ **chỉ của riêng em**. Không đăng lên Zalo/Facebook/GitHub. Nếu lộ, người khác dùng ké quota của em."

### **3\. Đề xuất giải pháp và lên kế hoạch (5 phút)**

#### **3.1. Mục tiêu**

- Mỗi HS/nhóm chốt được chủ đề chatbot cá nhân sẽ xây trong buổi hôm nay.
- Phác thảo được flow 3 component và phiên bản prompt đầu tiên.

#### **3.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Checklist + Flowchart trước*.

GV phát cho mỗi HS/nhóm **phiếu kế hoạch** ngắn:

```
Nhóm/HS: __________________________

1. Chủ đề chatbot em chọn:
   ☐ Chuyên gia giải mã giấc mơ
   ☐ Đầu bếp AI
   ☐ Dịch ngôn ngữ Gen Z sang phiên bản phụ huynh
   ☐ Chủ đề khác: ________________

2. Sơ đồ flow (điền vào):
   [Chat Input] → [Language Model (Gemma 4)] → [Chat Output]
                         ↑
                   System Message v1 (role + instruction)

3. System message v1 em sẽ viết:
   Role: Bạn là _______________________
   Instruction: Khi user _______, em _______________
   Constraint: _______________________
```

HS làm 5 phút, đưa GV duyệt nhanh (1 phút/nhóm).

### **4\. Chế tạo mẫu, thử nghiệm và đánh giá (25 phút)**

#### **4.1. Mục tiêu**

- Xây được flow chatbot 3 component trên Langflow, chạy trên Playground.
- Test được ít nhất 3 phiên bản prompt (không system message → có role → few-shot) trên cùng một input.
- Ghi nhận sự khác biệt giữa 3 phiên bản.

#### **4.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Hướng dẫn từng bước (Guided Lab) + Scaffolding nâng dần độ khó*.

GV chia 25 phút thành 3 checkpoint rõ ràng:

**Checkpoint 1 — Xây flow cơ bản (8 phút)**

HS thực hiện:

1. Mở Langflow → **+ New Flow** → **Blank Flow**.
2. Kéo 3 component: Chat Input, Language Model, Chat Output.
3. Cấu hình Language Model: Model Provider = Google, Model Name = Gemma (hoặc Gemini Flash), dán API key.
4. Nối: Chat Input → Language Model → Chat Output.
5. Mở Playground, gõ câu hỏi theo chủ đề đã chọn (ví dụ "Em mơ thấy mình bay") → ghi lại output **V1 (không system message)** vào sổ.

GV kiểm tra: đi quanh lớp, hỏi "Có thấy output V1 chưa? Output nghe có cá tính không?" Nhóm nào xong → sang Checkpoint 2.

**Checkpoint 2 — Thêm system message (10 phút)**

HS thực hiện:

1. Click Language Model → **System Message** → dán prompt v1 đã viết ở phần 3 (role + instruction + constraint).
2. Vào Playground, gõ **cùng câu hỏi** ở Checkpoint 1 → ghi output **V2 (có role)** vào sổ.
3. Nâng cấp prompt thêm **1 ví dụ few-shot** (dạng `Ví dụ: User: "..." → Em trả lời: "..."`). Dán lại vào System Message.
4. Gõ lại cùng câu hỏi → ghi output **V3 (few-shot)** vào sổ.

GV kiểm tra: "Cả 3 version đã có chưa? Sự khác biệt rõ chưa?"

Gợi ý phân hoá:

- **HS nhanh:** thử thêm biến thứ 2 trong few-shot (nhiều ví dụ), hoặc dùng component Prompt Template với biến động.
- **HS chậm:** GV hoặc bạn đã xong hỗ trợ, copy prompt mẫu từ giáo trình.

**Checkpoint 3 — So sánh và viết ghi chú (7 phút)**

HS so sánh V1/V2/V3 và viết ghi chú 3-5 câu trả lời:

- Phiên bản nào có cá tính rõ nhất?
- Phiên bản nào bám sát format mong muốn nhất?
- Em thích phiên bản nào nhất?

### **5\. Chia sẻ, thảo luận và điều chỉnh (10 phút)**

#### **5.1. Mục tiêu**

- Mỗi HS/nhóm chia sẻ ngắn về chatbot mình làm và nhận xét ngắn.
- Brainstorm danh sách câu hỏi mở đầu cho Agent Chiến lược — chuẩn bị cho buổi 2.
- Mỗi HS/nhóm chốt được danh sách câu hỏi của mình.

#### **5.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Think-Pair-Share (thảo luận nhóm)*.

**5.2.1. Chia sẻ chatbot (3 phút)**

GV mời 2-3 nhóm lên chiếu nhanh chatbot của mình lên máy chiếu, gõ thử 1 câu, cả lớp xem output. GV nhận xét ngắn về cá tính của prompt.

**5.2.2. Brainstorm câu hỏi mở đầu (7 phút)**

GV chuyển sang phần quan trọng cho buổi 2:

> "Giờ các em nghĩ về Agent Chiến lược ở buổi sau. Agent sẽ hỏi HS vài câu mở đầu rồi sinh bảng hỏi riêng. Câu hỏi: **Nếu là em, em sẽ hỏi những câu gì để hiểu một HS khác trước khi tư vấn tuyển sinh?**"

Tổ chức Think-Pair-Share:

1. **Suy nghĩ cá nhân (2 phút):** Mỗi HS viết 5-10 câu trên sticky note.
2. **Thảo luận cặp (2 phút):** 2 HS chia sẻ, chốt 5-8 câu tâm đắc nhất.
3. **Chia sẻ toàn lớp (2 phút):** Mỗi nhóm đọc nhanh danh sách. GV ghi câu được lặp nhiều lên bảng.
4. **GV chia sẻ (1 phút):** GV giới thiệu **8 câu hỏi gợi ý**:
    1. Em đang học lớp mấy? Khối nào?
    2. Em muốn học ở khu vực nào? (Hà Nội / TP.HCM / không giới hạn)
    3. Môn nào em thích nhất?
    4. Môn nào em tự tin nhất?
    5. Em có sở thích hoặc hoạt động ngoại khoá gì?
    6. Em đã có dự tính học ngành/lĩnh vực gì?
    7. Điểm trung bình / điểm thi thử khoảng bao nhiêu?
    8. Có yếu tố nào quan trọng khác không? (học bổng, học phí, ký túc xá...)

GV nhấn mạnh: "Đây chỉ là gợi ý. **Em là người chốt danh sách cuối** — dùng nguyên của thầy/cô, câu tự nghĩ, hoặc trộn. Ghi vào sổ cho buổi 2."

### **6\. Tổng kết (5 phút)**

*Chiến lược dẫn dắt: Cliffhanger*.

GV chốt 3 điều cốt lõi của buổi 1:

> "Các em đã học 3 điều quan trọng:
>
> 1. **LLM → Chatbot → Agent** — khác nhau ở chỗ có tool hay không.
> 2. **Prompt có 4 chìa khoá:** role, instruction, constraint, few-shot.
> 3. **Brainstorm câu hỏi mở đầu** — cần gì để hiểu một HS?"

GV nhận xét ngắn tình hình lớp (nhóm làm tốt nhất, khó khăn chung nếu có).

**Cliffhanger cho buổi 2:**

> "Hôm nay chatbot của các em chỉ biết nói chuyện. Buổi sau, các em sẽ cho nó **một cánh tay** — cụ thể là tool Notion. Agent sẽ tự tạo page mới trên Notion và ghi bảng hỏi cá nhân hoá vào đó — không cần các em copy-paste.
>
> Trước buổi 2, các em hãy:
>
> 1. Đăng ký tài khoản Notion (free) nếu chưa có — [notion.so](https://notion.so).
> 2. Giữ API key Gemma 4 cho buổi sau.
> 3. Xem lại danh sách câu hỏi mở đầu đã brainstorm."

## HẾT BUỔI 1


## BUỔI 2

HS ổn định, chào hỏi GV.

### **1\. Ôn tập (10 phút)**

*Chiến lược dẫn dắt: Câu hỏi đóng kiểm tra nhanh + Kết nối với bài trước*.

GV mở đầu buổi 2 bằng 3 câu hỏi nhanh để kiểm tra HS còn nhớ kiến thức buổi 1 không. Mời HS giơ tay trả lời:

**Câu 1 (Nhớ):** Ba component cơ bản nhất để xây một chatbot trong Langflow là gì?
→ Đáp: Chat Input, Language Model, Chat Output.

**Câu 2 (Hiểu):** Chatbot và Agent khác nhau ở điểm gì?
→ Đáp: Agent có tool (tay chân), chatbot chỉ có LLM (bộ não), chatbot chỉ nói, agent biết làm việc ngoài.

**Câu 3 (Áp dụng):** Em muốn chatbot luôn trả lời ngắn dưới 3 câu, dùng tiếng Việt, có role là "trợ lý học tập". Viết nhanh 1 system message có đủ 3 thành phần.
→ Đáp mẫu: "Bạn là trợ lý học tập. Luôn trả lời bằng tiếng Việt, ngắn gọn dưới 3 câu."

GV chuyển tiếp đặt vấn đề buổi 2:

> "Buổi trước chatbot của các em chỉ biết nói chuyện. Hôm nay, các em nâng cấp nó thành **agent thật sự** — có tay chân là tool Notion. Agent sẽ hỏi các câu mở đầu các em đã brainstorm, rồi tự tạo page mới trên Notion và ghi bảng hỏi cá nhân hoá vào đó."

GV mời HS **mở sổ kiểm tra** danh sách câu hỏi mở đầu đã chọn ở cuối buổi 1. HS nào chưa có → 3 phút bổ sung nhanh (có thể dùng nguyên 8 câu của GV).

HS ổn định nhóm, xem lại danh sách câu hỏi, chuẩn bị vào phần chế tạo chính.

### **2\. Chế tạo mẫu, thử nghiệm và đánh giá (55 phút)**

#### **2.1. Mục tiêu**

- Tạo được Notion Internal Integration, parent page "Bảng hỏi học sinh", chia sẻ page với integration.
- Xây được flow Agent Chiến lược: Chat Input → Agent (gắn Create Page + Add Content to Page ở Tool Mode) → Chat Output.
- Viết được Agent Instructions rõ ràng để agent hỏi lần lượt câu mở đầu → sinh bảng hỏi cá nhân hoá → gọi tool Notion.
- Test thành công với 1 hồ sơ HS trước, rồi mở rộng sang 3 hồ sơ khác nhau để kiểm chứng cá nhân hoá.

#### **2.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Hướng dẫn từng bước (Guided Lab) + 4 checkpoint rõ ràng + Scaffolding nâng dần*.

GV chia 55 phút thành **4 checkpoint** — tương ứng 4 giai đoạn của quy trình kỹ thuật. Sau mỗi checkpoint GV đi quanh kiểm tra trước khi cho sang checkpoint tiếp theo.

**Checkpoint 1 — Chuẩn bị Notion (12 phút)**

*Chiến lược: Làm mẫu từng bước*.

> "Nếu em chưa có tài khoản Notion, làm theo 3 bước:
>
> 1. Vào [notion.so](https://www.notion.so) → click **Get Notion free** (hoặc **Sign up**).
> 2. Chọn **Continue with Google** → đăng nhập bằng tài khoản Gmail của em.
> 3. Chọn **For personal use** → bỏ qua các bước mời thành viên → vào thẳng workspace.
>
> Vậy là xong — không cần xác nhận email, không cần tạo mật khẩu riêng."


GV demo nhanh 4 bước trên máy chiếu (3 phút), sau đó HS tự làm (9 phút):

1. Tạo Notion Integration tại [notion.so/my-integrations](https://www.notion.so/my-integrations), tên tuỳ chọn. Copy Integration Token (`secret_XXX`).
2. Tạo page mới trong Notion với tiêu đề "Bảng hỏi học sinh" — đây là **parent page**.
3. **⚠️ Bước dễ quên:** Mở page "Bảng hỏi học sinh" → click "..." → **Add connections** → chọn integration vừa tạo → Confirm.
4. Copy **Parent Page ID** từ URL (chuỗi 32 ký tự hex cuối URL).

GV đi vòng kiểm tra: hỏi "Đã có Integration Token + Parent Page ID chưa? Đã Share parent page với integration chưa?" Nhóm nào xong thì sang Checkpoint 2.

Gợi ý phân hoá:

- **HS nhanh:** hướng dẫn tạo thêm page con thủ công để hiểu cấu trúc parent-child.
- **HS chậm:** GV / bạn đã xong hỗ trợ, hoặc chia sẻ link parent page mẫu để HS Duplicate vào workspace của mình.

**Checkpoint 2 — Xây flow Agent (18 phút)**

*Chiến lược: Hướng dẫn từng bước + Checklist*.

HS thực hiện:

1. Mở Langflow → **+ New Flow** → **Blank Flow** (flow mới, không dùng chung buổi 1).
2. Kéo 5 component vào canvas: Chat Input, Agent, Chat Output, **Create Page** (Notion bundle), **Add Content to Page** (Notion bundle).
3. Cấu hình **Agent**:
    - Model Provider: Google
    - Model Name: Gemma (hoặc Gemini Flash)
    - API Key: dán từ buổi 1
    - **Agent Instructions:** dán template GV chuẩn bị sẵn (phát qua link hoặc in ra), thay phần `[Dán danh sách câu hỏi]` bằng danh sách em đã chọn

Template Agent Instructions GV cung cấp:

```
Bạn là "Agent Chiến lược" — trợ lý tư vấn tuyển sinh ĐH cho HS THPT Việt Nam.

Nhiệm vụ:
1. Hỏi HS lần lượt các câu mở đầu sau (mỗi lần 1 câu, chờ trả lời rồi hỏi tiếp):
   [Dán danh sách câu hỏi đã chọn]

2. Sau khi user trả lời đủ, phân tích và sinh BẢNG HỎI CÁ NHÂN HOÁ 6-8 câu chuyên sâu RIÊNG cho HS này. Bảng hỏi phải:
   - Dựa trên sở thích, điểm mạnh, ràng buộc cụ thể của HS (không chung chung)
   - Mỗi câu có mục đích rõ (xác định chuyên ngành / ràng buộc / mục tiêu)
   - Tránh câu yes/no
   - Tiếng Việt, xưng "em" với HS

3. GỌI TOOL create_student_page để tạo page mới với tiêu đề là tên HS.
4. GỌI TOOL add_questionnaire_to_page để đổ bảng hỏi vào thân page (markdown numbered list).
5. Thông báo với user bảng hỏi đã lưu trên Notion.

Nói tự nhiên, mỗi lượt 1 câu — không hỏi dồn.
```

4. Cấu hình **Create Page** ở Tool Mode:
    - Bật Tool Mode
    - Notion Secret: dán Integration Token
    - Parent Page ID: dán Parent Page ID
    - Tool Name: `create_student_page`
    - Tool Description: dán mẫu GV chuẩn bị

5. Cấu hình **Add Content to Page** ở Tool Mode:
    - Bật Tool Mode
    - Notion Secret: dán Integration Token
    - Tool Name: `add_questionnaire_to_page`
    - Tool Description: dán mẫu

6. Nối: `Chat Input → Agent`, `Create Page (Tool)` → Agent (Tools port), `Add Content to Page (Tool)` → Agent (Tools port), `Agent → Chat Output`.

GV đi vòng kiểm tra: "Đã nối đủ 2 tool vào Agent chưa? Agent Instructions có danh sách câu hỏi của em chưa?"

Gợi ý phân hoá:

- **HS nhanh:** khuyến khích tự viết Agent Instructions thay vì dùng mẫu; thử thêm 1 ví dụ few-shot vào instructions.
- **HS chậm:** dùng nguyên mẫu, chỉ thay danh sách câu hỏi.

**Checkpoint 3 — Test lần 1 với HS1 và debug (15 phút)**

*Chiến lược: Sai lầm có chủ đích (Productive Failure) + Checklist debug*.

HS mở Playground, đóng vai **HS1 (hồ sơ mẫu 1)** — GV phát hồ sơ in sẵn hoặc chiếu:

> **HS1:** Lớp 11, khối B. Không giới hạn khu vực. Thích Sinh học nhất. Tự tin Hoá. Tham gia CLB môi trường. Chưa biết ngành. TB 8.5. Muốn có học bổng.

Chạy flow đến khi:

- Agent hỏi đủ các câu mở đầu.
- Agent sinh bảng hỏi cá nhân hoá (trả lời ngay trong Playground).
- Agent gọi được Create Page và Add Content to Page.
- HS mở Notion → parent page "Bảng hỏi học sinh" → thấy page mới có tiêu đề và nội dung bảng hỏi.

GV phát **checklist debug** (in sẵn hoặc dán lên bảng) khi HS gặp lỗi:

| Triệu chứng                                | Nguyên nhân có thể                                     | Giải pháp                                                                                     |
| :------------------------------------------- | :---------------------------------------------------------- | :---------------------------------------------------------------------------------------------- |
| Agent không hỏi câu nào, trả lời ngay     | Instructions chưa yêu cầu hỏi lần lượt                  | Thêm vào Instructions: "Hỏi từng câu một, chờ user trả lời rồi mới hỏi câu tiếp"           |
| "page not found" / 404                      | Quên Share parent page với integration                     | Quay lại Checkpoint 1 bước 3                                                                   |
| "Invalid token" / 401                       | Dán nhầm token                                              | Copy lại token từ tab Secrets của integration                                                 |
| Agent không gọi tool                       | Tool Description mơ hồ                                     | Viết rõ "Dùng tool này khi ..." trong description                                             |
| Page tạo được nhưng thân trống          | Agent quên gọi Add Content to Page                         | Thêm instruction: "Sau khi Create Page trả về page_id, gọi NGAY add_questionnaire_to_page" |

GV đi vòng hỗ trợ nhóm gặp lỗi. Nhóm nào xong → sang Checkpoint 4.

**Checkpoint 4 — Test với HS2 và HS3 để kiểm chứng cá nhân hoá (10 phút)**

*Chiến lược: So sánh giải pháp (Analysis & Evaluation)*.

GV phát tiếp 2 hồ sơ nữa:

> **HS2:** Lớp 12, khối A, TP.HCM. Thích Toán nhất, tự tin Toán. Code game Unity. Dự tính CNTT. Điểm thi thử 25. Lo học phí cao.
>
> **HS3:** Lớp 12, khối D, Hà Nội. Thích Văn nhất, tự tin Anh. CLB văn học, viết blog. Dự tính Báo chí. TB 8.2. Muốn du học sau ĐH.

HS chạy flow 2 lần nữa với HS2 và HS3. Trên Notion, parent page "Bảng hỏi học sinh" sẽ có **3 page con** với 3 nội dung khác nhau.

GV đặt **câu hỏi phản biện** cho HS tự kiểm tra:

> "Các em so sánh 3 bảng hỏi xem có khác nhau thực sự không:
>
> - HS1 có được hỏi câu về di truyền/sinh thái/ngành môi trường không?
> - HS2 có được hỏi câu về chuyên ngành CNTT/trường top TP.HCM/học bổng tư thục không?
> - HS3 có được hỏi câu về Báo chí/du học/khối D không?
>
> Nếu 3 bảng hỏi giống nhau → agent chưa cá nhân hoá. Sửa ở đâu trong Agent Instructions?"

Nhóm nào bảng hỏi giống nhau → quay lại Agent Instructions, nhấn mạnh yêu cầu "dựa RIÊNG vào câu trả lời của HS" + thêm 1 ví dụ few-shot.

### **3\. Chia sẻ, thảo luận và điều chỉnh (20 phút)**

#### **3.1. Mục tiêu**

- Mỗi nhóm trình bày được 1 bảng hỏi cá nhân hoá trước lớp.
- Nhận xét và góp ý được cho bảng hỏi của nhóm khác (mức độ cá nhân hoá, chất lượng câu hỏi).
- Đề xuất được các định hướng cải tiến Agent Instructions.

#### **3.2. Tổ chức thực hiện**

*Chiến lược dẫn dắt: Gallery Walk + Two Stars and a Wish*.

**3.2.1. Gallery Walk (12 phút)**

Mỗi nhóm mở **1 trong 3 page bảng hỏi** của mình lên Notion và chiếu lên màn hình nhóm / để ở chế độ public-view. GV in hoặc phát phiếu Gallery Walk cho mỗi HS với cột:

| Nhóm | Hồ sơ HS mẫu | ⭐ 2 điểm tốt | 💡 1 gợi ý cải tiến |
| :--: | :------------: | :--------------: | :--------------------: |
|  ...  |      ...      |       ...       |          ...          |

HS đi vòng quanh lớp 10 phút, xem 3-4 bảng hỏi của các nhóm khác, điền vào phiếu theo mẫu **Two Stars and a Wish** (2 điểm tốt + 1 gợi ý cải thiện).

GV hướng dẫn nhìn vào 3 tiêu chí khi đánh giá:

1. **Cá nhân hoá thực sự:** Bảng hỏi có phản ánh sở thích/ràng buộc cụ thể của HS không?
2. **Chất lượng câu hỏi:** Có câu yes/no không? Mỗi câu có mục đích rõ?
3. **Hành động đúng:** Agent có lưu đúng lên Notion kèm tên HS không?

**3.2.2. Thảo luận lớp (8 phút)**

GV tổng kết các nhận xét trên phiếu Gallery Walk. Đặt 3 câu hỏi cho cả lớp:

> **Câu hỏi mở:** "Nhóm nào có bảng hỏi cá nhân hoá sâu nhất? Tại sao em thấy vậy?"
>
> **Câu hỏi mở:** "Có nhóm nào bảng hỏi bị chung chung không? Theo em, nguyên nhân là gì — do prompt chưa đủ rõ, do model, hay do câu hỏi mở đầu chưa đủ thông tin?"
>
> **Câu hỏi phản biện:** "Nếu em là HS thật sự, em có muốn trả lời 6-8 câu này không? Có câu nào em thấy quá riêng tư?"

GV chốt: "Cá nhân hoá thực sự không đến từ model — đến từ **chất lượng prompt + dữ liệu mở đầu**. Nếu em chỉ hỏi 2 câu, agent không đủ dữ liệu để cá nhân hoá. Nếu em hỏi 8 câu chất lượng, bảng hỏi đầu ra sẽ khác hẳn."

### **4\. Tổng kết (5 phút)**

*Chiến lược dẫn dắt: 3-2-1 Reflection + Cliffhanger cho Dự án 2*.

**4.1. 3-2-1 Reflection (3 phút)**

GV yêu cầu mỗi HS viết vào sổ (hoặc phiếu exit ticket):

> - **3** điều em học được hôm nay.
> - **2** điều em thấy thú vị hoặc bất ngờ.
> - **1** câu hỏi em còn thắc mắc.

GV thu lại (hoặc chụp ảnh) — làm tư liệu để đầu Dự án 2 giải đáp thắc mắc.

**4.2. GV đúc kết và dặn dò (2 phút)**

GV chốt 3 kiến thức cốt lõi xuyên suốt dự án 1:

> "Các em đã hoàn thành một **agent thật sự** đầu tiên. Ôn lại 3 điều cốt lõi:
>
> 1. **Agent = LLM + Tool** — không chỉ nói, mà còn làm việc ngoài Langflow.
> 2. **Notion Integration + Share** — 2 bước bắt buộc để Langflow ghi được Notion.
> 3. **Tool Description** — viết rõ ràng → agent gọi đúng lúc; viết mơ hồ → agent gọi sai."

GV nhận xét tình hình lớp, khen các nhóm làm tốt, chỉ ra khó khăn chung.

**Cliffhanger cho Dự án 2:**

> "Dự án 1 kết thúc, nhưng hệ thống AI Agent tư vấn tuyển sinh mới chỉ có **1/3**. Hôm nay Agent Chiến lược đã biết thu thập thông tin HS. Nhưng nó **chưa biết về tuyển sinh** — điểm chuẩn, ngành, trường ra sao?
>
> Dự án 2 (3 buổi) sẽ xây **Agent Thông tin tuyển sinh** — biết search web tìm tin mới, biết đọc file PDF tuyển sinh, biết lưu dữ liệu vào một kho khổng lồ để truy vấn theo nghĩa. Trước buổi 3, các em hãy nghĩ trước: **mùa tuyển sinh thông tin thay đổi hàng ngày — làm sao để AI luôn trả lời bằng tin mới nhất?**"

## Thông tin tài liệu

| Được soạn ngày:        |  | Bởi: |  |
| :-------------------------- | :- | :---- | :- |
| Được review ngày:       |  | Bởi: |  |
| Được hiệu chỉnh ngày: |  | Bởi: |  |

## Phân bổ các hoạt động

| Nội dung                                                                       | Chương trình 2T |                                | Chương trình 1T |                                |
| :-------------------------------------------------------------------------------- | :----------------: | :-----------------------------: | :----------------: | :-----------------------------: |
|                                                                                    |  **Buổi**  | **Thời lượng (phút)** |  **Buổi**  | **Thời lượng (phút)** |
| Buổi 1 — 1. Xác định vấn đề (Case study + Driving Question)                |         1         |               15               |         1         |               15               |
| Buổi 1 — 2. Nghiên cứu kiến thức nền (Agent vs Chatbot, Langflow, Prompt) |         1         |               30               |         1         |               30               |
| Buổi 1 — 3. Đề xuất giải pháp + lên kế hoạch                              |         1         |               5                |         1         |               5                |
| Buổi 1 — 4. Chế tạo mẫu (3 checkpoint: flow → prompt v2 → prompt v3)       |         1         |               25               |         1         |               25               |
| Buổi 1 — 5. Chia sẻ + brainstorm câu hỏi mở đầu                            |         1         |               10               |         1         |               10               |
| Buổi 1 — 6. Tổng kết (Cliffhanger cho buổi 2)                                  |         1         |               5                |         1         |               5                |
| Buổi 2 — 1. Ôn tập + đặt vấn đề buổi 2                                       |         2         |               10               |         2         |               10               |
| Buổi 2 — 2. Chế tạo mẫu Agent Chiến lược (4 checkpoint)                 |         2         |               55               |         2         |               55               |
| Buổi 2 — 3. Chia sẻ, thảo luận (Gallery Walk + Two Stars a Wish)          |         2         |               20               |         2         |               20               |
| Buổi 2 — 4. Tổng kết (3-2-1 Reflection + Cliffhanger)                          |         2         |               5                |         2         |               5                |
| **Tổng**                                                                    |                    |             **180**             |                    |             **180**             |
