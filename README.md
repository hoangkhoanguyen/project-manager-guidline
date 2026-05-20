# Quy Trình Phát Triển Sản Phẩm Phần Mềm (SDLC)

> Tài liệu này mô tả quy trình chuẩn từ khi tiếp nhận yêu cầu khách hàng (chưa rõ ràng) cho đến khi sản phẩm hoàn chỉnh, bao gồm cả giai đoạn hậu launch: bảo trì, xử lý lỗi production và nâng cấp.

---

## Tổng Quan Các Giai Đoạn

```
[Phase 1] Discovery & Requirements
        ↓
[Phase 2] Analysis & Planning
        ↓
[Phase 3] Design
        ↓
[Phase 4] Development  ←──────────────┐
        ↓                             │ (iteration)
[Phase 5] Testing & QA ───────────────┘
        ↓
[Phase 6] Deployment
        ↓
[Phase 7] Maintenance & Production Support
        ↓
[Phase 8] Enhancement & Upgrade ──────→ (quay lại Phase 2/3/4)
```

---

## Phase 1 — Discovery & Requirements

> 📄 Chi tiết: [01-discovery-requirements.md](phases/01-discovery-requirements.md)

**Mục tiêu:** Biến yêu cầu mơ hồ, chưa rõ ràng từ khách hàng thành một tập yêu cầu có thể làm việc được.

| Step | Tên                       | Mô tả                                                   |
| ---- | ------------------------- | ------------------------------------------------------- |
| 1.1  | Tiếp nhận yêu cầu ban đầu | Ghi nhận tất cả những gì khách hàng mô tả, dù còn mơ hồ |
| 1.2  | Stakeholder Mapping       | Xác định tất cả các bên liên quan và mức độ ảnh hưởng   |
| 1.3  | Discovery Interviews      | Phỏng vấn sâu để hiểu pain point, mong muốn thực sự     |
| 1.4  | Research & Benchmark      | Nghiên cứu thị trường, đối thủ, giải pháp hiện tại      |
| 1.5  | Tổng hợp & viết PRD sơ bộ | Gom tất cả insight thành tài liệu yêu cầu ban đầu       |
| 1.6  | Validate & Sign-off       | Xác nhận lại với khách hàng và chốt yêu cầu             |

**Đầu ra:** PRD sơ bộ (Product Requirements Document) được khách hàng ký duyệt.

---

## Phase 2 — Analysis & Planning

> 📄 Chi tiết: [02-analysis-planning.md](phases/02-analysis-planning.md)

**Mục tiêu:** Chuyển yêu cầu đã chốt thành kế hoạch thực thi cụ thể, đo lường được.

| Step | Tên                                        | Mô tả                                                                         |
| ---- | ------------------------------------------ | ----------------------------------------------------------------------------- |
| 2.0  | Project Charter                            | Văn bản chính thức khởi động dự án, xác định mục tiêu, scope, team, authority |
| 2.1  | Phân tích nghiệp vụ (Business Analysis)    | Vẽ luồng nghiệp vụ, use case, business rule                                   |
| 2.2  | Xác định Scope & In/Out of Scope           | Chốt rõ cái gì làm, cái gì không làm trong phiên bản này                      |
| 2.3  | Phân tích kỹ thuật (Technical Feasibility) | Đánh giá khả năng kỹ thuật, stack, tích hợp bên thứ 3                         |
| 2.4  | Phân tích rủi ro (Risk Assessment)         | Nhận diện rủi ro và lập kế hoạch xử lý                                        |
| 2.5  | Ước lượng & Resource Planning              | Estimate effort, phân công team, xác định deadline                            |
| 2.6  | Communication Plan                         | Kế hoạch họp, báo cáo định kỳ, escalation path                                |
| 2.7  | Definition of Done                         | Định nghĩa tiêu chí "done" 3 cấp: Task, Sprint, Release                       |
| 2.8  | Lập Roadmap & Milestone                    | Chia giai đoạn, đặt checkpoint rõ ràng                                        |

**Đầu ra:** Project Charter (signed), Project Plan, Roadmap, Risk Register, Resource Allocation, Communication Plan.

---

## Phase 3 — Design

> 📄 Chi tiết: [03-design.md](phases/03-design.md)

**Mục tiêu:** Thiết kế toàn bộ giải pháp trước khi code — từ kiến trúc hệ thống đến giao diện người dùng.

| Step | Tên                          | Mô tả                                                                                                    |
| ---- | ---------------------------- | -------------------------------------------------------------------------------------------------------- |
| 3.1  | System Architecture Design   | Thiết kế kiến trúc tổng thể, các thành phần và quan hệ; viết ADR                                         |
| 3.2  | UI/UX Wireframe & Prototype  | Thiết kế luồng người dùng, wireframe, mockup, prototype — làm trước DB/API để xác định data cần hiển thị |
| 3.3  | Database Design              | Thiết kế schema, ERD, data model, data dictionary                                                        |
| 3.4  | API Design                   | Định nghĩa contract API (REST/GraphQL/gRPC), error response standard                                     |
| 3.5  | Sequence Diagrams            | Vẽ luồng runtime cho các flow phức tạp (auth, payment, async)                                            |
| 3.6  | Test Strategy                | Định nghĩa chiến lược test toàn dự án: phạm vi, tool, automation roadmap, bug SLA                        |
| 3.7  | Security & Compliance Design | STRIDE threat modeling, role/permission matrix, compliance requirements                                  |
| 3.8  | Design Review & Approval     | Cross-review toàn bộ thiết kế với team và khách hàng                                                     |

