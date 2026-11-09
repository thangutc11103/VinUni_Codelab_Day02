# 01 — Problem Scan & Quick Assessments (Cá nhân)

**Học viên thực hiện:** Việt Hoàng (viethoang)  
**Đơn vị:** AI Product Engineer — Vin Smart Future (Vingroup)  
**Bài Lab:** Lab 02 — AI Product Scoping  

---

# 🔍 Phase 1 — SCAN: Quét Cơ Hội Qua 4 Lenses

Sử dụng **4 Lenses** (*Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác*) để quét qua các mảng vận hành của các công ty thành viên thuộc Tập đoàn Vingroup.

### 📋 Bảng Quét Cơ Hội Bài Toán Vận Hành (Scan Opportunities):

| # | Đơn vị thành viên (Subsidiary) | Lens áp dụng | Mô tả ngắn bài toán & Điểm nghẽn vận hành (Pain Point) |
|---|--------------------------------|--------------|---------------------------------------------------------|
| 1 | **Xanh SM (GSM)** | **Tốn thời gian** (Time-consuming) | **Xử lý sự cố sạc pin / cạn pin thực địa:** Tài xế gọi tổng đài báo xe sắp hết pin (< 5%) hoặc trụ sạc dự kiến bị lỗi. Điều phối viên phải tra cứu bản đồ vị trí, mở hệ thống tìm trụ sạc VinFast còn trống phù hợp chuẩn sạc rồi soạn tin hướng dẫn thủ công (mất 15–20 phút/sự cố). |
| 2 | **VinFast** | **AI có thể tốt hơn** (AI-upgrade) | **Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt tự nhiên:** Khách hàng báo hỏng trên ứng dụng VinFast bằng ngôn ngữ đời thường (ví dụ: *"xe đi qua gờ giảm tốc kêu cụp cụp ở gầm trước bên lái"*). Hiện tại nhân viên CSKH chưa có chuyên môn kỹ thuật sâu phải gọi hỏi lại nhiều lần để phân loại mã lỗi trước khi điều phối về xưởng dịch vụ. |
| 3 | **Vinhomes** | **Lặp lại** (Repetitive) | **Phân loại & điều hướng phản ánh kỹ thuật cư dân:** Hằng ngày có hàng nghìn phản ánh (mất nước, chập điện, hỏng đèn hành lang, tiếng ồn) gửi về App Vinhomes Resident. Nhân viên ban quản lý phải đọc từng tin nhắn để gán thủ công cho tổ kỹ thuật hoặc tổ an ninh của từng phân khu. |
| 4 | **Vinmec** | **Pain từ người khác** (Stakeholder Pain) | **Soạn thảo tóm tắt bệnh án xuất viện (Discharge Summary):** Bác sĩ điều trị mất 20–30 phút/bệnh nhân để tổng hợp lịch sử khám, kết quả xét nghiệm, chẩn đoán hình ảnh thành văn bản tóm tắt cho bệnh nhân. Bác sĩ phàn nàn vì quá tải thủ tục hành chính, giảm thời gian khám chữa bệnh trực tiếp. |
| 5 | **Vinpearl / VinWonders** | **Tốn thời gian** (Time-consuming) | **Trích xuất thông tin đặt phòng đoàn (Group Booking) từ email đại lý:** Các đại lý du lịch (OTA, tour agency) gửi email yêu cầu đặt đoàn với định dạng tự do, danh sách phòng thay đổi nhiều lần. Nhân viên đặt phòng phải đối chiếu thủ công chuỗi email vào hệ thống Opera PMS mất 30–45 phút/đoàn. |
| 6 | **VinFast** | **Lặp lại** (Repetitive) | **Đối chiếu hóa đơn sạc điện đối tác nhượng quyền:** So khớp tự động giữa dữ liệu đo đếm điện năng từ trụ sạc (telemetry logs) với hóa đơn thanh toán đối tác hàng tuần để phát hiện sai lệch chỉ số điện năng tiêu thụ. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Thẻ Bài Toán Tiềm Năng (Quick Problem Cards)

Từ 6 bài toán trên, lựa chọn **Top 3 bài toán** khả thi và có tác động kinh doanh cao nhất để tiến hành đánh giá nhanh.

---

### 📇 QUICK PROBLEM CARD #1: Xanh SM — Xử lý sự cố sạc pin / cạn kiệt pin thực địa

