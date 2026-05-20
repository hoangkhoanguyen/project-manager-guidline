# Phase 6: Deployment

> **Mục tiêu tổng quát:** Đưa sản phẩm lên production một cách an toàn, có kiểm soát, với khả năng rollback nhanh nếu có sự cố — và đảm bảo mọi người liên quan (team, khách hàng, user) sẵn sàng sử dụng sản phẩm mới.

**Đầu vào:** UAT sign-off, Zero P0/P1 bugs, Go/No-go decision
**Đầu ra:** Sản phẩm live trên production, monitoring active, release notes gửi, knowledge transfer hoàn tất

---

## RACI Phase 6

| Hoạt động | PM/PO | Tech Lead | Dev | QA | DevOps | Client |
|-----------|-------|-----------|-----|----|--------|--------|
| Deployment Planning | **A/R** | R | C | C | R | C |
| Staging Deploy & Smoke Test | I | **A** | R | **R** | **R** | I |
| Go-Live Checklist | **A/R** | R | C | R | R | C |
| Production Deploy | I | **A** | R | I | **R** | I |
| Post-Deploy Monitoring | I | **A** | C | C | **R** | I |
| Release Communication | **A/R** | I | I | I | I | **R** |
| Knowledge Transfer | **A/R** | R | R | R | R | **A/R** |

---

## Step 6.1 — Deployment Planning

### Mục tiêu
Lên kế hoạch chi tiết cho việc release: khi nào release, theo chiến lược nào, ai làm gì, và nếu có sự cố thì xử lý thế nào. Không bao giờ "xem tới đâu hay tới đó" với production.

### Focus vào
- Release window: khi nào ít người dùng nhất (thường cuối tuần, đêm khuya)
- Deployment strategy: chọn strategy phù hợp với rủi ro và resource
- Rollback plan: cụ thể bao nhiêu phút sau deploy nếu có vấn đề thì rollback
- Communication plan: thông báo downtime/maintenance cho users
- War room: ai cần có mặt/on-call trong giờ deploy

### Deployment Strategies

| Strategy | Mô tả | Khi dùng | Rủi ro |
|----------|-------|----------|--------|
| **Big Bang** | Down hệ thống, deploy toàn bộ, restart | Nhỏ, ít user, có thể chấp nhận downtime | Cao |
| **Rolling** | Deploy dần dần từng instance, không downtime | Stateless services | Trung bình |
| **Blue/Green** | Giữ 2 environment, switch traffic | Khi cần zero-downtime và rollback nhanh | Thấp (tốn resource) |
| **Canary** | Release cho % nhỏ users trước, tăng dần | Thay đổi lớn, cần validate với real traffic | Thấp (phức tạp hơn) |
| **Feature Flag** | Code đã deploy nhưng feature tắt, bật dần | Kiểm soát release độc lập với deploy | Thấp (cần infra hỗ trợ) |

### Deployment Checklist Template
```
PRE-DEPLOY:
□ UAT sign-off đã có
□ Release notes đã viết
□ Database migration script đã test trên staging
□ Rollback plan đã document
□ On-call team đã được notify
□ Maintenance page/banner đã chuẩn bị (nếu có downtime)
□ Monitoring alerts đã được kiểm tra
□ Backup database gần nhất đã được verify

DEPLOY:
□ Announce vào channel nội bộ: "Bắt đầu deploy v1.x"
□ Run database migration
□ Deploy code
□ Smoke test ngay sau deploy
□ Monitor 15-30 phút đầu

POST-DEPLOY:
□ Announce: "Deploy hoàn tất, đang theo dõi"
□ Send release notes
□ Monitor 24h đầu sau go-live
```

### Tiêu chuẩn đầu ra
- [ ] **Deployment Plan** document: strategy, timeline, team roles
- [ ] **Rollback Plan** document: trigger conditions, steps, estimated time
- [ ] **Go/No-go Meeting** đã tổ chức với PM, lead engineer, QA lead
- [ ] Tất cả checklist items pre-deploy đã pass

---

## Step 6.2 — Staging Deployment & Smoke Test

### Mục tiêu
Deploy phiên bản release candidate lên staging environment (giống production nhất có thể) và chạy smoke test để xác nhận mọi thứ hoạt động trước khi lên production thật.

