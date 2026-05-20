# Phase 2: Analysis & Planning

> **Mục tiêu tổng quát:** Chuyển yêu cầu đã được sign-off thành kế hoạch thực thi cụ thể, có thể đo lường được. Trả lời câu hỏi: "Làm cái gì, bao lâu, ai làm, rủi ro gì, và nếu yêu cầu thay đổi thì xử lý thế nào?"

**Đầu vào:** PRD v1.0 đã được Sponsor sign-off
**Đầu ra:** Project Charter, Project Plan, Communication Plan, Definition of Done, Roadmap, Risk Register, Resource Allocation

---

## RACI Phase 2

| Hoạt động | PM/PO | BA | Tech Lead | Dev | QA | Designer | Client |
|-----------|-------|----|-----------|-----|----|----------|--------|
| Project Charter | **A/R** | R | C | I | I | I | **A** |
| Business Analysis | **A** | **R** | C | C | C | C | C |
| Scope Definition | **A/R** | R | C | C | C | C | **A/C** |
| Technical Feasibility | C | C | **A/R** | R | I | I | I |
| Risk Assessment | **A/R** | R | R | C | C | I | C |
| Estimate & Resource | **A** | C | **R** | R | R | R | I |
| Communication Plan | **A/R** | R | C | I | I | I | **C** |
| Definition of Done | **A/R** | C | **R** | R | R | C | C |
| Roadmap & Milestone | **A/R** | R | C | C | C | C | **A/C** |

---

## Step 2.0 — Project Charter

### Tại sao bước này quan trọng?
Project Charter là **"giấy khai sinh" chính thức của dự án** — document ngắn (1-2 trang) được cấp trên/sponsor ký để xác nhận dự án được phép bắt đầu và PM có thẩm quyền phân bổ nguồn lực. Không có Charter, dự án tồn tại không chính thức: không ai có thẩm quyền rõ ràng, không có budget được duyệt, và khi conflict phát sinh sẽ không có căn cứ để giải quyết.

### Focus vào
- Phải **ngắn gọn** — ai đọc cũng hiểu trong 5 phút
- **Sponsor ký** — đây mới là điểm trao quyền chính thức cho PM
- Ghi rõ **constraints** (ngân sách tối đa, deadline cứng) để tránh kỳ vọng không thực tế
- Ghi rõ **exclusions** — những gì dự án này sẽ KHÔNG làm

### Project Charter Template
```markdown
# Project Charter: [Tên Dự Án]
**Ngày:** | **Version:** 1.0

## Thông Tin Dự Án
| Hạng mục | Nội dung |
|----------|---------|
| Project Manager | [Tên] |
| Sponsor | [Tên + chức vụ] |
| Khách hàng | [Tên tổ chức/cá nhân] |
| Ngày bắt đầu dự kiến | [Date] |
| Ngày kết thúc dự kiến | [Date] |

## Mục Đích Dự Án
[1-2 câu: Dự án này giải quyết vấn đề gì, cho ai, mang lại giá trị gì?]

## Mục Tiêu & Thành Công
| Mục tiêu | Tiêu chí đo lường thành công |
|----------|------------------------------|
| [Goal 1] | [Metric cụ thể] |
| [Goal 2] | [Metric cụ thể] |

## Phạm Vi Cao Cấp (High-Level Scope)
**Bao gồm:** [Danh sách ngắn gọn major deliverables]
**Không bao gồm:** [Những gì rõ ràng nằm ngoài scope]

## Constraints (Ràng Buộc Cứng)
- Budget tối đa: [Số tiền]
- Deadline cứng: [Ngày — và lý do tại sao cứng]
- Tech/Platform: [Nếu có constraint cụ thể]
- Nhân sự: [Nếu có constraint về team]

## Assumptions (Giả Định)
- [Giả định 1 — nếu sai thì phải review lại charter]
- [Giả định 2]

## Rủi Ro Cấp Cao (sẽ chi tiết hóa ở Risk Register)
- [Rủi ro 1]
- [Rủi ro 2]

## Thành Phần Dự Án (Core Team)
| Vai trò | Tên | Trách nhiệm |
|---------|-----|------------|
| PM/PO | [Tên] | Quản lý dự án, stakeholder |
| Tech Lead | [Tên] | Architecture, technical decisions |
| ... | ... | ... |

## Phê Duyệt
| Vai trò | Tên | Chữ ký | Ngày |
|---------|-----|--------|------|
| Sponsor | [Tên] | | |
| PM | [Tên] | | |
```

