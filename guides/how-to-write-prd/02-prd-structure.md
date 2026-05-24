# 02 — Cấu Trúc PRD: 10 Sections Chuẩn

> Đây là skeleton của mọi PRD. Dùng như template, scale up hoặc compress tùy scope.

---

## Full Template

```markdown
# PRD: [Tên Tính Năng / Sản Phẩm]

**Version:** 1.0  
**Status:** Draft / In Review / Approved  
**Author:** [Tên]  
**Last Updated:** YYYY-MM-DD  
**Reviewers:** [PM, Tech Lead, QA Lead, Designer]

---

## 1. Executive Summary
## 2. Problem Statement
## 3. Target Users
## 4. Goals & Success Metrics
## 5. Scope
## 6. User Flows
## 7. Functional Requirements
## 8. Business Rules
## 9. Non-Functional Requirements
## 10. Constraints & Assumptions
## 11. Open Questions
## 12. Appendix
```

---

## Section 1: Executive Summary

**Mục đích:** Ai cũng đọc section này. Phải hiểu toàn bộ bức tranh chỉ qua 5–10 dòng.

**Cần có:**
- Vấn đề đang giải quyết là gì (1 câu)
- Giải pháp đề xuất là gì (1–2 câu)
- Ai là user chính
- Metric thành công chính

**Ví dụ:**
> Hiện tại 67% user có coupon hợp lệ nhưng không apply khi checkout, khiến conversion rate thấp hơn tiềm năng. Tính năng này tự động tìm và áp dụng coupon tốt nhất cho user trong bước thanh toán. Target: tăng % checkout có áp coupon từ 33% lên 65% trong vòng 90 ngày sau launch.

---

## Section 2: Problem Statement

**Mục đích:** Chứng minh bằng data rằng vấn đề có thật và đáng giải quyết.

**Cấu trúc chuẩn:**
1. **Current state (As-Is):** Hiện tại user đang làm gì / trải qua gì?
2. **Pain points:** Khó khăn cụ thể là gì? Có data không?
3. **Business impact:** Vấn đề này ảnh hưởng đến business thế nào?

**Ví dụ:**

> **Current State:** User phải tự tìm coupon trong tab "Mã giảm giá" rồi quay lại nhập tay.
>
> **Pain Points:**
> - 67% user có coupon hợp lệ nhưng không apply (tracking data, T3/2025)
> - Thời gian trung bình ở bước nhập coupon: 48 giây
> - 23% user apply coupon không tối ưu (không phải coupon giảm nhiều nhất)
>
> **Business Impact:** Ước tính $1.2M/tháng GMV bị bỏ qua do coupon không được apply.

**Lưu ý:** Đừng đề xuất giải pháp trong section này. Chỉ mô tả vấn đề.

---

## Section 3: Target Users

**Mục đích:** Xác định rõ viết cho ai — đủ cụ thể để biết ưu tiên khi có conflict.

**Cấu trúc chuẩn (Persona):**

```
### Persona 1: [Tên đại diện] — [Vai trò]

**Đặc điểm:** [1–2 câu mô tả]
**Goal:** Họ muốn đạt được gì?
**Pain Point:** Họ đang gặp khó khăn gì liên quan đến tính năng này?
**Context:** Họ dùng sản phẩm trong hoàn cảnh nào? (mobile/desktop, busy/relaxed)
```

**Ví dụ:**

> **Hà — Người mua hàng thông thường**
> Đặc điểm: 28 tuổi, mua hàng online 2–3 lần/tháng, dùng app trên điện thoại.
> Goal: Thanh toán nhanh, không cần suy nghĩ nhiều.
> Pain Point: Biết mình có coupon nhưng không muốn mất thời gian tìm.

**Primary vs Secondary user:**
- Primary: User mà tính năng phục vụ trực tiếp nhất
- Secondary: User bị ảnh hưởng gián tiếp (ví dụ: admin quản lý coupon)

Khi có conflict giữa primary và secondary: ưu tiên primary.

---

## Section 4: Goals & Success Metrics

**Mục đích:** Định nghĩa thế nào là "thành công" — đo được, không phải cảm tính.

**Cấu trúc:**

| Loại Metric | Mô tả |
|-------------|-------|
| **Primary Metric** | Metric chính, tăng/giảm để biết tính năng có hiệu quả không |
| **Secondary Metric** | Metric bổ trợ, giúp explain primary |
| **Guardrail Metric** | Metric không được giảm (tránh optimize primary mà phá thứ khác) |

