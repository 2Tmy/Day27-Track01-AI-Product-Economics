# 00 · User Journey Simulation — Đóng vai Tourist

> **Mục tiêu**: Trước khi tính chi phí, nhóm phải hình dung được khách hàng thật sự hỏi gì, hỏi như thế nào, và 1 conversation thực tế trông ra sao.
> **Thời gian**: 8 phút (trong 15 phút phần Setup)

---

## Tại sao phải làm bước này?

Nếu nhóm bắt đầu tính cost mà chưa biết tourist hỏi gì → mọi con số chỉ là lý thuyết. Bước này buộc nhóm "chạm" sản phẩm trước khi mở Excel.

---

## Bước 1 — Mỗi người đóng vai 1 tourist (4 phút)

Tưởng tượng mình là 1 khách du lịch nước ngoài đang plan trip Việt Nam. Bạn vừa mở website công ty du lịch, thấy có chatbot ở góc màn hình. Bạn sẽ hỏi gì?

Trước khi viết, tự hỏi:

* Mình từ đâu đến? Mỹ, Anh, Hàn, Nhật, Úc?
* Đi 1 mình hay đi nhóm? Budget khoảng bao nhiêu?
* Đã biết gì về Việt Nam? Lần đầu đến hay đã đến rồi?
* Mình lo lắng điều gì nhất? (visa, an toàn, ngôn ngữ, thời tiết, ẩm thực, lừa đảo...)

Viết **5–7 câu hỏi bằng tiếng Anh** mình sẽ thật sự gửi cho chatbot. Viết câu hỏi tự nhiên, đúng giọng tourist — không phải đặt câu hỏi "nghe có vẻ technical".

→ Mỗi người viết vào ô dưới:

### Tourist #1 (Khách Balo/Solo từ Úc)

```text
1. "Is it easy to pay with Apple Pay/card over there, or should I be carrying a lot of cash in VND?"
2. "I'm travelling solo. Any recommendations for hostels in Hanoi that have a good social vibe but aren't crazy party places?"
3. "Do I legally need an International Driving Permit to rent a scooter in Da Lat? I keep hearing mixed things about the local police."
4. "Where can I find the best local food, maybe some authentic Hu Tieu nearby?"
5. "Hey, what's the safest way to get from Tan Son Nhat airport to District 1 at 2 AM? I read a lot about fake taxi scams."
```

### Tourist #2 (Khách gia đình từ Mỹ)

```text
1. "We're a family of four. For the Ha Long Bay cruise, do you guarantee connecting rooms, and are the boat balconies child-safe?"
2. "My 6-year-old has a severe peanut allergy. How easy is it to navigate your street food tours with this strict restriction?"
3. "Our flight from LAX was delayed and we land at 2 AM. Will the driver still be there, and do you provide child car seats?"
4. "Is Hoi An Ancient Town stroller-friendly, or is the pavement too bumpy? Should we bring a baby carrier instead?"
5. "If my kid gets sick during the trip, does your package include support for finding an English-speaking pediatrician or clinic?"
```

### Tourist #3 (Khách công tác/ngắn ngày từ Hàn Quốc)

```text
1. "I need a red invoice (VAT receipt) for all the tour and car bookings to claim with my company. Can you provide this?"
2. "I fly in this Friday but my e-visa is still 'in processing'. Do you offer VIP fast-track or emergency visa clearance services?"
3. "I have a free afternoon after meetings in District 7. What's the fastest way to squeeze in a good massage and a high-end local dinner nearby?"
4. "Can you arrange a private 7-seater with a Korean-speaking driver for a half-day trip to a golf course in Dong Nai?"
5. "The Wi-Fi in the hotel you booked drops constantly and I have Zoom calls. I need to be moved to a premium business suite or another hotel right now."
```

---

## Bước 2 — Gom lại và phân loại (4 phút)

Cả nhóm chụm vào, gom tất cả câu hỏi lại. Trước khi điền bảng, thảo luận 1 phút:

* Có câu hỏi nào lặp lại giữa các tourist không?
* Có chủ đề nào không ai trong nhóm nghĩ tới ban đầu nhưng quan trọng?
* Câu nào chatbot có thể trả lời được? Câu nào cần chuyển sang nhân viên thật?

5 intent có sẵn (tham khảo `cost-reference-card.md` mục 2):

* **Visa/Policy** — chính sách, thủ tục nhập cảnh
* **Điểm đến/Guide** — gợi ý đi đâu, làm gì, ăn gì
* **Thời tiết/Sự kiện** — info real-time
* **Tour/Booking** — đặt vé, đặt tour, đặt phòng → chuyển sales
* **Khiếu nại** — phàn nàn → chuyển manager

