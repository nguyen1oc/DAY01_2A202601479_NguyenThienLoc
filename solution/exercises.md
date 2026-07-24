# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng placeholder bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Temperature càng cao thì câu trả lời càng ngẫu nhiên, sáng tạo nhưng dễ lặp từ hoặc phi logic; temperature thấp (0.0) mang lại câu trả lời nhất quán, ổn định và lặp lại chính xác cùng một nội dung.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Đặt khoảng 0.0 đến 0.2 vì chatbot hỗ trợ khách hàng cần cung cấp thông tin chính xác, nhất quán từ cơ sở dữ liệu và tránh việc tự ý sáng tạo thông tin sai lệch.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> GPT-4o đắt hơn GPT-4o-mini khoảng 16.6 lần cho chi phí output. Nên dùng GPT-4o cho các tác vụ lập luận logic phức tạp hoặc lập trình, dùng mini cho các tác vụ phân loại văn bản hoặc chat cơ bản.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Phản hồi giáo viên tiểu học ngắn gọn, dùng từ ngữ đơn giản và ví dụ cuốn sổ ghi chép chung; phản hồi chuyên gia tài chính dùng thuật ngữ chuyên môn phức tạp hơn. System prompt ảnh hưởng sâu sắc giúp định hình phong cách, tông giọng và độ phức tạp của câu trả lời.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Hai con số chênh nhau khoảng 30-50%. Tiếng Việt tốn nhiều token hơn vì bộ mã hóa của OpenAI tối ưu cho tiếng Anh, khiến các từ ghép tiếng Việt bị tách thành nhiều mảnh token/byte nhỏ hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất cho chatbot thời gian thực nhằm giảm cảm giác chờ đợi của người dùng khi AI tạo ra câu trả lời dài. Non-streaming phù hợp hơn khi hệ thống cần tiền xử lý phản hồi hoặc trích xuất dữ liệu có cấu trúc (như JSON) trước khi hiển thị cho người dùng.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp kéo dãn thời gian chờ giữa các lần thử lại, giúp giảm tải dồn dập cho server đang bị nghẽn. Nếu hàng nghìn client cùng retry với delay cố định, nó sẽ tạo ra làn sóng yêu cầu đồng thời làm server tiếp tục quá tải.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt." Từ khóa "ngắn gọn" giúp tối ưu hóa chi phí token và tốc độ phản hồi; từ khóa "tiếng Việt" đảm bảo AI giao tiếp đúng ngôn ngữ học tập của học viên.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ lưu 3 lượt gần nhất và không ghi nhớ ngữ cảnh dài hạn giữa các phiên chat. Cải thiện: Tích hợp RAG sử dụng vector database để lưu trữ và tìm kiếm lại lịch sử hội thoại cũ khi cần thiết.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