### Focus vào
- Staging phải mirror production về config, data structure, và scale
- Smoke test: không phải full regression, chỉ test các chức năng cốt lõi
- Database migration phải chạy clean trên staging trước
- Test end-to-end với data thực tế (anonymized)

### Smoke Test Checklist (ví dụ)
- [ ] Ứng dụng mở được, không có error 5xx
- [ ] Login/logout hoạt động
- [ ] Core user journey hoàn thành được (ví dụ: đặt hàng, thanh toán)
- [ ] Các integrations chính hoạt động (payment, email, notification)
- [ ] Database migration chạy thành công, không có data corruption
- [ ] API health check endpoints return healthy
- [ ] Logs hiển thị bình thường, không có unexpected errors

### Tiêu chuẩn đầu ra
- [ ] Staging deploy thành công, không có blockers
- [ ] Smoke test pass 100%
- [ ] Database migration verified trên staging
- [ ] QA và PM đã xác nhận staging ready

---

## Step 6.3 — Go-Live Checklist

### Mục tiêu
Kiểm tra lần cuối toàn bộ điều kiện cần thiết trước khi bấm nút deploy production. Đây là safety gate quan trọng nhất.

### Go-Live Checklist Đầy Đủ

**Code & Quality:**
- [ ] Release branch đã freeze (không có commit mới sau UAT)
- [ ] CI pipeline pass (build, test, lint, security scan)
- [ ] No P0/P1 open bugs
- [ ] UAT sign-off document đã có

**Infrastructure:**
- [ ] Production infrastructure đã provision đủ
- [ ] SSL certificate valid và chưa gần hết hạn
- [ ] DNS configuration đúng
- [ ] CDN configured (nếu dùng)
- [ ] Auto-scaling configured (nếu cần)
- [ ] Backup và recovery procedure đã verify

**Database:**
- [ ] Database backup gần nhất đã có và verify restore được
- [ ] Migration scripts đã test trên staging
- [ ] Connection pool sizing phù hợp

**Security:**
- [ ] Production secrets đã được set (không dùng staging secrets)
- [ ] Security headers configured (HTTPS, CSP, HSTS...)
- [ ] Rate limiting configured
- [ ] Firewall rules đúng

**Monitoring & Alerting:**
- [ ] Application monitoring active (APM)
- [ ] Error tracking active (Sentry, Datadog...)
- [ ] Infrastructure monitoring active
- [ ] Alerting rules configured với đúng thresholds
- [ ] On-call rotation đã setup và ai đó đang on-call

**Communication:**
- [ ] Internal team đã biết về release
- [ ] Customer support đã được brief về tính năng mới
- [ ] Maintenance window đã thông báo (nếu có downtime)
- [ ] Release notes đã viết và ready to send

### Tiêu chuẩn đầu ra
- [ ] 100% Go-Live Checklist items đã pass (không skip bất kỳ item nào)
- [ ] Go/No-go decision đã được PM và tech lead đồng ký
- [ ] Rollback plan đã sẵn sàng và team đã đọc qua

---

## Step 6.4 — Production Deployment

### Mục tiêu
Thực hiện deploy production theo đúng plan đã chuẩn bị, có sự giám sát chặt chẽ ngay từ đầu.

### Focus vào
- Follow đúng deployment plan, không improvise
- Communication realtime trong war room channel
- Monitor ngay sau từng bước, không đợi đến cuối
- Quyết định rollback phải nhanh nếu thấy dấu hiệu xấu

### Quy trình Deploy (ví dụ Blue/Green)
```
1. [T-0h] Announce bắt đầu deploy → war room channel
2. [T-0h] Backup database
3. [T-0h] Deploy code lên Green environment
4. [T-0h] Run database migration
5. [T+5m] Smoke test trên Green environment
6. [T+10m] Switch 10% traffic sang Green (Canary) → monitor 10 phút
7. [T+20m] Switch 100% traffic sang Green → monitor 15 phút
8. [T+35m] Announce hoàn tất → gửi release notes
9. [T+1h] Monitor tiếp, giữ Blue environment standby thêm 24h
```

