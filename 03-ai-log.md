LAB 02: AI Product Scoping
Ngày: 07/11/2026
Bài toán: Hỗ trợ tài xế Xanh SM khi gặp sự cố sạc pin hoặc hết pin giữa đường.
1. Tôi đã sử dụng AI như thế nào?
Trong buổi học hôm nay, tôi sử dụng AI như một công cụ hỗ trợ tư duy và hoàn thiện bài làm về bài toán vận hành của Xanh SM. Tôi cung cấp bối cảnh: tài xế báo sự cố sạc pin hoặc hết pin giữa đường và cần tìm trạm sạc phù hợp hoặc được hỗ trợ điều phối cứu hộ.

AI hỗ trợ tôi:
- Brainstorm và hệ thống hóa các vấn đề vận hành theo các góc nhìn như công việc lặp lại, tốn thời gian, có thể cải thiện bằng AI và khó khăn của các bên liên quan.
- Chuyển vấn đề đã chọn thành quy trình hiện tại (Current-state Workflow), xác định các bước có khả năng gây tắc nghẽn và các bên tham gia.
- Sắp xếp Problem Statement theo 6 trường: Actor / Operator, Current Workflow, Bottleneck, Business Impact, Success Metric và Operational Boundary.
- So sánh Rule / State-Machine, LLM Feature và Agentic Loop; từ đó trình bày phương án kết hợp Rule Engine, LLM và Human-in-the-loop.
- Soạn bản nháp Future-state Workflow, các tình huống Fallback và nội dung Markdown theo cấu trúc worksheet để tôi dễ rà soát và đưa vào bài nộp.
2. AI đã giúp ích ở điểm nào?
AI giúp tôi bắt đầu từ một vấn đề còn khá rộng và diễn đạt nó thành các bước nghiệp vụ cụ thể hơn: tiếp nhận báo cáo, xác định vị trí xe, tìm trạm sạc, kiểm tra dữ liệu, hướng dẫn tài xế hoặc chuyển sang phương án cứu hộ.

Việc chia nội dung theo từng Phase giúp bài làm có cấu trúc rõ ràng và dễ theo dõi. AI cũng gợi ý cách phân chia trách nhiệm hợp lý: Rule Engine xử lý điều kiện và dữ liệu có cấu trúc; LLM hỗ trợ hiểu nội dung báo cáo và soạn hướng dẫn; điều phối viên kiểm tra, phê duyệt và quyết định các trường hợp cần cứu hộ.

Một điểm hữu ích khác là AI đề xuất các tình huống dự phòng như GPS không chính xác, dữ liệu trạm sạc chưa cập nhật, AI không hiểu yêu cầu hoặc xe không thể tiếp tục di chuyển. Nhờ đó, bài toán không chỉ mô tả luồng hoạt động thuận lợi mà còn xem xét trường hợp hệ thống không đủ thông tin.
3. Hạn chế và rủi ro tôi nhận thấy
- Số liệu có thể chỉ là giả định: Các con số như 15 phút xử lý, 12 phút tại bước nghẽn, mục tiêu dưới 3 phút và tỷ lệ 90% là số liệu giả định dùng để xây dựng đề xuất. AI không cung cấp bằng chứng rằng đây là số liệu vận hành thực tế của Xanh SM.
- Phụ thuộc dữ liệu bên ngoài: Muốn đề xuất trạm sạc đáng tin cậy cần có vị trí xe, loại xe/đầu sạc và dữ liệu trạng thái trạm sạc đủ mới. Nếu dữ liệu thiếu hoặc cũ, AI có thể đưa ra gợi ý không phù hợp.
- Cần phân biệt loại sự cố: Hết pin, lỗi sạc và xe không thể di chuyển là những tình huống khác nhau; không nên mặc định tất cả đều có thể giải quyết bằng cách chỉ đường đến trạm sạc.
- Không nên giao quyền quyết định an toàn cho AI: AI có thể hỗ trợ phân tích và soạn nội dung, nhưng không nên tự xác nhận trụ sạc còn khả năng phục vụ, tự điều phối cứu hộ hoặc tự quyết định phương án trong tình huống nguy hiểm.
- Câu trả lời trôi chảy chưa chắc đúng: Nội dung do AI tạo ra cần được đối chiếu với quy trình thực tế, dữ liệu và yêu cầu của đề bài trước khi sử dụng.
4. Tôi đã rà soát và điều chỉnh như thế nào?
Tôi xác định phạm vi bài toán tập trung vào tài xế Xanh SM báo sự cố sạc pin hoặc hết pin giữa đường. Tôi giữ phương án kết hợp Rule Engine + LLM Feature + Human-in-the-loop thay vì đề xuất một Agent tự vận hành toàn bộ quy trình.

Tôi cũng đưa ranh giới vận hành vào bài: AI chỉ phân tích thông tin, hỗ trợ lọc trạm dựa trên dữ liệu được cung cấp và soạn hướng dẫn; điều phối viên phải kiểm tra phương án; quyết định liên quan đến cứu hộ và an toàn thuộc về con người.

Các số liệu thời gian và KPI được ghi rõ là giả định phục vụ bài tập, cần được xác minh bằng log hoặc dữ liệu vận hành thực tế trước khi dùng để đánh giá hiệu quả. Những gợi ý về trạm sạc cũng phải phụ thuộc vào dữ liệu cập nhật, không được xem là thông tin đã xác nhận nếu hệ thống không có nguồn dữ liệu phù hợp.
5. Bài học rút ra
Qua buổi học, tôi nhận thấy AI phù hợp để làm thought-partner: giúp mở rộng ý tưởng, đặt câu hỏi, tổ chức quy trình và tạo bản nháp nhanh. Tuy nhiên, AI không thay thế việc hiểu nghiệp vụ, kiểm chứng dữ liệu và chịu trách nhiệm về quyết định.

Khi phối hợp với AI, tôi cần:
1. Cung cấp bối cảnh và phạm vi bài toán rõ ràng.
2. Yêu cầu AI nêu giả định thay vì trình bày số liệu ước tính như sự thật.
3. Kiểm tra từng bước workflow và tính khả thi của dữ liệu đầu vào.
4. Xác định rõ AI được làm gì, không được làm gì và khi nào phải chuyển cho con người.
5. Tự đọc, chỉnh sửa và chịu trách nhiệm về nội dung cuối cùng trước khi nộp.
6. Đánh giá cá nhân
Tôi đánh giá AI hỗ trợ tốt ở khâu brainstorm, cấu trúc hóa và viết bản nháp Markdown. Giá trị lớn nhất không phải là để AI quyết định thay tôi, mà là giúp tôi nhìn bài toán có hệ thống hơn và nhận ra các điểm cần kiểm chứng. Tôi vẫn cần chủ động đánh giá tính đúng đắn, tính thực tế và ranh giới an toàn của giải pháp.
Kết luận: Tôi sử dụng AI như một công cụ hỗ trợ tư duy và soạn thảo, không xem câu trả lời của AI là dữ liệu đã được xác minh hay quyết định cuối cùng. Nội dung chỉ nên được đưa vào đề xuất sau khi đã được tôi rà soát và đối chiếu với thực tế.