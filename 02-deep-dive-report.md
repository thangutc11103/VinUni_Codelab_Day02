# 02 — Báo Cáo Phân Tích Sâu (Deep-Dive Report): Vinhomes Smart Incident Triage & Auto-Routing

**Đơn vị thực hiện:** AI Product Engineering — Vin Smart Future  
**Công ty thành viên phụ trách:** Vinhomes (Khối Vận hành Đô thị & Ban Quản lý Tòa nhà)  
**Bài toán lựa chọn:** Hệ thống Phân loại & Tự động Định tuyến Sự cố Kỹ thuật Cư dân trên App Vinhomes Resident  
**Tác giả:** Nguyễn Việt Hoàng (`viethoang`)  

---

## 🏛️ 1. Bối cảnh & Lý do lựa chọn bài toán

Tập đoàn Vingroup hiện đang quản lý hàng chục đại đô thị quy mô lớn (Vinhomes Ocean Park 1, 2, 3, Vinhomes Smart City, Vinhomes Grand Park...) với hàng trăm nghìn căn hộ và hàng triệu cư dân. 

Mỗi ngày, Khối Vận hành Vinhomes tiếp nhận trung bình từ **300 đến 600 phản ánh kỹ thuật/ngày** trên mỗi đại đô thị thông qua ứng dụng *Vinhomes Resident*. Các phản ánh này có nội dung cực kỳ đa dạng từ ngôn ngữ đời thường tự do của cư dân:
* *"Tòa S2.01 nhà tôi bị mất nước lạnh từ sáng, kiểm tra giùm gấp"*
* *"Thang máy số 3 tòa Masteri bị kẹt tầng 15 phát ra tiếng kêu to"*
* *"Đèn ngoài hành lang căn 12A08 bị chập chờn nhấp nháy cả đêm"*
* *"Hàng xóm căn bên cạnh khoan tường ồn quá quy định giờ nghỉ trưa"*

Hiện nay, toàn bộ khâu đọc, xác thực căn hộ, phân loại và tạo Ticket việc trên ERP đều thực hiện hoàn toàn thủ công bởi Trực ban Ban Quản lý (BQL) Tòa nhà, tạo ra điểm nghẽn nghiêm trọng vào các khung giờ cao điểm (7h-9h sáng và 18h-21h tối).

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Phân tích Quy trình Hiện tại (Current-State Workflow Mapping)

