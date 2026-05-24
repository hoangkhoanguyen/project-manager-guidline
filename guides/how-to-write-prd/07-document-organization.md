# 07 — Tổ Chức Tài Liệu PRD và Chia Sẻ Với Khách Hàng

---

## Phần A: Cấu Trúc Folder Cho Hệ Thống Lớn

### Nguyên Tắc Tổ Chức

**Cây thư mục theo tầng** — từ tổng quát đến cụ thể. Dev làm domain nào chỉ cần đọc folder domain đó.

```
/docs
  /00-overview
      system-prd.md                    ← Tài liệu mẹ: goals, scope, constraints, stakeholders
      glossary.md                      ← Định nghĩa thuật ngữ dùng chung toàn dự án
      non-functional-requirements.md   ← Performance, security, scalability chung
      architecture-decisions.md        ← Các ADR lớn ảnh hưởng toàn hệ thống

  /01-domains
      /warranty
          feature-spec.md    ← PRD của domain này: user stories, AC đầy đủ
          user-flows.md      ← Các flow diagram bằng text hoặc link Figma
          business-rules.md  ← Business rules riêng của domain
      /products
          feature-spec.md
      /checkout
          feature-spec.md
          business-rules.md  ← Pricing rules, tax rules, coupon rules
      /user-auth
          feature-spec.md

  /02-cross-cutting
      api-contracts.md       ← API endpoint, request/response schema
      data-dictionary.md     ← Định nghĩa từng field, kiểu dữ liệu, constraint
      error-codes.md         ← Bảng mã lỗi chuẩn toàn hệ thống

  /03-decisions-log
      open-questions.md      ← Tất cả câu hỏi chưa có đáp án, ai trả lời, deadline
      change-log.md          ← Thay đổi scope theo thời gian

  /04-references
      wireframes.md          ← Link Figma hoặc embed ảnh
      competitive-analysis.md
      user-research-summary.md
```

---

### Giải Thích Từng Folder

**`/00-overview`** — Ai cũng cần đọc trước.
- `system-prd.md` là entry point. Người mới join team đọc file này trước để hiểu bức tranh lớn.
- `glossary.md` là tài liệu quan trọng nhất bị bỏ qua nhiều nhất — xem note bên dưới.

**`/01-domains`** — Tổ chức theo domain nghiệp vụ, không theo technical layer.
- Dev làm warranty chỉ đọc `/warranty`, không cần biết `/checkout`.
- Tránh file khổng lồ 200 trang mà không ai đọc hết.

**`/02-cross-cutting`** — Những thứ liên quan đến nhiều domain.
- `api-contracts.md`: Tất cả API endpoint, request/response format. Đây là nguồn truth duy nhất để frontend và backend align.
- `data-dictionary.md`: Định nghĩa từng field. Khi PM nói "order_date" và dev nói "created_at", đây là chỗ resolve conflict.
- `error-codes.md`: Bảng mã lỗi chuẩn. Tránh tình trạng mỗi module tự define error message khác nhau.

**`/03-decisions-log`** — Memory của dự án.
- `open-questions.md`: Trung tâm theo dõi tất cả blockers. PM phải review file này mỗi tuần.
- `change-log.md`: Ghi lại "ngày X, quyết định thay đổi scope Y vì lý do Z". Quan trọng khi có stakeholder conflict sau này.

**`/04-references`** — Supporting materials, không phải core requirements.

---

### Glossary: Tài Liệu Quan Trọng Nhất Bị Bỏ Qua

Khi khách hàng nói "order", họ có thể nghĩa là confirmed order.
Khi dev nghe "order", họ nghĩa là database record.
Khi QA test "order", họ hiểu là trạng thái trong workflow.

Ba người nói cùng một từ nhưng hiểu 3 khác nhau → 3 tháng sau mới phát hiện bug.

**Glossary chuẩn hóa ngôn ngữ chung** — càng team lớn, càng quan trọng.

Template entry:

```markdown
### Order
Một giao dịch mua hàng đã được buyer confirm.
- Bắt đầu tồn tại: khi buyer nhấn "Đặt hàng" và payment được authorized
- Kết thúc tồn tại: không bao giờ bị xóa (chỉ thay đổi status)
- Khác với: Cart (chưa confirm), Invoice (tài liệu thanh toán của order)
- Status lifecycle: pending → confirmed → processing → shipped → delivered / cancelled
```

---

### Business Rules Tách Riêng Khỏi User Stories

**Lý do:** Khi rule thay đổi, chỉ sửa một chỗ, không phải tìm trong 50 acceptance criteria khác nhau.

**Ví dụ thực tế:**
Nếu coupon stacking rule thay đổi từ "1 platform + 1 shop" thành "2 shop + 1 platform", bạn chỉ cần sửa `BR-001` trong `checkout/business-rules.md`, không phải edit 7 user stories và 23 AC.

---

### Open Questions Có File Riêng

Open questions KHÔNG nằm rải rác trong từng spec. Gom hết vào `/03-decisions-log/open-questions.md`.

