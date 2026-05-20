# Phase 1: Discovery & Requirements

> **Mục tiêu tổng quát:** Biến yêu cầu mơ hồ, chưa rõ ràng từ khách hàng thành một tập yêu cầu đã được validate — đủ rõ để team có thể bắt tay vào phân tích và lên kế hoạch.

**Đầu vào:** Yêu cầu ban đầu từ khách hàng (có thể chỉ là ý tưởng, email, cuộc họp ngắn)
**Đầu ra:** PRD v1.0 được khách hàng ký duyệt + Stakeholder Register + Competitive Analysis

---

## RACI Phase 1

> **R** = Responsible (người thực hiện) | **A** = Accountable (chịu trách nhiệm kết quả) | **C** = Consulted (được hỏi ý kiến) | **I** = Informed (được thông báo)


| Hoạt động                  | PM/PO   | BA    | Tech Lead | Designer | QA  | Client |
| -------------------------- | ------- | ----- | --------- | -------- | --- | ------ |
| Kickoff meeting            | **A/R** | R     | C         | I        | I   | **R**  |
| Stakeholder Mapping        | **A**   | **R** | C         | I        | I   | C      |
| Discovery Interviews       | **A**   | **R** | I         | I        | I   | **R**  |
| Research & Benchmark       | **A**   | **R** | C         | C        | I   | I      |
| Viết PRD                   | **A**   | **R** | C         | C        | C   | C      |
| PRD Walkthrough & Sign-off | **A/R** | R     | C         | I        | I   | **A**  |


*Ở nhiều team nhỏ, PM và BA là một người. Nếu không có BA riêng, PM đảm nhận toàn bộ cột BA.*

---

## Step 1.1 — Tiếp Nhận Yêu Cầu Ban Đầu

### Tại sao bước này quan trọng?

Khách hàng thường đến với **giải pháp trong đầu**, không phải **vấn đề**. Ví dụ: "Tôi muốn một app quản lý kho" — nhưng vấn đề thực sự có thể là "tôi mất hàng giờ đồng hồ để tính tồn kho mỗi tháng". Nếu chỉ nghe giải pháp và đi làm ngay, rất có thể build sai thứ. Bước này đặt nền tảng để khám phá vấn đề thực sự.

### Focus vào

- Nghe nhiều hơn nói — để khách hàng trình bày tự nhiên trước
- Ghi chép **nguyên văn** những gì khách hàng nói (quote lại, đừng paraphrase ngay)
- Chú ý từ khóa về pain point, mong muốn, kỳ vọng
- **Không** đề xuất giải pháp kỹ thuật trong buổi này
- Chú ý những gì khách hàng **không** nói — đôi khi quan trọng hơn những gì họ nói

### Các hoạt động

- Tổ chức buổi kickoff meeting (60-90 phút)
- Ghi âm/ghi chép toàn bộ nội dung (xin phép trước)
- Hỏi câu hỏi mở: "Vấn đề hiện tại là gì?", "Thành công trông như thế nào với bạn?"
- Thu thập tài liệu liên quan: báo cáo, quy trình hiện tại, screenshot hệ thống cũ (nếu có)
- Viết **Meeting Minutes** và gửi cho khách hàng trong vòng 24 giờ

### Meeting Minutes Template

```markdown
## Meeting Minutes — Kickoff [Tên dự án]
**Ngày:** [Date]  **Thời gian:** [HH:MM – HH:MM]
**Tham dự:** [Danh sách]  **Ghi chép:** [Tên PM/BA]

### Nội dung thảo luận
1. [Điểm 1]
2. [Điểm 2]

### Yêu cầu/mong muốn được đề cập
- "[Quote nguyên văn điều khách hàng nói]"
- "[Quote tiếp theo]"

### Quyết định đã đưa ra
- [Quyết định 1]

### Open Questions (chưa có câu trả lời)
- [Câu hỏi 1 — cần clarify với ai, deadline khi nào]

### Action Items
| # | Người thực hiện | Việc cần làm | Deadline |
|---|----------------|-------------|---------|
| 1 | [PM] | Gửi lại minutes này để confirm | [Ngày] |

*Vui lòng xác nhận nội dung trên chính xác hoặc phản hồi nếu có điểm cần điều chỉnh.*
```

