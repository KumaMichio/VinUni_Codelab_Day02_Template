# 🔍 Phase 1 — SCAN: Danh sách bài toán vận hành tại Vingroup

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | **Vinhomes** | **Repetitive & Time-consuming** | Ban Quản lý (BQL) phải đọc, phân loại mức độ khẩn cấp và điều phối hơn 3.000 phản ánh/ngày của cư dân trên app Vinhomes Resident (kèm ảnh/video mô tả tự do) sang đúng bộ phận kỹ thuật, an ninh hoặc vệ sinh. |
| 2 | **VinFast** | **Time-consuming** | Cố vấn dịch vụ tại xưởng phải đọc mô tả lỗi tự do của chủ xe, tra cứu thủ công hàng trăm trang tài liệu kỹ thuật (Service Manual) để xác định mã lỗi bảo hành và danh mục linh kiện cần kiểm tra. |
| 3 | **Vinmec** | **AI-upgrade** | Điều dưỡng mất 10–15 phút/ca để hỏi bệnh sử ban đầu, đọc kết quả xét nghiệm cũ dạng ảnh chụp/PDF từ viện khác và gõ tóm tắt vào hệ thống HIS trước khi bác sĩ khám. |
| 4 | **Xanh SM** | **Stakeholder Pain** | Tổng đài viên CSKH phải nghe lại ghi âm/đọc khiếu nại văn bản của khách và tài xế về việc "tính sai phụ phí/đi đường vòng", đối chiếu log GPS thủ công gây chậm trễ hoàn tiền (tài xế bị khóa tài khoản tạm thời nên bức xúc). |
| 5 | **Vinpearl / VinWonders** | **AI-upgrade** | Nhân viên đặt phòng/CSKH phải xử lý hàng trăm email và tin nhắn đa ngôn ngữ (Anh, Hàn, Trung) yêu cầu thay đổi lịch trình combo (vé máy bay + phòng + vé VinWonders), phải tra cứu chính sách hoàn hủy thủ công. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Thẻ Bài Toán Tiềm Năng

## QUICK PROBLEM CARD #1: Phân loại & Điều phối Ticket Phản ánh Cư dân Vinhomes

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **Bài toán (1 câu)** | Phân loại mức độ khẩn cấp và điều phối tự động yêu cầu sửa chữa/phản ánh của cư dân trên ứng dụng Vinhomes Resident. |
| **Công ty thành viên** | `[ ] VinFast` `[ ] Xanh SM` `[x] Vinhomes` `[ ] Vinmec` `[ ] Khác` |
| **Ai đang đau (Actor)?** | Chuyên viên trực Tổng đài BQL Vinhomes và Kỹ thuật viên tòa nhà. |
| **Workflow thủ công hiện tại (4 bước)** | **1.** Cư dân gửi ticket (text + ảnh) trên app Vinhomes $\rightarrow$ **2.** Nhân viên BQL mở đọc từng ticket, xem ảnh, gọi điện xác minh lại vị trí/tình trạng $\rightarrow$ **3.** Nhân viên BQL gán nhãn danh mục (Điện/Nước/An ninh) và chọn mức ưu tiên (P1-Khẩn cấp / P2 / P3) trên CRM $\rightarrow$ **4.** Giao việc cho tổ kỹ thuật; nếu gán sai tổ, kỹ thuật trả lại ticket để BQL phân bổ lại. |
| **Bước nào tốn thời gian/lỗi nhất?** | Bước 2 & Bước 3 (⏱ **8 phút/lượt**). |
| **AI có thể nhảy vào hỗ trợ ở bước nào?** | Bước 2 & 3: Đọc hiểu ngôn ngữ tự nhiên, trích xuất thông tin sự cố, phát hiện từ khóa nguy hiểm (rò điện, vỡ ống nước) để gắn nhãn P1–P3 và gợi ý bộ phận xử lý kèm bản nháp phản hồi cư dân. |
| **Đo thành công bằng gì (Metric có số)?** | - Giảm thời gian phân loại & điều phối từ **8 phút xuống dưới 45 giây/ticket**.<br>- Đạt độ chính xác gán đúng bộ phận kỹ thuật **>= 90%**. |
| **Quick Architecture** | `[ ] No AI` `[ ] Rule` `[x] LLM` `[ ] Agent` |

---

