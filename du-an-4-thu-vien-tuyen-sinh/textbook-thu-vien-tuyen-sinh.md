# Dự án 4: Thư viện tuyển sinh thông minh

**Số buổi:** 1 x 90 phút

---

## 1. Giới thiệu dự án

### Bối cảnh thực tế

Ở Dự án 3, em đã cho AI Agent khả năng tìm kiếm web để kiểm chứng thông tin. Nhưng mỗi lần hỏi, agent phải search lại từ đầu — chậm, tốn tài nguyên, và kết quả có thể khác nhau mỗi lần.

Hãy tưởng tượng em có một thư viện chứa tất cả thông tin tuyển sinh của 3-5 trường đại học: ngành học, điểm chuẩn, học bổng, phương thức xét tuyển... Thay vì search Google mỗi lần, AI chỉ cần "mở sách" trong thư viện và tìm đúng trang cần thiết — nhanh hơn, chính xác hơn, và nhất quán hơn.

Đó chính là ý tưởng của **RAG (Retrieval-Augmented Generation)** — kỹ thuật cho phép AI truy xuất thông tin từ kho dữ liệu riêng trước khi trả lời. Trong dự án này, em sẽ xây dựng "thư viện tuyển sinh" bằng **AstraDB** (vector database) và kết nối với AI để tạo hệ thống hỏi đáp thông minh.

### Câu hỏi dẫn dắt

"Nếu có một thư viện chứa tất cả thông tin tuyển sinh, làm sao AI tìm đúng thông tin em cần trong vài giây?"

### Mục tiêu học tập

- [ ] Giải thích được tại sao cần vector database (tìm kiếm theo nghĩa, không phải từ khoá)
- [ ] Tạo AstraDB collection, kết nối với Langflow, nhúng thông tin tuyển sinh vào database
- [ ] Xây dựng flow RAG cơ bản: nhúng tài liệu → truy xuất → đưa vào LLM để trả lời
- [ ] So sánh kết quả có RAG vs. không có RAG với cùng câu hỏi

### Sản phẩm đầu ra

Flow RAG "thư viện tuyển sinh" trên Langflow — nhúng thông tin tuyển sinh của 3-5 trường ĐH vào AstraDB, hỏi đáp và so sánh chất lượng câu trả lời khi có RAG vs. không có RAG.

---

## 2. Kiến thức nền tảng

### 2.1 Vấn đề: LLM có giới hạn

Ở các dự án trước, em đã thấy 2 hạn chế lớn của LLM:


**1. Context window có giới hạn:**
LLM chỉ có thể xử lý một lượng text nhất định trong mỗi lần hỏi (gọi là context window). Nếu em copy toàn bộ thông tin tuyển sinh của 50 trường ĐH vào prompt, nó sẽ quá dài — LLM không xử lý được.

**2. LLM không nhớ dữ liệu riêng:**
LLM được huấn luyện trên dữ liệu chung (internet, sách, báo). Nó không biết dữ liệu riêng của em — ví dụ file PDF thông tin tuyển sinh mà giáo viên cung cấp.

**Ví dụ cụ thể:**

```
Em có file "Thông tin tuyển sinh ĐH Bách Khoa 2025.pdf" (20 trang)

Cách 1: Copy toàn bộ vào prompt → Quá dài, LLM không xử lý được
Cách 2: Không đưa gì → LLM không biết, có thể bịa thông tin
Cách 3: RAG → Lưu file vào database, khi hỏi chỉ truy xuất phần liên quan
         → LLM nhận đúng thông tin cần thiết → Trả lời chính xác ✅
```

### 2.2 Embedding là gì?

**Embedding** là quá trình biến đoạn text thành một dãy số (vector) để máy tính có thể hiểu "nghĩa" của đoạn text đó.

**Ví dụ trực quan:**

Hãy tưởng tượng mỗi đoạn text là một điểm trên bản đồ. Các đoạn text có nghĩa giống nhau sẽ nằm gần nhau:

```
Bản đồ nghĩa (đơn giản hoá):

    "Ngành CNTT"  ●────── gần ──────● "Khoa học Máy tính"
                                      
    "Điểm chuẩn Y khoa"  ●── gần ──● "Điểm thi vào ngành Y"
                                      
    "Học bổng toàn phần"  ●── gần ──● "Miễn 100% học phí"
                                      
    "Thời tiết hôm nay"  ●──── xa ────● "Điểm chuẩn Bách Khoa"
```

Khi em hỏi "Điểm chuẩn ngành Công nghệ thông tin?", hệ thống sẽ:
1. Biến câu hỏi thành vector
2. Tìm các vector gần nhất trong database → đó là các đoạn text liên quan nhất
3. Đưa các đoạn text đó cho LLM để trả lời

**Cách embedding hoạt động (đơn giản hoá):**

```
"Ngành CNTT Bách Khoa"  →  [0.82, 0.15, 0.93, 0.41, ...]  (vector 768 số)
"Điểm chuẩn Y khoa"     →  [0.11, 0.87, 0.23, 0.65, ...]  (vector 768 số)
```

Em không cần hiểu chi tiết các con số — chỉ cần biết rằng embedding giúp máy tính "hiểu nghĩa" của text và tìm kiếm theo nghĩa thay vì theo từ khoá.

> 💡 **Mẹo:** Tìm kiếm theo nghĩa (semantic search) khác tìm kiếm theo từ khoá (keyword search). Ví dụ: nếu em search từ khoá "CNTT", hệ thống chỉ tìm đoạn text chứa chữ "CNTT". Nhưng semantic search sẽ tìm cả đoạn text chứa "Công nghệ Thông tin", "Computer Science", "Khoa học Máy tính" — vì chúng có nghĩa tương tự!

### 2.3 Vector Database là gì?

**Vector Database** là cơ sở dữ liệu chuyên lưu trữ và tìm kiếm vector (dãy số từ embedding).

**So sánh với database thông thường:**

| Tiêu chí | Database thông thường | Vector Database |
|----------|----------------------|-----------------|
| Lưu trữ | Bảng, hàng, cột (text, số) | Vector (dãy số đại diện cho nghĩa) |
| Tìm kiếm | Theo từ khoá chính xác | Theo nghĩa (semantic search) |
| Ví dụ tìm "CNTT" | Chỉ tìm đoạn chứa chữ "CNTT" | Tìm cả "Công nghệ Thông tin", "IT", "Computer Science" |
| Ứng dụng | Quản lý dữ liệu có cấu trúc | Tìm kiếm thông minh cho AI |

**AstraDB** là vector database cloud (chạy trên internet) mà em sẽ dùng trong khoá học. AstraDB có:
- **Free tier** — miễn phí cho lượng sử dụng nhỏ
- **Component có sẵn trên Langflow** — dễ kết nối
- **Collection** — nơi lưu trữ các vector (giống "thư mục" trong thư viện)

### 2.4 RAG là gì?

**RAG (Retrieval-Augmented Generation)** — Tạo sinh có bổ sung truy xuất — là kỹ thuật kết hợp 3 bước:

```
Bước 1: RETRIEVAL (Truy xuất)
   Câu hỏi → Embedding → Tìm trong Vector Database → Lấy ra đoạn text liên quan

Bước 2: AUGMENTED (Bổ sung)
   Đoạn text liên quan + Câu hỏi gốc → Ghép thành prompt đầy đủ

Bước 3: GENERATION (Tạo sinh)
   Prompt đầy đủ → LLM → Câu trả lời chính xác dựa trên dữ liệu
```

**Ví dụ cụ thể:**

```
Em hỏi: "Điểm chuẩn ngành CNTT Bách Khoa 2024?"

Bước 1 - Truy xuất:
   → Tìm trong AstraDB → Tìm được đoạn: "Năm 2024, ngành Khoa học 
     Máy tính ĐH Bách Khoa TP.HCM có điểm chuẩn 27.85 điểm, 
     phương thức xét điểm thi THPT."

Bước 2 - Bổ sung:
   → Ghép: "Dựa trên thông tin sau: [đoạn text trên]. 
     Hãy trả lời câu hỏi: Điểm chuẩn ngành CNTT Bách Khoa 2024?"

Bước 3 - Tạo sinh:
   → LLM trả lời: "Điểm chuẩn ngành Khoa học Máy tính (CNTT) 
     ĐH Bách Khoa TP.HCM năm 2024 là 27.85 điểm theo phương thức 
     xét điểm thi THPT."
```