### Rollback Triggers (ngay lập tức rollback nếu)
- Error rate tăng > 5% so với baseline
- Response time tăng > 2x so với baseline
- Critical functionality không hoạt động
- Data corruption detected
- Revenue-impacting bug

### Tiêu chuẩn đầu ra
- [ ] Deploy hoàn thành không có rollback
- [ ] Smoke test pass trên production
- [ ] Không có P0 incidents trong 30 phút đầu sau deploy
- [ ] War room channel đã được update realtime

---

## Step 6.5 — Post-Deploy Monitoring

### Mục tiêu
Theo dõi sát hệ thống trong giai đoạn "golden hour" (và 24h đầu) sau deploy — khoảng thời gian có khả năng phát sinh vấn đề cao nhất.

### Focus vào
- Error rate: có tăng đột biến sau deploy không?
- Response time: có chậm hơn đáng kể không?
- Resource usage: CPU, memory, DB connections có spike không?
- Business metrics: conversion rate, transaction volume có bình thường không?
- User reports: có support tickets tăng đột biến không?

### Monitoring Timeline
| Thời điểm | Hành động |
|-----------|-----------|
| 0-30 phút | Theo dõi mỗi 2-5 phút, toàn team sẵn sàng |
| 30 phút - 4 giờ | Theo dõi mỗi 15 phút |
| 4-24 giờ | Monitoring tự động + alert, team on-call |
| 24-72 giờ | Monitoring bình thường + daily review |

### Key Metrics cần watch
```
Application:
- Error rate (4xx, 5xx) → Alert nếu > baseline + 2%
- P95/P99 response time → Alert nếu tăng > 50%
- Request throughput → Alert nếu drop > 20%

Infrastructure:
- CPU usage → Alert nếu > 80% sustained
- Memory usage → Alert nếu > 85%
- Database connection pool → Alert nếu > 80% utilized
- Disk usage → Alert nếu > 80%

Business:
- Conversion rate
- Revenue/transaction volume
- Active users
```

### Tiêu chuẩn đầu ra
- [ ] Monitoring dashboard active và team đang theo dõi
- [ ] Không có P0/P1 incidents trong 24h đầu (hoặc đã được xử lý nhanh)
- [ ] Monitoring report sau 24h: mọi metric trong ngưỡng bình thường

---

## Step 6.6 — Release Communication

### Mục tiêu
Thông báo về release cho đúng đối tượng, với đúng thông tin, vào đúng thời điểm. Giúp stakeholders và users biết những gì thay đổi và tận dụng được tính năng mới.

### Đối tượng và nội dung thông báo
| Đối tượng | Kênh | Nội dung |
|-----------|------|---------|
| Internal team | Slack/Teams channel | Chi tiết kỹ thuật, known issues, on-call info |
| Khách hàng/Sponsor | Email | Release highlights, tính năng mới, link release notes |
| End users | In-app notification / Email | Tính năng mới theo ngôn ngữ của user, cách dùng |
| Customer Support | Internal briefing | FAQ về tính năng mới, cách xử lý complaint |

### Release Notes Template
```markdown
# Release v1.x.0 — [Ngày]

## Highlights (tóm tắt 2-3 dòng)

## Tính năng mới
- [Feature 1]: Mô tả ngắn gọn, benefit cho user
- [Feature 2]: ...

## Cải thiện
- [Improvement 1]

## Bug fixes
- [Fix 1]

## Known issues
- [Known issue nếu có, và workaround]

## Breaking changes (nếu có)
- [Thay đổi có thể ảnh hưởng tới user/API consumer]
```

### Tiêu chuẩn đầu ra
- [ ] Release notes đã được gửi đến tất cả stakeholders
- [ ] Internal team đã được brief
- [ ] Customer support đã có đủ thông tin để handle user questions
- [ ] Release được document trong changelog

---

## Step 6.7 — Knowledge Transfer

### Tại sao bước này quan trọng?
Đây là bước **hay bị bỏ quên nhất** trong toàn bộ SDLC. Team dev ship xong rồi move on, nhưng nếu không transfer knowledge:
- Support team không biết tính năng mới hoạt động như thế nào → user call in → không ai trả lời được
- Operations team không biết cách monitor/maintain → incident kéo dài
- User không biết tính năng mới có → adoption thấp → ROI thấp
- 6 tháng sau không ai nhớ tại sao một quyết định kỹ thuật được thực hiện như vậy