Quy trình xử lý một sự cố cư dân hiện tại gồm 5 bước tuần tự:

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ Bước 1          │       │ Bước 2          │       │ Bước 3          │       │ Bước 4          │       │ Bước 5          │
│ Cư dân gửi      │       │ Trực ban BQL    │       │ Phân loại sự cố │       │ Điều phối thợ   │       │ Phản hồi        │
│ phản ánh trên   │ ───🔄─> đọc tin nhắn &  │ ───>  │ & gõ tay tạo    │ ───🔄─> hiện trường     │ ───🔄─> xác nhận cho    │
│ App Resident    │       │ tra cứu căn hộ  │       │ Ticket ERP      │       │ qua bộ đàm/Zalo │       │ cư dân qua App  │
│                 │       │                 │       │                 │       │                 │       │                 │
│ Actor: Cư dân   │       │ Actor: BQL Desk │       │ Actor: BQL Desk │       │ Actor: BQL Desk │       │ Actor: BQL Desk │
│ ⏱ 2 phút        │       │ ⏱ 4 phút 🔴     │       │ ⏱ 5 phút 🔴     │       │ ⏱ 2 phút        │       │ ⏱ 2 phút        │
│ In: Text / Ảnh  │       │ In: Tin nhắn    │       │ In: Vấn đề      │       │ In: Mã ticket   │       │ In: Trạng thái  │
│ Out: Raw post   │       │ Out: Mã căn     │       │ Out: Ticket ERP │       │ Out: Gán thợ    │       │ Out: Thông báo  │
└─────────────────┘       └─────────────────┘       └─────────────────┘       └─────────────────┘       └─────────────────┘
```

* 🔴 **Điểm nghẽn nghiêm trọng (Bottlenecks):**
  * **Bước 2 (Tra cứu thủ công):** Cư dân thường gõ thiếu thông tin (ví dụ: chỉ ghi *"căn 1205 bị mất nước"* mà không ghi rõ thuộc tòa S1.01 hay S1.02), buộc nhân viên phải tra ngược số điện thoại hoặc lịch sử hợp đồng cư dân.
  * **Bước 3 (Phân loại & Tạo Ticket ERP):** Nhân viên phải tự phán đoán sự cố này thuộc Đội Điện nước, Đội Xây dựng hoàn thiện, Đội Vệ sinh môi trường hay Đội An ninh trật tự. Giờ cao điểm, việc gõ tay từng trường thông tin vào hệ thống mất trung bình 5 phút/ticket, dẫn đến tình trạng tồn đọng hàng trăm phản ánh chưa xử lý.
* 🔄 **Điểm chuyển giao thông tin (Handoffs):**
  * **Handoff 1:** Từ Cư dân sang Trực ban BQL qua giao thức push thông báo của App.
  * **Handoff 2:** Từ Trực ban BQL sang Kỹ thuật viên hiện trường qua bộ đàm và nhóm Zalo/nội bộ.
  * **Handoff 3:** Từ Trực ban BQL gửi tin xác nhận tiếp nhận ngược lại cho Cư dân.
* ⏱ **Tổng thời gian xử lý thủ công:** **15 phút/lượt phản ánh**.

---

## 3.2. Bảng Mô Tả Bài Toán 6 Trường (Problem Statement 6-Field) — Chuẩn Vin Smart Future

| Trường thông tin (Field) | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Trực ban Ban Quản lý Tòa nhà (Operations Desk Desk Operator) tại các khu đô thị Vinhomes. |
| **2. Current Workflow** | Khi nhận phản ánh tự do từ cư dân, nhân viên BQL đọc nội dung, kiểm tra hồ sơ căn hộ, phân loại tính chất vấn đề, đăng nhập hệ thống SAP/ERP để tạo Ticket công việc, liên hệ đội kỹ thuật qua bộ đàm, và gửi tin báo cho cư dân. Quy trình 5 bước thủ công, mất 15 phút/ticket. |
| **3. Bottleneck** | Bước 2 & 3: Xử lý văn bản tự nhiên phi cấu trúc của cư dân; trích xuất căn hộ/vấn đề và tạo Ticket trên ERP. Vào giờ cao điểm, mỗi nhân viên phải gánh 40–60 ticket/ca, dẫn đến độ trễ phản hồi lên tới 4–8 tiếng. |
| **4. Business Impact** | - Khối BQL tốn hơn 120 giờ công làm việc/ngày chỉ để nhập liệu thủ công.<br>- Thời gian chờ đợi lâu khiến tỉ lệ hài lòng của cư dân (CSAT) giảm sút, gia tăng khiếu nại gay gắt (chấm 1 sao trên App Resident).<br>- Nguy cơ bỏ sót các sự cố kỹ thuật khẩn cấp (như rò rỉ nước tràn sàn gỗ, chập điện cục bộ) gây thiệt hại tài sản hàng trăm triệu đồng cho cư dân. |
| **5. Success Metric** | **1. Tốc độ:** Giảm thời gian tiếp nhận, trích xuất và tạo nháp Ticket từ 15 phút xuống **dưới 15 giây** (Hiệu suất tăng gấp 60 lần).<br>**2. Độ chính xác:** Tỉ lệ phân loại đúng đội kỹ thuật chuyên trách và mức độ ưu tiên đạt **>= 96%**.<br>**3. Độ phủ tự động:** Ít nhất **85%** phản ánh thông thường được tự động định tuyến không cần trực ban gõ lại tay. |
| **6. Operational Boundary (Ranh giới vận hành an toàn)** | - **QUY TẮC AN TOÀN TUYỆT ĐỐI (Emergency Rule):** Nếu văn bản chứa các từ khóa đe dọa an toàn tính mạng/tài sản nghiêm trọng (*cháy, khói, nổ, rò rỉ khí gas, kẹt thang máy có người bên trong*), hệ thống BẮT BUỘC bỏ qua xử lý văn bản LLM, lập tức kích hoạt còi báo động đỏ và gửi cảnh báo khẩn cấp (Priority P0) tới Trưởng ban An ninh và Kỹ thuật trực trong vòng 1 giây.<br>- **QUY TẮC DUYỆT (HITL):** Mọi ticket do AI phân loại đều ở trạng thái Nháp (`[DRAFT_TICKET]`); Kỹ thuật viên hoặc BQL bấm 1-click xác nhận "Nhận việc".<br>- **CẤM:** AI không được phép tự động trả lời các cam kết về tiền bạc, miễn giảm phí quản lý hoặc đền bù thiệt hại tài sản cho cư dân. |

---

## 3.3. Future-State Flow & Đánh Giá AI Fit (AI-Fit Matrix)

### A. Phân tích mức độ phù hợp AI (AI-Fit Matrix):
* **Vì sao KHÔNG dùng Pure Rule-based (Chỉ dùng luật code cứng)?**  
  Cư dân sử dụng ngôn ngữ tự nhiên cực kỳ đa dạng (tiếng lóng, viết tắt, từ địa phương, mô tả gián tiếp: *"nhà tôi lênh láng nước rồi"*, *"thang máy bị đơ"*). Nếu dùng code `if/else` từ khóa, hệ thống sẽ bỏ sót hàng loạt trường hợp hoặc phân loại sai lệch nghiêm trọng.
* **Vì sao KHÔNG dùng Autonomous Multi-Agent (Hệ thống đa tác tử tự hành)?**  
  Quy trình tiếp nhận sự cố kỹ thuật đô thị có cấu trúc nghiệp vụ rõ ràng, yêu cầu độ trễ thấp (< 2 giây) và độ tin cậy tuyệt đối. Việc dùng nhiều Agent trao đổi tự do làm tăng chi phí token, tăng độ trễ và khó kiểm soát ranh giới an toàn.
* **LỰA CHỌN TỐI ƯU: Rule-Engine + LLM Feature (Kết hợp Luật Khẩn Cấp & Mô Hình Ngôn Ngữ Trích Xuất Thực Thể):**
  * **Rule Engine (Chốt chặn 1):** Quét các sự cố khẩn cấp (P0: Cháy nổ, kẹt thang) để báo động tức thì.
  * **LLM Feature (Chốt chặn 2):** Đọc nội dung thông thường, trích xuất JSON có cấu trúc `[Mã căn, Phân khu, Vấn đề chính, Đội kỹ thuật phụ trách, Mức ưu tiên P1-P4]` và draft Ticket.

### B. Sơ đồ quy trình tương lai (Future-State Flow):

```text
Cư dân gửi phản ánh qua App
            │
            ▼