**Đầu ra:** Architecture Document (với ADRs), ERD + Data Dictionary, API Spec, Sequence Diagrams, Figma/Prototype được duyệt, Test Strategy document.

---

## Phase 4 — Development

> 📄 Chi tiết: [04-development.md](phases/04-development.md)

**Mục tiêu:** Xây dựng sản phẩm đúng thiết kế, đúng chất lượng, theo từng sprint có thể kiểm soát.

| Step | Tên                             | Mô tả                                                           |
| ---- | ------------------------------- | --------------------------------------------------------------- |
| 4.1  | Setup môi trường & chuẩn hóa    | Cài đặt dev environment, CI/CD, coding standards                |
| 4.2  | Sprint Planning                 | Lên kế hoạch sprint, phân task, đặt sprint goal                 |
| 4.3  | Implementation                  | Lập trình theo task, áp dụng coding convention                  |
| 4.4  | Code Review                     | Peer review đảm bảo chất lượng và knowledge sharing             |
| 4.5  | Unit Testing                    | Dev tự viết và chạy unit test                                   |
| 4.6  | Integration Testing (Dev level) | Dev test API endpoints, DB integration, third-party connections |
| 4.7  | Technical Documentation         | Viết API docs, ADR updates, runbook, README trong khi code      |
| 4.8  | Sprint Demo & Retrospective     | Demo kết quả sprint, cải tiến quy trình                         |

**Đầu ra:** Source code được review, unit test đạt coverage tối thiểu, integration tests pass, technical docs cập nhật, demo được chấp nhận.

---

## Phase 5 — Testing & QA

> 📄 Chi tiết: [05-testing-qa.md](phases/05-testing-qa.md)

**Mục tiêu:** Đảm bảo sản phẩm hoạt động đúng, đủ, an toàn trước khi ra production.

| Step | Tên                           | Mô tả                                                                                |
| ---- | ----------------------------- | ------------------------------------------------------------------------------------ |
| 5.1  | Lập Test Plan & Test Cases    | Xác định phạm vi test, viết test case từ yêu cầu (dựa trên Test Strategy từ Phase 3) |
| 5.2  | Functional Testing            | Kiểm thử chức năng theo từng use case                                                |
| 5.3  | Integration Testing           | Kiểm thử tích hợp giữa các module và API bên thứ 3                                   |
| 5.4  | Performance Testing           | Kiểm tra hiệu năng, load test, stress test                                           |
| 5.5  | Security Testing              | Rà soát bảo mật, vulnerability scan                                                  |
| 5.6  | UAT (User Acceptance Testing) | Khách hàng/end user test thực tế                                                     |
| 5.7  | Bug Triage & Fix              | Phân loại, ưu tiên và sửa bug                                                        |
| 5.8  | Test Data Management          | Quản lý seeded data, factory/fixture, data reset strategy                            |
| 5.9  | Regression Test Automation    | Xây dựng automation theo Test Pyramid, phân loại P0/P1 test                          |

**Đầu ra:** Test Report, UAT Sign-off, Zero P0/P1 bug còn mở, Automation test suite baseline.

---

## Phase 6 — Deployment

> 📄 Chi tiết: [06-deployment.md](phases/06-deployment.md)

**Mục tiêu:** Đưa sản phẩm lên production an toàn, có kiểm soát và khả năng rollback.

| Step | Tên                             | Mô tả                                                                              |
| ---- | ------------------------------- | ---------------------------------------------------------------------------------- |
| 6.1  | Deployment Planning             | Lên kế hoạch release: thời gian, chiến lược (Blue/Green, Canary...), rollback plan |
| 6.2  | Staging Deployment & Smoke Test | Deploy lên staging, chạy smoke test xác nhận                                       |
| 6.3  | Go-Live Checklist               | Kiểm tra toàn bộ checklist trước khi bấm nút production                            |
| 6.4  | Production Deployment           | Triển khai lên production theo chiến lược đã chọn                                  |
| 6.5  | Post-Deploy Monitoring          | Theo dõi sát sau khi deploy: error rate, performance                               |
| 6.6  | Release Communication           | Thông báo tới stakeholders, viết release notes                                     |
| 6.7  | Knowledge Transfer              | Đào tạo end users, support team, ops team, và dev team                             |

**Đầu ra:** Sản phẩm live trên production, monitoring dashboard hoạt động, release notes, training materials.

---

## Phase 7 — Maintenance & Production Support

> 📄 Chi tiết: [07-maintenance-support.md](phases/07-maintenance-support.md)

