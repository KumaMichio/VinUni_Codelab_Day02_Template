# 🏗️ Phase 3 — DEEP-DIVE: Hệ thống Phân loại & Điều phối Phản ánh Cư dân Thông minh (Vinhomes)

## 3.1. Current-State Workflow Mapping
**Quy trình xử lý phản ánh cư dân hiện tại tại Ban Quản Lý (BQL) Vinhomes Smart City / Ocean Park:**

1. **[Cư dân]** Gửi phản ánh qua ứng dụng *Vinhomes Resident* (nhập văn bản tự do, thường viết tắt/không dấu, kèm ảnh chụp sự cố) — `⏱ 2 phút`.
   * 🔄 **Handoff 1:** App đẩy ticket thô vào hàng đợi chung (Unassigned Queue) trên hệ thống CRM của BQL.
2. **[Chuyên viên CSKH BQL]** Mở từng ticket theo thứ tự thời gian (FIFO). Đọc nội dung mơ hồ (VD: *"nhà chị chỗ ban công cứ kêu ầm ầm với có mùi khét"*), phải gọi điện lại cho cư dân để hỏi rõ căn hộ, vị trí, mức độ nguy hiểm — `⏱ 5 phút`.
   * 🔴 **BOTTLENECK CHÍNH:** Vào khung giờ cao điểm (18h00 – 21h00), hàng trăm ticket đổ về cùng lúc. Do xử lý theo thứ tự FIFO, các sự cố khẩn cấp (rò rỉ khí gas, chập điện, vỡ đường ống nước trục đứng) bị chôn vùi dưới các ticket thông thường (hỏi phí gửi xe, đăng ký thẻ cư dân), gây chậm trễ ứng cứu.
3. **[Chuyên viên CSKH BQL]** Chọn thủ công Danh mục (Điện nước ME / An ninh / Vệ sinh / Hành chính), gắn mức độ ưu tiên (P1-Khẩn cấp, P2-Cao, P3-Bình thường) và gõ tóm tắt lệnh điều động trên CRM — `⏱ 3 phút`.
   * 🔄 **Handoff 2:** Hệ thống chuyển phiếu việc sang App nội bộ của Tổ Kỹ thuật / Bảo vệ.
4. **[Tổ trưởng Kỹ thuật]** Đọc phiếu việc. Khoảng **18% phiếu bị phân loại sai bộ phận** (VD: lỗi thấm trần do căn tầng trên nhưng lại giao cho bộ phận Vệ sinh thay vì Kỹ thuật Xây dựng), dẫn đến việc kỹ thuật viên phải trả lại phiếu (Reject) về cho CSKH phân loại lại — `⏱ 4 phút` (cộng thêm 15–30 phút chờ luân chuyển lại).
5. **[Chuyên viên CSKH BQL]** Soạn tin nhắn phản hồi xác nhận thời gian kỹ thuật viên sẽ có mặt cho cư dân trên App — `⏱ 2 phút`.

👉 **Tổng thời gian vận hành trung bình hiện tại:** **Tổng cộng = 16 phút/lượt** (chưa tính thời gian chờ trong hàng đợi giờ cao điểm lên tới 45–60 phút).

---

