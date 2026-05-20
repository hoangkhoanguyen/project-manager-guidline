# Phase 5: Testing & QA

> **Mục tiêu tổng quát:** Đảm bảo sản phẩm hoạt động đúng, đủ, an toàn và đáp ứng kỳ vọng của người dùng trước khi ra production. Phase này **execute** Test Strategy đã được define ở Phase 3 — không phải lúc mới bắt đầu nghĩ về testing.

**Đầu vào:** Feature đã deploy staging, code pass CI, Test Strategy (từ Phase 3), Integration tests (từ Phase 4) pass
**Đầu ra:** Test Report, Regression Automation Suite, UAT Sign-off, Zero P0/P1 bug còn mở

---

## RACI Phase 5

| Hoạt động | PM/PO | Tech Lead | Dev | QA | Client |
|-----------|-------|-----------|-----|----|--------|
| Test Plan | **A** | C | C | **R** | I |
| Functional Testing | I | I | C | **A/R** | I |
| Integration Testing | I | C | **R** | **A/C** | I |
| Performance Testing | C | **A** | C | **R** | I |
| Security Testing | C | **A/R** | R | R | I |
| UAT | **A/R** | I | C | **R** | **A/R** |
| Bug Triage | **A/R** | C | R | **R** | C |
| Regression Automation | I | C | **R** | **A/R** | I |
| Go/No-go Decision | **A/R** | R | I | R | **A** |

---

## Nguyên Tắc "Shift Left Testing"

QA không phải chỉ tham gia ở Phase 5. Họ tham gia từ sớm:
- **Phase 1-2**: QA review requirements — tìm ambiguity, missing acceptance criteria, unstated requirements
- **Phase 3**: QA viết Test Strategy, bắt đầu viết test cases từ spec (trước khi code)
- **Phase 4**: QA review acceptance criteria trước sprint, verify stories trên staging trong sprint
- **Phase 5**: Full test execution cycle — chính thức và có hệ thống

> **Tại sao shift left?** Bug tìm thấy ở requirements phase tốn ~1 giờ fix. Tìm thấy ở QA phase tốn ~8 giờ. Tìm thấy ở production tốn ~1 ngày + reputational damage.

---

## Step 5.1 — Lập Test Plan & Test Cases

### Mục tiêu
Xác định phạm vi, chiến lược và tiêu chí test trước khi bắt đầu test. Đảm bảo không bỏ sót test case quan trọng và có cơ sở để báo cáo tiến độ testing.

### Focus vào
- Coverage: test case phải bao phủ tất cả acceptance criteria
- Viết test case từ user perspective, không từ technical perspective
- Bao gồm cả negative test cases (input sai, flow bất thường)
- Ưu tiên test P0 features trước

### Cấu trúc Test Plan
```
1. Scope: những gì sẽ test và không test
2. Test Strategy: các loại test sẽ thực hiện
3. Test Environment: môi trường test, data test
4. Entry/Exit Criteria: khi nào bắt đầu/kết thúc test
5. Test Schedule: timeline cho từng phase testing
6. Roles & Responsibilities: ai test gì
7. Risk & Mitigation
```

### Test Case Template
```
TC-001: [Tên test case]
Module: [Module/Feature]
Priority: P0 / P1 / P2
Precondition: [Điều kiện trước khi thực hiện]
Steps:
  1. [Bước 1]
  2. [Bước 2]
Expected Result: [Kết quả mong đợi]
Actual Result: [Điền khi test]
Status: Pass / Fail / Blocked
Notes: [Ghi chú thêm]
```

### Tiêu chuẩn đầu ra
- [ ] **Test Plan** được review và approve bởi QA lead và PM
- [ ] Test cases viết đầy đủ cho tất cả P0 và P1 features
- [ ] Test environment đã setup và data test đã chuẩn bị
- [ ] Entry criteria đã met: code deploy lên staging thành công, smoke test pass

---

## Step 5.2 — Functional Testing

### Mục tiêu
Kiểm thử toàn bộ chức năng của sản phẩm theo đúng requirements và acceptance criteria. Đây là layer testing quan trọng nhất — đảm bảo "sản phẩm làm đúng việc cần làm".

### Focus vào
- Từng use case theo đúng test case đã viết
- Happy path: luồng bình thường
- Negative path: input sai, không có quyền, timeout...
- Edge cases: giá trị biên, string rỗng, số âm, ký tự đặc biệt
- Cross-browser / cross-device (nếu là web/mobile)

