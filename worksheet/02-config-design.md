# 02 · Configuration Design — Đặt tên + Chốt knobs cho ≥3 Configs

> **Mục tiêu**: Biến phác thảo ở `01-base-flow.md` thành ≥3 configurations chi tiết, mỗi config có tên + 3 knobs đã chốt + lý do chọn.
>
> **Thời gian**: 15 phút (đầu phần Main, trước khi tính cost)

---

## Tại sao đặt tên + viết lý do?

Khi present, nhóm sẽ nói "Config 1, Config 2, Config 3" → người nghe sẽ chán ngay. Đặt tên gợi mở (Budget Bot, Premium Concierge, Smart Mix...) giúp memorable + cho thấy nhóm hiểu rõ tradeoff. Viết lý do giúp nhóm tự kiểm tra: "Mình chọn config này vì lý do gì? Có justify được không?"

---

## Config 1

**Tên config**: **Budget FAQ (Lean Mode)**

### 3 Knobs

**① Model tier**:

```text
Response model: Cheap (GPT-4o-mini) → giá $0.150 / $0.600 per 1M tokens (input/output)
Classifier model: Cheap (GPT-4o-mini) → giá $0.150 / $0.600 per 1M tokens
```

**② Web search**:

```text
☑ OFF
□ ON selective
□ ON broad
```

**③ History management**:

```text
☑ Last 3 turns
□ Last 5 turns
□ Full
□ Summarize every ___ turns
```

### Lý do nhóm chọn config này

```text
1. Phục vụ cho các kịch bản lưu lượng cao (spike traffic) hoặc ngoài giờ hành chính (Night Mode) khi khách hàng chủ yếu hỏi những thông tin cơ bản, lặp đi lặp lại.
2. Tối ưu hóa tuyệt đối về mặt chi phí. Mọi thông tin đều dựa vào Knowledge Base (RAG) nội bộ của công ty để trả lời, không phát sinh chi phí gọi tool bên ngoài.
3. Chấp nhận hy sinh độ sâu của ngữ cảnh (chỉ nhớ 3 lượt) vì phần lớn khách hàng phổ thông có xu hướng hỏi nhanh - đáp gọn.
```

### Rủi ro lớn nhất của config này

```text
Thông tin về Visa có thể bị outdated nếu đại sứ quán vừa đổi luật mà công ty chưa kịp cập nhật RAG, và AI có thể quên mất các yêu cầu đặc biệt của khách (như dị ứng, đi cùng trẻ em) nếu cuộc hội thoại kéo dài quá 3 lượt.
```

---

## Config 2

**Tên config**: **Premium Concierge (VIP Mode)**

### 3 Knobs

**① Model tier**:

```text
Response model: Premium (Claude 3.5 Sonnet / GPT-4o) → giá $3.00 / $15.00 per 1M tokens
Classifier model: Cheap (GPT-4o-mini) → giá $0.150 / $0.600 per 1M tokens (vẫn dùng cheap cho phân loại để tránh lãng phí)
```

**② Web search**:

```text
□ OFF
□ ON selective 
☑ ON broad (Bật cho mọi intent ngoại trừ Booking và Complaint)
```

**③ History management**:

```text
□ Last 3
□ Last 5
☑ Full history
□ Summarize every ___ turns
```

### Lý do nhóm chọn config này

```text
1. Phục vụ riêng cho tập khách hàng VIP, khách gia đình Mỹ/Hàn Quốc có nhu cầu đặt tour thiết kế riêng, itinerary phức tạp và mang lại biên lợi nhuận cực cao.
2. Với tệp khách này, trải nghiệm mượt mà và sự chính xác tuyệt đối là ưu tiên số 1 (không ngại chi trả). Model mạnh + Web search broad giúp AI có thể tư vấn như một hướng dẫn viên bản địa thực thụ (ví dụ: quán ăn nào đang mở cửa tối nay).
3. Ghi nhớ toàn bộ ngữ cảnh (Full history) để khách không bao giờ phải nhắc lại yêu cầu (ví dụ: luôn nhớ khách có trẻ em 6 tuổi bị dị ứng đậu phộng xuyên suốt 20 lượt chat).
```