PM cần nhìn thấy toàn bộ những gì chưa được quyết định trong một chỗ, với deadline và owner rõ ràng.

Format:

```markdown
| # | Câu hỏi | Domain | Owner | Deadline | Status |
|---|---------|--------|-------|----------|--------|
| OQ-01 | Guest checkout có được auto-apply không? | checkout | PM | 2025-08-01 | Open |
| OQ-02 | Contact form có backend xử lý không? | contact | BA | 2025-08-05 | Open |
```

---

## Phần B: Chia Sẻ Tài Liệu Với Khách Hàng

### Nguyên Tắc Quan Trọng Nhất

> **Không bao giờ yêu cầu khách hàng tạo tài khoản trên bất kỳ platform nào.**
> Đó là friction không cần thiết và hay làm chậm quá trình sign-off.

---

### So Sánh Các Lựa Chọn

| Lựa chọn | Không cần tài khoản | Chuyên nghiệp | Effort | Phù hợp khi |
|----------|-------------------|---------------|--------|-------------|
| Notion Public Page | ✅ | ⭐⭐⭐⭐ | Thấp | Hầu hết trường hợp |
| Gitbook | ✅ | ⭐⭐⭐⭐⭐ | Trung bình | Tài liệu kỹ thuật nhiều trang |
| Google Docs (link view) | ✅ | ⭐⭐⭐ | Rất thấp | Cần đơn giản, nhanh |
| PDF export | ✅ | ⭐⭐⭐ | Thấp | Sign-off chính thức, milestone |

---

### Lựa Chọn #1: Notion Public Page

Publish trang Notion thành public URL. Khách hàng mở link trên trình duyệt, đọc như website bình thường, không cần tài khoản.

**Ưu điểm:**
- Không cần tài khoản để đọc
- Sidebar navigation, có thể embed ảnh, table, code block
- Khách hàng *có thể* comment nếu muốn tạo tài khoản — nhưng không bắt buộc
- Update real-time, link không cần gửi lại

**Nhược điểm:**
- Đôi khi load chậm
- Design không customizable nhiều

**Cách dùng:** Viết và cộng tác nội bộ trong Notion → Publish selected pages → Gửi link cho khách hàng.

---

### Lựa Chọn #2: Gitbook

Tạo documentation site trông rất chuyên nghiệp, có search, sidebar, versioning.

**Ưu điểm:**
- Không cần tài khoản để đọc
- Có full-text search
- Versioning: giữ nhiều version tài liệu song song (v1.0, v1.1, v2.0)
- Sync được với GitHub repo nếu dùng markdown

**Nhược điểm:**
- Cần setup ban đầu mất 1–2 ngày
- Free tier đủ dùng cho hầu hết dự án

**Phù hợp nhất:** Dự án có nhiều module, tài liệu dài, cần professional look cho enterprise client.

---

### Lựa Chọn #3: Google Docs + "Anyone with link can view"

Share link với quyền "Viewer". Người nhận không cần Gmail hay Google account để đọc.

**Ưu điểm:**
- Không cần tài khoản
- 100% reliable, không phụ thuộc platform niche
- Nếu nhiều file: tạo Google Doc "index" chứa link đến tất cả file còn lại

**Nhược điểm:**
- Không đẹp
- Navigation kém khi tài liệu nhiều trang

**Phù hợp nhất:** Cần share nhanh, không cần đẹp, khách hàng quen Google Workspace.

---

### Lựa Chọn #4: PDF Export — Khi Cần Lưu Trữ Chính Thức

Export từ Notion/Google Docs ra PDF, gửi qua email hoặc upload lên Google Drive/Dropbox link.

**Ưu điểm:**
- Không cần tài khoản, không cần internet sau khi tải về
- Tồn tại mãi, không phụ thuộc platform
- Phù hợp cho milestone sign-off chính thức (có thể ký tên)

**Nhược điểm:**
- Không interactive, không update real-time
- Khi cần sửa phải export lại và gửi version mới

**Phù hợp nhất:** Cuối milestone, cần khách hàng sign-off chính thức.

---

### Workflow Thực Tế Phổ Biến Nhất

```
Viết & cộng tác nội bộ          Share với KH để review           Sign-off chính thức
─────────────────────           ──────────────────────           ──────────────────
Notion hoặc Confluence    →     Notion public page          →    Export PDF + email
(có tài khoản, comment,         hoặc Google Docs link            kèm chữ ký xác nhận
version history)                (không cần tài khoản)
```

---

### Lưu Ý Khi Share

- Luôn note **version number** và **date** trong file trước khi share
- Khi tài liệu update, thông báo cho khách hàng biết (đừng để họ đọc bản cũ)
- Nếu dùng Notion/Gitbook: đặt "Last updated: YYYY-MM-DD" ở đầu mỗi trang
- Nếu dùng PDF: tên file phải có version, ví dụ `PRD-AutoCoupon-v1.2-2025-08-01.pdf`

---

*Tiếp theo: [08-checklist.md](./08-checklist.md) — Checklist review trước khi submit.*
