# 01 — Problem Scan & Quick Problem Cards (Vin Smart Future)

> Phase 1 (SCAN) + Phase 2 (QUICK-ASSESS) — bài làm cá nhân.
>
> **Quy ước số liệu:** số không ghi chú là số liệu công khai (đa phần do chính doanh nghiệp công bố, xem mục Nguồn cuối file). Số ghi *(ước tính)* là giả định của tôi, tính theo công thức `quy mô có nguồn × tỷ lệ giả định × thời gian/lượt`, cần xác thực bằng dữ liệu nội bộ trước khi dùng cho quyết định.

---

## Phase 1 — SCAN

### Bối cảnh quy mô

| Công ty | Số liệu chính |
|---|---|
| Xanh SM (GSM) | ~1 triệu chuyến/ngày (8/2025); doanh thu 2025 ~17.400 tỷ đồng, ~51,5% thị phần taxi công nghệ |
| VinFast | 175.099 xe bàn giao tại Việt Nam năm 2025; 400 xưởng dịch vụ cuối 2025; triệu hồi ~16.000 VF 8, ~45 phút/xe |
| V-Green (hạ tầng sạc) | >155.000 cổng sạc toàn quốc; phí đỗ quá giờ 1.000đ/phút sau 10 phút miễn phí |
| VinWonders | 9,11 triệu lượt khách năm 2025 (+17%) |

### List bài toán của tôi

| # | Subsidiary | Lens | Mô tả ngắn bài toán | Bằng chứng / tác động |
|---|---|---|---|---|
| 1 | **V-Green / VinFast** | Pain từ người khác | Trụ sạc hỏng hoặc báo lỗi kéo dài nhiều ngày; báo lỗi đến từ nhiều kênh (hotline, app, hội nhóm) nên ghi nhận và chuyển đội kỹ thuật chậm. | Chủ xe phản ánh trụ 30kW hay báo lỗi, có trạm lỗi cả tuần, cổng còn trống thì hỏng. Nếu 2% trong 155.000 cổng đang lỗi → ~3.100 cổng không phục vụ được *(ước tính)*. |
| 2 | **VinFast** | Lặp lại | Tổng đài gọi điện/nhắn tin hẹn lịch cho từng chủ xe trong các đợt triệu hồi và bảo dưỡng định kỳ, xử lý đổi lịch thủ công. | Đợt VF 8: 16.000 xe × 2 cuộc gọi × 6 phút ≈ **3.200 giờ tổng đài** *(ước tính)*, chưa tính 12.000 giờ xưởng. |
| 3 | **VinFast** | AI có thể tốt hơn | Chủ xe báo lỗi qua hotline thường được yêu cầu đưa xe vào xưởng ngay, kể cả lỗi có thể tự khắc phục, làm xưởng quá tải. | Phản ánh "nơi nào có Xanh SM thì xưởng VinFast quá tải". Nếu 20% lượt vào xưởng là lỗi tự xử lý được *(ước tính)* → giải phóng đáng kể công suất 400 xưởng. |
| 4 | **Xanh SM** | Tốn thời gian | CSKH xử lý yêu cầu hoàn tiền khi chuyến bị huỷ (tài xế không đến, app tự huỷ): phải tra log chuyến, đối chiếu chính sách, rồi soạn phản hồi. | Khách phản ánh chờ 10 phút tài xế không di chuyển, hoàn tiền chậm, vài ngày sau mới được phản hồi. 1 triệu chuyến × 0,1% × 10 phút ≈ **167 giờ/ngày ≈ 21 nhân sự** *(ước tính)*. |
| 5 | **VinWonders** | AI có thể tốt hơn | Hotline/fanpage quá tải dịp lễ với câu hỏi lặp lại về vé, khung giờ, chính sách không hoàn/huỷ vé online. | Trang chính thức thừa nhận xếp hàng lâu dịp lễ, cuối tuần; vé online không hoàn/huỷ/đổi. 9,11 triệu khách × 2% × 6 phút ≈ **18.000 giờ/năm** *(ước tính)*. |

> Hệ số 6 phút/cuộc lấy theo benchmark AHT ngành tổng đài ~6 phút 10 giây.

## Phase 2 — QUICK-ASSESS

