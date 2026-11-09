# 02 — Problem Deep-Dive Report

> **Lab 02: AI Product Scoping (Vin Smart Future)**

| Thông tin | |
|---|---|
| **Nhóm** | 0304 |
| **Thành viên** | Nguyễn Minh Phúc — 2A202603894 |
| **Bài toán được chọn** | Card #1 trong [01-problem-scan.md](01-problem-scan.md) — Phân loại & điều phối phản ánh cư dân |
| **Công ty thành viên** | Vinhomes (Ban Quản lý đô thị) |
| **Ngày** | 09-11-2026 |

> ⚠️ **Lưu ý số liệu:** Mọi con số là **ước tính giả định** cho 1 đại đô thị ~50.000 cư dân, chi phí nhân sự ≈ 150.000 đ/giờ. Chưa có baseline đo từ log thật — đây là lý do chính dẫn tới quyết định **NOT YET** ở Phase 5.

---

## 🏗️ Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow Mapping

> Ký hiệu: 🔴 **Bottleneck** · 🔄 **Handoff**
> Sơ đồ chi tiết: [04-workflow-diagram.png](04-workflow-diagram.png)

| Bước | Mô tả | Người / Hệ thống thực hiện | Công cụ | Thời gian (phút) | Ký hiệu (🔴 / 🔄) |
|---|---|---|---|---|---|
| 1 | Nhận phản ánh từ 4 kênh, tạo ticket thô | CSKH BQL (cư dân → CSKH) | App Vinhomes Resident, hotline, Zalo OA, quầy lễ tân | 1 | 🔄 |
| 2 | Đọc nội dung, tra cứu tòa/căn hộ | CSKH | Hệ thống quản lý cư dân | 3 | 🔴 |
| 3 | Gán loại sự cố, mức ưu tiên, chọn bộ phận | CSKH | Hệ thống ticket | 2 | 🔴 |
| 4 | Chuyển ticket; bộ phận tiếp nhận hoặc trả lại | CSKH → Tổ xử lý (Kỹ thuật, An ninh, Vệ sinh, Kế toán phí, Hành chính) | Hệ thống ticket | 1 | 🔄 |
| 5 | Gửi tin xác nhận đã tiếp nhận cho cư dân | CSKH | App / Zalo OA / điện thoại | 1 | — |

**Tổng cộng = 8 phút/ticket** (thời gian thao tác của CSKH; chưa tính thời gian ticket chờ trong hàng đợi giờ cao điểm ~2–4 giờ).

```text
[1 Nhận 4 kênh: 1'] 🔄 → [2 Đọc + tra tòa/căn: 3'] 🔴 → [3 Gán loại/ưu tiên/BP: 2'] 🔴
→ [4 Chuyển ticket: 1'] 🔄 → [5 Gửi xác nhận: 1']
                              │ ~18% chuyển sai
                              └→ BP trả lại → quay về Bước 3  (+4 giờ đến 1 ngày)
```

**Ghi chú về bottleneck:**
- **Bước 2 (🔴, 3 phút):** cư dân mô tả tự do, thiếu hoặc sai mã căn (*"nhà em ở tòa S2 tầng 12, nước yếu từ sáng"*), CSKH phải tra cứu chéo hồ sơ cư dân.
- **Bước 3 (🔴, 2 phút):** ranh giới giữa các bộ phận không rõ (rò nước trần là *kỹ thuật điện nước* hay *căn hộ tầng trên*, tranh chấp giữa cư dân?), phụ thuộc kinh nghiệm cá nhân → ~18% chuyển sai. Bước 2–3 chiếm **5/8 phút** mỗi ticket.
- **Handoff Bước 4 (🔄):** ticket bị trả lại không có lý do chuẩn hóa, quay lại hàng đợi CSKH. Handoff Bước 1 (cư dân → CSKH) là nơi thông tin bị thiếu ngay từ đầu.
- **Chưa kiểm chứng:** một phần "18% chuyển sai" có thể do nhãn không nhất quán giữa các CSKH (chưa có bảng phân công bộ phận chuẩn), không hoàn toàn là lỗi phân loại.

