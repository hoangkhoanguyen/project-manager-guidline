# Phase 3: Design

> **Mục tiêu tổng quát:** Thiết kế toàn diện giải pháp **trước khi viết một dòng code** — từ kiến trúc hệ thống đến giao diện người dùng. Thay đổi ở giai đoạn này tốn 1 giờ. Thay đổi tương tự sau khi đã code tốn 3-5 ngày.

**Đầu vào:** Business Analysis Document, Scope Document, Technical Feasibility Report
**Đầu ra:** Architecture Document + ADRs, UI/UX Prototype approved, ERD, API Spec, Sequence Diagrams, Test Strategy

> **Thứ tự các bước:** Architecture → UI/UX → Database → API → Sequence Diagrams → Test Strategy → Security → Review
> *Lý do thứ tự này: Architecture đặt ra constraints tổng thể → UI/UX xác định dữ liệu cần hiển thị → Database design từ data requirements → API design từ UI/UX + Database → Sequence Diagrams document runtime flow → Test Strategy plan cho từ requirements → Security review toàn bộ.*

---

## RACI Phase 3

| Hoạt động | PM/PO | Tech Lead | Dev | QA | Designer | DevOps | Client |
|-----------|-------|-----------|-----|----|----------|--------|--------|
| Architecture Design | C | **A/R** | R | I | I | R | I |
| UI/UX Design | **A** | C | I | C | **R** | I | **A/C** |
| Database Design | C | **A/R** | R | I | I | I | I |
| API Design | C | **A** | **R** | C | C | I | I |
| Sequence Diagrams | C | **A/R** | R | C | I | I | I |
| Test Strategy | C | C | C | **A/R** | I | I | I |
| Security Design | C | **A/R** | R | R | I | R | I |
| Design Review | **A/R** | R | R | R | R | C | **A/C** |

---

## Step 3.1 — System Architecture Design

### Tại sao bước này quan trọng?
Architecture là quyết định **khó thay đổi nhất** và **tốn kém nhất** nếu sai. Chọn Monolith vs. Microservices, SQL vs. NoSQL, REST vs. GraphQL — tất cả những quyết định này sẽ ảnh hưởng đến mọi thứ sau đó: performance, scalability, team structure, deployment strategy. Làm đúng ở đây tiết kiệm hàng tháng work về sau.

### Focus vào
- Chọn architectural pattern **phù hợp với team và bài toán** — không phải trend (Microservices không phải lúc nào cũng tốt hơn Monolith)
- Xác định các service/module và **ranh giới giữa chúng** (separation of concerns)
- Authentication & Authorization flow (thiết kế chi tiết ở Step 3.7, nhưng define pattern ở đây)
- Caching, message queue, async processing — có cần không và ở đâu?
- Scalability và High Availability requirements

### Các hoạt động
- [ ] Vẽ **High-Level Architecture Diagram** — toàn cảnh hệ thống
- [ ] Vẽ **Component Diagram** — các module/service và quan hệ giữa chúng
- [ ] Vẽ **Deployment Diagram** — infrastructure, environments (dev/staging/prod)
- [ ] Viết **Architecture Decision Records (ADR)** cho mỗi quyết định quan trọng
- [ ] Architecture review với toàn team kỹ thuật

### Architecture Decision Record (ADR) Template
```markdown
## ADR-001: [Tên quyết định]
**Ngày:** | **Status:** Proposed / Accepted / Deprecated
**Người quyết định:** [Tech Lead + PM]

### Context
[Tại sao cần quyết định này? Bối cảnh là gì?]

### Decision
[Quyết định cụ thể là gì?]

### Rationale
[Tại sao chọn option này? Lý do kỹ thuật + business]

### Consequences
**Tốt:**
- [Hệ quả tốt]

**Không tốt / Trade-off:**
- [Hệ quả xấu hoặc trade-off phải chấp nhận]

### Alternatives Considered
| Option | Pros | Cons | Lý do không chọn |
|--------|------|------|-----------------|
| [Option A] | | | |
| [Option B] | | | |
```

### Tiêu chuẩn đầu ra
- [ ] **Architecture Diagram** (High-Level + Component + Deployment) — 3 layers
- [ ] **ADR list** — ít nhất 5 ADRs cho các quyết định quan trọng nhất
- [ ] Non-functional requirements về performance, availability, scalability đã được xác nhận
- [ ] Architecture được approve bởi Tech Lead và PM

---

## Step 3.2 — UI/UX Wireframe & Prototype

