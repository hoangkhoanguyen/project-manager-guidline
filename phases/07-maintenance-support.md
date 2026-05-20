# Phase 7: Maintenance & Production Support

> **Mục tiêu tổng quát:** Duy trì sản phẩm ổn định sau launch, xử lý nhanh chóng các sự cố production, cải thiện liên tục chất lượng hệ thống, và cung cấp cho user/khách hàng thông tin họ cần để tự tin sử dụng sản phẩm. Đây là giai đoạn **dài nhất** trong vòng đời sản phẩm.

**Đầu vào:** Sản phẩm đang chạy live, Runbook từ Phase 6, Monitoring đã setup
**Đầu ra:** SLA được đảm bảo và báo cáo, hệ thống ổn định, postmortem sau sự cố, Knowledge Base cho users

---

## RACI Phase 7

| Hoạt động | PM/PO | Tech Lead | Dev | QA | DevOps | Client |
|-----------|-------|-----------|-----|----|--------|--------|
| Monitoring & Alerting | I | **A** | C | I | **R** | I |
| Incident Management | **A/R** | **R** | R | I | R | I |
| Bug Triage | **A/R** | C | R | **R** | I | C |
| Hotfix Process | **A** | **R** | R | R | R | I |
| Postmortem | **A/R** | R | R | R | R | I |
| SLA Reporting | **A/R** | C | I | I | C | **R** (nhận báo cáo) |
| Knowledge Base | **A** | C | C | C | I | **R** (nhận tài liệu) |
| Routine Maintenance | I | **A** | R | I | **R** | I |

---

## Khái Niệm Quan Trọng

### SLA / SLO / SLI
| Khái niệm | Định nghĩa | Ví dụ |
|-----------|------------|-------|
| **SLI** (Service Level Indicator) | Metric đo lường thực tế | "Availability hiện tại là 99.95%" |
| **SLO** (Service Level Objective) | Mục tiêu nội bộ muốn đạt | "Phải duy trì availability ≥ 99.9%" |
| **SLA** (Service Level Agreement) | Cam kết với khách hàng, có penalty | "Cam kết 99.5% uptime, vi phạm → bồi thường" |

### Error Budget
- Nếu SLO là 99.9% availability → cho phép down tối đa **43.8 phút/tháng**
- Error budget giúp team cân bằng giữa feature development và reliability
- Khi gần hết error budget → dừng feature, tập trung vào reliability

---

## Step 7.1 — Monitoring & Alerting

### Mục tiêu
Biết ngay khi có vấn đề xảy ra trên production — trước cả khi user báo cáo. "Phải là hệ thống báo cho bạn, không phải khách hàng báo cho bạn."

### Focus vào
- Chủ động phát hiện (proactive) hơn là phản ứng (reactive)
- Alert phải actionable — không phải cứ có threshold là alert, phải có hành động rõ ràng
- Avoid alert fatigue: quá nhiều false positive làm người dùng bỏ qua alert
- Cover cả 4 golden signals: Latency, Traffic, Errors, Saturation

### 4 Golden Signals của Google SRE
| Signal | Mô tả | Ví dụ metric |
|--------|-------|-------------|
| **Latency** | Thời gian xử lý request | P95 response time |
| **Traffic** | Lượng request/load | Requests per second |
| **Errors** | Tỷ lệ lỗi | 5xx error rate |
| **Saturation** | Mức độ "đầy" của hệ thống | CPU %, Memory %, Queue depth |

### Alert Severity Levels
| Level | Điều kiện | Response Time | Hành động |
|-------|-----------|--------------|-----------|
| **P0 - Critical** | Hệ thống down hoàn toàn hoặc data loss | Ngay lập tức | PagerDuty/call trực tiếp, wake up if needed |
| **P1 - High** | Chức năng chính bị ảnh hưởng | 15 phút | Notify on-call engineer |
| **P2 - Medium** | Degraded performance, không mất chức năng | 1 giờ | Notify team channel |
| **P3 - Low** | Warning, threshold gần bị vượt | Next business day | Log ticket |

### Monitoring Stack (ví dụ)
```
Application Performance: Datadog / New Relic / Dynatrace
Error Tracking: Sentry / Rollbar
Infrastructure: CloudWatch / Prometheus + Grafana
Uptime: Pingdom / UptimeRobot / Better Uptime
Log Management: ELK Stack / Datadog Logs
```

