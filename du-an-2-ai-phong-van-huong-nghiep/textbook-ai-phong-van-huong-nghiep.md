# Dự án 2: AI phỏng vấn hướng nghiệp

**Số buổi:** 1 × 90 phút

---

## 1. Giới thiệu dự án

### Bối cảnh thực tế

Ở Dự án 1, em đã xây dựng một chatbot AI có thể gợi ý nghề nghiệp. Nhưng em có nhận ra một vấn đề không? Chatbot đó chỉ **trả lời** — nó không biết **hỏi ngược lại**. Nếu em nói "Mình thích công nghệ", AI sẽ gợi ý ngay mà không hỏi thêm: "Bạn thích lập trình hay thiết kế? Bạn muốn làm việc một mình hay theo nhóm?"

Một chuyên gia tư vấn hướng nghiệp giỏi sẽ không vội đưa ra lời khuyên. Họ sẽ **phỏng vấn** em trước — hỏi về sở thích, điểm mạnh, mục tiêu — rồi mới đưa ra gợi ý phù hợp. Trong dự án này, em sẽ học cách "dạy" AI làm điều tương tự bằng **Prompt Engineering** — nghệ thuật viết chỉ dẫn cho AI.

### Câu hỏi dẫn dắt

"Làm sao để AI biết hỏi đúng câu hỏi giúp em chọn ngành phù hợp?"

### Mục tiêu học tập

- [ ] Viết system prompt để định hướng hành vi LLM (ví dụ: "Bạn là chuyên gia tư vấn tuyển sinh ĐH Việt Nam")
- [ ] So sánh kết quả trả lời khi thay đổi prompt (không system prompt vs. có system prompt vs. few-shot)
- [ ] Thiết kế prompt có constraint (trả lời bằng tiếng Việt, giới hạn độ dài) và đánh giá hiệu quả
- [ ] Sử dụng component Prompt trên Langflow để quản lý system prompt

### Sản phẩm đầu ra

Flow chatbot "phỏng vấn hướng nghiệp" trên Langflow — AI chủ động hỏi học sinh về sở thích, điểm mạnh, mục tiêu, sau đó gợi ý 3 ngành phù hợp kèm lý do.

---

## 2. Kiến thức nền tảng

### 2.1 Prompt Engineering là gì?

**Prompt Engineering** là kỹ thuật viết chỉ dẫn (prompt) cho AI để AI trả lời đúng ý mình muốn. Giống như khi em giao việc cho một người — nếu em nói rõ ràng, cụ thể, người đó sẽ làm đúng hơn.

**Ví dụ so sánh:**

| Prompt mơ hồ | Prompt rõ ràng |
|--------------|----------------|
| "Tư vấn nghề cho mình" | "Bạn là chuyên gia tư vấn hướng nghiệp. Hãy hỏi mình 3 câu về sở thích, điểm mạnh và mục tiêu, sau đó gợi ý 3 ngành phù hợp kèm lý do." |
| → AI trả lời chung chung | → AI hỏi ngược, gợi ý cụ thể |

Prompt Engineering không phải viết code — nó là **viết tiếng Việt** (hoặc tiếng Anh) một cách có chiến lược để AI hiểu và làm đúng.

### 2.2 System Prompt vs. User Prompt

Trong một cuộc hội thoại với AI, có 2 loại prompt:

**System Prompt (Prompt hệ thống):**
- Là chỉ dẫn "ngầm" mà AI luôn tuân theo trong suốt cuộc hội thoại
- Người dùng không nhìn thấy system prompt
- Định nghĩa vai trò, phong cách, và quy tắc cho AI
- Được thiết lập một lần, áp dụng cho mọi câu hỏi

**User Prompt (Prompt người dùng):**
- Là câu hỏi hoặc yêu cầu mà người dùng gõ vào
- Thay đổi mỗi lần người dùng gửi tin nhắn

**Ví dụ trực quan:**

```
┌─────────────────────────────────────────────────┐
│ SYSTEM PROMPT (ẩn, AI luôn tuân theo):          │
│ "Bạn là chuyên gia tư vấn hướng nghiệp.        │
│  Hãy hỏi học sinh 3 câu trước khi gợi ý."      │
├─────────────────────────────────────────────────┤
│ USER PROMPT (học sinh gõ):                      │
│ "Mình muốn tìm ngành phù hợp"                  │
├─────────────────────────────────────────────────┤
│ AI TRẢ LỜI:                                    │
│ "Chào bạn! Mình sẽ giúp bạn tìm ngành phù hợp.│
│  Trước tiên, bạn thích môn học nào nhất ở       │
│  trường?"                                       │
└─────────────────────────────────────────────────┘
```

