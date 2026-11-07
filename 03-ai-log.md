# 03 — AI Log & Reflection

**Lab 02:** AI Product Scoping (Vin Smart Future)
**Ngày:** 07/11/2026
**Bài toán:** Hỗ trợ tài xế Xanh SM khi gặp sự cố sạc pin hoặc hết pin giữa đường
**Sinh viên:** ______________________ (MSSV: __________)

**Công cụ AI đã dùng:**
- **Claude** — hỗ trợ soạn tài liệu (Phase 1–3), viết và sửa code `prompt_prototype.py`, kiểm tra code theo `autograder.py`, giải thích lỗi.
- **Gemini** (`gemini-3.5-flash`) — model nằm bên trong bản mẫu, đóng vai "Dispatcher Co-pilot" để stress-test ranh giới.

---

## 1. Nhật ký phối hợp với AI

| # | Việc cần làm | Tôi yêu cầu AI | AI trả về | Tôi đánh giá / hành động tiếp theo |
|---|--------------|----------------|-----------|-------------------------------------|
| 1 | Phase 1–2: Scan và Quick-Assess | Tạo `01-problem-scan.md` từ Inspiration Kit, có ví dụ ở Phase 2 và các prompt có sẵn | 5 bài toán, 1 thẻ ví dụ, 3 thẻ đã điền | Dùng làm khung. Nhận ra các con số (phút/lượt, tỷ lệ) là **giả định của AI**, không phải dữ liệu thật |
| 2 | Phase 3: Deep-Dive | Chọn bài toán Xanh SM hết pin giữa đường, viết `02-deep-dive-report.md` theo 3.1–3.3 | Workflow 7 bước (≈20 phút/lượt), bảng 6-field, sơ đồ future-state (Rule + LLM, có HITL và fallback) | Giữ cấu trúc; số liệu vẫn là ước lượng và đã ghi chú trong file |
| 3 | Phase 4: hiểu yêu cầu | Hỏi có bắt buộc "vibe" bằng Gemini 2.5 Flash không | Giải thích: Gemini chỉ là model mà script gọi, không phải công cụ để viết code | Hiểu đúng yêu cầu 3 trước khi làm |
| 4 | Phase 4: viết code | Viết `SYSTEM_PROMPT`, định dạng JSON, `evaluate_prompt()`, thêm test tấn công | System prompt 4 rule, JSON có 8 trường, 6 test (starter có 2) | Đọc lại từng rule, đối chiếu với ranh giới ở mục 3.2 của deep-dive |
| 5 | Gặp lỗi API | Dán lỗi 404 (`gemini-2.5-flash` không còn cho tài khoản mới) | Cách đổi model qua `GEMINI_MODEL`, thêm `--list-models` | Đổi sang `gemini-3.5-flash` và ghi chú lại sự khác biệt với đề |
| 6 | Gặp lỗi quota | Dán lỗi 429 `RESOURCE_EXHAUSTED` (20 request/ngày/model, gói free) và 503 | Giải thích quota tính theo project, thêm `--only`, dừng sớm khi hết quota | Đổi sang key ở project khác, chạy tiết kiệm bằng `--only` |
| 7 | Đọc kết quả chạy | Dán output 2 lần chạy (4/6 và 5/6) | Phân tích: test 5 fail ổn định, vừa do hàm kiểm tra nhầm câu phủ định, vừa do model tự thêm chỉ dẫn an toàn | Đồng ý sửa cả hàm kiểm tra và Rule 3 |
| 8 | Đối chiếu autograder | Hỏi code đã đủ để thỏa `autograder.py` chưa | Phát hiện tiêu chí 2 chỉ đạt 0.5đ (autograder đọc `genai` bên trong `evaluate_prompt`); sửa; chỉ ra còn thiếu `03-ai-log.md` và `04-workflow-diagram` | Chép bản sửa vào dự án; làm tiếp 2 file còn thiếu |
| 9 | Debug tiện ích | Dán traceback `Cannot send a request, as the client has been closed` | Chỉ ra lỗi do client tạo tạm trong vòng `for`; sửa `--list-models` và `--ping` | Chạy lại sau khi sửa |

---

## 2. Kết quả thực tế của bản mẫu (Phase 4)

| Lần chạy | Kết quả | Ghi chú |
|----------|---------|---------|
| Lần 1 | 4/6 | Test 4 lỗi `503 UNAVAILABLE` (Google quá tải, không phải lỗi prompt). Test 5 fail |
| Lần 2 | 5/6 | Test 5 fail lại |
| Sau khi sửa Rule 3 và hàm kiểm tra | Test 5 **PASSED** (chạy riêng bằng `--only 5`) | Model chỉ còn trả "đã chuyển điều phối viên, vui lòng giữ liên lạc và chờ hỗ trợ" |

**Phát hiện chính:** Rule 1 (`[DRAFT_ONLY]`) và Rule 2 (pin < 5% → điều xe sạc di động) giữ vững trước cả 6 kiểu tấn công. Điểm yếu nằm ở Rule 3: với ca có khách + mùi khét, model dù chọn đúng `escalate_to_dispatcher` vẫn tự thêm chỉ dẫn "tấp xe, mời khách rời xe", vượt ranh giới "AI chỉ chuyển điều phối viên". Phải siết prompt mới hết.

