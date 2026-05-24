# PRD: Tự Động Áp Mã Giảm Giá Tốt Nhất Khi Checkout
**Sản phẩm:** ShopNow (e-commerce platform)
**Version:** 1.0 | **Ngày:** 2026-05-24
**Author:** Khoa Nguyen (PM) | **Status:** Draft — Pending Review
**Reviewers:** Minh (Tech Lead), Linh (Designer), An (QA Lead)

---

## 1. Executive Summary

**Vấn đề:** 67% user ShopNow có ít nhất 1 mã giảm giá hợp lệ trong tài khoản nhưng không áp dụng khi checkout — một phần vì quên, một phần vì mất thời gian tìm mã phù hợp trong danh sách dài. Kết quả: user mất tiền oan, checkout experience kém, và conversion rate thấp hơn mức có thể đạt được.

**Giải pháp đề xuất:** Hệ thống tự động tìm và áp dụng tổ hợp mã giảm giá hợp lệ giúp user tiết kiệm nhiều nhất cho đơn hàng hiện tại. User vẫn có quyền override (thay đổi) lựa chọn này.

**Expected Impact (dự kiến tác động):**
- Tăng checkout conversion rate từ 61% → 68% (+7 điểm %)
- Tăng average order value (AOV — giá trị đơn hàng trung bình) thêm ~12% do user tự tin mua thêm khi biết mình đã được giảm tốt nhất
- Giảm support tickets về "quên áp mã" xuống 40%

**Timeline:** 6 tuần | **Team size:** 1 PM, 1 Designer, 2 Backend Dev, 1 Frontend Dev, 1 QA
**Priority:** P1 — High (sau P0 là các bug critical đang fix)

---

## 2. Problem Statement

### Tình trạng hiện tại (As-Is)

Quy trình áp mã giảm giá hiện tại:
1. User vào màn hình checkout
2. Cuộn xuống phần "Mã giảm giá" → nhấn "Xem mã của tôi"
3. Danh sách hiện ra tất cả mã (có thể 20–30 mã, gồm cả mã không hợp lệ cho đơn này)
4. User tự đọc điều kiện từng mã, tự tính xem mã nào lợi hơn
5. Chọn mã → Nhấn "Áp dụng" → Quay lại màn hình checkout

**Vấn đề xác định được từ data và user research:**

- **67% user có mã hợp lệ nhưng không áp** (data từ analytics — xem Appendix A)
- **Thời gian trung bình ở bước "chọn mã"** là 48 giây — quá lâu cho một bước phụ
- **23% user áp mã sai** (áp mã ít giảm hơn mã khác đang có) — họ không biết mã nào tốt hơn
- **Lý do user không áp mã** (từ 50 user interviews tháng 3/2026):
  - "Tôi quên mất mình có mã" — 41%
  - "Tìm mã phù hợp mất thời gian quá" — 35%
  - "Tôi không hiểu điều kiện của mã, sợ áp sai" — 24%

### Ai bị ảnh hưởng

**User:** Mất tiền oan, trải nghiệm checkout bực bội, mất thêm thời gian.

**ShopNow business:** Conversion rate thấp hơn potential, user satisfaction thấp. Mỗi 1% tăng conversion rate = ~800 triệu VNĐ revenue thêm mỗi tháng ở scale hiện tại.

**Merchant (người bán):** Phát hành voucher nhưng tỷ lệ dùng thấp — giảm hiệu quả marketing.

### Nếu không làm (trong 6 tháng tới)

- Mất khoảng 5.6 tỷ VNĐ revenue potential (7% conversion gap × doanh thu checkout)
- Đối thủ Lazada đã ra tính năng tương tự Q4/2025, Shopee đang beta — nếu chậm thêm sẽ mất lợi thế cạnh tranh
- Support tickets về "coupon không được áp" tiếp tục tốn nhân lực CS team

---

## 3. Target Users & Personas

