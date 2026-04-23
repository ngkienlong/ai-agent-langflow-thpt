# Sản phẩm cuối khoá: Hệ thống AI Agent tư vấn tuyển sinh đại học

**Khoá học:** AI Agent với Langflow — THPT
**Số buổi:** 8 buổi × 90 phút
**Công cụ:** Langflow Desktop, Gemma 4 (free tier, Google AI Studio), AstraDB, Web Search, Notion MCP Server

---

## Mô tả tổng quan

Học sinh tự xây dựng một hệ thống AI Agent tư vấn tuyển sinh đại học trên Langflow Desktop, gồm **3 agent phối hợp** với nhau. Hỏi đáp trực tiếp trên Playground/Chat box của Langflow Desktop.

---

## Giai đoạn 1 — Agent Chiến lược

### Nhiệm vụ

Hỏi học sinh 8 câu mở đầu, sau đó tự động sinh ra một **bảng hỏi cá nhân hoá** (mỗi học sinh nhận bảng hỏi khác nhau tuỳ sở thích, điểm mạnh, hoàn cảnh). Bảng hỏi giúp học sinh tự phân tích và đánh giá lại bản thân + mục tiêu. Bảng hỏi được xuất ra Notion qua MCP để học sinh trả lời trực tiếp.

### 8 câu hỏi mở đầu (cố định cho mọi học sinh)

1. Em đang học lớp mấy? Khối nào?
2. Em muốn học ở khu vực nào? (Hà Nội / TP.HCM / không giới hạn)
3. Môn nào em thích nhất?
4. Môn nào em tự tin nhất?
5. Em có sở thích hoặc hoạt động ngoại khoá gì?
6. Em đã có dự tính học ngành/lĩnh vực gì? (hoặc chưa biết)
7. Điểm trung bình / điểm thi thử khoảng bao nhiêu?
8. Có yếu tố nào quan trọng khác không? (học bổng, học phí, ký túc xá...)

### Ví dụ bảng hỏi cá nhân hoá

#### Ví dụ 1 — Học sinh A (thích Sinh học, CLB môi trường, khối B, chưa biết ngành)

**Câu trả lời 8 câu mở đầu của A:**

- Lớp 11, khối B
- Không giới hạn khu vực
- Thích Sinh học nhất
- Tự tin nhất môn Hoá
- Tham gia CLB môi trường của trường
- Chưa biết ngành cụ thể
- Điểm TB 8.5
- Muốn có học bổng

**→ Bảng hỏi cá nhân hoá Agent sinh ra:**

| # | Câu hỏi                                                                                                                                      | Mục đích                             |
| - | ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| 1 | Trong Sinh học, em thích phần nào nhất: di truyền, sinh thái, vi sinh, hay sinh học phân tử?                                         | Xác định chuyên ngành hẹp         |
| 2 | Em thích làm việc trong phòng thí nghiệm hay ngoài thực địa hơn?                                                                    | Định hướng môi trường làm việc |
| 3 | Trong CLB môi trường, em thường đảm nhận vai trò gì: tổ chức sự kiện, nghiên cứu, hay truyền thông?                          | Đánh giá kỹ năng mềm              |
| 4 | Em đã từng nghe đến các ngành như Công nghệ sinh học, Khoa học môi trường, Y sinh chưa? Ngành nào em thấy hứng thú nhất? | Gợi mở ngành phù hợp khối B       |
| 5 | Em hình dung 5 năm sau mình đang làm công việc gì?                                                                                     | Mục tiêu dài hạn                    |
| 6 | Gia đình em có định hướng hoặc kỳ vọng gì về ngành học không?                                                                   | Yếu tố gia đình                     |
| 7 | Với điểm TB 8.5, em có tự tin đạt 26+ điểm thi THPT không?                                                                           | Đánh giá năng lực thực tế        |
| 8 | Học bổng em quan tâm là học bổng toàn phần, bán phần, hay học bổng khuyến khích học tập trong trường?                        | Làm rõ ràng buộc tài chính        |

