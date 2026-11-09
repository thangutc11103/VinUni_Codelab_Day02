# 03 — AI Log & Reflection

> **Lab 02: AI Product Scoping (Vin Smart Future)**

| Thông tin | |
|---|---|
| **Họ và tên** | Nguyễn Minh Phúc |
| **Mã sinh viên** | 2A202603894 |
| **Nhóm** | 0304 |
| **Branch cá nhân** | NguyenMinhPhuc |
| **Ngày** | 09-11-2026 |

---

## 1. Công cụ AI đã sử dụng

| Công cụ | Dùng cho phase nào | Mục đích |
|---|---|---|
| **Claude Code** (trợ lý AI trong VS Code) | Phase 1–3, 5, sơ đồ workflow | Tóm tắt yêu cầu lab, tạo khung file nộp bài, góp ý & phản biện Quick Cards và Deep-Dive Report, vẽ `04-workflow-diagram.png` |
| **Google Gemini** (qua `google-genai` SDK) | Phase 4 — Prompt Prototype | Mô hình được stress-test bằng system prompt và adversarial input trong `starter-code/prompt_prototype.py` |

---

## 2. AI đã giúp gì?

- **Hiểu nhanh yêu cầu:** AI đọc README, worksheet, autograder và tóm tắt các phase, cách chấm điểm, file cần nộp — giúp tôi biết ngay còn thiếu gì (sơ đồ workflow, prototype).
- **Làm "người phản biện":** AI soát bản nháp Quick Cards và Deep-Dive, chỉ ra số liệu không khớp giữa các file, lỗi lập luận về chi phí, lỗ hổng an toàn (ca khẩn cấp, dữ liệu cá nhân) và khoảng trống trong điều kiện ra quyết định.
- **Cấu trúc hóa lựa chọn:** Gợi ý ma trận chấm điểm 3 thẻ (khối lượng, dữ liệu, rủi ro, mức phù hợp LLM, khả năng đo) để việc chọn đề tài có căn cứ thay vì cảm tính.
- **Trực quan hóa:** Vẽ sơ đồ current-state và future-state bằng tiếng Việt, có đủ ký hiệu bottleneck / handoff / AI step / HITL / fallback.
- **Viết lại cho đúng người đọc:** Rút gọn đoạn lý do chọn đề tài cho giảng viên và nhóm.

---

## 3. AI trả lời sai / Hallucination ở đâu?

| # | Tình huống | AI đã trả lời gì | Vì sao sai / Cách phát hiện |
|---|---|---|---|
| 1 | Số liệu bài toán (Phase 1–3) | Đưa ra các con số cụ thể: ~400 ticket/ngày, 8 phút/ticket, ~18% chuyển sai, 2,9 tỷ đ/năm | Không có nguồn — là **ước tính giả định**, không phải dữ liệu thật của Vinhomes. Tôi yêu cầu ghi rõ "ước tính" ở mọi file và đưa việc đo baseline thành điều kiện bắt buộc ở Phase 5 (quyết định **NOT YET**). |
| 2 | Lập luận chi phí trong Deep-Dive | So chi phí gọi LLM (< 10 USD/tháng) với **toàn bộ** chi phí CSKH 2,9 tỷ đ/năm | Sai logic: AI chỉ giảm thời gian từ 8 → 2 phút, nên phần tiết kiệm tối đa ~75% (~2,2 tỷ), không phải 2,9 tỷ. Phát hiện khi tự kiểm tra lại phép tính trong lượt review. |
| 3 | Số liệu giữa các file | Card #1 ghi "5 phút/ticket" nhưng metric lại bắt đầu từ "8 phút"; Card đối soát ghi "4 ngày" trong khi bảng SCAN ghi "3–5 ngày công" | Không nhất quán giữa bảng SCAN và Quick Card. Phát hiện bằng cách đối chiếu chéo từng con số giữa hai phần. |
| 4 | Ranh giới an toàn | Khẳng định Rule từ khóa giúp "**0 ca khẩn cấp bị bỏ sót**" | Nói quá: từ khóa có thể sót cách diễn đạt khác (*"có mùi khét"*), và Rule không đọc được cuộc gọi hotline hay ảnh. Đã bổ sung lớp 2: LLM chỉ được **nâng** mức khẩn cấp, không được hạ; hotline do tổng đài viên ghi tóm tắt. |
| 5 | Tên model | Report ghi "Gemini 2.5 Flash", trong khi code dùng `gemini-3.5-flash-lite` (và banner trong code vẫn in "Gemini 2.5 Flash") | Không khớp giữa tài liệu và code. Cần thống nhất 1 tên model và kiểm tra lại bảng giá tương ứng. |
| 6 | AI tự làm thay tôi | AI tự viết phần "phản biện của CFO" và tự đổi số thẻ #1/#3/#5 → #1/#2/#3 | Đây không phải phản biện thật tôi nhận được khi stress-test, và tôi chưa yêu cầu đổi số thẻ. AI đã tự nêu rõ hai điểm này để tôi kiểm tra — bài học là phải đọc lại mọi thứ AI viết "thay" mình. |
| 7 | Trạng thái công việc | File kế hoạch ghi "Completed" cho 3 file `.md` | Thực tế 3 file đang trống (0 byte). Không được tin vào báo cáo trạng thái — phải mở file kiểm tra. |

