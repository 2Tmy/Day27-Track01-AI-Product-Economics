# 01 · Base Flow + Chốt 3 Knobs

> **Mục tiêu**: Hiểu chatbot hoạt động ra sao ở mức base (không chọn config gì) — và xác định 3 knobs nhóm sẽ tweak ở các bước sau.
>
> **Thời gian**: 7 phút (trong 15 phút phần Setup)

---

## Bước 1 — Đọc base flow trong cost reference card

Mở file `cost-reference-card.md` ở phần **2. Base Flow** — xem flow chatbot mặc định. Đây là cấu trúc mọi config sẽ build dựa trên.

Đọc xong, tự kiểm tra hiểu:

- Khi tourist gửi tin nhắn, AI làm gì đầu tiên?
- 5 intent dẫn đến 5 hành động khác nhau — hành động nào tốn LLM, hành động nào không?
- Sau khi route, AI ráp gì lại để generate response?

Nếu chưa hiểu → quay lại đọc lại 1 lần nữa. Đừng đi tiếp khi còn mơ hồ.

---

## Bước 2 — Vẽ lại flow theo cách hiểu của nhóm

Vẽ flow bằng ASCII đơn giản:

```text
Tourist sends message
        |
        v
+------------------------+
| Intent Classification  |
| Visa / Guide / Weather |
| Booking / Complaint    |
+-----------+------------+
            |
            v
  +---------+----------+-----------+-----------+
  |         |          |           |           |
  v         v          v           v           v
 Visa     Guide     Weather     Booking    Complaint
  |         |          |           |           |
  |         |          |           |           |
  v         v          v           v           v
RAG +    RAG only   Web search   Handoff    Escalate
optional             + RAG        to Sales   to Manager
web search                        ($0 LLM)   ($0 LLM)
  |         |          |
  +---------+----------+
            |
            v
+------------------------+
| Context Assembly       |
| System prompt          |
| + selected history     |
| + top 5 RAG chunks     |
| + web results if used  |
| + latest user message  |
+-----------+------------+
            |
            v
+------------------------+
| Response Generation    |
| Model answers tourist  |
+------------------------+
```

---

## Bước 3 — Xác định 3 Knobs

3 knobs là 3 quyết định thiết kế nhóm có thể tweak. Mỗi config nhóm thiết kế = 1 bộ chọn tại 3 knobs này.

Trước khi điền vào ô bên dưới, đọc nhanh mục **3. Decision Points** của `cost-reference-card.md`. Sau đó tự hỏi:

- Knob 1 — Model tier: model rẻ và model mạnh chênh bao nhiêu lần? Có thể mix theo intent không?
- Knob 2 — Web search: intent nào *cần* real-time? intent nào không cần?
- Knob 3 — History: 7 lượt chat cuối đắt hay rẻ? Cắt history có rủi ro gì?

### Knob 1 — Model tier

**Câu hỏi:** Chất lượng câu trả lời ở mức nào?

Options:

```text
□ Cheap        (Gemini Flash-Lite / DeepSeek V4 Flash / GPT-4o-mini)
□ Mid          (Gemini Flash / Claude Haiku 4.5)
□ Strong       (DeepSeek V4 Pro / Claude Sonnet 4.6)
□ Premium      (Claude Opus 4.7 / GPT-5.5)
□ Mix          (model khác nhau cho intent khác nhau — viết rõ)
```

**Câu hỏi gợi mở cho nhóm** (trả lời trước khi chọn):

- Mục tiêu chính là chi phí thấp hay chất lượng cao?
- Tourist hỏi câu phức tạp hay đơn giản hơn?
- Có nên dùng cheap cho phân loại + strong cho trả lời không?

```text
Nhóm không nên chọn 1 model mạnh cho mọi intent vì intent mix thực tế có 30% Guide,
30% Visa/Policy, 30% Booking, 10% Complaint. Booking và Complaint sẽ chuyển người,
nên không cần trả tiền generation cho các case đó. Guide thường là câu hỏi tư vấn,
KB/RAG đủ nhiều nên model cheap hoặc mid có thể xử lý tốt. Visa/Policy rủi ro cao
hơn vì sai chính sách có thể làm khách lỡ chuyến, nên nên dùng model mạnh hơn hoặc
bật web search cho intent này.

Hướng hợp lý nhất: dùng cheap/mid cho Guide, dùng strong cho Visa/Policy nếu cần
độ tin cậy cao, và vẫn dùng intent classification thật tốt để chuyển Booking +
Complaint sang người thật.
```

### Knob 2 — Web search

**Câu hỏi:** Có cần thông tin real-time không?

Options:

