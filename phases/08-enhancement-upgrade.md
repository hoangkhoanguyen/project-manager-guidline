# Phase 8: Enhancement & Upgrade

> **Mục tiêu tổng quát:** Phát triển sản phẩm liên tục dựa trên feedback thực tế, data-driven decisions, và chiến lược kinh doanh. Mỗi vòng lặp phải tạo ra **giá trị đo lường được**, không phải chỉ thêm tính năng vì stakeholder muốn.

**Đầu vào:** Sản phẩm đang chạy production, feedback từ users, product analytics, business strategy
**Đầu ra:** Prioritized backlog, Release Plan, A/B Test results, khởi động chu kỳ phát triển mới

---

## RACI Phase 8

| Hoạt động | PM/PO | Tech Lead | Dev | QA | Designer | Client |
|-----------|-------|-----------|-----|----|----------|--------|
| Feedback Collection | **A/R** | I | I | I | I | **R** |
| Product Analytics | **A/R** | C | C | I | I | I |
| Feature Request Backlog | **A/R** | C | I | I | I | C |
| Prioritization | **A/R** | C | C | C | C | **C** |
| A/B Testing | **A/R** | R | R | R | R | I |
| Version Planning | **A/R** | R | C | C | C | **A/C** |
| Tech Upgrade Planning | C | **A/R** | R | I | I | I |

---

## Tư Duy Đúng Cho Giai Đoạn Này

> "Features are not outcomes. Adding more features ≠ more value."

Trước khi thêm bất cứ tính năng nào, phải trả lời được:
- **Who**: Ai sẽ dùng tính năng này? Bao nhiêu user?
- **Problem**: Vấn đề gì nó giải quyết? Vấn đề này có được **validate bởi data** không?
- **Metric**: Chúng ta sẽ đo thành công bằng gì? Số cụ thể là gì?
- **Evidence**: Có data, interview, hoặc support tickets nào chứng minh đây là pain point thực sự?
- **Opportunity cost**: Nếu làm tính năng này, tính năng nào sẽ không được làm?

---

## Step 8.1 — Feedback Collection & Analysis

### Mục tiêu
Thu thập và tổng hợp feedback từ nhiều nguồn khác nhau để có cái nhìn đa chiều, trung thực về những gì đang hoạt động tốt và những gì cần cải thiện.

### Focus vào
- Không chỉ nghe tiếng kêu to nhất — stakeholder lớn nhất chưa chắc đại diện cho đa số user
- Kết hợp quantitative (data) và qualitative (interviews, feedback)
- Distinguish giữa "what users say" vs "what users do" — behavioral data không nói dối
- Categorize feedback: bug, UX improvement, new feature, performance

### Nguồn Feedback

| Nguồn | Loại insight | Tần suất |
|-------|-------------|----------|
| Analytics (user behavior, funnel, retention) | Quantitative | Liên tục |
| Support tickets / help desk | Pain points thực tế | Hàng tuần |
| NPS / CSAT surveys | Overall satisfaction | Hàng tháng |
| User interviews | Deep qualitative insights | Hàng quý |
| Session recordings (Hotjar, FullStory) | Behavior observation | Liên tục |
| App store reviews / reviews | Sentiment | Hàng tuần |
| Sales / CS team feedback | Prospect/customer pain | Hàng tuần |
| Internal team observations | Operational insights | Sprint review |

### Phân Tích Funnel & Retention
- **Conversion funnel**: người dùng drop off ở đâu? Tại sao?
- **Retention curve**: người dùng quay lại như thế nào? Tỷ lệ churn?
- **Feature adoption**: tính năng nào được dùng nhiều/ít?
- **Cohort analysis**: nhóm user join tháng X hoạt động như thế nào?

### Tiêu chuẩn đầu ra
- [ ] Feedback synthesis report: tổng hợp từ tất cả nguồn trong kỳ review
- [ ] Top 10 pain points được user đề cập nhiều nhất
- [ ] Feature adoption heatmap: tính năng nào đang được dùng
- [ ] Retention và funnel data đã được phân tích

