# 02 — Deep-Dive Report (Phase 3, Nhóm, 85 min)

**Bài toán:** Tài xế Xanh SM báo cáo sự cố hết pin giữa đường, cần điều phối xe cứu hộ hoặc chỉ đường đến trạm sạc gần nhất.
**Công ty thành viên:** Xanh SM (hệ sinh thái VinFast)

> ⚠️ Các con số thời gian, tần suất và chi phí trong báo cáo là **ước lượng giả định** để phục vụ phân tích. Cần xác thực bằng dữ liệu thực hoặc phỏng vấn điều phối viên trước khi dùng làm cam kết.

---

## 3.1. Current-State Workflow Mapping (25 min)

**Ký hiệu:** 🔴 Bottleneck · 🔄 Handoff

### Sơ đồ quy trình hiện tại

```mermaid
flowchart LR
    A["1. Tài xế phát hiện pin cạn / xe dừng"] --> B["2. Gọi hotline / nhắn điều phối 🔄"]
    B --> C["3. Điều phối hỏi lại thông tin 🔴"]
    C --> D["4. Tra trạm sạc / đội cứu hộ gần nhất 🔴"]
    D --> E["5. Liên hệ đội cứu hộ hoặc trạm 🔄"]
    E --> F["6. Báo lại tài xế phương án + ETA 🔄"]
    F --> G["7. Theo dõi và đóng ticket"]
```

### Bảng chi tiết từng bước

| # | Bước | Người/Hệ thống | Công cụ hiện tại | Thời gian (ước lượng) | Ký hiệu |
|---|------|----------------|------------------|-----------------------|---------|
| 1 | Tài xế nhận ra pin cạn, xe dừng hoặc sắp dừng | Tài xế | App tài xế, đồng hồ xe | 1 phút | |
| 2 | Tài xế gọi hotline hoặc nhắn tin cho điều phối, chờ có người nhận | Tài xế → Điều phối viên | Điện thoại, chat | 2 phút | 🔄 |
| 3 | Điều phối hỏi lại: vị trí, biển số/mã xe, % pin, có khách trên xe không | Điều phối viên | Lời nói/chat, ghi tay | 4 phút | 🔴 |
| 4 | Tra trạm sạc trống gần nhất và đội cứu hộ khả dụng gần nhất | Điều phối viên | Bản đồ, hệ thống trạm sạc, danh sách đội cứu hộ (nhiều nguồn tách rời) | 5 phút | 🔴 |
| 5 | Gọi đội cứu hộ hoặc xác nhận với trạm sạc | Điều phối viên → Đội cứu hộ/Trạm | Điện thoại | 4 phút | 🔄 |
| 6 | Báo lại tài xế phương án và thời gian dự kiến | Điều phối viên → Tài xế | Điện thoại/chat | 2 phút | 🔄 |
| 7 | Theo dõi tiến độ, ghi nhận và đóng ticket | Điều phối viên | Sổ/hệ thống ticket | 2 phút | |
| | **Tổng cộng** | | | **≈ 20 phút/lượt** | |

### Điểm nghẽn chính

- **🔴 Bước 3:** thông tin tài xế cung cấp thường thiếu, mơ hồ hoặc nói bằng lời tự nhiên ("xe em hết pin gần cầu Thanh Trì, còn khách"). Điều phối viên phải hỏi đi hỏi lại, trong khi tài xế đang căng thẳng, có thể đang có khách.
- **🔴 Bước 4:** dữ liệu nằm ở nhiều nơi (vị trí xe, trạng thái trạm, đội cứu hộ), điều phối viên tra thủ công và không có ước tính nhanh xe còn chạy được bao xa với lượng pin hiện tại.
- **🔄 Các điểm handoff (2, 5, 6):** mỗi lần chuyển giao bằng lời nói/điện thoại đều có nguy cơ sai địa điểm, sai xe, hoặc mất thời gian chờ máy.

**Tổng thời gian vận hành trung bình: ≈ 20 phút/lượt.**

---

## 3.2. Problem Statement (6-field) & Metrics (15 min)