### Tiêu chuẩn đầu ra
- [ ] **Project Charter v1.0** theo template trên
- [ ] Đã được **Sponsor ký duyệt**
- [ ] Team core đã được xác định và informed

---

## Step 2.1 — Phân Tích Nghiệp Vụ (Business Analysis)

### Tại sao bước này quan trọng?
PRD mô tả **what** (cần gì) và **why** (tại sao). Business Analysis dịch sang **how it works** — luồng nghiệp vụ cụ thể, điều kiện, exception. Dev không thể code từ PRD — họ cần biết: "Nếu user nhập sai thì sao? Nếu chưa có dữ liệu thì hiển thị gì? Business rule nào apply cho case đặc biệt này?" Business Analysis trả lời những câu hỏi này **trước** khi code.

### Focus vào
- Use case chi tiết hơn (từ PRD sơ bộ → đầy đủ actor, pre/post conditions, exceptions)
- Business rules: điều kiện, ngoại lệ, validation
- Luồng nghiệp vụ: **happy path + alternative paths + error cases**
- Xác định ranh giới hệ thống (system boundary) — cái gì hệ thống làm, cái gì con người làm

### Các hoạt động
- [ ] Vẽ **Business Flow Diagram** (BPMN hoặc flowchart) cho từng use case P0
- [ ] Viết **Use Case Specification** chi tiết
- [ ] Liệt kê tất cả **Business Rules**
- [ ] Xác định các **data entity** chính và quan hệ sơ bộ
- [ ] Clarify tất cả yêu cầu còn mơ hồ với stakeholders

### Use Case Specification Template
```markdown
## UC-001: [Tên Use Case]
**Actor chính:** [Ai thực hiện]
**Actor phụ:** [Hệ thống bên ngoài nếu có]
**Trigger:** [Điều gì khởi động use case này]
**Precondition:** [Điều kiện phải đúng trước khi bắt đầu]
**Postcondition (Success):** [Trạng thái hệ thống sau khi thành công]
**Postcondition (Failure):** [Trạng thái hệ thống khi thất bại]

### Main Flow (Happy Path)
1. [Bước 1 — actor làm gì]
2. [Bước 2 — hệ thống làm gì]
3. [Bước 3...]

### Alternative Flows
**Alt 1 — [Tên tình huống thay thế]:**
- Bắt đầu từ bước [X] của main flow
- [Mô tả flow thay thế]

### Exception Flows
**Ex 1 — [Tên exception]:**
- Xảy ra khi: [Điều kiện]
- Hệ thống làm: [Hành động]
- Kết quả: [Trạng thái cuối]

### Business Rules Áp Dụng
- BR-001: [Rule áp dụng cho use case này]
```

### Tiêu chuẩn đầu ra
- [ ] **Business Flow Diagrams** cho tất cả use cases P0
- [ ] **Use Case Specification** đầy đủ
- [ ] **Business Rules Register** — danh sách tất cả rules, conditions, exceptions
- [ ] Tất cả câu hỏi mơ hồ đã được clarify và ghi lại câu trả lời

---

## Step 2.2 — Xác Định Scope & In/Out of Scope

### Tại sao bước này quan trọng?
Scope creep là nguyên nhân số 1 khiến dự án trễ và vượt ngân sách. "Thêm một tính năng nhỏ thôi" × 20 lần = một sprint bị mất. Scope document + sign-off tạo ra **ranh giới có thể defend** — khi có yêu cầu thêm, PM có tài liệu để nói "cái này ngoài scope đã thống nhất, cần đi qua Change Management Process".