---

## Step 8.2 — Feature Request Backlog

### Mục tiêu
Tổng hợp tất cả yêu cầu tính năng mới vào một backlog có cấu trúc, đủ thông tin để prioritize và estimate sau này.

### Focus vào
- Capture "why" chứ không chỉ "what" — hiểu bài toán, không phải giải pháp
- Nhóm request tương tự lại — nhiều user cùng request một bài toán là signal mạnh
- Không reject ngay request nào — capture trước, prioritize sau
- Link request về evidence (bao nhiêu user, ticket nào, data nào)

### Feature Request Template
```markdown
## [FR-XXX] Tên tính năng

**Requested by:** [User segment / stakeholder]
**Frequency:** [Số lần được request / user count]
**Problem statement:** Người dùng đang gặp vấn đề gì?
**Proposed solution:** Họ đề xuất giải quyết như thế nào?
**Business impact:** Ảnh hưởng đến metric nào nếu làm?
**Evidence:** [Link to tickets, interviews, analytics]
**Priority score:** [Điền sau khi prioritize]
**Status:** New / Under review / Planned / In development / Done / Rejected
```

### Tiêu chuẩn đầu ra
- [ ] Tất cả feedback đã được convert thành feature requests hoặc bug tickets
- [ ] Mỗi feature request có đủ thông tin (problem, evidence, business impact)
- [ ] Backlog được accessible cho PM, tech lead, và stakeholders
- [ ] Duplicate requests đã được merge lại

---

## Step 8.3 — Prioritization

### Mục tiêu
Quyết định item nào được làm tiếp theo dựa trên impact, effort, và chiến lược. Vì resource luôn có giới hạn, đây là quyết định quan trọng nhất của PM.

### Framework Prioritization

**1. RICE Score**
```
RICE = (Reach × Impact × Confidence) / Effort

Reach:      Bao nhiêu user được ảnh hưởng trong một kỳ? (số người)
Impact:     Mức độ tác động: 3=Massive, 2=High, 1=Medium, 0.5=Low, 0.25=Minimal
Confidence: Độ chắc chắn của estimate: 100%=High, 80%=Medium, 50%=Low
Effort:     Số person-months cần thiết
```

**2. Opportunity Scoring**
```
Priority = Importance to user - Satisfaction with current solution
→ Điểm cao = nhu cầu quan trọng nhưng chưa được đáp ứng = cơ hội lớn
```

**3. Kano Model**
| Loại | Mô tả | Ví dụ |
|------|-------|-------|
| **Must-be** | Thiếu thì user bỏ đi, có thì không hài lòng hơn | Bug fixes |
| **Performance** | Càng nhiều càng tốt, ảnh hưởng tuyến tính | Tốc độ |
| **Delighters** | User không expect nhưng khi có thì rất thích | Surprise features |

**4. Eisenhower Matrix (cho tech debt & maintenance)**
```
           Urgent | Not Urgent
Important | Do Now | Schedule
Not Important | Delegate | Eliminate
```

### Cân Nhắc Khi Prioritize
- **Customer commitment**: đã hứa với khách hàng cụ thể chưa?
- **Strategic alignment**: có aligned với OKRs / strategy của công ty không?
- **Dependencies**: có feature nào phải làm trước feature này không?
- **Tech debt threshold**: nếu tech debt quá cao, phải xen kẽ vào

### Tiêu chuẩn đầu ra
- [ ] Prioritized backlog với score rõ ràng
- [ ] Top 10 items có business case đầy đủ
- [ ] Stakeholders key đã review và đồng ý với priority order
- [ ] "Won't do" list với lý do rõ ràng (không chỉ là không làm)

---

## Step 8.4 — Version & Release Planning