| Field | Nội dung chi tiết |
|-------|-------------------|
| **1. Actor / Operator** | **Điều phối viên Xanh SM** (người xử lý sự cố hằng ngày) và **tài xế Xanh SM** (người báo sự cố, thường đang ở ngoài đường, có thể đang chở khách). |
| **2. Current Workflow** | Tài xế gọi/nhắn điều phối, điều phối hỏi lại thông tin, tra thủ công trạm sạc và đội cứu hộ gần nhất trên nhiều công cụ rời rạc, gọi điện xác nhận, rồi báo lại tài xế. Toàn bộ dựa vào lời nói và điện thoại, tổng ≈ 20 phút/lượt. |
| **3. Bottleneck** | (a) Bước 3: thu thập và chuẩn hóa thông tin từ lời tài xế (vị trí, mã xe, % pin, có khách). (b) Bước 4: tra cứu thủ công trạm sạc/đội cứu hộ phù hợp. Đây là nơi cần xử lý ngôn ngữ tự nhiên và tổng hợp dữ liệu nhiều nhất. |
| **4. Business Impact** | - Xe dừng giữa đường kéo dài thời gian mất doanh thu của tài xế và xe (xe không chạy được cuốc mới). <br>- Khách bị kẹt lâu, ảnh hưởng trải nghiệm và uy tín thương hiệu, rủi ro hủy chuyến/khiếu nại. <br>- Điều phối viên bị chiếm dụng thời gian cho các cuộc gọi lặp lại, giảm khả năng xử lý đồng thời nhiều sự cố. <br>- Ví dụ ước lượng: nếu 100 sự cố/ngày × 20 phút ≈ 33 giờ công điều phối/ngày (giả định, cần xác thực). |
| **5. Success Metric** | - Thời gian từ lúc tài xế báo đến lúc có **phương án đề xuất**: 12 phút ──> **under 2 phút**. <br>- Tổng thời gian từ báo cáo đến tài xế nhận phương án đã duyệt: 20 phút ──> **under 6 phút**. <br>- Trích xuất đúng thông tin cốt lõi (vị trí, mã xe, % pin, có khách): **≥ 95%**. <br>- Phương án gợi ý được điều phối viên duyệt mà không cần sửa lớn: **≥ 85%**. <br>- Gợi ý trạm sạc không khả dụng/không thể tới được: **< 3%**. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** trích xuất thông tin từ tin nhắn/giọng nói tài xế; ước tính quãng đường còn đi được; đề xuất danh sách trạm sạc/đội cứu hộ gần nhất đã xếp hạng; soạn sẵn tin nhắn gửi tài xế và đội cứu hộ. <br>**AI TUYỆT ĐỐI KHÔNG:** tự động điều đội cứu hộ hoặc xác nhận với trạm khi chưa có người duyệt; đưa ra hướng dẫn kỹ thuật/an toàn về xe hoặc pin (ví dụ: tự xử lý sự cố pin); tự quyết định đối với trường hợp khẩn cấp có người gặp nguy hiểm; bịa vị trí hoặc trạng thái trạm khi dữ liệu thiếu. <br>**CẦN DUYỆT (HITL):** mọi lệnh điều cứu hộ và phương án cuối gửi cho tài xế do điều phối viên phê duyệt. Mọi ca có khách trên xe, có yếu tố nguy hiểm (đường cao tốc, tai nạn, khói/cháy) hoặc AI không tự tin phải **chuyển ngay cho điều phối viên/quy trình khẩn cấp**. |

---

## 3.3. Future-State Flow & AI Fit (25 min)

### AI-Fit Matrix

| Nhóm giải pháp | Chọn |
|----------------|------|
| Rule / State-Machine | [X] (lõi: tính khoảng cách, lọc trạm khả dụng, xếp hạng, quy tắc khẩn cấp) |
| LLM Feature | [X] (hiểu và trích xuất thông tin từ tin nhắn/giọng nói tự nhiên của tài xế, soạn tin phản hồi) |
| Agentic Loop | [ ] |

**Lý do chọn (Problem First, AI Second):** phần lớn quyết định (trạm nào gần, còn trống, xe đi tới được không) là **xác định được bằng rule và dữ liệu**, không cần LLM. LLM chỉ dùng ở điểm thực sự cần ngôn ngữ tự nhiên: biến lời tài xế thành dữ liệu có cấu trúc và soạn tin nhắn. **Không cần Agentic Loop** vì hành động có hậu quả (điều cứu hộ) phải do con người duyệt, và quy trình ngắn, ít nhánh.

### Future-State Flow