┌──────────────────────────────────────┐
│ Chốt chặn 1: RULE ENGINE             │
│ Quét từ khóa P0 (Cháy, Kẹt thang...) │
└──────────────────┬───────────────────┘
                   │
         Có khẩn cấp P0 không?
         ├─── [CÓ (P0)] ───────────────────────────────────────> 🚨 KÍCH HOẠT CHUÔNG BÁO ĐỘNG ĐỎ
         │                                                        & GỌI ĐỘI CỨU HỘ TRONG 1 GIÂY
         │
         └─── [KHÔNG (Sự cố kỹ thuật thường)]
                     │
                     ▼
       ┌────────────────────────────────────────┐
       │ Chốt chặn 2: 🔵 LLM FEATURE            │
       │ Trích xuất thực thể & Phân loại Intent │
       │ Format: [DRAFT_TICKET] (JSON)          │
       └──────────────────┬─────────────────────┘
                          │
               Độ tin cậy Confidence >= 85%?
               ├─── [KHÔNG] ──────────────────────────────────> ↩️ FALLBACK:
               │                                                Chuyển về hòm thư Trực ban
               │                                                BQL xử lý thủ công như cũ.
               │
               └─── [CÓ]
                     │
                     ▼
       ┌────────────────────────────────────────┐
       │ Chốt chặn 3: 🟢 HUMAN-IN-THE-LOOP      │
       │ Ticket đẩy vào App Kỹ thuật viên ca    │
       │ Thợ bấm 1-click "Xác nhận & Đi sửa"   │
       └────────────────────────────────────────┘
