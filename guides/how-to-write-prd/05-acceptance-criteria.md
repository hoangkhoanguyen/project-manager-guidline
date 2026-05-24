# 05 — Acceptance Criteria

> AC là "hợp đồng" giữa PM và team: tính năng này được coi là done khi những điều này đúng.

---

## AC Là Gì?

**Acceptance Criteria (AC)** là tập hợp điều kiện cụ thể, có thể kiểm tra được, dùng để xác nhận một user story đã được implement đúng.

AC trả lời câu hỏi: **"Làm thế nào chúng ta biết cái này đã xong?"**

---

## Format Chuẩn: Given-When-Then (GWT)

```
GIVEN [điều kiện ban đầu / context]
WHEN [hành động xảy ra]
THEN [kết quả mong đợi]
```

**Ví dụ:**

```
GIVEN user đã login và có 2 coupon hợp lệ trong tài khoản
WHEN user vào trang Checkout
THEN hệ thống hiển thị section "Ưu đãi đã áp dụng" với coupon giảm nhiều nhất đã được chọn
AND hiển thị số tiền tiết kiệm so với giá gốc
AND hiển thị link "Thay đổi" để user chọn coupon khác
```

---

## Một User Story Cần Bao Nhiêu AC?

Không có số cố định. Tuy nhiên:
- Mỗi **happy path scenario** → ít nhất 1 AC
- Mỗi **alternate flow** → ít nhất 1 AC
- Mỗi **error case** quan trọng → ít nhất 1 AC
- Mỗi **edge case** đã được identify → 1 AC

**Cảnh báo:** Quá ít AC = bỏ sót case. Quá nhiều AC = viết spec thay vì AC. Nếu thấy mình đang viết hơn 10 AC cho 1 user story → xem xét tách user story thành nhỏ hơn.

---

## AC Tốt vs AC Tệ

### AC quá mơ hồ (không test được)

```
❌ THEN hệ thống hiển thị thông báo lỗi phù hợp
```

"Phù hợp" là thế nào? QA sẽ hỏi message chính xác là gì.

```
❌ THEN trang load nhanh
```

Nhanh = bao nhiêu giây?

### AC đúng chuẩn (test được)

```
✅ GIVEN coupon đã hết hạn
   WHEN user nhấn "Đặt hàng"
   THEN hệ thống hiển thị message: "Ưu đãi [tên coupon] đã hết hạn. Vui lòng chọn ưu đãi khác."
   AND order KHÔNG được tạo

✅ GIVEN user nhấn "Tiến hành thanh toán"
   WHEN trang Checkout được load
   THEN coupon section xuất hiện trong < 1.5 giây (measured from navigation start)
```

---

## Viết AC Ở Giai Đoạn Nào?

AC không phải viết một lần xong. Nó có **progressive elaboration** — tinh chỉnh dần qua từng phase.

```
Phase 1 (Discovery)
  PRD Draft: User stories + AC cấp cao (skeleton)
  "GIVEN user checkout WHEN có coupon THEN hiển thị coupon tốt nhất"
  → Đủ để estimate, không đủ để build
       ↓
Phase 2 (Analysis)
  PRD Final: AC đầy đủ, cover happy path + error flows
  → Đủ để design, đủ để estimate chính xác hơn
       ↓
Phase 3 (Design) — Three Amigos Session
  PM/BA + Dev + QA ngồi cùng review AC
  QA thêm test cases: "Nếu 2 user dùng cùng coupon cùng lúc thì sao?"
  Dev clarify: "Race condition ở đây cần handle ở backend"
  → AC hoàn thiện, sign off
       ↓
Phase 4 (Development)
  Dev implement dựa trên AC đã sign off
  Có thể thêm AC kỹ thuật nhỏ (unit test level)
       ↓
Phase 5 (Testing)
  QA dùng AC làm basis cho test cases
  Không pass AC → không merge
```

---

## Three Amigos: Quy Trình Quan Trọng Nhất