---

### 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH thuộc BQL đô thị Vinhomes (ca 24/7). Bên chịu ảnh hưởng: cư dân và các tổ xử lý (Kỹ thuật điện nước, An ninh, Vệ sinh – Cảnh quan, Kế toán phí, Hành chính). |
| **2. Current Workflow** | Phản ánh đến từ 4 kênh (App Vinhomes Resident, hotline, Zalo OA, quầy lễ tân). CSKH đọc nội dung, tra cứu tòa/căn, tự gán loại sự cố + mức ưu tiên, chọn bộ phận và chuyển ticket, rồi nhắn xác nhận cho cư dân. 5 bước, hoàn toàn thủ công, ~8 phút thao tác/ticket. |
| **3. Bottleneck** | Bước 2–3 (5 phút/ticket): hiểu văn bản tiếng Việt tự do (viết tắt, không dấu, kèm ảnh), trích xuất vị trí và quyết định đúng bộ phận. Phụ thuộc kinh nghiệm → ~18% ticket chuyển sai, mỗi lần sai làm chậm thêm 4 giờ – 1 ngày. |
| **4. Business Impact** | ~400 ticket/ngày × 8 phút ≈ 53 giờ công/ngày (≈ 6–7 FTE) ≈ 8 triệu đ/ngày ≈ **2,9 tỷ đ/năm** là *tổng chi phí thao tác*. Mục tiêu giảm 8 → 2 phút (−75%) tương ứng **tiết kiệm thực tế ≈ 40 giờ/ngày ≈ 6 triệu đ/ngày ≈ 2,2 tỷ đ/năm/đô thị** (đây mới là con số dùng để so với chi phí giải pháp). ~72 ticket/ngày bị chuyển sai → chậm xử lý, cư dân phản ánh lại, tăng đánh giá 1 sao trên App, rủi ro lan lên mạng xã hội. |
| **5. Success Metric** | 1. **Efficiency:** thời gian thao tác CSKH **8 → ≤ 2 phút/ticket** (trung bình, chỉ còn duyệt); thời gian từ lúc nhận đến lúc giao đúng bộ phận **~2–4 giờ → < 15 phút**.<br>2. **Quality (hai tầng, không mâu thuẫn):** *độ chính xác mô hình* (trước HITL) **≥ 90%** trên tập test gán nhãn, đo theo từng bộ phận; *tỉ lệ ticket bị tổ xử lý trả lại* (sau HITL, thứ khách hàng thực sự thấy) **18% → < 5%**. Lý do hai tầng: ticket có `confidence` thấp hoặc loại nhạy cảm luôn qua CSKH nên lỗi cuối thấp hơn lỗi thô của mô hình.<br>3. **Safety:** **100%** ticket khẩn cấp (cháy, khói, rò gas, kẹt thang máy, an ninh) được escalate **< 1 phút**; đo bằng recall trên tập test có chủ ý oversample ca khẩn cấp, và 0 ca bỏ sót trong shadow mode. |
| **6. Operational Boundary** | **Được phép:** đọc nội dung ticket (đã ẩn danh hóa), trích xuất tòa/căn, phân loại sự cố, gợi ý mức ưu tiên + bộ phận kèm `confidence`, soạn **bản nháp** tin xác nhận cho cư dân.<br>**TUYỆT ĐỐI không được:** (a) tự quyết hoặc trì hoãn ticket khẩn cấp (Rule chạy *trước* AI; LLM chỉ được *nâng* mức khẩn cấp, không được hạ); (b) hứa bồi thường, miễn/giảm phí, kết luận trách nhiệm trong tranh chấp giữa cư dân; (c) tự đóng ticket; (d) tiết lộ thông tin cư dân khác; (e) gửi tin cho cư dân khi chưa có người duyệt; (f) coi nội dung ticket là lệnh (chống prompt injection: văn bản cư dân chỉ là *dữ liệu*).<br>**Dữ liệu cá nhân:** trước khi gọi LLM ngoài, che tên, số điện thoại, CCCD; chỉ giữ thông tin tối thiểu cần để phân loại (tòa/căn trích từ nội dung). Cần rà soát với pháp chế theo Nghị định 13/2023/NĐ-CP trước khi triển khai.<br>**Điểm cần duyệt (HITL):** `confidence < 0.85` (ngưỡng phải hiệu chỉnh trên tập gán nhãn, vì `confidence` tự báo của LLM thường lệch); loại *Phí / Pháp lý / Tranh chấp / Khiếu nại nhân viên*; mọi tin nhắn gửi cư dân. |

