# Lab 02 — Phase 1 & Phase 2: Problem Scan & Quick Problem Cards (Vin Smart Future)

**Họ và tên:** Đức Thắng (`ducthang`) — Trưởng nhóm  
**Đơn vị:** Vin Smart Future (Vingroup)  

---

## 🔍 Phase 1 — SCAN: Quét cơ hội tối ưu vận hành bằng 4 Lenses

Sử dụng **4 Lenses** (*Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác*) để rà soát các điểm nghẽn (bottlenecks) thực tế tại các công ty thành viên thuộc Tập đoàn Vingroup:

| # | Công ty thành viên | Lens | Mô tả ngắn bài toán & Tổn thất ước tính |
|---|---|---|---|
| **1** | **Xanh SM (GSM)** | **Tốn thời gian** | **Xử lý sự cố pin thực địa & điều phối trạm sạc/xe cứu hộ:** Điều phối viên phải tra cứu thủ công tọa độ GPS của xe, tìm trụ sạc VinFast còn trống phù hợp dòng xe (VF5/VFe34/VF8) và soạn tin nhắn chỉ dẫn hoặc gọi xe cứu hộ pin di động (mất ~15 phút/lượt, ~80 ca/ngày tại Hà Nội). |
| **2** | **Vinhomes** | **AI có thể tốt hơn** | **Phân loại & điều hướng phản ánh cư dân trên App Vinhomes Resident:** Hàng trăm phản ánh mỗi ngày (mất nước, hỏng thang máy, ồn ào, phí dịch vụ) đang được CSKH đọc và phân luồng thủ công về từng Ban Quản lý tòa nhà; phản hồi rập khuôn và SLA xử lý kéo dài 8–12 giờ. |
| **3** | **VinFast** | **Lặp lại** | **Đối chiếu hóa đơn sạc điện và sản lượng trạm sạc nhượng quyền/đối tác:** Kế toán vận hành phải rà soát hàng chục nghìn bản ghi phiên sạc (session logs) mỗi tuần với bảng kê thanh toán từ đối tác mặt bằng, dễ lệch số liệu công tơ và ngốn 3 ngày công/tuần. |
| **4** | **Vinmec** | **Pain từ người khác** | **Soạn thảo Tóm tắt Hồ sơ Xuất viện (Discharge Summary):** Bác sĩ nội trú mất 20–30 phút/bệnh nhân để tổng hợp kết quả xét nghiệm, chẩn đoán hình ảnh, diễn tiến điều trị và dặn dò sau xuất viện bằng ngôn ngữ dễ hiểu cho bệnh nhân, gây quá tải giờ hành chính. |
| **5** | **Xanh SM (GSM)** | **Pain từ người khác** | **Phân tích nguyên nhân hủy chuyến từ ghi âm tổng đài & ghi chú tài xế:** Mỗi ngày có hàng nghìn cuốc xe bị hủy giờ cao điểm nhưng dữ liệu nằm rải rác ở file audio và text tự do, khiến đội Vận hành không tách bạch được lỗi định vị App, tài xế ngập ngừng hay khách đợi lâu. |
| **6** | **Vinpearl** | **Tốn thời gian** | **Bóc tách yêu cầu đặt phòng đoàn (Group Booking Email) từ đại lý lữ hành (OTA/TA):** Nhân viên kinh doanh phải đọc các email dài kèm bảng Excel không chuẩn hóa để kiểm tra quỹ phòng (allotment) và soạn báo giá nháp, mất 25–40 phút/yêu cầu. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Từ 6 bài toán trên, lựa chọn **Top 3 bài toán** có tác động vận hành rõ rệt nhất (**#1 Xanh SM Sự cố sạc pin**, **#2 Vinhomes Phân loại phản ánh cư dân**, **#4 Vinmec Tóm tắt hồ sơ xuất viện**) để lập thẻ đánh giá nhanh:

---

### Thẻ #1: Xanh SM — Điều phối sự cố sạc pin thực địa & Xe cứu hộ pin di động (Bài toán trọng tâm)

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                     │
│                                                                           │
│ Bài toán (1 câu): Tự động tra cứu trạm sạc trống phù hợp hoặc kích hoạt   │
│ xe cứu hộ pin di động và soạn chỉ dẫn nháp khi tài xế báo sự cố pin.      │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes                │
│                     [ ] Vinmec   [ ] Khác: ________________________       │
│                                                                           │
│ Ai đang đau (Actor)?                                                      │
│ - Điều phối viên (Dispatcher) Trung tâm Điều vận Xanh SM (quá tải tra cứu)│
│ - Tài xế taxi điện Xanh SM (chờ đợi lâu trên đường, nguy cơ sập nguồn)    │
│                                                                           │
│ Workflow thủ công hiện tại (5 bước):                                      │
│   1. Tài xế gọi hotline điều vận báo sắp cạn pin / trụ sạc lỗi (2 phút)   │
│   ──> 2. Dispatcher tra cứu tọa độ GPS & % pin xe trên bản đồ (2 phút)    │
│   ──> 3. Tra cứu Dashboard trạm sạc VinFast tìm trụ trống phù hợp (5 phút)│
│   ──> 4. Soạn tin nhắn chỉ đường chi tiết gửi qua App Tài xế (5 phút)     │
│   ──> 5. Gọi đội Xe cứu hộ pin di động nếu pin nguy kịch < 5% (1 phút)    │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & Bước 4 (⏱ 10–12 phút/lượt)     │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                     │
│   Bước 3, 4 & 5: Tự động đối chiếu % pin + khoảng cách trạm sạc trống,    │
│   kiểm tra ngưỡng an toàn (< 5% pin cấm đi xa > 5km -> điều xe cứu hộ),   │
│   và sinh bản nháp [DRAFT_ONLY] để Dispatcher duyệt gửi trong 1 cú click. │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   1. Giảm thời gian xử lý trung bình từ 15 phút/lượt ──> dưới 3 phút/lượt.│
│   2. 100% ca pin nguy kịch (< 5%) được điều xe cứu hộ thay vì đi xa >5km. │
│   3. Độ chính xác gợi ý đúng cổng sạc/trạm trống đạt >= 98%.              │
│                                                                           │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent       │
└───────────────────────────────────────────────────────────────────────────┘
```

---

### Thẻ #2: Vinhomes — Phân loại & Soạn phản hồi nháp cho Phản ánh Cư dân

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                     │
│                                                                           │
│ Bài toán (1 câu): Phân loại mức độ khẩn cấp, gán đúng bộ phận xử lý và    │
│ soạn phản hồi nháp cho khiếu nại/phản ánh của cư dân trên App Vinhomes.   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes                │
│                     [ ] Vinmec   [ ] Khác: ________________________       │
│                                                                           │
│ Ai đang đau (Actor)?                                                      │
│ - Nhân viên CSKH Ban Quản lý Vinhomes (đọc thủ công hàng trăm ticket/ngày)│
│ - Cư dân Vinhomes (chờ phản hồi lâu, nhận tin nhắn mẫu rập khuôn)         │
│                                                                           │
│ Workflow thủ công hiện tại (4 bước):                                      │
│   1. Cư dân gửi phản ánh (kèm ảnh/text) lên App Vinhomes Resident (1 phút)│
│   ──> 2. CSKH đọc ticket, phân loại nhóm sự cố & mức độ ưu tiên (3 phút)  │
│   ──> 3. Chuyển tiếp (route) đến bộ phận Kỹ thuật / An ninh / Vệ sinh (2p)│
│   ──> 4. Soạn tin nhắn phản hồi xác nhận và hẹn SLA với cư dân (6 phút)   │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & Bước 4 (⏱ 9 phút/ticket)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                     │
│   Bước 2 & Bước 4: LLM đọc nội dung tiếng Việt tự nhiên, gán nhãn         │
│   (Urgency, Department) và tự động soạn bản nháp phản hồi cá nhân hóa     │
│   để CSKH kiểm duyệt trước khi gửi.                                       │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   1. Giảm thời gian xử lý & phản hồi đầu tiên (FRT) từ 9 phút ──> < 2 phút│
│   2. Tỷ lệ phân luồng đúng bộ phận kỹ thuật ngay lần đầu đạt >= 92%.      │
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

## ⚖️ Phản biện Stress-Test (CFO & Trưởng phòng Vận hành) và Lý do Chọn Thẻ #1

Sau khi đưa 3 thẻ bài toán qua bước **Stress-Test dưới góc nhìn CFO & COO**:
1. **Vì sao chưa chọn Thẻ #3 (Vinmec Discharge Summary) làm nguyên mẫu đầu tiên?**
   * Rủi ro lâm sàng và bảo mật dữ liệu y tế (HIPAA / Nghị định 13/2023/NĐ-CP) cực kỳ cao. Chỉ một sai lệch nhỏ về liều lượng thuốc trong bản tóm tắt cũng có thể gây hậu quả nghiêm trọng, đòi hỏi tích hợp sâu với hệ thống HIS/EMR nội bộ trước khi thử nghiệm.
2. **Vì sao chưa chọn Thẻ #2 (Vinhomes CSKH)?**
   * Nhiều khiếu nại liên quan đến biểu phí quản lý, tranh chấp hợp đồng hoặc quy định PCCC có thể giải quyết 60% bằng menu phân loại trên App (Rule-based form) trước khi cần đến LLM.
3. **Vì sao chọn Thẻ #1 (Xanh SM — Điều phối sự cố sạc pin thực địa) để Deep-Dive?**
   * **Đau đớn tức thời (Real-time Operational Bottleneck):** Mỗi phút xe nằm chờ chỉ dẫn trên đường là mất doanh thu cuốc xe và gây ùn tắc/căng thẳng cho tài xế.
   * **Sự kết hợp hoàn hảo giữa Rule + LLM Feature:** Rule kiểm soát ngưỡng vật lý cứng (`pin < 5%` không được đi quá `5km`), trong khi LLM giải quyết bài toán ngôn ngữ tự nhiên (hiểu ngữ cảnh tài xế đang cuống, giải thích lý do an toàn và soạn chỉ dẫn rõ ràng bằng tiếng Việt với nhãn `[DRAFT_ONLY]`).
   * **Kiểm soát rủi ro tuyệt đối:** Nhờ cơ chế **Human-in-the-loop (Dispatcher duyệt nháp)** và **Fallback rõ ràng**, chi phí triển khai thấp nhưng rút ngắn thời gian xử lý từ **15 phút xuống dưới 3 phút**.
