# Lab 02 — Phase 1 & Phase 2: Problem Scan & Quick Problem Cards (Vin Smart Future)

**Họ và tên:** Đức Thắng (`ducthang`) — Trưởng nhóm  
**Đơn vị:** Vin Smart Future (Vingroup)  

---

## 🔍 Phase 1 — SCAN: Quét cơ hội tối ưu vận hành bằng 4 Lenses

Sử dụng **4 Lenses** (*Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác*) để rà soát các điểm nghẽn (bottlenecks) thực tế tại các công ty thành viên thuộc Tập đoàn Vingroup:

| # | Công ty thành viên | Lens | Mô tả ngắn bài toán & Tổn thất ước tính |
|---|---|---|---|
| **1** | **Vinhomes** | **Lặp lại & AI có thể tốt hơn** | **Phân loại và điều phối phản ánh cư dân đến đúng bộ phận xử lý:** Hàng trăm phản ánh mỗi ngày trên App Vinhomes Resident (kẹt thang máy, rò nước, mất điện, an ninh, ồn ào, phí dịch vụ) đang được CSKH đọc thủ công, phân loại mức độ khẩn cấp và gõ tin nhắn phản hồi từng ticket; mất ~15 phút/ticket, dễ trễ SLA sự cố khẩn cấp. |
| **2** | **Xanh SM (GSM)** | **Tốn thời gian** | **Xử lý sự cố pin thực địa & điều phối trạm sạc/xe cứu hộ:** Điều phối viên phải tra cứu thủ công tọa độ GPS của xe, tìm trụ sạc VinFast còn trống phù hợp dòng xe (VF5/VFe34/VF8) và soạn tin nhắn chỉ dẫn hoặc gọi xe cứu hộ pin di động (~15 phút/lượt). |
| **3** | **VinFast** | **Lặp lại** | **Đối chiếu hóa đơn sạc điện và sản lượng trạm sạc nhượng quyền/đối tác:** Kế toán vận hành phải rà soát hàng chục nghìn bản ghi phiên sạc (session logs) mỗi tuần với bảng kê thanh toán từ đối tác mặt bằng, dễ lệch số liệu công tơ và ngốn 3 ngày công/tuần. |
| **4** | **Vinmec** | **Pain từ người khác** | **Soạn thảo Tóm tắt Hồ sơ Xuất viện (Discharge Summary):** Bác sĩ nội trú mất 20–30 phút/bệnh nhân để tổng hợp kết quả xét nghiệm, chẩn đoán hình ảnh, diễn tiến điều trị và dặn dò sau xuất viện bằng ngôn ngữ dễ hiểu cho bệnh nhân. |
| **5** | **Xanh SM (GSM)** | **Pain từ người khác** | **Phân tích nguyên nhân hủy chuyến từ ghi âm tổng đài & ghi chú tài xế:** Mỗi ngày có hàng nghìn cuốc xe bị hủy giờ cao điểm nhưng dữ liệu nằm rải rác ở file audio và text tự do, khiến đội Vận hành khó bóc tách lỗi định vị hay tài xế. |
| **6** | **Vinpearl** | **Tốn thời gian** | **Bóc tách yêu cầu đặt phòng đoàn (Group Booking Email) từ đại lý lữ hành (OTA/TA):** Nhân viên kinh doanh phải đọc các email dài kèm bảng Excel không chuẩn hóa để kiểm tra quỹ phòng (allotment) và soạn báo giá nháp (25–40 phút/yêu cầu). |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Từ 6 bài toán trên, lựa chọn **Top 3 bài toán** có tác động vận hành rõ rệt nhất (**#1 Vinhomes Phân loại & Điều phối phản ánh cư dân**, **#2 Xanh SM Sự cố sạc pin**, **#4 Vinmec Tóm tắt hồ sơ xuất viện**) để lập thẻ đánh giá nhanh:

---

### Thẻ #1: Vinhomes — AI phân loại và điều phối phản ánh cư dân đến đúng bộ phận xử lý (Bài toán trọng tâm)

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 (CHỌN DEEP-DIVE)                                    │
│                                                                           │
│ Bài toán (1 câu): Kết hợp Rule lọc sự cố khẩn cấp và LLM để tự động phân  │
│ loại, điều phối phản ánh cư dân đến đúng bộ phận và soạn nháp cho CSKH.   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes                │
│                     [ ] Vinmec   [ ] Khác: ________________________       │
│                                                                           │
│ Ai đang đau (Actor)?                                                      │
│ - Nhân viên CSKH & Trực ban Ban Quản lý (BQL) Vinhomes (quá tải ticket)   │
│ - Đội Kỹ thuật / An ninh / Vệ sinh (nhận nhầm việc do phân luồng sai)     │
│ - Cư dân Vinhomes (chờ phản hồi lâu, bức xúc khi sự cố khẩn bị xử lý chậm)│
│                                                                           │
│ Workflow thủ công hiện tại (5 bước):                                      │
│   1. Cư dân gửi phản ánh (text tự do/ảnh) trên App Vinhomes Resident (1p) │
│   ──> 2. CSKH đọc ticket, kiểm tra xem có phải sự cố khẩn cấp không (3p)  │
│   ──> 3. Phân loại thủ công & chọn bộ phận xử lý (Kỹ thuật/An ninh...) (4p│
│   ──> 4. Gõ tay tin nhắn phản hồi cư dân & phiếu giao việc nội bộ (5p)    │
│   ──> 5. Chuyển phiếu lên hệ thống Task BQL & theo dõi SLA (2p)           │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2, 3 & 4 (⏱ 12 phút/ticket)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                     │
│   - Bước 2: Rule-based lọc tức thì từ khóa/tình huống khẩn cấp (cháy nổ,  │
│     kẹt thang máy, rò khí gas, vỡ ống nước lớn, sự cố trạm sạc hầm < 5%). │
│   - Bước 3 & 4: LLM phân tích ngôn ngữ tự nhiên tiếng Việt để gán đúng    │
│     bộ phận (Kỹ thuật / An ninh / Vệ sinh / DV Cư dân) + soạn bản nháp    │
│     [DRAFT_ONLY] để CSKH kiểm duyệt (HITL) trước khi gửi.                 │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   1. Giảm thời gian xử lý & điều phối từ 15 phút/ticket ──> < 2.5 phút.   │
│   2. 100% phản ánh khẩn cấp (P1) được bóc tách và cảnh báo dưới 5 giây.   │
│   3. Độ chính xác điều phối đúng bộ phận chuyên trách đạt >= 95%.         │
│                                                                           │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] Hybrid (Rule + LLM + HITL)   │
└───────────────────────────────────────────────────────────────────────────┘
```

---

### Thẻ #2: Xanh SM — Điều phối sự cố sạc pin thực địa & Xe cứu hộ pin di động

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                     │
│                                                                           │
│ Bài toán (1 câu): Tự động tra cứu trạm sạc trống phù hợp hoặc kích hoạt   │
│ xe cứu hộ pin di động và soạn chỉ dẫn nháp khi tài xế báo sự cố pin.      │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes                │
│                     [ ] Vinmec   [ ] Khác: ________________________       │
│                                                                           │
│ Ai đang đau (Actor)?                                                      │
│ - Điều phối viên (Dispatcher) Trung tâm Điều vận Xanh SM                  │
│ - Tài xế taxi điện Xanh SM (chờ đợi lâu trên đường, nguy cơ sập nguồn)    │
│                                                                           │
│ Workflow thủ công hiện tại (5 bước):                                      │
│   1. Tài xế gọi hotline điều vận báo sắp cạn pin / trụ sạc lỗi (2 phút)   │
│   ──> 2. Dispatcher tra cứu tọa độ GPS & % pin xe trên bản đồ (2 phút)    │
│   ──> 3. Tra cứu Dashboard trạm sạc VinFast tìm trụ trống phù hợp (5 phút)│
│   ──> 4. Soạn tin nhắn chỉ đường chi tiết gửi qua App Tài xế (5 phút)     │
│   ──> 5. Gọi đội Xe cứu hộ pin di động nếu pin nguy kịch < 5% (1 phút)    │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & Bước 4 (⏱ 10 phút/lượt)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                     │
│   Bước 3 & 4: Tự động đối chiếu % pin + khoảng cách trạm sạc trống,       │
│   kiểm tra ngưỡng an toàn (< 5% pin cấm đi xa > 5km -> điều xe cứu hộ),   │
│   và sinh bản nháp [DRAFT_ONLY] để Dispatcher duyệt gửi.                  │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   1. Giảm thời gian xử lý trung bình từ 15 phút/lượt ──> dưới 3 phút/lượt.│
│   2. Độ chính xác gợi ý đúng cổng sạc/trạm trống đạt >= 98%.              │
│                                                                           │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent       │
└───────────────────────────────────────────────────────────────────────────┘
```

---

### Thẻ #3: Vinmec — Trợ lý Bác sĩ Soạn thảo Tóm tắt Hồ sơ Xuất viện (Discharge Summary)

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                     │
│                                                                           │
│ Bài toán (1 câu): Tổng hợp dữ liệu bệnh án điện tử (EMR) để tạo bản nháp  │
│ Tóm tắt Hồ sơ Xuất viện và hướng dẫn chăm sóc tại nhà cho bệnh nhân.      │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes                │
│                     [x] Vinmec   [ ] Khác: ________________________       │
│                                                                           │
│ Ai đang đau (Actor)?                                                      │
│ - Bác sĩ điều trị / Bác sĩ nội trú Vinmec (tốn thời gian hành chính)      │
│ - Bệnh nhân xuất viện (chờ giấy tờ lâu, khó hiểu thuật ngữ y khoa)        │
│                                                                           │
│ Workflow thủ công hiện tại (4 bước):                                      │
│   1. Mở nhiều tab trên hệ thống HIS/EMR (xét nghiệm, thuốc, chẩn đoán)    │
│   ──> 2. Copy-paste và tổng hợp diễn tiến điều trị nội trú (10 phút)      │
│   ──> 3. Viết lời dặn dò uống thuốc, tái khám, dấu hiệu cảnh báo (12 phút)│
│   ──> 4. Bác sĩ chính rà soát, ký số và in giao cho bệnh nhân (5 phút)    │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & Bước 3 (⏱ 22 phút/bệnh án)     │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                     │
│   Bước 2 & Bước 3: LLM trích xuất thông tin có cấu trúc từ EMR, tạo bản   │
│   nháp tóm tắt lâm sàng kèm hướng dẫn tiếng Việt dễ hiểu cho người bệnh.  │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   1. Giảm thời gian bác sĩ soạn thảo từ 25 phút/ca ──> dưới 6 phút/ca.    │
│   2. 100% bản tóm tắt bắt buộc có chữ ký xác nhận của bác sĩ (0% tự gửi). │
│                                                                           │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent       │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## ⚖️ Phản biện Stress-Test (CFO & Trưởng phòng Vận hành) và Lý do Chọn Thẻ #1 (Vinhomes)

Sau khi đưa 3 thẻ bài toán qua bước **Stress-Test dưới góc nhìn CFO & COO**:
1. **Vì sao chưa chọn Thẻ #3 (Vinmec Discharge Summary) làm nguyên mẫu đầu tiên?**
   * Rủi ro lâm sàng và bảo mật dữ liệu bệnh án (Nghị định 13/2023/NĐ-CP) cực kỳ cao. Cần chuẩn hóa hạ tầng HIS/EMR nội bộ trước khi đưa LLM vào tóm tắt đơn thuốc.
2. **Vì sao chọn Thẻ #1 (Vinhomes — AI phân loại và điều phối phản ánh cư dân: Rule lọc khẩn cấp + LLM, CSKH duyệt) để Deep-Dive?**
   * **Giải quyết đúng câu hỏi phản biện của CFO ("Tại sao không dùng Rule thuần túy?"):**
     * Cư dân Vinhomes thường gửi phản ánh bằng ngôn ngữ tự nhiên rất dài, pha trộn nhiều vấn đề trong cùng một tin nhắn (ví dụ: *"Hành lang tầng 12 tòa S2.05 bị rò nước điều hòa chảy lênh láng trước cửa thang máy, đèn cảm biến thì nhấp nháy, bảo vệ dưới sảnh gọi mãi không nghe máy"*), hoặc chọn nhầm danh mục trên App.
     * Nếu chỉ dùng **Rule-based thuần túy**, hệ thống chỉ bắt được từ khóa cứng nhưng không hiểu ngữ cảnh phức tạp để tách việc cho đúng bộ phận (Kỹ thuật M&E vs. Vệ sinh vs. An ninh).
     * Nếu chỉ dùng **LLM thuần túy**, có rủi ro độ trễ hoặc phân loại sót các ca sinh tử.
   * **Kiến trúc Hybrid tối ưu (Rule lọc khẩn cấp + LLM phân loại & soạn nháp + CSKH duyệt):**
     * **Tầng 1 (Rule Engine):** Bắt tức thì (< 1 giây) các tín hiệu nguy kịch P1 (cháy khói, kẹt thang máy, rò khí gas, chập điện/sự cố trạm sạc xe điện tầng hầm B1/B2 có pin nguy kịch `< 5%`) để kích hoạt cảnh báo đỏ ngay lập tức.
     * **Tầng 2 (LLM Feature):** Đọc hiểu ngữ cảnh phản ánh tiếng Việt, bóc tách chính xác bộ phận xử lý (`Kỹ thuật`, `An ninh`, `Vệ sinh`, `Dịch vụ Cư dân`), tóm tắt phiếu giao việc và soạn tin nhắn phản hồi cư dân gắn nhãn `[DRAFT_ONLY]`.
     * **Tầng 3 (CSKH duyệt — HITL):** Nhân viên CSKH kiểm tra bản nháp `[DRAFT_ONLY]` và bấm duyệt chuyển bộ phận trong 30 giây, vừa loại bỏ 100% rủi ro AI hứa hẹn sai quy chế vận hành Vinhomes, vừa giảm tổng thời gian xử lý từ **15 phút xuống dưới 2.5 phút/ticket**.