**Ví dụ:**

| Metric | Baseline | Target | Timeline |
|--------|----------|--------|----------|
| % checkout có apply coupon | 33% | 65% | 90 ngày |
| Checkout conversion rate | 61% | 68% | 90 ngày |
| Avg time ở coupon step (guardrail) | 48s | ≤ 5s | Launch day |
| Order cancellation rate (guardrail) | 3.2% | ≤ 3.2% | 90 ngày |

**Lưu ý quan trọng:**
- Luôn có baseline (số hiện tại). Không có baseline = không đo được improvement.
- Guardrail metric bảo vệ bạn khỏi optimize sai chỗ.
- Tránh "vanity metric" (số lớn trông đẹp nhưng không liên quan đến business outcome).

---

## Section 5: Scope

**Mục đích:** Định rõ ranh giới. Cái gì CÓ trong v1, cái gì KHÔNG có.

**Cấu trúc:**

```markdown
### Trong scope (v1)
- [Feature A]
- [Feature B]

### Ngoài scope (v1) — có thể xem xét v2
- [Feature C] — lý do defer
- [Feature D] — lý do defer

### Explicitly out of scope (không bao giờ làm trong tính năng này)
- [Feature E]
```

**Ví dụ:**

> **Trong scope:** Tự động áp coupon tốt nhất khi user vào trang checkout. User có thể chọn coupon khác.
>
> **Defer sang v2:** Notification push khi coupon sắp hết hạn. Gamification "Bạn tiết kiệm được X đồng".
>
> **Ngoài scope hoàn toàn:** Tạo/phát hành coupon mới (thuộc Marketing module, không liên quan).

**Tại sao section này quan trọng:**
Scope creep — yêu cầu mở rộng tính năng giữa chừng — là nguyên nhân phổ biến nhất khiến dự án trễ. Viết scope rõ ràng giúp bạn có căn cứ nói "cái đó ngoài scope" khi bị yêu cầu thêm.

---

## Section 6: User Flows

> Xem chi tiết tại `04-flows-and-rules.md`

**Mục đích:** Mô tả step-by-step user làm gì, hệ thống phản hồi gì — cho mọi scenario quan trọng.

**Các loại flow cần có:**
1. Happy Path (main flow — mọi thứ đúng)
2. Alternate Flow (user chọn con đường khác)
3. Error Flow (điều gì đó sai)
4. Edge Case (tình huống đặc biệt)

**Format tối thiểu (text-based):**

```
Flow 1: Auto-apply coupon khi checkout

1. User nhấn "Thanh toán"
2. Hệ thống kiểm tra coupon hợp lệ trong tài khoản user
3. [IF có coupon] Hệ thống tính toán coupon giảm nhiều nhất
4. Coupon được hiển thị đã áp dụng, hiển thị số tiền tiết kiệm
5. User xem tóm tắt đơn hàng và tiến hành thanh toán

[IF không có coupon] → Flow 3: Không có coupon
[IF nhiều coupon cùng giảm giống nhau] → Business Rule BR-007
```

---

## Section 7: Functional Requirements

**Mục đích:** Liệt kê đầy đủ những gì hệ thống CẦN LÀM từ góc nhìn user.

**Format chuẩn: User Story + Acceptance Criteria**

```
### US-001: [Tên ngắn của user story]

**User Story:**
As a [type of user],
I want to [do something],
So that [I get some benefit].

**Acceptance Criteria:**
- GIVEN [điều kiện ban đầu]
  WHEN [hành động của user]
  THEN [kết quả mong đợi]
```

**Ví dụ:**

```
### US-001: Tự động áp coupon tốt nhất

User Story:
As a logged-in buyer,
I want the system to automatically apply my best available coupon,
So that I save the most money without having to search manually.

Acceptance Criteria:
- GIVEN user đã login và có coupon hợp lệ trong tài khoản
  WHEN user vào trang checkout
  THEN hệ thống tự động hiển thị coupon đã được áp dụng, kèm số tiền tiết kiệm

- GIVEN user đã login nhưng không có coupon hợp lệ
  WHEN user vào trang checkout
  THEN không hiển thị coupon section (không để trống gây confuse)
```