Nếu không có system prompt, AI sẽ trả lời theo cách mặc định — thường là liệt kê ngay một danh sách ngành mà không hỏi thêm.

### 2.3 Kỹ thuật viết prompt

Có 4 kỹ thuật chính để viết prompt hiệu quả:

**1. Role (Vai trò):** Gán cho AI một vai trò cụ thể.

```
"Bạn là chuyên gia tư vấn tuyển sinh đại học Việt Nam với 10 năm kinh nghiệm."
```

Tại sao hiệu quả? Khi AI "nhập vai", nó sẽ trả lời theo phong cách và kiến thức của vai trò đó.

**2. Instruction (Chỉ dẫn):** Nói rõ AI cần làm gì, theo trình tự nào.

```
"Khi học sinh bắt đầu hội thoại, hãy:
1. Chào hỏi thân thiện
2. Hỏi về sở thích và môn học yêu thích
3. Hỏi về điểm mạnh và kỹ năng
4. Hỏi về mục tiêu nghề nghiệp
5. Dựa trên câu trả lời, gợi ý 3 ngành phù hợp kèm lý do"
```

**3. Constraint (Ràng buộc):** Đặt giới hạn cho AI.

```
"Luôn trả lời bằng tiếng Việt.
Mỗi câu hỏi chỉ hỏi 1 vấn đề, không hỏi nhiều cùng lúc.
Không gợi ý quá 3 ngành.
Không đưa ra thông tin điểm chuẩn cụ thể (vì có thể không chính xác)."
```

**4. Few-shot (Ví dụ mẫu):** Cho AI xem ví dụ về cách trả lời mong muốn.

```
"Ví dụ cuộc hội thoại mẫu:
Học sinh: Mình muốn tìm ngành phù hợp.
AI: Chào bạn! Mình rất vui được giúp. Trước tiên, bạn thích môn học nào nhất ở trường?
Học sinh: Mình thích Sinh học và Hoá học.
AI: Hay quá! Bạn thích phần nào của Sinh học nhất — thí nghiệm, nghiên cứu, hay chăm sóc sức khoẻ?
..."
```

> 💡 **Mẹo:** Kết hợp cả 4 kỹ thuật sẽ cho kết quả tốt nhất. Bắt đầu với Role + Instruction, sau đó thêm Constraint và Few-shot để tinh chỉnh.

### 2.4 Component Prompt trên Langflow

Ở Dự án 1, em kết nối trực tiếp Chat Input → LLM. Nhưng để thêm system prompt, em cần component **Prompt** — đóng vai trò "bộ lọc" giữa Input và LLM.

Component Prompt cho phép em:
- Viết system prompt trong một ô riêng
- Chèn biến (variable) vào prompt — ví dụ: `{user_message}` sẽ được thay bằng tin nhắn của người dùng
- Dễ dàng chỉnh sửa prompt mà không cần thay đổi flow

**Flow mới sẽ trông như thế này:**

```
[Chat Input] ──→ [Prompt] ──→ [Google Generative AI] ──→ [Chat Output]
   (tai)        (bộ lọc)         (bộ não)                   (loa)
```

### 2.5 Persona trong AI Agent

Trong kiến thức về AI Agent, **Persona** là một trong 4 thành phần chính (Persona, Memory, Tools, Model). Persona định nghĩa vai trò, tính cách và phong cách giao tiếp của agent.

Ở dự án này, khi em viết system prompt với role + instruction + constraint, em đang **tạo Persona** cho AI Agent. Đây là bước quan trọng thứ hai (sau khi đã có Model ở Dự án 1) trên hành trình xây dựng AI Agent hoàn chỉnh.

**Ánh xạ với 4 thành phần AI Agent:**

| Thành phần | Dự án 1 | Dự án 2 |
|-----------|---------|---------|
| Model | ✅ Gemma 4 | ✅ Gemma 4 |
| Persona | ❌ Chưa có | ✅ System prompt |
| Memory | ❌ (nâng cao) | Sẽ thêm ở phần nâng cao |
| Tools | ❌ Chưa có | ❌ Chưa có (Dự án 3) |

