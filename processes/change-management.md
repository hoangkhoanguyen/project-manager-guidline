# Change Management Process

> **Mục đích:** Xử lý có kiểm soát mọi yêu cầu thay đổi scope, requirements, hoặc thiết kế phát sinh **sau khi đã sign-off**. Đây là một trong những tình huống xảy ra thường xuyên nhất và gây rối loạn nhất nếu không có quy trình.

> **Áp dụng từ:** Phase 2 (sau khi có PRD sign-off) đến hết Phase 6
> **Tham chiếu từ:** Tất cả các phase

---

## Tại Sao Cần Change Management?

Không có dự án nào chạy đúng 100% theo plan ban đầu. Requirements thay đổi vì nhiều lý do:
- Khách hàng hiểu sản phẩm hơn sau khi thấy prototype
- Thị trường thay đổi, đối thủ ra tính năng mới
- Technical constraints phát sinh khi implement
- Stakeholder mới tham gia và có yêu cầu khác
- "Ý tôi không phải vậy" sau khi thấy sản phẩm thực tế

Nếu không có quy trình rõ ràng, thay đổi nhỏ cộng dồn thành **scope creep** — dự án trễ, ngân sách vượt, team kiệt sức, khách hàng vẫn không hài lòng.

---

## Các Loại Thay Đổi

| Loại | Mô tả | Ví dụ |
|------|-------|-------|
| **Minor Change** | Thay đổi nhỏ, không ảnh hưởng timeline/budget | Đổi màu button, thêm field optional |
| **Moderate Change** | Ảnh hưởng 1-2 sprint, cần re-estimate | Thêm 1 tính năng mới vào đang làm |
| **Major Change** | Ảnh hưởng lớn đến scope, timeline, budget | Thay đổi core business logic, thêm module mới |
| **Emergency Change** | Yêu cầu khẩn cấp từ khách hàng hoặc business | Compliance deadline, security incident |

---

## RACI

| Hoạt động | PM/PO | Tech Lead | Dev | QA | Client |
|-----------|-------|-----------|-----|----|--------|
| Tiếp nhận change request | **R** | I | I | I | A |
| Đánh giá impact | C | **A/R** | R | R | I |
| Quyết định approve/reject | **A/R** | C | I | I | C |
| Communicate quyết định | **R** | I | I | I | **R/I** |
| Implement change | I | **A** | **R** | I | I |
| Verify change | I | C | C | **R** | A |

---

## Quy Trình Xử Lý Change Request

### Bước 1 — Tiếp Nhận & Log

**Ai thực hiện:** PM/PO

Bất kỳ ai nhận được yêu cầu thay đổi (qua email, Slack, meeting, phone call) đều phải:
- Không nói "được" hay "không được" ngay lập tức
- Log vào **Change Request Register** trong vòng 24 giờ
- Xác nhận với người yêu cầu: "Tôi đã nhận, sẽ đánh giá và phản hồi trong [X ngày]"

> ⚠️ **Anti-pattern phổ biến nhất:** Dev nhận trực tiếp yêu cầu thay đổi từ khách hàng và bắt tay vào làm mà không thông qua PM. Hậu quả: scope thay đổi mà không ai biết, không có estimate, không có sign-off.

### Bước 2 — Impact Assessment

**Ai thực hiện:** PM (dẫn dắt) + Tech Lead + QA lead

Đánh giá đầy đủ 4 chiều:

```
1. SCOPE IMPACT
   → Bao nhiêu tính năng/use case bị ảnh hưởng?
   → Có mâu thuẫn với requirement nào đã có không?
   → In scope hay Out of scope đã ký?

2. EFFORT IMPACT
   → Cần bao nhiêu ngày dev thêm?
   → Cần bao nhiêu ngày QA thêm?
   → Có phải thiết kế lại gì không?

3. TIMELINE IMPACT
   → Sprint nào bị ảnh hưởng?
   → Milestone nào bị trễ?
   → Có thể compensate bằng cách cắt gì không?

4. COST IMPACT
   → Bao nhiêu tiền thêm (nhân công + infrastructure)?
   → Có trong budget contingency không?
```

**Thời gian đánh giá:**
- Minor Change: 1 ngày
- Moderate Change: 2-3 ngày
- Major Change: 3-5 ngày (cần sprint planning lại)

### Bước 3 — Quyết Định

**Ai quyết định:** PM/PO (sau khi có Impact Assessment)

