# 01 — Problem Scan (Vin Smart Future Edition)

> Mục tiêu: quét vận hành của các công ty thành viên Vingroup, tìm bài toán thực tế đáng giải bằng AI, rồi đánh giá nhanh top 3.
> Nguồn bài toán: `03-inspiration-kit.md`. Các con số trong thẻ là **ước lượng giả định**, cần thay bằng số liệu thực khi có.

---

## 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Dùng 4 Lenses để quét hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại ít nhất 5 bài toán/bottleneck thực tế.

### 4 Lenses

| Lens | Ý nghĩa | Ví dụ |
|------|---------|-------|
| **Lặp lại** (Repetitive) | Tác vụ lặp đi lặp lại nhiều lần hằng ngày | So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM |
| **Tốn thời gian** (Time-consuming) | Tác vụ ngốn thời gian xử lý thủ công của nhân viên | Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes |
| **AI có thể tốt hơn** (AI-upgrade) | Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn | Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi |
| **Pain từ người khác** (Stakeholder Pain) | Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn | Tài xế Xanh SM phàn nàn về điểm đón khách không chính xác |

> 🤖 **AI Prompt — Partner brainstorm** (dùng nếu chưa có ý tưởng):
>
> ```
> Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất.
> ```

### 📝 List bài toán của tôi

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinUni** | Lặp lại | Tự động hóa chấm điểm và phản hồi bài lab: autograder chấm code, LLM phân tích lỗi cú pháp/logic và draft phản hồi sư phạm cho sinh viên. |
| 2 | **Vinpearl** | Pain từ người khác | Tổng hợp review từ Booking.com, Agoda, Google Maps để lọc các phàn nàn khẩn cấp ("phòng bẩn", "nhân viên thái độ tệ") gửi về Manager. |
| 3 | **VinFast** | Lặp lại | Đối chiếu dữ liệu sạc điện hằng tuần từ hàng nghìn trụ sạc đối tác với hóa đơn gửi về hệ thống tài chính. |
| 4 | **Vinhomes** | Lặp lại | Phân loại và điều hướng phản ánh cư dân (mất nước, hỏng đèn, ồn ào) từ App Vinhomes Resident đến đúng ban quản lý tòa nhà. |
| 5 | **Xanh SM** | Pain từ người khác | Phân tích lý do hủy chuyến từ ghi âm cuộc gọi và ghi chú tài xế, phân loại 10 lý do phổ biến nhất gây rò rỉ cuốc. |

---

## 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn top 3 bài toán từ Phase 1 (**#1, #2, #3**) và hoàn thiện 3 Quick Problem Cards (10 phút/card). Bài #4 (Vinhomes) được dùng làm ví dụ mẫu.