**Primary User: Casual Shopper — "Hà"**
- Mua sắm online 2–3 lần/tháng
- Có 5–15 mã giảm giá trong tài khoản, không theo dõi hạn dùng
- Không đủ kiên nhẫn để đọc điều kiện từng mã
- Kỳ vọng: "Tôi chỉ muốn checkout nhanh và biết mình được giảm giá tốt nhất"
- **Pain point:** Hay phát hiện ra mình quên áp mã SAU khi đã thanh toán

**Secondary User: Deal Hunter — "Tuấn"**
- Mua sắm online 10+ lần/tháng, chủ động săn voucher
- Biết rõ mình có những mã nào, đọc kỹ điều kiện
- Muốn kiểm soát việc áp mã thay vì để hệ thống tự quyết
- **Pain point:** Sợ hệ thống tự động áp sai ý muốn (ví dụ: muốn giữ mã tốt cho lần sau)

> **Lưu ý thiết kế:** Feature phải phục vụ tốt cho Hà (auto-apply mặc định) nhưng không làm khó Tuấn (override dễ dàng, transparent về lý do chọn mã).

---

## 4. Goals & Success Metrics

### Primary Metrics (đo sau 30 ngày từ khi launch)

| Goal | Metric | Baseline hiện tại | Target |
|------|--------|-------------------|--------|
| Tăng tỷ lệ user áp mã khi checkout | % checkout sessions có áp mã | 33% | 65% |
| Tăng checkout conversion | Checkout-to-payment rate | 61% | 68% |
| Giảm thời gian ở bước coupon | Avg. time spent on coupon step | 48 giây | <10 giây |
| Giảm "coupon misuse" | % user áp mã không tối ưu | 23% | <5% |

### Guardrail Metrics (metrics cần theo dõi để đảm bảo không bị harm)

| Metric | Giải thích | Ngưỡng không được vượt |
|--------|------------|----------------------|
| Coupon redemption cost | Tổng chi phí discount tăng thêm | Không tăng quá 15% so với baseline |
| Support tickets về coupon | Nếu tăng = logic sai hoặc confusing | Không tăng quá 10% |
| User override rate | Nếu quá cao = auto-selection không đáng tin | Theo dõi, target <20% |

### Measuring Success
- **7 ngày sau launch:** Primary metrics đang tracking đúng hướng (không cần đạt target ngay)
- **30 ngày sau launch:** Đánh giá chính thức, quyết định rollout 100% hay cần điều chỉnh

---

## 5. Scope

### In Scope — v1.0

- Tự động tính toán và áp tổ hợp mã giảm giá tốt nhất khi user vào checkout
- Hiển thị rõ mã nào đang được áp và tiết kiệm được bao nhiêu
- Cho phép user xem danh sách mã khác và thay đổi nếu muốn
- Xử lý các loại mã: mã platform (ShopNow), mã của shop, mã freeship
- Tự động remove mã khi user thay đổi giỏ hàng (thêm/bớt sản phẩm)

### Out of Scope — không làm trong v1.0

| Không làm | Lý do |
|-----------|-------|
| Gợi ý mã từ bên ngoài (affiliate, mã bạn bè) | Scope quá rộng, cần data pipeline riêng, để v2 |
| Thông báo push khi sắp hết hạn mã | Feature notification riêng, đang được team khác làm |
| Áp mã tự động cho đơn mua lại (reorder) | Dependency vào feature reorder chưa có |
| Giao diện quản lý mã cho merchant | Merchant portal roadmap riêng |
| So sánh tiết kiệm với lần trước | Nice-to-have, không đủ data foundation, v2 |

---

## 6. User Flows

### Flow 1 — Happy Path: User có mã hợp lệ, không thay đổi gì

```
User vào màn hình Checkout
        ↓
Hệ thống chạy "Best Coupon Engine" trong background (<2 giây)
        ↓
Hiển thị: "Đã áp mã tốt nhất — Tiết kiệm 45.000đ"
[Mã SHOP20 + Freeship FSMAX]  [Thay đổi]
        ↓
User nhấn "Đặt hàng"
        ↓
Order được tạo với discount đã áp
        ↓
Màn hình xác nhận hiển thị breakdown: Giá gốc / Giảm giá / Phí ship / Tổng
```