### Focus vào
- **In Scope**: Những gì chắc chắn làm trong phiên bản này
- **Out of Scope**: Những gì **không** làm lần này — viết rõ, không để ai assume
- **Parking Lot**: Ý hay nhưng để lại sau — đừng reject hoàn toàn, chỉ defer
- **MVP vs. Full Product**: Tập tối thiểu để go-live và tạo giá trị

### Các hoạt động
- [ ] Tạo bảng In/Out/Parking Lot rõ ràng
- [ ] Phân loại tất cả requirements theo MoSCoW
- [ ] Xác định MVP scope — tập tối thiểu để đưa ra thị trường
- [ ] Sign-off từ Sponsor

### Tiêu chuẩn đầu ra
- [ ] **Scope Document** với In/Out/Parking Lot rõ ràng
- [ ] **MVP Definition** — danh sách tính năng tối thiểu để launch
- [ ] MoSCoW priority trên tất cả requirements
- [ ] Sign-off từ Sponsor

---

## Step 2.3 — Phân Tích Kỹ Thuật (Technical Feasibility)

### Tại sao bước này quan trọng?
PM commit timeline với khách hàng dựa trên gì? Nếu không có Technical Feasibility, mọi estimate đều là đoán mò. Bước này đảm bảo rằng trước khi commit, team kỹ thuật đã xem qua yêu cầu và xác nhận "làm được, dự kiến mất X". Đặc biệt quan trọng với tích hợp bên thứ 3 — API của bên ngoài có thể không hỗ trợ những gì bạn cần.

### Focus vào
- Có yêu cầu nào chưa rõ cách làm? → cần **technical spike** (PoC nhỏ) trước khi estimate
- Tech stack nào phù hợp với yêu cầu **và** năng lực team hiện tại?
- Tích hợp bên thứ 3: đã có API? Có sandbox để test không? Có charge phí không?
- Constraint về infrastructure, performance, security?
- Technical debt từ hệ thống cũ cần xử lý?

### Các hoạt động
- [ ] Tech stack review với Tech Lead
- [ ] Liệt kê tất cả third-party integrations và đánh giá rủi ro
- [ ] Xác định **technical spikes** cần làm — thường 1-3 ngày/spike
- [ ] Đánh giá infrastructure requirements
- [ ] Review security và compliance technical requirements

### Tiêu chuẩn đầu ra
- [ ] **Technical Feasibility Report**: khả năng thực hiện của từng nhóm yêu cầu
- [ ] **Proposed Tech Stack** với lý do chọn
- [ ] Danh sách **technical spikes** cần làm và estimate thời gian
- [ ] **Integration Map**: sơ đồ tất cả hệ thống bên ngoài cần kết nối

---

## Step 2.4 — Phân Tích Rủi Ro (Risk Assessment)

### Tại sao bước này quan trọng?
Project manager không phải người giải quyết mọi vấn đề — họ là người **không để vấn đề bất ngờ xảy ra**. Risk Assessment buộc team phải suy nghĩ trước về những gì có thể đi sai, từ đó có plan B sẵn sàng thay vì panic khi sự cố xảy ra. Risk Register cũng là công cụ giao tiếp với sponsor — họ cần biết rủi ro nào đang được theo dõi.

### Focus vào
- Rủi ro về requirements (thay đổi nhiều, chưa rõ)
- Rủi ro về nhân sự (thiếu skill, người nghỉ, team mới chưa làm việc cùng nhau)
- Rủi ro về kỹ thuật (complexity cao, chưa làm bao giờ, third-party dependency)
- Rủi ro về dependency (bên thứ 3 chậm, API thay đổi, vendor delay)
- Rủi ro về ngân sách và timeline

