# 01 — Problem Scan & Quick Assess (Vin Smart Future)

> **Lưu ý về số liệu:** Các con số thời gian, khối lượng và tổn thất trong file này là **giả định ước tính** (kèm cách tính) để phục vụ scoping, chưa phải số liệu vận hành thật của Vingroup. Cần xác thực lại với bộ phận vận hành trước khi triển khai.

---

# 🔍 Phase 1 — SCAN

Sử dụng **4 Lenses** (Lặp lại / Tốn thời gian / AI có thể tốt hơn / Pain từ người khác) để quét vận hành của 3 công ty thành viên: **VinFast, Xanh SM, Vinhomes**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | AI có thể tốt hơn | Phân loại lỗi xe từ mô tả tiếng Việt tự do của khách (VD: *"kêu cụp cụp bánh trước khi qua gờ"*). Cố vấn dịch vụ mất ~9 phút/phiếu để diễn giải, ~20% phiếu bị xếp sai nhóm lỗi → kỹ thuật viên chẩn đoán lại. |
| 2 | **VinFast** | Lặp lại | Đối chiếu hóa đơn sạc điện từ hàng trăm trạm đối tác với log phiên sạc nội bộ. Mỗi đối tác một định dạng file, tốn ~10 ngày công/tuần. *(Đánh giá sơ bộ: Rule-based/ETL phù hợp hơn LLM.)* |
| 3 | **Xanh SM** | Pain từ người khác | Xử lý khiếu nại cước phí/lộ trình (*"tài xế đi đường vòng"*). CSKH phải mở log GPS, so với lộ trình tối ưu và soạn phản hồi, ~11 phút/khiếu nại; mức hoàn tiền không nhất quán giữa các nhân viên. |
| 4 | **Vinhomes** | Tốn thời gian | Duyệt hồ sơ đăng ký thi công nội thất. Hồ sơ thiếu/sai bị trả về 2–3 vòng, mỗi vòng 2–3 ngày; cư dân chờ 7–10 ngày mới được thi công. |
| 5 | **Vinhomes** | Lặp lại | Đăng ký thẻ gửi xe và khách ra vào: lễ tân nhập tay ~5 phút/lượt, hay sai biển số. *(Đánh giá sơ bộ: số hóa form + Rule là đủ, không cần AI.)* |
| 6 | **Xanh SM** | Tốn thời gian | Tìm đồ thất lạc trên xe khi khách chỉ nhớ mơ hồ thời gian/địa điểm. CSKH mất ~15 phút/ca để xác định đúng chuyến và liên hệ tài xế. |
| 7 | **VinFast** | Tốn thời gian | Đọc thủ công hàng nghìn ghi chú phiếu bảo hành mỗi tuần để phát hiện lỗi hàng loạt theo linh kiện; lỗi thường bị phát hiện chậm 3–4 tuần. |

**Top 3 chọn cho Quick Assess:** #1 (VinFast), #3 (Xanh SM), #4 (Vinhomes) — mỗi công ty một bài, đều có nhu cầu xử lý ngôn ngữ thực sự và ranh giới vận hành rõ ràng.

**Lý do không chọn các bài còn lại:**
- **#2, #5:** Dữ liệu có cấu trúc, logic xác định → Rule-based rẻ và chính xác hơn LLM.
- **#6:** Giá trị thấp hơn, phần khó nhất (truy vấn chuyến) là bài toán rule/query.
- **#7:** Có giá trị lớn nhưng là phân tích offline, khó đo metric trong phạm vi lab.

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

