# Phase 1 --- SCAN

Mục tiêu: Xác định các vấn đề thực tế trong sinh hoạt có khả năng cải thiện bằng AI. Ưu tiên những vấn đề có người gặp phải thường xuyên, quy trình hiện tại có điểm nghẽn rõ ràng và có thể đo lường kết quả.

### Các vấn đề được lựa chọn

| STT | Bài toán | Người gặp vấn đề | Vấn đề cốt lõi |
|---|---|---|---|
| 1 | Dự đoán thời gian di chuyển theo tuyến đường và thời điểm xuất phát | Sinh viên, nhân viên văn phòng, người thường xuyên di chuyển | Thời gian đến thực tế thường khác với dự kiến; người dùng khó chủ động chọn thời điểm xuất phát |
| 2 | Phát hiện lỗ hổng kiến thức trong quá trình học tập | Người tự học, sinh viên, người học trực tuyến | Người học biết mình làm sai nhưng không biết đang thiếu kiến thức nền tảng hoặc kỹ năng nào |

---

## Phase 2 --- QUICK-ASSESS

### Bài toán 1 --- Dự đoán thời gian di chuyển theo tuyến đường và thời điểm xuất phát

| Field | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Dự đoán thời gian di chuyển thực tế trên một tuyến đường dựa vào thời điểm xuất phát và điều kiện giao thông, giúp người dùng chủ động sắp xếp lịch trình. |
| **Đối tượng (Actor)** | Sinh viên, nhân viên văn phòng và người thường xuyên di chuyển trên các tuyến đường cố định. |
| **Workflow hiện tại** | 1. Người dùng xác định điểm đi và điểm đến. 2. Tra cứu thời gian di chuyển trên ứng dụng bản đồ. 3. Chọn tuyến đường và thời điểm xuất phát. 4. Di chuyển và tự điều chỉnh nếu gặp ùn tắc. |
| **Điểm nghẽn (Bottleneck)** | Thời gian di chuyển thay đổi theo khung giờ, mật độ giao thông, thời tiết và sự cố trên đường. Người dùng khó ước lượng chính xác thời gian cần thiết, đặc biệt khi phải di chuyển vào giờ cao điểm. |
| **AI can thiệp** | Phân tích dữ liệu lịch sử di chuyển kết hợp với thời điểm, tuyến đường và dữ liệu giao thông để dự đoán thời gian di chuyển. Hệ thống đưa ra thời gian ước lượng kèm mức độ tin cậy thay vì chỉ hiển thị một con số cố định. |
| **Metric đánh giá** | - MAE giảm tối thiểu 15% so với baseline.<br>- Tỷ lệ đến đúng giờ tăng tối thiểu 10 điểm phần trăm.<br>- Theo dõi sai số dự đoán theo từng khung giờ và loại tuyến đường. |
| **Baseline cần có** | Thời gian di chuyển thực tế, thời điểm xuất phát, tuyến đường và dữ liệu giao thông tương ứng. |
| **Phạm vi MVP** | Thử nghiệm trên một khu vực địa lý giới hạn, một nhóm tuyến đường và một phương tiện di chuyển. Chưa xây dựng hệ thống điều hướng hoặc tự động thay đổi tuyến đường. |
| **Hướng triển khai** | Machine Learning dự đoán thời gian di chuyển từ dữ liệu lịch sử và các yếu tố đầu vào có liên quan. |

---

### Bài toán 2 --- Phát hiện lỗ hổng kiến thức trong quá trình học tập

