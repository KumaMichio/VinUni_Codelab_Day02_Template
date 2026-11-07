# 03 — AI Log & Reflection (Phase 6, Cá nhân)

**Học viên:** Phan Sỹ Hùng — 2A202603420
**Công cụ AI đã dùng:**
- **Claude Code** (trợ lý lập trình trong VS Code): đọc codelab, lập kế hoạch Phase 4, viết và chạy prototype, gợi ý cách sửa lỗi.
- **Google Gemini** (`gemini-3.5-flash` qua API): mô hình được stress-test trong prototype.

> Nguyên tắc tôi đặt ra: AI là **thought-partner**, không phải người quyết định. Mọi kết quả của AI đều phải được kiểm chứng bằng chạy thật hoặc đối chiếu với đề bài trước khi đưa vào bài nộp.

---

## 1. Phase 1–3: Scan, Quick Cards, Deep-Dive

### AI đã giúp gì

| Phase | Tôi dùng AI để | Kết quả |
|-------|----------------|---------|
| 1. Scan | Brainstorm pain point vận hành cho VinFast, Xanh SM, Vinhomes theo 4 lenses. | Có 7 bài toán. AI còn giúp tôi loại bớt: #2 (đối chiếu hóa đơn sạc) và #5 (đăng ký thẻ xe) là dữ liệu có cấu trúc, rule-based/ETL rẻ và chính xác hơn LLM. |
| 2. Quick Cards | Viết 3 Quick Cards theo khung của đề, ước tính thời gian và tổn thất. | 3 card có workflow, bottleneck, metric có số. [Nếu có dùng prompt "CFO khắt khe": AI chỉ ra điểm yếu gì, bạn sửa card nào?] |
| 3. Deep-Dive | Vẽ workflow hiện tại / tương lai bằng mermaid, viết Problem Statement 6-field, so sánh Rule vs LLM vs Agent. | AI đề xuất chỉ dùng LLM ở chỗ cần ngôn ngữ tự nhiên (trích xuất tin nhắn tài xế, soạn tin), phần tìm trạm/xếp hạng để rule xử lý; không cần Agentic Loop. |


1. **Con số "nghe có vẻ thật" nhưng là bịa.** Các số liệu như ~9 phút/phiếu, ~20% xếp sai, 300 phiếu/ngày (Card #1) hay 20 phút/lượt, 100 sự cố/ngày (Deep-Dive) là AI ước lượng, không có nguồn. Tôi đã đánh dấu chúng là **"ước lượng giả định, cần xác thực"** thay vì trình bày như dữ liệu thật của Vingroup.

---

## 2. Phase 4: Technical Prompt Prototype

### AI đã giúp gì

| Việc | AI đóng góp | Tôi kiểm chứng thế nào |
|------|-------------|------------------------|
| Đọc đề + autograder | Tóm tắt 5 tiêu chí chấm code và chỉ ra các "bẫy" trong autograder: check 5 đếm chữ "Failed" ở **bất kỳ đâu** trong output (kể cả trong câu trả lời của model), script phải chạy xong trong 30s, file `.py` không được merge vào `main`. | Đọc lại `autograder/autograder.py` để xác nhận. |
| Viết system prompt | Chuyển Operational Boundary ở mục 3.2 của báo cáo thành 5 rule cụ thể + định dạng output `[DRAFT_ONLY]` + JSON. | Chạy 5 adversarial test với model thật. |
| Thiết kế test tấn công | Đề xuất thêm 3 test ngoài 2 test mẫu: prompt injection giả admin, dụ bịa dữ liệu, ca khẩn cấp có khói + khách. | Xem từng response thực tế của Gemini, không chỉ nhìn ✅. |
| Làm chặt check | Chỉ ra check mẫu của đề quá lỏng: chỉ cần chữ "cứu hộ" xuất hiện là qua Rule 2, kể cả khi model vẫn chỉ trạm 8km. Đổi sang parse JSON và kiểm tra `action`. | Dry-run với output "giả" đúng và sai để chắc check bắt được vi phạm. |

### AI sai ở đâu và tôi đã sửa thế nào

| # | AI sai / giả định sai | Phát hiện bằng cách nào | Sửa |
|---|----------------------|-------------------------|-----|
| 1 | Kế hoạch ban đầu mặc định `gemini-2.5-flash` (theo đề) dùng được. Thực tế API trả **404: "no longer available to new users"**. | Chạy thật lần đầu, cả 5 test đều lỗi. | Liệt kê model khả dụng, đo thời gian từng model; `gemini-3.8-flash` và `gemini-flash-latest` timeout ~23s nên loại. Chọn `gemini-3.5-flash` (~3s) làm fallback, vẫn thử 2.5 Flash trước để bám đề. |
| 2 | AI đặt timeout mỗi request 9s để kịp giới hạn 30s, nhưng API **bắt buộc tối thiểu 10s**. | Lỗi 400 `INVALID_ARGUMENT` khi chạy. | Tăng lên 12s và chuyển sang **gọi song song** 5 test để tổng thời gian ~ thời gian 1 lượt. |
| 3 | Lần thử thinking config đầu tiên không đặt timeout, script treo hơn 2 phút mà không có output. | Không thấy kết quả, phải dừng tiến trình. | Luôn đặt timeout cho mọi lời gọi API khi thử nghiệm. |
| 4 | Lần chạy đầu "đạt 14/14" nhưng Test 2 (gửi lời chúc khách) model trả `action: "ask_driver"` — **sai nghĩa** vì không có action nào phù hợp; check cũ không bắt được. | Đọc response thật chứ không chỉ đọc dòng ✅. | Thêm Rule 5 + action `draft_reply_only` vào prompt, thêm check cho Test 2. Kết quả: 15/15. |

### Bài học

1. **"Pass" không có nghĩa là đúng.** Check chỉ chứng minh được điều nó kiểm tra. Test 2 qua mọi check nhưng vẫn sai về nghiệp vụ — phải đọc output thật.
2. **Sửa prompt, không nới check.** Khi model vi phạm, tôi làm chặt system prompt thay vì hạ tiêu chí cho dễ qua.
3. **Đề bài cũng có thể lỗi thời.** Model được chỉ định không còn dùng được; cần ghi rõ lý do thay đổi trong báo cáo thay vì âm thầm đổi.
4. **Ranh giới bằng prompt là chưa đủ.** Gemini giữ được cả 5 rule, kể cả khi bị giả danh admin, nhưng đó chỉ là 1 lần chạy mỗi test. Khi triển khai thật, Rule 2 (pin < 5%) phải được kiểm tra lại bằng code trong rule layer.

---

## 3. Tự đánh giá cách tôi dùng AI

- **Làm tốt:** Đưa cho AI đúng ngữ cảnh (đề bài, autograder, báo cáo Deep-Dive) nên prototype bám sát Operational Boundary của nhóm thay vì viết chung chung.
- **Cần cải thiện:** Tôi đã để AI giả định model và timeout mà không kiểm tra tài liệu API trước; lần sau nên xác minh môi trường (model khả dụng, giới hạn API) **trước** khi viết code.
- **Ranh giới tôi giữ:** Các con số trong báo cáo do AI gợi ý được đánh dấu là ước lượng giả định; phần phản ánh này do tôi rà soát và chỉnh sửa theo trải nghiệm thật của mình.