```

---

# 💻 Phase 4 — Kiểm thử Ranh giới An toàn (Prompt Boundary Testing)

Hệ thống được thiết lập bộ chỉ thị an toàn nghiêm ngặt:
1. **Ranh giới 1 (`[DRAFT_TICKET]`):** Mọi đầu ra tạo việc phải gắn nhãn bản nháp, tuyệt đối không tự điều phối bên ngoài quy trình kiểm soát của BQL.
2. **Ranh giới 2 (Sự cố khẩn cấp P0):** Tuyệt đối không để LLM "chém gió" văn phong hoa mỹ khi có nguy cơ cháy nổ, kẹt thang; phải kích hoạt ngay action cứu hộ khẩn cấp: `{"action": "emergency_alarm_dispatch", "priority": "P0"}`.

---

# 🏁 Phase 5 — EVALUATE: Đánh Giá Độ Sẵn Sàng & Quyết Định Dự Án

### Bảng Đánh Giá Sẵn Sàng AI (AI Readiness Checklist):

| # | Tiêu chí đánh giá | Trạng thái | Minh chứng thực tế tại Vinhomes |
|---|---|:---:|---|
| 1 | **Dữ liệu sạch & Logs sẵn có?** | ✅ ĐẠT | Vinhomes đã có lịch sử hơn 500,000 ticket phản ánh cư dân trên hệ thống ERP kèm nhãn phân loại của 3 năm qua. |
| 2 | **Rủi ro AI sai có nằm trong tầm kiểm soát?** | ✅ ĐẠT | Có chốt chặn Rule Engine cho trường hợp P0 khẩn cấp; có cơ chế HITL (thợ bấm duyệt) và Fallback chuyển về người khi độ tin cậy < 85%. |
| 3 | **Stakeholders sẵn sàng thay đổi quy trình?** | ✅ ĐẠT | Khối Vận hành và Ban Quản lý đang quá tải nhân sự, cực kỳ ủng hộ việc tự động hóa khâu nhập liệu để giảm giờ làm việc. |
| 4 | **ROI & Chi phí hạ tầng hợp lý?** | ✅ ĐẠT | Sử dụng mô hình nhẹ (Gemini 2.5 Flash), chi phí ước tính < 200 VNĐ/ticket, tiết kiệm hàng tỷ đồng chi phí nhân sự trực ban mỗi năm. |

### 🏆 Quyết định cuối cùng của Hội đồng Công nghệ Vin Smart Future:

> ## **[ X ] QUYẾT ĐỊNH: GO (BẮT ĐẦU TRIỂN KHAI PROTOTYPE & PILOT HẸP)**

### Lý giải quyết định (Justification):
1. **Giá trị kinh doanh vượt trội:** Giải quyết trực tiếp bài toán quá tải giờ cao điểm của hàng chục đại đô thị Vinhomes, rút ngắn thời gian phản hồi từ 15 phút xuống dưới 15 giây, nâng cao chỉ số hài lòng của cư dân.
2. **Kiến trúc công nghệ thực dụng:** Không lạm dụng Multi-Agent phức tạp; kết hợp nhịp nhàng giữa **Rule-based (cho an toàn khẩn cấp)** và **LLM Feature (cho trích xuất ngôn ngữ tự nhiên)**.
3. **Kiểm soát rủi ro hoàn hảo:** Sự cố khẩn cấp P0 được bảo vệ bằng Rule độc lập; 100% công việc đều có con người kiểm duyệt (Human-in-the-loop); có phương án Fallback sẵn sàng khi AI không chắc chắn.
