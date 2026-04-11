# Calculation Formula Reference

**Calculator:** No Tax on Overtime Calculator
**URL:** https://notaxovertimecalculator.com
**Author:** Michal Anderson, CPA
**Formula Version:** v1.1

---

## Input Variables

| Variable | Description | Example |
|---|---|---|
| filing_status | "single" or "married_filing_jointly" | "single" |
| weekly_base_pay | Regular weekly wages (no overtime) | $800 |
| weekly_overtime_premium | The extra 0.5x portion of OT pay only | $100 |
| weekly_tips | Reported tip income per week | $200 |

---

## Constants (Tax Year 2025)

| Constant | Single | Married Filing Jointly |
|---|---|---|
| Overtime deduction cap | $12,500 | $25,000 |
| Tips deduction cap | $12,500 | $25,000 |
| Phase-out threshold | $150,000 MAGI | $300,000 MAGI |
| Phase-out elimination | $275,000 MAGI | $550,000 MAGI |
| Phase-out rate | $100 per $1,000 over threshold | Same |
| Standard deduction | $15,000 | $30,000 |

---

## Formula

```
annual_base    = weekly_base_pay x 52
annual_ot      = weekly_overtime_premium x 52
annual_tips    = weekly_tips x 52
magi           = annual_base
capped_ot      = MIN(annual_ot, ot_cap)
excess         = MAX(0, magi - phase_out_threshold)
reduction      = FLOOR(excess / 1000) x 100
final_ot_ded   = MAX(0, capped_ot - reduction)
taxable_income = MAX(0, magi - standard_deduction)
marginal_rate  = bracket_lookup(taxable_income, filing_status)
ot_savings     = final_ot_ded x marginal_rate
tips_savings   = MIN(annual_tips, tips_cap) x marginal_rate
total_savings  = ot_savings + tips_savings
```

---

## Worked Example 1 — Single Nurse

Input: $900/week base | $150/week OT premium | Single filer

```
annual_ot      = $7,800
capped_ot      = $7,800  (under $12,500 cap)
magi           = $46,800  (below phase-out)
reduction      = $0
taxable_income = $31,800
marginal_rate  = 12%
savings        = $7,800 x 0.12 = $936/year
```

---

## Worked Example 2 — Restaurant Worker with Tips

Input: $600/week base | $80 OT premium | $300/week tips | Single filer

```
annual_ot      = $4,160
annual_tips    = $15,600 capped to $12,500
magi           = $31,200
reduction      = $0
marginal_rate  = 12%
ot_savings     = $499/year
tips_savings   = $1,500/year
total          = $1,999/year
```

---

## Worked Example 3 — Phase-Out Applies

Input: $3,200/week base | $400 OT premium | Single filer

```
annual_ot      = $20,800 capped to $12,500
magi           = $166,400
excess         = $16,400
reduction      = $1,600
final_ot_ded   = $10,900
marginal_rate  = 24%
savings        = $2,616/year
```

---

## Pending IRS Guidance

| Item | Status |
|---|---|
| Schedule 1 line designation | Pending |
| State-mandated overtime only | Pending |
| Double-time premium | Pending |
| 2026 W-2 mandatory OT reporting | Confirmed mandatory |

---

*Version v1.1 | Last updated July 2025 | Reviewed by Michal Anderson, CPA*