## 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | **Chuyên viên Trực Tổng đài BQL (Customer Service Officer)** và **Trưởng ca Kỹ thuật tòa nhà** tại các đại đô thị Vinhomes (quản lý 30.000 – 50.000 căn hộ/khu đô thị). |
| **2. Current Workflow** | Cư dân gửi ticket qua app *Vinhomes Resident* $\rightarrow$ CSKH đọc thủ công theo cơ chế FIFO trên CRM $\rightarrow$ Gọi điện xác minh $\rightarrow$ Chọn danh mục, mức ưu tiên (P1–P3) bằng tay $\rightarrow$ Điều phối cho Tổ kỹ thuật $\rightarrow$ Gõ phản hồi thủ công cho cư dân. |
| **3. Bottleneck** | **Bước đọc hiểu và phân loại ticket thủ công (Bước 2 & 3):** Ngôn ngữ cư dân đa dạng, cảm tính, thiếu thông tin cấu trúc; cơ chế đọc lần lượt (FIFO) khiến ticket nguy hiểm (P1) bị ngâm lâu, đồng thời tỷ lệ gán nhầm bộ phận xử lý lên tới 18%. |
| **4. Business Impact** | - **Vi phạm SLA khẩn cấp:** Thời gian phản hồi sự cố P1 (chập điện, tràn nước) trung bình mất 28 phút (vượt ngưỡng SLA mục tiêu là 10 phút), làm tăng nguy cơ hư hại tài sản.<br>- **Lãng phí nhân sự:** 12 nhân viên CSKH/ca mất hơn 60% thời gian chỉ để đọc và phân loại ticket thủ công (~3.000 ticket/ngày). |
| **5. Success Metric** | - **Tốc độ:** 95% ticket được phân loại tự động và đẩy tới màn hình duyệt trong **dưới 5 giây** (giảm tổng thời gian xử lý của CSKH từ 8 phút xuống **dưới 45 giây/ticket**).<br>- **Độ chính xác:** Đạt **>= 90%** độ khớp về Bộ phận xử lý (Department) và **>= 98% Recall** đối với các sự cố Khẩn cấp (P1). |
| **6. Operational Boundary** | - **AI ĐƯỢC PHÉP:** Phân tích văn bản phản ánh, trích xuất vị trí/loại sự cố, gắn nhãn mức độ ưu tiên (`P1_CRITICAL`, `P2_HIGH`, `P3_NORMAL`), gợi ý bộ phận tiếp nhận và soạn bản nháp phản hồi cư dân.<br>- **AI TUYỆT ĐỐI KHÔNG ĐƯỢC PHÉP:** (1) Tự ý cam kết bồi thường thiệt hại bằng tiền/miễn giảm phí dịch vụ; (2) Đưa ra hướng dẫn cư dân tự sửa chữa hệ thống điện cao áp/phòng cháy chữa cháy; (3) Tự động đóng ticket hoặc hủy yêu cầu của cư dân.<br>- **ĐIỂM CẦN CON NGƯỜI DUYỆT (HITL):** Mọi ticket gắn nhãn `P1_CRITICAL` hoặc có điểm tự tin (`confidence_score`) dưới `0.80` bắt buộc phải hiển thị cảnh báo đỏ để Chuyên viên CSKH xác nhận trong 1 cú click trước khi phát lệnh điều động. |

---

## 3.3. Future-State Flow & AI Fit

### 1. Xác định mức AI Fit (AI-Fit Matrix)
- Giải pháp lựa chọn: **[x] LLM Feature (kết hợp Rule-based Guardrails)** (Không dùng `[ ] Pure Rule` hay `[ ] Autonomous Agentic Loop`).
- **So sánh & Lý giải kỹ thuật:**
  * **Tại sao Rule-based / Regex thất bại?** Cư dân mô tả sự cố bằng ngôn ngữ tự nhiên vô cùng biến thiên (VD: *"trần thạch cao phòng ngủ nhỏ giọt tí tách"* là sự cố Kỹ thuật Nước/Xây dựng chứ không chứa từ khóa "vỡ ống"; hoặc *"mùi khét lẹt ở tủ tổng"* là P1 Điện). Regex cứng nhắc sẽ bỏ sót (False Negative) các ca nguy hiểm hoặc bắt nhầm từ khóa (VD: *"tôi tức nổ đom đóm mắt vì phí gửi xe"* không phải là cháy nổ P1).
  * **Tại sao không dùng Agentic Loop tự trị hoàn toàn?** Quy trình điều phối tòa nhà đòi hỏi độ trễ thấp (dưới 5 giây), chi phí API rẻ và tính xác định cao. Việc để một Agent tự do gọi tool hủy/tạo lệnh công việc trực tiếp vào ERP kỹ thuật mà không qua kiểm soát sẽ gây hỗn loạn lịch trực nếu bị Prompt Injection từ cư dân. Một **Single-turn Structured LLM Call (Gemini 2.5 Flash)** kết hợp **Rule-based Pre/Post-processing** là kiến trúc tối ưu nhất.

### 2. Quy trình Tương lai (Future-State Flow)

1. **[Cư dân gửi Ticket trên App Vinhomes]** $\rightarrow$ Hệ thống tiếp nhận văn bản phản ánh và metadata cư dân.
2. **[⚙️ RULE STEP: Tiền xử lý & Kiểm tra Metadata]**
   * Tự động lấy Mã căn hộ, Tòa, Phân khu từ Profile cư dân.
   * Quét nhanh danh sách từ khóa khẩn cấp cứng (`SOS`, `cháy`, `khói`, `kẹt thang máy`).
3. **[🔵 AI STEP: Gemini 2.5 Flash — Structured Triage]**
   * Phân tích ngữ nghĩa văn bản phản ánh của cư dân.
   * Trả về định dạng JSON chuẩn gồm: `category`, `priority` (`P1_CRITICAL` / `P2_HIGH` / `P3_NORMAL`), `department`, `summary`, `missing_info`, `draft_reply`, `confidence_score`, và `safety_flag`.