### Tiêu chuẩn đầu ra

- **Raw Requirements Notes**: Tài liệu ghi chép thô, đầy đủ nội dung buổi họp
- **Meeting Minutes** đã gửi và được khách hàng xác nhận
- **Problem Statement sơ bộ**: 1 đoạn mô tả vấn đề cốt lõi bằng ngôn ngữ business
- Danh sách open questions cần trả lời ở bước tiếp theo

---

## Step 1.2 — Stakeholder Mapping

### Tại sao bước này quan trọng?

Trong nhiều dự án thất bại, PM làm việc suốt với một người — rồi đến cuối phát hiện người đó không phải decision maker. Hoặc có một phòng ban bị ảnh hưởng bởi sản phẩm nhưng chưa bao giờ được hỏi ý kiến, và họ block deployment vào phút cuối. Biết rõ "bàn cờ" stakeholder từ đầu giúp tránh surprise muộn.

### Focus vào

- Ai là người **quyết định cuối** (budget, go/no-go)?
- Ai là người **dùng trực tiếp** (end user)?
- Ai có thể **bị ảnh hưởng tiêu cực** và cần được quản lý sớm?
- Ai có thể là **champion nội bộ** — người ủng hộ và thúc đẩy dự án từ bên trong?

### Phân loại Power/Interest

```
                HIGH POWER
                    │
    Keep Satisfied  │  Manage Closely ← Focus ở đây
    (báo cáo định   │  (engage chặt, sign-off quan trọng)
     kỳ, đừng để    │
     surprise)      │
────────────────────┼────────────────── HIGH INTEREST
                    │
    Monitor         │  Keep Informed
    (ít chú ý)      │  (update định kỳ)
                    │
                LOW POWER
```

### Các hoạt động

- Liệt kê tất cả cá nhân/nhóm liên quan đến sản phẩm
- Phân loại vào ma trận Power/Interest
- Xác định kênh giao tiếp phù hợp với từng stakeholder
- Xác định **Sponsor** (chịu trách nhiệm ngân sách + quyết định cuối)
- Xác định **Primary Contact** (người liên hệ hàng ngày)

### Stakeholder Register Template


| Tên      | Tổ chức/Vai trò | Power | Interest | Kỳ vọng chính      | Kênh liên hệ    | Tần suất update     | Ghi chú         |
| -------- | --------------- | ----- | -------- | ------------------ | --------------- | ------------------- | --------------- |
| Nguyễn A | CEO - Sponsor   | Cao   | Cao      | ROI, đúng deadline | Email + meeting | Hàng tháng          | Decision maker  |
| Trần B   | Trưởng phòng KD | Cao   | Cao      | Tính năng X        | Slack + meeting | Hàng tuần           | Primary contact |
| Lê C     | Nhân viên kho   | Thấp  | Cao      | Dễ dùng, ít bug    | Email           | Sau mỗi sprint demo | End user        |


### Tiêu chuẩn đầu ra

- **Stakeholder Map** (Power/Interest matrix)
- **Stakeholder Register** đầy đủ theo template trên
- Đã xác định rõ **Sponsor** và **Primary Contact**
- Communication cadence đã được draft (ai, bao lâu một lần, format gì)

---

## Step 1.3 — Discovery Interviews

### Tại sao bước này quan trọng?

Người dùng thường không biết họ muốn gì cho đến khi họ thấy cái họ **không** muốn. Phỏng vấn trực tiếp giúp phát hiện **unstated requirements** — những yêu cầu ngầm mà người dùng coi là hiển nhiên đến mức không nghĩ đến việc phải nói. Ví dụ: "Tất nhiên hệ thống phải xuất được Excel chứ!" — nhưng chưa ai ghi vào requirements.

### Focus vào