### Tiêu chuẩn đầu ra
- [ ] Dashboard monitoring với tất cả key metrics visible
- [ ] Alert rules đã được calibrate (không quá nhiều noise)
- [ ] On-call rotation setup, ai đang on-call biết mình cần làm gì
- [ ] Runbook cho từng alert: alert này nghĩa là gì, check gì, làm gì

---

## Step 7.2 — Incident Management

### Mục tiêu
Xử lý sự cố production một cách có hệ thống, nhanh chóng và có phối hợp. Giảm MTTR (Mean Time To Resolve) và đảm bảo communication rõ ràng với stakeholders trong suốt incident.

### Focus vào
- Restore service trước — root cause analysis sau
- Communicate thường xuyên, dù chưa có đáp án
- Không blame cá nhân trong lúc incident
- Document đầy đủ timeline để phân tích sau

### Quy Trình Xử Lý Incident

```
1. DETECT (phát hiện)
   → Alert từ monitoring hoặc user báo cáo

2. ACKNOWLEDGE (xác nhận)
   → Engineer on-call confirm đã nhận alert (trong SLA time)
   → Announce vào war room channel: "Đang điều tra issue X"

3. TRIAGE (đánh giá mức độ)
   → Xác định severity: P0/P1/P2?
   → Notify đúng người theo severity (lead, manager, C-level nếu P0)

4. INVESTIGATE (điều tra)
   → Check recent deployments
   → Check monitoring dashboards
   → Check logs
   → Form hypothesis và test

5. MITIGATE (giảm thiểu ảnh hưởng)
   → Rollback nếu có deploy gần đây gây ra
   → Feature flag off nếu dùng feature flag
   → Scale up nếu vấn đề về resource
   → Fail over sang backup nếu cần

6. RESOLVE (giải quyết)
   → Verify service restored
   → Announce resolved vào war room và status page

7. FOLLOW UP (xử lý hậu kỳ)
   → Viết postmortem trong 48 giờ (xem Step 7.5)
   → Track action items từ postmortem
```

### Incident Communication Template
```
[INCIDENT] [Severity] [Time] - [Service] [Status: OPEN/RESOLVED]

Impact: [Ảnh hưởng gì đến users]
Start time: [Khi nào bắt đầu]
Current status: [Đang điều tra / Đã tìm thấy nguyên nhân / Đang fix]
Next update: [Sẽ update lúc nào]
Lead: [Ai đang handle]
```

### Tiêu chuẩn đầu ra
- [ ] Incident được acknowledge trong SLA time
- [ ] War room channel được update ít nhất mỗi 30 phút
- [ ] Incident timeline được ghi lại đầy đủ
- [ ] Status page cập nhật (nếu có)
- [ ] Postmortem scheduled ngay sau khi resolve

---

## Step 7.3 — Bug Triage Production

### Mục tiêu
Phân loại và ưu tiên hóa bug từ production một cách khách quan và nhất quán. Đảm bảo đúng bug được fix đúng thời điểm, không để P0 bị chôn vùi trong backlog.

### Nguồn Bug Production
- Monitoring alerts tự động
- User reports qua support
- Internal team phát hiện
- Automated error tracking (Sentry...)

### Bug Triage Process (2 lần/ngày với P0/P1; 1 lần/ngày với P2/P3)
1. **Thu thập**: gom tất cả bug reports mới từ mọi nguồn
2. **Verify**: confirm bug reproducible, thu thập thông tin còn thiếu
3. **Classify**: gán severity và priority
4. **Assign**: giao cho engineer phù hợp
5. **Track**: theo dõi đến khi resolved

### Bug Aging Policy
| Priority | Target Resolution Time |
|----------|----------------------|
| P0 | < 4 giờ |
| P1 | < 24 giờ |
| P2 | < 1 tuần |
| P3 | Backlog, schedule khi có thể |

### Tiêu chuẩn đầu ra
- [ ] Không có P0 bug open > 4 giờ
- [ ] Không có P1 bug open > 24 giờ
- [ ] Weekly bug report: số bug mới, closed, open, age distribution

---

## Step 7.4 — Hotfix Process

### Mục tiêu
Deploy fix cho bug production nghiêm trọng (P0/P1) nhanh nhất có thể, với rủi ro tối thiểu. Hotfix phải nhanh nhưng không được reckless.

### Focus vào
- Hotfix scope tối thiểu — chỉ fix đúng vấn đề, không thêm bất cứ thứ gì
- Test kỹ hơn bình thường vì không có time cho full regression
- Deploy nhanh nhưng phải có smoke test
- Merge lại vào develop/main ngay sau khi hotfix (không để diverge)