## QUICK PROBLEM CARD #2: Trợ lý Tra cứu Mã lỗi & Bảo hành cho Cố vấn Dịch vụ VinFast

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **Bài toán (1 câu)** | Chuyển đổi mô tả triệu chứng xe bằng ngôn ngữ đời thường của khách thành mã chẩn đoán kỹ thuật và quy trình kiểm tra chuẩn cho Cố vấn dịch vụ (CVD) VinFast. |
| **Công ty thành viên** | `[x] VinFast` `[ ] Xanh SM` `[ ] Vinhomes` `[ ] Vinmec` `[ ] Khác` |
| **Ai đang đau (Actor)?** | Cố vấn dịch vụ (Service Advisor) tại các xưởng dịch vụ VinFast 3S. |
| **Workflow thủ công hiện tại (4 bước)** | **1.** Chủ xe mô tả triệu chứng lỗi (VD: *"xe kêu lục cục dưới gầm khi đánh lái hết cỡ lúc trời mưa"*) $\rightarrow$ **2.** CVD ghi chép và mở PDF cẩm nang sửa chữa (Service Manual) hoặc bản tin kỹ thuật (TSB) để tìm lỗi khớp $\rightarrow$ **3.** CVD đối chiếu điều kiện bảo hành của từng linh kiện $\rightarrow$ **4.** Lập lệnh sửa chữa (Repair Order) chuyển xuống xưởng. |
| **Bước nào tốn thời gian/lỗi nhất?** | Bước 2 & Bước 3 (⏱ **15 phút/xe**). |
| **AI có thể nhảy vào hỗ trợ ở bước nào?** | Bước 2 & 3: Chuẩn hóa mô tả của khách, truy xuất (RAG) vào kho tài liệu kỹ thuật VinFast để gợi ý Top 3 nguyên nhân gốc và checklist kiểm tra. |
| **Đo thành công bằng gì (Metric có số)?** | - Giảm thời gian tra cứu & lập lệnh sửa chữa từ **15 phút xuống dưới 3 phút/xe**.<br>- Giảm tỷ lệ chẩn đoán ban đầu sai lệch xuống **dưới 10%**. |
| **Quick Architecture** | `[ ] No AI` `[ ] Rule` `[x] LLM` `[ ] Agent` |

---

## QUICK PROBLEM CARD #3: Phân tích & Xử lý Khiếu nại Cước phí Xanh SM

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **Bài toán (1 câu)** | Tự động tổng hợp bằng chứng hành trình và phân loại khiếu nại cước phí/phụ phí của khách hàng Xanh SM. |
| **Công ty thành viên** | `[ ] VinFast` `[x] Xanh SM` `[ ] Vinhomes` `[ ] Vinmec` `[ ] Khác` |
| **Ai đang đau (Actor)?** | Chuyên viên xử lý khiếu nại (Dispute Resolution Specialist) tại Xanh SM. |
| **Workflow thủ công hiện tại (4 bước)** | **1.** Khách gửi khiếu nại trên app Xanh SM (*"tài xế đi lòng vòng"*, *"thu thêm phí cầu đường sai"*) $\rightarrow$ **2.** Nhân viên CSKH mở 3 màn hình: nội dung chat, lộ trình GPS thực tế vs. Google Maps chuẩn, và bảng kê cước $\rightarrow$ **3.** Đọc giải trình của tài xế và tính toán lại chênh lệch km/phụ phí bằng tay $\rightarrow$ **4.** Ra quyết định hoàn tiền (voucher/ví) và soạn tin nhắn phản hồi cho cả khách lẫn tài xế. |
| **Bước nào tốn thời gian/lỗi nhất?** | Bước 2 & Bước 3 (⏱ **12 phút/ca**). |
| **AI có thể nhảy vào hỗ trợ ở bước nào?** | Bước 2, 3 & 4: Tóm tắt mâu thuẫn giữa lời khai khách và tài xế, đối chiếu dữ liệu độ lệch GPS (từ rule-engine) và soạn thảo thông báo kết quả. |
| **Đo thành công bằng gì (Metric có số)?** | - Rút ngắn thời gian xử lý mỗi ticket từ **12 phút xuống dưới 2 phút**.<br>- Đảm bảo **95%** khiếu nại cước phí được phản hồi trong vòng 2 giờ. |
| **Quick Architecture** | `[ ] No AI` `[ ] Rule` `[x] LLM` `[ ] Agent` |