### ✅ Ví dụ mẫu — Bài toán #4 (Vinhomes)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #EXAMPLE                                 │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại và điều hướng phản ánh  │
│ của cư dân gửi qua App Vinhomes Resident đến đúng ban quản  │
│ lý tòa nhà.                                                 │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [X] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên tiếp nhận phản ánh (đọc và   │
│ chuyển tay từng ticket); cư dân (chờ lâu, bị chuyển nhầm).  │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Cư dân gửi phản ánh trên app                           │
│   ──> 2. Nhân viên tiếp nhận đọc nội dung                   │
│   ──> 3. Tự xác định loại sự cố (điện/nước/ồn/vệ sinh...)   │
│   ──> 4. Chuyển ticket đến bộ phận phụ trách                │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4: đọc, phân loại   │
│ và chuyển đúng nơi (⏱ ~4 phút/lượt, dễ chuyển nhầm).        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3: gán nhãn loại │
│ sự cố + mức khẩn cấp, gợi ý bộ phận ở bước 4.               │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Thời gian phân loại + điều hướng: 4 min ──> under 30 giây │
│ - Tỷ lệ ticket chuyển sai bộ phận: 15% ──> under 5%         │
│                                                             │
│ Quick Architecture: [ ] No AI  [X] Rule  [X] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Card #1 — Bài toán #1 (VinUni: chấm lab và phản hồi)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tự động chấm bài lab lập trình và draft   │
│ phản hồi sư phạm giải thích lỗi cho từng sinh viên.         │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [X] Khác (Ghi rõ) VinUni   │
│                                                             │
│ Ai đang đau (Actor)? Giảng viên/trợ giảng (đọc từng bài      │
│ nộp, viết nhận xét lặp lại); sinh viên (nhận phản hồi muộn, │
│ chung chung).                                               │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Sinh viên nộp bài lab                                  │
│   ──> 2. Chạy autograder/test case, xem điểm pass/fail      │
│   ──> 3. Trợ giảng đọc code, tìm nguyên nhân lỗi            │
│   ──> 4. Viết nhận xét cho từng sinh viên                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4: đọc code và viết │
│ nhận xét (⏱ ~8 phút/bài, lớp 60 sinh viên ≈ 8 giờ/lab).     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4: LLM nhận    │
│ code + kết quả test, phân loại lỗi (cú pháp/logic) và draft │
│ phản hồi; trợ giảng duyệt trước khi gửi.                    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Thời gian phản hồi mỗi bài: 8 min ──> under 2 min         │
│ - Tỷ lệ draft được trợ giảng duyệt không cần sửa lớn: > 80% │
│                                                             │
│ Quick Architecture: [ ] No AI  [X] Rule  [X] LLM  [ ] Agent │
│ (Autograder = rule; LLM chỉ giải thích lỗi, không tự chấm   │
│  điểm cuối.)                                                │
└─────────────────────────────────────────────────────────────┘
```

### Card #2 — Bài toán #2 (Vinpearl: lọc review khẩn cấp)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Quét review khách sạn Vinpearl trên nhiều │
│ nền tảng, lọc phàn nàn khẩn cấp và gửi cảnh báo cho Manager.│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [X] Khác (Ghi rõ) Vinpearl │
│                                                             │
│ Ai đang đau (Actor)? Manager khách sạn (không kịp biết khách │
│ đang phàn nàn); khách lưu trú (vấn đề không được xử lý      │
│ khi vẫn đang ở).                                            │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhân viên mở lần lượt Booking.com/Agoda/Google Maps    │
│   ──> 2. Đọc review mới                                     │
│   ──> 3. Tự đánh giá review nào nghiêm trọng                │
│   ──> 4. Báo/copy cho Manager qua chat hoặc email           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3: đọc hàng trăm    │
│ review/tuần, dễ bỏ sót (⏱ ~2 giờ/ngày/khách sạn).           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: LLM phân    │
│ loại chủ đề (vệ sinh, thái độ nhân viên...) và mức khẩn     │
│ cấp, tóm tắt gửi Manager.                                   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Thời gian từ lúc đăng review đến lúc Manager biết:        │
│   24 giờ ──> under 1 giờ                                    │
│ - Recall phàn nàn khẩn cấp: > 90% (so với gán nhãn tay)     │
│                                                             │
│ Quick Architecture: [ ] No AI  [X] Rule  [X] LLM  [ ] Agent │
│ (Rule bắt từ khóa rõ; LLM xử lý ngữ cảnh, mỉa mai, nhiều    │
│  ngôn ngữ.)                                                 │
└─────────────────────────────────────────────────────────────┘
```

### Card #3 — Bài toán #3 (VinFast: đối chiếu hóa đơn sạc)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tự động so khớp dữ liệu sạc điện hằng tuần │
│ từ trụ sạc đối tác với hóa đơn gửi về hệ thống tài chính.   │
│ Công ty thành viên: [X] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Kế toán/đối soát tài chính (dò từng    │
│ dòng giao dịch); đối tác trụ sạc (thanh toán chậm do lệch   │
│ số liệu).                                                   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận file dữ liệu sạc từ đối tác                       │
│   ──> 2. Xuất hóa đơn từ hệ thống tài chính                 │
│   ──> 3. So khớp theo mã giao dịch/trụ/thời gian bằng Excel │
│   ──> 4. Điều tra và xử lý các dòng lệch                    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4: so khớp hàng     │
│ nghìn dòng và truy vết dòng lệch (⏱ ~1-2 ngày/tuần).        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Phần lớn là so khớp   │
│ xác định nên dùng rule/script; AI (nếu có) chỉ ở bước 4:    │
│ gợi ý nguyên nhân lệch hoặc fuzzy-match mã không đồng nhất. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Thời gian đối soát hằng tuần: 1.5 ngày ──> under 2 giờ    │
│ - Tỷ lệ dòng tự khớp không cần người: > 95%                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [X] Rule  [ ] LLM  [ ] Agent │
│ (Theo nguyên tắc "Problem First, AI Second": rule-based đủ  │
│  cho phần lớn bài toán này.)                                │
└─────────────────────────────────────────────────────────────┘
```

> 🤖 **AI Prompt — Stress-Test thẻ bài toán** (dán nội dung từng thẻ vào LLM để nhận phản biện):
>
> ```
> Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI.
> ```

---

## 💡 Lưu ý khi chọn bài toán

1. **Chọn bài toán bạn hiểu rõ nhất:** ưu tiên bài toán mà ít nhất một thành viên trong nhóm hiểu rõ workflow hiện tại.
2. **Operational Boundary quan trọng nhất:** ở mảng nhạy cảm (Vinmec, an toàn xe VinFast) phải có Human-in-the-loop. Ở các thẻ trên, AI chỉ *draft/gợi ý*, con người duyệt (trợ giảng, Manager, kế toán).
3. **Problem First, AI Second:** Card #3 cố ý chọn "Rule" để minh họa: giải pháp đơn giản mang lại giá trị cao vẫn được điểm tối đa.