**Ký hiệu:** 🔵 AI Step · 🟢 Human Step (HITL) · ↩️ Fallback · ⚙️ Rule/hệ thống

```mermaid
flowchart TD
    A["Tài xế báo sự cố: nút SOS / tin nhắn / giọng nói"] --> B["🔵 LLM trích xuất: vị trí, mã xe, % pin, có khách, mức khẩn cấp"]
    B --> C{"⚙️ Đủ thông tin và độ tin cậy cao?"}
    C -- "Không" --> F1["↩️ Hỏi lại tài xế 1 câu ngắn bằng mẫu có sẵn; nếu vẫn thiếu chuyển điều phối viên"]
    F1 --> H
    C -- "Có" --> D{"⚙️ Có khách / yếu tố nguy hiểm?"}
    D -- "Có" --> E["↩️ Chuyển NGAY điều phối viên, kích hoạt quy trình khẩn cấp"]
    D -- "Không" --> G["⚙️ Rule: ước tính quãng đường còn đi được từ % pin; lọc trạm sạc trống trong tầm với, xếp hạng"]
    G --> I{"Có trạm tới được?"}
    I -- "Có" --> J["🔵 LLM soạn tin: hướng dẫn tài xế đi tới trạm"]
    I -- "Không" --> K["⚙️ Rule: chọn đội cứu hộ khả dụng gần nhất"]
    K --> L["🔵 LLM soạn tin: tài xế và đội cứu hộ"]
    J --> H["🟢 Điều phối viên xem đề xuất, sửa nếu cần, phê duyệt"]
    L --> H
    E --> H
    H --> M["Gửi phương án + ETA cho tài xế (và điều cứu hộ nếu cần)"]
    M --> N["Theo dõi và đóng ticket, ghi log để cải thiện"]
```

### Tóm tắt các loại bước

| Loại | Bước |
|------|------|
| 🔵 **AI Step** | Trích xuất thông tin từ tin nhắn/giọng nói tài xế; soạn tin nhắn gửi tài xế và đội cứu hộ. |
| ⚙️ **Rule/Hệ thống** | Ước tính quãng đường còn đi được; lọc và xếp hạng trạm sạc/đội cứu hộ; kiểm tra điều kiện khẩn cấp. |
| 🟢 **Human Step (HITL)** | Điều phối viên duyệt và phê duyệt phương án cuối trước khi gửi/điều cứu hộ; xử lý trực tiếp mọi ca khẩn cấp. |
| ↩️ **Fallback** | Thiếu thông tin hoặc LLM không tự tin ──> hỏi lại bằng mẫu hoặc chuyển điều phối viên. LLM lỗi/timeout ──> quay về quy trình thủ công hiện tại (không chặn xử lý sự cố). Không có trạm/đội cứu hộ khả dụng ──> báo điều phối viên xử lý thủ công. Có khách/nguy hiểm ──> chuyển ngay, bỏ qua tự động hóa. |

### Thời gian dự kiến sau khi có AI (ước lượng)

| Bước | Hiện tại | Tương lai |
|------|----------|-----------|
| Thu thập thông tin (bước 3) | 4 phút | ~ 30 giây (LLM trích xuất + hỏi lại tối đa 1 lần) |
| Tra trạm/cứu hộ (bước 4) | 5 phút | ~ 15 giây (rule tự động) |
| Duyệt và liên hệ (bước 5-6) | 6 phút | ~ 2-3 phút (duyệt + gửi tin soạn sẵn) |
| **Tổng** | **≈ 20 phút** | **≈ 5-6 phút** |

### Rủi ro chính và cách giảm thiểu

| Rủi ro | Giảm thiểu |
|--------|------------|
| LLM hiểu sai vị trí hoặc mã xe | Chỉ dùng LLM để trích xuất; đối chiếu vị trí với GPS thực tế và mã xe với danh sách đội xe; sai lệch thì hỏi lại hoặc chuyển người. |
| Ước tính quãng đường còn đi được sai | Dùng biên an toàn (buffer) trong rule; ưu tiên cứu hộ khi sát ngưỡng. |
| Dữ liệu trạng thái trạm sạc cũ | Hiển thị thời điểm cập nhật; điều phối viên xác nhận khi cần. |
| Phụ thuộc quá mức vào AI | Điều phối viên luôn là bước duyệt cuối; có đường lui về quy trình thủ công. |


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