```text
┌──────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                            │
│                                                                                  │
│ Bài toán (1 câu): Hỗ trợ điều phối viên Xanh SM tra cứu trạm sạc khả dụng, ra    │
│ quyết định điều xe sạc lưu động hoặc soạn chỉ dẫn trạm sạc gần nhất cho tài xế. │
│ Công ty thành viên: [x] Xanh SM (GSM)                                            │
│                                                                                  │
│ Ai đang đau (Actor)?                                                             │
│ - Điều phối viên (Dispatcher): Quá tải vào giờ cao điểm, áp lực xử lý tức thì.   │
│ - Tài xế Xanh SM: Lo lắng xe cạn pin giữa đường, nguy cơ tắc đường và hủy cuốc.  │
│                                                                                  │
│ Workflow thủ công hiện tại (5 bước):                                             │
│   1. Nhận cuộc gọi khẩn cấp từ tài xế qua hệ thống tổng đài                      │
│   ──> 2. Mở bản đồ định vị tra cứu tọa độ xe và phần trăm pin hiện tại           │
│   ──> 3. Mở cổng thông tin trạm sạc VinFast tra cứu trụ sạc tương thích còn trống │
│   ──> 4. Soạn thảo tin nhắn hướng dẫn đường đi chi tiết và gửi qua Driver App    │
│   ──> 5. (Nếu pin < 5%) Liên hệ tổ xe cứu hộ sạc pin lưu động đến ứng cứu         │
│                                                                                  │
│ Bước nào tốn thời gian / dễ lỗi nhất?                                            │
│ - Bước 3 và Bước 4: Mất 10–12 phút/lượt. Dễ lỗi nếu điều xe đến trạm đã kín chỗ  │
│   hoặc trạm không có cổng sạc công suất phù hợp dòng xe (VF5 / VF8).             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                            │
│ - Bước 2, 3, 4: Tự động gom dữ liệu GPS + pin + trạm sạc, LLM tự động soạn thảo │
│   tin nhắn chỉ dẫn dạng nháp ([DRAFT_ONLY]) cho điều phối viên duyệt 1-click.    │
│                                                                                  │
│ Đo thành công bằng gì (Metric có số)?                                            │
│ - Thời gian xử lý trung bình một sự cố giảm từ 15 phút ──> dưới 3 phút.          │
│ - Tỉ lệ chỉ dẫn chính xác loại cổng sạc và trạm khả dụng đạt >= 98%.             │
│                                                                                  │
│ Quick Architecture: [x] LLM Feature (kết hợp API trạm sạc & Human-in-the-loop)   │
└──────────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #2: VinFast — Trợ lý phân loại lỗi kỹ thuật xe điện ban đầu

```text
┌──────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                            │
│                                                                                  │
│ Bài toán (1 câu): Phân tích mô tả triệu chứng bất thường bằng tiếng Việt tự nhiên │
│ của chủ xe điện trên App VinFast để gợi ý phân loại mã lỗi và hướng dẫn ban đầu.  │
│ Công ty thành viên: [x] VinFast                                                  │
│                                                                                  │
│ Ai đang đau (Actor)?                                                             │
│ - Chủ xe điện: Hoang mang khi xe phát ra tiếng động lạ hoặc cảnh báo taplo.      │
│ - Nhân viên CSKH / Cố vấn dịch vụ xưởng: Mất nhiều thời gian gặng hỏi khách để  │
│   hiểu triệu chứng kỹ thuật, dẫn đến đặt lịch sai chuyên môn xưởng.              │
│                                                                                  │
│ Workflow thủ công hiện tại (4 bước):                                             │
│   1. Chủ xe nhập mô tả văn bản hoặc gửi ghi âm qua tính năng hỗ trợ App          │
│   ──> 2. Nhân viên CSKH đọc mô tả, gọi điện lại cho khách gặng hỏi chi tiết     │
│   ──> 3. Nhân viên tra sổ tay kỹ thuật để dự đoán cụm linh kiện bị lỗi           │
│   ──> 4. Hướng dẫn khách tự kiểm tra cơ bản hoặc đặt lịch xưởng phù hợp          │
│                                                                                  │
│ Bước nào tốn thời gian / dễ lỗi nhất?                                            │
│ - Bước 2 và Bước 3: Mất 15–20 phút/lượt. Dễ sai lệch do khách mô tả cảm tính    │
│   (tiếng kêu lọc cọc, ù tai, giật cục) mà CSKH không có chuyên môn thợ máy.      │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                            │
│ - Bước 2 & 3: LLM phân tích ngôn ngữ tự nhiên, trích xuất triệu chứng, đối chiếu │
│   với cơ sở tri thức lỗi VinFast (RAG), sinh bản tóm tắt kỹ thuật sơ bộ.         │
│                                                                                  │
│ Đo thành công bằng gì (Metric có số)?                                            │
│ - Thời gian phân loại sơ bộ giảm từ 20 phút ──> dưới 30 giây.                    │
│ - Độ chính xác định tuyến đúng chuyên khoa xưởng (gầm, pin, phần mềm) >= 90%.   │
│                                                                                  │
│ Quick Architecture: [x] LLM Feature (RAG tri thức kỹ thuật + Human review)       │
└──────────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #3: Vinhomes — Phân loại & Điều hướng sự cố cư dân thông minh

