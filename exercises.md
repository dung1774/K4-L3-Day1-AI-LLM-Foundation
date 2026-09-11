# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tăng temperature từ 0.0 lên 1.5, phản hồi của model có xu hướng khác nhau và khó đoán. Ở temperature thấp, model trả lời giống nhau và tập trung vào một cách diễn đạt, còn ở temperature cao thì các phản hồi của model thay đổi nhiều hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature khoảng 0.2–0.4 cho chatbot hỗ trợ khách hàng. Tôi nghĩ mức này giúp câu trả lời ổn định và phù hợp, đồng thời vẫn diễn đạt tự nhiên và không quá máy móc.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Với cùng lượng token đầu ra, GPT-4o có chi phí gấp khoảng 16,7 lần GPT-4o-mini. GPT-4o xứng đáng với chi phí cao hơn khi xử lý các yêu cầu phức tạp, cần khả năng suy luận và chất lượng câu trả lời cao. Ta sẽ dùng mini với các tác vụ đơn giản như trả lời câu hỏi thường gặp hoặc phân loại yêu cầu của khách hàng.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona là giáo viên tiểu học, model có xu hướng dùng từ vựng đơn giản, câu ngắn và các ví dụ gần gũi với trẻ em. Với persona là chuyên gia tài chính, câu trả lời có xu hướng chuyên sâu hơn, sử dụng nhiều thuật ngữ kỹ thuật và giải thích dưới góc nhìn tài chính. Như vậy, cùng một câu hỏi nhưng system prompt có thể thay đổi cách model lựa chọn từ ngữ, mức độ chi tiết và cách trình bày.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Đoạn văn có 80 từ. Hàm `count_tokens` cho kết quả 91 token, trong khi cách ước lượng số từ / 0.75 cho khoảng 106.67 token. Hai con số chênh nhau khoảng 14.69% so với giá trị ước lượng. Sự khác biệt là do token không tương ứng trực tiếp với một từ; cách mã hóa token phụ thuộc vào từng ngôn ngữ và cách viết. Với tiếng Việt, dấu, ký tự và các đơn vị từ có thể được tách thành nhiều token, nên số token có thể cao hơn so với tiếng Anh có cùng độ dài.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt quan trọng khi model tạo ra câu trả lời dài hoặc mất nhiều thời gian xử lý, vì người dùng có thể nhìn thấy kết quả từng phần thay vì phải chờ toàn bộ câu trả lời hoàn thành. Điều này giúp giảm cảm giác phải chờ đợi và làm trải nghiệm tương tác tự nhiên hơn, đặc biệt với chatbot. Ngược lại, non-streaming phù hợp hơn khi câu trả lời ngắn, khi cần xử lý toàn bộ kết quả trước khi hiển thị, hoặc khi ứng dụng cần một response hoàn chỉnh để thực hiện bước xử lý tiếp theo.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm áp lực lên API khi hệ thống đang quá tải bằng cách tăng dần thời gian chờ giữa các lần retry, ví dụ 1 giây, 2 giây, 4 giây, 8 giây. So với delay cố định, cách này cho API thêm thời gian để phục hồi trước khi nhận thêm request. Nếu hàng nghìn client cùng retry với delay cố định giống nhau, chúng có thể gửi request lại đồng thời, tạo ra một đợt tải mới và khiến tình trạng quá tải kéo dài hoặc nghiêm trọng hơn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona là một trợ lý AI hỗ trợ học tập và lập trình. System prompt của tôi là: "Bạn là một trợ lý AI hỗ trợ học tập và lập trình. Hãy trả lời bằng tiếng Việt, giải thích rõ ràng, dễ hiểu và đưa ra ví dụ khi cần. Ưu tiên câu trả lời ngắn gọn nhưng đủ thông tin và không tự bịa thông tin khi không chắc chắn." Tôi chỉ định trả lời bằng tiếng Việt để phù hợp với người dùng và giúp việc trao đổi dễ dàng hơn. Yêu cầu trả lời ngắn gọn giúp giảm thông tin không cần thiết, trong khi yêu cầu không tự bịa thông tin giúp tăng độ tin cậy của trợ lý.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất của trợ lý hiện tại là history chỉ lưu tối đa 3 lượt hội thoại gần nhất, nên trợ lý có thể mất những thông tin quan trọng từ các lượt trao đổi cũ. Một cải thiện cụ thể là xây dựng bộ nhớ dài hạn bằng cách lưu các thông tin quan trọng của người dùng vào cơ sở dữ liệu hoặc vector database. Khi có câu hỏi mới, hệ thống có thể tìm kiếm những thông tin liên quan từ bộ nhớ và đưa chúng vào context trước khi gọi model, giúp trợ lý duy trì được ngữ cảnh trong các cuộc hội thoại dài hơn.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
