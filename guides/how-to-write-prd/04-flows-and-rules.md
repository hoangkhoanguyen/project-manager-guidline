# 04 — User Flows và Business Rules

> Hai phần này là nơi PM/BA non-experience hay bỏ sót nhất — và cũng là nơi gây ra nhiều bugs nhất.

---

## Phần A: User Flows

### Tại Sao Cần Viết Flow?

User story ("As a user, I want to...") mô tả **intent** của user. Flow mô tả **journey** — từng bước user đi qua, và hệ thống phản hồi gì tại mỗi bước.

Thiếu flow, dev sẽ tự suy luận journey và mỗi người suy luận khác nhau.

---

### 4 Loại Flow Cần Viết

**1. Happy Path** — Tất cả mọi thứ đúng, không có exception.
- Đây là flow chính, cần viết đầu tiên.
- Khoảng 60–70% user sẽ đi qua flow này.

**2. Alternate Flow** — User chủ động chọn con đường khác.
- Ví dụ: Thay vì dùng coupon auto-applied, user muốn chọn coupon khác.
- Ví dụ: User nhập coupon code thủ công thay vì dùng từ danh sách.

**3. Error Flow** — Có lỗi xảy ra (do user hoặc do hệ thống).
- Ví dụ: Coupon hết hạn ngay khi user đang checkout.
- Ví dụ: Payment gateway timeout.
- Mỗi error phải có: thông báo gì, user có thể làm gì tiếp theo.

**4. Edge Case** — Tình huống đặc biệt, hiếm xảy ra nhưng cần xử lý.
- Ví dụ: User thêm sản phẩm vào cart khi đang ở trang checkout (cart thay đổi mid-checkout).
- Ví dụ: Session timeout khi user đang nhập thông tin.

---

### Format Viết Flow (Text-Based)

Không nhất thiết phải có Figma hay diagram công cụ. Text đủ rõ là ổn.

```markdown
### Flow [Số]: [Tên Flow]

**Trigger:** [Điều gì khiến flow này bắt đầu]
**Precondition:** [Điều kiện phải đúng trước khi flow chạy]

**Steps:**
1. [Actor] [hành động]
2. [System] [phản hồi]
3. [Actor] [hành động tiếp theo]
4. [System] [phản hồi]
...
N. [End state — flow kết thúc như thế nào]

**Postcondition:** [Trạng thái sau khi flow hoàn thành]
**Branching:** Tại bước [X], nếu [điều kiện] → [Flow Y]
```

---

### Ví Dụ Thực Tế: Auto-Apply Coupon