#### Ví dụ 2 — Học sinh B (thích Toán, code game, khối A, muốn học CNTT)

**Câu trả lời 8 câu mở đầu của B:**

- Lớp 12, khối A
- Muốn học ở TP.HCM
- Thích Toán nhất
- Tự tin nhất môn Toán
- Code game Unity lúc rảnh
- Dự tính học CNTT
- Điểm thi thử 25
- Lo học phí cao

**→ Bảng hỏi cá nhân hoá Agent sinh ra:**

| # | Câu hỏi                                                                                                                         | Mục đích                      |
| - | --------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| 1 | Trong CNTT em thích hướng nào: lập trình ứng dụng, AI/ML, an toàn thông tin, game development, hay khoa học dữ liệu? | Xác định chuyên ngành CNTT  |
| 2 | Em đã làm được game nào hoàn chỉnh chưa? Ngôn ngữ/engine gì?                                                         | Đánh giá mức độ tự học   |
| 3 | Em có quan tâm đến các ngành giao thoa như Toán-Tin, Khoa học máy tính (nghiêng lý thuyết hơn) không?             | Mở rộng lựa chọn             |
| 4 | Ở TP.HCM, em đã tìm hiểu về ĐH Bách Khoa, KHTN, FPT, Fulbright, RMIT chưa? Trường nào em thấy phù hợp?             | Định hướng trường cụ thể |
| 5 | Với 25 điểm thi thử, em có biết điểm chuẩn CNTT các trường em quan tâm khoảng bao nhiêu không?                    | Đánh giá tính khả thi       |
| 6 | Em sẵn sàng học công lập (học phí thấp, cạnh tranh cao) hay tư thục (học phí cao, nhiều học bổng)?                | Làm rõ ràng buộc tài chính |
| 7 | Ngoài code game, em có làm project cá nhân nào khác không (web, app, AI)?                                                 | Đánh giá portfolio            |
| 8 | Mục tiêu xa của em là đi làm trong nước, startup riêng, hay đi du học / làm ở nước ngoài?                         | Mục tiêu dài hạn             |

### Output

- Bảng hỏi cá nhân hoá được đẩy lên **Notion database** (mỗi học sinh 1 entry/page) qua Notion MCP Server.
- Học sinh mở Notion → trả lời trực tiếp → lưu lại thành file/page để dùng cho Agent Tư vấn ở giai đoạn 3.

---

## Giai đoạn 2 — Agent Thông tin tuyển sinh

### Nhiệm vụ

#### Nhiệm vụ 1 — Trả lời câu hỏi của user (ưu tiên RAG → fallback Web Search)

- **Bước 1:** Agent tra cứu dữ liệu đã lưu trong AstraDB (RAG).
- **Bước 2:** Nếu RAG có đủ thông tin → trả lời kèm dẫn nguồn.
- **Bước 3:** Nếu RAG không có hoặc không đủ thông tin → Agent **báo cho user** và **hỏi có muốn tìm kiếm trên các trang web uy tín không**. Nếu user đồng ý → dùng Web Search.

#### Nhiệm vụ 2 — Thu thập & lưu trữ dữ liệu vào AstraDB (phục vụ RAG)

- Nhận lệnh từ user (ví dụ: "Thu thập thông tin trường X" hoặc đính kèm file PDF/text tuyển sinh).
- Search web từ nguồn uy tín → chunking → embedding → lưu AstraDB kèm metadata (tên trường, ngành, năm, loại thông tin).

#### Nhiệm vụ 3 — Trích xuất phi cấu trúc → Notion

- Đọc dữ liệu text thô từ AstraDB → LLM trích xuất thành các trường có cấu trúc → đẩy lên Notion database qua MCP.

### Ví dụ câu hỏi Agent Thông tin tuyển sinh có thể trả lời

#### Nhóm 1 — Tra cứu thông tin cụ thể (RAG trả lời ngay nếu có dữ liệu)