> Xem chi tiết cách viết AC tại `05-acceptance-criteria.md`

---

## Section 8: Business Rules

> Xem chi tiết tại `04-flows-and-rules.md`

**Mục đích:** Tách logic nghiệp vụ ra riêng để dễ maintain, dễ reference từ nhiều chỗ.

**Format:**

```
### BR-001: [Tên rule ngắn gọn]
[Mô tả rule]
```

**Ví dụ:**

```
### BR-001: Stacking rule
User chỉ có thể áp dụng tối đa: 1 coupon platform + 1 coupon shop + 1 coupon freeship cùng lúc.
Không thể áp 2 coupon platform hoặc 2 coupon shop cùng lúc.

### BR-002: Định nghĩa "coupon tốt nhất"
Coupon tốt nhất = coupon dẫn đến số tiền user phải trả thấp nhất sau khi áp dụng tất cả coupon hợp lệ trong cùng category.
```

---

## Section 9: Non-Functional Requirements

**Mục đích:** Những yêu cầu về chất lượng, không liên quan đến tính năng cụ thể.

**Các loại thường gặp:**

| Loại | Ví dụ |
|------|-------|
| **Performance** | "Coupon engine tính toán xong trong < 1.5 giây P95" |
| **Accuracy** | "Coupon được áp đúng 100% — không được sai lệch số tiền" |
| **Availability** | "Uptime 99.9% trong giờ cao điểm" |
| **Scalability** | "Hỗ trợ 5000 concurrent checkout sessions" |
| **Security** | "Coupon không thể bị apply nhiều lần bởi cùng một user" |
| **Accessibility** | "WCAG 2.1 AA — màu contrast ratio ≥ 4.5:1" |

**Lưu ý:** Chỉ viết NFR khi bạn thực sự có số cụ thể hoặc biết đây là constraint thực tế. Tránh viết generic như "hệ thống phải nhanh" — vô nghĩa và không test được.

---

## Section 10: Constraints & Assumptions

**Constraints (ràng buộc):** Những điều bạn không có quyền thay đổi.

```
- Must integrate với payment gateway hiện tại (VNPay) qua REST API
- Must deploy trước ngày 15/08 (campaign deadline)
- Budget engineering: không vượt 2 engineer-months
```

**Assumptions (giả định):** Những điều bạn giả sử là đúng khi viết PRD này.

```
- Giả định backend coupon service đã có API kiểm tra coupon hợp lệ
- Giả định user đã login (flow này không cover guest checkout)
- Giả định coupon data được sync real-time (không phải batch hourly)
```

> **Quan trọng:** Khi assumption bị bác bỏ, PRD phải được cập nhật. Assumptions sai là nguồn gốc của scope creep ngầm.

---

## Section 11: Open Questions

**Mục đích:** Trung tâm theo dõi tất cả những gì chưa được quyết định.

**Format:**

| # | Câu hỏi | Owner | Deadline | Status |
|---|---------|-------|----------|--------|
| OQ-01 | Guest checkout có được auto-apply không? | PM + Business | 2025-08-01 | Open |
| OQ-02 | Khi coupon hết trong lúc đang checkout thì xử lý thế nào? | PM + Tech | 2025-08-05 | Open |

**Nguyên tắc:** Open questions KHÔNG được nằm rải rác trong từng section. Gom hết vào đây để PM nhìn thấy toàn bộ blockers trong một chỗ.

---

## Section 12: Appendix

Những thứ hỗ trợ nhưng không thuộc nội dung chính:
- Link wireframe / Figma
- Link user research report
- Link competitive analysis
- Glossary (nếu có thuật ngữ đặc thù của dự án)
- Changelog (thay đổi PRD theo version)

---

## Một Số Lưu Ý Về Format

- **Version control:** Luôn có version number và date ở đầu file. Khi có thay đổi lớn, bump version.
- **Status:** Draft → In Review → Approved. Đừng build dựa trên Draft chưa được review.
- **Links:** Dùng relative link nếu trong cùng folder, absolute URL nếu đến Figma/external.
- **Length:** PRD không có giới hạn trang. Nhưng nếu một section quá dài, xem xét tách ra file riêng.

---

*Tiếp theo: [03-writing-rules.md](./03-writing-rules.md) — Nguyên tắc viết: WHAT vs HOW, level of specificity.*