```text
┌──────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                            │
│                                                                                  │
│ Bài toán (1 câu): Tự động trích xuất thông tin tòa nhà/căn hộ và phân loại nội   │
│ dung khiếu nại/sự cố của cư dân trên App Vinhomes Resident đến đúng ban phụ trách│
│ Công ty thành viên: [x] Vinhomes                                                 │
│                                                                                  │
│ Ai đang đau (Actor)?                                                             │
│ - Cư dân Vinhomes: Bực bội vì phản ánh mất 4–8 tiếng mới được tiếp nhận xử lý.  │
│ - Trực ban BQL (Building Operation Desk): Mỗi ca nhận hàng trăm tin nhắn hỗn loạn│
│   phải đọc và chuyển tiếp thủ công sang các tổ Kỹ thuật, Vệ sinh, An ninh.       │
│                                                                                  │
│ Workflow thủ công hiện tại (4 bước):                                             │
│   1. Cư dân gửi phản ánh tự do qua ứng dụng Vinhomes Resident                    │
│   ──> 2. Nhân viên tiếp nhận đọc nội dung, xác định tòa/căn hộ và tính chất việc │
│   ──> 3. Đăng nhập hệ thống điều phối nội bộ tạo Ticket công việc tương ứng      │
│   ──> 4. Gọi bộ đàm hoặc gửi thông báo đến trưởng nhóm kỹ thuật khu vực          │
│                                                                                  │
│ Bước nào tốn thời gian / dễ lỗi nhất?                                            │
│ - Bước 2 và Bước 3: Mất 8–10 phút/ticket, dễ nhầm lẫn giữa tổ kỹ thuật điện nước │
│   với bảo dưỡng hạ tầng chung; giờ cao điểm bị dồn ứ hàng trăm phản ánh.        │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                            │
│ - Bước 2 & 3: LLM trích xuất thực thể (Căn hộ, Vấn đề, Mức độ khẩn cấp) và xuất │
│   ra định dạng JSON để hệ thống tự động gán Ticket ngay tức thì.                 │
│                                                                                  │
│ Đo thành công bằng gì (Metric có số)?                                            │
│ - Tốc độ tiếp nhận và tạo ticket giảm từ 10 phút ──> dưới 10 giây (gần real-time)│
│ - Tỉ lệ phân loại đúng phòng ban xử lý đạt >= 95%.                               │
│                                                                                  │
│ Quick Architecture: [x] Rule + LLM Classifier (Rule lọc khẩn cấp + LLM trích xuất)│
└──────────────────────────────────────────────────────────────────────────────────┘
```

---

# 🎯 Đề Xuất Bài Toán Trọng Tâm Để Làm Prototype & Báo Cáo Sâu:
* **Lựa chọn:** **Card #1 — Xanh SM Xử lý sự cố sạc pin / cạn kiệt pin thực địa**.
* **Lý do lựa chọn:**
  1. Đây là quy trình nghiệp vụ gắn liền với an toàn vật lý và trải nghiệm khách hàng cốt lõi của Vingroup.
  2. Ranh giới vận hành (**Operational Boundary**) cực kỳ rõ ràng và cấp thiết: Nguy cơ xe cạn pin giữa đường gây tắc đường, do đó khi pin `< 5%` bắt buộc phải có cơ chế điều xe cứu hộ thay vì ép xe chạy tiếp tới trạm sạc xa.
  3. Có cơ chế **Human-in-the-loop (`[DRAFT_ONLY]`)** giúp kiểm soát rủi ro hoàn toàn trước khi thông tin được gửi tới tài xế.
