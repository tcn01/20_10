# Worksheet 0–5 — Đề tài 4: Manufacturing Maintenance Copilot

## Thông tin chủ đề
- **Tên nhóm:** [Điền tên nhóm]
- **Chủ đề:** Scenario 4 – Manufacturing Maintenance Copilot
- **Loại chủ đề:** Scenario card

---

# Worksheet 0 — Learning Timeline

## 1) 2–3 kỹ năng nhóm tự tin nhất
- Phân tích bài toán AI agent trong bối cảnh doanh nghiệp
- Thiết kế luồng RAG/tra cứu tài liệu kỹ thuật
- Đánh giá deployment, cost và reliability ở mức MVP

## 2) Mô tả ngắn sản phẩm/scenario được chọn
Manufacturing Maintenance Copilot là một AI assistant hỗ trợ kỹ thuật viên và đội vận hành trong nhà máy tra cứu SOP, manual máy móc, log bảo trì, lịch vận hành và checklist an toàn để xử lý sự cố nhanh hơn, đúng quy trình hơn và giảm phụ thuộc vào việc hỏi người có kinh nghiệm.

## 3) Chủ đề chốt dùng xuyên suốt cả ngày
**Manufacturing Maintenance Copilot**

## 4) Sản phẩm này giải quyết bài toán gì?
Giải quyết bài toán tra cứu tri thức vận hành phân tán và phản hồi chậm trong môi trường nhà máy. Hiện trạng thường là SOP nằm rải rác, log bảo trì nằm ở nhiều hệ thống cũ, checklist an toàn ở dạng file/tài liệu khác nhau, khiến kỹ thuật viên mất thời gian tìm thông tin khi máy có lỗi hoặc cần bảo trì định kỳ.

## 5) Ai là người dùng chính?
- Kỹ thuật viên bảo trì
- Quản đốc xưởng
- Team vận hành

## 6) Vì sao chủ đề này phù hợp để phân tích deployment và cost?
Vì đây là bài toán enterprise điển hình: có hệ thống legacy, có khu vực mạng nội bộ không cho internet, đòi hỏi ổn định cao, và môi trường thiết bị/mạng không đồng đều. Nhờ vậy nhóm có đủ chất liệu để tranh luận về On-prem vs Hybrid, cost hạ tầng, cost tích hợp, cost reliability và khả năng edge deployment.

---

# Worksheet 1 — Enterprise Deployment Clinic

## 1) Bối cảnh tổ chức/khách hàng
Khách hàng là một nhà máy sản xuất có nhiều dây chuyền, nhiều thiết bị công nghiệp, quy trình bảo trì định kỳ và xử lý sự cố vận hành. Tổ chức đã có một số hệ thống sẵn có như CMMS/MES/ERP hoặc kho tài liệu nội bộ, nhưng dữ liệu phân tán và mức độ số hóa không đồng đều.

## 2) Dữ liệu hệ thống sẽ động đến
- SOP vận hành và bảo trì
- Manual máy móc
- Log bảo trì
- Lịch vận hành
- Checklist an toàn
- Ticket sự cố
- Hướng dẫn xử lý theo mã lỗi
- Nhật ký ca trực
- Danh sách phụ tùng thay thế
- Lịch sử downtime theo máy

## 3) Mức độ nhạy cảm của dữ liệu
**Mức độ nhạy cảm: trung bình đến cao**

### Lý do
- Không nhạy cảm như dữ liệu y tế hay ngân hàng, nhưng vẫn là dữ liệu vận hành cốt lõi.
- SOP, manual nội bộ, log lỗi và quy trình an toàn có thể là tài sản vận hành quan trọng.
- Nếu lộ ra ngoài có thể ảnh hưởng bảo mật quy trình, bí quyết sản xuất hoặc an toàn vận hành.

## 4) Ba ràng buộc enterprise lớn nhất
1. Nhiều hệ thống legacy  
2. Có khu vực mạng nội bộ không cho internet  
3. Cần độ ổn định cao  

## 5) Chọn mô hình triển khai
**Đề xuất: Hybrid thiên về On-prem**

## 6) Hai lý do vì sao chọn mô hình đó

### Lý do 1: Phù hợp với vùng mạng nội bộ không cho internet
Một phần hệ thống bắt buộc phải chạy trong mạng nội bộ nhà máy để kỹ thuật viên vẫn truy cập được SOP, manual và log ngay cả ở khu vực hạn chế internet. Điều này khiến mô hình cloud-only kém phù hợp.