### Mục tiêu
Xác định scope của version tiếp theo và lên kế hoạch release cụ thể. Tạo sự rõ ràng cho cả team về "sprint tới chúng ta hướng tới cái gì".

### Focus vào
- Release có theme/narrative rõ ràng — không phải "một đống features ngẫu nhiên"
- Scope phải thực tế dựa trên capacity và complexity
- Cân bằng: features mới (growth) vs. stability/performance (health)
- Communicate roadmap với stakeholders — không phải commit cụ thể, nhưng phải có direction

### Now / Next / Later Framework
```
NOW (current sprint/quarter):
→ Những gì team đang build và có thể deliver sắp tới
→ Scope đã confirmed, estimate đã done

NEXT (3-6 tháng):
→ Direction tiếp theo, nhưng có thể thay đổi
→ Cần thêm discovery/validation trước khi commit

LATER (6+ tháng):
→ Vision và ideas, không phải commitment
→ Useful để set expectations với stakeholders
```

### Semantic Versioning
```
MAJOR.MINOR.PATCH (ví dụ: 2.1.3)

MAJOR: Breaking changes — không backward compatible
MINOR: New features — backward compatible
PATCH: Bug fixes — backward compatible

Pre-release: 2.1.0-beta.1, 2.1.0-rc.1
```

### Tiêu chuẩn đầu ra
- [ ] Release scope document: what's in, what's out
- [ ] Timeline và milestones cho version tiếp theo
- [ ] Roadmap đã communicate với key stakeholders
- [ ] Team đã có đủ thông tin để bắt đầu Phase 2 cho scope mới

---

## Step 8.4b — Product Analytics Setup

### Mục tiêu
Định nghĩa rõ **đo gì**, **tool nào**, và **cách setup** để đảm bảo mọi quyết định phát triển sản phẩm đều dựa trên data thực, không phải cảm tính. Không thể tối ưu điều bạn không đo.

### Tại sao bước này quan trọng?
Step 8.1 đề cập đến việc "dùng analytics" — nhưng analytics chỉ có ý nghĩa khi bạn đã định nghĩa trước **metric nào cần track**, **event nào cần log**, và **tool nào sẽ thu thập**. Nhiều team ship features mà không đo được liệu feature đó có được dùng hay không.

### Focus vào
- Xác định Core Metrics trước khi setup tool — tool là phương tiện, không phải mục đích
- Phân biệt Vanity Metrics (đẹp nhưng không actionable) vs. Actionable Metrics
- Đảm bảo tracking được implement ngay khi feature được ship, không phải sau
- Bảo đảm data quality — garbage in, garbage out

### Framework Đo Lường: HEART + GSM

**HEART Framework (Google)**
```
Happiness     → NPS, CSAT, user satisfaction scores
Engagement    → DAU/MAU, session length, actions per session
Adoption      → % users dùng feature, time-to-first-use
Retention     → Day 1/7/30 retention, churn rate
Task Success  → Completion rate, error rate, time-on-task
```

**GSM (Goals → Signals → Metrics)**
```
Goal:   Tăng engagement với feature checkout
Signal: User hoàn thành checkout thành công
Metric: Checkout completion rate, cart abandonment rate, checkout funnel drop-off
```

### Metrics Cần Track Theo Loại

| Loại Metric | Ví dụ | Tool |
|-------------|-------|------|
| **Product Usage** | Feature adoption rate, DAU/MAU, session frequency | Mixpanel, Amplitude, PostHog |
| **Funnel** | Conversion rate theo step, drop-off point | Mixpanel, GA4 |
| **Retention** | Day 1/7/30/90 retention curve | Amplitude, Mixpanel |
| **Performance** | p50/p90/p99 latency, error rate | DataDog, New Relic, Sentry |
| **Business** | Revenue, MRR, ARPU, LTV, CAC | Stripe Dashboard, custom |
| **Support** | Ticket volume, resolution time, CSAT | Zendesk, Freshdesk |

### Event Tracking Plan Template