### Hotfix Workflow
```
1. Tạo hotfix branch từ main (production tag)
   → git checkout -b hotfix/BUG-XXX main

2. Implement fix (tối thiểu, focused)

3. Test nhanh:
   → Unit test pass
   → Manual test case cụ thể
   → Smoke test quan trọng nhất

4. Code review (tối thiểu 1 reviewer, ngay lập tức)

5. Deploy lên staging → Smoke test nhanh

6. Deploy production (thường deploy manual, không qua full pipeline)

7. Verify fix trên production

8. Merge hotfix vào cả main VÀ develop

9. Tag version: v1.0.1

10. Announce resolved + brief postmortem
```

### Hotfix vs. Normal Bug Fix
| Tiêu chí | Hotfix | Normal Fix |
|----------|--------|------------|
| Severity | P0/P1 production | P2/P3 hoặc không production |
| Timeline | Hours | Next sprint |
| Branch | Từ main | Từ develop |
| Testing | Fast, focused | Full test cycle |
| Deploy | Ngay sau review | Qua sprint cycle |

### Tiêu chuẩn đầu ra
- [ ] Hotfix deploy trong SLA time (P0: < 4h, P1: < 24h)
- [ ] Hotfix đã được review (dù nhanh)
- [ ] Smoke test pass trên production sau deploy
- [ ] Hotfix branch đã merge vào cả main và develop
- [ ] Version tagged

---

## Step 7.5 — Postmortem & Root Cause Analysis

### Mục tiêu
Học từ sự cố để không lặp lại. Tìm nguyên nhân gốc rễ thực sự (không phải chỉ nguyên nhân bề mặt) và implement action items để ngăn chặn.

### Focus vào
- **Blameless**: tìm nguyên nhân hệ thống, không đổ lỗi cá nhân
- **Root cause**: dùng "5 Whys" để đào sâu đến nguyên nhân thực sự
- **Action items**: phải có owner và deadline cụ thể
- **Follow through**: review action items trong retrospective tiếp theo

### "5 Whys" Example
```
Vấn đề: Website down 45 phút

Why 1: Server crash
Why 2: Out of memory
Why 3: Memory leak trong service mới deploy
Why 4: Memory leak không được phát hiện trong testing
Why 5: Không có endurance/soak test trong CI pipeline

→ Root cause: Thiếu automated endurance testing
→ Action item: Thêm memory profiling test vào CI pipeline
```

### Postmortem Document Template
```markdown
# Postmortem: [Tên incident] — [Ngày]

## Severity & Duration
- Severity: P0/P1
- Start: [Time]
- End: [Time]  
- Duration: [X giờ Y phút]
- Services affected: [List]

## Impact
- Users affected: [số lượng hoặc %]
- Business impact: [doanh thu, transactions...]
- SLA impact: [vi phạm SLA không?]

## Timeline (chronological)
| Time | Event |
|------|-------|
| HH:MM | Alert triggered |
| HH:MM | Engineer acknowledged |
| HH:MM | Root cause identified |
| HH:MM | Fix deployed |
| HH:MM | Service restored |

## Root Cause
[Mô tả nguyên nhân gốc rễ, không phải triệu chứng]

## Contributing Factors
- [Factor 1]
- [Factor 2]

## What Went Well
- [Điều gì tốt trong quá trình xử lý]

## What Could Be Improved
- [Điều gì cần cải thiện]

## Action Items
| Action | Owner | Due Date | Status |
|--------|-------|----------|--------|
| [Hành động cụ thể] | [Tên] | [Ngày] | Open |

## Lessons Learned
[Key takeaways cho cả team]
```

### Tiêu chuẩn đầu ra
- [ ] Postmortem được viết trong 48 giờ sau incident
- [ ] Root cause được xác định (không chỉ dừng ở triệu chứng)
- [ ] Tất cả action items có owner và deadline
- [ ] Postmortem được chia sẻ với team và stakeholders liên quan
- [ ] Action items được theo dõi đến khi hoàn thành

---

## Step 7.6 — SLA Reporting

### Tại sao bước này quan trọng?
Đảm bảo SLA nội bộ là chưa đủ — khách hàng cần **bằng chứng** rằng cam kết được thực hiện. SLA Report định kỳ tạo **transparency** và **trust**. Nó cũng bảo vệ vendor khi khách hàng có complaint — có data thực tế để đối chiếu. Nếu SLA bị vi phạm, báo cáo chủ động và giải thích còn tốt hơn để khách hàng tự phát hiện.