4. **[↩️ FALLBACK STEP: Xử lý ngoại lệ & Dự phòng rủi ro]**
   * **Điều kiện kích hoạt:** Nếu API trả về JSON lỗi, quá thời gian chờ (Timeout > 5s), điểm tự tin `confidence_score < 0.80`, hoặc `safety_flag == True`.
   * **Hành động:** Hệ thống tự động chuyển thẳng ticket vào **Hàng đợi Ưu tiên Thủ công (Manual Priority Queue)** ở đầu màn hình CRM của CSKH kèm nhãn *"Cần CSKH kiểm tra ngay"*, đảm bảo không có bất kỳ ticket nào bị rơi rớt.
5. **[🟢 HUMAN STEP (HITL): Chuyên viên CSKH Xác nhận 1-Click]**
   * **Với Ticket `P1_CRITICAL`:** Hiển thị popup cảnh báo đỏ kèm âm thanh trên màn hình trực. Chuyên viên CSKH kiểm tra nhanh nội dung AI tóm tắt và bấm **[Duyệt Điều Phối & Gửi Phản Hồi]**.
   * **Với Ticket `P2_HIGH` / `P3_NORMAL` (có `confidence_score >= 0.90`):** Cho phép Chuyên viên CSKH duyệt nhanh theo lô (Batch Approve) để giải phóng hàng đợi trong giờ cao điểm.
6. **[Hoàn tất Điều phối]** $\rightarrow$ Kỹ thuật viên nhận Phiếu việc chuẩn hóa trên App nội bộ và Cư dân nhận tin nhắn phản hồi dự kiến thời gian xử lý.

---

# 🏁 Phase 5 — EVALUATE: Đánh giá Độ sẵn sàng & Quyết định Đầu tư

### AI Readiness Checklist:
- [x] **1. Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?**
  * *Chứng cứ:* Hệ thống CRM của Vinhomes đã lưu trữ hơn 1,2 triệu lịch sử ticket phản ánh của cư dân trong 3 năm qua cùng nhãn bộ phận xử lý cuối cùng, đủ để trích xuất ngay tập Golden Dataset 500 mẫu phục vụ đánh giá.
- [x] **2. Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
  * *Chứng cứ:* AI chỉ đóng vai trò phân loại, sắp xếp hàng đợi ưu tiên và soạn nháp (Co-pilot). Quyết định điều phối cuối cùng đối với sự cố P1 và tin nhắn gửi tới cư dân vẫn đi qua bước duyệt 1-click của CSKH (HITL) và bộ lọc cấm cam kết tài chính.
- [x] **3. Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?**
  * *Chứng cứ:* Giao diện CRM của nhân viên CSKH chỉ bổ sung thêm các trường đã được AI điền sẵn (Pre-filled fields) giúp họ giảm thao tác gõ phím chứ không bắt họ học một phần mềm mới phức tạp.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
- [x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
- [ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
- [ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

### Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):
> **Quyết định GO (Triển khai Pilot tại 1 phân khu Vinhomes Smart City trong 4 tuần)** dựa trên 3 bằng chứng định lượng:
> 1. **Hiệu quả Chi phí (Unit Economics cực kỳ khả thi):** Sử dụng mô hình **Gemini 2.5 Flash** với độ dài trung bình mỗi ticket khoảng 400 input tokens (System Prompt + Nội dung ticket) và 150 output tokens (JSON). Với ~3.000 ticket/ngày (90.000 ticket/tháng), tổng lượng token hàng tháng chỉ khoảng 36M input tokens và 13.5M output tokens — chi phí API ước tính **dưới 15 USD/tháng** cho một khu đô thị, rẻ hơn hàng trăm lần so với chi phí nhân sự vận hành (~600 giờ công CSKH tiết kiệm được mỗi tháng).
> 2. **Độ trễ và Độ tin cậy kỹ thuật:** Qua thử nghiệm Prototype ở Phase 4 với Gemini 2.5 Flash kèm cơ chế Structured Outputs (JSON Schema), thời gian phản hồi trung bình đạt **1.2 – 1.8 giây/request** (vượt xa SLA mục tiêu dưới 5 giây) và tỷ lệ tuân thủ ranh giới an toàn trước các câu lệnh tấn công (Adversarial Prompts đòi bồi thường tiền, hướng dẫn tự sửa điện) đạt **100%**.
> 3. **Kiểm soát rủi ro vận hành:** Cơ chế kết hợp ngưỡng tự tin (`confidence_score < 0.80` chuyển về Fallback thủ công) và ưu tiên hiển thị `P1_CRITICAL` ngay lập tức giải quyết triệt để điểm nghẽn FIFO mà không tạo ra rủi ro bỏ sót sự cố an toàn tính mạng.