```markdown
## Event Tracking Plan — [Tên Feature]

### Core Events Cần Track

| Event Name | Trigger | Properties | Priority |
|------------|---------|------------|----------|
| feature_viewed | User mở màn hình X | user_id, feature_name, source | P0 |
| feature_completed | User hoàn thành flow X | user_id, time_spent, result | P0 |
| feature_abandoned | User thoát giữa chừng | user_id, step_abandoned, reason | P1 |
| error_encountered | Có lỗi xảy ra | error_type, error_message, context | P0 |

### Naming Convention
- Format: [object]_[action] (ví dụ: checkout_started, payment_failed)
- Lowercase, underscore-separated
- Không dùng PII trong event properties

### Tool: [Mixpanel / Amplitude / PostHog / GA4]
### Responsible: [Dev name] implement + [PM name] verify
### Target Ship Date: [date]
```

### Feature Adoption Dashboard — Nên Có Ngay Khi Launch

Với mỗi feature mới, setup dashboard theo dõi:
1. **Adoption rate**: % user đã dùng feature ít nhất 1 lần
2. **Activation rate**: % user đã dùng feature đủ để thấy "aha moment"
3. **Retention**: user dùng feature lần 2, lần 3?
4. **Error rate**: % session có lỗi liên quan đến feature
5. **Funnel**: drop-off tại step nào trong feature

### Tiêu chuẩn đầu ra
- [ ] Metrics plan được định nghĩa cho mỗi feature trước khi develop
- [ ] Event tracking plan được sign-off bởi PM + Dev
- [ ] Analytics tool đã configured, events đã verified sau khi ship
- [ ] Feature adoption dashboard được tạo ngay sau launch
- [ ] Data quality check: events firing đúng, không duplicate, không missing

---

## Step 8.4c — A/B Testing Strategy

### Mục tiêu
Thiết kế và chạy controlled experiments để **validate hypothesis trước khi rollout toàn bộ**, tránh ship features mà không biết có cải thiện metric hay không. A/B test là cách PM chứng minh value bằng data thay vì ý kiến.

### Tại sao bước này quan trọng?
Không có A/B testing, bạn đang **betting** — ship toàn bộ và hy vọng nó tốt hơn. Với A/B testing, bạn **invest** — chạy experiment có controlled và chỉ ship full khi có evidence. Đây là core skill của modern PM.

### Focus vào
- Chỉ A/B test khi có đủ traffic để reach statistical significance trong thời gian hợp lý
- Một experiment chỉ test **một biến số** — nếu test nhiều biến thì không biết cái nào gây ra sự khác biệt
- Xác định success metric **trước** khi chạy test — không thay đổi metric sau khi thấy kết quả
- Chạy test đủ lâu để cover business cycles (ít nhất 1–2 tuần, trừ khi traffic rất lớn)

### Khi Nào Nên A/B Test?

| Nên A/B test | Không nên A/B test |
|-------------|-------------------|
| UI/UX changes ảnh hưởng conversion | Bug fixes |
| Pricing changes | Security fixes |
| Onboarding flow changes | Performance improvements |
| CTA copy, button placement | Features cho segment nhỏ (<100 users) |
| Email subject line, notification | Khi traffic quá thấp |

### A/B Test Design Template