---

## 3. Hướng dẫn thực hành

### 3.1 Vật tư và công cụ cần chuẩn bị

| STT | Vật tư / Công cụ | Số lượng | Ghi chú |
|-----|-------------------|----------|---------|
| 1 | Máy tính (laptop/PC) có kết nối internet | 1/học sinh hoặc 1/nhóm | Đã cài Langflow Desktop |
| 2 | Langflow Desktop | Đã cài từ Dự án 1 | Có thể mở flow cũ hoặc tạo mới |
| 3 | API key Google AI Studio | Đã tạo từ Dự án 1 | Dùng lại key cũ |
| 4 | Giấy ghi chú | 1 tờ/nhóm | Để ghi nhận so sánh các phiên bản prompt |

### 3.2 Hướng dẫn từng bước

**Bước 1: Mở flow từ Dự án 1 (hoặc tạo mới)**

1. Mở Langflow Desktop
2. Mở flow "AI đoán nghề nghiệp" từ Dự án 1
3. Hoặc tạo flow mới với 3 component cơ bản: Chat Input → Google Generative AI → Chat Output (như Dự án 1)
4. Đặt tên flow: "AI phỏng vấn hướng nghiệp"

**Bước 2: Thêm component Prompt — viết System Prompt v1 (đơn giản)**

1. Ở thanh bên trái, tìm mục **Prompts**
2. Kéo component **Prompt** vào canvas, đặt giữa Chat Input và Google Generative AI
3. Xoá kết nối cũ giữa Chat Input và Google Generative AI
4. Kết nối lại: Chat Input → Prompt → Google Generative AI → Chat Output
5. Trong component Prompt, viết system prompt v1:

```
Bạn là chuyên gia tư vấn hướng nghiệp. Hãy giúp học sinh tìm ngành học phù hợp.
```

6. Thêm biến `{user_message}` vào prompt để nhận tin nhắn từ Chat Input:

```
Bạn là chuyên gia tư vấn hướng nghiệp. Hãy giúp học sinh tìm ngành học phù hợp.

Tin nhắn của học sinh: {user_message}
```

7. Kết nối cổng **Message** của Chat Input vào biến `{user_message}` của Prompt

**Bước 3: Test v1 — AI có hỏi ngược không?**

1. Mở Playground
2. Gõ: "Mình muốn tìm ngành phù hợp"
3. Quan sát: AI có hỏi ngược lại không? Hay AI liệt kê ngay một danh sách ngành?
4. Ghi nhận kết quả vào giấy:

```
Prompt v1: "Bạn là chuyên gia tư vấn hướng nghiệp..."
Kết quả: AI [có/không] hỏi ngược. AI trả lời: [tóm tắt]
Đánh giá: [tốt/chưa tốt] vì [lý do]
```

> 💡 **Mẹo:** Rất có thể AI sẽ KHÔNG hỏi ngược ở v1 — vì prompt quá đơn giản, không yêu cầu AI phải hỏi. Đây là điều bình thường! Chúng ta sẽ cải tiến ở bước tiếp theo.

**Bước 4: Cải tiến Prompt v2 — thêm Role + Instruction rõ ràng**

Quay lại component Prompt, thay system prompt bằng v2:

```
## Vai trò
Bạn là chuyên gia tư vấn hướng nghiệp cho học sinh THPT Việt Nam, 
với 10 năm kinh nghiệm tư vấn tuyển sinh đại học.

## Nhiệm vụ
Khi học sinh bắt đầu hội thoại, hãy thực hiện theo trình tự:
1. Chào hỏi thân thiện, giới thiệu bản thân
2. Hỏi về sở thích và môn học yêu thích (1 câu hỏi)
3. Hỏi về điểm mạnh và kỹ năng nổi bật (1 câu hỏi)
4. Hỏi về mục tiêu nghề nghiệp hoặc ước mơ (1 câu hỏi)
5. Sau khi có đủ thông tin, gợi ý 3 ngành học phù hợp kèm lý do

QUAN TRỌNG: Hỏi từng câu một, KHÔNG hỏi nhiều câu cùng lúc. 
Chờ học sinh trả lời xong mới hỏi câu tiếp theo.

Tin nhắn của học sinh: {user_message}
```