### Lý do 2: Dễ cân bằng giữa bảo mật và khả năng mở rộng
- Thành phần truy xuất dữ liệu nhạy cảm, connector tới hệ thống legacy và cache tri thức nội bộ nên đặt on-prem.
- Một số dịch vụ ít nhạy cảm hơn như analytics tổng hợp, dashboard quản trị hoặc model API không chứa dữ liệu thô có thể đặt ở cloud nếu policy cho phép.

## 7) Kết luận ngắn
- Không nên cloud-only
- Nên hybrid hoặc on-prem-first
- Edge deployment đáng cân nhắc cho các khu vực mạng yếu hoặc thiết bị tại xưởng

---

# Worksheet 2 — Cost Anatomy Lab

## 1) Ước lượng user/ngày, request/ngày, peak traffic

### Giả định MVP cho 1 nhà máy quy mô vừa
- 40 kỹ thuật viên bảo trì
- 8 quản đốc / vận hành
- 12 người dùng gián tiếp khác
- **Tổng user hoạt động/ngày:** ~60

### Ước lượng usage
- Trung bình 8 request/người/ngày
- **Tổng request/ngày:** ~480 request/ngày

### Peak traffic
- Tập trung vào đầu ca, cuối ca, hoặc khi phát sinh sự cố
- Peak đồng thời: 15–25 request trong 5–10 phút

## 2) Ước lượng input/output tokens nếu dùng LLM API

### Mỗi request kiểu RAG
- Input: câu hỏi + system prompt + context từ SOP/manual/log  
  ≈ 2,000–4,000 tokens
- Output: câu trả lời ngắn + checklist/gợi ý thao tác  
  ≈ 300–800 tokens

### Ước lượng trung bình/request
- Input: 3,000 tokens
- Output: 500 tokens

### Tổng/ngày
- Input: 480 × 3,000 = **1,440,000 tokens**
- Output: 480 × 500 = **240,000 tokens**

## 3) Các lớp cost

### a) Token / Model inference
- Chi phí gọi LLM/API hoặc self-hosted inference
- Là lớp cost thấy rõ nhất nhưng không phải duy nhất

### b) Compute
- Server on-prem cho retrieval, embedding, reranking
- Máy edge hoặc gateway nội bộ
- GPU/CPU nếu tự host model nhỏ

### c) Storage
- Lưu SOP/manual/log
- Vector DB / search index
- Backup và versioning tài liệu

### d) Human review
- Chuyên gia vận hành xác minh nội dung trả lời quan trọng
- Nhân sự cập nhật knowledge base khi SOP thay đổi
- Chi phí đào tạo người dùng

### e) Logging / Monitoring / Audit
- Ghi log truy vấn
- Quan sát độ trễ, lỗi, tỷ lệ fallback
- Audit truy cập tài liệu nội bộ

### f) Maintenance / Integration
- Chi phí kết nối hệ thống legacy
- Bảo trì pipeline ingest tài liệu
- Chi phí vận hành định kỳ, patch, bảo mật

## 4) Cost MVP sơ bộ

### Phương án MVP đề xuất
- 1 ứng dụng nội bộ web/chat
- 1 pipeline ingest tài liệu
- 1 kho index/search
- 1 lớp LLM inference
- 1 lớp log/monitor cơ bản

### Ước lượng cơ cấu cost MVP/tháng
- Model/API/inference: **30–40%**
- Tích hợp và bảo trì hệ thống: **20–25%**
- Hạ tầng compute/search/index: **15–20%**
- Nhân sự cập nhật tri thức + human review: **15–20%**
- Logging, monitoring, backup, bảo mật: **10–15%**

## 5) Khi số user tăng 5x hoặc 10x, phần nào tăng mạnh nhất?
1. **Inference/token cost** nếu vẫn dùng LLM cho hầu hết request  
2. **Chi phí reliability/hạ tầng** vì phải chịu tải giờ cao điểm  
3. **Chi phí maintenance và knowledge ops** khi số tài liệu, thiết bị, dòng máy tăng lên

## 6) Cost driver lớn nhất là gì?
**Cost driver lớn nhất:** inference + retrieval cho các truy vấn vận hành thực tế, đặc biệt nếu nhiều request lặp nhưng chưa có cache tốt.

## 7) Hidden cost dễ bị quên nhất là gì?
- Tích hợp hệ thống legacy
- Dọn và chuẩn hóa tài liệu kỹ thuật
- Human review cho câu trả lời liên quan an toàn
- Chi phí downtime khi hệ thống AI không ổn định

