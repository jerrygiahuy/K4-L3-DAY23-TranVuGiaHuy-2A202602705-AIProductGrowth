# Worksheet - AI Notes

**Họ tên:** Trần Vũ Gia Huy · **MSSV:** 2A202602705 · **Ngày làm:** 13/10/2026  
**Phạm vi:** giả thuyết vận hành 90 ngày cho gói B2C $8/tháng, 12 qualifying notes. Số ghi “chưa đo” không phải kết quả thực tế.

## Trạm 1 - Loại mô hình

**Câu chốt loại:** AI Notes là **B2C** vì người học cá nhân là người dùng, người hưởng lợi và người trả tiền; chưa có trường học, doanh nghiệp hay partner trả thay.

| Đèn B2C trong Handbook | Trạng thái | Số nằm ở đâu / cần gì để đo |
|---|---:|---|
| Đường cong retention có phẳng không | 🔧 | Cần event log và cohort D1/D7/D21/D28; cohort đầu đủ tuổi ngày 12/11/2026 |
| Activation rate | 🔧 | Cần log `signup_at`, `note_id`, evidence, source và review; baseline ngày 12/11/2026 |
| p95 cost/user/tháng ÷ ARPU | 🔧 | Có ARPU $8 và mô hình cap; cần billing/token/infra log theo account |
| Trial → paid | 🔧 | Cần trial cohort và payment event; số đầu ngày 11/01/2027 |
| Retention M12 | ❌ | Không thể đo trong 90 ngày; cohort đầu đủ 12 tháng vào 13/10/2027 |
| Chi phí free tier ÷ tổng COGS | 🔧 | Cần cost ledger gắn `account_id` và trạng thái paid/free |
| Tỷ lệ refund | ❌ | Chưa có giao dịch thật; cần payment/refund log |
| LTV/CAC · payback · GM | 🔧 | GM mô hình có ở Day 22; LTV/CAC và payback thực cần paid cohort + acquisition spend |

**First light:** hình dạng đường cong retention. Nếu cohort tiếp tục rơi mạnh từ D21 đến D28, tăng acquisition chỉ làm tăng churn và token cost.

## Trạm 2 - Cây chỉ số và thẻ đèn

**North Star:** `D28/D21 retained-user ratio` - hiện tại **chưa đo** - mục tiêu **≥0,90**. Đây là tỷ lệ giữa retention D28 và D21 trong cùng cohort, dùng để nhận biết đường cong bắt đầu phẳng.

| # | Tầng | Đèn | Định nghĩa chặt | Công thức | Nhịp · owner | Báo trước cho |
|---:|:---:|---|---|---|---|---|
| 1 | L | Retention flattening ratio | User “retained” có ≥1 qualifying note trong cửa sổ ngày tương ứng; không đếm login, mở app hay note lỗi | `retention D28 / retention D21` theo signup cohort | Tuần · Gia Huy | Retention M3 và LTV |
| 2 | L | Activation trong 24h | % signup tạo ≥1 note có ≥2 evidence, ≥1 source, review mọi section và lưu `note_id` duy nhất trong 24h; không đếm draft chưa review | `activated signup / eligible signup` | Ngày · Gia Huy | Retention D28 và trial→paid |
| 3 | L | Assisted completion R | % normal attempts tạo qualifying note; không đếm retry kỹ thuật, draft hoặc note thiếu điều kiện | `completed qualifying notes / normal attempts` | Tuần · Gia Huy | Cost/note và gross margin |
| 4 | L | p95 cost/ARPU | Chi phí trực tiếp tháng của account ở phân vị 95, gồm LLM, retry, infra, QA/support; không gồm overhead và không dùng trung bình | `p95(direct cost/account/month) / $8` | Tuần · Gia Huy | Gross margin và nhu cầu tách gói |
| 5 | O | Trial→paid | % trial đủ thời gian quan sát phát sinh giao dịch $8 thành công trong 7 ngày sau trial; không đếm coupon 100%, refund hoặc test payment | `new paid accounts / eligible ended trials` | Tuần · Gia Huy | Paid growth và CAC payback |
| 6 | O | Free-tier cost share | Direct COGS của account free chia tổng direct COGS; không gồm overhead, marketing hay account nội bộ | `free-tier direct COGS / total direct COGS` | Tháng · Gia Huy | Cash burn và gross margin tổng |
| 7 | G | Gross margin sau overhead | Doanh thu thuần trừ direct COGS và overhead $84; không gọi đây là direct GM | `(net revenue - direct COGS - $84) / net revenue` | Tháng · Gia Huy | Khả năng duy trì gói $8 |

**Đèn bắt chi phí AI sớm:** số 4 - p95 cost/ARPU. Nó phát hiện power user làm chi phí tăng trước khi số trung bình và gross margin tháng chuyển đỏ.

## Trạm 3 - Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn | Lý do |
|---:|---|---:|---:|---:|---|---|
| 1 | Retention flattening ratio | ≥0,90 | 0,75-<0,90 | <0,75 | [TB] | Hai cohort đầu tạo baseline; tỷ lệ gần 1 nghĩa là mức rơi từ D21 đến D28 đã chậm rõ rệt. |
| 2 | Activation trong 24h | ≥50% | 35-<50% | <35% | [TB] | Chưa có chuẩn cho aha moment riêng của AI Notes; 35% là learning gate tối thiểu, 50% là mục tiêu sau một vòng FIX. |
| 3 | Assisted completion R | ≥78,405% | 52,155-<78,405% | <52,155% | [MH] | Day 22 cho thấy 78,405% giữ margin sau overhead ≥60%, còn dưới 52,155% làm direct GM <60%. |
| 4 | p95 cost/ARPU | ≤40% | >40-60% | >60% | [MH] | 40% là trần COGS cho direct GM 60%; trên 60% chỉ còn dưới 40% direct GM và phải chặn power-user loss. |
| 5 | Trial→paid | ≥8,5% | 5-<8,5% | <5% | [BM] | RevenueCat 2026 ghi median AI app trial conversion 8,5%; 5% là sàn falsification trước khi sửa activation/value metric. Kiểm tra 13/10/2026. |
| 6 | Free-tier cost share | ≤20% | >20-40% | >40% | [MH] | Với paid COGS nền $166,896/tháng, share 20% chỉ cho phép $41,724 free COGS; trên 40% đòi giảm quota/model. |
| 7 | Gross margin sau overhead | ≥60% | 50-<60% | <50% | [MH] | Base-case Day 22 là 55,1971% nên chưa đủ để scale; 60% là gate nội bộ, 50% là mức dừng chi acquisition. |

