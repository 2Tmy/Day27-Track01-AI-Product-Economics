 05 · Recommendation + Justification — Kết luận & Chuẩn bị Present

> **Mục tiêu**: Chọn 1 config (hoặc combo) nhóm recommend deploy, viết justification ngắn gọn, và chuẩn bị 5 phút present.
>
> **Thời gian**: 10 phút (cuối phần Final) — Pens down lúc 12:00

---

## 4 câu hỏi nhóm phải trả lời

### Câu 1 — Recommend config nào?

Nhóm đề xuất chọn **Config 3 (Smart Mix - Hybrid Router)** làm cấu hình chủ đạo vận hành quanh năm.
Lý do: Đây là mô hình kinh tế bền vững nhất. Thay vì cào bằng chi phí ("One-size-fits-all"), ta chi tiền thông minh dựa trên rủi ro của từng Intent. Ta sẵn sàng trả tiền cho model đắt (GPT-4o) + Web Search cho mục Visa/Policy vì sai sót ở đây sẽ làm khách lỡ chuyến bay, gây khủng hoảng truyền thông. Ngược lại, với hỏi đáp Guide thông thường, model tầm trung (Flash) kết hợp RAG nội bộ là thừa sức tạo ra trải nghiệm mượt mà.

### Câu 2 — So với human baseline $0.50/conv → tiết kiệm bao nhiêu? Có đắt hơn human ở chỗ nào không?

Smart Mix tiết kiệm ~95.7% ở mùa thấp điểm ($191 vs $4,500) và ~94.8% ở mùa cao điểm ($921 vs $18,000).
Tuy nhiên, cần làm rõ: AI **không thay thế** human trong việc chốt sale cuối cùng hay xử lý khiếu nại gay gắt. AI đang giải quyết bài toán "hứng phễu 24/7", lọc khách rác, và trả lời tức thời đa ngôn ngữ — những việc mà nếu bắt nhân viên làm sẽ gây quá tải và burn-out. Chi phí thực tế vẫn phải cộng thêm lương cho đội ngũ Sales/CS để xử lý 45% lượng hand-off ở mùa cao điểm.

### Câu 3 — Khi nào nên upgrade / downgrade config?

* **Nên upgrade (Chuyển 1 phần traffic sang Premium):** Khi bước vào mùa cao điểm, nếu dữ liệu CRM cho thấy khách có xu hướng chốt tour thiết kế riêng (Custom Tour) > $2,000/booking, ta có thể cài rule: Nếu hệ thống nhận diện IP từ các nước phát triển (Mỹ/Úc/Hàn) hoặc khách bắt đầu nhắc đến các từ khóa VIP/Private, tự động route phiên chat đó sang Config Premium để chăm sóc tận răng.
* **Nên downgrade (Chuyển sang Balanced Desk hoặc Budget):** Khi ở mùa thấp điểm, tỷ lệ chuyển đổi thành booking thấp, doanh thu trên mỗi đầu khách giảm, ta có thể hạ toàn bộ Response Model xuống Gemini Flash và tắt Web Search để siết chặt biên lợi nhuận.

### Câu 4 — Rủi ro lớn nhất của config được chọn?

Rủi ro lớn nhất của Smart Mix nằm ở **"Nút thắt cổ chai" Intent Classifier**. Nếu model phân loại (GPT-4o-mini) nhận diện sai ý định (Ví dụ: khách hỏi về rắc rối E-Visa nhưng bot lại xếp vào intent "Điểm đến"), hệ thống sẽ ném khách vào model rẻ và không bật Web Search, dẫn đến tư vấn luật sai.
*Mitigation plan (Kế hoạch giảm thiểu):* Theo dõi chặt chẽ Confidence Score (độ tự tin) của bộ phân loại. Nếu điểm dưới 0.85, yêu cầu bot tự động hỏi lại xác nhận "Có phải bạn đang hỏi về Thủ tục Visa không?" trước khi kích hoạt quy trình trả lời.

---

## Final answer — Recommendation in 1 paragraph

"Dựa trên phân tích chi phí và hành vi khách hàng, nhóm đề xuất triển khai cấu hình **Smart Mix (Hybrid Router)** làm giải pháp lõi. Mô hình này giúp tiết kiệm 95% chi phí so với nhân sự con người ở cả hai mùa, với tổng chi phí API duy trì ở mức an toàn (< $1,000/tháng ngay cả khi peak-season). Bằng cách phân bổ tài nguyên theo rủi ro Intent — dùng model mạnh và Web Search cho Visa/Pháp lý, dùng model tiết kiệm cho hỏi đáp thông thường — chúng ta đảm bảo được độ chính xác tuyệt đối ở các điểm chạm nhạy cảm, trong khi vẫn tối ưu được chi phí vận hành. Đặc biệt, quy trình hand-off 100% các ca Booking và Khiếu nại sang nhân viên thật đảm bảo AI không can thiệp làm hỏng tỷ lệ chuyển đổi doanh thu cốt lõi của công ty."

---

## Chuẩn bị Present (5 phút)