**Bước 5: Test v2 — so sánh với v1**

1. Mở Playground (nhấn **New Chat** để bắt đầu hội thoại mới)
2. Gõ: "Mình muốn tìm ngành phù hợp"
3. Quan sát: AI có hỏi ngược không? Hỏi có đúng trình tự không?
4. Trả lời các câu hỏi của AI và xem AI gợi ý ngành có phù hợp không
5. Ghi nhận và so sánh với v1:

```
Prompt v2: Role + Instruction
Kết quả: AI [có/không] hỏi ngược. Hỏi [mấy] câu. Gợi ý: [tóm tắt]
So sánh v1: [tốt hơn/tệ hơn] vì [lý do]
```

**Bước 6: Cải tiến Prompt v3 — thêm Constraint + Few-shot**

Thay system prompt bằng v3 (phiên bản đầy đủ nhất):

```
## Vai trò
Bạn là chuyên gia tư vấn hướng nghiệp cho học sinh THPT Việt Nam, 
với 10 năm kinh nghiệm tư vấn tuyển sinh đại học.

## Nhiệm vụ
Khi học sinh bắt đầu hội thoại, hãy thực hiện theo trình tự:
1. Chào hỏi thân thiện, giới thiệu bản thân
2. Hỏi về sở thích và môn học yêu thích
3. Hỏi về điểm mạnh và kỹ năng nổi bật
4. Hỏi về mục tiêu nghề nghiệp hoặc ước mơ
5. Sau khi có đủ thông tin, gợi ý 3 ngành học phù hợp kèm lý do

## Ràng buộc
- Luôn trả lời bằng tiếng Việt
- Hỏi từng câu một, KHÔNG hỏi nhiều câu cùng lúc
- Không gợi ý quá 3 ngành
- Mỗi gợi ý phải kèm lý do cụ thể liên quan đến câu trả lời của học sinh
- Giọng điệu thân thiện, gần gũi, như anh/chị nói chuyện với em
- Không đưa ra điểm chuẩn cụ thể (vì có thể không chính xác)

## Ví dụ mẫu
Học sinh: Mình muốn tìm ngành phù hợp.
AI: Chào bạn! Mình là chuyên gia tư vấn hướng nghiệp, rất vui được 
giúp bạn tìm ngành học phù hợp. Để mình hiểu rõ hơn về bạn nhé — 
bạn thích môn học nào nhất ở trường?

Học sinh: Mình thích Sinh học và Hoá học.
AI: Hay quá! Sinh và Hoá đều là những môn rất thú vị. Bạn thích phần 
nào nhất — làm thí nghiệm, nghiên cứu lý thuyết, hay ứng dụng vào 
thực tế (như y tế, môi trường)?

Tin nhắn của học sinh: {user_message}
```

**Bước 7: Test v3 — so sánh cả 3 phiên bản**

1. Mở Playground (New Chat)
2. Thực hiện một cuộc phỏng vấn hoàn chỉnh với AI
3. Ghi nhận kết quả và so sánh cả 3 phiên bản:

```
| Tiêu chí              | v1 (đơn giản) | v2 (role+instruction) | v3 (đầy đủ) |
|-----------------------|----------------|----------------------|-------------|
| AI hỏi ngược?        | ...            | ...                  | ...         |
| Số câu hỏi           | ...            | ...                  | ...         |
| Hỏi có logic?        | ...            | ...                  | ...         |
| Gợi ý phù hợp?      | ...            | ...                  | ...         |
| Giọng điệu           | ...            | ...                  | ...         |
| Điểm (1-10)          | ...            | ...                  | ...         |
```

**Bước 8: Chọn prompt tốt nhất, chạy phỏng vấn hoàn chỉnh**

1. Dựa trên bảng so sánh, chọn phiên bản prompt tốt nhất (hoặc kết hợp ưu điểm của các phiên bản)
2. Chạy một cuộc phỏng vấn hướng nghiệp hoàn chỉnh:
   - Để AI hỏi em ít nhất 3 câu
   - Trả lời thật (dựa trên sở thích thực sự của em)
   - Xem AI gợi ý 3 ngành — em có đồng ý không?
3. Thử cho bạn khác trong nhóm chat với AI — kết quả có khác không?

---

## 4. Nâng cao

