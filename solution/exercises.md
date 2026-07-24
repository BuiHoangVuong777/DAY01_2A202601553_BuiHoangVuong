# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> *Qua bốn mức temperature, mình thấy khi temperature tăng thì câu trả lời thường đa dạng và sáng tạo hơn, ít giống nhau giữa các lần gọi hơn. 
Ở temperature thấp, câu trả lời thường ổn định, ngắn gọn và bám sát prompt hơn.*

Với riêng độ dài hay tốc độ phản hồi, temperature không tạo ra quy luật tăng/giảm tuyến tính rõ ràng; nó chủ yếu ảnh hưởng đến mức độ ngẫu nhiên và sáng tạo của nội dung.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tôi chọn temperature thấp, khoảng 0.2–0.3, vì chatbot hỗ trợ khách hàng cần phản hồi ổn định, chính xác và ít ngẫu nhiên.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *- Với workload này, GPT-4o đắt hơn GPT-4o-mini khoảng 16,7 lần. 
GPT-4o đáng dùng khi cần chất lượng câu trả lời cao hơn, còn GPT-4o-mini phù hợp hơn cho chatbot phổ thông hoặc tác vụ cần tiết kiệm chi phí.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Phản hồi thứ nhất ngắn hơn, dễ hiểu hơn và dùng từ ngữ rất gần gũi . Phản hồi thứ hai dài hơn nhiều, dùng nhiều thuật ngữ chuyên môn. bản giáo viên tiểu học ví blockchain như cuốn sổ, còn bản chuyên gia tài chính giải thích theo cơ chế kỹ thuật của hệ thống. Như vậy, system prompt định hướng mạnh cách model chọn từ, độ sâu nội dung và phong cách trình bày, dù câu hỏi đầu vào vẫn giống nhau*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Theo data chạy, đoạn văn có 72 từ, Part 1 ước lượng 96 token, còn tiktoken đếm được 91 token. Hai con số chênh nhau khoảng 5.49%.

Tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài vì một từ tiếng Việt có thể gồm nhiều âm tiết và dấu cách không luôn trùng với ranh giới token. Ngoài ra, tokenizer thường tách các phần chữ, dấu và cụm từ theo cách tối ưu cho mô hình, nên số token thực tế có thể cao hơn ước lượng thô từ số từ.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming quan trọng nhất khi người dùng cần cảm giác phản hồi ngay lập tức, chẳng hạn chatbot đối thoại, hoặc các tác vụ sinh văn bản dài vì họ có thể đọc dần câu trả lời và không phải chờ toàn bộ nội dung xong mới thấy gì. Non-streaming phù hợp hơn khi cần đơn giản hóa code và dễ kiểm soát toàn bộ phản hồi trước khi dùng tiếp như trong các bước hậu xử lý, test tự động, hoặc trả về JSON có cấu trúc*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff giúp giảm số request retry dồn dập ngay khi API đang quá tải. Nếu dùng delay cố định, hàng nghìn client sẽ retry cùng một nhịp làm server càng nghẽn hơn*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Em chọn persona: “Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt.” System prompt này giúp trợ lý giữ phong cách gần gũi và dễ hiểu hơn trong suốt phiên chat. Cụm “trả lời ngắn gọn” được dùng để giảm câu trả lời dài dòng, còn “bằng tiếng Việt” giúp đầu ra thống nhất với ngôn ngữ người dùng và phù hợp bối cảnh bài lab*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Hạn chế lớn nhất của trợ lý hiện tại là chỉ giữ 3 lượt hội thoại gần nhất, nên dễ mất ngữ cảnh khi cuộc trò chuyện dài hơn. Một cải thiện cụ thể là thêm bước tóm tắt history cũ trước khi cắt bớt message, rồi chèn bản tóm tắt đó vào prompt ở các lượt sau để vừa tiết kiệm token vừa giữ được thông tin quan trọng*

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