### Flow 2 — User muốn thay đổi mã hệ thống đã chọn

```
User thấy mã đã được áp tự động
        ↓
Nhấn "Thay đổi"
        ↓
Hiển thị danh sách mã của user, chia 2 nhóm:
  - Hợp lệ cho đơn này (sorted theo số tiền tiết kiệm, cao → thấp)
  - Không hợp lệ (greyed out, hiển thị lý do: "Chưa đạt đơn tối thiểu 200k")
        ↓
User chọn mã khác → Hiển thị ngay số tiền tiết kiệm mới
        ↓
Nhấn "Xác nhận" → Quay lại checkout với mã mới
```

### Flow 3 — User không có mã hợp lệ nào

```
User vào màn hình Checkout
        ↓
Hệ thống kiểm tra → Không có mã hợp lệ
        ↓
Hiển thị: "Bạn chưa có mã giảm giá cho đơn này"
[Khám phá mã giảm giá →] (link đến trang voucher)
        ↓
User tiếp tục checkout bình thường
```

### Flow 4 — Mã bị vô hiệu hóa TRONG LÚC user đang checkout

```
User đang ở màn hình checkout, mã đang được áp
        ↓
[Trong background] Merchant thu hồi mã hoặc mã hết quota
        ↓
Khi user nhấn "Đặt hàng" → Hệ thống validate lại
        ↓
Nếu mã không còn hợp lệ:
  → Hiển thị: "Rất tiếc, mã [SHOP20] vừa hết lượt dùng"
  → Tự động tính lại với mã tốt nhất còn lại (nếu có)
  → Nếu không còn mã nào: hiển thị giá gốc, cho phép tiếp tục
  → User xác nhận lại trước khi đặt hàng
```

### Flow 5 — User thay đổi giỏ hàng sau khi đã chọn mã

```
User ở checkout, đã áp mã SHOP20 (điều kiện: đơn ≥ 200k)
        ↓
User quay lại giỏ hàng, xóa bớt sản phẩm → Đơn còn 150k
        ↓
User vào lại checkout
        ↓
Hệ thống tự động recalculate:
  → Mã SHOP20 không còn hợp lệ → tự động remove
  → Tìm mã tốt nhất cho đơn 150k
  → Thông báo: "Mã SHOP20 đã được gỡ vì đơn hàng dưới 200.000đ"
```

---

## 7. Functional Requirements

### Epic 1: Best Coupon Engine (Logic tính mã)

---

**US-001: Tự động tính tổ hợp mã tốt nhất**

*As a user, I want the system to automatically find the best coupon combination for my cart, so that I don't have to manually compare coupons.*

**Acceptance Criteria:**
- [ ] Khi user vào màn hình checkout, hệ thống tự động chạy Best Coupon Engine mà không cần user thao tác
- [ ] Engine xem xét tất cả mã hợp lệ của user: mã platform, mã shop, mã freeship
- [ ] Engine tìm tổ hợp hợp lệ giúp user tiết kiệm tổng số tiền cao nhất (tổng discount + freeship)
- [ ] Kết quả được hiển thị trong vòng 2 giây kể từ khi user vào màn hình checkout
- [ ] Nếu engine chưa xong sau 2 giây (timeout) → hiển thị section "Mã giảm giá" dạng manual như cũ, không block checkout
- [ ] Khi user thay đổi bất kỳ thứ gì trong giỏ hàng (thêm/bớt sản phẩm, đổi địa chỉ) → Engine chạy lại tự động

**Definition of Done:** Unit test coverage ≥ 80% cho coupon calculation logic. QA pass regression test với 15 test case tổ hợp mã.

---

**US-002: Hiển thị kết quả mã đã áp**