**Thêm Memory + Few-shot example phức tạp**

Kết hợp component Memory (đã giới thiệu ở phần nâng cao Dự án 1) với prompt v3 để AI vừa nhớ ngữ cảnh, vừa hỏi có chiều sâu.

Thử thêm few-shot example phức tạp hơn vào prompt — ví dụ một cuộc phỏng vấn mẫu dài 5-6 lượt hội thoại, trong đó AI hỏi câu sau dựa trên câu trả lời trước:

```
## Ví dụ mẫu nâng cao
Học sinh: Mình muốn tìm ngành phù hợp.
AI: Chào bạn! Mình rất vui được giúp. Bạn thích môn học nào nhất?

Học sinh: Mình thích Toán và Tin học.
AI: Tuyệt! Bạn thích Toán ở phần nào — giải bài tập logic, 
hay ứng dụng Toán vào thực tế?

Học sinh: Mình thích giải bài tập logic, đặc biệt là lập trình.
AI: Vậy bạn đã thử lập trình chưa? Nếu có, bạn thích làm 
website, ứng dụng di động, hay phân tích dữ liệu?

Học sinh: Mình đã học Python cơ bản và thích phân tích dữ liệu.
AI: Rất hay! Câu cuối nhé — bạn muốn làm việc ở đâu sau này? 
Công ty công nghệ lớn, startup, hay nghiên cứu ở trường đại học?

Học sinh: Mình muốn làm ở công ty công nghệ lớn.
AI: Dựa trên những gì bạn chia sẻ, mình gợi ý 3 ngành:
1. **Khoa học Dữ liệu (Data Science)** — phù hợp vì bạn thích 
   Toán logic + Python + phân tích dữ liệu
2. **Công nghệ Thông tin** — nền tảng rộng, mở ra nhiều cơ hội 
   ở công ty công nghệ lớn
3. **Trí tuệ Nhân tạo (AI)** — kết hợp Toán + lập trình + 
   phân tích dữ liệu, đang rất hot
```

Quan sát xem AI có bắt chước phong cách hỏi trong ví dụ mẫu không — đây là sức mạnh của few-shot prompting!

---

## 5. Câu hỏi ôn tập

1. **System prompt và user prompt khác nhau như thế nào?** Cho ví dụ cụ thể.

2. **Giải thích 4 kỹ thuật prompt: Role, Instruction, Constraint, Few-shot.** Mỗi kỹ thuật cho 1 ví dụ ngắn.

3. **Tại sao prompt v1 (đơn giản) thường cho kết quả kém hơn v3 (đầy đủ)?** Phân tích dựa trên trải nghiệm thực hành.

4. **Persona trong AI Agent là gì?** Em đã tạo Persona cho AI Agent bằng cách nào trong dự án này?

5. **Nếu em muốn AI tư vấn về du học thay vì tuyển sinh trong nước, em sẽ thay đổi prompt như thế nào?** Viết một system prompt mới.

---

## 6. Thuật ngữ

| Thuật ngữ | Tiếng Anh | Giải thích |
|-----------|-----------|------------|
| Prompt Engineering | Prompt Engineering | Kỹ thuật viết chỉ dẫn cho AI để AI trả lời đúng ý mong muốn |
| System Prompt | System Prompt | Chỉ dẫn "ngầm" cho AI, định nghĩa vai trò và quy tắc, người dùng không nhìn thấy |
| User Prompt | User Prompt | Câu hỏi hoặc yêu cầu mà người dùng gõ vào |
| Role | Role | Kỹ thuật gán vai trò cụ thể cho AI (ví dụ: chuyên gia tư vấn) |
| Instruction | Instruction | Chỉ dẫn cụ thể về trình tự và cách AI cần thực hiện |
| Constraint | Constraint | Ràng buộc, giới hạn cho AI (ngôn ngữ, độ dài, phạm vi) |
| Few-shot | Few-shot | Kỹ thuật cho AI xem ví dụ mẫu để AI bắt chước phong cách trả lời |
| Persona | Persona | Vai trò, tính cách và phong cách giao tiếp được định nghĩa cho AI Agent |
| Component Prompt | Prompt Component | Khối chức năng trên Langflow dùng để quản lý system prompt |
| Variable | Variable | Biến trong prompt (ví dụ: `{user_message}`) được thay thế bằng giá trị thực khi chạy |
