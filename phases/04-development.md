# Phase 4: Development

> **Mục tiêu tổng quát:** Xây dựng sản phẩm theo đúng thiết kế, đúng chất lượng, trong từng sprint có thể kiểm soát và đo lường. Code phải clean, có test, và maintainable.

**Đầu vào:** Architecture Document, ERD, API Spec, UI Mockup, Sequence Diagrams, Test Strategy — tất cả đã được approve
**Đầu ra:** Source code đã review, unit test đạt coverage, integration test pass, technical docs cập nhật, sprint demos được stakeholders acknowledge

---

## RACI Phase 4

| Hoạt động | PM/PO | Tech Lead | Dev | QA | Designer | DevOps |
|-----------|-------|-----------|-----|----|----------|--------|
| Setup môi trường | I | **A/R** | R | C | I | **R** |
| Sprint Planning | **A/R** | R | R | R | C | I |
| Implementation | I | **A** | **R** | I | C | I |
| Code Review | I | **A/R** | **R** | I | I | I |
| Unit Testing | I | **A** | **R** | C | I | I |
| Integration Testing | I | **A** | **R** | C | I | I |
| Technical Documentation | I | **A** | **R** | I | I | I |
| Sprint Demo | **A/R** | R | R | R | R | I |
| Retrospective | **A/R** | R | R | R | R | R |

---

## Vai Trò PM/PO Trong Phase Development

> Đây là phần thường bị thiếu nhất khi học về PM — **PM làm gì khi dev đang code?**

Trong lúc dev đang implement, PM **không ngồi chờ** — họ đang làm một bộ công việc song song quan trọng không kém:

### PM làm gì hàng ngày trong Development Phase?

**Track progress & surface blockers sớm:**
- Nhìn sprint board mỗi sáng — task nào chưa bắt đầu quá lâu? Task nào stuck?
- Daily standup: lắng nghe blocker, **hành động ngay trong ngày** để remove blocker
- Nếu dev báo "dependency X chưa có" — PM là người escalate và resolve, không phải dev tự lo

**Manage stakeholder expectations:**
- Gửi weekly status report cho khách hàng/sponsor theo Communication Plan
- Cập nhật roadmap nếu có delay — **thông báo sớm luôn tốt hơn thông báo muộn**
- Không để khách hàng surprise khi demo — brief trước những gì sẽ thấy và không thấy

**Chuẩn bị cho sprint tiếp theo:**
- Backlog refinement — làm rõ stories cho sprint kế tiếp (acceptance criteria, mockup, dependency)
- Clarify requirements mơ hồ với khách hàng **trước khi** dev cần làm việc đó
- Cập nhật Risk Register — có risk nào mới phát sinh không?

**Handle change requests:**
- Khi khách hàng yêu cầu thay đổi (rất thường xuyên), follow [Change Management Process](../processes/change-management.md)
- **Không** để dev nhận task mới trực tiếp từ khách hàng
- Quyết định scope trade-off: thêm tính năng mới → cắt gì ra?

**Chuẩn bị Sprint Demo:**
- Coordinate với dev để có môi trường staging sẵn sàng trước demo
- Chuẩn bị agenda và script demo
- Invite đúng stakeholders

### PM Anti-Patterns Trong Phase Development
❌ Micromanage dev (hỏi "xong chưa?" mỗi giờ)
❌ Approve change request không qua impact assessment
❌ Không gửi weekly update → stakeholders lo lắng → họ bắt đầu gọi điện hỏi → distract team
❌ "Để dev tự xử lý blocker" — blocker là việc của PM, không phải dev

---

## Step 4.1 — Setup Môi Trường & Chuẩn Hóa

### Tại sao bước này quan trọng?
"Works on my machine" là câu nói tốn kém nhất trong phát triển phần mềm. Nếu environment không reproducible, onboarding dev mới tốn 1-2 ngày thay vì 30 phút. CI/CD từ ngày đầu không phải là "nice to have" — nó là safety net đảm bảo code luôn build được và test luôn chạy.

### Các hoạt động
- [ ] Setup **Git repository** và branching strategy
- [ ] Viết **README** — developer mới setup được trong < 30 phút
- [ ] Setup **linter + formatter** và enforce qua pre-commit hook
- [ ] Setup **CI pipeline**: build, test, lint tự động khi push
- [ ] Setup **CD pipeline**: tự động deploy lên staging khi merge vào main
- [ ] Cấu hình **environment variables** và secret management
- [ ] Setup **logging, error tracking** (Sentry...) và **monitoring** cơ bản
- [ ] Tạo **project board** (Jira, Linear, Github Projects) và workflow