### Focus vào
- Báo cáo phải **proactive** — gửi định kỳ, không chờ khách hàng hỏi
- Data phải **chính xác và có thể verify** được
- Khi vi phạm SLA: **acknowledge + giải thích + action taken** — không im lặng
- Ngôn ngữ **business**, không technical — khách hàng không cần biết chi tiết kỹ thuật

### Các Metrics cần báo cáo
| Metric | Đo lường thế nào | SLA Target (ví dụ) |
|--------|-----------------|-------------------|
| **Uptime / Availability** | % thời gian hệ thống available | ≥ 99.5%/tháng |
| **Incident Count** | Số P0/P1 incidents trong kỳ | ≤ 2 P1/tháng |
| **MTTR** (Mean Time To Resolve) | Trung bình thời gian resolve incident | ≤ 4h cho P1 |
| **Support Ticket Resolution** | % tickets giải quyết trong SLA time | ≥ 90% |
| **Planned Downtime** | Thông báo trước bao lâu, duration | ≥ 48h notice |

### SLA Report Template (Hàng Tháng)
```markdown
# Báo Cáo SLA — [Tháng/Năm]
**Sản phẩm:** [Tên]  **Gửi ngày:** [Date]  **Prepared by:** [PM]

## Tóm Tắt Tổng Quan
| Metric | Target | Thực tế | Status |
|--------|--------|---------|--------|
| Uptime | ≥ 99.5% | 99.87% | ✅ Đạt |
| P1 Incidents | ≤ 2 | 1 | ✅ Đạt |
| MTTR (P1) | ≤ 4h | 2.3h | ✅ Đạt |

## Sự Cố Trong Kỳ
| Ngày | Severity | Thời gian | Nguyên nhân | Đã giải quyết |
|------|----------|-----------|-------------|---------------|
| [Date] | P1 | 45 phút | [Brief description] | ✅ + Postmortem link |

## Planned Maintenance
[Liệt kê bảo trì có lịch trong tháng tới]

## Action Items từ Kỳ Trước
| Action | Status | Ghi chú |
|--------|--------|---------|
| [Action] | ✅ Done / 🔄 In Progress | |
```

### Tiêu chuẩn đầu ra
- [ ] SLA Report gửi cho khách hàng **đúng hạn** hàng tháng (hoặc hàng quý tùy contract)
- [ ] Tất cả incidents trong kỳ đã có explanation
- [ ] Khi vi phạm SLA: đã có written apology + root cause + prevention plan
- [ ] SLA performance trend visible (không chỉ snapshot 1 tháng)

---

## Step 7.7 — Knowledge Base & Self-Service

### Tại sao bước này quan trọng?
Mỗi câu hỏi support ticket là một lần developer hoặc PM phải dừng việc đang làm để trả lời. Nếu cùng câu hỏi được hỏi 20 lần, đó là dấu hiệu cần có Knowledge Base. KB tốt **giảm support load, tăng user satisfaction** (user tìm được đáp án ngay), và **scale** — không cần thêm người support khi user base tăng.

### Focus vào
- Viết từ góc nhìn **user, không phải developer**
- **Use case driven** — tổ chức theo "Tôi muốn làm gì" không phải theo tính năng
- **Selalu update** khi có tính năng mới hoặc thay đổi
- Gắn link KB vào UI (contextual help) khi có thể

### Nguồn Nội Dung cho Knowledge Base
1. **Support tickets phổ biến** — câu hỏi nào xuất hiện ≥ 3 lần → viết bài KB
2. **User Guide từ Phase 6** — mở rộng thành các bài viết chi tiết hơn
3. **FAQ từ UAT và training** — những điều user thắc mắc khi mới dùng
4. **Postmortem lessons** — nếu incident gây ra user confusion, viết bài giải thích

### Cấu Trúc Knowledge Base
```
📁 Getting Started
   └── Tạo tài khoản lần đầu
   └── Hướng dẫn nhanh (Quick Start)

📁 [Feature Chính 1]
   └── Cách thực hiện [Use case A]
   └── Cách thực hiện [Use case B]
   └── Xử lý lỗi thường gặp

📁 Tài Khoản & Cài Đặt
   └── Đổi mật khẩu
   └── Phân quyền người dùng

📁 Troubleshooting
   └── [Lỗi phổ biến 1]
   └── [Lỗi phổ biến 2]
```