### Focus vào
- **Ai cần được transfer?** User, support team, ops team, dev team tương lai (qua docs)
- **Format phù hợp**: User guide (business language), Technical runbook (kỹ thuật), Training session (interactive)
- **Timing**: Knowledge Transfer **trước** go-live hoặc ngay sau — không phải 2 tuần sau khi đã forget

### Các hoạt động

**A. Cho End Users:**
- [ ] Viết **User Guide** — hướng dẫn sử dụng tính năng mới bằng ngôn ngữ business (không technical)
- [ ] Tổ chức **training session** nếu tính năng phức tạp (có thể online)
- [ ] Tạo **FAQ** cho các câu hỏi thường gặp
- [ ] In-app tooltips hoặc onboarding flow nếu phù hợp

**B. Cho Customer Support / Help Desk:**
- [ ] **Support Briefing** — demo tính năng mới, giải thích cách hoạt động
- [ ] **Escalation Guide** — loại issue nào tự xử lý được, loại nào cần escalate lên dev
- [ ] **Known Issues List** — những gì đã biết là chưa hoàn hảo, workaround là gì

**C. Cho Operations / DevOps:**
- [ ] **Runbook** cập nhật — cách deploy, cách rollback, cách xử lý common issues
- [ ] **Monitoring Guide** — metric nào watch, threshold nào alert, alert này nghĩa là gì
- [ ] **Architecture Overview** — hệ thống hoạt động thế nào, để khi có incident biết đào ở đâu

**D. Cho Dev Team (tương lai):**
- [ ] **ADRs** cập nhật — quyết định kỹ thuật nào được đưa ra và tại sao
- [ ] **README** chính xác — setup, architecture, how to contribute
- [ ] **Codebase walkthrough** nếu có dev mới join

### User Guide Template (tối giản)
```markdown
# Hướng Dẫn: [Tên Tính Năng]

## Tính năng này dùng để làm gì?
[1-2 câu, ngôn ngữ business]

## Ai có thể dùng?
[Role nào, điều kiện gì]

## Cách sử dụng
### Bước 1: [Tên bước]
[Screenshot + mô tả]

### Bước 2: [Tên bước]
[Screenshot + mô tả]

## Câu Hỏi Thường Gặp
**Q: [Câu hỏi]**
A: [Trả lời]

## Cần hỗ trợ?
[Liên hệ ai, kênh nào]
```

### Tiêu chuẩn đầu ra
- [ ] User Guide / Training materials đã có và accessible
- [ ] Support team đã được brief trước go-live
- [ ] Operations runbook đã cập nhật
- [ ] Knowledge base / help center đã update (nếu có)
- [ ] Training session đã diễn ra (nếu tính năng phức tạp)

---

## Checklist Tổng Kết Phase 6

- [ ] Go/No-go meeting đã tổ chức và quyết định Go
- [ ] Staging deploy và smoke test pass
- [ ] 100% Go-Live Checklist đã pass
- [ ] Production deploy thành công
- [ ] Không có P0 incidents trong 24h đầu
- [ ] Release notes đã gửi đến tất cả stakeholders
- [ ] Knowledge Transfer hoàn tất: user guide, support briefing, ops runbook
- [ ] Monitoring active và stable

## Dấu Hiệu Cần Cẩn Thận

⚠️ **Deploy vào thứ Sáu chiều** — không có team đầy đủ xử lý incident cuối tuần
⚠️ **Không có rollback plan** — nếu có vấn đề sẽ panic fix thay vì rollback có kiểm soát
⚠️ **Skip Knowledge Transfer vì vội** — support team overwhelmed sau go-live, adoption thấp
⚠️ **Deploy xong rồi ai cũng về** — ai sẽ monitor và xử lý incident trong 24h đầu?
⚠️ **User guide không có screenshot** — user đọc mà không hiểu thực hành như thế nào

---

*← [Phase 5: Testing & QA](05-testing-qa.md) | [SDLC Overview](../SDLC-Overview.md) | [Phase 7: Maintenance →](07-maintenance-support.md)*