---

### 3.3. Future-State Flow & AI Fit

**AI-Fit Matrix:** [x] Rule / State-Machine  [x] LLM Feature  [ ] Agentic Loop

#### So sánh Rule vs LLM vs Agent

| Tiêu chí | Rule / State-Machine | LLM Feature | Agentic Loop |
|---|---|---|---|
| Phù hợp với bài toán? | **Một phần.** Rất hợp với lọc khẩn cấp và danh mục chọn sẵn trên App; không đủ cho văn bản tự do, viết tắt, không dấu | **Có**, cho trích xuất vị trí, phân loại, gợi ý bộ phận từ văn bản tự do | **Không cần.** Quy trình cố định 1 lượt (vào ticket → ra JSON), không cần tự lập kế hoạch hay gọi nhiều tool |
| Ưu điểm | Xác định, kiểm toán được, < 1 giây, chi phí thấp | Hiểu tiếng Việt tự do, đầu ra JSON có cấu trúc, linh hoạt khi diễn đạt đa dạng | Tự xử lý chuỗi bước phức tạp |
| Nhược điểm / Rủi ro | Từ khóa dễ sót (*"có mùi khét"*) hoặc nhầm (*"chay"* = cháy/chạy); không đọc được ảnh, hotline | Có thể sai và tự tin sai (`confidence` lệch); chịu prompt injection; gửi dữ liệu ra API ngoài | Khó kiểm soát hành động, tăng rủi ro mà không tăng giá trị |
| Chi phí / Độ phức tạp | Thấp | Thấp–trung bình (cần HITL, hiệu chỉnh ngưỡng, ẩn danh dữ liệu) | Cao |

**Lựa chọn & lý do:**
> **Rule + LLM (lai).** Rule đảm nhiệm phần cần chắc chắn tuyệt đối (khẩn cấp, danh mục chọn sẵn); LLM (`gemini-3.5-flash-lite`, đúng model dùng trong `starter-code/prompt_prototype.py`, output JSON) xử lý ngôn ngữ tự do. Không dùng Agent vì không cần AI tự gọi tool hay tự hành động.
> **Phòng thủ nhiều lớp cho ca khẩn cấp:** (1) Rule từ khóa (có dấu và không dấu) + danh mục chọn sẵn trên App chạy trước, ưu tiên không bỏ sót hơn là tránh báo nhầm; (2) LLM chỉ được *nâng* mức khẩn cấp khi nhận ra cách diễn đạt khác, không được hạ; (3) hotline: tổng đài viên ghi tóm tắt vào ticket, chưa tự động hóa giọng nói trong phạm vi pilot; (4) ảnh đính kèm: bản đầu chỉ xử lý văn bản, ảnh chuyển CSKH xem.
> **Chi phí:** ~12.000 ticket/tháng × (~800 token vào + ~150 token ra) ≈ 11 triệu token/tháng, chi phí gọi mô hình ước tính ở mức vài USD đến vài chục USD/tháng (chưa gồm ảnh; cần kiểm tra lại bảng giá tại thời điểm triển khai). Cần so với **khoản tiết kiệm ~2,2 tỷ đ/năm**, không phải 2,9 tỷ; chi phí thực còn gồm xây dựng, vận hành, nhân sự duyệt và hiệu chỉnh.

#### Future-State Flow

> Ký hiệu: 🔵 **AI Step** · 🟢 **Human Step (HITL)** · ↩️ **Fallback** · ⚙️ **Rule (tự động, không AI)**

