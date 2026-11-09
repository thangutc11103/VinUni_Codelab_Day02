# 01 — Problem Scan & Quick Assess

> **Lab 02: AI Product Scoping (Vin Smart Future)**

| Thông tin | |
|---|---|
| **Họ và tên** | Nguyễn Minh Phúc |
| **Mã sinh viên** | 2A202603894 |
| **Nhóm** | |
| **Branch cá nhân** | NguyenMinhPhuc |
| **Ngày** | 09-11-2026 |

---

## 🔍 Phase 1 — SCAN

**4 Lenses:**
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ hiện tại còn chậm hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn.

### 📝 List bài toán của tôi

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinhomes** | Lặp lại | CSKH đọc và phân loại thủ công ~400 phản ánh cư dân/ngày (App Resident, hotline, Zalo, lễ tân) rồi chuyển cho bộ phận kỹ thuật; mất ~8 phút/ticket, ~18% bị chuyển sai bộ phận. |
| 2 | **VinFast** | AI có thể tốt hơn | Cố vấn dịch vụ tại xưởng tự diễn giải mô tả lỗi bằng tiếng Việt của khách (*"đi qua gờ giảm tốc kêu cụp cụp bánh trước"*) thành hạng mục kiểm tra sơ bộ; mất 10–15 phút/xe, thường phải hỏi lại khách. |
| 3 | **Vinhomes** | Lặp lại | Kế toán Ban Quản lý (BQL) dò sao kê ngân hàng để đối soát phí dịch vụ khi cư dân chuyển khoản ghi sai/thiếu mã căn (~5–10% giao dịch); mất 3–5 ngày công mỗi kỳ thu phí. |
| 4 | **Vinpearl** | Tốn thời gian | Nhân viên Sales đọc email đặt phòng theo đoàn (Group Booking) không theo mẫu từ đại lý lữ hành, nhập tay vào hệ thống quản lý khách sạn; 20–30 phút/email, dễ sai ngày/loại phòng. |
| 5 | **Vinhomes** | AI có thể tốt hơn | BQL duyệt hồ sơ đăng ký thi công nội thất bằng cách đối chiếu thủ công với quy định tòa nhà; thời gian duyệt 3–5 ngày, 40–50% hồ sơ bị trả lại ít nhất 1 lần. |
| 6 | **Vincom Retail** | Pain từ người khác | Khách thuê mặt bằng tại TTTM gửi yêu cầu kỹ thuật/đăng ký sự kiện qua email và điện thoại; BQL TTTM tổng hợp bằng Excel, phản hồi chậm 2–3 ngày khiến khách thuê phàn nàn. |
| 7 | **Vinmec** | Pain từ người khác | Tổng đài viên đặt lịch hỏi triệu chứng để chọn chuyên khoa; 5–8 phút/cuộc gọi, một phần lịch hẹn bị xếp sai chuyên khoa khiến bệnh nhân phải chuyển khám. |
| 8 | **Vinschool** | Tốn thời gian | Giáo viên chủ nhiệm trả lời lặp lại tin nhắn phụ huynh về lịch học, học phí, thực đơn, thủ tục; ước tính 1–2 giờ/ngày/giáo viên. |
| 9 | **VinBus** | Lặp lại | Bộ phận chất lượng tổng hợp thủ công phản ánh hành khách (trễ chuyến, thái độ, điều hòa) từ hotline/fanpage thành báo cáo tuần; mất 1–2 ngày công/tuần, phát hiện vấn đề chậm. |


---

## 🃏 Phase 2 — QUICK-ASSESS

Chọn top 3 từ danh sách SCAN: **#1 (Phân loại phản ánh cư dân)**, **#3 (Đối soát phí dịch vụ)**, **#5 (Duyệt hồ sơ thi công)** — cả ba thuộc **Vinhomes**.

> **Vì sao top 3 đều thuộc Vinhomes?** Ba bài toán chia sẻ cùng Actor chính (Ban Quản lý đô thị — BQL) và cùng nguồn dữ liệu (App Vinhomes Resident), nên có thể so sánh công bằng mức độ phù hợp của **Rule / LLM / Agent** trên cùng một bối cảnh vận hành, thay vì so sánh các bài toán khác hẳn nhau về dữ liệu và rủi ro.

> ⚠️ **Lưu ý số liệu:** Mọi con số dưới đây là **ước tính** để scoping (chưa có dữ liệu thật của Vingroup). Cột "Nguồn baseline" chỉ ra nơi cần lấy số đo thực tế trước khi build.

---

### Quick Problem Card #1 — Phân loại & điều phối phản ánh cư dân *(SCAN #1)*