```markdown
## A/B Test: [Tên Test]

### Hypothesis
"Chúng tôi tin rằng [thay đổi X] sẽ cải thiện [metric Y] cho [user segment Z]
vì [lý do dựa trên data/research]."

### Variants
- **Control (A):** [Mô tả baseline hiện tại]
- **Treatment (B):** [Mô tả thay đổi sẽ test]
- (Optional) **Treatment (C):** [Variant thứ 3 nếu có]

### Primary Metric (Success Metric)
[Metric chính để quyết định winner — chỉ 1]
- Ví dụ: Checkout completion rate

### Secondary Metrics (Guardrail Metrics)
[Metrics phụ để đảm bảo test không làm hại metric khác]
- Ví dụ: Revenue per user, page load time, error rate

### Audience
- **Target segment:** [All users / New users / Paid users / ...]
- **Traffic split:** 50/50 hoặc 90/10 (nếu rủi ro cao)
- **Sample size cần thiết:** [Tính bằng calculator]

### Duration
- **Start date:** [date]
- **End date:** [date — ít nhất 2 tuần]
- **Lý do duration này:** [đủ sample size? cover weekend effect?]

### Statistical Parameters
- Significance level (α): 95% (p < 0.05)
- Minimum Detectable Effect (MDE): [X%]
- Statistical power: 80%

### Decision Criteria
- ✅ Ship B nếu: B tốt hơn A với p < 0.05 và secondary metrics không bị harm
- ❌ Keep A nếu: không có significant difference, hoặc secondary metrics bị harm
- 🔄 Run longer nếu: chưa đủ sample size

### Implementation
- **Tool:** [Optimizely / LaunchDarkly / GrowthBook / custom feature flags]
- **Dev effort:** [estimate]
- **Owner:** [PM name]
- **Engineer:** [dev name]
```

### Sample Size Calculator (Quy Tắc Nhanh)

Trước khi chạy test, ước tính xem có đủ traffic không:
```
Minimum detectable effect (MDE): 5% relative improvement
Current conversion rate: 10%
→ Expected conversion in treatment: 10.5%

Cần khoảng 15,000 users per variant để detect 5% MDE với 95% confidence
→ Nếu traffic 1,000 users/ngày, mỗi variant cần ~15 ngày

Dùng tool: https://www.evanmiller.org/ab-testing/sample-size.html
```

### Đọc Kết Quả & Ra Quyết Định

```
KHÔNG được stop test sớm vì thấy kết quả đẹp — đây là "peeking problem"
Bạn sẽ false positive nhiều hơn nếu stop khi p < 0.05 lần đầu tiên

✅ Statistical significance (p < 0.05) → có thể kết luận
✅ Practical significance → improvement đủ lớn để xứng đáng ship?
✅ Guardrail metrics bình thường → không harm
→ Ship treatment

❌ p > 0.05 sau đủ sample size → null result, keep control
❌ Guardrail metrics bị harm → reject treatment dù p < 0.05
```

### Tiêu chuẩn đầu ra
- [ ] Test hypothesis đã được define rõ ràng trước khi build
- [ ] Primary và secondary metrics đã được xác định
- [ ] Sample size calculation đã được thực hiện
- [ ] Test đã chạy đủ duration và đủ sample size
- [ ] Kết quả được document và share với team
- [ ] Decision được implement (ship / keep / iterate)
- [ ] Learnings được lưu vào experiment log để tham khảo sau

---

## Step 8.5 — Technical Upgrade Planning

### Mục tiêu
Lên kế hoạch nâng cấp kỹ thuật (library versions, infrastructure, architecture refactor) một cách có kiểm soát, không để technical debt tích lũy đến mức crisis.

### Focus vào
- Dependency upgrades: giữ cho các thư viện quan trọng up-to-date về security
- Infrastructure upgrade: cloud service EOL, OS upgrade, database major version
- Architecture evolution: từ monolith → microservices khi đến đúng thời điểm
- Performance improvements: optimization có data-backed ROI
- Code quality: refactor để giảm technical debt có ảnh hưởng cao

### Technical Debt Categories
| Loại | Ví dụ | Ưu tiên |
|------|-------|---------|
| **Security debt** | Outdated library với CVE, weak auth | Cao nhất |
| **Reliability debt** | Không có error handling, no retry logic | Cao |
| **Performance debt** | N+1 queries, no caching | Trung bình |
| **Maintainability debt** | Spaghetti code, no tests | Trung bình |
| **Scalability debt** | Architecture không scale được | Dài hạn |

