# 03 — Nguyên Tắc Viết PRD

> Biết cấu trúc (section nào) chưa đủ — còn phải biết viết đúng (viết cái gì, cụ thể đến mức nào).

---

## Nguyên Tắc #1: WHAT, Không Phải HOW

### Phân biệt 3 tầng

```
[Problem]  →  [Product Solution]  →  [Technical Solution]
                     ↑                        ↑
               PRD dừng ở đây          Phase 3 Design mới cover
```

PRD mô tả **WHAT** (cần làm gì từ góc nhìn user) và **WHY** (tại sao cần làm).  
PRD **không** mô tả **HOW** (implementation, technology, architecture).

### Bảng đối chiếu

| Viết trong PRD ✅ | Không viết trong PRD ❌ |
|------------------|----------------------|
| "Tìm kiếm trả về kết quả trong 1 giây" | "Dùng Elasticsearch với inverted index" |
| "User có thể upload file Excel tối đa 10MB" | "Parse bằng Apache POI library" |
| "Mật khẩu phải ≥ 8 ký tự, có chữ hoa + số" | "Hash bằng bcrypt với salt rounds = 12" |
| "Checkout không quá 3 bước" | "Dùng React stepper component" |
| "Hệ thống gửi email xác nhận trong 30 giây" | "Dùng SendGrid qua SMTP" |
| "Dữ liệu được lưu và có thể truy xuất lại" | "Lưu trong PostgreSQL table orders" |

### Câu hỏi tự kiểm tra khi viết

> "Câu này mô tả thứ **user cần**, hay mô tả **cách dev sẽ build nó**?"

Nếu là cách dev build → xóa hoặc chuyển vào Technical Design Document (Phase 3).

---

### Ngoại lệ: Khi PM được phép đề cập kỹ thuật

Được phép khi đó là **constraint** hoặc **assumption** (không phải requirement):

```markdown
# Constraints
- Phải tích hợp với SAP ERP hiện tại qua REST API (không được thay SAP)
- Phải chạy trên AWS (công ty đã có Enterprise Agreement)

# Assumptions  
- Frontend sẽ dùng React (đã được team Tech thống nhất trước)
```

Đặt những thứ này vào Section 10 (Constraints & Assumptions), không phải Functional Requirements.

---

## Nguyên Tắc #2: Đủ Cụ Thể Để Dev Build Được, Không Phải Hơn

### Chuẩn đo: "Đọc xong, dev biết làm gì tiếp theo không?"

**Quá mơ hồ — dev phải tự đoán:**
> "Hệ thống cần có tính năng tìm kiếm."

Dev sẽ hỏi: tìm theo field nào? Kết quả hiển thị gì? Real-time hay batch? Phân trang không?

**Đúng chuẩn — behavior rõ ràng từ góc nhìn user:**
> "Staff có thể tìm kiếm product bằng tên hoặc SKU. Kết quả hiển thị trong ≤ 1 giây, bao gồm: tên sản phẩm, SKU, số lượng tồn kho, vị trí kho. Nếu không có kết quả, hiển thị message 'Không tìm thấy, kiểm tra lại chính tả'. Maximum 50 kết quả trả về mỗi lần tìm."

**Quá kỹ thuật — PM đang làm việc của Tech Lead:**
> "Dùng Elasticsearch. API: GET /api/v1/inventory/search?q={query}&limit=50. Cache Redis TTL 60s. Index fields: name, sku."

---

## Nguyên Tắc #3: Viết Cho Reader Ít Kinh Nghiệm Nhất

Giả định người đọc PRD là:
- Dev junior vừa join team
- QA không biết context background
- Stakeholder không phải technical người

Nếu cần background knowledge để hiểu → định nghĩa trong Glossary hoặc giải thích inline.

**Ví dụ tệ:**
> "Coupon được áp theo stacking rule, ưu tiên theo tier."