**Tại sao RAG tốt hơn?**

| Không có RAG | Có RAG |
|-------------|--------|
| LLM trả lời từ kiến thức cũ | LLM trả lời từ dữ liệu em cung cấp |
| Có thể bịa thông tin (hallucination) | Thông tin chính xác từ nguồn đáng tin |
| Không biết dữ liệu riêng | Biết dữ liệu riêng em đã nhúng |
| Không cập nhật | Cập nhật khi em thêm dữ liệu mới |

> ⚠️ **Lưu ý:** RAG không hoàn hảo. Nếu dữ liệu em nhúng vào database không chính xác, AI cũng sẽ trả lời sai. "Garbage in, garbage out" — dữ liệu rác vào, kết quả rác ra.

### 2.5 AstraDB — Vector Database cho khoá học

**AstraDB** là dịch vụ vector database cloud của DataStax. Em sẽ dùng AstraDB vì:

1. **Free tier:** Miễn phí cho lượng sử dụng nhỏ — đủ cho khoá học
2. **Component Langflow:** Có sẵn component AstraDB trên Langflow — kéo thả là xong
3. **Dễ sử dụng:** Giao diện web đơn giản, không cần cài đặt gì trên máy

**Các khái niệm trong AstraDB:**

| Khái niệm | Giải thích | Ví dụ |
|-----------|------------|-------|
| Database | Cơ sở dữ liệu — chứa nhiều collection | "tuyen-sinh-db" |
| Collection | Bộ sưu tập vector — giống thư mục | "thong-tin-truong" |
| Document | Một đoạn text đã được embedding | "Ngành CNTT Bách Khoa, điểm chuẩn 27.85..." |
| Vector | Dãy số đại diện cho nghĩa của document | [0.82, 0.15, 0.93, ...] |

---

## 3. Hướng dẫn thực hành

### 3.1 Vật tư và công cụ cần chuẩn bị