| Kết quả đánh giá | Quyết định |
|-----------------|------------|
| Impact nhỏ, nằm trong buffer | Approve — thêm vào backlog ngay |
| Impact trung bình, cần đánh đổi | Negotiate — cắt gì để thêm cái này? |
| Impact lớn, ngoài ngân sách/timeline | Reject hoặc defer sang version sau |
| Emergency, không thể trì hoãn | Escalate lên sponsor để quyết định |

> **Nguyên tắc "Triple Constraint":** Scope, Time, Cost — chỉ có thể thay đổi 2 trong 3 cùng lúc. Muốn thêm scope phải hoặc tốn thêm tiền hoặc trễ deadline. Không thể có cả ba.

### Bước 4 — Communicate Quyết Định

**Ai thực hiện:** PM

Bất kể approve hay reject, phải communicate bằng văn bản (email/ticket) trong vòng:
- Minor: 1 ngày làm việc
- Moderate: 2 ngày làm việc
- Major: 3 ngày làm việc

**Template phản hồi (Approve):**
```
Chào [Tên],

Change request [CR-XXX] đã được xem xét và APPROVE.

Thay đổi: [Mô tả ngắn]
Impact:
  - Effort thêm: X ngày
  - Trễ milestone [Y] thêm: Z ngày
  - Chi phí thêm: [Số tiền hoặc N/A]
  - Trade-off: [Tính năng gì bị đẩy ra để làm cái này]

Sẽ được implement trong Sprint [số].

Trân trọng,
[PM]
```

**Template phản hồi (Defer/Reject):**
```
Chào [Tên],

Change request [CR-XXX] đã được xem xét.

Quyết định: DEFER sang v[X.X] vì lý do sau:
  - [Lý do cụ thể: timeline, budget, complexity]
  - Impact nếu implement ngay: [mô tả]

Yêu cầu này đã được thêm vào Product Backlog với priority [P2/P3]
và sẽ được xem xét trong planning của version tiếp theo.

Trân trọng,
[PM]
```

### Bước 5 — Implement & Verify

Nếu Approved:
- PM cập nhật backlog, sprint plan, roadmap
- PM thông báo toàn team về thay đổi
- Dev implement theo sprint cycle bình thường
- QA verify theo acceptance criteria mới
- Cập nhật tài liệu liên quan (PRD, spec, design)

---

## Change Request Register

Đây là bảng tracking tất cả change requests — dù approve hay reject.

| CR-ID | Ngày | Người yêu cầu | Mô tả | Loại | Impact | Quyết định | Ngày quyết định | Sprint implement | Status |
|-------|------|--------------|-------|------|--------|------------|-----------------|-----------------|--------|
| CR-001 | 2026-01-10 | Nguyễn A | Thêm export Excel | Minor | +2 ngày | Approved | 2026-01-11 | Sprint 4 | Done |
| CR-002 | 2026-01-15 | Trần B | Thêm module báo cáo | Major | +3 tuần | Deferred v2.0 | 2026-01-17 | — | Backlog |

**Lưu ý:** Register này phải được cập nhật sau mỗi CR, không phải chỉ khi được nhớ đến.

---

## Change Management trong Từng Giai Đoạn

| Phase | Mức độ linh hoạt | Cost of Change | Ghi chú |
|-------|-----------------|----------------|---------|
| Phase 1 Discovery | Rất cao | Thấp | Chưa có gì để thay đổi |
| Phase 2 Analysis | Cao | Thấp | Dễ điều chỉnh PRD |
| Phase 3 Design | Trung bình | Trung bình | Sửa thiết kế trước khi code |
| Phase 4 Development | Thấp | Cao | Thay đổi = refactor code |
| Phase 5 Testing | Rất thấp | Rất cao | Thay đổi = re-test toàn bộ |
| Phase 6 Deployment | Cực thấp | Cực cao | Thay đổi = có thể cần rollback |

> **Nguyên tắc:** Kéo thay đổi về trái càng nhiều càng tốt. Phát hiện sớm, sửa sớm, tốn ít nhất.

---

## Dấu Hiệu Cần Cẩn Thận

⚠️ Khách hàng nói "chỉ thêm nhỏ thôi" — không có thứ gì "nhỏ thôi" khi đã ở giữa sprint
⚠️ Dev nhận task từ khách hàng trực tiếp, bypass PM — phải ngăn chặn ngay từ đầu dự án
⚠️ PM approve change mà không có Impact Assessment — blind commitment
⚠️ Reject mà không giải thích — khách hàng không hài lòng, relationship bị tổn thương
⚠️ Change Register không được update — mất track, lặp lại discussion cũ, không có lịch sử

---

*[← SDLC Overview](../SDLC-Overview.md)*