- Hiểu **"tại sao"** trước khi hiểu **"cái gì"**
- Khám phá workflow **hiện tại**: họ đang làm thế nào bây giờ? (As-is state)
- Tìm pain points và bottleneck trong workflow hiện tại
- Hiểu **định nghĩa thành công** của từng stakeholder — thường khác nhau đáng kể

### Các hoạt động

- Chuẩn bị **Interview Guide** riêng cho từng nhóm stakeholder
- Tổ chức 1:1 interviews (30-60 phút/người) hoặc focus group
- Quan sát thực tế nếu có thể (shadowing — ngồi xem họ làm việc)
- Ghi chép và tổng hợp **trong 24 giờ** sau mỗi interview

### Câu hỏi Discovery hiệu quả

```
HIỂU VẤN ĐỀ:
→ "Hãy mô tả cho tôi một ngày làm việc điển hình liên quan đến [vấn đề này]"
→ "Điều gì khiến bạn mất nhiều thời gian/công sức nhất trong công việc hiện tại?"
→ "Điều gì hay đi sai, gây ra lỗi hoặc phải làm lại?"

HIỂU MONG MUỐN:
→ "Nếu vấn đề này được giải quyết hoàn toàn, công việc của bạn thay đổi như thế nào?"
→ "Điều gì là quan trọng nhất, nếu chỉ có thể làm 1 thứ?"

HIỂU LỊCH SỬ:
→ "Bạn đã thử giải pháp nào trước đây? Tại sao không hiệu quả?"
→ "Hệ thống/quy trình hiện tại có gì bạn muốn giữ lại?"

TRÁNH:
→ "Bạn có muốn tính năng X không?" (leading question — dễ làm họ nói "có")
→ Đề xuất giải pháp trong lúc phỏng vấn
```

### User Persona Template

```markdown
## Persona: [Tên đặt cho persona]

**Mô tả:** [1-2 câu tóm tắt]
**Đại diện cho:** [Nhóm user này chiếm bao nhiêu % user base?]

### Demographics
- Vai trò/Chức danh: [...]
- Độ tuổi trung bình: [...]
- Mức độ tech-savvy: Thấp / Trung bình / Cao

### Goals (Điều họ muốn đạt được)
1. [Goal chính]
2. [Goal phụ]

### Pain Points (Vấn đề họ đang gặp)
1. "[Quote từ interview]" — [Tên người được phỏng vấn]
2. [Pain point 2]

### Current Workflow (Họ đang làm như thế nào)
Bước 1 → Bước 2 → Bước 3 → [Điểm đau ở đây] → Bước 4...

### What Success Looks Like
"[Quote về điều họ muốn đạt được]"

### Key Behaviors & Preferences
- Dùng thiết bị: [Desktop/Mobile/Both]
- Môi trường làm việc: [Văn phòng/Di động]
- Tần suất dùng sản phẩm: [Hàng ngày/Hàng tuần]
```

### Tiêu chuẩn đầu ra

- **Interview Notes** cho từng buổi (ai, khi nào, nội dung chính, quotes)
- **Insight Summary**: Tổng hợp patterns và pain points xuất hiện nhiều lần
- **User Personas** (ít nhất 2-3 persona cho các nhóm user chính)
- Ít nhất **3 discovery interviews** đã được thực hiện

---

## Step 1.4 — Research & Benchmark

### Tại sao bước này quan trọng?

Trước khi build từ đầu, cần biết thị trường đã có gì. Có thể có giải pháp tốt hơn cần mua thay vì build. Nếu quyết định build, cần biết chuẩn ngành là gì để không thua kém về UX/feature. Ngoài ra, nghiên cứu còn giúp phát hiện constraint pháp lý có thể block toàn bộ dự án nếu phát hiện muộn.

### Focus vào

- Ai đang giải quyết bài toán tương tự? Họ làm như thế nào?
- Điểm mạnh/yếu của các giải pháp hiện tại?
- **Make vs. Buy analysis**: build mới vs. mua sẵn vs. customize open-source?
- Có constraint pháp lý nào cần lưu ý không?