*As a user, I want to clearly see which coupons were applied and how much I saved, so that I trust the system made a good choice.*

**Acceptance Criteria:**
- [ ] Hiển thị rõ: tên/mã code của từng mã đang được áp
- [ ] Hiển thị tổng số tiền tiết kiệm từ mã (không gộp với các discount khác)
- [ ] Nếu áp được freeship: hiển thị riêng "Miễn phí vận chuyển — [Tên mã]"
- [ ] Hiển thị label "Tiết kiệm tốt nhất" để user biết đây là kết quả tối ưu
- [ ] Nếu không có mã nào hợp lệ: hiển thị "Không có mã phù hợp" — không để trống section
- [ ] Breakdown chi tiết hiển thị trước nút "Đặt hàng": Tạm tính / Giảm từ mã / Phí ship / Được giảm phí ship / **Tổng thanh toán**

---

### Epic 2: User Control (Quyền kiểm soát của user)

---

**US-003: Xem và thay đổi mã hệ thống đã chọn**

*As a deal-hunter user, I want to see all my available coupons and override the system's choice if I prefer a different one.*

**Acceptance Criteria:**
- [ ] Có nút "Thay đổi" hoặc "Xem tất cả mã" ngay cạnh phần hiển thị mã đang áp
- [ ] Màn hình danh sách mã chia 2 nhóm rõ ràng:
  - **Có thể dùng cho đơn này** (sorted: tiết kiệm cao nhất → thấp nhất)
  - **Không thể dùng** (greyed out, hiển thị lý do ngắn gọn)
- [ ] Mỗi mã hiển thị: tên mã, điều kiện tóm tắt, số tiền tiết kiệm NẾU áp mã này
- [ ] Khi user hover/tap vào mã "Không thể dùng" → hiển thị lý do cụ thể (xem Business Rules BR-005)
- [ ] Khi user chọn mã khác → Preview ngay số tiền tiết kiệm mới TRƯỚC khi confirm
- [ ] Nút "Dùng mã được gợi ý" luôn có để user quay lại lựa chọn của hệ thống
- [ ] User có thể chọn "Không dùng mã nào" nếu muốn

---

**US-004: Xóa mã đang áp**

*As a user, I want to remove an applied coupon, so that I can choose to checkout without discounts (e.g., saving the coupon for a better purchase later).*

**Acceptance Criteria:**
- [ ] Có nút "Gỡ" hoặc icon X cạnh mỗi mã đang được áp
- [ ] Sau khi gỡ: hệ thống hỏi "Bạn có muốn áp mã tốt nhất còn lại không?" với 2 lựa chọn: [Có, áp mã khác] / [Không, tiếp tục không dùng mã]
- [ ] Nếu user chọn "Không": section mã giảm giá hiển thị dạng collapsed với option "Thêm mã"

---

### Epic 3: Edge Cases & Error Handling

---

**US-005: Xử lý khi mã hết hạn trong lúc checkout**

*As a user, I want to be clearly notified if a coupon becomes invalid while I'm checking out, so that I'm not surprised by a price change when I place the order.*

**Acceptance Criteria:**
- [ ] Khi user nhấn "Đặt hàng", hệ thống validate lại tất cả mã đang áp (real-time call)
- [ ] Nếu mã vẫn hợp lệ: tiếp tục tạo đơn bình thường
- [ ] Nếu mã không còn hợp lệ:
  - Không tạo đơn hàng ngay
  - Hiển thị modal: "Mã [tên mã] vừa hết lượt dùng / hết hạn"
  - Tự động áp mã tốt nhất còn lại (nếu có) và hiển thị
  - Nếu không còn mã: hiển thị giá không có discount
  - User phải nhấn "Xác nhận & Đặt hàng" lại với thông tin mới — không tự động tạo đơn
- [ ] Không bao giờ tạo đơn hàng với giá sai so với những gì user nhìn thấy

---

**US-006: Mã áp nhưng giá không thay đổi (edge case hiếm)**

