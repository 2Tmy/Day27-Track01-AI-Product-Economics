# 04 · Comparison Table — Bảng so sánh đầy đủ

> **Mục tiêu**: Tổng hợp tất cả số đã tính ở `03-cost-calculation.md` thành 1 bảng so sánh duy nhất — đây là artifact chính nhóm sẽ present.
>
> **Thời gian**: 10 phút (đầu phần Final)

---

## Vì sao có bảng so sánh?

Khi sếp hỏi "Nên deploy config nào?", bạn cần đặt lên bàn **1 bảng** thay vì đọc 3 báo cáo riêng. Bảng so sánh đầy đủ cho phép so sánh thẳng từng dòng, dễ nhìn ra tradeoff.

---

## Bảng chính

|  | Config 1 | Config 2 | Config 3 | (Config 4) |
| --- | --- | --- | --- | --- |
| **Tên** | **Budget FAQ** | **Premium Concierge** | **Smart Mix** | **Balanced Desk** |
| **① Model** | Cheap (GPT-4o-mini) | Premium (GPT-4o) | Mix (GPT-4o & Flash) | Mid (Gemini Flash) |
| **② Web search** | OFF | ON broad | ON selective (Visa/Wea) | ON selective (Wea) |
| **③ History** | Last 3 | Full history | Last 5 | Last 5 |
| **Intent classifier** | LLM (Mini) | LLM (Mini) | LLM (Mini) | LLM (Mini) |
| **Cost / conv (Scenario A — 4 turns)** | $0.001622 | $0.057106 | $0.021313 | $0.005519 |
| **Cost / conv (Scenario B — 7 turns)** | $0.001943 | $0.069179 | $0.025604 | $0.008039 |
| **Monthly A** (300 conv/day × 30) | $14.60 | $513.96 | $191.82 | $49.67 |
| **Monthly B** (1,200 conv/day × 30) | $69.94 | $2,490.44 | $921.76 | $289.40 |
| **vs human $4,500/mo (A)** | rẻ 308.3× | rẻ 8.8× | rẻ 23.5× | rẻ 90.6× |
| **vs human $18,000/mo (B)** | rẻ 257.4× | rẻ 7.2× | rẻ 19.5× | rẻ 62.2× |
| **Savings % (A)** | 99.68% | 88.58% | 95.74% | 98.90% |
| **Savings % (B)** | 99.61% | 86.16% | 94.88% | 98.39% |
| **Quality estimate** | Low-Med | High | High | Medium |
| **Speed estimate** | High | Low-Med | Medium | Medium-High |
| **Điểm yếu chính** | Dễ sai info (Visa/Weather) và quên context khách | Chi phí rất đắt đỏ, gọi web search nhiều gây trễ (latency) | Lệ thuộc hoàn toàn vào độ chính xác của bộ Router | Rủi ro sai luật Visa nếu database (RAG) chưa kịp update |
| **Best for** (khi nào nên dùng) | Khách phổ thông, cần reply nhanh, tối ưu chi phí | Khách VIP, tour thiết kế riêng, cần trải nghiệm 5 sao | "Sweet spot" cân bằng, kinh tế bền vững cho công ty | Scale up nhanh với chất lượng ổn định nhưng giá rẻ |

---

## Quan sát nhanh từ bảng

Trước khi sang file recommendation, trả lời 4 câu — đây là material để present:

### Câu 1 — Config rẻ nhất là gì? Đắt nhất là gì?

```text
Rẻ nhất: Budget FAQ (Config 1) — monthly B = $69.94
Đắt nhất: Premium Concierge (Config 2) — monthly B = $2,490.44
Chênh: ~35.6× lần
```

### Câu 2 — Knob nào ảnh hưởng cost nhiều nhất?

So sánh các config khác nhau ở knob nào, chênh bao nhiêu. Thường: model tier > history > web search.

```text
Model Tier là yếu tố thổi bùng chi phí lớn nhất. Việc đổi từ Cheap (GPT-4o-mini) sang Premium (GPT-4o) đẩy giá token lên gấp 20–25 lần. Đứng thứ hai là Web Search vì không chỉ cộng thêm token (800 context) mà còn phát sinh fixed cost ($0.005/API call). Yếu tố History (Full vs Last 3) ảnh hưởng lũy tiến ở các lượt chat sau, nhưng tác động không sốc bằng Model Tier.
```

### Câu 3 — Tại sao Scenario B không đắt ×4 lần Scenario A?

Volume Scenario B = ×4 lần Scenario A. Turns dài hơn (7 vs 4 = ×1.75). Mong đợi monthly B ≈ A × 7. Thực tế có thể thấp hơn vì sao?

Trước khi viết, nghĩ: intent mix Scenario B có gì khác? Booking + Complaint = $0 LLM ở scenario B là bao nhiêu %?

```text
Volume Scenario B gấp 4 lần, số lượt chat dài hơn (7 vs 4), theo lý thuyết cost phải gấp khoảng ~7 lần. Tuy nhiên, thực tế Monthly B của các model chỉ gấp khoảng 4.8x. Lý do "cứu cánh" là vì tỷ lệ phân bổ Intent: Trong mùa cao điểm (B), lượng chat mang tính chất "Booking" và "Khiếu nại" vọt lên chiếm tới 45% (so với 15% ở A). Những intent này được hand-off ngay sang nhân viên (Cost Generate = $0), giúp gánh lại đáng kể chi phí tổng thể.
```

### Câu 4 — Có config nào AI đắt hơn human không?

So sánh monthly từng config với human baseline ($4,500 cho A, $18,000 cho B). Nếu AI rẻ hơn → savings %. Nếu đắt hơn → cần justify.

```text
Cả 4 Config đều rẻ hơn Human Baseline. Ngay cả Config đắt nhất (Premium Concierge) vẫn tiết kiệm ~86% chi phí (từ $18,000 xuống còn $2,490 ở Scenario B). Tuy nhiên, vì sự chênh lệch giữa các Config quá lớn (Budget chỉ tốn $69), việc bỏ ra gần $2,500 cho Premium phải được justify bằng: Khả năng upsell tour xịn, hỗ trợ 24/7 các ca lệch múi giờ (Mỹ/Úc), và trải nghiệm VIP không thể có được với nhân viên trực ca đêm.
```

---

## Bảng kiểm trước khi sang file tiếp theo

* [x] Bảng đầy đủ — không còn ô trống
* [x] Đã có 4 câu trả lời cho 4 quan sát ở trên
* [x] Nhóm đồng thuận về số trong bảng (đã sanity check)

Xong → mở `05-recommendation.md` để viết recommendation cuối + chuẩn bị present.