### Branching Strategy
```
main          ← production-ready, protected
  └── develop ← integration branch
        ├── feature/[ticket-id]-feature-name
        ├── bugfix/[ticket-id]-bug-description
        └── hotfix/[ticket-id]-issue ← merge vào main VÀ develop
```

### Tiêu chuẩn đầu ra
- [ ] README đầy đủ, setup trong < 30 phút
- [ ] CI pipeline chạy và pass
- [ ] Tất cả dev đã setup local environment thành công
- [ ] Coding standards document đã được team đọc và ký nhận
- [ ] Project board có task breakdown ban đầu

---

## Step 4.2 — Sprint Planning

### Tại sao bước này quan trọng?
Sprint Planning không phải là "PM giao task cho dev". Đó là **collaborative commitment** — team cùng nhau quyết định làm gì trong sprint này, với sự hiểu biết đầy đủ về yêu cầu. Khi team tự chọn và tự estimate, họ có **ownership** và **accountability** cao hơn nhiều so với khi bị giao từ trên xuống.

### Hai phần của Sprint Planning
**Phần 1 (What):** PM/PO present sprint goal và prioritized backlog items
**Phần 2 (How):** Dev team breakdown items thành tasks, estimate, và commit

### Definition of Ready (task phải đạt trước khi vào sprint)
- [ ] Acceptance criteria đã được viết rõ ràng
- [ ] Design/mockup đã có (nếu cần)
- [ ] Không bị block bởi dependency chưa resolve
- [ ] Đã được estimate bởi team
- [ ] Không có task nào > 3 ngày (nếu lớn hơn → breakdown tiếp)

### Tiêu chuẩn đầu ra
- [ ] **Sprint Goal** — 1 câu mô tả giá trị sprint này mang lại
- [ ] Sprint backlog đầy đủ với estimate và owner
- [ ] Không có task nào thiếu acceptance criteria
- [ ] Team đã confirm capacity (đã trừ PTO, meetings, overhead)

---

## Step 4.3 — Implementation (Coding)

### Tại sao bước này quan trọng?
Code là tài sản dài hạn, không phải kết quả tạm thời. Code viết vội hôm nay sẽ là technical debt mà ai đó phải trả trong 6 tháng nữa — và người đó có thể là chính bạn. Coding standards và best practices không phải là "academic" — chúng là sự đầu tư vào maintainability.

### Focus vào
- Code cho **người đọc**, không chỉ cho máy chạy
- Small, focused commits với message rõ ràng
- Không commit code chưa test cơ bản
- Update task status **realtime** trên board
- Raise blocker **sớm** — không im lặng chờ đến cuối sprint