### KB Article Template
```markdown
# [Tiêu đề mô tả hành động — "Cách xuất báo cáo Excel"]
**Cập nhật:** [Ngày]  **Áp dụng cho:** [Role/Version]

## Mục Đích
[1 câu: Bài này giúp bạn làm gì?]

## Các Bước Thực Hiện
1. [Bước 1 — có screenshot]
2. [Bước 2 — có screenshot]

## Lưu Ý Quan Trọng
- [Điều cần biết trước khi làm]

## Gặp Vấn Đề?
[Link đến support, hoặc các lỗi thường gặp và cách fix]
```

### Tiêu chuẩn đầu ra
- [ ] Knowledge Base có đầy đủ nội dung cho tất cả P0 features
- [ ] KB được update trong vòng 1 tuần sau mỗi release có tính năng mới
- [ ] Support ticket volume giảm theo thời gian (KB đang hoạt động hiệu quả)
- [ ] Link KB được embed trong sản phẩm (help center, tooltip...) nếu có thể

---

## Step 7.8 — Routine Maintenance

### Mục tiêu
Giữ hệ thống khỏe mạnh theo thời gian thông qua các hoạt động bảo trì định kỳ. Phòng bệnh hơn chữa bệnh.

### Focus vào
- Security patches: cập nhật thư viện có CVE kịp thời
- Performance: phát hiện và xử lý degradation trước khi trở thành incident
- Data hygiene: dọn dẹp data cũ, tối ưu storage
- Technical debt: trả dần debt để không tích lũy đến mức crisis

### Maintenance Calendar

**Hàng tuần:**
- [ ] Review monitoring dashboard và alerts
- [ ] Review open bugs và ưu tiên
- [ ] Check disk usage và resource utilization trends
- [ ] Review failed background jobs

**Hàng tháng:**
- [ ] Dependency audit: cập nhật thư viện có CVE
- [ ] Performance review: compare với baseline
- [ ] Cost optimization review (cloud costs)
- [ ] Backup restore test (verify backup thực sự restore được)
- [ ] Review và rotate secrets/certificates gần hết hạn

**Hàng quý:**
- [ ] Security audit
- [ ] Capacity planning review
- [ ] Technical debt review và prioritization
- [ ] Disaster recovery drill

### Tiêu chuẩn đầu ra
- [ ] Maintenance log được ghi lại đều đặn
- [ ] Không có critical CVE nào tồn tại > 7 ngày
- [ ] Backup restore test pass mỗi tháng
- [ ] Technical debt backlog được review hàng quý

---

## Checklist Tổng Kết Phase 7

- [ ] Monitoring và alerting hoạt động ổn định, calibrated (không quá nhiều noise)
- [ ] On-call rotation đã setup, không có single person dependency
- [ ] Runbooks cho các incident phổ biến đã có và được test
- [ ] Bug triage process chạy đều đặn (P0 < 4h, P1 < 24h)
- [ ] Postmortem đã viết cho tất cả P0/P1 incidents, action items được follow
- [ ] SLA Report gửi đúng hạn cho khách hàng
- [ ] Knowledge Base có đủ nội dung cho P0 features và đang được update
- [ ] Routine maintenance schedule đang được follow

## Dấu Hiệu Cần Cẩn Thận

⚠️ **Alert fatigue** — team bỏ qua alert vì quá nhiều noise: calibrate thresholds, chỉ alert khi có action cần thực hiện
⚠️ **Postmortem chỉ là hình thức** — action items không được follow up: track trong backlog như mọi task khác
⚠️ **"Chạy được thì không đụng"** — dependency cũ tích lũy CVE và kỹ thuật bẩn theo thời gian
⚠️ **Không test backup restore** — backup không được test = không có backup thực sự
⚠️ **On-call burnout** — cùng một người liên tục: rotation công bằng + compensate thỏa đáng
⚠️ **SLA Report trễ hoặc không gửi** — khách hàng mất trust, bắt đầu tự track và đặt câu hỏi
⚠️ **Knowledge Base lỗi thời** — tệ hơn không có KB vì user làm theo hướng dẫn cũ và bị lỗi

---

*← [Phase 6: Deployment](06-deployment.md) | [SDLC Overview](../SDLC-Overview.md) | [Phase 8: Enhancement →](08-enhancement-upgrade.md)*