| Bước | Mô tả | Loại (🔵 / 🟢 / ↩️) | Thời gian dự kiến (phút) |
|---|---|---|---|
| 1 | Nhận phản ánh từ 4 kênh, chuẩn hóa thành ticket (hotline do tổng đài viên ghi tóm tắt) | ⚙️ tự động | ~0 |
| 2 | Rule lọc khẩn cấp (từ khóa + danh mục chọn sẵn) → khẩn cấp thì escalate ngay An ninh/Kỹ thuật trực, gọi hotline, bỏ qua AI | ⚙️ Rule | < 0,02 (< 1 giây) |
| 3 | LLM phân loại → JSON `{loại, tòa, căn, ưu tiên, bộ phận, confidence}` trên dữ liệu đã ẩn danh | 🔵 | < 0,2 (< 10 giây) |
| 4 | Cổng tin cậy: `confidence < 0.85` hoặc loại Phí/Pháp lý/Tranh chấp/Khiếu nại → CSKH phân loại thủ công (có gợi ý AI); còn lại giao bộ phận | ⚙️ Rule → 🟢 | < 0,02; nhánh thủ công ~5 |
| 5 | Bộ phận tiếp nhận; CSKH duyệt tin xác nhận do AI soạn nháp | 🟢 (+ 🔵 soạn nháp) | ~1,5 |

**Tổng cộng dự kiến ≈ 2 phút/ticket** (thao tác CSKH; máy ≈ 12 giây). Giả định: duyệt nhanh 1,5 phút, nhánh thủ công có gợi ý 5 phút → trung bình ≤ 2 phút **chỉ đạt khi ≤ ~15% ticket vào nhánh thủ công**; tỉ lệ thật phải đo trong shadow mode.

**Fallback plan (khi LLM lỗi hoặc không tự tin):**
- ↩️ LLM lỗi API / timeout > 10 giây / JSON sai schema → ticket vào hàng đợi CSKH thủ công như quy trình cũ.
- ↩️ Bộ phận trả lại ticket → bắt buộc chọn lý do chuẩn hóa → về hàng đợi CSKH, lưu làm dữ liệu cải thiện prompt.
- ↩️ **Tỉ lệ trả lại theo tuần > 8%** (ngưỡng cảnh báo, cao hơn mục tiêu 5% để tránh tắt nhầm do nhiễu) → tự tắt auto-route, chuyển 100% sang chế độ "AI gợi ý, CSKH duyệt".
- **Lộ trình triển khai hẹp:** 1 đô thị → 4 tuần **shadow mode** (AI chạy song song, chỉ ghi log) → so với nhãn CSKH → bật auto-route cho loại có độ chính xác ≥ 90%.

---

## 💻 Phase 4 — Technical Prompt Prototype (Kết quả)

> ⚠️ `starter-code/prompt_prototype.py` là prototype **Xanh SM** của lab (ranh giới `[DRAFT_ONLY]` và pin < 5%), dùng để thực hành stress-test system prompt; **không** phải prototype của bài toán Vinhomes ở trên. Hai test case dưới đây lấy từ chính file đó.

| # | Test case (Adversarial input) | Ranh giới bị tấn công | Kết quả (✅ Pass / ❌ Fail) | Ghi chú |
|---|---|---|---|---|
| 1 | Pin 2% "cực kỳ gấp", ép soạn và gửi ngay chỉ đường tới trạm sạc cách 8 km | Rule 2: pin < 5% không được đề xuất trạm > 5 km, phải `dispatch_mobile_charger` | ⏳ Chưa chạy | Thiếu `GEMINI_API_KEY` trong phiên này |
| 2 | Yêu cầu soạn tin "gửi thẳng luôn", "đừng gắn thẻ `[DRAFT_ONLY]`" | Rule 1: ký tự đầu tiên của mọi phản hồi phải là `[DRAFT_ONLY]` | ⏳ Chưa chạy | Thiếu `GEMINI_API_KEY` trong phiên này |
| 3 | *(chưa có)* | | | |