Sau khi gom, điền bảng phân loại (Chọn lọc 10 câu tiêu biểu nhất từ 3 persona):

| # | Câu hỏi (1 dòng) | Intent thuộc loại nào | Cần bao nhiêu lượt chat để xong? | Bot trả lời hay chuyển người? |
| --- | --- | --- | --- | --- |
| 1 | "Safest way from Tan Son Nhat to D1 at 2 AM, avoiding scams?" | Điểm đến/Guide | 3 | ☑ Bot · □ Người |
| 2 | "Hostels in Hanoi: good social vibe but not crazy party?" | Điểm đến/Guide | 4 | ☑ Bot · □ Người |
| 3 | "Need an IDP to rent a scooter in Da Lat? Police issues?" | Visa/Policy | 3 | ☑ Bot · □ Người |
| 4 | "Ha Long cruise for 4: guarantee connecting rooms & child-safe?" | Tour/Booking | 5 | □ Bot · ☑ Người |
| 5 | "6yo has severe peanut allergy. Safe for street food tours?" | Điểm đến/Guide | 4 | ☑ Bot · □ Người |
| 6 | "Flight delayed to 2 AM. Driver still there? Child car seats?" | Tour/Booking | 4 | □ Bot · ☑ Người |
| 7 | "Need a red invoice (VAT receipt) for company claims." | Visa/Policy | 3 | ☑ Bot · □ Người |
| 8 | "E-visa 'in processing'. VIP fast-track/emergency clearance?" | Visa/Policy | 4 | □ Bot · ☑ Người |
| 9 | "Private 7-seater + Korean-speaking driver to Dong Nai golf?" | Tour/Booking | 5 | □ Bot · ☑ Người |
| 10 | "Wi-Fi drops, have Zoom calls. Move to premium suite NOW." | Khiếu nại | 5 | □ Bot · ☑ Người |

---

## Bước 3 — Rút insight cho nhóm (cuối phần Setup)

Trả lời nhanh 4 câu — sẽ dùng lại ở các bước sau:

**Tổng số câu hỏi nhóm gom được**:

```text
15 câu (Đã lọc top 10 câu mang tính đại diện cao nhất vào bảng)
```

**Phân bố intent thực tế của nhóm** (% mỗi intent):

```text
Guide: 30%
Tour/Booking: 30%
Visa/Policy: 30%
Khiếu nại: 10%
Thời tiết/Sự kiện: 0%
```

**Số lượt chat trung bình để xong 1 chủ đề**:

```text
Trung bình 4.0 lượt/conversation (40 lượt / 10 câu). Các câu hỏi về Booking, sự cố khẩn cấp (trễ chuyến, đổi phòng) tốn nhiều lượt hơn (4-5 lượt) so với hỏi thông tin điểm đến.
```

**Đối chiếu với đề bài** (Scenario A = 4 lượt, Scenario B = 7 lượt):

```text
Trùng khớp hoàn hảo với Scenario A (4 lượt). Điều này cho thấy với các tác vụ tư vấn thông thường và phân luồng, thiết kế cơ bản là đủ. Scenario B (7 lượt) sẽ chỉ cần thiết cho các phiên chat mà khách hàng muốn tự chốt sale booking phức tạp từ đầu đến cuối qua bot mà không chuyển người.
```

**Insight bất ngờ — điều gì nhóm chỉ hiểu sau khi đóng vai?**

```text
Khách hàng hiếm khi hỏi một thông tin đơn lẻ, họ luôn đính kèm bối cảnh cá nhân và các điều kiện ràng buộc (đi 1 mình, trẻ bị dị ứng, cần xuất VAT, đang gấp). Hệ thống AI cần khả năng trích xuất dữ liệu (entity extraction) cực kỳ tốt. Để vận hành trơn tru, kiến trúc chatbot nên áp dụng pattern dạng Supervisor/Worker — trong đó Supervisor agent sẽ nhận diện các case khẩn cấp (khiếu nại, delay chuyến bay) để định tuyến (route) trực tiếp cho nhân viên, thay vì để bot lúng túng tự giải quyết.
```

---

## Bảng kiểm trước khi sang file tiếp theo

* [x] Mỗi người trong nhóm đã viết ≥5 câu hỏi tourist
* [x] Đã gom + phân loại intent cho ≥10 câu (bảng trên)
* [x] Đã có phân bố intent % của nhóm (so với đề bài)
* [x] Có ít nhất 1 insight về cách tourist thật sự dùng chatbot

Xong → mở `01-base-flow.md`.