## Card #1 — VinFast: Phân loại lỗi xe từ mô tả tiếng Việt

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Cố vấn dịch vụ mất nhiều thời gian diễn giải mô   │
│ tả lỗi tiếng Việt tự do của khách để xếp đúng nhóm lỗi và   │
│ đúng khoang sửa chữa.                                       │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Cố vấn dịch vụ xưởng (quá tải), kỹ     │
│ thuật viên (chẩn đoán lại), khách hàng (chờ lâu).           │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gọi hotline / nhắn app mô tả lỗi                 │
│   → 2. CSKH ghi chú tự do vào phiếu                         │
│   → 3. Cố vấn đọc ghi chú, đoán nhóm lỗi (phanh, treo,      │
│        điện, pin, phần mềm...)                              │
│   → 4. Xếp khoang sửa + kỹ thuật viên                       │
│   → 5. Kỹ thuật viên chẩn đoán lại từ đầu nếu xếp sai       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (⏱ ~9 phút/phiếu),  │
│ ~20% phiếu xếp sai → bước 5 tốn thêm ~1 giờ khoang sửa.     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3: LLM đọc mô tả,│
│ đề xuất nhóm lỗi + câu hỏi làm rõ + draft phiếu; cố vấn     │
│ duyệt. Từ khóa an toàn (phanh, lái, khói pin) → escalate.   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│  - Giảm thời gian phân loại từ ~9 phút ──> dưới 2 phút.     │
│  - Tỉ lệ xếp đúng nhóm lỗi từ ~80% ──> ≥ 90%.               │
│  - 100% mô tả có dấu hiệu an toàn được escalate cho người.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ (LLM Feature + Rule lọc từ khóa an toàn, HITL bắt buộc)     │
└─────────────────────────────────────────────────────────────┘
```

**Ước tính tổn thất:** 300 phiếu/ngày × 9 phút ≈ **45 giờ công cố vấn/ngày**; 300 × 20% × 1 giờ ≈ **60 giờ khoang sửa/ngày** bị lãng phí do xếp sai.

---

## Card #2 — Xanh SM: Xử lý khiếu nại cước phí & lộ trình

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: CSKH xử lý thủ công khiếu nại "tài xế đi đường    │
│ vòng / tính cước cao", phải tự đối chiếu log GPS và quyết   │
│ định mức hoàn tiền thiếu nhất quán.                         │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH (quá tải), khách hàng   │
│ (chờ phản hồi), tài xế (bị khiếu nại oan nếu xử lý sai).    │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gửi khiếu nại qua app/hotline                    │
│   → 2. CSKH mở log GPS + hóa đơn chuyến đi                  │
│   → 3. So sánh km thực tế với lộ trình tối ưu trên bản đồ   │
│   → 4. Quyết định mức hoàn tiền theo cảm tính/kinh nghiệm   │
│   → 5. Soạn và gửi phản hồi cho khách                       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3–5 (⏱ ~11 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 bằng Rule (tính│
│ % lệch lộ trình); bước 4–5 bằng LLM: tóm tắt bằng chứng,    │
│ đề xuất mức xử lý theo chính sách, draft phản hồi.          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│  - Giảm thời gian xử lý từ ~11 phút ──> dưới 3 phút.        │
│  - ≥ 95% đề xuất hoàn tiền khớp với chính sách (audit mẫu). │
│  - 0 trường hợp AI tự hoàn tiền không qua duyệt.            │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│ (Rule tính lệch lộ trình + LLM draft; hoàn > 100.000đ phải  │
│  có supervisor duyệt)                                       │
└─────────────────────────────────────────────────────────────┘
```

**Ước tính tổn thất:** 500 khiếu nại/ngày × 11 phút ≈ **90 giờ công CSKH/ngày**, cộng chi phí hoàn tiền sai chính sách và điểm hài lòng giảm.

---

## Card #3 — Vinhomes: Duyệt hồ sơ đăng ký thi công nội thất

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Hồ sơ đăng ký thi công nội thất của cư dân thường │
│ thiếu/sai, bị BQL trả về nhiều vòng khiến cư dân chờ lâu.   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên BQL tòa nhà (kiểm tra thủ    │
│ công), cư dân (chờ 7–10 ngày mới được thi công).            │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân nộp đơn + bản vẽ + thông tin nhà thầu           │
│   → 2. BQL kiểm tra đủ giấy tờ                              │
│   → 3. Đối chiếu quy định tòa nhà (giờ thi công, không đục  │
│        kết cấu, đặt cọc...)                                 │
│   → 4. Trả về yêu cầu bổ sung (lặp lại 2–3 vòng)            │
│   → 5. Kỹ sư duyệt kỹ thuật & cấp phép                      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–4 (⏱ ~45 phút/vòng, │
│ 2–3 vòng/hồ sơ)                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–4: LLM kiểm tra│
│ hồ sơ theo checklist quy định ngay khi nộp, draft danh sách │
│ cần bổ sung gửi cư dân (sau khi BQL duyệt).                 │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│  - Giảm số vòng trả hồ sơ từ 2.5 ──> dưới 1.2 vòng.         │
│  - Giảm thời gian chờ cấp phép từ 7–10 ngày ──> dưới 3 ngày.│
│  - 0 hồ sơ được cấp phép mà không có kỹ sư duyệt.           │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ (LLM Feature kiểm tra checklist; cấm tự cấp phép, mọi nội   │
│  dung liên quan kết cấu phải do kỹ sư quyết định)           │
└─────────────────────────────────────────────────────────────┘
```

**Ước tính tổn thất:** 150 hồ sơ/tháng × 45 phút × 2.5 vòng ≈ **280 giờ công BQL/tháng/khu đô thị**.

---

# 🗳️ Đề xuất cho Deep-Dive

Đề xuất chọn **Card #1 — VinFast: Phân loại lỗi xe từ mô tả tiếng Việt** để Deep-Dive:
- **AI Fit rõ ràng nhất:** mô tả tiếng Việt tự do, đa dạng cách diễn đạt → Rule/keyword không xử lý được, LLM có lợi thế thực sự.
- **Ranh giới an toàn dễ định nghĩa và stress-test:** AI chỉ đề xuất + draft; không bao giờ khẳng định xe "an toàn để chạy tiếp"; các triệu chứng phanh/lái/pin bắt buộc escalate cho con người.
- **Metric đo được:** thời gian phân loại và tỉ lệ xếp đúng nhóm lỗi có thể kiểm chứng trên log phiếu dịch vụ hiện có.