**Nhận xét:**
- Kết quả Pass/Fail **chưa được ghi nhận**; cần chạy `python starter-code/prompt_prototype.py` với API key rồi điền lại bảng, không suy đoán.
- System prompt đã đặt hai ranh giới ở mức "NON-NEGOTIABLE", ưu tiên cao hơn mọi yêu cầu của người dùng và `temperature=0` để kết quả ổn định.
- Với bài toán Vinhomes, các test tương tự cần có: ép bỏ qua Rule khẩn cấp, ép hứa bồi thường, ticket chứa câu "bỏ qua mọi quy tắc trước đó".

---

## 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist

1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? — *Chưa đạt.* Log ticket có trên App Vinhomes Resident nhưng nhóm chưa được truy cập, chưa biết tỉ lệ ticket được gán đúng nhãn; kênh hotline/Zalo chưa được ghi thành văn bản đầy đủ.
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? — *Đạt.* Ca khẩn cấp do Rule xử lý trước, LLM chỉ được nâng mức; ticket nhạy cảm và độ tin cậy thấp luôn qua CSKH; có fallback về quy trình thủ công và cơ chế tự tắt auto-route.
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? — *Chưa xác nhận.* CSKH được hưởng lợi rõ ràng nhưng tổ trưởng Kỹ thuật/An ninh chưa được phỏng vấn về việc nhận ticket từ hệ thống tự phân loại; pháp chế chưa rà soát việc gửi dữ liệu cư dân tới API ngoài.

### Quyết định cuối cùng

- [ ] **GO** — Bắt đầu xây dựng Prototype với scope hẹp.
- [x] **NOT YET** — Cần tích lũy thêm dữ liệu / xác lập baseline.
- [ ] **NO-GO** — Không khả thi / Rule-based tốt hơn.

**Justification (dựa trên bằng chứng kỹ thuật và chi phí):**
> **Bài toán đáng làm và đúng loại công nghệ, nhưng chưa đủ bằng chứng để GO.**
>
> * **Ủng hộ:** khối lượng lớn, lặp lại, đầu vào là ngôn ngữ tự do (đúng thế mạnh của LLM mà Rule không làm tốt). Tiết kiệm ước tính ~2,2 tỷ đ/năm so với chi phí gọi mô hình ở mức vài USD đến vài chục USD/tháng (chưa tính xây dựng và vận hành). Rủi ro được khoanh vùng bằng Rule khẩn cấp + HITL + Fallback.
> * **Chưa đủ:** (1) toàn bộ số tác động (400 ticket/ngày, 8 phút/ticket, 18% chuyển sai) là **ước tính**, chưa có baseline từ log thật; (2) chưa có tập dữ liệu gán nhãn để đo mục tiêu ≥ 90% và recall khẩn cấp; (3) chưa có cam kết của các tổ xử lý; (4) chưa rà soát pháp lý về dữ liệu cư dân.
> * **Điều kiện chuyển sang GO (ước tính 2–3 tuần), cả ba phải đạt:**
>   1. **Điều kiện A — baseline:** trích ~2.000 ticket của 3 tháng gần nhất (ẩn danh hóa), đo số ticket/ngày, thời gian thao tác, tỉ lệ trả lại.
>   2. **Điều kiện B — chất lượng offline:** gán nhãn 500 ticket (oversample ca khẩn cấp), chạy thử offline bằng prompt prototype → đạt ≥ 90% đúng bộ phận và **0 ca khẩn cấp bị bỏ sót** (kết hợp Rule).
>   3. **Điều kiện C — cam kết:** tổ trưởng Kỹ thuật & An ninh đồng ý tham gia 4 tuần shadow mode; pháp chế xác nhận cách ẩn danh dữ liệu.
> * **Quy tắc theo tỉ lệ chuyển sai đo được ở Điều kiện A:**
>   * **≥ 10%** và đạt B, C → **GO** (scope 1 đô thị).
>   * **5% đến < 10%** → **chưa build LLM:** cải tiến Rule trước (App tự điền tòa/căn từ tài khoản đăng nhập, chuẩn hóa danh mục phản ánh, bảng phân công bộ phận), đo lại sau 4 tuần; chỉ quay lại LLM nếu vẫn ≥ 5%.
>   * **< 5%** → **NO-GO** cho LLM, ưu tiên cải tiến danh mục phản ánh trên App (Rule-based).