### Rủi ro lớn nhất của config này

```text
Chi phí (Cost/Conversation) cực kỳ đắt đỏ. Khả năng cao không có ROI (tỷ suất hoàn vốn) nếu áp dụng đại trà cho các khách hàng chỉ lên web để "hỏi dạo" mà không chốt tour.
```

---

## Config 3

**Tên config**: **Smart Mix (Hybrid Router)**

### 3 Knobs

**① Model tier**:

```text
Response model: Mix 
  - Visa/Policy: Strong (GPT-4o) → $3.00 / $15.00 per 1M tokens (Cần suy luận luật lệ phức tạp)
  - Guide/Weather: Mid (Gemini 1.5 Flash) → $0.35 / $1.05 per 1M tokens (Tư vấn nhanh, đủ tốt)
  - Booking/Complaint: Không tốn phí generate (Route thẳng cho con người)
Classifier model: Cheap (GPT-4o-mini) → $0.15 / $0.60 per 1M tokens
```

**② Web search**:

```text
□ OFF
☑ ON selective — bật cho intent: Visa/Policy và Thời tiết/Sự kiện.
□ ON broad
```

**③ History management**:

```text
□ Last 3
☑ Last 5 turns
□ Full
□ Summarize every ___ turns
```

### Lý do nhóm chọn config này

```text
1. Đây là cấu hình "Sweet Spot" (điểm cân bằng lý tưởng nhất) cho một mô hình kinh tế bền vững. Thay vì dùng một model cho mọi thứ, ta phân bổ ngân sách AI theo rủi ro của từng Intent.
2. Vấn đề Visa/Pháp lý có rủi ro cao (sai lệch khiến khách lỡ chuyến bay) → Dùng Model xịn và cho phép search Web để update luật mới nhất. 
3. Vấn đề tư vấn Điểm đến (Guide) chỉ cần RAG nội bộ và Model tầm trung là đủ đáp ứng 90% nhu cầu. Giới hạn nhớ 5 lượt là đủ để cover một chủ đề trước khi khách chuyển sang Intent mới.
```

### Rủi ro lớn nhất của config này

```text
Hệ thống Router (phân loại intent ở bước đầu) mang trọng trách rất lớn. Nếu Router phân loại sai (ví dụ: khách hỏi Visa nhưng AI phân loại nhầm thành Guide), khách sẽ bị đẩy vào model rẻ và không được update web real-time, dẫn đến sai lệch nghiêm trọng.
```

---

## Config 4 (Optional)

**Tên config**: **Balanced Travel Desk**

### 3 Knobs

```text
Model: Mid (Gemini 1.5 Flash cho mọi Intent)    Web: ON selective (Weather/Event only)    History: Last 5 turns
```

### Lý do

```text
Lựa chọn nâng cấp từ Budget Bot. Giá của Model Mid (Flash) hiện nay đã rất rẻ, đủ sức đảm đương chất lượng sinh văn bản tốt hơn hẳn dòng Mini. Chỉ bật Web cho thời tiết vì Visa có thể được giải quyết bằng việc update RAG nội bộ định kỳ hàng tuần thay vì search web liên tục (giảm cost API search).
```

---

## Bảng kiểm trước khi tính cost

* [x] ≥3 configs đã đặt tên (không chỉ "Config 1/2/3")
* [x] Mỗi config đã chốt rõ 3 knobs (không còn ô trống)
* [x] Mỗi config có ≥2 câu lý do
* [x] 3 configs đủ khác biệt — không phải chỉ đổi mỗi 1 knob nhỏ
* [x] Nhóm đồng thuận đây là 3 configs đáng so sánh

Xong → mở `03-cost-calculation.md` để bắt đầu tính cost.