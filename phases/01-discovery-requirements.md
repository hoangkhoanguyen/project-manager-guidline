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

### ✅ VÍ DỤ Executive Summary (ĐÚNG)
**Background:** 
Nhân viên kho hiện tại phải tính tồn kho bằng Excel mỗi tuần, mất 3 giờ/lần và sai số ≈15%.

**Proposed Solution:** 
Xây dựng hệ thống quản lý kho tổng hợp với tính năng tự động tracking, báo cáo real-time, và tích hợp ERP.

**Expected Business Impact:**
- Giảm thời gian tính tồn từ 3h → 30 phút (90% giảm)
- Tăng độ chính xác từ 85% → 99.5%
- Tiết kiệm: 22.5M/tháng (3h × 15 người × 200k/h)
- ROI: 6 tháng (Cost: 200M, monthly savings: 22.5M)

**Timeline:** 4 tháng | **Budget:** 200M | **Priority:** P0

---

## 2. Problem Statement
**Vấn đề:** [Mô tả vấn đề từ góc nhìn user/business - KHÔNG nói giải pháp]
**Đối tượng bị ảnh hưởng:** [Ai đang gặp vấn đề này?]
**Tại sao phải giải quyết ngay:** [Business case — nếu không làm, điều gì xảy ra?]

### ✅ VÍ DỤ Problem Statement (ĐÚNG)

**Current Situation (As-Is):**
Mỗi tuần thứ 5, nhân viên kho phải ngừng công việc bình thường để tính toàn bộ tồn kho. Quy trình:
1. In out danh sách hàng hóa từ hệ thống (không update real-time) → 30 phút
2. Đi từng kệ, kiểm tra hàng vật lý, ghi chú vào giấy → 2 giờ
3. Sau đó về, nhập lại vào Excel, so sánh với hệ thống, tìm sai lệch → 30 phút

**Problem:**
- Quy trình thủ công này gây ra **15% sai số** giữa tồn kho thực tế và tồn kho trong hệ thống
- Mất **3 giờ/tuần × 15 người = 45 giờ công/tuần** chỉ để tính tồn kho
- Dữ liệu không real-time: quản lý lấy báo cáo ngày hôm trước, nhưng đến hôm nay đã có sự thay đổi

**Đối tượng Bị Ảnh Hưởng:**
- **15 nhân viên kho:** Phải làm công việc lặp lại, stress, dễ sai sót
- **Quản lý kho:** Không có visibility real-time, báo cáo luôn delay 1 ngày
- **Sales/Commercial:** Thường không biết tồn kho thực tế → Quote sai, oblige khách hàng, phải apologize

**Tại Sao Phải Giải Quyết Ngay:**
- **Tài chính:** Thua 22.5M/tháng tiền nhân công + overhead
- **Khách hàng:** Đôi khi hứa hàng nhưng không có → customer satisfaction giảm, chảy máu khách
- **Operasi:** Sai số 15% dẫn đến stock-out (không có hàng) hoặc over-stock (hàng nằm kho), cả hai đều mất tiền
- **Nhân sự:** Team mệt mỏi, chất lượng công việc giảm, dễ tìm việc khác

**If We Do Nothing (Scenario 6 tháng nữa):**
- Thua thêm 135M (22.5M × 6)
- Sai số còn cao → khách hàng sẽ chuyển sang đối thủ cạnh tranh
- Nhân viên burnout → phải tuyển thêm hoặc increase lương để giữ người

### ❌ VÍ DỤ Problem Statement (SAI)

**SAI - Vì nó nói giải pháp:**
```
Problem: Nhân viên kho mất thời gian tính tồn kho bằng Excel.
Solution: Chúng ta sẽ build một hệ thống với barcode scan, 
database real-time, API tích hợp SAP, v.v.
```

**Lý do sai:**
- "Barcode scan, database real-time, API tích hợp" ← Đây là **HOW (cách làm)**, không phải **WHAT (vấn đề)**
- Phần này nên nằm ở Functional Requirements (Section 6), không phải Problem Statement
- Problem Statement chỉ nói: "Vấn đề là gì, tại sao nó xảy ra, ai bị ảnh hưởng, tốn bao nhiêu tiền/time"

---

## 3. Target Users & Personas
[Reference đến User Persona đã làm ở Step 1.3]

### ✅ VÍ DỤ Target Users & Personas

**Primary Users:**
- Warehouse Staff (15 người): Nhân viên kho hàng ngày sử dụng
- Warehouse Manager (1 người): Quản lý kho, lấy report hàng ngày

**Secondary Users:**
- Sales/Commercial Team: Check tồn kho trước khi quote
- Finance: Cần số liệu tồn kho cuối tháng để inventory valuation

**Personas:**
- **Minh (Warehouse Staff):** Đã làm kho 3 năm, không quen dùng công nghệ, prefer simple UI
- **Linh (Warehouse Manager):** Cần report real-time, muốn visibility vào chi tiết từng SKU