### Phân loại test
| Loại | Mô tả | Ai làm |
|------|-------|--------|
| Smoke Test | Test cơ bản: app mở được, login được, core flow chạy được | QA (sau mỗi deploy) |
| Sanity Test | Verify nhanh area vừa fix bug | QA |
| Regression Test | Test lại toàn bộ để đảm bảo không có gì bị break | QA (trước release) |
| Exploratory Test | Test không có script, dựa vào kinh nghiệm và intuition | QA senior |

### Tiêu chuẩn đầu ra
- [ ] Tất cả test cases P0 đã được execute
- [ ] Test execution report với pass/fail rate
- [ ] Tất cả bugs được log vào bug tracker với đầy đủ thông tin
- [ ] Regression test suite đã được chạy và pass

---

## Step 5.3 — Integration Testing

### Mục tiêu
Kiểm thử tích hợp giữa các module nội bộ và các hệ thống bên thứ 3. Đảm bảo "các phần khi ghép lại với nhau vẫn hoạt động đúng".

### Focus vào
- API integration: request/response đúng format, error handling đúng
- Third-party services: payment gateway, SMS, email, cloud services
- Data flow giữa các module: data đúng khi đi từ A sang B
- Async flows: message queue, webhook, background jobs

### Các hoạt động
- [ ] Test tất cả API endpoints với Postman/similar tool
- [ ] Test integration với từng third-party service
- [ ] Test error scenarios: third-party service down, timeout, invalid response
- [ ] Test async flows: message được process đúng, retry logic hoạt động
- [ ] Contract testing nếu có nhiều services

### Tiêu chuẩn đầu ra
- [ ] Tất cả critical integrations đã được test
- [ ] Error handling với external service failures đã được verify
- [ ] Integration test results documented

---

## Step 5.4 — Performance Testing

### Mục tiêu
Đảm bảo hệ thống đáp ứng yêu cầu hiệu năng đã định nghĩa trong phase requirements. Phát hiện bottleneck trước khi production.

### Các loại Performance Test
| Loại | Mục đích | Khi nào |
|------|----------|---------|
| **Load Test** | Kiểm tra hiệu năng dưới tải bình thường và peak | Trước mọi release |
| **Stress Test** | Tìm điểm gãy của hệ thống | Với hệ thống critical |
| **Spike Test** | Kiểm tra phản ứng khi tải tăng đột ngột | Khi có event promotions |
| **Endurance Test** | Kiểm tra memory leak, resource exhaustion theo thời gian | Với long-running systems |

### Performance Benchmarks (ví dụ, tùy chỉnh theo project)
| Metric | Target |
|--------|--------|
| API response time (p95) | < 500ms |
| API response time (p99) | < 2000ms |
| Page load time | < 3s |
| Concurrent users | 1000 (hoặc số đã định nghĩa trong requirements) |
| Error rate dưới load | < 1% |

### Tiêu chuẩn đầu ra
- [ ] Load test report với kết quả vs benchmark đã định nghĩa
- [ ] Các bottleneck đã được xác định (nếu có)
- [ ] Performance fixes đã được implement và re-test (nếu cần)
- [ ] Kết quả đáp ứng hoặc vượt benchmark → Go/No-go decision

---

## Step 5.5 — Security Testing

### Mục tiêu
Phát hiện và vá các lỗ hổng bảo mật trước khi hacker tìm thấy chúng. Đảm bảo dữ liệu người dùng được bảo vệ.

### Các hoạt động
- [ ] **OWASP Top 10 review**: kiểm tra các lỗ hổng phổ biến nhất
  - Injection (SQL, XSS, Command injection)
  - Broken Authentication
  - Insecure Direct Object Reference
  - Security Misconfiguration
  - Sensitive Data Exposure
  - Missing Authorization checks
- [ ] **Dependency scanning**: check thư viện/package có CVE không
- [ ] **SAST (Static Application Security Testing)**: scan code tự động
- [ ] **Penetration testing** (nếu hệ thống high-security): thuê security expert

### Authentication & Authorization Test Cases
- [ ] Không thể access resource của user khác
- [ ] Token expired đúng theo config
- [ ] Brute force protection hoạt động
- [ ] Sensitive data không log ra
- [ ] HTTPS enforced, HTTP bị redirect

### Tiêu chuẩn đầu ra
- [ ] Security scan report
- [ ] Tất cả Critical và High severity vulnerabilities đã được fix
- [ ] Dependency CVEs đã được address (fix hoặc accepted risk với lý do)

---