### Competitive Analysis Template


| Tiêu chí            | [Đối thủ A] | [Đối thủ B] | [Đối thủ C] | Sản phẩm của chúng ta |
| ------------------- | ----------- | ----------- | ----------- | --------------------- |
| Tính năng cốt lõi   | ✅ Có        | ✅ Có        | ❌ Không     | ✅ Có                  |
| Giá cả              | $X/tháng    | $Y/tháng    | Free        | [TBD]                 |
| UX/Ease of use      | ⭐⭐⭐         | ⭐⭐⭐⭐        | ⭐⭐          | Target ⭐⭐⭐⭐⭐          |
| [Tính năng đặc thù] | ❌           | ✅           | ❌           | ✅ (differentiator)    |
| [Điểm yếu]          | [Mô tả]     | [Mô tả]     | [Mô tả]     | —                     |


### Các hoạt động

- Phân tích 3-5 đối thủ hoặc giải pháp tương tự
- Thực hiện **Make vs. Buy analysis**
- Nghiên cứu quy định pháp lý liên quan
- Thu thập data thị trường nếu cần

### Tiêu chuẩn đầu ra

- **Competitive Analysis** theo template trên
- **Make vs. Buy Recommendation** với lý do
- **Research Summary**: insight quan trọng từ nghiên cứu
- Danh sách constraints (kỹ thuật, pháp lý, ngân sách) đã phát hiện

---

## Step 1.5 — Tổng Hợp & Viết PRD Sơ Bộ

### Tại sao bước này quan trọng?

Mọi người trong cuộc — khách hàng, dev, QA, designer — đều đang làm việc dựa trên **mental model của riêng mình** về sản phẩm. Không có PRD, mỗi người sẽ build theo hiểu biết của mình và kết quả sẽ là một mớ rối. PRD là "single source of truth" — khi có conflict, mọi người quay về đây để giải quyết.

### Focus vào

- Phân biệt rõ: **Must Have** vs. **Should Have** vs. **Nice to Have** (MoSCoW)
- Mô tả yêu cầu từ góc nhìn user (user story), không từ góc nhìn kỹ thuật
- **Không mô tả giải pháp kỹ thuật** trong PRD — chỉ mô tả "cần gì" và "tại sao"
- Ghi rõ các **assumptions** (giả định) và **constraints** (ràng buộc) đã biết

### Cấu trúc PRD

```markdown
# PRD: [Tên Sản Phẩm/Feature]
Version: 0.1 | Ngày: | Tác giả: | Status: Draft

## 1. Executive Summary
[Tóm tắt 1 trang: vấn đề, giải pháp đề xuất, kỳ vọng kết quả]

## 2. Problem Statement
**Vấn đề:** [Mô tả vấn đề từ góc nhìn user/business]
**Đối tượng bị ảnh hưởng:** [Ai đang gặp vấn đề này?]
**Tại sao phải giải quyết ngay:** [Business case — nếu không làm, điều gì xảy ra?]

## 3. Target Users & Personas
[Reference đến User Persona đã làm ở Step 1.3]

## 4. Goals & Success Metrics
| Goal | Metric | Baseline hiện tại | Target sau 3 tháng |
|------|--------|-------------------|-------------------|
| [Goal] | [KPI] | [Số hiện tại] | [Số mục tiêu] |

## 5. Scope
**In Scope (phiên bản này):**
- [Feature 1]
- [Feature 2]

**Out of Scope (sẽ không làm trong phiên bản này):**
- [Feature A] — lý do: [...]
- [Feature B] — lý do: [...]

## 6. Functional Requirements
### Epic 1: [Tên]
**US-001:** As a [persona], I want to [action], so that [benefit]
  - Acceptance Criteria:
    - [ ] [Tiêu chí 1]
    - [ ] [Tiêu chí 2]
  - Priority: P0 (Must Have)

### Epic 2: [Tên]
...

## 7. Non-Functional Requirements
- **Performance:** [Ví dụ: API response < 500ms ở P95]
- **Security:** [Ví dụ: Tất cả data phải encrypt at rest]
- **Availability:** [Ví dụ: 99.5% uptime]
- **Scalability:** [Ví dụ: Hỗ trợ 1000 concurrent users]

## 8. Constraints & Assumptions
**Constraints (ràng buộc cứng):**
- Budget: [...]
- Timeline: [...]
- Tech: [...]

**Assumptions (giả định — nếu sai thì phải review lại):**
- [Giả định 1]
- [Giả định 2]

## 9. Open Questions
| # | Câu hỏi | Owner | Deadline | Trả lời |
|---|---------|-------|----------|---------|
| 1 | [Câu hỏi chưa có đáp án] | [Người trả lời] | [Ngày] | [Pending] |

## 10. Appendix
[Interview notes, research data, competitive analysis]
```