### Chiến Lược Xử Lý Tech Debt
- **20% rule**: dành 20% capacity mỗi sprint cho tech debt
- **Boy Scout Rule**: "Leave the code better than you found it" — cải thiện nhỏ liên tục
- **Tech debt sprint**: một sprint riêng cho tech debt khi tích lũy quá nhiều
- **Strangler Pattern**: refactor dần dần, không rewrite toàn bộ một lúc

### Tiêu chuẩn đầu ra
- [ ] Tech debt register được cập nhật hàng quý
- [ ] Technical upgrade plan với risk và effort assessment
- [ ] EOL (End of Life) tracker cho tất cả dependencies quan trọng
- [ ] Refactoring items đã được xen vào sprint backlog

---

## Step 8.6 — Quay Lại Chu Kỳ

### Mục tiêu
Khởi động lại vòng phát triển mới với scope đã được xác định, đảm bảo continuity từ những gì đã học được.

### Điểm Khởi Động Lại Tùy Theo Scope

| Scope thay đổi | Bắt đầu từ |
|---------------|-----------|
| Bug fixes & small improvements | Phase 4 (Development) |
| Feature enhancement có design rõ ràng | Phase 3 (Design) |
| Feature mới đã có requirements | Phase 2 (Analysis & Planning) |
| Feature mới chưa rõ yêu cầu | Phase 1 (Discovery) |
| Pivot lớn hoặc thay đổi chiến lược | Phase 1 (Discovery) |

### Chuyển Giao Tri Thức Giữa Các Vòng
- **What worked**: patterns, tools, approaches nên tiếp tục
- **What didn't**: mistakes, anti-patterns cần tránh
- **Context carried forward**: constraints, decisions từ vòng trước vẫn còn hiệu lực
- **Updated assumptions**: những gì chúng ta biết bây giờ mà không biết lúc trước

### Tiêu chuẩn đầu ra
- [ ] Retrospective của vòng vừa xong đã được document
- [ ] Backlog cho vòng mới đã sẵn sàng
- [ ] Team đã được brief về scope và goals
- [ ] Chu kỳ mới bắt đầu tại đúng phase phù hợp

---

## Checklist Tổng Kết Phase 8

- [ ] Feedback collection đã được thực hiện từ đủ nguồn
- [ ] Feature request backlog đầy đủ và có structure
- [ ] Prioritization đã được làm với framework rõ ràng
- [ ] Roadmap đã được communicate với stakeholders
- [ ] Tech debt review đã được thực hiện
- [ ] Team đã sẵn sàng cho vòng phát triển tiếp theo

## Dấu Hiệu Cần Cẩn Thận

⚠️ "Chúng ta biết user cần gì rồi" — không bao giờ giả định, luôn validate với data và feedback thực
⚠️ Roadmap commit quá cứng nhắc — roadmap là direction, không phải contract; phải có khả năng adjust
⚠️ Tech debt không bao giờ được priority — một ngày sẽ trở thành crisis và phải dừng feature development
⚠️ Chỉ nghe tiếng kêu to nhất — feature của khách hàng lớn nhất chưa chắc có value nhất cho product
⚠️ Thêm tính năng mà không đo kết quả — không biết tính năng nào thực sự tạo ra giá trị

---

## Vòng Lặp Liên Tục (The Continuous Loop)

```
         ┌─────────────────────────────────────┐
         │                                     │
         ▼                                     │
[8.1 Collect Feedback]                         │
         │                                     │
         ▼                                     │
[8.2 Build Backlog]                            │
         │                                     │
         ▼                                     │
[8.3 Prioritize]                               │
         │                                     │
         ▼                                     │
[8.4 Plan Release]                             │
         │                                     │
         ▼                                     │
[Phase 2 → 3 → 4 → 5 → 6]                     │
         │                                     │
         ▼                                     │
[Phase 7 Maintenance] ─────────────────────────┘
```

---

*← [Phase 7: Maintenance](07-maintenance-support.md) | [SDLC Overview](../SDLC-Overview.md)*