## Step 5.6 — UAT (User Acceptance Testing)

### Mục tiêu
Khách hàng và/hoặc end users xác nhận rằng sản phẩm đáp ứng yêu cầu nghiệp vụ của họ trong điều kiện thực tế. Đây là checkpoint cuối cùng trước khi release.

### Focus vào
- User test trên môi trường staging với data gần giống production
- Test theo real-world scenarios, không phải test cases kỹ thuật
- Feedback về UX: có gì khó dùng, không trực quan?
- Business acceptance: "Sản phẩm này giải quyết được vấn đề của tôi không?"

### Quy trình UAT
1. **Chuẩn bị**: setup môi trường staging, data mẫu thực tế, UAT test scenarios
2. **Training**: brief user về những gì cần test và cách báo cáo issue
3. **Execution**: user thực hiện test, QA hỗ trợ (không hướng dẫn cách làm)
4. **Feedback Collection**: thu thập bug reports và UX feedback
5. **Fix & Re-test**: fix critical issues, user verify lại
6. **Sign-off**: thu thập chữ ký/email xác nhận từ khách hàng

### UAT Feedback Categories
- 🔴 **Blocker**: không thể accept, phải fix trước go-live
- 🟡 **Major**: nên fix trước go-live nếu có thể
- 🟢 **Minor**: có thể fix sau, đưa vào backlog
- 💡 **Enhancement**: ý tưởng tính năng mới, đưa vào product backlog

### Tiêu chuẩn đầu ra
- [ ] UAT completed với tất cả scenarios đã được test
- [ ] Tất cả Blocker issues đã được fix và verify
- [ ] **UAT Sign-off document** có chữ ký khách hàng
- [ ] Major issues được xác nhận scope: fix trước hay sau go-live?

---

## Step 5.7 — Bug Triage & Fix

### Mục tiêu
Phân loại và xử lý có hệ thống tất cả bugs được phát hiện. Đảm bảo không có bug nghiêm trọng nào bị bỏ qua và team tập trung vào đúng thứ quan trọng.

### Bug Severity vs Priority

**Severity** (mức độ nghiêm trọng về kỹ thuật):
| Level | Mô tả | Ví dụ |
|-------|-------|-------|
| **S1 - Critical** | Hệ thống không dùng được, data bị mất/hỏng | Không login được, data bị corrupt |
| **S2 - High** | Chức năng chính bị ảnh hưởng, không có workaround | Không checkout được |
| **S3 - Medium** | Chức năng bị ảnh hưởng nhưng có workaround | Filter không hoạt động nhưng scroll được |
| **S4 - Low** | Lỗi nhỏ, không ảnh hưởng chức năng | Typo, màu sắc sai |

**Priority** (mức độ ưu tiên fix, do PM quyết định kết hợp severity + business context):
- **P0**: Fix ngay lập tức (blocker)
- **P1**: Fix trong sprint hiện tại
- **P2**: Fix trong sprint tiếp theo
- **P3**: Backlog, fix khi có thời gian

### Bug Report Template
```
Bug ID: BUG-001
Title: [Mô tả ngắn gọn]
Severity: S1/S2/S3/S4
Priority: P0/P1/P2/P3
Environment: Staging / Production
Steps to Reproduce:
  1.
  2.
Expected Result:
Actual Result:
Attachments: [Screenshot, video, log]
Reported by: [Tên]
Date: [Ngày]
```

### Go/No-Go Criteria (điều kiện để go-live)
- [ ] Zero P0 bugs open
- [ ] Zero P1 bugs open (hoặc accepted với lý do rõ ràng)
- [ ] P2 bugs được PM và khách hàng acknowledge và đồng ý defer
- [ ] UAT sign-off đã có

### Tiêu chuẩn đầu ra
- [ ] Bug tracker cập nhật realtime
- [ ] Bug report cho từng release cycle
- [ ] Go/No-go decision được document với danh sách known issues

---

## Step 5.8 — Test Data Management

### Tại sao bước này quan trọng?
Test data kém là một trong những nguyên nhân lớn nhất khiến test kết quả không đáng tin. Test với data thiếu → miss bug. Test với data bị "ô nhiễm" từ run trước → kết quả không consistent. Khi data test trên staging giống production nhất có thể, confidence vào test results mới cao.

### Nguyên tắc quản lý test data
- **Isolation**: mỗi test run phải có data độc lập, không ảnh hưởng nhau
- **Reproducibility**: test phải cho kết quả giống nhau khi chạy lại
- **Realistic**: data phải đủ gần với production để cover edge cases thực tế
- **Privacy**: không dùng data thật của user — phải **anonymize** trước khi dùng làm test data