### Card #1 — V-Green: Ghi nhận và điều phối sửa trụ sạc lỗi

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Báo lỗi trụ sạc đến từ nhiều kênh, dạng văn bản   │
│ tự do, nên ghi nhận - phân loại - chuyển đội kỹ thuật chậm, │
│ trụ hỏng nằm chờ nhiều ngày.                                │
│ Công ty thành viên: [x] VinFast / V-Green                   │
│                                                             │
│ Ai đang đau? Tài xế Xanh SM & chủ xe (chờ sạc, mất cuốc);   │
│ tổng đài viên V-Green (nhập tay); đội kỹ thuật khu vực.     │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Người dùng phát hiện trụ lỗi, gọi hotline / nhắn app   │
│   ──> 2. Tổng đài hỏi lại vị trí, tra mã trụ thủ công       │
│   ──> 3. Đọc mô tả, đoán loại lỗi, lọc báo trùng            │
│   ──> 4. Tạo ticket, chuyển đội kỹ thuật khu vực            │
│   ──> 5. Kỹ thuật viên đến kiểm tra, sửa                    │
│                                                             │
│ Bước tốn nhất? Bước 2-3 (~12 phút/báo lỗi, ước tính)        │
│ AI hỗ trợ ở bước nào? Bước 2-3: trích mã trụ, vị trí, mã lỗi│
│ từ văn bản/ảnh; gộp báo trùng; đề xuất mức ưu tiên.         │
│                                                             │
│ Metric:                                                     │
│ - Thời gian từ báo lỗi đến ticket đúng đội: ~15 phút ──> <2 │
│ - Thời gian trụ hỏng (downtime): "nhiều ngày" ──> < 24 giờ  │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │
│ (LLM trích xuất + Rule định tuyến theo khu vực)             │
└─────────────────────────────────────────────────────────────┘
```

### Card #2 — Xanh SM: Xử lý yêu cầu hoàn tiền chuyến bị huỷ

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: CSKH mất nhiều thời gian tra log chuyến và đối    │
│ chiếu chính sách để quyết định hoàn tiền, khách chờ vài ngày│
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau? Khách hàng (mất tiền, chờ lâu); nhân viên CSKH │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Khách gửi yêu cầu qua app / hotline                    │
│   ──> 2. CSKH tra log chuyến: GPS tài xế, thời điểm huỷ,    │
│          trạng thái thanh toán                              │
│   ──> 3. Đối chiếu chính sách: lỗi do tài xế, khách hay hệ  │
│          thống                                              │
│   ──> 4. Trình duyệt hoàn tiền, soạn phản hồi cho khách     │
│                                                             │
│ Bước tốn nhất? Bước 2-3 (~8/10 phút mỗi yêu cầu, ước tính)  │
│ AI hỗ trợ ở bước nào? Tóm tắt log chuyến, đề xuất kết luận  │
│ theo chính sách kèm lý do, soạn sẵn phản hồi [DRAFT_ONLY].  │
│                                                             │
│ Metric:                                                     │
│ - Thời gian xử lý/yêu cầu: ~10 phút ──> < 3 phút            │
│ - Thời gian khách nhận phản hồi: vài ngày ──> < 4 giờ       │
│ - Tỷ lệ đề xuất AI được CSKH giữ nguyên: ≥ 90%              │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │
│ (AI KHÔNG tự hoàn tiền — CSKH duyệt từng yêu cầu)           │
└─────────────────────────────────────────────────────────────┘
```