---

## 4. Goals & Success Metrics
| Goal | Metric | Baseline hiện tại | Target sau 3 tháng |
|------|--------|-------------------|-------------------|
| Giảm thời gian tính tồn kho | Hours/week | 45 giờ/tuần | 5 giờ/tuần |
| Tăng độ chính xác tồn kho | Accuracy % | 85% | 99.5% |
| Cải thiện response time báo cáo | Data freshness | 1 ngày delay | Real-time |
| Tiết kiệm chi phí nhân công | Monthly savings | 0 | 22.5M/tháng |

### ✅ VÍ DỤ Goals & Success Metrics

**Business Goal 1: Reduce Operational Cost**
- **Metric:** Thời gian tính tồn kho (hours/week)
- **Baseline:** 45 giờ/tuần (3 giờ × 15 người)
- **Target:** 5 giờ/tuần (30 phút × 15 người)
- **Why matters:** Tiết kiệm 40 giờ/tuần = 22.5M/tháng

**Business Goal 2: Improve Data Quality**
- **Metric:** Inventory Accuracy (%)
- **Baseline:** 85% (sai số 15%)
- **Target:** 99.5%
- **Why matters:** Giảm stock-out, over-stock → tối ưu cash flow

**Operational Goal 1: Enable Real-time Visibility**
- **Metric:** Freshness của data tồn kho (lag time)
- **Baseline:** 24 giờ (báo cáo hôm trước)
- **Target:** Real-time (update tức thì khi có movement)
- **Why matters:** Sales/Commercial có thể quote chính xác

## 5. Scope
**In Scope (phiên bản v1.0 — 4 tháng đầu):**
- [Feature 1]
- [Feature 2]

**Out of Scope (sẽ không làm trong phiên bản này):**
- [Feature A] — lý do: [...]
- [Feature B] — lý do: [...]

### ✅ VÍ DỤ Scope

**In Scope (v1.0):**
- Hệ thống quản lý tồn kho cơ bản (thêm, xóa, sửa SKU)
- Tính năng inventory count (manual, barcode scan)
- Real-time dashboard cho manager (tồn kho hiện tại)
- Export báo cáo (Excel, PDF)
- User role: Warehouse Staff, Manager, Viewer (Sales)
- Tích hợp cơ bản với ERP hiện tại (API sync)

**Out of Scope (sẽ làm v2.0 nếu client hài lòng):**
- **Multi-warehouse support** — lý do: Phase 1 chỉ focus 1 warehouse, multi-warehouse logic phức tạp, để v2
- **Mobile app native** — lý do: Phase 1 web-based (responsive), native app có thêm test/deployment complexity, đẩy lên v2
- **Advanced forecasting (AI-based demand planning)** — lý do: Cần data 6-12 tháng, chưa có baseline, v2 khi có data
- **Integration với 3rd-party logistics** — lý do: Out of scope business, warehouse chỉ quản lý nội bộ

## 6. Functional Requirements
### Epic 1: [Tên]
**US-001:** As a [persona], I want to [action], so that [benefit]
  - Acceptance Criteria:
    - [ ] [Tiêu chí 1]
    - [ ] [Tiêu chí 2]
  - Priority: P0 (Must Have)

### Epic 2: [Tên]
...

### ✅ VÍ DỤ Functional Requirements

### Epic 1: Inventory Tracking & Management
**US-001: Create/Add New SKU**
As a **Warehouse Manager**, I want to **add a new product SKU** (name, code, unit, location), so that **the system has complete inventory list**

- Acceptance Criteria:
  - [ ] Manager có thể thêm SKU mới với: product name, SKU code, unit (pcs/box/kg), location in warehouse
  - [ ] System không cho phép duplicate SKU code (validation)
  - [ ] Hệ thống tự động assign quantity = 0 khi SKU được tạo (chưa có hàng vào kho)
  - [ ] Manager có thể bulk upload SKU từ Excel (max 500 rows/lần)
  - [ ] Log được ghi: ai tạo, khi nào, thay đổi gì

- Priority: **P0 (Must Have)** — Nếu không có cơ bản này, không thể track gì cả

---

**US-002: Record Inventory Movement (In/Out)**
As a **Warehouse Staff**, I want to **record khi hàng vào hoặc ra khỏi kho**, so that **tồn kho được update real-time**

- Acceptance Criteria:
  - [ ] Staff có thể ghi "Hàng vào kho" (receiving): Scan barcode or manual SKU input → nhập số lượng → confirm
  - [ ] Staff có thể ghi "Hàng ra kho" (outbound): Scan SKU → nhập số lượng → confirm
  - [ ] System check: nếu outbound quantity > tồn kho hiện tại → reject với error message rõ ràng
  - [ ] Mỗi transaction ghi log: SKU, quantity, loại movement, người ghi, thời gian exact
  - [ ] Real-time update: ngay sau khi confirm, dashboard manager thấy tồn kho mới
  - [ ] Staff có thể undo/adjust trong 5 phút, sau đó cần manager approval