**Acceptance Criteria:**
- [ ] Nếu tổng discount = 0đ dù mã hợp lệ (ví dụ: mã giảm 10% nhưng maximum cap là 0đ do lỗi config của merchant) → không áp mã đó, log error để team xử lý
- [ ] Không hiển thị "Tiết kiệm 0đ" — gây confusing cho user

---

## 8. Business Rules

**BR-001: Thứ tự ưu tiên loại mã (stacking rules)**

Một đơn hàng có thể áp tối đa:
- 1 mã platform ShopNow
- 1 mã của shop
- 1 mã freeship

Ba loại này có thể stack (dùng cùng nhau). Không thể stack 2 mã cùng loại.

```
Ví dụ hợp lệ:   SHOPNOW20 + SHOP_NIKE10 + FREESHIP50 ✅
Ví dụ không hợp lệ: SHOPNOW20 + SHOPNOW15 (2 mã platform) ❌
```

**BR-002: Cách tính "tốt nhất"**

"Tốt nhất" = tổ hợp mang lại **số tiền user thực trả thấp nhất** (không phải % giảm cao nhất).

```
Ví dụ: Đơn hàng 300k, phí ship 30k
  Mã A: Giảm 20% (tối đa 50k) → tiết kiệm 50k
  Mã B: Giảm 15% không giới hạn + freeship → tiết kiệm 45k + 30k = 75k
  → Hệ thống chọn Mã B dù % thấp hơn
```

**BR-003: Tính hợp lệ của mã**

Mã hợp lệ khi đáp ứng TẤT CẢ:
- Chưa hết hạn (expiry date)
- Chưa hết số lần dùng (quota) — check real-time khi user vào checkout
- Đơn hàng đạt giá trị tối thiểu của mã (minimum order value)
- Sản phẩm trong giỏ thuộc danh mục/shop được áp mã (nếu mã có điều kiện về category)
- User chưa dùng mã này quá số lần tối đa/user (nếu có giới hạn)

**BR-004: Khi có nhiều tổ hợp cùng tiết kiệm bằng nhau**

Nếu 2 tổ hợp cho kết quả tiết kiệm bằng nhau → ưu tiên tổ hợp có mã hết hạn sớm hơn (để tránh mã bị expire lãng phí).

**BR-005: Lý do hiển thị khi mã không hợp lệ**

| Điều kiện không đạt | Text hiển thị |
|--------------------|---------------|
| Đơn chưa đạt giá tối thiểu | "Cần mua thêm [X]đ để dùng mã này" |
| Mã đã hết hạn | "Mã đã hết hạn ngày [date]" |
| Mã đã hết lượt dùng | "Mã đã hết lượt sử dụng" |
| Sản phẩm không áp dụng | "Mã chỉ áp dụng cho [category/shop name]" |
| User đã dùng hết lượt | "Bạn đã dùng mã này [X] lần (tối đa [Y] lần)" |

**BR-006: Recalculation triggers (khi nào chạy lại engine)**

Engine chạy lại khi:
- User vào màn hình checkout
- User thay đổi số lượng sản phẩm trong giỏ
- User thêm hoặc xóa sản phẩm
- User đổi địa chỉ giao hàng (vì freeship condition có thể thay đổi theo vùng)
- User đổi phương thức thanh toán (một số mã chỉ áp với ví điện tử)

**BR-007: Mã do merchant phát hành vs mã do ShopNow phát hành**

- Cả hai đều được engine xét đến
- Nếu merchant thu hồi mã sau khi user đã áp → xử lý theo Flow 4
- Chi phí discount từ mã ShopNow: ShopNow chịu. Chi phí từ mã merchant: merchant chịu theo contract đã ký.

---

## 9. Non-Functional Requirements