| Field | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Phân tích quá trình làm bài của người học để xác định những kiến thức hoặc kỹ năng nền tảng mà họ chưa nắm vững, từ đó đề xuất nội dung cần ôn tập. |
| **Đối tượng (Actor)** | Sinh viên, người tự học và người học trên các nền tảng trực tuyến. |
| **Workflow hiện tại** | 1. Người học tiếp nhận kiến thức mới. 2. Làm bài tập hoặc bài kiểm tra. 3. Xem kết quả đúng/sai và điểm số. 4. Tự tìm tài liệu để ôn tập. 5. Tiếp tục học nội dung tiếp theo. |
| **Điểm nghẽn (Bottleneck)** | Kết quả đúng/sai chỉ phản ánh biểu hiện ở một câu hỏi, chưa chỉ ra nguyên nhân người học làm sai. Điểm số tổng thể cũng không thể hiện rõ từng kiến thức thành phần mà người học đang yếu. |
| **AI can thiệp** | Phân tích lịch sử trả lời, mức độ khó của câu hỏi và mối quan hệ giữa các kiến thức tiên quyết để ước lượng mức độ nắm vững của người học. Hệ thống xác định các nội dung có khả năng bị hổng và đề xuất bài tập kiểm tra hoặc tài liệu ôn tập phù hợp. |
| **Metric đánh giá** | - F1-score tối thiểu 0.80 trên tập dữ liệu được chuyên gia gán nhãn.<br>- Thời gian xác định nội dung cần ôn tập giảm tối thiểu 30%.<br>- Đo mức cải thiện kết quả học tập trước và sau khi sử dụng hệ thống. |
| **Baseline cần có** | Lịch sử làm bài, kết quả theo từng chủ đề, ngân hàng câu hỏi có nhãn kiến thức và đánh giá của giảng viên hoặc người có chuyên môn. |
| **Phạm vi MVP** | Chỉ triển khai trên một môn học hoặc một nhóm kiến thức có cấu trúc tiên quyết rõ ràng. Hệ thống đưa ra dự đoán theo từng kỹ năng, không đánh giá toàn diện năng lực người học. |
| **Hướng triển khai** | Knowledge Tracing và mô hình hóa mức độ nắm vững kiến thức theo thời gian. Có thể khảo sát các phương pháp như BKT, IRT hoặc DKT tùy theo dữ liệu. |

---

## Phase 3 --- QUICK COMPARISON

| Tiêu chí | Bài toán 1: Dự đoán thời gian di chuyển | Bài toán 2: Phát hiện lỗ hổng kiến thức |
|---|---|---|
| **Mức độ thường xuyên** | Cao | Cao |
| **Mức độ rõ ràng của vấn đề** | Khá rõ | Rõ |
| **Khả năng thu thập dữ liệu** | Phụ thuộc dữ liệu giao thông và lịch sử di chuyển | Phụ thuộc ngân hàng câu hỏi và lịch sử làm bài |
| **Khả năng xây dựng MVP** | Trung bình | Khá cao nếu giới hạn một môn học |
| **Khả năng đánh giá định lượng** | Cao | Cao nếu có nhãn đánh giá đáng tin cậy |
| **Hướng AI phù hợp** | Machine Learning / Time Series | Knowledge Tracing / Machine Learning |

---

## Phase 4 --- INITIAL RECOMMENDATION

### Bài toán được ưu tiên: Phát hiện lỗ hổng kiến thức trong quá trình học tập

**Lý do lựa chọn:**
- Có thể giới hạn phạm vi thử nghiệm trong một môn học hoặc một nhóm kiến thức cụ thể.
- Có thể xây dựng tập dữ liệu từ ngân hàng câu hỏi, lịch sử làm bài và nhãn đánh giá của người có chuyên môn.
- Có các hướng nghiên cứu và phương pháp định lượng để so sánh, chẳng hạn BKT, IRT và DKT.
- Kết quả đầu ra có thể được đánh giá bằng cách đối chiếu dự đoán của hệ thống với đánh giá chuyên gia và mức tiến bộ thực tế của người học.

**Điều kiện cần xác minh trước khi triển khai:**
- Có đủ câu hỏi được gắn nhãn theo từng kỹ năng hoặc kiến thức thành phần.
- Có thể xác định mối quan hệ tiên quyết giữa các kiến thức.
- Có dữ liệu lịch sử làm bài đủ để mô hình hóa quá trình học, thay vì chỉ dựa trên một lần kiểm tra.
- Có người có chuyên môn tham gia đánh giá độ chính xác của kết quả chẩn đoán.

### Bài toán dự phòng: Dự đoán thời gian di chuyển

Bài toán này vẫn có tiềm năng, nhưng cần xác minh khả năng tiếp cận dữ liệu thời gian di chuyển thực tế và dữ liệu giao thông. Nếu không có dữ liệu đủ tin cậy, việc đánh giá mô hình sẽ khó thuyết phục.

---

## Kết luận

Hai bài toán đều có thể phát triển thành sản phẩm AI ở quy mô nhỏ. Tuy nhiên, cần xác minh dữ liệu và hành vi thực tế của người dùng trước khi quyết định xây dựng mô hình. Việc lựa chọn AI chỉ có ý nghĩa khi mô hình giải quyết được điểm nghẽn cụ thể và tạo ra kết quả tốt hơn phương pháp hiện tại.