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

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

---

### 📝 List bài toán của tôi:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **VinFast** | **Lặp lại (Repetitive)** | Đối soát và kiểm toán tự động dòng tiền/hóa đơn sạc pin tại hàng ngàn trụ sạc công cộng (so khớp giữ nợ vi thanh toán, hóa đơn điện lực và dữ liệu trụ sạc realtime). |
| 2 | **Vinhomes** | **Tốn thời gian (Time-consuming)** | Phân loại tự động, trích xuất yêu cầu và tạo đề xuất phản hồi cho các phản ánh/khiếu nại của cư dân (qua app Resident/Ban quản lý) để giảm thời gian xử lý thủ công. |
| 3 | **Vinpearl / VinWonders** | **AI có thể tốt hơn (AI-upgrade)** | Personalization & Conversational Booking Assistant: Thay thế chatbot rập khuôn bằng AI Assistant có khả năng gợi ý lịch trình chơi/đặt vé/khách sạn tối ưu theo thời gian thực và cá nhân hóa nhu cầu gia đình. |
| 4 | **Xanh SM** | **Pain từ người khác (Stakeholder Pain)** | Dự báo nhu cầu theo điểm (Hotspot Demand Forecasting) & Điều phối xe thông minh: Xử lý việc gợi ý sai điểm đón/khu vực đông khách khiến tài xế tốn chi phí di chuyển rỗng (deadhead miles). |
| 5 | **Vinmec** | **Tốn thời gian (Time-consuming)** | Tự động hóa trích xuất dữ liệu lâm sàng & Soạn thảo tóm tắt hồ sơ bệnh án (Discharge Summary) từ ghi chú của bác sĩ để rút ngắn thời gian làm thủ tục xuất viện. |

---

### 💡 Chi tiết chuyên sâu cho từng bài toán:

#### 1. VinFast — Đối soát tự động hóa đơn & dòng tiền sạc điện
- **Nghiệp vụ:** Hằng ngày có hàng trăm nghìn phiên sạc trên toàn quốc. Xử lý lệch dòng tiền giữa app khách hàng, hệ thống thanh toán ngân hàng và lượng điện tiêu thụ thực tế tại trụ sạc tốn nhiều nhân lực kế toán.
- **Tác động AI:** Dùng AI Anomaly Detection phát hiện sai lệch phiên sạc realtime và tự động đối soát chứng từ/hóa đơn GTGT điện lực.

#### 2. Vinhomes — Tự động hóa xử lý Feedback & Ticket cư dân
- **Nghiệp vụ:** BQL nhận hàng ngàn phản hồi mỗi ngày (tiếng ồn, sửa chữa, vệ sinh, phí dịch vụ). Nhân viên tốn 15–30 phút/ticket để đọc, phân loại, giao đúng bộ phận và viết phản hồi chuẩn mực.
- **Tác động AI:** LLM phân loại mức độ khẩn cấp, tự động route ticket đến đúng kỹ thuật viên và soạn sẵn phản hồi chuẩn hóa cho BQL duyệt chỉ với 1 click.

#### 3. Vinpearl — Chatbot CSKH & Tư vấn đặt dịch vụ thế hệ mới
- **Nghiệp vụ:** Chatbot cũ dựa trên quy tắc (rule-based), phản hồi rập khuôn khi khách hỏi lịch trình phức tạp (*"Gia đình 4 người có con nhỏ chơi VinWonders 2 ngày nên đi những trò nào?"*).
- **Tác động AI:** RAG + LLM đóng vai trò Chuyên gia tư vấn du lịch cá nhân hóa, tự động chốt đơn (up-sell/cross-sell) dịch vụ ăn uống, khách sạn và vé vui chơi.

#### 4. Xanh SM — Tối ưu vị trí chờ đón cho Tài xế (Smart Dispatch & Hotspot)
- **Nghiệp vụ:** Tài xế di chuyển tự do hoặc theo gợi ý tĩnh, dẫn đến tình trạng dư thừa xe ở khu vực vắng và thiếu xe ở vùng cầu cao (sân bay, sự kiện, mưa lớn), gây tốn pin và giảm thu nhập tài xế.
- **Tác động AI:** Mô hình dự báo chuỗi thời gian (Time-series) kết hợp Spatial AI để dự báo nhu cầu chuyến đi trước 15–30 phút, điều hướng tài xế đón khách tối ưu.