| Field | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Phản ánh cư dân (mất nước, hỏng đèn, ồn ào, rò rỉ, an ninh...) được CSKH đọc và chuyển thủ công tới bộ phận xử lý — chậm và hay chuyển sai. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác |
| **Ai đang đau (Actor)?** | **CSKH BQL** (quá tải ~400 ticket/ngày/đô thị); **cư dân** (chờ lâu); **tổ kỹ thuật** (nhận ticket sai, phải trả lại). |
| **Workflow thủ công hiện tại** | 1. Nhận phản ánh từ App / hotline / Zalo / lễ tân → 2. Đọc nội dung, tra cứu tòa / căn hộ → 3. Gán loại sự cố + mức ưu tiên, chọn bộ phận → 4. Chuyển ticket; bộ phận tiếp nhận (hoặc trả lại) → 5. Gửi xác nhận đã tiếp nhận cho cư dân |
| **Bước tốn thời gian/lỗi nhất** | **Bước 2–3** (⏱ ~5 / 8 phút tổng mỗi ticket; ~18% ticket chuyển sai bộ phận) |
| **AI hỗ trợ ở bước nào?** | **Bước 2–3:** LLM trích xuất vị trí (tòa/căn) → phân loại sự cố → gợi ý mức ưu tiên & bộ phận; CSKH duyệt 1 click. **Bước 5:** soạn nháp tin xác nhận (CSKH duyệt trước khi gửi). |
| **Metric thành công (có số)** | • Tỉ lệ chuyển sai bộ phận: **18% → < 5%** <br>• Thời gian CSKH xử lý: **8 phút → < 2 phút/ticket** <br>• *Guardrail:* **100%** sự cố khẩn cấp (cháy, rò gas, kẹt thang máy, ngập) được chuyển trong **< 1 phút** |
| **Nguồn baseline** | Log ticket trên hệ thống CSKH (thời điểm tạo → chuyển → trả lại) của 1–3 tháng gần nhất. |
| **Ranh giới AI** | AI **chỉ gợi ý**, không tự đóng ticket, không hứa thời gian sửa chữa. Ca khẩn cấp đi luồng **Rule** (từ khóa) và báo động ngay — **không chờ LLM**. Độ tin cậy thấp → CSKH xử lý thủ công. |
| **Quick Architecture** | [ ] No AI  [x] Rule (lọc khẩn cấp)  [x] LLM (phân loại & trích xuất)  [ ] Agent |

---

### Quick Problem Card #2 — Đối soát chuyển khoản phí dịch vụ *(SCAN #3)*

| Field | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Cư dân chuyển khoản phí dịch vụ với nội dung ghi sai/thiếu mã căn; kế toán phải dò và xác nhận thủ công. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác |
| **Ai đang đau (Actor)?** | **Kế toán công nợ BQL** (dò tay sao kê); **cư dân** (bị nhắc nợ nhầm dù đã đóng). |
| **Workflow thủ công hiện tại** | 1. Tải sao kê ngân hàng cuối ngày → 2. Hệ thống tự khớp theo mã căn + số tiền (~90–95%) → 3. Kế toán dò tay giao dịch không khớp (~5–10%) → 4. Gọi điện / nhắn cư dân xác nhận → 5. Ghi nhận thanh toán, cập nhật công nợ |
| **Bước tốn thời gian/lỗi nhất** | **Bước 3–4** (⏱ 5–10 phút/giao dịch; tổng **3–5 ngày công** mỗi kỳ thu phí) |
| **AI hỗ trợ ở bước nào?** | **Bước 3:** đọc nội dung chuyển khoản tự do (VD: *"phi dv t10 can 1205 S2.03"*) → gợi ý căn hộ khớp nhất; kế toán xác nhận. |
| **Metric thành công (có số)** | • Tỉ lệ khớp tự động: **~92% → ≥ 98%** <br>• Công sức đối soát phần dư: **3–5 ngày công → ≤ 1 ngày công/kỳ** <br>• *Guardrail:* **0** giao dịch bị ghi nhận sai căn mà không có người duyệt |
| **Nguồn baseline** | Sao kê ngân hàng + file đối soát của kế toán 2–3 kỳ thu phí gần nhất. |
| **Ranh giới AI** | AI **không được tự ghi nhận thanh toán** — chỉ đề xuất căn hộ; mọi ghi nhận do kế toán xác nhận (bài toán tài chính, sai sẽ gây tranh chấp). |
| **Quick Architecture** | [ ] No AI  [x] Rule (chính: mã căn + số tiền + regex)  [x] LLM (chỉ phần dư)  [ ] Agent |

---

### Quick Problem Card #3 — Duyệt hồ sơ đăng ký thi công nội thất *(SCAN #5)*