### Three Amigos Là Gì?

Buổi họp ngắn (30–60 phút) giữa:
- **PM/BA** — business context và requirements
- **Dev** — technical feasibility và implementation concern
- **QA** — test strategy và edge cases

Mục đích: Align 3 góc nhìn trước khi dev bắt đầu code.

### Khi Nào Chạy Three Amigos?

Trước sprint, sau khi user story vào backlog và trước khi được pull vào sprint.

### Output Của Three Amigos

- AC được review và update
- Dev đã hiểu đủ để estimate chính xác
- QA đã biết sẽ test gì
- Open questions được resolve hoặc assign owner

### Cách Chạy Session

**PM/BA present:**
> "Story này là về [user story]. Tôi đã viết AC như này. Có chỗ nào không rõ không?"

**QA hỏi:**
> "Nếu [edge case X] thì expect result là gì?"
> "Message lỗi chính xác là gì để tôi test được?"

**Dev hỏi:**
> "Flow này có race condition không? Nếu 2 user dùng coupon cùng lúc?"
> "Cái này depend vào service Y, nếu Y down thì fallback là gì?"

**PM update AC realtime** trong buổi họp.

---

## Definition of Ready (DoR)

Một user story chỉ được pull vào sprint khi đạt **Definition of Ready**:

- [ ] User story đã được viết rõ (As a / I want / So that)
- [ ] AC đầy đủ, cover happy path + error flow chính
- [ ] AC đã qua Three Amigos và được sign off
- [ ] Không có open question blocking implementation
- [ ] Story đã được estimate (story point)
- [ ] Dependencies đã được identify và không blocking
- [ ] Design/wireframe đã available (nếu có UI changes)

> Nếu chưa đạt DoR → không được pull vào sprint. Đây là rule cứng, không phải suggestion.

---

## Ví Dụ Đầy Đủ: User Story + AC

```markdown
### US-003: Xử lý coupon bị invalidate mid-checkout

User Story:
As a buyer who is on the Checkout page,
I want to be notified immediately when my applied coupon becomes invalid,
So that I can take action instead of unknowingly having a broken order.

Acceptance Criteria:

AC-1: Phát hiện coupon invalid khi submit order
GIVEN user đang ở trang Checkout với coupon X đã được auto-applied
AND coupon X hết hạn TRONG KHI user đang ở trang
WHEN user nhấn "Đặt hàng"
THEN hệ thống validate coupon trước khi tạo order
AND hệ thống KHÔNG tạo order
AND hiển thị message: "Ưu đãi [tên coupon X] đã hết hạn."

AC-2: Auto-apply coupon thay thế
GIVEN coupon X vừa bị invalidate (theo AC-1)
WHEN system check lại danh sách coupon hợp lệ
AND user có coupon Y hợp lệ khác
THEN hệ thống tự động apply coupon Y
AND hiển thị message: "Đã tự động áp ưu đãi tốt hơn cho bạn: [tên coupon Y]"

AC-3: Không có coupon thay thế
GIVEN coupon X vừa bị invalidate (theo AC-1)
AND user không có coupon hợp lệ nào khác
THEN hệ thống không apply coupon nào
AND hiển thị message: "Ưu đãi đã hết hạn. Bạn có thể tiếp tục thanh toán mà không có ưu đãi."
AND hiển thị 2 button: "Tiếp tục không có ưu đãi" và "Tìm ưu đãi khác"
```

---

## Lưu Ý Cuối

- AC là của cả team, không phải của PM. QA và Dev được quyền và nên đề xuất thêm AC.
- Khi AC thay đổi giữa chừng (scope change), phải update PRD và thông báo cả team.
- Đừng dùng AC để mô tả UI chi tiết (màu nút, font size) — đó là job của Design Spec.

---

*Tiếp theo: [06-scale-adjustment.md](./06-scale-adjustment.md) — PRD cho hệ thống lớn vs app nhỏ.*