```markdown
### Flow 1: Auto-apply coupon (Happy Path)

Trigger: User nhấn "Tiến hành thanh toán" từ trang Cart
Precondition: User đã login, có ít nhất 1 coupon hợp lệ trong tài khoản

Steps:
1. User nhấn "Tiến hành thanh toán"
2. Hệ thống fetch danh sách coupon hợp lệ của user
3. Hệ thống tính toán combination coupon cho tổng tiết kiệm cao nhất (xem BR-002)
4. Trang Checkout hiển thị với section "Ưu đãi đã áp dụng":
   - Tên coupon và mô tả
   - Số tiền tiết kiệm
   - Link "Thay đổi" để user chọn coupon khác
5. User review đơn hàng và tiến hành thanh toán bình thường
6. Coupon được ghi nhận sử dụng khi order confirmed

Postcondition: Order được tạo với coupon đã áp dụng, số lần dùng coupon tăng 1

---

### Flow 2: Không có coupon hợp lệ

Trigger: User nhấn "Tiến hành thanh toán"
Precondition: User đã login, KHÔNG có coupon hợp lệ

Steps:
1. User nhấn "Tiến hành thanh toán"
2. Hệ thống fetch danh sách coupon → không tìm thấy coupon hợp lệ
3. Trang Checkout KHÔNG hiển thị section coupon
4. [Optional UX] Hiển thị text nhỏ "Bạn chưa có ưu đãi nào" kèm link đến trang Voucher
5. User tiến hành checkout bình thường không có coupon

---

### Flow 3: Coupon bị invalidate mid-checkout

Trigger: Coupon hết hạn hoặc bị thu hồi trong khi user đang ở trang Checkout
Precondition: User đã vào Checkout, coupon đã được hiển thị

Steps:
1. [Background] Coupon X hết hạn trong lúc user đang đọc trang checkout
2. User nhấn "Đặt hàng"
3. Hệ thống validate coupon trước khi tạo order → phát hiện coupon X đã invalid
4. Hệ thống KHÔNG tạo order
5. Trang Checkout hiển thị thông báo: "Ưu đãi [tên coupon] đã hết hạn"
6. Hệ thống tự động kiểm tra và áp coupon khác tốt nhất nếu có → quay lại Flow 1
   HOẶC nếu không còn coupon hợp lệ → Flow 2

---

### Flow 4: Cart thay đổi mid-checkout

Trigger: User thêm/xóa sản phẩm trong khi đang ở trang Checkout
Precondition: User đang ở Checkout, đã có coupon auto-applied

Steps:
1. User quay lại Cart và thêm 1 sản phẩm
2. User quay lại trang Checkout
3. Hệ thống recalculate coupon vì cart đã thay đổi
4. Nếu coupon vẫn hợp lệ cho cart mới → hiển thị lại với số tiền tiết kiệm updated
5. Nếu coupon không còn hợp lệ cho cart mới (ví dụ: điều kiện min order không còn đạt)
   → Hiển thị thông báo: "Ưu đãi [tên] không còn áp dụng được vì đơn hàng thay đổi"
   → Tự động áp coupon tốt nhất còn hợp lệ (nếu có)
```

---

### Checklist Flow Đã Đủ Chưa?

- [ ] Happy path được viết rõ ràng từ trigger đến end state
- [ ] Mỗi branching point (IF/ELSE) đều có xử lý cho cả hai nhánh
- [ ] Mỗi error state đều có: message gì hiển thị + user làm được gì tiếp theo
- [ ] Không có "nếu X thì sao" nào bị bỏ lửng
- [ ] Actor luôn rõ ràng: "User" hay "System" làm gì

---

## Phần B: Business Rules

### Business Rules Là Gì?

Business rules là **logic nghiệp vụ** quyết định cách hệ thống xử lý dữ liệu và quyết định.

Khác với Functional Requirement (user làm gì → hệ thống phản hồi gì), Business Rule là **điều kiện và logic** mà hệ thống phải follow bất kể user làm gì.

**Ví dụ phân biệt:**

| Functional Requirement | Business Rule |
|----------------------|---------------|
| "User có thể áp coupon khi checkout" | "Không thể áp 2 coupon cùng platform" |
| "Hệ thống tính ngày hết hạn warranty" | "expiresAt = activatedAt + warrantyTermDays" |
| "User có thể xem điểm loyalty" | "1 điểm = 1000 VND chi tiêu, điểm hết hạn sau 12 tháng" |

---

### Tại Sao Tách Business Rules Ra Riêng?

**Nếu nhúng business rule vào user story:**
- Khi rule thay đổi, phải tìm khắp nơi để sửa
- Dev và QA dễ bỏ sót một AC trong list dài
- Conflict giữa các rule không dễ thấy

**Khi tách ra section riêng:**
- Thay đổi rule → sửa một chỗ duy nhất
- Reference bằng BR-ID từ mọi nơi: "Xem BR-001"
- Dễ review tính nhất quán giữa các rules

---

### Format Business Rules

```markdown
### BR-[ID]: [Tên Rule Ngắn Gọn]

**Mô tả:** [Giải thích rule bằng ngôn ngữ tự nhiên]
**Công thức / Điều kiện:** [Nếu có logic tính toán hoặc điều kiện if/else]
**Ví dụ:** [Ví dụ cụ thể để làm rõ]
**Ngoại lệ:** [Trường hợp rule không áp dụng, nếu có]
```