## 8) Nhóm đang ước lượng quá lạc quan ở đâu?
- Giả định tài liệu SOP/manual đã sạch và dễ ingest
- Giả định log bảo trì có cấu trúc tốt
- Giả định kỹ thuật viên luôn dùng chat thay cho gọi người có kinh nghiệm
- Giả định mạng xưởng đủ ổn định cho mọi khu vực

---

# Worksheet 3 — Cost Optimization Debate

## Chiến lược 1 — Semantic caching cho câu hỏi lặp lại

### Tiết kiệm phần nào
Giảm số lần gọi model/retrieval cho các câu hỏi lặp như:
- “Mã lỗi E17 xử lý thế nào?”
- “Checklist an toàn trước khi bảo trì máy A?”
- “SOP thay dầu định kỳ ở đâu?”

### Lợi ích
- Giảm latency
- Giảm cost inference
- Ổn định trải nghiệm ở giờ cao điểm

### Trade-off
- Cần cơ chế invalidate khi SOP cập nhật
- Nếu cache cũ có thể trả thông tin lỗi thời

### Thời điểm áp dụng
**Làm ngay từ MVP**, vì nhà máy có nhiều câu hỏi lặp theo ca và theo dòng máy.

---

## Chiến lược 2 — Model routing

### Tiết kiệm phần nào
Chỉ dùng model mạnh/đắt cho câu hỏi phức tạp; câu hỏi tra cứu chuẩn, FAQ, checklist thì dùng model nhỏ hơn hoặc rule-based retrieval.

### Lợi ích
- Giảm mạnh cost bình quân/request
- Dễ scale hơn khi tăng số người dùng
- Giữ chất lượng ở các case quan trọng

### Trade-off
- Phải có cơ chế phân loại request
- Routing sai có thể làm giảm chất lượng trả lời

### Thời điểm áp dụng
Áp dụng sớm sau khi có dữ liệu log sử dụng 2–4 tuần.

---

## Chiến lược 3 — Smaller/self-hosted model cho mạng nội bộ

### Tiết kiệm phần nào
Với các tác vụ cố định như tóm tắt log lỗi, trích SOP liên quan, chuẩn hóa câu trả lời nội bộ, có thể dùng model nhỏ tự host on-prem thay vì luôn gọi model cloud.

### Lợi ích
- Giảm phụ thuộc internet/public provider
- Hợp với môi trường có mạng nội bộ tách biệt
- Tốt cho bảo mật và độ sẵn sàng

### Trade-off
- Tốn công vận hành model
- Cần đánh giá phần cứng
- Chất lượng có thể thấp hơn model lớn ở câu hỏi mở

### Thời điểm áp dụng
Áp dụng khi volume đủ lớn hoặc sau giai đoạn MVP chứng minh được nhu cầu thật.

---

## Chốt ưu tiên
### Làm ngay
1. Semantic caching  
2. Model routing cơ bản  

### Làm sau khi có usage ổn định
3. Smaller/self-hosted model on-prem

---

# Worksheet 4 — Scaling & Reliability Tabletop

## Tình huống 1 — Traffic tăng đột biến
Ví dụ: đầu ca sáng, nhiều kỹ thuật viên cùng hỏi do một dây chuyền gặp sự cố.

### Tác động tới user
- Trả lời chậm
- Timeout
- Kỹ thuật viên bỏ hệ thống và quay lại gọi người trực tiếp
- Mất niềm tin vào công cụ

### Phản ứng ngắn hạn
- Bật queue cho request không khẩn cấp
- Ưu tiên request liên quan lỗi máy đang active
- Trả lời tạm bằng top SOP/FAQ liên quan trước, rồi mới trả lời sâu

### Giải pháp dài hạn
- Pre-warm cache cho lỗi phổ biến
- Tách class request: real-time vs async
- Scale retrieval/index layer
- Bổ sung edge node hoặc on-prem serving ở xưởng lớn

---

## Tình huống 2 — Provider timeout / lỗi model
Ví dụ: LLM provider ngoài bị lỗi hoặc đường truyền tới cloud không ổn định.

### Tác động tới user
- Không nhận được câu trả lời
- Tình huống khẩn cấp bị chậm xử lý
- Có thể ảnh hưởng downtime máy

### Phản ứng ngắn hạn
- Retry có giới hạn
- Circuit breaker để tránh treo hàng loạt
- Fallback sang:
  - cached answer
  - search-only mode
  - rule-based SOP lookup
  - human escalation

### Giải pháp dài hạn
- Thiết kế multi-provider hoặc hybrid inference
- Dùng model nhỏ on-prem cho use case tối thiểu
- Xây knowledge retrieval độc lập với provider

