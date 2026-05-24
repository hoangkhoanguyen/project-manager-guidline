# 08 — Checklist Review Trước Khi Submit PRD

> Dùng checklist này trước khi gửi PRD cho stakeholder review hoặc trước khi dev bắt đầu sprint.

---

## Checklist A: Nội Dung Cơ Bản

### Problem & Context
- [ ] Problem Statement có data/evidence cụ thể (không chỉ opinion)
- [ ] Business impact được định lượng (revenue, cost, user số lượng)
- [ ] Rõ tại sao làm tính năng này bây giờ (urgency/timing)

### Target Users
- [ ] Ít nhất 1 persona đã được define với goal + pain point cụ thể
- [ ] Rõ ai là primary user (khi conflict, ưu tiên ai)
- [ ] Persona dựa trên research thực tế, không phải suy đoán

### Goals & Metrics
- [ ] Ít nhất 1 primary metric với baseline và target rõ ràng
- [ ] Ít nhất 1 guardrail metric (thứ không được giảm)
- [ ] Timeline đo metric được định rõ (sau 30/60/90 ngày)
- [ ] Không dùng vanity metric (số lớn trông đẹp nhưng không liên quan business)

### Scope
- [ ] Danh sách rõ ràng: cái gì CÓ trong v1
- [ ] Danh sách rõ ràng: cái gì KHÔNG có (defer hoặc explicitly out)
- [ ] Lý do defer được ghi rõ (tránh hiểu nhầm "quên" vs "conscious decision")

---

## Checklist B: Chất Lượng Requirements

### User Stories
- [ ] Tất cả user stories có đủ 3 phần: As a / I want / So that
- [ ] "So that" mô tả business value thực sự, không phải lặp lại "I want"
- [ ] Không có user story nào quá lớn (nếu estimate > 5–8 SP, nên tách nhỏ)

### User Flows
- [ ] Happy path được viết đầy đủ từ trigger đến end state
- [ ] Các alternate flow chính đã được cover
- [ ] Mỗi error case có: message gì + user làm được gì tiếp theo
- [ ] Mọi branching point (IF/ELSE) đều có xử lý cả hai nhánh
- [ ] Không có "nếu X thì sao" nào đang lơ lửng

### Business Rules
- [ ] Tất cả logic nghiệp vụ đã được viết thành BR-XXX riêng biệt
- [ ] Công thức tính toán (nếu có) được viết rõ ràng, có ví dụ số
- [ ] Edge cases quan trọng đã được xử lý trong business rules
- [ ] Không có rule nào conflict với rule khác

### Acceptance Criteria
- [ ] Mỗi user story có ≥ 1 AC
- [ ] Mỗi AC dùng Given-When-Then hoặc format tương đương
- [ ] Mỗi AC là **testable** — QA có thể pass/fail mà không cần interpret
- [ ] Không có AC nào dùng từ mơ hồ: "nhanh", "đẹp", "phù hợp", "dễ dùng"
- [ ] Error messages được define cụ thể (không phải "hiển thị thông báo lỗi")

---

## Checklist C: Viết Đúng Chuẩn

### WHAT vs HOW
- [ ] Scan toàn bộ PRD: không có implementation detail (framework, library, database, API path)
- [ ] Nếu có kỹ thuật được đề cập → đã chuyển vào Constraints/Assumptions section

### Ngôn ngữ
- [ ] Terminology nhất quán — cùng một concept dùng cùng một từ xuyên suốt tài liệu
- [ ] Không có từ mơ hồ: "nhanh", "nhiều", "thường xuyên", "phù hợp" → đã thay bằng số cụ thể
- [ ] Mỗi requirement rõ subject: "Hệ thống...", "User...", không phải passive voice không rõ ai làm

### Completeness
- [ ] Cả Non-Functional Requirements đã được check: performance, security, availability
- [ ] Constraints & Assumptions đã được ghi rõ
- [ ] Assumptions đặc biệt quan trọng: nếu assumption sai thì PRD phải viết lại

---

## Checklist D: Open Questions

- [ ] Tất cả open questions được gom vào 1 chỗ (không rải rác trong file)
- [ ] Mỗi OQ có: owner, deadline, status
- [ ] Không có OQ nào đang blocking implementation mà không có plan để resolve
- [ ] PM đã review và prioritize tất cả OQ trước khi submit

---

## Checklist E: Process

### Before Review
- [ ] Version number và date đã được update
- [ ] Status: Draft → In Review
- [ ] Đã self-review ít nhất 1 lần (không submit ngay sau khi viết xong)

### Three Amigos Readiness
- [ ] User stories đủ rõ để Dev estimate
- [ ] AC đủ rõ để QA bắt đầu plan test strategy
- [ ] Không còn OQ nào mà PM chưa có plan resolve

### Stakeholder Review
- [ ] List người cần review đã được xác định (PM, Tech Lead, QA Lead, key stakeholder)
- [ ] Review deadline được đặt (không để "khi nào rảnh thì review")
- [ ] Feedback channel được định rõ (comment trực tiếp? email? meeting?)

---

## Red Flags: Những Điều Luôn Cần Sửa

Nếu thấy bất kỳ điều nào sau trong PRD → dừng lại và sửa trước khi submit:

🚩 **"Hệ thống cần nhanh"** → Phải có số cụ thể (< X giây P95)

🚩 **"Dùng [technology X] để làm Y"** → Đây là HOW, không phải WHAT. Chuyển vào Constraints hoặc xóa.

🚩 **"[Tính năng Z] sẽ được làm sau"** → Phải ghi rõ vào Scope section với lý do defer. Không để implicit.

🚩 **AC: "Hiển thị thông báo lỗi phù hợp"** → Thông báo chính xác là gì? Rewrite.

🚩 **User story: "As a user, I want to..."** → "User" quá chung. Ai? Buyer? Admin? Seller?

🚩 **Không có guardrail metric** → Luôn phải có ít nhất 1 guardrail để tránh optimize sai chỗ.

🚩 **Open question không có owner/deadline** → OQ không có owner = không ai giải quyết.

🚩 **Scope không có "out of scope" section** → Dễ bị scope creep sau này.

---

## Signing Off

Trước khi từ "In Review" → "Approved":

- [ ] Tất cả reviewer đã đọc và xác nhận
- [ ] Tất cả comments đã được resolve hoặc convert thành OQ
- [ ] Status update thành "Approved" với date và tên người approve
- [ ] Notify toàn team: dev, QA, designer biết PRD đã được approve

---

*Xem thêm ví dụ PRD hoàn chỉnh tại: [example/auto-coupon-prd.md](./example/auto-coupon-prd.md)*