---

### Ví Dụ Business Rules: Hệ Thống Warranty

```markdown
### BR-001: Công thức tính ngày hết hạn

Mô tả: Ngày hết hạn warranty được tính từ ngày kích hoạt.
Công thức: expiresAt = activatedAt + warrantyTermDays
Ví dụ: Kích hoạt 2025-01-01, bảo hành 365 ngày → hết hạn 2026-01-01
Lưu ý: Tính theo timezone UTC+7 (giờ Việt Nam)

---

### BR-002: Tính remainingDays

Công thức: remainingDays = ceil((expiresAt - now) / 1 ngày), minimum là 0
Ví dụ: Còn 2.3 ngày → hiển thị "3 ngày"
Lý do ceil: Ưu tiên trải nghiệm user — nếu còn bất kỳ phần nào của ngày thì vẫn tính là 1 ngày.

---

### BR-003: Security khi lookup SN chưa activate

Điều kiện: User tra cứu SN chưa được activate nhưng không cung cấp secret code
Xử lý: Trả về status "not_found" (không phải "not_activated")
Lý do: Tránh tiết lộ SN có tồn tại trong hệ thống hay không (prevent enumeration attack)
Ngoại lệ: Nếu có secret code và đúng → trả về thông tin đầy đủ
```

---

### Ví Dụ Business Rules: Auto-Apply Coupon

```markdown
### BR-001: Stacking rule

User chỉ có thể áp dụng tối đa: 1 coupon platform + 1 coupon shop + 1 coupon freeship.
Không thể áp 2 coupon cùng loại cùng lúc.
Ví dụ: Coupon ShopNow -50k + Coupon Shop A -20k + Freeship → Hợp lệ
        Coupon ShopNow -50k + Coupon ShopNow -30k → Không hợp lệ

---

### BR-002: Định nghĩa "coupon tốt nhất"

Tốt nhất = combination coupon dẫn đến [tổng tiền user phải trả] thấp nhất.
Hệ thống tính bằng cách enumerate tất cả combination hợp lệ theo BR-001 và chọn combination có final_price thấp nhất.
Tie-breaking (nhiều combination cùng final_price): ưu tiên coupon có expiry sớm nhất.

---

### BR-003: Coupon hợp lệ được định nghĩa là

- Trạng thái: active (chưa bị vô hiệu hóa)
- Chưa hết hạn (expiresAt > now)
- Chưa đạt max usage limit
- User chưa dùng hết personal usage limit
- Điều kiện cart thỏa mãn: min order value, specific products, specific categories
```

---

### Edge Cases: Đừng Bỏ Qua

Edge case là những tình huống "điều gì xảy ra khi...":
- Khi hai action xảy ra đồng thời (concurrent)
- Khi user ở trạng thái trung gian (đang làm dở)
- Khi data không như kỳ vọng (null, empty, max value)
- Khi external dependency fail

**Cách tìm edge cases:**

Với mỗi user story, hỏi:
1. Điều gì xảy ra nếu field này là null/empty?
2. Điều gì xảy ra nếu 2 user làm cùng một việc cùng lúc?
3. Điều gì xảy ra nếu user làm bước 3 mà không làm bước 2?
4. Min và max value là gì? Điều gì xảy ra ở boundary?
5. Điều gì xảy ra nếu external service (email, payment) không respond?

---

### Template: Business Rules Section

```markdown
## Business Rules

### BR-001: [Tên]
[Mô tả]

### BR-002: [Tên]
[Mô tả]

...
```

Đặt section này **sau** User Flows và **trước** Non-Functional Requirements trong PRD.
Reference từ User Flows bằng "(xem BR-00X)".

---

*Tiếp theo: [05-acceptance-criteria.md](./05-acceptance-criteria.md) — Viết AC đúng cách và đúng thời điểm.*