- "Điểm chuẩn ngành Y đa khoa ĐH Y Hà Nội năm 2024 là bao nhiêu?"
- "ĐH Bách Khoa Hà Nội có ngành Trí tuệ nhân tạo không?"
- "Học phí ngành CNTT của ĐH FPT là bao nhiêu một năm?"
- "Ngành Logistics ở ĐH Ngoại thương xét tuyển tổ hợp nào?"

#### Nhóm 2 — So sánh, tổng hợp (RAG + LLM tổng hợp)

- "So sánh điểm chuẩn ngành CNTT giữa ĐH Bách Khoa và ĐH KHTN TP.HCM 3 năm gần nhất."
- "Liệt kê các trường ở Hà Nội có ngành Khoa học dữ liệu, kèm điểm chuẩn 2024."
- "Trường nào có học bổng toàn phần cho ngành Y?"
- "Tổng hợp các phương thức xét tuyển của ĐH Kinh tế Quốc dân năm 2025."

#### Nhóm 3 — Câu hỏi mà RAG có thể không đủ thông tin → chuyển Web Search

- "Điểm chuẩn dự báo ngành CNTT năm 2026 là bao nhiêu?" *(chưa có trong dữ liệu)*
- "Có tin tức gì mới về kỳ thi đánh giá năng lực ĐHQG năm nay không?" *(tin mới)*
- "Mốc thời gian nộp hồ sơ xét tuyển sớm 2026 của các trường là khi nào?" *(thông tin cập nhật liên tục)*

**Agent phản hồi mẫu khi RAG không đủ:**

> "Tôi không tìm thấy thông tin về điểm chuẩn dự báo năm 2026 của ngành CNTT trong cơ sở dữ liệu hiện có. Em có muốn anh/chị tìm kiếm trên các trang web uy tín (website trường, Bộ GD&ĐT, báo chính thống) không?"

#### Nhóm 4 — Yêu cầu thu thập & trích xuất lên Notion

- "Thu thập thông tin điểm chuẩn 2024 của 5 trường top đầu Hà Nội rồi đẩy lên Notion."
- "Đọc file PDF tuyển sinh ĐH Bách Khoa TP.HCM em đính kèm và thêm các ngành CNTT vào Notion database 'Ngành học'."
- "Trích xuất danh sách học bổng của ĐH FPT thành bảng trên Notion."

### Cơ sở dữ liệu trên Notion (sau khi trích xuất)

1. **Trường Đại học** — Tên, mã trường, loại hình, địa chỉ, website, mô tả
2. **Ngành học** — Tên ngành, mã ngành, trường (liên kết), tổ hợp xét tuyển, phương thức, cơ hội nghề nghiệp
3. **Điểm chuẩn** — Trường + Ngành (liên kết), năm, điểm chuẩn theo phương thức, chỉ tiêu
4. **Học bổng** — Tên học bổng, trường (liên kết), đối tượng, giá trị, điều kiện, thời hạn

---

## Giai đoạn 3 — Agent Tư vấn

### Nhiệm vụ

Tư vấn chọn trường, ngành nghề, kế hoạch học tập cho học sinh dựa trên:

- Thông tin cá nhân của học sinh (từ Agent Chiến lược giai đoạn 1)
- Dữ liệu tuyển sinh (từ Agent Thông tin giai đoạn 2)

### Luồng hoạt động khi bắt đầu tư vấn

**Bước 1 — Agent hỏi:** *"Em có file trả lời bảng hỏi cá nhân hoá từ Agent Chiến lược không?"*

**Bước 2a — Nếu CÓ file:**

- User đính kèm file (hoặc paste nội dung / link Notion).
- Agent đọc, phân tích, rồi bắt đầu tư vấn ngay.

**Bước 2b — Nếu KHÔNG có file:**

- Agent hỏi nhanh 8 câu (giống Agent Chiến lược):
  1. Em đang học lớp mấy? Khối nào?
  2. Em muốn học ở khu vực nào?
  3. Môn nào em thích nhất?
  4. Môn nào em tự tin nhất?
  5. Em có sở thích hoặc hoạt động ngoại khoá gì?
  6. Em đã có dự tính học ngành/lĩnh vực gì?
  7. Điểm trung bình / điểm thi thử khoảng bao nhiêu?
  8. Có yếu tố nào quan trọng khác không?