### Chiến lược Test Data
| Loại | Cách tạo | Khi dùng |
|------|---------|---------|
| **Seeded Data** | Script SQL/seed file được version control | Setup initial state cho test suite |
| **Factory/Fixture** | Code tạo data trước mỗi test | Unit/Integration test |
| **Anonymized Production** | Snapshot prod + anonymize PII | Staging environment, performance test |
| **Synthetic Data** | Tools như Faker | Load test, edge cases |

### Test Data Reset Strategy
```
Trước mỗi test run (E2E):
1. Restore database từ baseline snapshot
2. Run seed scripts
3. Execute tests
4. (Optional) Take post-test snapshot để debug nếu fail

Hoặc với transaction rollback (integration test):
- Wrap mỗi test trong transaction
- Rollback sau khi test xong
- Đảm bảo test isolation hoàn toàn
```

### Tiêu chuẩn đầu ra
- [ ] Seed scripts được version control cùng codebase
- [ ] Staging database có thể reset về trạng thái sạch trong < 5 phút
- [ ] Không có PII của user thật trong test data
- [ ] Test data đủ đa dạng để cover các edge cases quan trọng

---

## Step 5.9 — Regression Test Automation

### Tại sao bước này quan trọng?
Manual regression testing không scale được. Sprint 1: 20 test cases, 2 giờ. Sprint 10: 200 test cases, 2 ngày. Sprint 20: không còn thời gian để làm việc khác. Regression automation là đầu tư — tốn thời gian setup ban đầu, nhưng sau đó chạy trong < 30 phút không cần người.

### Phân tầng Automation (Test Pyramid)

```
         /\
        /E2E\ ← Ít nhất, chậm nhất, tốn kém nhất
       /------\   (5-10% test coverage)
      /Integr.\ ← Trung bình
     /----------\  (20-30% test coverage)
    /  Unit Test \ ← Nhiều nhất, nhanh nhất, rẻ nhất
   /--------------\  (60-70% test coverage)
```

**Nguyên tắc:** Càng lên cao = càng tốn kém để maintain. Đừng cố automate 100% UI test.

### Ưu tiên Automation theo thứ tự
1. **Regression-critical paths**: các flow quan trọng nhất hay bị break khi có change
2. **Happy paths của P0 features**: checkout, login, core business flow
3. **Flaky manual tests**: test hay cho kết quả inconsistent khi làm manual
4. **Long running manual tests**: test tốn > 30 phút làm manual

### Automation Roadmap Template
```
Sprint 1-2 (MVP): Unit + Integration tests đã có từ Phase 4
Sprint 3-4: E2E automation cho top 3 critical flows
Sprint 5-6: Expand E2E + Performance test automation
Post-launch: Maintenance + expand coverage dựa trên production bugs
```

### Tiêu chuẩn đầu ra
- [ ] E2E automation suite cho top 5 critical user flows
- [ ] Automation suite chạy trong CI (ít nhất hàng ngày)
- [ ] Kết quả automation visible cho toàn team (dashboard hoặc Slack notification)
- [ ] Automation maintenance guide — cách update khi UI thay đổi

---

## Checklist Tổng Kết Phase 5

- [ ] Test Plan đã được approve (reference Test Strategy từ Phase 3)
- [ ] Tất cả P0 test cases đã execute và pass
- [ ] Regression automation suite chạy và pass
- [ ] Performance benchmarks đạt targets đã định nghĩa
- [ ] Security scan không còn Critical/High CVEs
- [ ] Test data management đã thiết lập, staging có thể reset
- [ ] UAT sign-off từ khách hàng
- [ ] Zero P0/P1 bugs còn mở

## Dấu Hiệu Cần Cẩn Thận

⚠️ "QA test sau khi dev done" — quá muộn, QA phải tham gia từ phase requirements
⚠️ Bỏ qua performance test vì "ít user lúc đầu" — khi scale sẽ không kịp xử lý
⚠️ UAT chỉ là formality, khách hàng không thực sự test — risk cao nhất
⚠️ Test chỉ happy path — bugs thường nằm ở edge cases và error flows
⚠️ "Bug nhỏ, ship trước fix sau" — không có "sau" nào cả nếu không được track và commit

---

*← [Phase 4: Development](04-development.md) | [SDLC Overview](../SDLC-Overview.md) | [Phase 6: Deployment →](06-deployment.md)*
