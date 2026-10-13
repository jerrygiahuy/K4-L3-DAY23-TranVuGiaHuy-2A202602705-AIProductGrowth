# OPERATING DASHBOARD - AI NOTES

**B2C · Cập nhật 13/10/2026 · Trần Vũ Gia Huy - 2A202602705**  
**NORTH STAR:** Retention flattening `D28/D21` - **chưa đo** - mục tiêu **≥0,90**

## Đèn báo sớm - ngày/tuần

| Đèn | Hiện tại | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---:|---|---|---|
| Retention D28/D21 | Chưa đo | ≥0,90 / 0,75-<0,90 / <0,75 | [TB] | Retention M3, LTV |
| Activation trong 24h | Chưa đo | ≥50% / 35-<50% / <35% | [TB] | D28 retention, trial→paid |
| Assisted completion R | 70% giả định | ≥78,405% / 52,155-<78,405% / <52,155% | [MH] | Cost/note, GM |
| p95 cost/ARPU | Chưa đo | ≤40% / >40-60% / >60% | [MH] | GM, tách gói |

## Đèn vận hành - tuần/tháng

| Đèn | Hiện tại | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---:|---|---|---|
| Trial→paid | Chưa đo | ≥8,5% / 5-<8,5% / <5% | [BM] RevenueCat 2026; kiểm tra 13/10/2026 | Paid growth, payback |
| Free-tier cost / total COGS | Chưa đo | ≤20% / >20-40% / >40% | [MH] | Cash burn, GM |

## Đèn kết quả - tháng

| Đèn | Hiện tại | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---:|---|---|
| Gross margin sau overhead | 55,1971% mô hình | ≥60% / 50-<60% / <50% | [MH] Day 22 |

## 5 luật quyết định

1. **⏹ Retention:** NẾU D28/D21 <0,75 TRONG 2 cohort, VÀ mỗi cohort ≥30 signup, THÌ freeze paid acquisition 3 tuần và sửa activation. KHÔNG THÌ không tăng ads để bù churn.
2. **⏹ Completion:** NẾU R <52,155% TRONG 2 tuần, VÀ tổng ≥100 attempts, THÌ dừng mở rộng pilot, freeze feature và sửa 3 failure mode lớn nhất. KHÔNG THÌ không lấy attempts làm thành công.
3. **Cost tail:** NẾU p95 cost/ARPU >60% TRONG 1 kỳ billing, VÀ ≥30 paid accounts, THÌ cap usage hoặc tách gói credit. KHÔNG THÌ không tăng giá đại trà.
4. **⏹ Conversion:** NẾU trial→paid <5% TRONG 3 paywall test, VÀ mỗi test ≥30 trials, THÌ dừng giảm giá và đổi aha moment/value metric. KHÔNG THÌ không thêm coupon.
5. **Free tier:** NẾU free-tier cost >40% COGS TRONG 2 tháng, VÀ ledger phủ ≥95% request, THÌ giảm quota/model hoặc buộc đăng nhập. KHÔNG THÌ không mở rộng free tier.

## Cổng gác 90 ngày

| Ngày | Một metric | Ngưỡng qua cổng | Bằng chứng vật lý | Nếu trượt |
|---|---|---|---|---|
| 30 · 12/11/2026 | Activation 24h | ≥35% | `events.csv` + `activation_cohort_D30.pdf` | **FIX một lần:** sửa onboarding/aha moment |
| 60 · 12/12/2026 | Retention D28/D21 | ≥0,75 | `retention_cohort_D60.csv` + chart | **PIVOT:** thu hẹp use case học hằng ngày |
| 90 · 11/01/2027 | Trial→paid | ≥5% | payment export + `trial_conversion_D90.pdf` | **KILL** gói B2C $8; chuyển discovery B2B |

**KILL CRITERIA:** Đến **11/01/2027**, nếu trial→paid <5% trên ≥60 eligible trials sau đúng một vòng FIX paywall/activation, dừng gói B2C $8 và mở 3 cuộc phỏng vấn trường/doanh nghiệp trong 4 tuần.

**CHƯA ĐO ĐƯỢC:** Chưa có production cohort, paid buyer hoặc cost ledger. Instrument trước 20/10; R có số 26/10; activation 12/11; retention/cost share 12/12; conversion 11/01/2027. Số 70% R và 55,1971% loaded margin là mô hình Day 22, không phải kết quả pilot.

---

## Phụ lục [MH] dùng khi xuất PDF trang 2

1. `R_direct60 = 0,13908 / (($8/12) × 40%) = 52,155%`; loaded-margin 60% cần `R = 78,405%`.
2. Full quota direct COGS/account `$3,07/$8 = 38,375%`; retry infra ngoài reserve `$3,205/$8 = 40,0625%`.
3. Với paid COGS `$166,896`: free share ≤20% ⇒ free COGS ≤`$41,724`; free share >40% ⇒ free COGS >`$111,264`.
4. Loaded margin base `($560-$166,896-$84)/$560 = 55,1971%`; để đạt 60% cần giảm cost ít nhất `$26,896/tháng`.

**Benchmark:** RevenueCat, *State of Subscription Apps 2026*, 19/03/2026, cập nhật 22/04/2026, kiểm tra 13/10/2026: https://www.revenuecat.com/blog/growth/subscription-app-trends-benchmarks-2026