### Tại sao bước này quan trọng?
UI/UX **đứng trước** Database và API vì nó xác định **dữ liệu nào cần hiển thị và tương tác nào cần hỗ trợ** — từ đó mới biết cần lưu gì trong database và API cần trả về gì. Làm ngược lại (design database trước, rồi UI/UX sau) dẫn đến UI bị constrain bởi database schema, không phải bởi user needs.

Ngoài ra, prototype là cách **rẻ nhất để validate** với khách hàng. Sửa Figma mất 1 giờ. Sửa code đã implement mất 2 ngày.

### Focus vào
- **User flow** trước — người dùng đi qua màn hình nào để hoàn thành nhiệm vụ?
- Information hierarchy — thông tin quan trọng nhất phải nổi bật nhất
- Consistency — cùng loại action phải có cùng cách tương tác xuyên suốt
- **Tất cả states** — loading, empty state, error state, success state (không chỉ happy path)
- Mobile-first nếu cần (quyết định từ Phase 1 requirements)

### Các giai đoạn thiết kế (theo thứ tự)
```
1. User Flow Diagram → Người dùng đi qua màn hình nào?
         ↓
2. Wireframe (Low-fidelity) → Bố cục, không màu sắc
         ↓
3. Review với PM + Client → Feedback early
         ↓
4. High-fidelity Mockup → Màu, font, component chi tiết
         ↓
5. Clickable Prototype → Simulate thực tế
         ↓
6. Usability Test (3-5 users nếu có) → Validate trước khi handoff
```

### Tiêu chuẩn đầu ra
- [ ] **User Flow Diagrams** cho tất cả use cases P0
- [ ] **Wireframes** đã được PM và khách hàng review và approve
- [ ] **High-fidelity Mockups** trong Figma (hoặc equivalent)
- [ ] **Interactive Prototype** của core user journeys
- [ ] **Design System / Style Guide**: colors, typography, spacing, components
- [ ] **Screen Inventory**: danh sách tất cả màn hình và states (loading/empty/error/success)
- [ ] Khách hàng đã thao tác với prototype và approve

---

## Step 3.3 — Database Design

### Tại sao bước này quan trọng?
Database design **khó refactor** sau khi đã có data thật. Thêm column thì dễ, nhưng split một bảng lớn thành 2 bảng khi đã có 10 triệu rows là một migration nightmare. Nghĩ kỹ về data model từ đầu — đặc biệt là indexing strategy — tiết kiệm rất nhiều headache về performance sau này.

### Focus vào
- Chọn loại database phù hợp (SQL vs NoSQL vs hybrid) — **dựa trên data access patterns, không phải trend**
- Normalization đủ mức — không over-normalize (quá nhiều joins) hay under-normalize (data redundancy)
- Indexing strategy — nghĩ về **queries phổ biến nhất** trước khi thiết kế index
- PII (Personally Identifiable Information) — được lưu và bảo vệ thế nào?
- Migration strategy — schema thay đổi không down system như thế nào?

### Các hoạt động
- [ ] Vẽ **ERD (Entity Relationship Diagram)** đầy đủ
- [ ] Viết **Data Dictionary** — mô tả từng table, field, data type, constraints
- [ ] Xác định **indexing plan** cho top queries quan trọng nhất
- [ ] Thiết kế **soft-delete strategy** (xóa mềm vs xóa cứng)
- [ ] Plan **data archiving và retention policy**
- [ ] Thiết kế **migration strategy** — forward-only migrations, rollback plan

### Data Dictionary Template
```markdown
## Table: orders

| Column | Data Type | Nullable | Default | Description |
|--------|-----------|----------|---------|-------------|
| id | UUID | NOT NULL | gen_random_uuid() | Primary key |
| user_id | UUID | NOT NULL | — | FK → users.id |
| status | ENUM('pending','processing','completed','cancelled') | NOT NULL | 'pending' | Trạng thái đơn hàng |
| total_amount | DECIMAL(15,2) | NOT NULL | 0 | Tổng tiền (VND) |
| created_at | TIMESTAMPTZ | NOT NULL | NOW() | Thời điểm tạo |
| deleted_at | TIMESTAMPTZ | NULL | NULL | Soft delete timestamp |

**Indexes:**
- PRIMARY KEY (id)
- INDEX idx_orders_user_id (user_id) — cho query lấy orders của user
- INDEX idx_orders_status_created (status, created_at) — cho admin dashboard

**Business Rules:**
- total_amount không được âm
- Khi status = 'cancelled', phải có cancelled_reason
```