| Field | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Hồ sơ đăng ký thi công nội thất bị duyệt chậm và trả lại nhiều lần do thiếu giấy tờ hoặc vi phạm quy định tòa nhà. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác |
| **Ai đang đau (Actor)?** | **Kỹ thuật viên BQL** (đối chiếu thủ công); **cư dân & nhà thầu** (chờ đợi, nộp lại nhiều lần). |
| **Workflow thủ công hiện tại** | 1. Cư dân nộp đơn + bản vẽ (PDF/ảnh) qua App hoặc quầy → 2. Kỹ thuật viên đối chiếu quy định tòa (PCCC, kết cấu, giờ thi công, mặt ngoài) → 3. Trả lại danh sách điểm cần bổ sung → 4. Cư dân nộp lại → duyệt lại → 5. Cấp phép thi công, thu tiền cọc |
| **Bước tốn thời gian/lỗi nhất** | **Bước 2** (⏱ 20–30 phút/lượt duyệt; 40–50% hồ sơ bị trả lại ≥ 1 lần) |
| **AI hỗ trợ ở bước nào?** | **Bước 1–2:** tiền kiểm hồ sơ theo checklist quy định (thiếu giấy tờ, giờ thi công, hạng mục cấm) **trước khi** cư dân nộp chính thức. |
| **Metric thành công (có số)** | • Tỉ lệ hồ sơ bị trả lại: **45% → < 20%** <br>• Thời gian duyệt: **3–5 ngày (TB ~4) → ≤ 1,5 ngày** |
| **Nguồn baseline** | Sổ/hệ thống theo dõi hồ sơ thi công của BQL (ngày nộp, số lần trả lại, lý do). |
| **Ranh giới AI** | AI **không được cấp phép** và không đánh giá kết cấu/PCCC — chỉ chỉ ra điểm thiếu theo checklist; quyết định cấp phép thuộc kỹ thuật viên. |
| **Quick Architecture** | [ ] No AI  [ ] Rule  [x] LLM (RAG trên bộ quy định tòa nhà)  [ ] Agent |

---

## 🎯 Bài toán được chọn để Deep-Dive

### Ma trận so sánh (thang 1–5, càng cao càng tốt)

| Tiêu chí | Card #1 — Phản ánh cư dân | Card #2 — Đối soát phí | Card #3 — Hồ sơ thi công |
|---|:---:|:---:|:---:|
| Khối lượng & tần suất | 5 (~400 ticket/ngày) | 3 (theo kỳ thu phí) | 2 (theo đợt bàn giao) |
| Dữ liệu sẵn sàng | 4 (log ticket có sẵn) | 4 (sao kê có sẵn) | 2 (quy định chưa số hóa) |
| Rủi ro khi AI sai (thấp = điểm cao) | 4 (có HITL, có thể trả lại) | 2 (sai tiền → tranh chấp) | 3 (có người duyệt cuối) |
| Mức phù hợp với LLM | 5 (văn bản tự do, đa kênh) | 2 (Rule đã giải ~92%) | 3 (cần đa phương thức) |
| Dễ đo metric | 5 | 4 | 3 |
| **Tổng** | **23** | **15** | **13** |

- **Card được chọn:** **#1 — Phân loại & điều phối phản ánh cư dân Vinhomes** (Deep-Dive tại [02-deep-dive-report.md](02-deep-dive-report.md)).
- **Lý do chọn:**
  - Khối lượng lớn, lặp lại hằng ngày; dữ liệu văn bản tự do đa kênh là thế mạnh của LLM.
  - Sai sót kiểm soát được: bộ phận nhận sai có thể trả lại, CSKH duyệt các ca AI không chắc chắn, ca khẩn cấp đi luồng Rule riêng.
  - Metric đo được ngay từ log ticket sẵn có.
- **Lý do loại các thẻ khác:**
  - **Card #2 (loại):** Rule-based (mã căn + số tiền + regex) đã giải phần lớn; LLM chỉ xử lý ~5–10% phần dư nên ROI nhỏ. Đây là bài toán **tài chính** — gán nhầm thanh toán sang căn khác gây tranh chấp, nên ưu tiên cải tiến rule trước.
  - **Card #3 (chưa làm lúc này):** Đầu vào là bản vẽ PDF/ảnh (cần mô hình đa phương thức); quy định khác nhau theo từng dự án và **chưa được số hóa** thành văn bản chuẩn để làm RAG. Cần chuẩn hóa bộ quy định trước — phù hợp làm giai đoạn 2.
- **Phản biện khi stress-test (CFO / Trưởng phòng Vận hành) & cách trả lời:**
  - *"Rule theo từ khóa có thể phân loại được, cần gì LLM?"* → Rule vẫn dùng cho ca khẩn cấp; nhưng phản ánh viết tự do, đa kênh, nhiều cách diễn đạt (*"nước nhỏ giọt trần WC"* = rò rỉ, thuộc tổ kỹ thuật nước) nên rule thuần khó đạt < 5% chuyển sai.
  - *"Con số 18% chuyển sai lấy từ đâu?"* → Hiện là ước tính; phải đo baseline từ log ticket trước khi quyết định build (xem Phase 5).
  - *"Nếu AI phân loại sai một ca khẩn cấp thì sao?"* → Ca khẩn cấp không phụ thuộc LLM: luồng Rule báo động ngay, LLM chỉ xử lý ca thường.
