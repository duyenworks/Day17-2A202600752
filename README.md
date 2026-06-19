# Lab Assignment Day 17 — Kiểm chứng giả thuyết sản phẩm

**Dự án:** AI Hỗ Trợ Đọc Ảnh Y Khoa & Đánh Dấu Vùng Nghi Ngờ  
**Prototype tương tác:** [hypothesis-test.html](./hypothesis-test.html) (mở bằng trình duyệt)

---

## Bước 1 — Chọn 1 giả thuyết quan trọng

Chúng tôi chọn giả thuyết cốt lõi **A4** từ phân tích JTBD:

> *"Lớp phủ YOLO trên ảnh X-quang ngực đủ chính xác và đáng tin — bác sĩ CĐHA sẽ dùng nó để xác định vùng bất thường nhanh hơn thay vì tắt hoặc bỏ qua hệ thống."*

**Tại sao quan trọng:** Nếu giả thuyết này sai, toàn bộ giá trị sản phẩm ở giai đoạn 1 (YOLO) sụp đổ — dù Med Gemma diễn giải hay không cũng không cứu được.

---

## Bước 2 — Cách team đã / đang định kiểm chứng (phương pháp đắt)

| Thuộc tính | Chi tiết |
|---|---|
| **Chi phí** | Cao · 4–8 tuần |
| **Pilot** | Tích hợp pipeline đầy đủ vào workstation bệnh viện thật (2 tuần) |
| **Dữ liệu** | Thu thập 100+ ca có chẩn đoán chuẩn vàng (ground truth) từ bác sĩ senior |
| **Đo lường** | ROC / AUC, độ nhạy, độ đặc hiệu trên ảnh DICOM thật của BV |
| **Hạ tầng** | UI workstation hoàn chỉnh + kết nối PACS + chạy Med Gemma giai đoạn 2 |
| **Nguồn lực** | 3–5 kỹ sư, phần cứng GPU, phê duyệt IRB/đạo đức y khoa |

---

## Bước 3 — Ba cách rẻ hơn để test cùng giả thuyết

### Cách 1 — So sánh ảnh tĩnh (A/B) · ~2 ngày

Chạy YOLO offline trên 20 ảnh X-quang. In hoặc hiển thị song song: ảnh gốc vs ảnh có khung đánh dấu. Đo thời gian bác sĩ tìm vùng nghi ngờ và hỏi mức độ tin cậy (1–5).

**Cùng test:** YOLO có giúp phát hiện nhanh hơn không? Bác sĩ có tin lớp phủ không?

### Cách 2 — Phỏng vấn + mô hình giấy/HTML · ~3 ngày

Mời 3–5 bác sĩ xem prototype click được (không cần AI thật). Hỏi có dùng trong ca trực không, điều gì khiến họ tắt hệ thống.

**Cùng test:** Workflow có phù hợp thói quen đọc phim không? UI có đủ tin cậy để không bị bỏ qua?

### Cách 3 — Wizard of Oz (người giả lập AI) · ~1 tuần

Một người trong team đánh dấu vùng nghi ngờ thủ công (hoặc dùng YOLO chạy trước), bác sĩ nghĩ đó là AI realtime. Quan sát hành vi thật trong 10–15 ca.

**Cùng test:** Bác sĩ có thực sự dựa vào lớp phủ khi nghĩ đó là AI không? Họ sửa/xóa đánh dấu bao nhiêu lần?

---

## Bước 4 — Prototype nhanh 3 cách

Ba prototype tương tác nằm trong file [hypothesis-test.html](./hypothesis-test.html):

| Prototype | Mô tả | Tính năng demo |
|---|---|---|
| **1 — So sánh ảnh tĩnh** | Bật/tắt lớp phủ YOLO trên ảnh X-quang mô phỏng | Nút bật/tắt khung đỏ, đồng hồ đo thời gian tìm vùng nghi ngờ |
| **2 — Khảo sát sau xem mô hình** | Form phỏng vấn bác sĩ sau khi xem UI | Thang điểm tin cậy (1–5), ý định dùng (1–5), lý do tắt hệ thống |
| **3 — Wizard of Oz** | Bác sĩ click vùng nghi ngờ, không biết AI hay người đánh dấu | Đếm số click, so sánh với vùng AI đánh dấu khi tiết lộ |

**Cách chạy:** Mở `hypothesis-test.html` trong trình duyệt (double-click hoặc `open hypothesis-test.html`).

---

## Bước 5 — Present nhóm

### Nội dung trình bày đề xuất

1. **Giả thuyết A4** — lớp phủ YOLO phải đủ tin cậy để bác sĩ dùng, không tắt
2. **Cách test đắt** — pilot 100+ ca, ROC/AUC, tích hợp PACS (4–8 tuần)
3. **3 cách rẻ** — so sánh ảnh tĩnh, phỏng vấn + mockup, Wizard of Oz (1–2 tuần tổng)
4. **Demo live** — chạy 3 prototype trong `hypothesis-test.html`
5. **Tiêu chí quyết định** — xem bảng so sánh bên dưới

---

## Tóm tắt so sánh phương pháp

| Phương pháp | Thời gian | Chi phí | Đo được gì |
|---|---|---|---|
| Pilot đầy đủ | 4–8 tuần | Rất cao | ROC, AUC, hành vi thật |
| So sánh ảnh tĩnh | 2 ngày | Thấp | Thời gian, mức tin cậy |
| Phỏng vấn + mockup | 3 ngày | Thấp | Ý định dùng, rào cản |
| Wizard of Oz | 1 tuần | Trung bình thấp | Hành vi thật trước khi build |

### Kế hoạch tiếp theo

Chạy 3 cách rẻ trước trong **1–2 tuần**. Nếu:

- Bác sĩ tin lớp phủ **≥ 4/5**, và
- Thời gian tìm vùng nghi ngờ giảm **≥ 20%**

→ Mới đầu tư pilot 100+ ca với phương pháp đắt.