### Tiêu chuẩn đầu ra
- [ ] **ERD** đầy đủ, có thể hiểu toàn bộ data model từ diagram
- [ ] **Data Dictionary** cho tất cả tables
- [ ] Naming convention database đã thống nhất
- [ ] Indexing plan cho top 10 queries quan trọng nhất
- [ ] Data migration + seeding plan

---

## Step 3.4 — API Design

### Tại sao bước này quan trọng?
API là **contract** giữa Frontend và Backend (và giữa các services). Nếu không có API spec trước khi code, FE và BE làm song song theo assumption của mỗi người — rồi khi integrate sẽ không khớp và phải refactor nhiều. API spec cho phép FE dùng mock server để làm song song với BE — tăng tốc đáng kể.

### Focus vào
- **Consistency** — naming convention, error format nhất quán xuyên suốt
- **Versioning** — v1, v2... để thay đổi không break existing clients
- **Contract-first** — define spec trước, cả FE và BE implement theo spec
- Authentication/Authorization rõ ràng cho từng endpoint
- **Pagination** cho list endpoints — không bao giờ trả toàn bộ data

### Các hoạt động
- [ ] Liệt kê tất cả endpoints từ use cases đã phân tích
- [ ] Viết **API Specification** (OpenAPI/Swagger format)
- [ ] Định nghĩa **error codes và format** thống nhất
- [ ] Review API spec với cả FE và BE team
- [ ] Setup **Mock Server hoặc Postman Collection** để FE làm song song

### Chuẩn Error Response
```json
{
  "error": {
    "code": "ORDER_NOT_FOUND",
    "message": "Order with ID abc-123 was not found",
    "details": [
      {
        "field": "orderId",
        "message": "Must be a valid UUID"
      }
    ],
    "requestId": "req-xyz-789",
    "timestamp": "2026-01-01T10:00:00Z"
  }
}
```

### API Spec Template
```yaml
# GET /api/v1/orders/{orderId}
summary: Lấy thông tin đơn hàng
authentication: Bearer JWT token (required)
authorization: Chủ đơn hàng hoặc role ADMIN

parameters:
  - name: orderId
    in: path
    required: true
    type: string (UUID)

responses:
  200:
    description: Thành công
    body:
      id: string
      status: enum [pending, processing, completed, cancelled]
      items: array of OrderItem
      totalAmount: number
      createdAt: ISO8601 string

  404:
    description: Đơn hàng không tồn tại
    body: Error response standard

  403:
    description: Không có quyền xem đơn hàng này
    body: Error response standard

  401:
    description: Chưa đăng nhập
    body: Error response standard
```

### Tiêu chuẩn đầu ra
- [ ] **API Specification** đầy đủ (Swagger/OpenAPI hoặc tương đương)
- [ ] Error code list và response format thống nhất
- [ ] API đã được review và approve bởi FE Lead + BE Lead
- [ ] Mock server hoặc Postman collection sẵn sàng cho FE

---

## Step 3.5 — Sequence Diagrams & Flow Documentation

### Tại sao bước này quan trọng?
Architecture diagram và ERD mô tả **cấu trúc tĩnh** — các thành phần là gì. Sequence Diagram mô tả **runtime behavior** — các thành phần tương tác với nhau như thế nào theo thứ tự thời gian. Đây là tài liệu cực kỳ quan trọng cho các flow phức tạp: thanh toán, authentication, background jobs, webhook. Không có Sequence Diagram, dev sẽ implement dựa trên assumption, dẫn đến race conditions và edge cases bị bỏ sót.

### Khi nào cần vẽ Sequence Diagram
- Flows liên quan đến **nhiều hơn 2 services/actors**
- Flows có **trạng thái phức tạp** (payment, order lifecycle)
- Flows có **async processing** (message queue, webhook, background job)
- Authentication flows
- **Critical business flows** mà lỗi ở đây = business impact cao

### Sequence Diagram Example (Payment Flow)
```
User        FE App      BE API      Payment GW    DB
 │           │            │             │           │
 │ Checkout  │            │             │           │
 │──────────>│            │             │           │
 │           │ POST /pay  │             │           │
 │           │───────────>│             │           │
 │           │            │ Create order│           │
 │           │            │─────────────────────────>
 │           │            │ order_id ←──────────────│
 │           │            │ Initiate payment        │
 │           │            │────────────>│           │
 │           │            │ payment_url │           │
 │           │            │<────────────│           │
 │           │ redirect   │             │           │
 │<──────────│            │             │           │
 │ [User pays on GW]      │             │           │
 │           │            │ Webhook     │           │
 │           │            │<────────────│           │
 │           │            │ Update status           │
 │           │            │─────────────────────────>
 │           │            │ Email confirmation      │
 │           │            │────────> [Email Service]│
```