### Card #3 — VinWonders: Trợ lý CSKH vé và chính sách mùa cao điểm

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Hotline/fanpage quá tải dịp lễ với câu hỏi lặp lại│
│ về vé, giờ mở cửa, chính sách không hoàn/huỷ vé online.     │
│ Công ty thành viên: [x] Khác: Vinpearl / VinWonders         │
│                                                             │
│ Ai đang đau? Du khách (chờ máy, không được giải đáp);       │
│ tổng đài viên (quá tải mùa cao điểm).                       │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Khách gọi 1900 6677 hoặc nhắn fanpage                  │
│   ──> 2. Chờ tới lượt (cao điểm lâu)                        │
│   ──> 3. Tổng đài tra điều khoản / tình trạng đơn vé        │
│   ──> 4. Trả lời; ca khó thì chuyển cấp trên                │
│                                                             │
│ Bước tốn nhất? Bước 2-3 (~6 phút/cuộc + thời gian chờ)      │
│ AI hỗ trợ ở bước nào? Chatbot RAG trên điều khoản, bảng giá,│
│ giờ mở cửa; tra trạng thái đơn vé; chuyển người khi cần.    │
│                                                             │
│ Metric:                                                     │
│ - ≥ 60% câu hỏi được chatbot giải đáp đúng, không cần người │
│ - Thời gian phản hồi đầu tiên: vài phút ──> < 30 giây       │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │
│ (Không được hứa hoàn tiền trái điều khoản; khách bức xúc    │
│  hoặc câu hỏi ngoài phạm vi ──> chuyển tổng đài viên)       │
└─────────────────────────────────────────────────────────────┘
```

---

## Nguồn

**Xanh SM (GSM)**
- [Dân Việt — Xanh SM cán mốc 1 triệu chuyến/ngày](https://danviet.vn/xanh-sm-can-moc-1-trieu-chuyen-ngay-huong-toi-clb-100-trieu-giao-dich-thang-d1353440.html)
- [Sức khỏe & Đời sống — GSM đạt doanh thu 17.400 tỷ đồng năm 2025](https://phapluat.suckhoedoisong.vn/gsm-dat-doanh-thu-17400-ty-dong-nam-2025-chiem-hon-51-thi-phan-taxi-cong-nghe-281523.html)
- [Otofun — Huỷ chuyến nhưng Xanh SM không hoàn tiền](https://www.otofun.net.vn/threads/huy-chuyen-nhung-xanh-sm-khong-hoan-tien-cccm-luu-y.1954758/)

**VinFast / V-Green**
- [VinFast IR — 175.099 xe điện bàn giao tại Việt Nam năm 2025](https://vinfastauto.us/investor-relations/news/vinfast-sets-record-with-175099-electric-vehicles-delivered-in-vietnam-in)
- [TNGlobal — VinFast delivers 175,099 EVs in Vietnam in 2025](https://technode.global/2026/01/16/vinfast-delivers-175099-evs-in-vietnam-in-2025/)
- [Znews — VinFast triệu hồi gần 16.000 xe VF 8](https://znews.vn/vinfast-trieu-hoi-gan-16000-xe-vf8-nham-nang-cap-tui-khi-post1523326.html)
- [VnEconomy — V-Green đặt mục tiêu 500.000 cổng sạc](https://vneconomy.vn/v-green-dat-muc-tieu-xay-dung-500-000-cong-sac-oto-dien.htm)
- [VnBusiness — VinFast rút ngắn thời gian miễn phí đỗ sau sạc](https://vnbusiness.vn/vinfast-rut-ngan-thoi-gian-mien-phi-do-sau-sac.html)
- [Diễn đàn VinFast — Phản ánh về vấn đề trạm sạc](https://vinfast.vn/dien-dan/thao-luan/phan-anh-ve-van-de-tram-sac-cua-vinfast/)
- [Diễn đàn VinFast — Sạc xe giờ bất tiện quá](https://vinfast.vn/dien-dan/thao-luan/sac-xe-gio-bat-tien-qua/)
- [Diễn đàn VinFast — Xưởng dịch vụ quá tải](https://vinfast.vn/dien-dan/thao-luan/tinh-hinh-he-thong-xuong-dich-vu-cua-vinfast-co-ve-qua-tai-roi-mong-vin-mo-them-xuong-dich-vu/)

**Vinpearl / VinWonders**
- [Mekong ASEAN — Vinpearl hoàn thành 111% kế hoạch doanh thu 2025](https://mekongasean.vn/vinpearl-hoan-thanh-111-ke-hoach-doanh-thu-nam-2025-51486.html)
- [VinWonders — Hướng dẫn đặt vé online từ A–Z](https://vinwonders.com/vi/wonderpedia/news/huong-dan-cac-buoc-dat-ve-vinwonders-online-tu-a-z/)
- [VinWonders — Điều khoản và điều kiện](https://vinwonders.com/vi/terms-and-conditions/)

**Benchmark ngành**
- [Plivo — Contact center statistics & benchmarks 2025](https://www.plivo.com/blog/contact-center-statistics-benchmarks-2025/)