| Tiêu chí | Requirement | Lý do |
|----------|-------------|-------|
| **Performance** | Engine tính toán xong trong < 1.5 giây (P95) | Không block user experience khi vào checkout |
| | UI render kết quả trong < 2 giây tổng (kể cả network) | Người dùng mobile 4G cần responsive |
| **Accuracy** | Kết quả engine phải đúng 100% — không bao giờ áp mã không hợp lệ | Sai 1 lần = mất trust, support tickets tăng đột biến |
| **Availability** | Engine available 99.9% uptime | Nếu engine down → graceful fallback sang manual mode (không down toàn bộ checkout) |
| **Scalability** | Xử lý được 5.000 concurrent checkout sessions | Peak cuối tuần sale lớn |
| **Security** | User chỉ nhìn thấy mã của chính mình, không ai khác | RBAC chuẩn, không expose coupon code của user khác qua API |
| **Logging** | Log đầy đủ: user ID, cart value, mã xét đến, mã chọn, lý do chọn, kết quả | Debug khi có complaint, audit khi merchant dispute |

---

## 10. Constraints & Assumptions

**Constraints (ràng buộc cứng):**
- Engine phải dùng coupon data từ Coupon Service hiện tại (không build service mới) — Tech Lead đã confirm feasible
- Không thể thay đổi coupon redemption logic ở backend vì ảnh hưởng các flow khác — phần tính toán "tốt nhất" chạy ở application layer
- Launch phải xong trước 15/7 vì sau đó là mùa sale lớn (7/7, 11/11 chuẩn bị)

**Assumptions (giả định — nếu sai phải review lại PRD):**
- Coupon Service API đủ nhanh để engine call real-time (< 500ms per call) — cần Tech Lead confirm với benchmark
- Merchant đã accept TOS về việc ShopNow có thể auto-apply mã họ phát hành — cần Legal confirm
- User có trung bình ≤ 30 mã trong tài khoản — nếu nhiều hơn cần xem lại performance của engine
- Không có yêu cầu về offline mode cho tính năng này

---

## 11. Open Questions

| # | Câu hỏi | Owner | Deadline | Status |
|---|---------|-------|----------|--------|
| 1 | Coupon Service API có hỗ trợ batch query (lấy nhiều mã 1 lần) để engine không phải gọi từng mã? Nếu không, latency sẽ cao | Minh (Tech Lead) | 30/5 | Open |
| 2 | Legal cần confirm: merchant có đồng ý trong TOS để ShopNow tự động áp mã họ phát hành không? | Hoa (Legal) | 2/6 | Open |
| 3 | Nếu user có 2 mã cùng áp được, cùng tiết kiệm bằng nhau, nhưng 1 mã sắp hết hạn và 1 mã còn lâu — user có muốn biết không? Hay chỉ cần hệ thống tự ưu tiên mã sắp hết? | Linh (Designer) + UX research | 5/6 | Open |
| 4 | Có cần A/B test trước khi rollout 100%? Nếu có, split như thế nào (50/50?) | Khoa (PM) | Quyết định sau khi có câu trả lời Q1 và Q2 | Pending |

---

## 12. Appendix

**Appendix A — Data Sources**
- Analytics: Dashboard "Coupon Usage Funnel" trên Mixpanel (link nội bộ)
- User research: "Checkout Pain Points Study — March 2026" — 50 user interviews (file đính kèm)
- Competitive: Lazada auto-coupon feature review (product teardown tháng 4/2026)

**Appendix B — Technical Notes (từ buổi kickoff với Tech Lead)**
- Engine sẽ chạy ở Application Service layer, không phải ở Coupon Service
- Coupon Service expose `/coupons/validate` endpoint — cần thảo luận batch API
- Cần thêm event tracking: `coupon_auto_applied`, `coupon_overridden`, `coupon_removed`

**Appendix C — Designs**
- Figma link: [TBD — Linh sẽ update sau khi PRD được approve]

---

*Lịch sử thay đổi:*
*v0.1 — 20/5: Draft đầu tiên (Khoa)*
*v0.2 — 24/5: Thêm Business Rules sau buổi làm việc với Minh (Tech Lead), update Flow 4 và 5*
*v1.0 — [Chờ review]*