**Nguồn [BM]:** RevenueCat, *State of Subscription Apps 2026*, công bố 19/03/2026, cập nhật 22/04/2026: https://www.revenuecat.com/blog/growth/subscription-app-trends-benchmarks-2026 (kiểm tra 13/10/2026).

### Phụ lục [MH] - phép tính

**[MH] 1 - Assisted completion R**

```text
Day 22: 1.200 normal attempts/tháng; variable cost = $0,13908/attempt;
giá hiệu dụng P = $8/12 = $0,666667/note; overhead = $84/tháng.
Direct GM = 1 - (0,13908/R) / 0,666667.
Direct GM ≥60% => R ≥ 0,13908 / (0,666667 × 40%) = 52,155%.
Margin sau overhead ≥60% => R ≥78,405% (Day 22 model, ô 2_Pricing!B63).
Kết quả: xanh ≥78,405%; vàng 52,155-<78,405%; đỏ <52,155%.
```

**[MH] 2 - p95 cost/ARPU**

```text
ARPU = $8/account/tháng. Trần direct COGS để giữ GM 60% = $8 × 40% = $3,20.
Full-quota stress Day 22: 22 attempts + 3 retries = 50 calls;
direct COGS/account = $3,07; ratio = $3,07/$8 = 38,375%.
Nếu retry infra phát sinh ngoài reserve: $3,205/$8 = 40,0625%.
Kết quả: xanh ≤40%; vàng >40-60%; đỏ >60%.
```

**[MH] 3 - Free-tier cost share**

```text
Paid direct COGS nền C = $166,896/tháng. Gọi F là free-tier COGS.
F/(F+C) ≤20% => F ≤0,25C = $41,724/tháng.
F/(F+C) >40% => F > (2/3)C = $111,264/tháng.
Kết quả: xanh ≤20%; vàng >20-40%; đỏ >40%.
```

**[MH] 4 - Gross margin sau overhead**

```text
Base case Day 22: revenue = 70 × $8 = $560;
direct COGS = $166,896; overhead = $84.
Loaded margin = ($560 - $166,896 - $84)/$560 = 55,1971% => vàng.
Để đạt 60%, tổng cost tối đa = $560 ×40% = $224; hiện tại = $250,896;
cần giảm ít nhất $26,896/tháng hoặc tăng completion/doanh thu mà không phá retention.
```

## Trạm 4 - 5 luật quyết định

1. **⏹ NẾU** retention flattening ratio <0,75 **TRONG** 2 cohort liên tiếp **VÀ** mỗi cohort có ≥30 signup đủ tuổi D28, **THÌ** đóng băng paid acquisition trong 3 tuần và chuyển sprint sang activation/onboarding. **KHÔNG THÌ** không tăng quảng cáo để bù số user rời đi.
2. **⏹ NẾU** assisted completion R <52,155% **TRONG** 2 tuần liên tiếp **VÀ** có tổng ≥100 normal attempts, **THÌ** dừng mở rộng pilot, freeze feature mới và sửa ba failure mode lớn nhất từ log. **KHÔNG THÌ** không lấy số attempt làm thành công hoặc che lỗi bằng số trung bình tháng.
3. **NẾU** p95 cost/ARPU >60% **TRONG** 1 kỳ billing **VÀ** có ≥30 paid accounts, **THÌ** áp usage cap ở sprint kế tiếp hoặc tách gói power-user theo credit. **KHÔNG THÌ** không tăng giá đại trà cho toàn bộ user.
4. **⏹ NẾU** trial→paid <5% **TRONG** 3 thử nghiệm paywall liên tiếp **VÀ** mỗi biến thể có ≥30 eligible trials, **THÌ** dừng thử giảm giá và thay aha moment hoặc value metric trước thử nghiệm tiếp theo. **KHÔNG THÌ** không chạy thêm coupon để làm đẹp conversion.
5. **NẾU** free-tier cost share >40% **TRONG** 2 tháng liên tiếp **VÀ** cost ledger bao phủ ≥95% request, **THÌ** giảm quota free, chuyển request free sang model rẻ hơn hoặc yêu cầu đăng nhập trong sprint kế tiếp. **KHÔNG THÌ** không mở rộng free tier để mua tăng trưởng.

## Chưa đo được

- Retention, activation và trial→paid chưa có vì prototype chưa có production event log hoặc paid cohort; cần instrument các event đã nêu và tuyển pilot, có baseline đầu tiên ngày **12/11/2026**.
- R=70%, 55,1971% loaded margin và các cost ratio hiện là kết quả mô hình, chưa phải telemetry; cần ≥100 attempts gắn cost/job, có số đầu ngày **26/10/2026**.
- p95 và free-tier cost share cần cost ledger theo account; có số tháng đầu ngày **12/12/2026**.
- Retention M12, refund, LTV/CAC thực chưa thể có trong 90 ngày; retention M12 sớm nhất ngày **13/10/2027**.
