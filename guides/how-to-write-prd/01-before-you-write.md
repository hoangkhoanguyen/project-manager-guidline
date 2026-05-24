# 01 — Trước Khi Viết PRD

> Sai lầm lớn nhất: mở file mới và bắt đầu viết section "Problem Statement" khi chưa có đủ thông tin.
> PRD tệ không xuất phát từ viết kém — nó xuất phát từ **research không đủ**.

---

## Checklist Trước Khi Viết

Trước khi gõ một chữ vào PRD, bạn phải có câu trả lời cho các câu hỏi sau.

### 1. Vấn đề có thật không?

- [ ] Có data/evidence chứng minh vấn đề đang xảy ra? (số liệu analytics, support ticket, user interview)
- [ ] Đây là vấn đề của bao nhiêu % user? Ảnh hưởng đến revenue hay churn như thế nào?
- [ ] Đây có phải là vấn đề quan trọng nhất cần giải quyết lúc này không, hay còn thứ ưu tiên hơn?

**Nếu chưa có data:** Đừng viết PRD. Đi thu thập data trước.

### 2. Ai là người dùng?

- [ ] Đã nói chuyện với ít nhất 3–5 người dùng thực tế (user interview)?
- [ ] Biết được: họ đang làm gì, gặp khó khăn gì, họ muốn gì?
- [ ] Phân biệt được các nhóm user khác nhau nếu có (ví dụ: buyer vs seller, admin vs staff)?

**Nếu chưa interview:** Ít nhất phải có survey data hoặc session recording. Đừng suy đoán về user.

### 3. Business context rõ chưa?

- [ ] Biết được tính năng này phục vụ mục tiêu business nào? (tăng conversion? giảm churn? mở market mới?)
- [ ] Ai là stakeholder? Họ muốn gì từ tính năng này?
- [ ] Có constraint nào về timeline, budget, hoặc tech không?
- [ ] Có dependency nào với team khác không?

### 4. Competitive landscape

- [ ] Đối thủ đang giải quyết vấn đề này như thế nào?
- [ ] Có best practice nào trong ngành mà nên follow không?
- [ ] Điểm khác biệt của solution mình là gì?

---

## Các Hoạt Động Discovery Cần Làm

### User Interview (bắt buộc cho feature mới)

**Khi nào cần:** Tính năng ảnh hưởng trực tiếp đến UX của user.

**Cách làm cơ bản:**
1. Xác định 5–8 người dùng đại diện cho target segment
2. Đặt lịch 30–45 phút mỗi người
3. Hỏi về **hành vi hiện tại**, không hỏi về **sản phẩm mong muốn**

Câu hỏi đúng:
> "Bạn thường làm gì khi muốn [đạt goal X]? Hãy kể tôi nghe lần gần nhất bạn làm điều đó."

Câu hỏi sai:
> "Bạn có muốn có tính năng [Y] không?" — Người ta sẽ luôn nói có.

**Output cần ghi lại:**
- Pain points thực tế (quote trực tiếp từ user)
- Current workaround họ đang dùng
- Điều gì đang blocking họ

### Stakeholder Interview

**Khi nào cần:** Luôn luôn — trước khi viết bất kỳ PRD nào.

**Câu hỏi cần hỏi:**
- "Tính năng này thành công trông như thế nào với bạn, sau 3 tháng?"
- "Constraint nào mà tôi cần biết? (timeline, budget, tech, legal)"
- "Có quyết định nào đã được đưa ra rồi mà tôi không được thay đổi?"
- "Ai là người approve cuối cùng?"

### Data Review

Trước khi viết Problem Statement, review:
- **Funnel analytics:** User đang drop off ở bước nào?
- **Session recording** (Hotjar/FullStory): User đang làm gì trên màn hình?
- **Support tickets:** Complaint phổ biến nhất về flow này là gì?
- **A/B test history:** Có test nào trước đây liên quan không?

---

## Thông Tin Bàn Giao Cho BA Khi Làm Việc Cùng

> Nếu bạn là PM và có BA hỗ trợ viết PRD, bạn cần bàn giao những thông tin sau TRƯỚC KHI BA bắt đầu viết.

### Thông tin Business (PM bàn giao)

1. **Vấn đề và context:** Tại sao làm tính năng này, tại sao làm bây giờ
2. **Target user:** Ai là người dùng chính, ai là người dùng phụ
3. **Success metrics:** Sau khi launch, đo bằng gì để biết thành công
4. **Scope và non-scope:** Cái gì có trong v1, cái gì defer sang v2
5. **Constraints:** Timeline cứng, tech constraint, legal/compliance nếu có
6. **Stakeholder list:** Ai approve, ai được consult, ai chỉ cần inform
7. **Research đã có:** Share raw user interview notes, analytics data

### Thông tin Nghiệp Vụ (Domain expert bàn giao)

1. **Business rules hiện tại:** Logic tính toán, điều kiện áp dụng
2. **Exception cases đã biết:** Edge case nào đang được xử lý bằng tay
3. **Regulation/Compliance:** Có rule pháp lý nào ảnh hưởng không
4. **Integration points:** Hệ thống nào đang tương tác với nhau

### Hướng Dẫn Làm Việc Với BA Chưa Có Kinh Nghiệm

**Đừng chỉ throw spec vào và nói "viết PRD đi"** — BA mới cần được pair trong 1–2 tuần đầu.

Cách làm việc hiệu quả:
1. **Pair session đầu tiên:** PM ngồi cùng BA, giải thích vấn đề, answer questions trực tiếp
2. **Outline review:** BA viết outline (không phải full PRD) trước, PM review direction
3. **Section-by-section:** Không review toàn bộ một lúc — review từng section khi BA xong
4. **Giải thích why:** Khi sửa, luôn giải thích tại sao sai, không chỉ sửa

Lỗi phổ biến của BA mới và cách dạy:
- Viết HOW thay vì WHAT → Hỏi: "Đây là requirement hay là implementation?"
- AC quá mơ hồ → Hỏi: "QA sẽ test cái này như thế nào? Pass/fail condition là gì?"
- Bỏ qua edge case → Hỏi: "Nếu user làm X nhưng Y thì sao?"

---

## Template: Discovery Notes (dùng trước khi viết PRD)

```markdown
## Discovery Notes — [Tên Tính Năng]

### Problem Evidence
- [Data point 1]
- [User quote 1]

### User Research Summary
- Đã interview: [số người, vai trò]
- Top pain points:
  1.
  2.
  3.

### Stakeholder Alignment
- Business goal: 
- Success metric đồng ý:
- Scope đã align:
- Constraint:

### Competitive / Best Practice Research
- Đối thủ A làm:
- Đối thủ B làm:
- Best practice trong ngành:

### Open Questions Trước Khi Viết
- [ ]
- [ ]
```

---

*Tiếp theo: [02-prd-structure.md](./02-prd-structure.md) — Cấu trúc 10 sections của một PRD.*
