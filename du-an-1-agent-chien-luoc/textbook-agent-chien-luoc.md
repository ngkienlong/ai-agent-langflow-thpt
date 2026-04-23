# Dự án 1: Agent Chiến lược định hướng chọn trường và ngành

**Số buổi:** 2 × 90 phút

---

## 1. Giới thiệu dự án

### Bối cảnh thực tế

Mỗi năm có hơn **một triệu học sinh THPT Việt Nam** đứng trước câu hỏi quan trọng nhất của tuổi 18: *"Mình sẽ học trường nào, ngành nào?"*. Ban tư vấn tuyển sinh của trường thường chỉ có 2-3 thầy cô, không thể gặp riêng từng em. Cùng lúc đó, các chatbot tư vấn trên mạng thì hỏi ai cũng giống ai — 10 câu cứng nhắc, xong đưa ra gợi ý chung chung "em hợp ngành Kinh tế" cho cả người mê Toán lẫn người mê Văn.

Bạn có nhận ra vấn đề không? Người tư vấn giỏi **không hỏi ai cũng giống ai**. Họ hỏi vài câu mở đầu, *lắng nghe*, rồi dựa vào câu trả lời mà **đặt câu hỏi chuyên sâu khác nhau** cho từng người. Bạn thích Sinh học và đi CLB môi trường? Câu hỏi tiếp theo sẽ về "di truyền học hay sinh thái hơn?". Bạn thích Toán và code game Unity? Câu hỏi sẽ là "AI/ML hay game development hơn?". Mỗi học sinh xứng đáng có một bộ câu hỏi dành riêng cho mình.

Trong dự án này, bạn sẽ xây một **Agent Chiến lược** — một AI biết hỏi 8 câu mở đầu, rồi **tự sinh ra một bảng hỏi cá nhân hoá** gồm 6-8 câu chuyên sâu khác nhau cho từng học sinh. Bảng hỏi được đẩy thẳng lên Notion để học sinh trả lời và lưu lại cho Agent Tư vấn dùng ở các buổi sau.

Đây không phải là một chatbot cho vui. Đây là bước đầu tiên trong việc xây **một hệ thống 3 agent thật sự** có thể thay thế phần nào ban tư vấn tuyển sinh.

### Câu hỏi dẫn dắt

> "Làm sao để AI biết hỏi một vài câu, rồi tự sinh ra một bảng hỏi dành riêng cho mỗi học sinh?"

### Mục tiêu học tập

**Buổi 1 — Đặt vấn đề + Làm quen Langflow & Prompt Engineering:**

- [ ] Giải thích được **AI Agent** là gì, khác chatbot thông thường ở điểm nào.
- [ ] Nhận biết và gọi tên các thành phần chính trên giao diện Langflow Desktop (canvas, components, flow, Playground, component inspection panel).
- [ ] Tạo API key trên Google AI Studio và cấu hình Model Provider trong Langflow Settings.
- [ ] Xây dựng flow `Chat Input → Language Model (Gemma 4) → Chat Output` và chạy thử trên Playground — bài tập nhỏ làm quen.
- [ ] Viết **system message** với role + instruction + constraint để định hướng hành vi LLM.
- [ ] Sử dụng component **Prompt Template** với biến động.
- [ ] So sánh kết quả khi thay đổi prompt (không system message vs. có system message vs. few-shot).
- [ ] Brainstorm danh sách câu hỏi mở đầu cho Agent Chiến lược.

**Buổi 2 — Notion bundle & Agent Chiến lược hoàn chỉnh:**

- [ ] Giải thích được tại sao agent cần **tool** để tương tác với công cụ bên ngoài (Notion).
- [ ] Tạo **Notion Internal Integration**, lấy integration token, chia sẻ page với integration.
- [ ] Sử dụng các component trong **Notion bundle** (Create Page, Add Content to Page) làm tool cho Agent.
- [ ] Xây dựng flow Agent Chiến lược hoàn chỉnh: `Chat Input → Agent (Language Model + Notion bundle tools) → Chat Output`.
- [ ] Agent hỏi câu mở đầu → sinh bảng hỏi cá nhân hoá → tạo page/entry trên Notion qua Notion bundle.
- [ ] Kiểm tra bảng hỏi cá nhân hoá thực sự khác nhau giữa các học sinh.

### Sản phẩm đầu ra

**Cuối buổi 1:**

- Một flow chatbot cá nhân trên Langflow (chủ đề do bạn tự chọn: chuyên gia giải mã giấc mơ, đầu bếp AI, trợ lý học tập...), chạy được trên Playground.
- 3 phiên bản prompt khác nhau đã được test trên cùng một input và ghi lại sự khác biệt.
- Một danh sách câu hỏi mở đầu (5-10 câu) cho Agent Chiến lược, do bạn hoặc nhóm tự chọn.

**Cuối buổi 2:**

- Flow **Agent Chiến lược** chạy được trên Playground: agent hỏi các câu mở đầu đã chọn → dựa vào câu trả lời sinh ra bảng hỏi cá nhân hoá 6-8 câu chuyên sâu → tạo page mới trên Notion kèm toàn bộ bảng hỏi.
- Test với **ít nhất 3 hồ sơ học sinh khác nhau** → 3 bảng hỏi xuất hiện trên Notion với nội dung **khác nhau rõ rệt**.

---

## 2. Kiến thức nền tảng

### 2.1 LLM, Chatbot, AI Agent — 3 khái niệm dễ nhầm lẫn

Trước khi chạm tay vào Langflow, cần phân biệt 3 khái niệm hay bị trộn lẫn:

**LLM (Large Language Model — mô hình ngôn ngữ lớn)** là "bộ não" đọc hiểu và sinh văn bản. Ví dụ: **Gemma 4, Gemini, GPT, Claude, Llama**. LLM tự nó không nhớ cuộc hội thoại trước, không biết thông tin mới sau ngày huấn luyện, và không "làm" được gì ngoài sinh chữ.

**Chatbot** = LLM + giao diện chat. Người dùng gõ → LLM sinh câu trả lời → hiển thị. Chatbot thông thường chỉ *nói chuyện*, không thao tác ra thế giới ngoài.

**AI Agent** = LLM + **tool** (công cụ) + khả năng **tự quyết định** khi nào dùng tool. Một agent có thể:

- Gọi Web Search để lấy tin mới (bạn sẽ thấy ở buổi 3).
- Đọc/ghi file.
- Tạo page trên Notion (bạn làm ngay buổi 2).
- Truy vấn vector database (buổi 4, 5).

> 💡 **Cách nhớ:** LLM là bộ não. Chatbot là bộ não biết nói. Agent là bộ não biết nói **và có tay chân**.

Trong dự án này:

- **Buổi 1:** bạn xây một **chatbot** đầu tay để làm quen Langflow. Chưa có tool.
- **Buổi 2:** bạn nâng cấp thành một **agent thật sự** — gắn tool Notion, cho nó "tay" để ghi ra ngoài.

### 2.2 Langflow — công cụ kéo thả để xây AI flow

**Langflow** là phần mềm cho phép bạn xây các "flow" AI bằng cách **kéo thả khối (component)** và nối chúng bằng dây — giống xếp Lego. Bạn không cần viết code để dựng flow cơ bản.

Langflow Desktop đã được cài sẵn trên máy trường. Mở lên, bạn sẽ thấy các vùng chính:

| Vùng | Tên | Vai trò |
|---|---|---|
| Giữa màn hình | **Canvas** (bảng vẽ) | Nơi bạn thả component và nối dây |
| Cột trái | **Sidebar** (thanh công cụ) | Danh sách tất cả component có sẵn |
| Phải màn hình (khi click 1 component) | **Component inspection panel** | Xem và chỉnh thông số component |
| Nút góc phải trên | **Playground** | Nơi chat thử với flow vừa xây |
| Icon avatar góc phải trên | **Settings** | Cấu hình API key, Model Providers... |

Một **flow** là tập hợp các component nối với nhau thành một "đường đi" cho dữ liệu: từ input → xử lý → output.

### 2.3 Ba component cơ bản nhất — khung sườn mọi chatbot

Để xây một chatbot đơn giản nhất, chỉ cần 3 component:

**Chat Input** — nhận tin nhắn từ người dùng. Có ô text để người dùng gõ câu hỏi.

**Language Model** — "gọi" LLM (ví dụ Gemma 4) để sinh câu trả lời. Đây là component cốt lõi. Cần cấu hình:

- **Model Provider:** chọn Google (vì bạn dùng Gemma qua Gemini API).
- **Model Name:** chọn model cụ thể (ví dụ `gemma-3-27b-it` hoặc Gemini Flash nếu Gemma chưa sẵn).
- **System Message:** câu hướng dẫn định hướng vai trò cho LLM.

**Chat Output** — hiển thị câu trả lời của LLM trên Playground.

Ba component này nối nhau theo thứ tự: `Chat Input → Language Model → Chat Output`. Đây là khung sườn của **mọi chatbot** trong Langflow.

### 2.4 Google AI Studio và Gemma 4 — "bộ não" miễn phí

