# Hướng Dẫn Viết PRD — Dành Cho Người Mới

> **Mục tiêu:** Sau khi đọc xong folder này, bạn có thể viết một PRD đủ tốt để dev build được, QA test được, và stakeholder approve được — dù bạn chưa từng viết PRD trước đây.

---

## Folder Này Chứa Gì

```
how-to-write-prd/
  README.md                  ← File này — đọc trước
  01-before-you-write.md     ← Cần làm gì TRƯỚC khi mở file mới
  02-prd-structure.md        ← 10 sections chuẩn của một PRD
  03-writing-rules.md        ← Nguyên tắc viết: WHAT vs HOW, độ cụ thể
  04-flows-and-rules.md      ← Cách viết User Flow và Business Rules
  05-acceptance-criteria.md  ← Viết AC đúng cách, đúng thời điểm
  06-scale-adjustment.md     ← PRD cho hệ thống lớn vs app nhỏ
  07-document-organization.md← Tổ chức folder tài liệu, share với khách hàng
  08-checklist.md            ← Checklist review trước khi submit
  example/
    auto-coupon-prd.md       ← PRD mẫu hoàn chỉnh để đọc tham khảo
```

---

## Thứ Tự Đọc Đề Xuất

### Nếu bạn là người mới hoàn toàn
Đọc theo thứ tự: `01 → 02 → 03 → 04 → 05 → example → 08`

### Nếu bạn đã biết cơ bản, cần nâng cao
Đọc: `03 → 04 → 05 → 06 → 07 → 08`

### Nếu bạn chỉ cần template nhanh
Mở `02-prd-structure.md` để lấy template, rồi dùng `08-checklist.md` để review.

---

## PRD Là Gì? (Một Câu)

> **PRD (Product Requirements Document)** là tài liệu mô tả **CẦN LÀM GÌ** và **TẠI SAO** — không phải **LÀM NHƯ THẾ NÀO**.

PRD là tiếng nói chung giữa Business và Engineering. Nó đủ cụ thể để dev hiểu cần build gì, đủ rõ để QA biết cần test gì, nhưng không dictate solution kỹ thuật.

---

## Một PRD Tốt Trông Như Thế Nào?

Khi hoàn thành, PRD của bạn nên trả lời được 5 câu hỏi này:

1. **Vấn đề gì đang cần giải quyết?** — Và có data/evidence nào chứng minh đây là vấn đề thật?
2. **Ai là người dùng cần giải quyết?** — Đủ cụ thể để không phải viết cho "tất cả mọi người".
3. **Thành công trông như thế nào?** — Metrics đo được, không phải cảm tính.
4. **Hệ thống cần làm gì?** — User stories, flows, business rules, edge cases.
5. **Những gì KHÔNG nằm trong scope?** — Tránh scope creep sau này.

---

## Sai Lầm Phổ Biến Nhất Của Người Mới

| Sai lầm | Hệ quả | Cách tránh |
|---------|--------|-----------|
| Viết HOW thay vì WHAT | Dev bị limited, không dùng được expertise của họ | Đọc `03-writing-rules.md` |
| Bỏ qua Business Rules | Dev tự đoán logic, sinh bugs | Đọc `04-flows-and-rules.md` |
| AC quá mơ hồ | QA không biết pass hay fail | Đọc `05-acceptance-criteria.md` |
| Không có data chứng minh vấn đề | Stakeholder không approve | Đọc `01-before-you-write.md` |
| Viết quá dài, không ai đọc | PRD thành "tài liệu trang trí" | Đọc `06-scale-adjustment.md` |
| Open questions bị chôn vùi | Blockers xuất hiện giữa sprint | Luôn có section Open Questions |

---

## Quy Tắc Vàng

> **Một BA/PM giỏi không phải là người viết nhiều nhất — mà là người làm cho dev, QA, và stakeholder cần đọc ít nhất để hiểu nhiều nhất.**

---

*Guide được tổng hợp từ thực tế dự án. Cập nhật lần cuối: 2026-05-24.*