- Priority: **P0 (Must Have)** — Core của hệ thống

---

**US-003: Barcode Scan (Optional Phase 1, Can defer to v1.1)**
As a **Warehouse Staff**, I want to **scan barcode thay vì gõ SKU code**, so that **nhập liệu nhanh hơn, sai ít hơn**

- Acceptance Criteria:
  - [ ] System hỗ trợ barcode scanner (hardware: USB or Bluetooth)
  - [ ] Khi scan → tự điền SKU code, chỉ cần nhập quantity
  - [ ] Nếu scan invalid barcode (không match SKU nào) → popup warning
  - [ ] Offline mode: nếu mất kết nối network, scanner vẫn hoạt động, sync khi có lại connection

- Priority: **P1 (Should Have)** — Là improvement, nhưng không critical. Có thể defer sang v1.1 nếu deadline tight

---

### Epic 2: Reporting & Dashboard
**US-004: Real-time Inventory Dashboard**
As a **Warehouse Manager**, I want to **see current inventory level của mỗi SKU in real-time**, so that **biết tình hình kho bất cứ lúc nào**

- Acceptance Criteria:
  - [ ] Dashboard hiển thị: SKU name, current quantity, location, last updated time
  - [ ] Có thể filter/search theo: SKU code, product name, location
  - [ ] Có thể sort theo: quantity (cao→thấp hoặc thấp→cao), last updated, SKU name
  - [ ] Highlight "low stock" items: nếu quantity < min threshold, show với màu đỏ
  - [ ] Update frequency: Real-time (max 1 giây delay)
  - [ ] Responsive design: xem được trên desktop (primary) và tablet (secondary), mobile (nice-to-have)

- Priority: **P0 (Must Have)**

---

**US-005: Weekly Inventory Report**
As a **Manager**, I want to **export weekly inventory report**, so that **gửi cho Finance cuối tuần**

- Acceptance Criteria:
  - [ ] Manager có thể chọn date range → generate báo cáo
  - [ ] Report bao gồm: SKU code, name, opening qty, inbound, outbound, closing qty, variance
  - [ ] Export format: Excel (CSV optional)
  - [ ] Email report automatically khi generate (với recipient list configurable)
  - [ ] Report schedule: có thể set auto-send mỗi Thứ 6 5PM

- Priority: **P1 (Should Have)** — Important cho Finance, nhưng có thể manual first

## 7. Non-Functional Requirements
- **Performance:** [Ví dụ: API response < 500ms ở P95]
- **Security:** [Ví dụ: Tất cả data phải encrypt at rest]
- **Availability:** [Ví dụ: 99.5% uptime]
- **Scalability:** [Ví dụ: Hỗ trợ 1000 concurrent users]

### ✅ VÍ DỤ Non-Functional Requirements

| Tiêu chí | Requirement | Lý do |
|----------|-------------|-------|
| **Performance** | API response time < 200ms (P95) | Warehouse staff dùng barcode scanner, không thể chờ lâu, UX phải smooth |
| | Dashboard load < 2 giây | Manager mở dashboard, cần thấy data ngay |
| **Availability** | 99.5% uptime (tối đa 3.6 giờ downtime/tháng) | Kho hoạt động 8am-6pm, không thể down giữa giờ làm việc |
| | Maintenance window: Thứ 7 10PM - Chủ nhật 2AM | Ngoài giờ hoạt động |
| **Security** | Encrypt data at rest (AES-256) | Chứa dữ liệu hàng hóa, giá trị hàng |
| | Encrypt data in transit (HTTPS/TLS 1.3) | Data phải bảo vệ khi truyền qua internet |
| | Authentication: Password + 2FA cho Manager | Manager access sensitive report |
| | Role-based access control (RBAC) | Warehouse staff không thể xóa transaction; Manager không thể edit inventory count |
| | Audit log: Tất cả action ghi log không thể modify | Compliance & troubleshooting |
| **Scalability** | Hỗ trợ ≥100 concurrent users | Peak time: afternoon shift (15 staff + manager + viewers) |
| | Database: support ≥1 triệu SKU + 1 triệu transactions/tháng | Prepare cho growth |
| **Usability** | UI simple, Warehouse staff (không tech-savvy) có thể dùng trong 1 giờ training | Minh (persona) không quen tech, phải dễ dùng |
| **Browser Support** | Chrome, Firefox, Safari, Edge (latest 2 versions) | Warehouse không control IT infra, staff dùng browser nào cũng được |
| **Mobile Responsive** | Desktop (primary), Tablet (secondary), Mobile (optional) | Manager có thể check dashboard từ office/mệet, staff dùng barcode scanner (không phone) |

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