### Risk Register Template
| Risk ID | Mô tả rủi ro | Probability | Impact | Risk Score | Mitigation Plan | Contingency | Owner | Review Date | Status |
|---------|------------|-------------|--------|------------|----------------|-------------|-------|-------------|--------|
| R-001 | Key dev nghỉ giữa dự án | Trung bình | Cao | 6/9 | Document knowledge, cross-train | Thuê contractor | PM | 2-tuần/lần | Open |
| R-002 | API bên thứ 3 thay đổi | Thấp | Cao | 3/9 | Pin API version, theo dõi changelog | Implement adapter pattern | Tech Lead | Hàng tháng | Open |

**Cách tính Risk Score:** Probability (1-3) × Impact (1-3) = Score (1-9)
- Score 7-9: Ưu tiên xử lý ngay
- Score 4-6: Monitor chặt
- Score 1-3: Accept hoặc monitor nhẹ

### Tiêu chuẩn đầu ra
- [ ] **Risk Register** đầy đủ theo template
- [ ] Top 5 rủi ro cao nhất có mitigation plan cụ thể và contingency plan
- [ ] Risk review schedule (thường 2 tuần/lần)
- [ ] Sponsor đã được informed về top risks

---

## Step 2.5 — Ước Lượng & Resource Planning

### Tại sao bước này quan trọng?
Estimate là cam kết — khi PM đưa ra deadline, tất cả mọi người sẽ plan theo đó. Estimate sai dẫn đến crunch, burnout, hoặc phải cắt scope vào phút cuối. Buffer không phải là "lười" hay "không tự tin" — đó là sự thừa nhận thực tế rằng luôn có unexpected work trong mọi dự án phần mềm.

### Kỹ thuật estimate
- **Story Points + Velocity**: dùng khi team đã có velocity lịch sử (team cũ, loại task quen thuộc)
- **T-Shirt Sizing (S/M/L/XL)**: dùng khi cần estimate nhanh, tương đối ở level epic
- **3-Point Estimate**: `(Optimistic + 4×Most Likely + Pessimistic) / 6` — dùng khi task mới, nhiều ẩn số

### Các hoạt động
- [ ] Breakdown requirements thành tasks/stories đủ nhỏ để estimate
- [ ] Team tự estimate (Planning Poker hoặc individual + review) — PM **không** áp đặt số
- [ ] Xác định team composition: bao nhiêu FE, BE, QA, DevOps, Designer?
- [ ] Tính toán **actual capacity** (trừ meetings, PTO, overhead)
- [ ] Add **20-30% buffer** cho unexpected work
- [ ] Xác định tool/license/infrastructure cần mua

### Capacity Calculation Example
```
Team: 2 BE + 2 FE + 1 QA + 1 Designer + 1 DevOps
Sprint duration: 2 tuần (10 ngày làm việc)
Gross capacity: 7 người × 10 ngày = 70 person-days

Trừ overhead:
- Daily standup: 7 × 10 × 15min = 17.5h ≈ 2.2 person-days
- Sprint planning, review, retro: ≈ 2 person-days
- Unexpected meetings, ad-hoc support: ≈ 3 person-days

Net capacity: 70 - 7.2 ≈ 63 person-days/sprint
```

### Tiêu chuẩn đầu ra
- [ ] **Effort Estimation Sheet**: breakdown task + estimate + người phụ trách
- [ ] **Resource Plan**: team composition, start date, allocation %
- [ ] **Budget Estimate** với mức độ tin cậy rõ ràng (±10%, ±20%...)
- [ ] Danh sách dependencies nội bộ và ngoại bộ

---

## Step 2.6 — Communication Plan

### Tại sao bước này quan trọng?
Trong nhiều dự án thất bại, vấn đề không phải là kỹ thuật — mà là communication. Stakeholder không biết dự án đang đi đâu, dev không biết requirements thay đổi thế nào, khách hàng lo lắng vì không nghe tin tức. Communication Plan xác định **ai cần biết gì, khi nào, qua kênh nào** — loại bỏ uncertainty và giảm thiểu drama.