### Các hoạt động
- [ ] Identify tất cả complex flows cần Sequence Diagram
- [ ] Vẽ Sequence Diagram cho **tất cả P0 flows** có nhiều bên tham gia
- [ ] Đặc biệt chú ý: **error handling** trong sequence — điều gì xảy ra nếu bước 3 fail?
- [ ] Review với cả FE, BE, và QA team

### Tiêu chuẩn đầu ra
- [ ] Sequence Diagrams cho tất cả complex flows (payment, auth, data sync...)
- [ ] Mỗi diagram có cả **happy path và error paths**
- [ ] Diagrams được review bởi FE + BE + QA
- [ ] Stored cùng codebase hoặc design documents (không phải chỉ trong đầu người design)

---

## Step 3.6 — Test Strategy

### Tại sao bước này quan trọng?
Test Strategy được viết **ở Phase Design, không phải Phase Testing**, vì một lý do quan trọng: **thiết kế phải testable**. Nếu QA chỉ tham gia khi code xong, họ sẽ thấy rất nhiều thứ không thể test tự động được vì architecture không hỗ trợ. Bằng cách define Test Strategy sớm, team design hệ thống với testability in mind từ đầu.

### Nội dung Test Strategy
```markdown
# Test Strategy: [Tên Dự Án]

## Scope của Testing
Những gì SẼ test: [...]
Những gì KHÔNG test (và lý do): [...]

## Test Levels & Ownership
| Level | Mô tả | Tool | Ownership | Target Coverage |
|-------|-------|------|-----------|----------------|
| Unit Test | Test từng function/class | Jest, pytest | Developer | ≥80% business logic |
| Integration Test | Test service-to-service | Supertest, Postman | Developer | Tất cả API endpoints |
| E2E Test | Test full user journey | Cypress, Playwright | QA | Top 5 user flows |
| Performance Test | Load/stress test | k6, JMeter | DevOps + QA | Trước mỗi release |
| Security Test | OWASP, CVE scan | OWASP ZAP, Snyk | DevOps + QA | Trước mỗi release |

## Environments
| Environment | Mục đích | Data | Ai access |
|-------------|---------|------|----------|
| Dev (local) | Developer testing | Mock/seed data | Developers |
| Staging | Integration + QA + UAT | Anonymized prod data | All team + Client |
| Production | Live | Real data | End users |

## Regression Strategy
- Sau mỗi PR: Unit tests + Integration tests (tự động trong CI)
- Trước mỗi release: Full regression (E2E + manual test cho critical paths)
- Sau hotfix: Targeted regression + smoke test

## Test Automation Strategy
Phase 1 (MVP): Unit tests + API integration tests
Phase 2 (Post-launch): E2E automation cho top 5 flows
Phase 3 (Mature): Expand E2E coverage, performance test automation

## Bug Severity & SLA
[Reference Step 5.7 — định nghĩa severity và thời gian fix]

## Non-Functional Testing
- Performance benchmark: [Định nghĩa targets]
- Security: OWASP Top 10 check trước mỗi release
- Accessibility: WCAG 2.1 AA (nếu public-facing)
```

### Tiêu chuẩn đầu ra
- [ ] **Test Strategy Document** đã được PM, Tech Lead, QA Lead review và approve
- [ ] Test environments đã được plan và provisioned (hoặc scheduled)
- [ ] Test tool selection đã được confirm
- [ ] QA resources đã được allocated trong Resource Plan

---

## Step 3.7 — Security & Compliance Design

### Tại sao bước này quan trọng?
Security added **after the fact** là security that doesn't work. Retrofit authentication vào một hệ thống đã build tốn nhiều gấp 5-10 lần so với thiết kế từ đầu. Ngoài ra, nhiều security decisions ảnh hưởng đến architecture (ví dụ: cách lưu sensitive data, audit logging) — phải quyết định ở phase design.

### Focus vào
- Authentication: login mechanism, session management, token lifecycle
- Authorization: **RBAC** (Role-Based) hoặc **ABAC** (Attribute-Based)
- Data encryption: at rest và in transit (TLS minimum, encryption cho PII)
- Input validation và sanitization (XSS, SQL Injection, CSRF)
- Audit logging: ai làm gì, khi nào — **không thể thêm sau**
- Compliance: GDPR, PDPA, hoặc quy định ngành cụ thể