---

## 3. AI giúp tốt ở đâu

- **Dựng khung nhanh:** từ yêu cầu bằng lời, AI tạo ngay cấu trúc tài liệu và code chạy được, giúp tôi tập trung vào phần đọc, đối chiếu và sửa.
- **Chẩn đoán lỗi từ log:** các lỗi 404, 429, 503, traceback đều được giải thích nguyên nhân và cách xử lý rõ ràng.
- **Phát hiện điều tôi chưa thấy:** AI đối chiếu code với `autograder.py` và chỉ ra tiêu chí 2 mất điểm, exit code, cách autograder đếm chữ "Failed", hai file nộp còn thiếu.
- **Stress-test:** đề xuất thêm các kiểu tấn công (giả danh admin, ca sát ngưỡng, ép bịa dữ liệu) vượt yêu cầu tối thiểu.

## 4. AI sai hoặc có giới hạn ở đâu

- **Số liệu bịa theo giả định:** 20 phút/lượt, 100 sự cố/ngày, tỷ lệ chuyển sai 15%... đều là ước lượng của AI, không có nguồn. Phải ghi rõ trong tài liệu và thay bằng dữ liệu thật nếu có.
- **Không kiểm chứng được với Gemini thật:** AI không có API key nên chỉ thử code bằng SDK giả. Chỉ khi tôi tự chạy mới biết kết quả thật.
- **Hàm kiểm tra sai:** bản đầu tìm từ khóa "tháo" nên báo fail cả khi model viết "KHÔNG tháo pin". Nếu chỉ nhìn "5/6 pass" mà không đọc output thì sẽ hiểu sai nguyên nhân.
- **Bản đầu chưa đạt autograder:** `evaluate_prompt` gọi SDK qua hàm phụ nên mất 0.5đ; phải sửa sau khi tôi hỏi.
- **Lỗi code do AI viết:** tiện ích `--list-models` tạo client tạm và bị đóng kết nối.
- **Không chắc về tên model:** thông báo lỗi gợi ý `gemini-3.8-flash` nhưng AI không xác nhận được tên nào hợp lệ, phải tự dùng `--list-models`/`--ping` để kiểm tra.
- **Nhất quán giữa các file:** `01-problem-scan.md` được tạo trước khi chốt bài toán, nên top 3 trong file đó chưa khớp bài toán Xanh SM hết pin ở `02-deep-dive-report.md`. Cần cập nhật để các file cùng một câu chuyện.

## 5. Phần việc tôi tự làm và tự quyết định

- Chọn bài toán cho Phase 3 và quyết định nội dung yêu cầu từng bước.
- Cấu hình môi trường (`.venv`, `GEMINI_API_KEY`), tạo key mới, chạy script và dán log lỗi/kết quả để AI phân tích.
- Chọn model thay thế khi `gemini-2.5-flash` không dùng được.
- Quyết định có áp dụng các bản sửa hay không sau khi đọc kết quả test.
- Một điểm thiết kế cần nhóm cân nhắc: cấm AI đưa lời khuyên sơ tán khách là **lựa chọn thiết kế** (AI không tự quyết định an toàn, điều phối viên quyết định), dù ngoài đời lời khuyên đó có thể hợp lý.

## 6. Bài học rút ra

1. **Đọc output thô, không chỉ nhìn số pass/fail.** Test 5 "fail" ẩn hai vấn đề khác nhau: lỗi của bộ kiểm tra và lỗi của model.
2. **Ranh giới bằng prompt phải được kiểm chứng bằng tấn công thật.** Rule nghe có vẻ chặt nhưng model vẫn thêm lời khuyên ngoài phạm vi.
3. **Số liệu do AI đưa ra là giả thuyết cần xác thực**, không phải sự thật.
4. **Đọc autograder sớm** để biết chính xác mình bị chấm gì, tránh mất điểm vì chi tiết kỹ thuật.
5. **Quản lý hạn mức API:** gói miễn phí chỉ 20 request/ngày/model, nên cần chạy test có kế hoạch (dùng `--only`, dừng khi hết quota).
6. **Giữ các file cùng nhất quán** khi bài toán thay đổi giữa các phase.

## 7. Điều tôi sẽ làm khác lần sau

- Chốt bài toán trước rồi mới điền Phase 1–2 để các file khớp nhau.
- Đọc `autograder.py` ngay từ đầu Phase 4.
- Chạy `--list-models` và `--ping` trước khi chạy đủ bộ test để khỏi tốn quota.
- Hỏi giảng viên khi đề yêu cầu một model đã bị đóng (`gemini-2.5-flash`) thay vì tự đổi.
- Kiểm tra lại các số liệu giả định bằng nguồn thật trước khi đưa vào báo cáo.

## 8. Việc còn lại

- [ ] Chạy đủ 6 test với Gemini thật và ghi kết quả cuối vào worksheet.
- [ ] Tạo `04-workflow-diagram` (.png/.jpg/.pdf) từ sơ đồ trong `02-deep-dive-report.md`.
- [ ] Cập nhật `01-problem-scan.md` cho khớp bài toán Xanh SM hết pin.
- [ ] Đặt cả 4 file ở thư mục gốc repo.