### Focus vào
- Mỗi stakeholder group có **nhu cầu thông tin khác nhau** — không one-size-fits-all
- **Frequency phải realistic** — nếu commit weekly report thì phải maintain được
- Xác định rõ **escalation path** — khi có vấn đề, leo lên đâu?
- Ghi rõ **kênh giao tiếp chính thức** vs. informal

### Meeting Cadence Template
| Meeting | Participants | Frequency | Duration | Owner | Output |
|---------|-------------|-----------|----------|-------|--------|
| Daily Standup | Dev team + PM | Hàng ngày | 15 phút | PM | Blockers identified |
| Sprint Planning | Dev team + PM + QA | Đầu mỗi sprint | 2-4 giờ | PM | Sprint backlog |
| Sprint Demo | Team + Stakeholders | Cuối mỗi sprint | 1 giờ | PM | Demo notes, feedback |
| Sprint Retrospective | Dev team + PM | Cuối mỗi sprint | 1 giờ | PM | Action items |
| Stakeholder Update | PM + Key stakeholders | Hàng tuần | 30 phút | PM | Status report |
| Steering Committee | PM + Sponsor + Key leads | Hàng tháng | 1 giờ | PM | Go/No-go decisions |

### Status Report Template (hàng tuần cho stakeholders)
```markdown
## Weekly Status Report — [Tên Dự Án]
**Tuần:** [Số] | **Ngày:** | **PM:** [Tên]

### Overall Status: 🟢 On Track / 🟡 At Risk / 🔴 Off Track

### Progress This Week
- [Điều đã hoàn thành]
- [Điều đã hoàn thành]

### Plan Next Week
- [Điều sẽ làm]

### Risks & Issues
| # | Mô tả | Severity | Action |
|---|-------|----------|--------|
| | | | |

### Milestones
| Milestone | Planned | Forecast | Status |
|-----------|---------|----------|--------|
| [Tên] | [Ngày] | [Ngày] | 🟢/🟡/🔴 |

### Decisions Needed
- [Quyết định cần từ stakeholder, deadline khi nào]
```

### Escalation Path
```
Issue phát sinh
    ↓
PM xử lý (trong thẩm quyền)
    ↓ (nếu quá thẩm quyền hoặc chặn milestone)
Tech Lead + PM thảo luận
    ↓ (nếu cần decision về scope/budget)
Sponsor được notify và quyết định
    ↓ (nếu liên quan đến contract)
Legal/Business giải quyết
```

### Tiêu chuẩn đầu ra
- [ ] **Communication Plan Document**: meeting cadence, channels, frequency cho từng stakeholder
- [ ] **Status Report Template** đã thiết lập
- [ ] Escalation path đã được communicate và confirm
- [ ] Tất cả stakeholders biết kênh nào để liên hệ PM và khi nào expect update

---

## Step 2.7 — Definition of Done (DoD)

### Tại sao bước này quan trọng?
"Done" là từ nguy hiểm nhất trong phát triển phần mềm — mỗi người có định nghĩa khác nhau. Dev nói "done" nghĩa là "code xong". QA hiểu là "đã test". PM muốn là "đã deploy staging". Khách hàng mong đợi "đã deploy production". **Không thống nhất DoD từ đầu** dẫn đến friction liên tục và burndown chart không phản ánh thực tế.

### Definition of Done — 3 Levels

**Level 1: Task/Story Done**
```
Một story được coi là DONE khi:
□ Code implement đúng acceptance criteria
□ Unit tests viết và pass (coverage ≥ target)
□ Code review đã được approve (ít nhất 1 reviewer)
□ CI pipeline pass (build, lint, test)
□ Không có P0/P1 bugs đã biết
□ Deploy lên staging thành công
□ QA verify trên staging pass
□ Technical docs cập nhật (nếu cần)
```

**Level 2: Sprint Done**
```
Một sprint được coi là DONE khi:
□ Tất cả committed stories đạt Level 1 Done
□ Sprint demo đã thực hiện và được acknowledge
□ Retrospective đã tổ chức
□ Sprint velocity đã ghi lại
□ Backlog đã cập nhật theo feedback
```

