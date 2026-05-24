# 06 — Scale Adjustment: Hệ Thống Lớn vs App Nhỏ

> Cấu trúc PRD không đổi. Những gì thay đổi là độ sâu từng section và cách tổ chức tài liệu.

---

## Nguyên Tắc Cơ Bản

> **PRD ngắn không phải là PRD kém. Scope nhỏ thì PRD ngắn là đúng.**

Vấn đề không phải là nhiều trang hay ít trang — vấn đề là mỗi trang có đầy đủ thông tin cần thiết không.

---

## So Sánh: Hệ Thống Lớn vs App Nhỏ

| Section | Hệ thống lớn (e-commerce, ERP) | App nhỏ (internal tool, microfeature) |
|---------|-------------------------------|---------------------------------------|
| Executive Summary | 1 trang, đầy đủ business case | 3–5 câu |
| Problem Statement | Đầy đủ As-Is, data, business impact | 1 đoạn ngắn |
| Target Users | 3–5 persona, nhiều segment | 1–2 persona |
| Goals & Metrics | North Star + contributing metrics per module | 1–2 metrics |
| Scope | Phân chia module v1/v2, integration map | Gạch đầu dòng 5–10 items |
| User Flows | 4–6 flows per module | 1–3 flows |
| Functional Requirements | Hàng chục đến hàng trăm user stories | 5–15 user stories |
| Business Rules | File riêng per domain | Inline trong feature spec |
| Non-Functional | Nhiều tiêu chí, phân tầng | Chỉ những cái thực sự quan trọng |
| Tổ chức tài liệu | Folder structure đa tầng | 1 file duy nhất |

---

## Hệ Thống Lớn: 2-Tầng Tài Liệu

Khi hệ thống có nhiều module (≥3 domain) hoặc nhiều team, một file PRD không còn đủ.

### Kiến Trúc Tài Liệu 2 Tầng

```
Tầng 1: System PRD (tài liệu mẹ)
  → Ai đọc: C-level, all stakeholders, Tech Lead
  → Độ dài: 5–15 trang
  → Nội dung: big picture, goals, scope, constraints

Tầng 2: Feature Spec / Module PRD (tài liệu con)
  → Ai đọc: Dev team cụ thể, QA của module đó
  → Độ dài: 5–20 trang mỗi module
  → Nội dung: chi tiết implementation-ready
```

### System PRD (Tầng 1) Cần Có

- Executive Summary tổng thể
- Business goals ở cấp hệ thống (North Star Metric)
- Architecture Overview: hệ thống gồm những module nào, tích hợp với nhau thế nào
- Scope: Module nào trong v1, v2 — và tại sao
- Non-functional requirements ở cấp hệ thống (uptime SLA, security baseline, scalability)
- Stakeholder map
- Link đến Feature Spec của từng module

### Feature Spec (Tầng 2) Cho Mỗi Module

- Problem Statement cụ thể của module đó
- User Stories và AC đầy đủ
- Business Rules riêng của module
- Dependencies với module khác

**Ví dụ cấu trúc cho e-commerce system:**

```
System PRD (system-prd.md)
  ├── Feature Spec: User & Auth (user-auth/feature-spec.md)
  ├── Feature Spec: Product Catalog (products/feature-spec.md)
  ├── Feature Spec: Cart & Checkout (checkout/feature-spec.md)
  │   └── Business Rules (checkout/business-rules.md)  ← Pricing, tax, coupon rules
  ├── Feature Spec: Order Management (orders/feature-spec.md)
  ├── Feature Spec: Payment (payment/feature-spec.md)
  └── Feature Spec: Admin Dashboard (admin/feature-spec.md)
```

---

## App Nhỏ: Compress Từng Section

Với app nhỏ hoặc single feature, dùng 1 file PRD duy nhất nhưng rút ngắn từng section.

**Template compressed:**

```markdown
# PRD: [Tên Tính Năng]
Version 1.0 | Author | Date

## 1-liner Summary
[1 câu mô tả problem + giải pháp]

## Problem
[2–3 bullet points với data]

## Users
[1–2 persona, 2–3 câu mỗi người]

## Success Metrics
| Metric | Now | Target |
|--------|-----|--------|

## Scope (v1)
- [item 1]
- [item 2]
**Out of scope:** [item A], [item B]

## User Flows
[2–3 flows ngắn]

## Requirements
[5–15 user stories với AC]

## Business Rules
[Chỉ những rule không hiển nhiên]

## Non-Functional
[Chỉ những cái thực sự quan trọng]

## Open Questions
[Bảng open questions]
```

---

## Goals & Metrics Theo Scale

### Hệ thống lớn: North Star + Contributing Metrics

**North Star Metric** = một số duy nhất đại diện cho giá trị cốt lõi mà sản phẩm tạo ra cho user.

Ví dụ:
- E-commerce: GMV (Gross Merchandise Value)
- SaaS: Weekly Active Users
- Marketplace: Số giao dịch thành công/tháng

**Contributing Metrics** = metrics của từng module đóng góp vào North Star:

```
North Star: GMV tháng
  ├── Search module: Click-through rate từ search → PDP
  ├── Checkout module: Checkout conversion rate
  ├── Payment module: Payment success rate
  └── Marketing module: Coupon redemption rate
```

Mỗi module PRD đặt contributing metric của mình là primary metric.

### App nhỏ: 1–2 Metrics Đủ Dùng

Không cần framework phức tạp. Chỉ cần trả lời: "Sau 30/60/90 ngày, đo cái gì để biết tính năng này có hiệu quả?"

---

## Khi Nào Cần Tách Thành Multi-File?

Tách khi có ít nhất một trong các dấu hiệu sau:

- PRD đơn lẻ > 30 trang
- Có ≥ 3 domain/module riêng biệt
- Có ≥ 2 dev team làm song song
- Domain logic khác nhau đáng kể (ví dụ: checkout vs admin dashboard)
- Stakeholder khác nhau quan tâm đến module khác nhau

---

## Lưu Ý: Phân Biệt Feature Spec vs Tài Liệu Kỹ Thuật

Feature Spec (PRD tầng 2) vẫn là tài liệu của PM/BA — mô tả WHAT, không phải HOW.

Tài liệu kỹ thuật (Technical Design Document — TDD) là do Tech Lead viết ở Phase 3:
- Architecture diagram
- Database schema
- API contract
- Sequence diagram

**Hai tài liệu này tách biệt nhau.** Feature Spec xong trước, TDD viết sau dựa trên Feature Spec.

---

*Tiếp theo: [07-document-organization.md](./07-document-organization.md) — Tổ chức folder tài liệu và chia sẻ với khách hàng.*