### Coding Best Practices
- **Single Responsibility**: mỗi function/class làm một việc
- **DRY** (Don't Repeat Yourself): không copy-paste logic
- **YAGNI** (You Aren't Gonna Need It): không code "phòng trừ tương lai"
- **Meaningful naming**: tên biến/function tự giải thích được
- **Error handling**: xử lý **tất cả** error cases, không để exception unhandled

### Commit Message (Conventional Commits)
```
feat(auth): add JWT refresh token endpoint
fix(order): correct total calculation when discount applied
docs(api): update authentication endpoint documentation
refactor(user): extract email validation to shared util
test(payment): add unit tests for payment gateway service
```

### Daily Standup — 15 phút mỗi ngày
- Hôm qua tôi đã làm gì?
- Hôm nay tôi sẽ làm gì?
- Có blocker nào không? **(PM lắng nghe và action ngay)**

### Tiêu chuẩn đầu ra
- [ ] Code implement đúng acceptance criteria
- [ ] Không có debug code / console.log còn sót
- [ ] Error handling đầy đủ cho tất cả paths
- [ ] Unit test đã được viết (xem Step 4.5)
- [ ] Self-review diff trước khi tạo PR

---

## Step 4.4 — Code Review

### Tại sao bước này quan trọng?
Code Review là cơ chế **knowledge sharing quan trọng nhất** trong team — không phải chỉ là quality gate. Dev senior review code dev junior không phải để kiểm soát, mà để transfer knowledge. Ngược lại, dev mới nhìn code của người cũ và học patterns. Ngoài ra, bug tìm thấy trong code review tốn ~1 giờ fix. Bug tìm thấy ở production tốn ~1 ngày.

### Quy tắc cho Reviewer
- Review trong vòng **4 giờ làm việc** — không để PR chờ > 1 ngày
- Comment phải **cụ thể và actionable** — "code này không tốt" không đủ
- Phân biệt: `[Blocking]` (phải sửa) vs `[Suggestion]` (nên xem xét) vs `[Nit]` (nhỏ, tùy)
- **Giải thích lý do** — không chỉ "sai", mà "sai vì..."
- Approve khi hài lòng; đừng approve nếu còn Blocking comments chưa resolve

### Quy tắc cho Author
- **Self-review** toàn bộ diff trước khi tạo PR
- PR description: làm gì, tại sao, cách test
- Không merge khi chưa có đủ approvals
- Trả lời tất cả comments: `done`, hoặc giải thích tại sao không làm theo

### PR Checklist (Author)
- [ ] Code chạy local, không break test
- [ ] Đã self-review toàn bộ diff
- [ ] PR mô tả rõ: làm gì, tại sao, screenshot nếu có UI change
- [ ] Không có secret/credential trong code
- [ ] Không có file không liên quan trong PR

### Tiêu chuẩn đầu ra
- [ ] Mỗi PR có ít nhất 1 reviewer approve
- [ ] Tất cả Blocking comments đã resolve
- [ ] CI pipeline pass (test, lint, build)

---

## Step 4.5 — Unit Testing

### Tại sao bước này quan trọng?
Unit tests là **safety net cho refactoring**. Khi có unit test, dev có thể tự tin thay đổi implementation mà không sợ break behavior — vì nếu break, test sẽ bắt. Không có unit test = mỗi lần refactor là một lần adventure không biết kết quả.

### Focus vào
- Test **behavior**, không test implementation detail
- **AAA pattern**: Arrange (chuẩn bị data), Act (gọi function), Assert (verify kết quả)
- Test cả happy path, **edge cases**, và **error cases**
- Unit test phải chạy **nhanh** — không call real database, không call real API (dùng mock)
- Coverage là **indicator**, không phải mục tiêu — 90% coverage với test vô nghĩa < 60% coverage với test tốt

### Coverage Targets
| Loại code | Target |
|-----------|--------|
| Business logic (tính toán, validation, rules) | ≥ 80% |
| Utility functions (dùng nhiều nơi) | ≥ 90% |
| API handlers/controllers | ≥ 70% |
| UI components | ≥ 60% |

### Tiêu chuẩn đầu ra
- [ ] Coverage targets đạt theo bảng trên
- [ ] Tất cả tests pass trong CI
- [ ] Không có flaky tests
- [ ] Test code được review cùng production code trong PR

---

## Step 4.6 — Integration Testing (Developer Level)

### Tại sao bước này quan trọng?
Unit test kiểm tra từng function trong isolation. Integration test kiểm tra **các phần khi ghép lại có hoạt động đúng không**. FE gọi API endpoint thật có nhận đúng data không? Service A publish message queue, Service B consume có xử lý đúng không? **Dev phải làm integration test trước khi đẩy sang QA** — không phải để QA phát hiện những lỗi integration cơ bản.

### Phân biệt Integration Test (Dev) vs. QA Testing
| | Integration Test (Dev) | Functional Testing (QA) |
|---|---|---|
| Ai làm | Developer | QA Engineer |
| Focus | Technical integration đúng | Business logic đúng theo use case |
| Scope | Module-to-module, API endpoints | End-to-end user flows |
| Tool | Supertest, Postman/Newman, pytest | Postman, Cypress, manual |
| Khi nào | Trong sprint, trước merge | Sau feature-complete |

### Các loại Integration Test Dev cần làm
- **API Endpoint Tests**: Gọi thực tế vào API (với database test), verify response format và status codes
- **Database Integration**: ORM queries thực sự trả đúng data không?
- **Third-party Integration**: Mocked responses có reflect thực tế không? Sandbox test nếu có
- **Message Queue**: Message publish rồi consume có đúng format không?
- **Auth Flows**: Login → token → authenticated request hoạt động end-to-end không?

### Tiêu chuẩn đầu ra
- [ ] Tất cả API endpoints có integration test
- [ ] Auth flow integration test pass
- [ ] Third-party integration test với sandbox/mock server
- [ ] Integration tests chạy trong CI (có thể chậm hơn unit test, chạy trên branch merge)

---

## Step 4.7 — Technical Documentation

### Tại sao bước này quan trọng?
Documentation viết **song song khi code** luôn chính xác hơn documentation viết sau khi xong. Khi code xong, dev đã move on mentally sang task khác và không còn nhớ rõ các quyết định design. Ngoài ra, docs là phần thường bị cắt khi áp lực deadline — nếu không làm sớm, thường là không bao giờ làm.

### Loại Technical Documentation Cần Có
**1. API Documentation (auto-generated + manual):**
- Swagger/OpenAPI spec được update khi có API change
- Example requests và responses thực tế
- Authentication guide

**2. Architecture Decision Log:**
- ADRs từ Phase 3 được update khi có quyết định mới trong quá trình dev
- Lý do tại sao làm theo cách này (future dev sẽ cảm ơn)

**3. Runbook / Operation Guide:**
- Cách deploy (step by step)
- Cách config environment variables
- Common issues và cách fix

**4. README cập nhật:**
- Setup instructions luôn chính xác
- How to run tests
- Architecture overview (link đến diagrams)

**5. Inline Code Comments:**
- Giải thích "tại sao" — không phải "cái gì" (code đã nói rõ "cái gì")
- Complex business logic cần comment giải thích rule
- Workaround và known limitation

### Tiêu chuẩn đầu ra
- [ ] API Spec (Swagger) được update mỗi khi API thay đổi
- [ ] README luôn đúng và developer mới setup được trong < 30 phút
- [ ] ADR mới được tạo cho mọi quyết định kiến trúc trong quá trình dev
- [ ] Runbook cơ bản đã có trước Phase 6

---

## Step 4.8 — Sprint Demo & Retrospective

### Tại sao bước này quan trọng?
**Demo:** Nhận feedback sớm. Cost of change khi demo (cuối sprint) << Cost of change khi UAT (Phase 5). "Tôi muốn khác" khi xem prototype = 1 giờ fix. "Tôi muốn khác" khi xem code đã xong = 2 ngày.

**Retrospective:** Team cải thiện cách làm việc liên tục. Sau 10 sprints với retrospective tốt, team làm việc hiệu quả hơn đáng kể so với team không có.

### Sprint Demo Best Practices
- Demo **trên staging**, không phải localhost
- Chỉ demo những gì đạt **Level 1 DoD** (từ Phase 2)
- **Cho stakeholders tự thao tác** nếu có thể — họ phát hiện issue nhanh hơn
- Ghi lại feedback thành **action items cụ thể** với owner

### Sprint Retrospective (3 câu hỏi)
1. **What went well?** — Giữ lại, nhân rộng
2. **What could be improved?** — Thay đổi gì cụ thể?
3. **Action items** — Ai làm gì, trước khi nào?

### Tiêu chuẩn đầu ra
- [ ] Sprint Demo notes với feedback
- [ ] Retrospective action items có owner và deadline
- [ ] Sprint velocity ghi lại
- [ ] Backlog cập nhật theo feedback từ demo

---

## Checklist Tổng Kết Phase 4

- [ ] Tất cả P0 features implement và pass unit + integration test
- [ ] Coverage targets đạt
- [ ] Technical documentation: API Spec, README, Runbook cơ bản
- [ ] Không có critical/high bug đã biết mà chưa fix
- [ ] CI/CD pipeline hoạt động ổn định
- [ ] Tất cả sprint demos đã được acknowledge

## Dấu Hiệu Cần Cẩn Thận

⚠️ **Dev nhận task từ khách hàng trực tiếp** — vi phạm Change Management Process, PM phải address ngay
⚠️ **"Done" mà không có test** — khi refactor sẽ không biết mình break gì
⚠️ **PR chờ review > 1 ngày** — blocking progress và tạo merge conflict
⚠️ **Không demo định kỳ** — surprise lớn khi UAT, khách hàng thấy gì đó hoàn toàn khác kỳ vọng
⚠️ **Documentation để sau** — "sau" = không bao giờ, vì luôn có sprint tiếp theo cần làm

---

*← [Phase 3: Design](03-design.md) | [SDLC Overview](../SDLC-Overview.md) | [Phase 5: Testing & QA →](05-testing-qa.md)*