**Level 3: Release Done**
```
Một release được coi là DONE khi:
□ Tất cả P0/P1 features đạt Level 1 Done
□ UAT sign-off đã có
□ Zero P0/P1 bugs open
□ Performance benchmarks đạt
□ Security scan không còn critical issues
□ Deployment checklist pass
□ Release notes đã viết
□ Monitoring đang active
```

### Tiêu chuẩn đầu ra
- [ ] **DoD Document** cho cả 3 levels, đã được team đồng thuận
- [ ] DoD được post ở chỗ team dễ thấy (confluence, notion, readme...)
- [ ] Sponsor/khách hàng đã hiểu và đồng ý với Level 3 DoD

---

## Step 2.8 — Lập Roadmap & Milestone

### Tại sao bước này quan trọng?
Roadmap không phải là commit cứng — nó là **direction** và **communication tool**. Stakeholders cần biết "khi nào tôi có thể thấy tính năng X?" mà không cần đọc Jira. Milestone tạo checkpoint giúp PM phát hiện sớm nếu dự án đang lệch khỏi kế hoạch.

### Focus vào
- Milestone phải có **deliverable cụ thể**, không chỉ là ngày trong lịch
- Ưu tiên **MVP trước**, enhancement sau
- **Buffer** giữa các milestone — đừng pack đầy 100%
- Alignment với **business calendar** của khách hàng (events, deadlines quan trọng của họ)

### Cấu trúc Roadmap
```
Phase A: Foundation (Tuần 1-4)
  → Milestone M1: Architecture + Design approved

Phase B: Core Development / MVP (Tuần 5-12)
  → Milestone M2: MVP feature-complete (all P0 done)

Phase C: Testing & Hardening (Tuần 13-15)
  → Milestone M3: UAT sign-off

Phase D: Launch (Tuần 16)
  → Milestone M4: Production go-live

Phase E: Stabilization (Tuần 17-20)
  → Milestone M5: Stable 30 days, v1.1 planning begins
```

### Tiêu chuẩn đầu ra
- [ ] **Project Roadmap** — visual timeline (tuần/tháng) trong tool PM đang dùng
- [ ] **Milestone Document**: tên, ngày, deliverable cụ thể, người chịu trách nhiệm
- [ ] **Sprint Plan sơ bộ**: epic/feature chính dự kiến trong từng sprint
- [ ] Sponsor và key stakeholders đã review và đồng ý

---

## Checklist Tổng Kết Phase 2

- [ ] Project Charter đã có Sponsor sign-off
- [ ] Business Flow Diagrams đầy đủ cho tất cả use cases P0
- [ ] Scope document có sign-off, Out of Scope được viết rõ
- [ ] Technical Feasibility được Tech Lead confirm
- [ ] Risk Register có top risks với mitigation + contingency plan
- [ ] Effort estimation được team đồng thuận (không bị áp đặt)
- [ ] Communication Plan đã được communicate cho tất cả stakeholders
- [ ] Definition of Done (3 levels) team đã ký nhận
- [ ] Budget đã được Sponsor approve
- [ ] Roadmap và milestones đã được khách hàng review

## Dấu Hiệu Cần Cẩn Thận

⚠️ **Estimate bị áp đặt** bởi management/khách hàng — dev estimate sẽ chính xác hơn nhiều
⚠️ **Không có buffer** trong timeline — Murphy's Law: bất cứ thứ gì có thể sai, sẽ sai
⚠️ **Technical spike chưa làm mà đã estimate** — estimate từ unknown = rác
⚠️ **Communication Plan chỉ là hình thức** — nếu không maintain weekly report, mất trust với stakeholders rất nhanh
⚠️ **DoD thiếu QA verify** — dev "done" ≠ feature thực sự done

---

*← [Phase 1: Discovery](01-discovery-requirements.md) | [SDLC Overview](../SDLC-Overview.md) | [Phase 3: Design →](03-design.md)*