Để dùng được LLM trong Langflow, bạn cần một **API key** — giống chiếc chìa khoá cho phép phần mềm (Langflow) gọi tới server AI của Google. Key này lấy từ [Google AI Studio](https://aistudio.google.com). Bản **free tier** đủ dùng cho cả khoá học.

**Gemma 4** là model mã nguồn mở của Google, truy cập qua **Gemini API**. Nó nhẹ, đủ mạnh cho chat và làm agent.

> ⚠️ **Bảo mật API key:** API key là thứ **chỉ của riêng bạn**. Đừng đưa cho ai, đừng đăng lên Zalo/Facebook/GitHub. Nếu lộ, người khác có thể dùng ké quota của bạn.

### 2.5 Prompt — "thần chú" điều khiển LLM

**Prompt** là văn bản bạn gửi cho LLM. Có 2 loại:

- **User prompt** — câu hỏi/yêu cầu cụ thể của người dùng. Ví dụ: "Hôm nay nên ăn gì?".
- **System message** (system prompt) — câu hướng dẫn ở **tầng hệ thống**, LLM sẽ xem nó như "nội quy" cho mọi câu trả lời. Người dùng không thấy system message.

Ví dụ dễ hiểu:

- **System message:** "Bạn là chuyên gia dinh dưỡng. Luôn trả lời ngắn gọn, dưới 3 câu, bằng tiếng Việt. Gợi ý món tốt cho sức khoẻ."
- **User prompt:** "Hôm nay nên ăn gì?"
- **Output:** "Bạn nên ăn salad rau củ với ức gà áp chảo. Bổ sung trái cây tươi. Uống đủ 2 lít nước."

Cùng câu hỏi đó, nếu đổi system message thành "Bạn là đầu bếp hài hước, luôn gợi ý món ngon khó cưỡng", output sẽ hoàn toàn khác: "Bún bò Huế cay xè một tô, hoặc bánh xèo giòn rụm kẹp rau sống thôi!".

### 2.6 Kỹ thuật viết prompt — 4 "chìa khoá"

Một system message hiệu quả thường có 4 thành phần:

| Thành phần | Mô tả | Ví dụ |
|---|---|---|
| **Role** (vai trò) | Nói cho LLM biết nó là ai | "Bạn là chuyên gia tư vấn tuyển sinh ĐH Việt Nam." |
| **Instruction** (nhiệm vụ) | Nói cho LLM biết cần làm gì | "Hỏi lần lượt 8 câu về học sinh, chờ câu trả lời rồi hỏi tiếp." |
| **Constraint** (ràng buộc) | Giới hạn cách trả lời | "Trả lời bằng tiếng Việt. Xưng 'em' với HS. Không hỏi 2 câu cùng lúc." |
| **Few-shot example** | 1-3 ví dụ mẫu để LLM "bắt chước" | "Ví dụ: User: 'Em thích Sinh học'. Agent: 'Trong Sinh học, em thích phần nào: di truyền, sinh thái, hay vi sinh?'" |

Role + Instruction là tối thiểu. Constraint và Few-shot dùng khi bạn cần output **có cá tính riêng** hoặc **format cố định**.

### 2.7 Prompt Template — prompt có biến số

Đôi khi bạn muốn prompt có **biến số** — chỗ trống để điền input từ user. Langflow có component **Prompt Template** để làm việc này.

Ví dụ template:

```
Bạn là đầu bếp AI. Hãy gợi ý 2 món nấu được từ nguyên liệu: {ingredients}.
Mỗi món gồm: tên món, thời gian nấu, 3 bước chính.
```

`{ingredients}` là **biến**. Khi user nhập "trứng, cà chua, hành tây", Langflow tự thay `{ingredients}` bằng chuỗi đó rồi gửi cho LLM. Template cho phép viết prompt một lần, dùng với nhiều input khác nhau.

### 2.8 Từ chatbot sang Agent — "bộ não" mọc "tay chân"

Đến đây kết thúc phần buổi 1. Từ 2.8 trở đi là kiến thức cho **buổi 2**.

Ở 2.1 bạn đã biết: **Agent = LLM + tool**. Chatbot chỉ nói, agent làm được việc. Cụ thể với Agent Chiến lược trong dự án này:

- **Bộ não** (Language Model / Gemma 4): sinh câu hỏi mở đầu, đọc câu trả lời, sinh bảng hỏi cá nhân hoá.
- **Tay chân** (Notion bundle tools): **Create Page** để tạo page mới trên Notion, **Add Content to Page** để đổ bảng hỏi dài vào page đó.

Trong Langflow, component **Agent** là "bộ não có ổ cắm cho tay chân" — bạn gắn bao nhiêu tool vào cũng được, miễn mỗi tool có mô tả rõ ràng để agent biết khi nào dùng.

### 2.9 Notion — nơi lưu bảng hỏi

**Notion** là app ghi chú + quản lý thông tin rất phổ biến. Trong Notion có 2 đối tượng chính:

- **Page** (trang) — "trang giấy" chứa text, hình, bảng... Đây là nơi bạn sẽ lưu bảng hỏi cá nhân hoá cho từng HS. Một HS = một page riêng.
- **Parent page** — page "cha" chứa các page con. Agent sẽ tạo các page bảng hỏi bên trong một parent page "Bảng hỏi học sinh" do bạn chuẩn bị trước.

### 2.10 Notion Internal Integration — "chìa khoá" cho agent

Notion không cho phép bất kỳ app ngoài nào tự ý truy cập workspace. Để Langflow ghi được lên Notion, bạn phải làm 3 bước:

1. **Tạo Internal Integration** trên trang [notion.so/my-integrations](https://www.notion.so/my-integrations). Đây là "danh tính" của app bạn cấp cho Langflow dùng.
2. **Copy Integration Token** (chuỗi `secret_XXXXXX`) — tương đương API key, xác thực khi Langflow gọi Notion.
3. **Share parent page với integration** — vào page "Bảng hỏi học sinh" → click "..." → Connections → chọn integration → Confirm. **Bước này rất dễ quên.** Nếu quên, agent sẽ báo "không tìm thấy page" dù token đúng.

> ⚠️ **Integration Token cũng là dữ liệu nhạy cảm** — giữ bí mật, không đăng lên mạng, không chia sẻ trong Zalo/GitHub.

### 2.11 Notion bundle trong Langflow — 2 component quan trọng

Langflow có sẵn **Notion bundle** với nhiều component. Trong dự án này bạn chỉ cần 2:

| Component | Chức năng | Dùng khi nào |
|---|---|---|
| **Create Page** | Tạo page mới bên trong một parent page | Khi agent cần tạo page bảng hỏi mới cho 1 HS |
| **Add Content to Page** | Thêm nội dung markdown vào thân một page đã tạo | Sau khi Create Page, dùng để đổ bảng hỏi 6-8 câu dài vào thân page |

Luồng chuẩn của agent: **Create Page** (tạo page rỗng với tiêu đề là tên HS) → **Add Content to Page** (đổ bảng hỏi dài vào thân page vừa tạo).

### 2.12 Tool Mode — biến component thành tool của Agent

Mỗi component trong Langflow có thể chạy ở 2 chế độ:

- **Chạy trực tiếp (Standalone)** — bạn cấu hình cố định mọi input, component chạy đúng như vậy.
- **Tool Mode** — component được "đóng gói" thành tool để **agent tự gọi**. Agent tự quyết định input (ví dụ nội dung page, tiêu đề) dựa trên ngữ cảnh cuộc chat.

Trong dự án này bạn dùng **Tool Mode** — để agent chủ động. Bật Tool Mode bằng cách click vào component → toggle "Tool Mode" trong inspection panel.

Khi Tool Mode bật, cần viết **Tool Name** và **Tool Description** rõ ràng để agent biết khi nào gọi tool:

- **Tool Name:** `create_student_page` — tên ngắn, không dấu, dễ đọc.
- **Tool Description:** `Tạo một page mới trên Notion cho một học sinh. Input: tên học sinh (làm tiêu đề page). Dùng tool này SAU khi đã thu thập đủ câu trả lời của học sinh và trước khi ghi bảng hỏi.`

Description viết mơ hồ → agent gọi sai tool hoặc không gọi. Description chi tiết → agent gọi đúng lúc.

### 2.13 Component Agent — "điều phối viên" trong Langflow

Component **Agent** (có sẵn trong Langflow core) là một "wrapper" bao gồm:

- Một **Language Model** bên trong (vẫn cấu hình Model Provider + Model Name + API key).
- Một danh sách **Tools** (bạn gắn các component Notion ở Tool Mode vào đây).
- **Agent Instructions** (giống system message — hướng dẫn agent nhiệm vụ và cách dùng tool).
- **Chat Memory** tích hợp sẵn — agent nhớ các lượt hội thoại trước đó, không cần gắn Memory riêng.

Khi chạy, agent sẽ **lặp** quy trình: đọc input user → suy nghĩ → quyết định **trả lời trực tiếp** hay **gọi tool** → nếu gọi tool thì nhận kết quả và suy nghĩ tiếp → cuối cùng trả lời. Bạn không cần code vòng lặp — Agent tự làm.

### 2.14 Agent Chiến lược hoạt động thế nào

Đây là kịch bản của Agent Chiến lược trong buổi 2:

1. User mở Playground, bắt đầu chat.
2. Agent chào và đặt **câu hỏi mở đầu số 1** (ví dụ: "Em đang học lớp mấy? Khối nào?").
3. User trả lời.
4. Agent đặt câu hỏi 2, 3, ... cho đến hết danh sách câu mở đầu (đã brainstorm ở buổi 1).
5. **Bước quan trọng nhất:** Agent đọc toàn bộ câu trả lời → dựa vào đặc điểm của HS (sở thích, điểm mạnh, ràng buộc) → **sinh ra bảng hỏi chuyên sâu 6-8 câu** phù hợp riêng với HS này.
6. Agent gọi tool **Create Page**: tạo page mới trong parent page "Bảng hỏi học sinh" với tiêu đề là tên HS.
7. Agent gọi tool **Add Content to Page**: đổ bảng hỏi cá nhân hoá vào thân page vừa tạo.
8. Agent thông báo cho user: "Em đã lưu bảng hỏi trên Notion. Em hãy mở Notion, trả lời các câu đó, và lưu lại cho Agent Tư vấn dùng ở các buổi sau."

> 💡 **Bí mật của "cá nhân hoá":** điểm mấu chốt là bước 5 — agent phải **đọc kỹ câu trả lời** rồi sinh câu hỏi sâu **dựa trên** đó. Nếu prompt viết cẩu thả, agent sẽ sinh bảng hỏi giống nhau cho mọi HS → không cá nhân hoá. Nếu prompt tốt, 3 HS khác nhau → 3 bảng hỏi khác nhau rõ rệt.

---

## 3. Hướng dẫn thực hành

### 3.1 Vật tư và công cụ cần chuẩn bị

| STT | Vật tư / Công cụ | Số lượng | Ghi chú |
|-----|-------------------|----------|---------|
| 1 | Máy tính có internet | 1/HS hoặc 1/nhóm 2 | Langflow Desktop đã cài sẵn |
| 2 | Tài khoản Google | 1/HS | Đăng nhập Google AI Studio |
| 3 | API key Gemma 4 (Gemini API) | 1/HS | Lấy ở buổi 1 từ Google AI Studio |
| 4 | Tài khoản Notion (free tier) | 1/HS | Buổi 2 — đăng ký sẵn tại [notion.so](https://notion.so) |
| 5 | Notion Internal Integration + token | 1/HS | Buổi 2 — tạo tại [notion.so/my-integrations](https://www.notion.so/my-integrations) |
| 6 | Sổ/note để ghi so sánh prompt và câu hỏi brainstorm | 1/HS | Giấy hoặc Notion |

### 3.2 Sơ đồ các flow sẽ xây

**Flow buổi 1 (chatbot đầu tay — chưa có tool):**

```
[Chat Input] → [Language Model] → [Chat Output]
                    ▲
              System Message
              (role + instruction + constraint)
```

**Flow buổi 2 (Agent Chiến lược hoàn chỉnh):**

```
                  [Create Page] ──┐
                                  ▼ (tool)
[Chat Input] → [Agent] → [Chat Output]
                  ▲
                  │ (tool)
              [Add Content to Page] ──┘
```

### 3.3 Hướng dẫn từng bước

#### ━━━ BUỔI 1 ━━━

**Bước 1: Lấy API key Gemma 4 từ Google AI Studio**

- Truy cập [aistudio.google.com](https://aistudio.google.com), đăng nhập bằng tài khoản Google.
- Click **Get API key** (hoặc **Lấy khoá API**) ở góc trên.
- Click **Create API key** → chọn tạo trong project mặc định.
- Copy key (chuỗi dài bắt đầu bằng `AIza...`) và lưu vào nơi an toàn.

Kết quả mong đợi: bạn có một chuỗi API key sẵn sàng để dán vào Langflow.

**Bước 2: Cấu hình Model Provider trong Langflow**

- Mở Langflow Desktop (đã cài sẵn trên máy).
- Click avatar góc phải trên → **Settings** → **Model Providers**.
- Chọn **Google Generative AI** (hoặc **Google** / **Gemini**).
- Dán API key → **Save**.
- Enable model muốn dùng (chọn Gemma nếu có, ví dụ `gemma-3-27b-it`; hoặc Gemini Flash nếu Gemma chưa xuất hiện).

Kết quả mong đợi: provider Google hiện "Connected" hoặc có icon check xanh.

**Bước 3: Tạo flow chatbot đầu tay**

Đây là **bài tập nhỏ làm quen** — chưa phải Agent Chiến lược. Chủ đề tuỳ bạn chọn. Gợi ý:

- **Chuyên gia giải mã giấc mơ** — kể giấc mơ → AI phân tích theo phong cách hài hước.
- **Đầu bếp AI** — liệt kê 3 nguyên liệu → AI gợi ý món nấu được.
- **Dịch ngôn ngữ Gen Z** — nhập câu teen → AI dịch sang phiên bản bố mẹ / thầy cô / ông bà.

Thao tác:

- Click **+ New Flow** → **Blank Flow**.
- Từ Sidebar kéo vào canvas: **Chat Input** (mục Inputs), **Language Model** (mục Models hoặc Core), **Chat Output** (mục Outputs).
- Nối: click chấm output của Chat Input → kéo dây tới chấm input của Language Model. Tương tự nối Language Model → Chat Output.

**Bước 4: Cấu hình Language Model**

- Click component Language Model → mở Component inspection panel bên phải.
- **Model Provider:** Google.
- **Model Name:** Gemma hoặc Gemini Flash.
- **Bật** tuỳ chọn **System Message**.
- Tạm để System Message trống — test phiên bản "không prompt" trước.

**Bước 5: Test phiên bản 1 — KHÔNG có system message**

- Click **Playground** góc phải trên.
- Gõ câu hỏi liên quan chủ đề. Ví dụ "Hôm qua em mơ thấy mình bay lên trời".
- Ghi lại nguyên văn câu trả lời vào sổ, đánh nhãn **V1 — không prompt**.

Kết quả mong đợi: AI trả lời một câu trung tính, không có "cá tính" rõ.

**Bước 6: Thêm system message — phiên bản 2 có ROLE**

- Quay lại canvas, click Language Model.
- Vào ô **System Message**, gõ prompt có role + instruction. Ví dụ cho chủ đề giải mã giấc mơ:

```
Bạn là chuyên gia giải mã giấc mơ hài hước.
Khi người dùng kể giấc mơ, em đưa ra 3 cách hiểu khác nhau: khoa học, tâm linh, và hài hước.
```

- Vào Playground, gõ **cùng câu hỏi** ở Bước 5.
- Ghi lại câu trả lời, đánh nhãn **V2 — có role + instruction**.

**Bước 7: Thêm few-shot — phiên bản 3**

- Vẫn click Language Model, chỉnh System Message thành:

```
Bạn là chuyên gia giải mã giấc mơ hài hước.
Khi người dùng kể giấc mơ, đưa ra 3 cách hiểu: khoa học, tâm linh, và hài hước.

Ví dụ mẫu:
User: "Em mơ thấy mình bị một con gà khổng lồ đuổi."
Em trả lời:
1. Khoa học: Não em đang xử lý căng thẳng về một việc em đang "trốn tránh".
2. Tâm linh: Con gà tượng trưng cho cơ hội em đang bỏ lỡ.
3. Hài hước: Chắc tối qua em ăn cánh gà nhiều quá!

Luôn trả lời bằng tiếng Việt, giọng hài hước, không quá 200 từ.
```

- Vào Playground, gõ lại **cùng câu hỏi** ở Bước 5.
- Ghi lại câu trả lời, đánh nhãn **V3 — few-shot + constraint**.

**Bước 8: So sánh 3 phiên bản và viết ghi chú**

Đặt V1 / V2 / V3 cạnh nhau. Viết ghi chú 3-5 câu trả lời:

- Phiên bản nào có "cá tính" rõ nhất? Vì sao?
- Phiên bản nào bám sát format mong muốn nhất?
- Nếu là người dùng cuối, bạn thích phiên bản nào nhất?

**Bước 9: Brainstorm câu hỏi mở đầu cho Agent Chiến lược**

Đây là bước quan trọng để chuẩn bị cho buổi 2. Làm việc cá nhân 2 phút → thảo luận nhóm 4 phút → chia sẻ lớp 3 phút.

Câu hỏi brainstorm:

> 🤔 Nếu là bạn, bạn sẽ hỏi những câu gì để hiểu một học sinh khác trước khi tư vấn tuyển sinh? Liệt kê 5-10 câu mà bạn nghĩ là quan trọng nhất.

Sau khi các nhóm chia sẻ, giáo viên sẽ giới thiệu **8 câu hỏi gợi ý**:

1. Em đang học lớp mấy? Khối nào?
2. Em muốn học ở khu vực nào? (Hà Nội / TP.HCM / không giới hạn)
3. Môn nào em thích nhất?
4. Môn nào em tự tin nhất?
5. Em có sở thích hoặc hoạt động ngoại khoá gì?
6. Em đã có dự tính học ngành/lĩnh vực gì? (hoặc chưa biết)
7. Điểm trung bình / điểm thi thử khoảng bao nhiêu?
8. Có yếu tố nào quan trọng khác không? (học bổng, học phí, ký túc xá...)

**Em là người chốt danh sách cuối cùng** — dùng nguyên 8 câu GV, dùng câu tự nghĩ, hoặc trộn cả hai. Ghi danh sách cuối vào sổ để dùng ở buổi 2.

#### ━━━ BUỔI 2 ━━━

**Bước 10: Tạo Notion Internal Integration**

- Mở trình duyệt → [notion.so/my-integrations](https://www.notion.so/my-integrations) (đăng nhập Notion).
- Click **+ New integration**.
- Đặt tên: `Langflow Agent Chien Luoc` (hoặc tuỳ em).
- Chọn workspace muốn kết nối → **Submit**.
- Vào tab **Secrets** → copy **Internal Integration Token** (chuỗi `secret_XXX`). Lưu vào nơi an toàn.

Kết quả mong đợi: có 1 integration và 1 token đã lưu.

**Bước 11: Tạo parent page "Bảng hỏi học sinh" trên Notion**

- Mở Notion, tạo một page mới trong workspace với tiêu đề: `Bảng hỏi học sinh`.
- Đây sẽ là "page cha" — agent sẽ tạo các page con bên trong page này, mỗi HS = 1 page con.

**Bước 12: Share parent page với integration** ⚠️ *(Bước rất dễ quên)*

- Mở page "Bảng hỏi học sinh" → click "..." (menu ba chấm) ở góc phải trên → **Add connections** (hoặc **Connections**).
- Gõ tên integration vừa tạo (`Langflow Agent Chien Luoc`) → chọn → **Confirm**.
- Integration giờ đã có quyền đọc/ghi page này.

Nếu lúc chạy agent báo lỗi "page not found" dù token đúng, 90% là do bước này chưa làm.

**Bước 13: Lấy Parent Page ID**

- Mở page "Bảng hỏi học sinh" trên trình duyệt.
- URL trông như: `https://www.notion.so/workspace/Bang-hoi-hoc-sinh-<PAGE_ID>`.
- `<PAGE_ID>` là chuỗi 32 ký tự hex (chữ + số). Copy và lưu lại — lát dán vào Langflow.

**Bước 14: Tạo flow mới trong Langflow cho Agent Chiến lược**

- Click **+ New Flow** → **Blank Flow** (flow mới, không dùng chung với flow buổi 1).
- Kéo từ Sidebar vào canvas:
    - **Chat Input** (Inputs).
    - **Agent** (Agents).
    - **Chat Output** (Outputs).
    - **Create Page** (tìm Notion bundle trong Bundles → Notion).
    - **Add Content to Page** (cũng trong Notion bundle).

**Bước 15: Cấu hình Agent**

- Click **Agent** → mở Component inspection panel.
- **Model Provider:** Google.
- **Model Name:** Gemma hoặc Gemini Flash.
- **API Key:** dán API key Gemma 4.
- **Agent Instructions:** dán system prompt (thay `[Dán danh sách câu hỏi đã chọn ở Bước 9]` bằng danh sách thực của em):

```
Bạn là "Agent Chiến lược" — trợ lý tư vấn tuyển sinh đại học cho học sinh THPT Việt Nam.

Nhiệm vụ:
1. Hỏi học sinh lần lượt các câu mở đầu sau (mỗi lần hỏi 1 câu, chờ user trả lời rồi mới hỏi câu tiếp):
   [Dán danh sách câu hỏi đã chọn ở Bước 9 vào đây]

2. Sau khi user trả lời đủ, phân tích câu trả lời và sinh ra một BẢNG HỎI CÁ NHÂN HOÁ gồm 6-8 câu chuyên sâu phù hợp RIÊNG với học sinh này. Bảng hỏi phải:
   - Dựa trên sở thích, điểm mạnh, ràng buộc cụ thể của HS (không chung chung)
   - Mỗi câu có mục đích rõ (xác định chuyên ngành / ràng buộc / mục tiêu)
   - Tránh câu yes/no
   - Viết bằng tiếng Việt, xưng "em" với học sinh

3. Sau khi có bảng hỏi, GỌI TOOL create_student_page để tạo page mới trên Notion với tiêu đề là tên học sinh (hỏi thêm nếu chưa biết tên).

4. Sau khi Create Page thành công (tool trả về page_id), GỌI TOOL add_questionnaire_to_page để đổ bảng hỏi 6-8 câu vào thân page đó (format markdown numbered list).

5. Thông báo với user rằng bảng hỏi đã được lưu trên Notion.

Khi hỏi học sinh, nói tự nhiên, thân thiện, mỗi lượt 1 câu — không hỏi dồn.
```

**Bước 16: Cấu hình Create Page ở Tool Mode**

- Click **Create Page** → bật **Tool Mode** (toggle trong inspection panel).
- Điền:
    - **Notion Secret:** dán Integration Token từ Bước 10.
    - **Parent Page ID:** dán Parent Page ID từ Bước 13.
    - **Tool Name:** `create_student_page`.
    - **Tool Description:**

```
Tạo một page mới trên Notion bên trong parent page "Bảng hỏi học sinh". Input: title (string, tên học sinh). Trả về page_id của page vừa tạo. Dùng tool này sau khi đã thu thập đủ thông tin của học sinh và chuẩn bị ghi bảng hỏi.
```

**Bước 17: Cấu hình Add Content to Page ở Tool Mode**

- Click **Add Content to Page** → bật **Tool Mode**.
- Điền:
    - **Notion Secret:** dán Integration Token.
    - **Tool Name:** `add_questionnaire_to_page`.
    - **Tool Description:**

```
Thêm nội dung markdown vào thân một page Notion đã có. Input: page_id (string) và markdown_content (string, bảng hỏi 6-8 câu dạng numbered list). Dùng tool này SAU KHI đã gọi create_student_page và có page_id.
```

**Bước 18: Nối các component**

- `Chat Input` → `Agent` (input port của Agent).
- `Create Page` (output Tool) → `Agent` (input Tools port).
- `Add Content to Page` (output Tool) → `Agent` (input Tools port).
- `Agent` (output Message) → `Chat Output`.

Kết quả mong đợi: canvas có Agent ở giữa, 2 Notion tool nối vào Tools port, Chat Input và Chat Output ở 2 đầu.

**Bước 19: Test lần 1 — Hồ sơ HS1**

- Click **Playground**.
- Em đóng vai "học sinh HS1" — chat với agent. Gợi ý hồ sơ:
    - Lớp 11, khối B, không giới hạn khu vực.
    - Thích Sinh học nhất, tự tin Hoá.
    - CLB môi trường, chưa biết ngành, TB 8.5, muốn có học bổng.
- Agent nên hỏi lần lượt các câu mở đầu. Em trả lời theo hồ sơ trên.
- Sau khi hết câu mở đầu, agent sinh bảng hỏi cá nhân hoá rồi gọi 2 tool Notion.
- Mở Notion → page "Bảng hỏi học sinh" → thấy một page con mới với tiêu đề là tên HS1, thân page là bảng hỏi 6-8 câu.

**Bước 20: Test với HS2 và HS3 để kiểm tra cá nhân hoá**

Tạo 2 hồ sơ khác biệt để test:

- **HS2:** Lớp 12, khối A, TP.HCM. Thích Toán nhất, tự tin Toán, code game Unity. Dự tính học CNTT. Điểm thi thử 25. Lo học phí cao.
- **HS3:** Lớp 12, khối D, Hà Nội. Thích Văn nhất, tự tin Anh. CLB văn học, viết blog. Dự tính học Báo chí. TB 8.2. Muốn du học sau ĐH.

Chạy flow 2 lần nữa, mỗi lần 1 hồ sơ. Mở Notion → kiểm tra:

- Phải có **3 page con khác nhau**.
- Bảng hỏi của 3 page **khác nhau rõ rệt**:
    - HS1 nhận câu về di truyền/sinh thái, ngành Công nghệ sinh học / Khoa học môi trường, học bổng khối B.
    - HS2 nhận câu về chuyên ngành CNTT, so sánh trường top TP.HCM, học bổng tư thục.
    - HS3 nhận câu về Báo chí/Truyền thông, du học, các trường khối D.

Nếu 3 bảng hỏi giống nhau → agent chưa cá nhân hoá. Quay lại Bước 15 sửa Agent Instructions, nhấn mạnh yêu cầu "dựa RIÊNG vào câu trả lời của HS".

**Bước 21: Ghi nhận kết quả**

Viết ghi chú 5-7 câu trả lời:

- Bảng hỏi của 3 HS khác nhau ở điểm nào cụ thể?
- Có câu nào bảng hỏi bị lặp giữa 2 HS?
- Agent có chỗ nào "cứng", trả lời không tự nhiên?
- Nếu làm lại, bạn sửa phần nào trong Agent Instructions?

**Checklist debug khi gặp lỗi:**

| Triệu chứng | Nguyên nhân | Giải pháp |
|---|---|---|
| Agent không hỏi câu nào, trả lời ngay | Instructions chưa yêu cầu hỏi lần lượt | Thêm "Hỏi từng câu một, chờ user trả lời rồi mới hỏi câu tiếp" |
| "page not found" | Quên Share parent page với integration | Quay lại Bước 12 |
| "Invalid token" / 401 | Dán nhầm token | Copy lại từ tab Secrets của integration |
| Agent không gọi tool | Tool Description mơ hồ | Viết rõ "Dùng tool này khi..." trong description |
| Page tạo được nhưng thân trống | Agent quên gọi Add Content | Thêm instruction: "Sau khi create_student_page trả về page_id, gọi NGAY add_questionnaire_to_page" |

---

## 4. Nâng cao

**Mở rộng A — Thêm few-shot cho agent**

Thêm vào Agent Instructions **1 ví dụ mẫu** về bảng hỏi cá nhân hoá tốt (1 HS mẫu + bảng hỏi mẫu). Agent sẽ bắt chước format → chất lượng ổn định hơn. Đây là kỹ thuật few-shot đã học ở buổi 1.

**Mở rộng B — Xác nhận trước khi ghi Notion**

Sửa instruction: "Sau khi sinh bảng hỏi, **trình bày cho user xem trước**. Hỏi 'Em có đồng ý với bảng hỏi này không? Nếu đồng ý, trả lời YES để tôi lưu lên Notion.' Chỉ khi user xác nhận mới gọi create_student_page." Đây là kỹ thuật **human-in-the-loop** — rất hay dùng khi agent làm việc hệ trọng.

**Mở rộng C — Bảng hỏi song ngữ**

Yêu cầu agent sinh bảng hỏi bằng **cả tiếng Việt và tiếng Anh**. Hữu ích cho HS có kế hoạch du học, cần làm quen terminology tiếng Anh của ngành.

**Mở rộng D — Emoji cho mỗi câu**

Yêu cầu agent gắn emoji chủ đề cho mỗi câu hỏi (ví dụ: 🧬 cho câu về sinh học, 💻 cho câu về CNTT). Giúp bảng hỏi trực quan hơn, HS đọc đỡ ngại.

---

## 5. Câu hỏi ôn tập

1. **(Nhớ)** Ba component cơ bản nhất để xây một chatbot trong Langflow là gì?
2. **(Hiểu)** Phân biệt **chatbot** và **AI agent**. Ở buổi 1 bạn làm ra một cái nào, ở buổi 2 là cái nào?
3. **(Hiểu)** **User prompt** và **system message** khác nhau ở điểm nào? Người dùng có thấy system message không?
4. **(Hiểu)** Liệt kê 3 bước bắt buộc để Langflow có thể ghi được lên một Notion page.
5. **(Áp dụng)** Bạn muốn xây một chatbot đóng vai "người bạn luôn động viên khi buồn". Hãy viết 1 system message có đủ 3 thành phần: role, instruction, constraint.
6. **(Áp dụng)** Bạn muốn Agent Chiến lược đánh dấu ngày HS trả lời bảng hỏi. Bạn sẽ cần dùng tool nào trong Notion bundle? (Gợi ý: đây là component chưa nói trong giáo trình, bạn tự tra trên Langflow docs)
7. **(Phân tích)** Cùng câu hỏi "Gợi ý cho mình một quán cà phê để đi hôm nay", vì sao cùng 1 model (Gemma 4) lại có thể cho ra câu trả lời hoàn toàn khác nhau? Yếu tố nào quyết định sự khác biệt?
8. **(Phân tích)** Tại sao **Tool Description** lại quan trọng? Nếu bạn viết mơ hồ như "Tạo entry trên Notion", agent có thể xử lý sai ở đâu?
9. **(Đánh giá)** Bạn A và bạn B cùng xây Agent Chiến lược với cùng model Gemma 4. Bạn A cho agent chỉ có 1 tool Create Page. Bạn B cho agent cả Create Page + Add Content to Page. Bảng hỏi của ai sẽ đầy đủ hơn? Vì sao?
10. **(Sáng tạo)** Ngoài tư vấn tuyển sinh, bạn có thể dùng pattern "Agent hỏi → sinh nội dung cá nhân hoá → ghi Notion" cho chủ đề nào khác trong đời sống? Nêu 1 ý tưởng và phác thảo các câu hỏi mở đầu.

---

## 6. Thuật ngữ

| Thuật ngữ | Giải thích |
|---|---|
| **LLM** (Large Language Model) | Mô hình ngôn ngữ lớn, "bộ não" sinh văn bản. Ví dụ: Gemma, Gemini, GPT. |
| **Chatbot** | Giao diện chat kết nối người dùng với LLM. Chỉ "nói chuyện". |
| **AI Agent** | LLM có thêm tool (công cụ) và khả năng tự quyết định khi nào dùng tool. Có thể "làm việc" ngoài phần mềm, không chỉ nói. |
| **Langflow** | Công cụ kéo-thả để xây các flow AI mà không cần (hoặc ít cần) viết code. |
| **Component** | Một khối chức năng trong Langflow (ví dụ Chat Input, Language Model). Kéo từ Sidebar vào Canvas. |
| **Flow** | Tập hợp các component nối với nhau thành "đường đi" cho dữ liệu. |
| **Canvas** | Vùng giữa màn hình Langflow, nơi bạn thả component và nối dây. |
| **Playground** | Khu vực chat thử với flow đã xây, có sẵn trong Langflow. |
| **Component inspection panel** | Panel bên phải hiện khi click vào component, cho phép xem và chỉnh thông số. |
| **API key** | "Chìa khoá" cho phép một phần mềm gọi tới server AI. Phải giữ bí mật. |
| **Google AI Studio** | Công cụ của Google để lấy API key và test các model Gemini/Gemma. |
| **Gemma 4** | Model mã nguồn mở của Google, truy cập qua Gemini API, có bản free tier. |
| **Prompt** | Văn bản gửi cho LLM để điều khiển output. |
| **User prompt** | Phần prompt do người dùng cuối nhập vào. |
| **System message** (system prompt) | Phần prompt đặt ở tầng hệ thống, định hướng hành vi LLM xuyên suốt cuộc hội thoại. Người dùng không thấy. |
| **Role** | Thành phần của prompt — nói cho LLM "nó là ai". |
| **Instruction** | Thành phần của prompt — nói cho LLM "cần làm gì". |
| **Constraint** | Thành phần của prompt — giới hạn độ dài, ngôn ngữ, phong cách. |
| **Few-shot** | Kỹ thuật đưa 1-3 ví dụ mẫu trong prompt để LLM bắt chước phong cách. |
| **Prompt Template** | Component Langflow chứa prompt có biến `{tên_biến}`, biến được thay bằng input lúc chạy. |
| **Tool** | "Cánh tay nối dài" cho agent: component/hàm thực hiện hành động (ghi DB, gọi API, search web). |
| **Tool Mode** | Chế độ biến một component Langflow thành tool để agent gọi. |
| **Tool Name** | Tên ngắn của tool, agent dùng để gọi (ví dụ `create_student_page`). |
| **Tool Description** | Mô tả cho agent biết tool làm gì và khi nào nên dùng. |
| **Notion** | App ghi chú + quản lý thông tin. Có khái niệm Page (trang) và Parent page (trang cha chứa các page con). |
| **Notion Page** | "Trang giấy" chứa nội dung text, hình, bảng. |
| **Notion Integration** | "Danh tính" của một app mà bạn cấp quyền truy cập workspace Notion. |
| **Integration Token** | Chuỗi bí mật xác thực integration khi gọi Notion API. Bắt đầu bằng `secret_`. |
| **Share with integration** | Thao tác cho phép integration đọc/ghi một page cụ thể. Bắt buộc làm. |
| **Notion bundle** | Nhóm component Langflow có sẵn để thao tác với Notion. Dự án này dùng Create Page và Add Content to Page. |
| **Create Page** | Notion bundle component — tạo page mới bên trong parent page. |
| **Add Content to Page** | Notion bundle component — thêm nội dung markdown vào thân page đã có. |
| **Human-in-the-loop** | Pattern thiết kế agent có xác nhận của user trước khi thực hiện hành động quan trọng. |
| **Cá nhân hoá (personalization)** | Output khác nhau cho mỗi người dùng, dựa trên dữ liệu riêng của họ. |
