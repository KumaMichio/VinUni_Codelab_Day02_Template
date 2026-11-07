# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Dùng **4 Lenses** quét qua vận hành của công ty thành viên **Xanh SM**, tập trung tìm kiếm các quy trình thủ công, tốn thời gian và gây rò rỉ hiệu suất.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Nhân viên điều phối tiếp nhận và xử lý thủ công các chuyến xe bất thường như khách đổi điểm đến, hủy chuyến hoặc tài xế không thể tiếp tục chuyến (ước tính 8 phút/trường hợp). |
| 2 | **Xanh SM** | Lặp lại | Nhân viên tổng đài tiếp nhận thông tin sự cố từ tài xế qua cuộc gọi/tin nhắn, xác minh tình trạng xe và chuyển yêu cầu đến bộ phận phụ trách (ước tính 12 phút/trường hợp). |
| 3 | **Xanh SM** | AI-upgrade | Điều phối viên phải kiểm tra thủ công dung lượng pin, vị trí xe, tình trạng trạm sạc và nhu cầu đặt xe để đề xuất địa điểm, thời điểm sạc phù hợp. |
| 4 | **Xanh SM** | Lặp lại | Nhân viên vận hành theo dõi lịch bảo dưỡng, tiếp nhận yêu cầu sửa chữa, kiểm tra lịch trống của xưởng và phối hợp với tài xế (ước tính 5 phút/yêu cầu). |
| 5 | **Xanh SM** | Pain từ người khác | Nhân viên chăm sóc khách hàng phải đọc từng phản hồi, phân loại khiếu nại và tổng hợp nguyên nhân để báo cáo; quy trình thủ công có thể mất khoảng 10 phút/phản hồi. |


# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)
# PHASE 2 — QUICK-ASSESS
## QUICK PROBLEM CARD #1

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo cáo sự cố như hết pin, lỗi xe  │
│ hoặc va chạm cần được xác minh và chuyển đến bộ phận xử lý. │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (chờ hỗ trợ), Điều phối viên (quá tải)  │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài báo cáo sự cố                       │
│   → 2. Nhân viên tiếp nhận và ghi lại thông tin              │
│   → 3. Điều phối viên xác minh vị trí và tình trạng xe       │
│   → 4. Phân loại sự cố và tìm bộ phận hỗ trợ phù hợp         │
│   → 5. Liên hệ cứu hộ hoặc hướng dẫn tài xế xử lý            │
│                                                             │
│ Bước nào tốn nhất? Bước 2-4 (⏱ 12 phút/lượt, ước tính)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4              │
│ (Chuyển giọng nói thành văn bản -> Phân loại sự cố ->        │
│ Trích xuất vị trí và thông tin -> Đề xuất bộ phận xử lý)    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian tiếp nhận và phân loại sự cố từ 12 phút      │
│ xuống dưới 3 phút/lượt.                                     │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (Tự động phân loại sự cố, trích xuất thông tin và đề xuất    │
│ hướng xử lý; nhân viên xác nhận trước khi điều phối.)        │
└─────────────────────────────────────────────────────────────┘


## QUICK PROBLEM CARD #2

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Điều phối xe đến trạm sạc phù hợp dựa trên dung   │
│ lượng pin, vị trí xe, tình trạng trạm sạc và nhu cầu chuyến │
│ đi tiếp theo.                                                │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (mất thời gian tìm trạm), Điều phối viên│
│ (khó tối ưu hoạt động đội xe).                              │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế hoặc hệ thống ghi nhận xe sắp hết pin            │
│   → 2. Nhân viên kiểm tra vị trí và dung lượng pin           │
│   → 3. Tra cứu các trạm sạc gần đó và tình trạng trạm        │
│   → 4. So sánh khoảng cách, thời gian chờ và nhu cầu xe      │
│   → 5. Thông báo trạm sạc phù hợp cho tài xế                │
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 10 phút/lượt, ước tính)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (Phân tích dữ liệu trạm sạc -> Dự báo khả năng chờ ->       │
│ Đề xuất trạm sạc tối ưu theo vị trí và nhu cầu vận hành.)   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian tìm kiếm và lựa chọn trạm sạc từ 10 phút     │
│ xuống dưới 2 phút/lượt.                                     │
│                                                             │
│ Quick Architecture: [x] Agent                               │
│ (Agent thu thập dữ liệu vị trí, pin và trạm sạc, đề xuất    │
│ phương án phù hợp; điều phối viên hoặc tài xế xác nhận.)    │
└─────────────────────────────────────────────────────────────┘


## QUICK PROBLEM CARD #3

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Nhân viên chăm sóc khách hàng phải đọc và phân    │
│ loại thủ công lượng lớn phản hồi, khiếu nại để xác định     │
│ nguyên nhân và tổng hợp báo cáo vận hành.                   │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Nhân viên CSKH (khối lượng công việc lớn),     │
│ Quản lý vận hành (khó phát hiện vấn đề lặp lại).            │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tiếp nhận phản hồi từ ứng dụng, email hoặc tổng đài     │
│   → 2. Đọc nội dung từng phản hồi của khách hàng             │
│   → 3. Phân loại theo nhóm: tài xế, giá cước, xe, ứng dụng   │
│   → 4. Chuyển khiếu nại đến bộ phận phụ trách                │
│   → 5. Tổng hợp số liệu và lập báo cáo định kỳ               │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 10 phút/phản hồi, ước tính)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-5              │
│ (LLM đọc phản hồi -> Phân loại nguyên nhân -> Tóm tắt vấn đề│
│ -> Tạo báo cáo thống kê tự động.)                            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian đọc và phân loại phản hồi từ 10 phút         │
│ xuống dưới 2 phút/phản hồi.                                 │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (Tự động phân loại, tóm tắt phản hồi và tạo báo cáo;        │
│ nhân viên CSKH kiểm tra trước khi chuyển xử lý.)            │
└─────────────────────────────────────────────────────────────┘

---

## GHI CHÚ VỀ SỐ LIỆU

- Các mốc thời gian là giả định ban đầu để xây dựng mục tiêu AI, chưa phải số liệu nội bộ Xanh SM xác nhận.
- Cần đo thời gian xử lý thực tế trước khi triển khai để xác định mức cải thiện.
- Các quyết định liên quan đến cứu hộ, sự cố nghiêm trọng và điều phối phương tiện cần có nhân viên xác nhận.
# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Ai đang thực hiện tác vụ hằng ngày? |
| **2. Current Workflow** | Mô tả tóm tắt quy trình thủ công hiện tại và công cụ sử dụng. |
| **3. Bottleneck** | Bước nào chậm, lỗi, hoặc cần xử lý ngôn ngữ tự động nhiều nhất? |
| **4. Business Impact** | Tổn thất thực tế đo bằng thời gian, chi phí, hoặc SLA của Vingroup. |
| **5. Success Metric** | AI giải quyết được thì đạt ngưỡng số mấy? (Ví dụ: *"85% vé được phân loại dưới 10s"*). |
| **6. Operational Boundary** | AI được phép làm gì, TUYỆT ĐỐI không được làm gì, điểm nào cần duyệt? |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [ ] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