```text
□ OFF              (chỉ dùng RAG — knowledge base có sẵn)
□ ON selective    (bật cho 1–2 intent cần real-time: visa, weather)
□ ON broad         (bật cho hầu hết intent)
```

**Câu hỏi gợi mở:**

- Visa policy đổi mỗi tháng — RAG có đủ không?
- Weather là thông tin real-time tự nhiên — không có lựa chọn khác đúng không?
- Web search tốn $0.005/call + 800 tokens — bật bừa có lợi không?

```text
Không nên bật web search cho tất cả vì mỗi query thêm khoảng $0.008 + 800 input
tokens và làm chậm phản hồi. Guide/Destination có thể dùng KB có sẵn cho địa điểm,
food, transport, safety. Booking và Complaint chuyển người nên không cần web.

Web search nên bật selective cho Visa/Policy và Weather/Event. Visa có thể thay đổi
theo tháng, e-visa hoặc emergency clearance cần thông tin mới. Weather/Event là
real-time nên nếu có intent này thì gần như bắt buộc phải search. Với intent mix của
nhóm hiện tại Weather = 0%, nhưng thiết kế vẫn nên hỗ trợ vì tourist thật sẽ hỏi.
```

### Knob 3 — History management

**Câu hỏi:** Chatbot cần nhớ bao nhiêu context của conversation?

Options:

```text
□ Last 3 turns        (nhẹ nhất, dễ quên)
□ Last 5 turns        (cân bằng)
□ Full history        (nhớ tất cả, đắt nhất ở conv dài)
□ Summarize every 5   (nâng cao — cần 1 LLM call phụ để tóm tắt)
```

**Câu hỏi gợi mở:**

- Tourist hay nói "tôi đã nói budget là $500 ở turn 1" rồi turn 7 hỏi gợi ý — nếu quên thì sao?
- Scenario A trung bình 4 lượt → full history có tốn nhiều không?
- Scenario B trung bình 7 lượt → mỗi turn thêm 260 tokens — tổng thêm bao nhiêu?

```text
Conversation trung bình của nhóm là 4 turns, gần Scenario A, nên full history chưa
quá đắt ở case ngắn. Tuy nhiên Scenario B có 7 turns, mỗi prior turn thêm khoảng
260 tokens, nên full history sẽ tăng cost dần theo độ dài chat.

Last 3 turns rẻ nhưng dễ quên các ràng buộc quan trọng như budget, allergy, child
seat, arrival time, hoặc business invoice. Last 5 turns là lựa chọn cân bằng cho
tourist context-rich. Với khách VIP hoặc itinerary phức tạp, có thể cân nhắc full
history hoặc summarize every 5, nhưng summary thêm 1 LLM call phụ nên chưa cần cho
base configs đầu tiên.
```

---

## Bước 4 — Sơ bộ nhóm muốn thử những combo nào?

Chưa cần quyết định cuối cùng. Chỉ cần phác thảo: nhóm dự định thử ít nhất 3 combo khác nhau. Càng khác nhau, càng dễ thấy tradeoff.

**Combo 1 (định hướng cheap)**:

```text
Model: Cheap - GPT-4o-mini hoặc Gemini Flash-Lite
Web: OFF
History: Last 3 turns
(đặt tên dự kiến: Budget FAQ)
```

**Combo 2 (định hướng premium)**:

```text
Model: Premium - GPT-5.5 hoặc Claude Opus 4.7
Web: ON broad cho Visa/Policy, Guide, Weather/Event
History: Full history
(đặt tên dự kiến: Premium Concierge)
```

**Combo 3 (định hướng balanced / smart mix)**:

```text
Model: Mix - cheap/mid cho Guide, strong cho Visa/Policy, no generation cho Booking/Complaint
Web: ON selective cho Visa/Policy và Weather/Event
History: Last 5 turns
(đặt tên dự kiến: Smart Mix)
```

**Combo 4** (optional — nếu nhóm có ý tưởng khác):

```text
Model: Mid - Gemini Flash hoặc Claude Haiku 4.5
Web: ON selective cho Weather/Event only, Visa dùng RAG trước
History: Last 5 turns
(đặt tên dự kiến: Balanced Travel Desk)
```

---

## Bảng kiểm trước khi sang file tiếp theo

- [x] Đã vẽ flow base có đủ 4 bước (Intent → Route → Context → Response)
- [x] Hiểu Booking + Khiếu nại = $0 LLM cost (chuyển con người)
- [x] Đã phác thảo ≥3 combo khác nhau (chưa cần chi tiết)
- [x] Nhóm đồng thuận về hướng đi mỗi combo

Xong → 10:25 chuyển sang **Main phase**. Mở `02-config-design.md`.
