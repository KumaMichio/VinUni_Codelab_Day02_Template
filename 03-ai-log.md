# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

## 1. AI Readiness Checklist

| # | Tiêu chí đánh giá | Trạng thái | Giải thích |
|---|-------------------|------------|------------|
| 1 | Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? | [x] Có điều kiện | Có thể chuẩn bị dữ liệu giả lập hoặc dữ liệu sự cố đã ẩn danh, bao gồm vị trí xe, mã xe, phần trăm pin, tình trạng trạm sạc và đội cứu hộ. Tuy nhiên, cần xác minh khả năng truy cập dữ liệu thực tế trước khi tích hợp. |
| 2 | Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? | [x] Có | LLM chỉ trích xuất thông tin và soạn tin nhắn. Rule-based xử lý việc lọc, xếp hạng trạm sạc và kiểm tra điều kiện khẩn cấp. Điều phối viên duyệt phương án cuối cùng. Nếu AI lỗi hoặc thiếu thông tin, hệ thống chuyển về quy trình thủ công. |
| 3 | Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? | [x] Có điều kiện | Cần có sự tham gia của tài xế, điều phối viên và bộ phận cứu hộ trong quá trình thử nghiệm. Giai đoạn đầu chạy song song với quy trình hiện tại để lấy phản hồi và đánh giá hiệu quả. |

## 2. Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future

- [x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
- [ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
- [ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

## 3. Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí)

Nhóm quyết định GO vì bài toán xử lý sự cố hết pin giữa đường của tài xế Xanh SM có tính khả thi về mặt kỹ thuật, quy trình hiện tại tồn tại những điểm nghẽn rõ ràng và có thể xây dựng Prototype với phạm vi kiểm soát được.

### a. Cơ sở kỹ thuật

- Quy trình hiện tại mất khoảng 20 phút/lượt xử lý sự cố theo ước tính tại Phase 3.
- Hai bước tốn thời gian nhất là thu thập thông tin từ tài xế (4 phút) và tra cứu trạm sạc/đội cứu hộ (5 phút).
- LLM có thể trích xuất thông tin từ tin nhắn hoặc giọng nói tự nhiên của tài xế, bao gồm vị trí, mã xe, phần trăm pin và tình trạng có khách trên xe.
- Rule-based có thể xử lý việc ước tính quãng đường còn đi được, lọc trạm sạc khả dụng và xếp hạng đội cứu hộ gần nhất.
- Không cần sử dụng Agentic Loop vì quy trình không quá phức tạp và các hành động điều cứu hộ cần được con người phê duyệt.

### b. Cơ sở chi phí và hiệu quả

Theo số liệu ước tính tại Phase 3:

- Thời gian xử lý hiện tại: khoảng 20 phút/lượt.
- Thời gian xử lý dự kiến sau khi có AI: khoảng 5–6 phút/lượt.
- Thời gian tiết kiệm: khoảng 14–15 phút/lượt.
- Mục tiêu giảm thời gian xử lý tổng thể khoảng 70–75%.

Ví dụ, nếu có 100 sự cố/ngày:

- Thời gian xử lý hiện tại: khoảng 33 giờ công/ngày.
- Thời gian xử lý sau khi ứng dụng AI: khoảng 8–10 giờ công/ngày.
- Thời gian có thể tiết kiệm: khoảng 23–25 giờ công/ngày.

Như vậy, giải pháp có tiềm năng giảm đáng kể thời gian làm việc của điều phối viên, đồng thời giúp tài xế nhận được phương án hỗ trợ nhanh hơn.

Lưu ý: Các con số trên là giả định phục vụ đánh giá, chưa phải số liệu nội bộ được Xanh SM xác nhận. Chi phí phát triển, API, tích hợp dữ liệu và bảo trì cần được tính toán thêm trước khi triển khai thực tế.

### c. Phạm vi Prototype đề xuất

Nhóm giới hạn Prototype trong 4 chức năng chính:

1. **Tiếp nhận thông tin sự cố:** Nhận nội dung báo cáo từ tài xế dưới dạng văn bản hoặc giọng nói.
2. **Trích xuất thông tin bằng LLM:** Xác định vị trí, mã xe, phần trăm pin, tình trạng có khách và mức độ khẩn cấp.
3. **Đề xuất phương án bằng Rule-based:** Lọc trạm sạc phù hợp hoặc lựa chọn đội cứu hộ khả dụng dựa trên dữ liệu mẫu.
4. **Phê duyệt bởi điều phối viên (HITL):** Hiển thị phương án và tin nhắn đề xuất để điều phối viên kiểm tra, chỉnh sửa và xác nhận.

Prototype chỉ sử dụng dữ liệu mẫu hoặc dữ liệu đã được cho phép. Không tự động điều động đội cứu hộ hoặc gửi phương án cuối cùng đến tài xế khi chưa có người phê duyệt.

### d. Tiêu chí đánh giá Prototype

| Metric | Mục tiêu thử nghiệm |
|--------|---------------------|
| Thời gian đưa ra phương án đề xuất | Dưới 2 phút |
| Tổng thời gian xử lý sự cố | Dưới 6 phút |
| Độ chính xác trích xuất thông tin cốt lõi | ≥ 95% |
| Tỷ lệ phương án được điều phối viên duyệt mà không cần sửa lớn | ≥ 85% |
| Tỷ lệ gợi ý trạm sạc không khả dụng hoặc không thể tới được | < 3% |
| Tỷ lệ sự cố được chuyển đúng quy trình khẩn cấp | 100% |

Các mục tiêu trên là ngưỡng đánh giá đề xuất, cần kiểm thử trên bộ dữ liệu có kết quả chuẩn do con người xác nhận.

## 4. Kết luận

Nhóm quyết định GO để bắt đầu xây dựng Prototype cho bài toán điều phối hỗ trợ tài xế Xanh SM khi gặp sự cố hết pin giữa đường.

Giải pháp kết hợp LLM Feature và Rule-based nhằm tận dụng ưu điểm của từng công nghệ: LLM xử lý ngôn ngữ tự nhiên, còn Rule-based đảm nhiệm các phép tính và điều kiện nghiệp vụ có thể xác định rõ ràng.

Nhóm sẽ triển khai theo phạm vi nhỏ, sử dụng dữ liệu mẫu, có Human-in-the-loop và cơ chế Fallback. Mọi quyết định điều cứu hộ hoặc gửi phương án cuối cùng đều phải được điều phối viên phê duyệt.

Sau khi Prototype đạt các Metric đề ra và được kiểm chứng bằng dữ liệu thực tế, nhóm mới xem xét mở rộng phạm vi tích hợp vào quy trình vận hành của Xanh SM.