### Tiêu chuẩn đầu ra

- **PRD Draft v0.1** đầy đủ theo cấu trúc trên
- Mỗi requirement có priority (MoSCoW) và acceptance criteria
- Success metrics định nghĩa rõ và đo lường được
- Tất cả assumptions được ghi rõ

---

## Step 1.6 — Validate & Sign-off

### Tại sao bước này quan trọng?

Đây là **điểm không thể quay lại** — sau khi sign-off, mọi thay đổi đều phải đi qua Change Management Process. Thay đổi ở Phase 1 tốn 1 giờ để sửa PRD. Thay đổi tương tự ở Phase 4 có thể tốn 3-5 ngày để refactor. Bước này buộc khách hàng đọc kỹ và confirm thay vì "ừ tôi hiểu" mà thực ra không đọc.

### Focus vào

- **Decision maker phải có mặt** trong buổi walkthrough, không phải chỉ người liên hệ
- Đảm bảo họ **thực sự hiểu** scope và goals, không chỉ ký tên
- Làm rõ expectation về timeline và những gì sẽ **không** có trong v1
- Ghi lại tất cả feedback và thay đổi yêu cầu

### Các hoạt động

- Tổ chức **PRD Walkthrough meeting** (60-90 phút)
- Walk through từng section của PRD — không chỉ send file qua email
- Đặc biệt làm rõ phần **Out of Scope** — đây là nguồn conflict thường gặp nhất
- Thu thập feedback bằng văn bản
- Cập nhật PRD theo feedback
- Lấy **chữ ký hoặc email xác nhận** từ Sponsor

### Tiêu chuẩn đầu ra

- **PRD v1.0** đã cập nhật theo feedback
- **Sign-off confirmation**: email hoặc chữ ký từ Sponsor
- Tất cả open questions P0 đã được giải quyết
- Team đã được brief về nội dung PRD

---

## Checklist Tổng Kết Phase 1

- Meeting Minutes từ kickoff đã được khách hàng confirm
- Stakeholder Register đầy đủ, đã xác định Sponsor và Primary Contact
- Ít nhất 3 discovery interviews đã thực hiện
- User Personas đã có (ít nhất 2)
- Competitive Analysis đã hoàn thành
- PRD v1.0 đã có sign-off từ Sponsor
- Không còn open questions P0 nào chưa giải quyết

## Dấu Hiệu Cần Cẩn Thận

⚠️ **"Cứ làm đi rồi tính"** — chưa có sign-off thì không chuyển sang Phase 2, dù bị áp lực deadline
⚠️ **Khách hàng ký mà không đọc** — walk through từng section, hỏi câu hỏi xác nhận hiểu
⚠️ **Chỉ phỏng vấn 1 stakeholder** — người đó có thể không đại diện cho end user thực sự
⚠️ **Requirements xuất hiện mâu thuẫn** — escalate lên Sponsor để quyết định, không tự xử lý
⚠️ **Scope quá lớn** so với ngân sách/timeline — thảo luận về MVP ngay ở đây, không để đến Phase 4

---

*← [SDLC Overview](../SDLC-Overview.md) | [Phase 2: Analysis & Planning →*](02-analysis-planning.md)