---

## Tình huống 3 — Response chậm
Ví dụ: truy vấn log dài, nhiều nguồn dữ liệu, mạng yếu.

### Tác động tới user
- Người dùng thấy hệ thống “đơ”
- Bị gián đoạn thao tác tại hiện trường
- Không phù hợp cho tình huống cần quyết định nhanh

### Phản ứng ngắn hạn
- Streaming câu trả lời từng phần
- Trả top tài liệu liên quan trước
- Rút ngắn context/prompt
- Timeout sớm và gợi ý truy vấn cụ thể hơn

### Giải pháp dài hạn
- Tối ưu index và reranking
- Prompt compression
- Cache theo loại lỗi/mã máy
- Edge retrieval cho tài liệu thường dùng

---

## Các metric cần monitor
- P95 latency
- Success rate / error rate
- Timeout rate
- Cache hit rate
- Fallback rate
- Số request theo ca / theo xưởng
- Tỷ lệ câu trả lời được người dùng đánh dấu hữu ích
- Tỷ lệ escalation sang con người

## Fallback proposal
**Fallback nhiều lớp:**
1. **Cache answer** nếu câu hỏi quen thuộc  
2. **Search-only mode** trả SOP/manual liên quan  
3. **Rule-based response** cho checklist an toàn chuẩn  
4. **Human escalation** tới quản đốc/kỹ sư trực ca  

## Kết luận reliability
Với scenario này, do môi trường có mạng yếu, hệ thống legacy và yêu cầu ổn định cao, fallback không thể chỉ là “đổi provider” mà cần có retrieval cục bộ + rule-based + human escalation.

---

# Worksheet 5 — Skills Map & Track Direction

## 1) Tự chấm nhóm theo 3 mảng

| Mảng | Điểm trung bình nhóm (1–5) |
|---|---:|
| Business / Product | 3.5 |
| Infra / Data / Ops | 4.0 |
| AI Engineering / Application | 3.5 |

## 2) Điểm mạnh của nhóm nằm ở đâu?
**Điểm mạnh nên chốt:** Infra / Data / Ops

### Lý do
- Đề tài này thiên mạnh về deployment thực tế trong môi trường enterprise
- Cần hiểu network nội bộ, legacy integration, reliability, fallback, edge/on-prem
- Đây là nơi nhóm có thể tạo khác biệt tốt hơn là chỉ làm demo chatbot

## 3) Track Phase 2 phù hợp nhất
**Đề xuất Track Phase 2:** Infra / Data / Ops  
Kết hợp thêm AI Application nếu nhóm vẫn muốn giữ phần agent, nhưng trọng tâm nên là hệ thống hóa deployment và reliability cho môi trường công nghiệp.

## 4) 2–3 kỹ năng cần bù nếu muốn tiếp tục dự án này
1. Thiết kế hệ thống hybrid / on-prem / edge cho AI agent  
2. Tích hợp dữ liệu legacy và xây pipeline ingest tài liệu kỹ thuật  
3. Monitoring, fallback, incident response cho AI system trong môi trường production  

---

# Team Note Sheet

- **Tên nhóm:** [Điền tên nhóm]
- **Chủ đề:** Manufacturing Maintenance Copilot

## 3 ràng buộc enterprise lớn nhất
- Nhiều hệ thống legacy
- Có khu vực mạng nội bộ không cho internet
- Cần độ ổn định cao

## Cost driver lớn nhất
- Inference + retrieval cho truy vấn vận hành thực tế

## 3 chiến lược optimize được chọn
- Semantic caching
- Model routing
- Smaller/self-hosted model khi volume đủ lớn

## Fallback / reliability plan ngắn gọn
- Cache → search-only SOP/manual → rule-based checklist → human escalation

## Track Phase 2 đề xuất
- Infra / Data / Ops

---

# Phiếu chốt chủ đề

- **Tên nhóm:** [Điền tên nhóm]
- **Chủ đề được chọn:** Scenario 4 – Manufacturing Maintenance Copilot
- **Nguồn chủ đề:** Scenario card

## Lý do chọn chủ đề này
Chủ đề có bối cảnh enterprise rất rõ, đủ để tranh luận sâu về deployment, cost, optimization và reliability. Đây cũng là một bài toán thực tế vì nhà máy có nhiều hệ thống legacy, mạng nội bộ hạn chế internet và yêu cầu độ ổn định cao.

## 3 từ khóa mô tả bối cảnh enterprise của chủ đề
- Legacy systems
- On-prem / Hybrid
- Reliability