---

## 4. Tôi đã sửa prompt / thiết lập ranh giới thế nào?

| # | Prompt ban đầu | Prompt đã chỉnh sửa | Kết quả sau khi sửa |
|---|---|---|---|
| 1 | *"viết 1 đoạn mô tả, phân tích ngắn tại sao lại dùng topic này"* | *"viết lại ngắn gọn hơn và không đưa quá nhiều số liệu"* | Bản đầu dài, dày đặc con số ước tính; bản sau còn 1 đoạn, tập trung vào lý do (lặp lại, phù hợp LLM, rủi ro kiểm soát được). |
| 2 | *"improve it for me"* (chỉ dán bản nháp Phase 2) | Duyệt kế hoạch cụ thể trước khi AI sửa: liệt kê từng điểm sẽ thay đổi | AI chỉ sửa đúng phạm vi đã duyệt, không đụng Phase 1; mọi thay đổi đều được liệt kê lại để kiểm tra. |
| 3 | System prompt prototype: khung `TODO` của starter code | Ranh giới viết thành **"NON-NEGOTIABLE"**, ưu tiên cao hơn mọi yêu cầu của người dùng (kể cả "bỏ qua", "gửi thẳng", giả danh khẩn cấp); `[DRAFT_ONLY]` bắt buộc ở **ký tự đầu tiên** của mọi phản hồi; pin < 5% → bắt buộc JSON `dispatch_mobile_charger`; pin không rõ nhưng "sắp hết" → coi như < 5%; `temperature=0` | Prompt chặn được đúng 2 kiểu tấn công trong test (dụ gửi thẳng bỏ tag, ép chỉ đường trạm 8 km khi pin 2%). **Kết quả chạy thực tế:** *(chưa chạy được trong phiên này vì thiếu file `.env` — cần điền Pass/Fail sau khi chạy `python starter-code/prompt_prototype.py`)* |

---

## 5. Nhật ký các prompt quan trọng

| # | Phase | Prompt | Output hữu ích? |
|---|---|---|---|
| 1 | Chuẩn bị | *"summary about this lab"* | **Có** — nắm được phase, rubric, file cần nộp và những gì còn thiếu. |
| 2 | Chuẩn bị | *"create for me format md to submit without answer"* | **Có** — có khung 3 file đúng cấu trúc worksheet để tự điền. |
| 3 | Phase 2 | *"improve it for me"* + bản nháp 3 Quick Cards | **Có** — sửa số liệu lệch, thêm guardrail metric, ma trận chọn thẻ. |
| 4 | Phase 3, 5 | *"evaluate this deep report for me"* | **Có** — phát hiện lỗi chi phí, lỗ hổng ca khẩn cấp, khoảng trống quyết định 5–10%. |
| 5 | Phase 3 | *"tạo ảnh workflow diagram bằng tiếng Việt"* | **Có** — ra file `04-workflow-diagram.png` dùng được ngay. |
| 6 | Nộp bài | *"viết 1 đoạn mô tả… tại sao lại dùng topic này"* | **Một phần** — đúng ý nhưng quá dài và nhiều số; phải yêu cầu viết lại. |

---

## 6. Bài học rút ra

- **Khi nào nên tin AI:** Khi cần cấu trúc hóa vấn đề, tìm lỗ hổng logic, so sánh phương án (Rule vs LLM vs Agent) hoặc trình bày lại nội dung — AI làm nhanh và phản biện tốt.
- **Khi nào cần kiểm chứng lại:** Mọi **con số** (AI tạo ra số liệu rất "thật" nhưng không có nguồn), mọi **phép tính** chi phí/ROI, **tên model và bảng giá**, và mọi báo cáo "đã xong" — luôn mở file và tự đối chiếu.
- **Điều tôi sẽ làm khác lần sau:** Ghi rõ đối tượng người đọc và độ dài mong muốn ngay trong prompt đầu tiên; yêu cầu AI đánh dấu số nào là giả định; tự viết phần trải nghiệm cá nhân (như phản biện stress-test) thay vì để AI viết hộ; chạy prototype sớm để có kết quả thật thay vì suy đoán.