Chia 5 phút thành 5 nhịp. 1 người trong nhóm chính phụ trách 1 nhịp. Người còn lại trả lời Q&A.

### Nhịp 0:00 – 0:30 — Base flow + 3 knobs đã chọn

**Ai trình bày:** Chi
**Nói gì:**
Chào mọi người, để xây dựng một chatbot du lịch bền vững, nhóm chúng tôi đánh giá chi phí dựa trên 3 trụ cột (Knobs): Model Tier (đắt/rẻ), Web Search (bật/tắt để lấy real-time info) và Độ sâu lịch sử trò chuyện (History).

### Nhịp 0:30 – 1:00 — Config overview

**Ai trình bày:** Chi
**Nói gì:**
Chúng tôi đã test 3 phương án: Config 1 (Budget FAQ - ép chi phí cực đoan), Config 2 (Premium Concierge - đắt xắt ra miếng cho khách VIP), và Config 3 (Smart Mix - phân luồng thông minh dựa trên rủi ro câu hỏi).

### Nhịp 1:00 – 2:00 — Cost comparison

**Ai trình bày:** Chi
**Nói gì:**
Nhìn vào bảng so sánh, nếu chạy bạo lực bằng Premium, mùa cao điểm sẽ ngốn của công ty gần $2,500/tháng API. Dù vẫn rẻ hơn đội ngũ human ($18,000), nhưng tỷ suất lợi nhuận mỏng. Với Smart Mix, chi phí rớt xuống chỉ còn $921/tháng (giảm ~60% so với Premium) mà vẫn đảm bảo an toàn thông tin ở các luồng quan trọng như Visa.

### Nhịp 2:00 – 3:00 — Key insight

**Ai trình bày:** My
**Nói gì:**
Insight đắt giá nhất nhóm rút ra là: Đừng tốn tiền LLM cho mọi thứ. Ở mùa cao điểm, lượng khách hỏi Booking và Phàn nàn chiếm tới 45%. Việc cấu hình hệ thống "bắt" các intent này và đá thẳng cho Sales/CS Agent (Cost API = 0) chính là mấu chốt giúp hóa đơn API của hệ thống không bị phình to tuyến tính theo lượng traffic.

### Nhịp 3:00 – 4:30 — Recommendation + justification

**Ai trình bày:** My
**Nói gì:**
[Đọc paragraph "Final answer" ở trên, kết hợp ngôn ngữ cơ thể nhấn mạnh các con số 95% và phân bổ rủi ro]

### Nhịp 4:30 – 5:00 — Hardest question prep

**Nhóm dự đoán câu hỏi khó nhất sẽ bị hỏi là gì?**
"Nhóm có Config 4 (Balanced Desk) chạy full Gemini Flash giá chỉ $289 ở mùa cao điểm, tiết kiệm hơn Smart Mix ($921) gấp 3 lần. Tại sao không chọn cái rẻ nhất mà chất lượng vẫn ổn (Mid)?"

**Câu trả lời sẵn:**
"Vì biên độ rủi ro ngành du lịch quá lớn. Nếu Visa Policy thay đổi mà RAG chưa cập nhật, Gemini Flash (không bật Web Search) sẽ tự tin trả lời sai. Chỉ cần 1 khách hàng Hàn Quốc lỡ chuyến bay vì sai Visa, công ty sẽ mất booking vài ngàn đô và hứng chịu khủng hoảng truyền thông. Khoản tiền tiết kiệm $600/tháng không đáng để đánh đổi rủi ro vận hành khổng lồ này."

---

## Q&A — 2 phút sau khi present xong

Sẵn sàng cho 1 câu từ class + 1 câu từ instructor. Không cần lo lắng — nếu chưa biết câu trả lời, nói "đây là điểm nhóm chưa nghĩ đến — sẽ tính lại sau buổi".

**3 câu instructor thường hỏi**:

1. *"Knob nào ảnh hưởng cost nhiều nhất trong config của nhóm? Tại sao?"*
* **Trả lời:** Model Tier. Bước nhảy giá giữa dòng Mini/Flash lên dòng Premium (GPT-4o/Claude 3.5 Sonnet) lên tới 20-30 lần. Do đó, việc dùng đúng Model cho đúng việc quyết định sự sống còn của ngân sách.


2. *"Nếu provider tăng giá API ×2 → config của nhóm còn sống được không?"*
* **Trả lời:** Chắc chắn sống tốt. Hiện Smart Mix chỉ tốn < $1,000 ở đỉnh điểm. Nếu x2 lên $2,000 thì nó vẫn chỉ bằng ~11% so với chi phí thuê nhân sự ($18,000). Biên độ an toàn của Smart Mix là cực kỳ rộng.


3. *"So với nhóm X (vừa present trước) — tại sao nhóm bạn chọn khác?"*
* **Trả lời:** Tùy theo nhóm trước nói gì, nhưng Key Defense của nhóm bạn là: "Chúng tôi không chọn 1 model duy nhất vì rủi ro các Intent là khác nhau. Chúng tôi tối ưu hóa dựa trên Risk-Based Routing chứ không tối ưu mù quáng vào token rẻ nhất."

---