### Threat Modeling (STRIDE)
```
S — Spoofing (giả danh identity): Có thể giả làm user khác không?
T — Tampering (giả mạo data): Có thể sửa data trong transit không?
R — Repudiation (phủ nhận): Có thể chứng minh ai làm gì không?
I — Information Disclosure (lộ thông tin): Data nào có thể bị lộ?
D — Denial of Service: Hệ thống có thể bị làm quá tải không?
E — Elevation of Privilege (leo thang quyền hạn): User thường có thể làm được việc của admin không?
```

### Role & Permission Matrix Template
| Permission | Admin | Manager | Staff | Guest |
|-----------|-------|---------|-------|-------|
| Xem tất cả orders | ✅ | ✅ | ❌ | ❌ |
| Tạo order | ✅ | ✅ | ✅ | ✅ (own only) |
| Xóa order | ✅ | ❌ | ❌ | ❌ |
| Export report | ✅ | ✅ | ❌ | ❌ |
| Quản lý user | ✅ | ❌ | ❌ | ❌ |

### Tiêu chuẩn đầu ra
- [ ] **Security Design Document**: authentication flow, session management, encryption plan
- [ ] **Role & Permission Matrix** đầy đủ
- [ ] **Data Classification Map**: loại dữ liệu và cách bảo vệ tương ứng
- [ ] **Audit Log Plan**: những action nào log, log gì, lưu bao lâu
- [ ] Threat modeling cơ bản đã thực hiện (STRIDE hoặc tương đương)

---

## Step 3.8 — Design Review & Approval

### Tại sao bước này quan trọng?
Design Review là **cơ hội cuối để thay đổi với chi phí thấp nhất**. Một giờ review có thể phát hiện một inconsistency giữa API spec và UI design mà nếu bỏ qua sẽ tốn 2 ngày refactor sau này. Đây cũng là cơ hội để **cross-pollinate knowledge** — FE học về architecture, BE học về UX flow.

### Focus vào
- **Cross-review**: FE review API spec (họ sẽ phải dùng nó); BE review UI flow (họ cần hiểu data flow)
- PM kiểm tra: design có đáp ứng **đúng requirements** không?
- Phát hiện **inconsistencies** và **gaps** giữa các phần thiết kế
- Khách hàng xác nhận UI/UX prototype (cuộc gặp riêng, không chung với tech review)

### Checklist Design Review
```
Architecture Review (Tech team):
□ Architecture pattern phù hợp với scale yêu cầu
□ Single points of failure đã được identify và có plan
□ ADRs cho tất cả quyết định quan trọng
□ Security design không có gap rõ ràng

UI/UX Review (Client + PM + Designer):
□ Tất cả user stories P0 có màn hình tương ứng
□ Flow làm sense với khách hàng
□ Error states và empty states đã được thiết kế
□ Mobile/responsive đã được consider

API Review (FE + BE Lead):
□ Tất cả data FE cần đều có trong API response
□ Error codes đủ để FE hiển thị message phù hợp
□ Authentication rõ ràng cho từng endpoint

Database Review (BE Lead + Tech Lead):
□ Tất cả entities từ business analysis đã có table
□ Relationships đúng cardinality
□ No obvious performance bottleneck trong schema
```

### Tiêu chuẩn đầu ra
- [ ] Tất cả design documents đã được review và có **sign-off** từ đúng người
- [ ] Không còn open design questions P0
- [ ] Team đã có đủ thông tin để bắt đầu development
- [ ] Design documents được lưu vào **version-controlled repository**

---

## Checklist Tổng Kết Phase 3

- [ ] Architecture Diagram + ít nhất 5 ADRs
- [ ] UI/UX Prototype được khách hàng approve
- [ ] ERD và Data Dictionary hoàn chỉnh
- [ ] API Spec đầy đủ, đã review FE + BE
- [ ] Sequence Diagrams cho tất cả complex flows
- [ ] Test Strategy đã được QA Lead approve
- [ ] Security Design: Role Matrix, Data Classification, Audit Log Plan
- [ ] Tất cả design documents có sign-off

## Dấu Hiệu Cần Cẩn Thận

⚠️ **Thiếu Sequence Diagram cho payment/auth** — đây là nơi bugs tốn kém nhất ẩn náu
⚠️ **API design sau khi FE đã code** — sẽ phải refactor cả FE lẫn BE
⚠️ **UI chỉ có happy path** — error states và empty states phải thiết kế trước khi handoff
⚠️ **Security "để sau"** — không có "sau" nào đủ rẻ để retrofit security vào hệ thống đã build
⚠️ **Test Strategy không có automation plan** — manual regression không scale được

---

*← [Phase 2: Analysis & Planning](02-analysis-planning.md) | [SDLC Overview](../SDLC-Overview.md) | [Phase 4: Development →](04-development.md)*