#### 5. Vinmec — Tóm tắt bệnh án & Trích xuất dữ liệu y tế
- **Nghiệp vụ:** Bác sĩ và điều dưỡng tốn nhiều giờ nhập liệu thủ công từ kết quả xét nghiệm, chẩn đoán hình ảnh và ghi chú thăm khám để ra tóm tắt nhập/xuất viện.
- **Tác động AI:** Medical NLP trích xuất thông tin quan trọng từ hồ sơ EHR, tổng hợp báo cáo y khoa sơ bộ để bác sĩ thẩm định lại nhanh chóng.

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo cáo sự cố sạc pin / hết pin    │
│ giữa đường cần điều phối cứu hộ hoặc trạm sạc gần nhất.     │
│ Công ty thành viên: [x] Xanh SM (GSM)   [ ] VinFast         │
│                     [ ] Vinhomes        [ ] Vinmec          │
│                                                             │
│ Ai đang đau? Tài xế (chờ đợi), Điều phối viên (quá tải)     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài điều vận báo hết pin               │
│   → 2. Điều phối viên tra cứu thủ công vị trí xe trên bản đồ│
│   → 3. Tra cứu thủ công các trạm sạc VinFast còn trụ trống   │
│   → 4. Viết tin nhắn chỉ dẫn/đường đi gửi qua App tài xế    │
│   → 5. Liên hệ đội xe cứu hộ nếu xe đã cạn kiệt pin         │
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 12 phút/lượt)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (Tự động hóa lấy vị trí -> Tra cứu trạm trống -> Draft tin) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.      │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Tự động soạn chỉ dẫn)  │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Ban quản lý Vinhomes tiếp nhận, phân loại và soạn │
│ phản hồi cho hàng ngàn khiếu nại/yêu cầu dịch vụ từ cư dân. │
│ Công ty thành viên: [ ] Xanh SM (GSM)   [ ] VinFast         │
│                     [x] Vinhomes        [ ] Vinmec          │
│                                                             │
│ Ai đang đau? Nhân viên BQL (ngợp tin nhắn), Cư dân (chờ lâu)│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh qua App (kèm hình ảnh/mô tả rối rắm)│
│   → 2. Nhân viên đọc thủ công để phân loại lỗi/vấn đề       │
│   → 3. Chuyển ticket sang đúng phòng ban kỹ thuật/vệ sinh   │
│   → 4. Soạn thảo email/tin nhắn phản hồi tiêu chuẩn cho cư dân│
│                                                             │
│ Bước nào tốn nhất? Bước 2 & 4 (⏱ 15-20 phút/ticket)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 4            │
│ (Tự động phân loại độ khẩn cấp & Draft sẵn phản hồi chuẩn)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian phản hồi đầu tiên (First Response Time)      │
│ từ 4 giờ ──> dưới 15 phút.                                  │
│                                                             │
│ Quick Architecture: [x] LLM (Phân loại intent & Draft text) │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Bác sĩ và điều dưỡng tốn nhiều thời gian tổng hợp │
│ kết quả cận lâm sàng để viết tóm tắt bệnh án xuất viện.     │
│ Công ty thành viên: [ ] Xanh SM (GSM)   [ ] VinFast         │
│                     [ ] Vinhomes        [x] Vinmec          │
│                                                             │
│ Ai đang đau? Bác sĩ điều trị (quá tải hành chính), Bệnh nhân│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Tổng hợp kết quả xét nghiệm, CĐHA, y lệnh từ hệ thống EHR│
│   → 2. Đọc thủ công toàn bộ lịch sử điều trị trong đợt viện │
│   → 3. Soạn thảo văn bản Tóm tắt bệnh án theo form chuẩn    │
│   → 4. Bác sĩ trưởng khoa kiểm tra, ký duyệt hồ sơ xuất viện │
│                                                             │
│ Bước nào tốn nhất? Bước 2 & 3 (⏱ 30-45 phút/bệnh nhân)     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 2 & 3         │
│ (Trích xuất dữ liệu EHR và tự động generate Discharge Summary)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian hoàn tất hồ sơ xuất viện từ 45 phút          │
│ ──> dưới 10 phút/bệnh nhân.                                 │
│                                                             │
│ Quick Architecture: [x] Agent / Medical LLM (NLP trích xuất)│
└─────────────────────────────────────────────────────────────┘

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

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