- Sau khi có câu trả lời → bắt đầu tư vấn.

**Bước 3 — Tư vấn:**

- Agent gọi Agent Thông tin (hoặc dùng chung tool RAG/Web Search) để tra cứu dữ liệu trường/ngành/điểm chuẩn phù hợp với hồ sơ học sinh.
- Đưa ra đề xuất: 3-5 ngành phù hợp, 3-5 trường phù hợp, kế hoạch học tập (môn cần tập trung, phương thức xét tuyển nên chọn, học bổng có thể nhắm đến).

### Ví dụ output tư vấn

> **Dựa trên thông tin của em** (lớp 12 khối A, TP.HCM, thích Toán, code game, điểm thi thử 25, lo học phí):
>
> **3 ngành phù hợp nhất:**
>
> 1. Kỹ thuật Phần mềm — ĐH Bách Khoa TP.HCM (điểm chuẩn 2024: 26.5, hơi cao nhưng khả thi nếu em tăng 1-2đ)
> 2. Khoa học Máy tính — ĐH KHTN TP.HCM (điểm chuẩn 2024: 26.0, phù hợp hướng AI/ML)
> 3. Công nghệ Thông tin — ĐH Sư phạm Kỹ thuật TP.HCM (điểm chuẩn 2024: 24.5, an toàn với mức điểm hiện tại)
>
> **Kế hoạch học tập:**
>
> - Tập trung môn Toán (đã mạnh) + Vật lý (để chắc 8+ điểm)
> - Cân nhắc thi đánh giá năng lực ĐHQG TP.HCM (phương thức phụ)
> - Tiếp tục code game → làm 2-3 project có GitHub để bổ sung hồ sơ xét tuyển học bổng
>
> **Học bổng gợi ý:** Học bổng khuyến khích học tập của ĐH Bách Khoa (top 10% đầu vào), học bổng tài năng ĐH KHTN...

---

## Kiến trúc kỹ thuật tổng quan

```
┌─────────────────────────┐      ┌──────────────────────────┐      ┌─────────────────────┐
│  Agent Chiến lược       │      │ Agent Thông tin tuyển sinh│      │  Agent Tư vấn       │
│  (Giai đoạn 1)          │      │ (Giai đoạn 2)            │      │  (Giai đoạn 3)      │
│                         │      │                          │      │                     │
│  Gemma 4 + Notion MCP   │      │ Gemma 4 + Web Search     │      │ Gemma 4 + RAG +     │
│                         │      │ + AstraDB (RAG)          │      │ Web Search + Notion │
│                         │      │ + Notion MCP             │      │                     │
└──────────┬──────────────┘      └────────┬─────────────────┘      └──────────┬──────────┘
           │                              │                                   │
           ▼                              ▼                                   ▼
    ┌──────────────┐              ┌──────────────┐                    ┌──────────────┐
    │ Notion:      │              │ AstraDB:     │                    │ Đọc:         │
    │ Bảng hỏi cá  │              │ Vector store │                    │ - File bảng  │
    │ nhân hoá     │              │ tuyển sinh   │                    │   hỏi (GĐ 1) │
    └──────────────┘              │              │                    │ - AstraDB    │
                                  │ Notion:      │                    │ - Web Search │
                                  │ 4 database   │                    │              │
                                  │ tuyển sinh   │                    │ Output:      │
                                  └──────────────┘                    │ Tư vấn cá    │
                                                                       │ nhân hoá     │
                                                                       └──────────────┘
```

---

## Công cụ và tài khoản cần thiết

**Phần mềm:**

- Langflow Desktop (miễn phí, cài trên máy)
- Google AI Studio — API key Gemma 4 (free tier)
- AstraDB — free tier
- Notion — workspace miễn phí
- Notion MCP Server

**Tài khoản cần tạo trước khoá học:**

- Google Account
- DataStax Account (AstraDB)
- Notion Account