| STT | Vật tư / Công cụ | Số lượng | Ghi chú |
|-----|-------------------|----------|---------|
| 1 | Máy tính (laptop/PC) có kết nối internet | 1/HS hoặc 1/nhóm | Đã cài Langflow Desktop |
| 2 | Langflow Desktop | Đã cài | Phiên bản có component AstraDB |
| 3 | API key Google AI Studio | Đã tạo | Dùng lại key từ Dự án 1 |
| 4 | Tài khoản AstraDB | 1/nhóm | Đăng ký free tier tại [astra.datastax.com](https://astra.datastax.com) |
| 5 | File dữ liệu tuyển sinh | 3-5 file | Giáo viên chuẩn bị sẵn (text hoặc PDF) |

### 3.2 Hướng dẫn từng bước

**Bước 1: Đăng ký AstraDB free tier, tạo database và collection**

1. Mở trình duyệt, truy cập [https://astra.datastax.com](https://astra.datastax.com)
2. Nhấn **Sign Up** — đăng ký bằng Google Account hoặc email
3. Sau khi đăng nhập, nhấn **Create Database**:
   - **Database name:** `tuyen-sinh-db`
   - **Region:** Chọn region gần nhất (ví dụ: Asia Pacific)
   - **Provider:** Chọn provider mặc định
4. Chờ database được tạo (1-2 phút)
5. Vào database vừa tạo, nhấn **Create Collection**:
   - **Collection name:** `thong-tin-truong`
   - **Dimensions:** Chọn theo model embedding (thường 768 hoặc để auto)
   - **Similarity metric:** Cosine (mặc định)
6. Lấy thông tin kết nối:
   - **API Endpoint:** Copy URL endpoint của database
   - **Application Token:** Tạo token mới và copy

> 💡 **Mẹo:** Lưu API Endpoint và Application Token vào file text trên máy. Em sẽ cần chúng khi cấu hình Langflow.

**Bước 2: Chuẩn bị dữ liệu tuyển sinh**

Giáo viên sẽ cung cấp file text hoặc PDF chứa thông tin tuyển sinh của 3-5 trường ĐH. Mỗi file chứa:
- Tên trường, mã trường
- Danh sách ngành, mã ngành
- Điểm chuẩn các năm gần nhất
- Phương thức xét tuyển
- Thông tin học bổng (nếu có)

Ví dụ nội dung file `bach-khoa-tphcm.txt`:

```
ĐẠI HỌC BÁCH KHOA TP.HCM (HCMUT)
Mã trường: QSB

Ngành Khoa học Máy tính (mã: 7480101):
- Tổ hợp xét tuyển: A00 (Toán, Lý, Hoá), A01 (Toán, Lý, Anh)
- Điểm chuẩn 2024: 27.85 (xét điểm thi THPT)
- Điểm chuẩn 2023: 27.50
- Chỉ tiêu: 200

Ngành Kỹ thuật Phần mềm (mã: 7480103):
- Tổ hợp xét tuyển: A00, A01
- Điểm chuẩn 2024: 27.20
- Điểm chuẩn 2023: 26.80
- Chỉ tiêu: 150
...
```

> ⚠️ **Lưu ý:** Dữ liệu mẫu trên chỉ mang tính minh hoạ. Giáo viên sẽ cung cấp dữ liệu thực tế và cập nhật.

**Bước 3: Tạo flow nhúng dữ liệu (Ingest flow)**

Flow này có nhiệm vụ đọc file → chia nhỏ → embedding → lưu vào AstraDB.

1. Mở Langflow Desktop, tạo **New Flow**, đặt tên: "Nhúng dữ liệu tuyển sinh"
2. Kéo các component sau vào canvas:

   a. **File** (mục Inputs): Để upload file dữ liệu tuyển sinh
   
   b. **Text Splitter** (mục Processing): Chia file dài thành các đoạn nhỏ (chunk)
      - **Chunk size:** 500 (mỗi đoạn khoảng 500 ký tự)
      - **Chunk overlap:** 50 (các đoạn chồng nhau 50 ký tự để không mất ngữ cảnh)
   
   c. **Google Generative AI Embeddings** (mục Embeddings): Biến text thành vector
      - Cấu hình API key Google AI Studio
   
   d. **AstraDB** (mục Vector Stores): Lưu vector vào database
      - **API Endpoint:** Dán endpoint từ Bước 1
      - **Application Token:** Dán token từ Bước 1
      - **Collection Name:** `thong-tin-truong`

3. Kết nối theo thứ tự:

```
[File] ──→ [Text Splitter] ──→ [Google Generative AI Embeddings] ──→ [AstraDB]
(đọc file)   (chia nhỏ)          (biến text → vector)              (lưu vào DB)
```

**Bước 4: Chạy flow nhúng — kiểm tra dữ liệu đã vào AstraDB**

1. Trong component File, upload file dữ liệu tuyển sinh đầu tiên
2. Nhấn nút **Run** (hoặc **Build**) để chạy flow
3. Chờ flow hoàn thành — Langflow sẽ hiển thị trạng thái thành công
4. Kiểm tra trên AstraDB:
   - Quay lại trang web AstraDB
   - Vào collection `thong-tin-truong`
   - Xem số lượng document — nếu có dữ liệu, nghĩa là nhúng thành công!
5. Lặp lại với các file dữ liệu còn lại (3-5 trường)

> 💡 **Mẹo:** Nếu flow báo lỗi, kiểm tra: (1) API key đúng chưa? (2) AstraDB endpoint đúng chưa? (3) File có đúng định dạng text/PDF không?

**Bước 5: Tạo flow RAG (Query flow)**

Flow này có nhiệm vụ nhận câu hỏi → tìm thông tin liên quan trong AstraDB → đưa cho LLM trả lời.

1. Tạo **New Flow**, đặt tên: "Thư viện tuyển sinh RAG"
2. Kéo các component sau vào canvas:

   a. **Chat Input** (mục Inputs): Nhận câu hỏi từ em
   
   b. **AstraDB** (mục Vector Stores — chế độ Retriever/Search):
      - Cấu hình giống Bước 3 (endpoint, token, collection)
      - Đây là component tìm kiếm — nó sẽ tìm đoạn text liên quan nhất trong database
   
   c. **Google Generative AI Embeddings** (mục Embeddings):
      - Để embedding câu hỏi trước khi tìm kiếm
   
   d. **Prompt** (mục Prompts): Ghép thông tin tìm được + câu hỏi gốc
   
   e. **Google Generative AI** (mục Models): LLM trả lời
      - Cấu hình API key, chọn Gemma 4
   
   f. **Chat Output** (mục Outputs): Hiển thị câu trả lời

3. Viết prompt trong component Prompt:

```
Bạn là chuyên gia tư vấn tuyển sinh đại học Việt Nam.
Hãy trả lời câu hỏi của học sinh DỰA TRÊN thông tin được cung cấp bên dưới.
Nếu thông tin không đủ để trả lời, hãy nói rõ "Thông tin chưa có trong 
hệ thống" thay vì bịa.

## Thông tin tham khảo:
{context}

## Câu hỏi:
{user_message}

## Yêu cầu:
- Trả lời bằng tiếng Việt
- Trích dẫn thông tin cụ thể từ dữ liệu (điểm số, tên ngành, mã ngành)
- Nếu có nhiều thông tin liên quan, tổng hợp lại rõ ràng
```

4. Kết nối flow:

```
[Chat Input] ──→ [Embedding] ──→ [AstraDB Retriever] ──→ [Prompt] ──→ [LLM] ──→ [Chat Output]
                                        ↓                    ↑
                                  (context — đoạn text    (user_message —
                                   tìm được từ DB)        câu hỏi gốc)
```

Chi tiết kết nối:
- Chat Input → Embedding (để embedding câu hỏi)
- Embedding → AstraDB Retriever (để tìm kiếm trong DB)
- AstraDB Retriever → Prompt (nối vào biến `{context}`)
- Chat Input → Prompt (nối vào biến `{user_message}`)
- Prompt → Google Generative AI → Chat Output

**Bước 6: Test hỏi đáp — AI trả lời từ dữ liệu đã nhúng**

1. Mở Playground
2. Hỏi các câu liên quan đến dữ liệu đã nhúng:
   - "Điểm chuẩn ngành CNTT Bách Khoa 2024 là bao nhiêu?"
   - "Trường nào có ngành Kỹ thuật Phần mềm?"
   - "So sánh điểm chuẩn ngành CNTT giữa Bách Khoa và trường X"
3. Kiểm tra: AI có trả lời đúng từ dữ liệu đã nhúng không?
4. Thử hỏi câu mà dữ liệu KHÔNG có:
   - "Điểm chuẩn ngành Luật ĐH Luật TP.HCM?" (nếu chưa nhúng dữ liệu trường này)
   - AI có nói "Thông tin chưa có trong hệ thống" không? Hay nó bịa?

**Bước 7: So sánh — hỏi cùng câu hỏi KHÔNG có RAG vs. CÓ RAG**

Đây là bước quan trọng nhất — giúp em thấy rõ giá trị của RAG.

1. Mở flow Dự án 1 (Chat Input → LLM → Chat Output, KHÔNG có RAG)
2. Hỏi: "Điểm chuẩn ngành CNTT Bách Khoa 2024?"
3. Ghi nhận câu trả lời

4. Mở flow RAG (Dự án 4)
5. Hỏi cùng câu hỏi: "Điểm chuẩn ngành CNTT Bách Khoa 2024?"
6. Ghi nhận câu trả lời

7. So sánh:

```
| Tiêu chí           | Không RAG (Dự án 1) | Có RAG (Dự án 4) |
|--------------------|---------------------|-------------------|
| Có trả lời được?   | ...                 | ...               |
| Thông tin chính xác?| ...                | ...               |
| Có dẫn nguồn?      | ...                 | ...               |
| Có bịa thông tin?  | ...                 | ...               |
| Tốc độ trả lời     | ...                 | ...               |
```

> 💡 **Mẹo:** Thử hỏi 3-5 câu khác nhau và so sánh. Em sẽ thấy rõ: với câu hỏi cần dữ liệu cụ thể, RAG cho kết quả tốt hơn hẳn!

---

## 4. Nâng cao

**Thêm Text Splitter với chunk size tối ưu và metadata**

Chunk size (kích thước mỗi đoạn khi chia nhỏ) ảnh hưởng lớn đến chất lượng RAG:

- **Chunk quá nhỏ** (100 ký tự): Mỗi đoạn chứa quá ít thông tin → AI thiếu ngữ cảnh
- **Chunk quá lớn** (2000 ký tự): Mỗi đoạn chứa nhiều thông tin không liên quan → AI bị nhiễu
- **Chunk vừa phải** (300-500 ký tự): Cân bằng giữa đủ ngữ cảnh và tập trung

Thử nghiệm:
1. Nhúng cùng dữ liệu với chunk size 200, 500, và 1000
2. Hỏi cùng câu hỏi với mỗi phiên bản
3. So sánh chất lượng câu trả lời — chunk size nào cho kết quả tốt nhất?

Ngoài ra, em có thể thêm **metadata** cho mỗi document khi nhúng — ví dụ: tên trường, năm, loại thông tin (điểm chuẩn / ngành / học bổng). Metadata giúp lọc kết quả chính xác hơn.

---

## 5. Câu hỏi ôn tập

1. **Embedding là gì?** Giải thích bằng ví dụ đơn giản tại sao "Ngành CNTT" và "Khoa học Máy tính" có vector gần nhau.

2. **Vector database khác database thông thường ở điểm nào?** Cho ví dụ về tìm kiếm theo từ khoá vs. tìm kiếm theo nghĩa.

3. **RAG gồm 3 bước nào?** Mô tả từng bước khi em hỏi "Trường nào có học bổng toàn phần?"

4. **Tại sao kết quả có RAG tốt hơn không có RAG?** Dựa trên trải nghiệm so sánh ở Bước 7, phân tích cụ thể.

5. **Nếu AI trả lời sai dù đã có RAG, nguyên nhân có thể là gì?** (Gợi ý: nghĩ về chất lượng dữ liệu, chunk size, prompt)

---

## 6. Thuật ngữ

| Thuật ngữ | Tiếng Anh | Giải thích |
|-----------|-----------|------------|
| Embedding | Embedding | Quá trình biến text thành vector (dãy số) để máy tính hiểu "nghĩa" |
| Vector | Vector | Dãy số đại diện cho nghĩa của một đoạn text |
| Vector Database | Vector Database | Cơ sở dữ liệu chuyên lưu trữ và tìm kiếm vector |
| AstraDB | AstraDB | Dịch vụ vector database cloud của DataStax, có free tier |
| Collection | Collection | Bộ sưu tập vector trong AstraDB — giống thư mục trong thư viện |
| RAG | Retrieval-Augmented Generation | Kỹ thuật truy xuất thông tin từ database rồi đưa cho LLM để trả lời |
| Retrieval | Retrieval | Bước truy xuất — tìm đoạn text liên quan trong database |
| Context window | Context window | Giới hạn lượng text mà LLM có thể xử lý trong một lần |
| Chunk | Chunk | Đoạn text nhỏ sau khi chia nhỏ tài liệu dài |
| Chunk size | Chunk size | Kích thước mỗi đoạn khi chia nhỏ (tính bằng ký tự) |
| Chunk overlap | Chunk overlap | Phần chồng nhau giữa các chunk liền kề để không mất ngữ cảnh |
| Semantic search | Semantic search | Tìm kiếm theo nghĩa — khác với tìm kiếm theo từ khoá |
| Ingest | Ingest | Quá trình nhúng (đưa) dữ liệu vào vector database |
| Metadata | Metadata | Thông tin mô tả bổ sung cho document (tên trường, năm, loại) |