**Ví dụ tốt:**
> "Coupon được áp theo nguyên tắc stacking (xem BR-001): user có thể dùng tối đa 1 coupon platform + 1 coupon shop + 1 coupon freeship cùng lúc."

---

## Nguyên Tắc #4: Mỗi Requirement Là Một Statement Rõ Ràng

### Tránh các từ mơ hồ

| Từ mơ hồ | Thay bằng |
|----------|-----------|
| "nhanh" | "trong < 2 giây P95" |
| "dễ dùng" | "user hoàn thành task trong < 3 clicks" |
| "thường xuyên" | "ít nhất 1 lần/ngày" |
| "nhiều" | "> 100 records" |
| "phù hợp" | "đáp ứng WCAG 2.1 AA" |
| "tốt" | định nghĩa metric cụ thể |

### Tránh viết requirement kép trong một câu

**Sai (2 requirements trong 1):**
> "User có thể filter theo ngày và export kết quả ra Excel."

**Đúng (tách ra):**
> "User có thể filter danh sách đơn hàng theo khoảng ngày (from–to)."
> "User có thể export danh sách đơn hàng đang hiển thị ra file Excel (.xlsx)."

---

## Nguyên Tắc #5: Luôn Viết Rõ Subject Của Hành Động

**Sai — không rõ ai làm:**
> "Email xác nhận được gửi sau khi order thành công."

**Đúng — rõ ràng ai là subject:**
> "Hệ thống tự động gửi email xác nhận đến địa chỉ email của buyer trong vòng 30 giây sau khi order được tạo thành công."

---

## Nguyên Tắc #6: Negative Scope Cũng Là Requirement

Đừng chỉ viết những gì hệ thống PHẢI làm. Viết cả những gì hệ thống KHÔNG làm (khi điều đó dễ bị hiểu nhầm).

**Ví dụ:**
> "Tính năng auto-apply coupon chỉ hoạt động cho logged-in user. Guest checkout KHÔNG có tính năng này (scope v2)."

> "Hệ thống hiển thị thông tin warranty nhưng KHÔNG lưu log lần xem của user (privacy)."

---

## Nguyên Tắc #7: Dùng Active Voice

**Passive (tránh):**
> "Notification sẽ được gửi khi order được xác nhận."

**Active (dùng):**
> "Hệ thống gửi push notification cho user khi order được confirm."

---

## Nguyên Tắc #8: Consistent Terminology

Định nghĩa thuật ngữ một lần, dùng nhất quán xuyên suốt tài liệu.

**Ví dụ tệ:**
> "User tạo đơn hàng... Khách hàng đặt order... Buyer submit purchase..."

Ba câu trên nói về cùng một hành động nhưng dùng 3 cách khác nhau. Dev và QA sẽ hỏi: đây có phải cùng một flow không?

**Quyết định một từ, dùng mãi:**

| Thuật ngữ dùng | KHÔNG dùng xen lẫn |
|----------------|-------------------|
| "buyer" | user / customer / khách hàng |
| "order" | đơn hàng / purchase / transaction |
| "coupon" | voucher / promo code / discount code |

Đặt glossary vào Appendix hoặc file `glossary.md` riêng nếu có nhiều thuật ngữ.

---

## Quick Self-Check: 5 Câu Hỏi Trước Khi Submit

1. **Dev có thể build không?** — Đọc lại mỗi requirement, tự hỏi: có đủ thông tin để implement không?
2. **QA có thể test không?** — Mỗi AC có điều kiện pass/fail rõ ràng không?
3. **Có chỗ nào mơ hồ không?** — Tìm từ "nhanh", "dễ", "phù hợp", "thường xuyên" → thay bằng số.
4. **Có HOW nào lẫn vào không?** — Review toàn bộ, xóa implementation detail.
5. **Edge case nào bị bỏ sót?** — Hỏi: "Nếu user làm X nhưng Y thì sao?"

---

*Tiếp theo: [04-flows-and-rules.md](./04-flows-and-rules.md) — Cách viết User Flow và Business Rules.*