**Mục tiêu:** Duy trì sản phẩm ổn định, xử lý nhanh các sự cố, cải thiện liên tục sau launch.

| Step | Tên                              | Mô tả                                                      |
| ---- | -------------------------------- | ---------------------------------------------------------- |
| 7.1  | Monitoring & Alerting            | Theo dõi hệ thống 24/7, thiết lập cảnh báo tự động         |
| 7.2  | Incident Management              | Quy trình xử lý sự cố khi có alert hoặc báo cáo            |
| 7.3  | Bug Triage Production            | Phân loại và ưu tiên bug từ production                     |
| 7.4  | Hotfix Process                   | Quy trình phát triển và deploy hotfix khẩn cấp             |
| 7.5  | Postmortem & Root Cause Analysis | Phân tích nguyên nhân gốc rễ sau sự cố (5 Whys)            |
| 7.6  | SLA Reporting                    | Báo cáo SLA định kỳ cho khách hàng: uptime, incident, MTTR |
| 7.7  | Knowledge Base & Self-Service    | Xây dựng và duy trì KB để giảm support tickets lặp lại     |
| 7.8  | Routine Maintenance              | Cập nhật dependency, dọn dẹp data, tối ưu định kỳ          |

**Đầu ra:** SLA được đảm bảo, Postmortem document, Monthly SLA Report, Knowledge Base cập nhật, hệ thống ổn định.

---

## Phase 8 — Enhancement & Upgrade

> 📄 Chi tiết: [08-enhancement-upgrade.md](phases/08-enhancement-upgrade.md)

**Mục tiêu:** Phát triển sản phẩm liên tục dựa trên feedback thực tế và chiến lược kinh doanh.

| Step | Tên                            | Mô tả                                                                         |
| ---- | ------------------------------ | ----------------------------------------------------------------------------- |
| 8.1  | Feedback Collection & Analysis | Thu thập feedback từ user, data, support tickets, interviews                  |
| 8.2  | Feature Request Backlog        | Gom tất cả yêu cầu mới vào backlog có cấu trúc                                |
| 8.3  | Prioritization                 | Ưu tiên hóa theo RICE, Kano, Opportunity Score                                |
| 8.4  | Version & Release Planning     | Lên kế hoạch version mới theo Now/Next/Later                                  |
| 8.4b | Product Analytics Setup        | Định nghĩa metrics cần track, event tracking plan, feature adoption dashboard |
| 8.4c | A/B Testing Strategy           | Thiết kế và chạy controlled experiments trước khi rollout toàn bộ             |
| 8.5  | Technical Upgrade Planning     | Lên kế hoạch nâng cấp kỹ thuật: dependency, infra, refactor                   |
| 8.6  | Quay lại chu kỳ                | Khởi động lại từ đúng phase phù hợp với scope                                 |

**Đầu ra:** Prioritized Backlog, Release Plan, A/B Test results, Metrics Dashboard, quay lại vòng lặp phát triển.

---

## Quy Trình Xuyên Suốt (Cross-Cutting Processes)

Ngoài 8 phases chính, có một số quy trình chạy song song xuyên suốt dự án:

| Quy trình             | Mô tả                                                                                                        | Tài liệu                                               |
| --------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------ |
| **Change Management** | Quy trình tiếp nhận, đánh giá impact, approve/reject và implement yêu cầu thay đổi scope sau khi đã sign-off | [change-management.md](processes/change-management.md) |

> ⚠️ **Mọi thay đổi scope** sau khi PRD đã được ký duyệt (Phase 1) hoặc khi sprint đang chạy (Phase 4) đều **phải đi qua Change Management Process** — không được accept miệng, không được add vào sprint hiện tại mà không có impact assessment.

---

## Nguyên Tắc Xuyên Suốt

| Nguyên tắc                | Áp dụng                                                                            |
| ------------------------- | ---------------------------------------------------------------------------------- |
| **Documentation First**   | Mọi quyết định đều phải có tài liệu, dù ngắn                                       |
| **Definition of Done**    | Mỗi task/feature chỉ "done" khi đáp ứng tiêu chí đã định nghĩa trước (Phase 2.7)   |
| **Shift Left Testing**    | Test Strategy được viết ở Phase 3, test bắt đầu từ Phase 4 — không chờ đến Phase 5 |
| **Fail Fast**             | Phát hiện và xử lý vấn đề ở giai đoạn sớm nhất có thể                              |
| **Stakeholder Alignment** | Luôn có sign-off rõ ràng ở các milestone quan trọng                                |
| **Blameless Culture**     | Khi có sự cố, tìm nguyên nhân gốc rễ (5 Whys), không đổ lỗi cá nhân                |
| **Data-Driven Decisions** | Mọi thay đổi sản phẩm đều cần evidence — analytics, user research, hoặc A/B test   |
| **RACI Clarity**          | Mỗi phase và mỗi hoạt động đều có Responsible và Accountable rõ ràng               |

---

_Phiên bản: 1.1 | Cập nhật: 2026-05-20_
