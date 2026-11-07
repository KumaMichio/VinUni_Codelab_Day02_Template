# 03 — AI Log & Reflection

**Lab 02:** AI Product Scoping (Vin Smart Future)
**Ngày:** 07/11/2026
**Bài toán:** Hỗ trợ tài xế Xanh SM khi gặp sự cố sạc pin hoặc hết pin giữa đường

---

## 1. Nhật ký phối hợp với AI

| # | Việc cần làm | Tôi yêu cầu AI | AI trả về | Tôi đánh giá / hành động tiếp theo |
|---|--------------|----------------|-----------|-------------------------------------|
| 1 | Phase 1–2: Scan và Quick-Assess | Tạo `01-problem-scan.md` từ Inspiration Kit, có ví dụ ở Phase 2 và các prompt có sẵn | 5 bài toán, 1 thẻ ví dụ, 3 thẻ đã điền | Dùng làm khung. Nhận ra các con số (phút/lượt, tỷ lệ) là **giả định của AI**, không phải dữ liệu thật |
| 2 | Gặp lỗi API | Dán lỗi 404 (`gemini-2.5-flash` không còn cho tài khoản mới) | Cách đổi model qua `GEMINI_MODEL`, thêm `--list-models` | Đổi sang `gemini-3.5-flash` và ghi chú lại sự khác biệt với đề |
| 3 | Gặp lỗi quota | Dán lỗi 429 `RESOURCE_EXHAUSTED` (20 request/ngày/model, gói free) và 503 | Giải thích quota tính theo project, thêm `--only`, dừng sớm khi hết quota | Đổi sang key ở project khác, chạy tiết kiệm bằng `--only` |
| 4 | Đọc kết quả chạy | Dán output 2 lần chạy (4/6 và 5/6) | Phân tích: test 5 fail ổn định, vừa do hàm kiểm tra nhầm câu phủ định, vừa do model tự thêm chỉ dẫn an toàn | Đồng ý sửa cả hàm kiểm tra và Rule 3 |
| 5 | Đối chiếu autograder | Hỏi code đã đủ để thỏa `autograder.py` chưa | Phát hiện tiêu chí 2 chỉ đạt 0.5đ (autograder đọc `genai` bên trong `evaluate_prompt`); sửa; chỉ ra còn thiếu `03-ai-log.md` và `04-workflow-diagram` | Chép bản sửa vào dự án; làm tiếp 2 file còn thiếu |


---

## 2. Kết quả thực tế của bản mẫu (Phase 4)

| Lần chạy | Kết quả | Ghi chú |
|----------|---------|---------|
| Lần 1 | 4/6 | Test 4 lỗi `503 UNAVAILABLE` (Google quá tải, không phải lỗi prompt). Test 5 fail |
| Lần 2 | 5/6 | Test 5 fail lại |

**Phát hiện chính:** Rule 1 (`[DRAFT_ONLY]`) và Rule 2 (pin < 5% → điều xe sạc di động) giữ vững trước cả 6 kiểu tấn công. Điểm yếu nằm ở Rule 3: với ca có khách + mùi khét, model dù chọn đúng `escalate_to_dispatcher` vẫn tự thêm chỉ dẫn "tấp xe, mời khách rời xe", vượt ranh giới "AI chỉ chuyển điều phối viên". Phải siết prompt mới hết.

---

## 3. AI giúp tốt ở đâu

- **Dựng khung nhanh:** từ yêu cầu bằng lời, AI tạo ngay cấu trúc tài liệu và code chạy được, giúp tôi tập trung vào phần đọc, đối chiếu và sửa.
- **Chẩn đoán lỗi từ log:** các lỗi 404, 429, 503, traceback đều được giải thích nguyên nhân và cách xử lý rõ ràng.
- **Stress-test:** đề xuất thêm các kiểu tấn công (giả danh admin, ca sát ngưỡng, ép bịa dữ liệu) vượt yêu cầu tối thiểu.

## 4. AI sai hoặc có giới hạn ở đâu

- **Số liệu bịa theo giả định:** 20 phút/lượt, 100 sự cố/ngày, tỷ lệ chuyển sai 15%... đều là ước lượng của AI, không có nguồn. Phải ghi rõ trong tài liệu và thay bằng dữ liệu thật nếu có.
- **Không kiểm chứng được với Gemini thật:** AI không có API key nên chỉ thử code bằng SDK giả. Chỉ khi tôi tự chạy mới biết kết quả thật.
- **Hàm kiểm tra sai:** bản đầu tìm từ khóa "tháo" nên báo fail cả khi model viết "KHÔNG tháo pin". Nếu chỉ nhìn "5/6 pass" mà không đọc output thì sẽ hiểu sai nguyên nhân.
- **Bản đầu chưa đạt autograder:** `evaluate_prompt` gọi SDK qua hàm phụ nên mất 0.5đ; phải sửa sau khi tôi hỏi.
- **Lỗi code do AI viết:** tiện ích `--list-models` tạo client tạm và bị đóng kết nối.
- **Không chắc về tên model:** thông báo lỗi gợi ý `gemini-3.8-flash` nhưng AI không xác nhận được tên nào hợp lệ, phải tự dùng `--list-models`/`--ping` để kiểm tra.

## 5. Phần việc tôi tự làm và tự quyết định

- Chọn bài toán cho Phase 3 và quyết định nội dung yêu cầu từng bước.
- Cấu hình môi trường (`.venv`, `GEMINI_API_KEY`), tạo key mới, chạy script và dán log lỗi/kết quả để AI phân tích.
- Chọn model thay thế khi `gemini-2.5-flash` không dùng được.
- Quyết định có áp dụng các bản sửa hay không sau khi đọc kết quả test.

## 6. Bài học rút ra

1. **Đọc output thô, không chỉ nhìn số pass/fail.** Test 5 "fail" ẩn hai vấn đề khác nhau: lỗi của bộ kiểm tra và lỗi của model.
2. **Ranh giới bằng prompt phải được kiểm chứng bằng tấn công thật.** Rule nghe có vẻ chặt nhưng model vẫn thêm lời khuyên ngoài phạm vi.
3. **Số liệu do AI đưa ra là giả thuyết cần xác thực**, không phải sự thật.
4. **Đọc autograder sớm** để biết chính xác mình bị chấm gì, tránh mất điểm vì chi tiết kỹ thuật.
5. **Quản lý hạn mức API:** gói miễn phí chỉ 20 request/ngày/model, nên cần chạy test có kế hoạch (dùng `--only`, dừng khi hết quota).
6. **Giữ các file cùng nhất quán** khi bài toán thay đổi giữa các phase.

## 7. Điều tôi sẽ làm khác lần sau

- Đọc `autograder.py` ngay từ đầu Phase 4.
- Chạy `--list-models` và `--ping` trước khi chạy đủ bộ test để khỏi tốn quota.
- Hỏi giảng viên khi đề yêu cầu một model đã bị đóng (`gemini-2.5-flash`) thay vì tự đổi.
- Kiểm tra lại các số liệu giả định bằng nguồn thật trước khi đưa vào báo cáo.
