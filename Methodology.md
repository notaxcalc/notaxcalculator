
# Methodology — No Tax on Overtime Calculator

**Live Tool:** [notaxovertimecalculator.com](https://notaxovertimecalculator.com)  
**Law:** One Big Beautiful Bill Act (OBBBA), signed July 4, 2025  
**Applies to:** Tax Years 2025 – 2028  
**Last Updated:** April 2026  
**Formula Version:** v1.1  
**Status:** Open for public audit — no credentials claimed

---

## Table of Contents

1. [Purpose of This Document](#1-purpose-of-this-document)
2. [Legal Basis](#2-legal-basis)
3. [Key Definitions](#3-key-definitions)
4. [What Qualifies — and What Does Not](#4-what-qualifies--and-what-does-not)
5. [Step-by-Step Calculation Methodology](#5-step-by-step-calculation-methodology)
6. [Tax Bracket Tables](#6-tax-bracket-tables)
7. [Phase-Out Worked Examples](#7-phase-out-worked-examples)
8. [Tips Deduction Methodology](#8-tips-deduction-methodology)
9. [Combined Overtime + Tips Example](#9-combined-overtime--tips-example)
10. [Edge Cases and Boundary Conditions](#10-edge-cases-and-boundary-conditions)
11. [What This Calculator Does Not Cover](#11-what-this-calculator-does-not-cover)
12. [Known Limitations and Pending IRS Guidance](#12-known-limitations-and-pending-irs-guidance)
13. [Data Privacy](#13-data-privacy)
14. [How to Challenge or Correct This Methodology](#14-how-to-challenge-or-correct-this-methodology)
15. [Changelog](#15-changelog)

---

## 1. Purpose of This Document

This document explains every calculation decision made inside the  
No Tax on Overtime Calculator — why each step exists, what law it  
comes from, and how edge cases are handled.

It is written for three audiences:

- **Workers** who want to understand what the calculator is actually doing  
- **Tax professionals** who want to audit the formula against the statute  
- **Developers** who want to identify bugs or suggest improvements

This is not legal advice. This is not tax advice. It is a transparent  
explanation of how a free estimation tool works.

If you find an error in the methodology, open a  
[GitHub Issue](https://github.com/notaxcalc/notaxcalculator/issues)  
with your source and we will review it within 48 hours.

---

## 2. Legal Basis

The overtime premium deduction was created by the  
**One Big Beautiful Bill Act (OBBBA)**, signed into law on July 4, 2025.

| Provision | Detail |
|---|---|
| Deduction type | Above-the-line federal income tax deduction |
| Qualifying income | FLSA overtime premium pay (0.5x of regular rate only) |
| Deduction cap — Single | $12,500 per tax year |
| Deduction cap — Married Filing Jointly | $25,000 per tax year |
| Phase-out start — Single | $150,000 MAGI |
| Phase-out start — Married Filing Jointly | $300,000 MAGI |
| Full phase-out — Single | $275,000 MAGI |
| Full phase-out — Married Filing Jointly | $550,000 MAGI |
| Phase-out reduction rate | $100 per $1,000 of MAGI above threshold |
| Effective tax years | 2025, 2026, 2027, 2028 |
| Expiration | December 31, 2028 (unless extended by Congress) |

**This deduction did not exist before July 4, 2025.**  
It cannot be claimed on tax returns for 2024 or earlier years.

**Official sources:**
- [Congress.gov — OBBBA full text](https://www.congress.gov/)
- [IRS.gov — Tax professionals guidance](https://www.irs.gov/tax-professionals)
- [DOL FLSA overtime rules](https://www.dol.gov/agencies/whd/flsa)

---

## 3. Key Definitions

### Regular Rate of Pay
The employee's standard hourly wage before any overtime  
multiplier is applied. This is the base number used in all  
premium calculations.

### Overtime Premium Pay
Only the **extra half** (0.5x) of time-and-a-half pay qualifies  
for the OBBBA deduction. Not the full 1.5x overtime paycheck.

**Why only 0.5x?**  
When you work overtime at 1.5x your regular rate, that pay  
consists of two parts:

```
1.0x  →  Regular pay (you would have earned this anyway)
0.5x  →  Premium pay (the bonus for working overtime)
```

The OBBBA specifically targets the **premium portion** — the  
incremental amount above regular wages. The 1.0x base portion  
is treated as regular income.

**Example:**
```
Regular rate:     $20.00/hr
Overtime rate:    $30.00/hr (1.5x)
Premium portion:  $10.00/hr (0.5x) ← Only this qualifies
```

### MAGI (Modified Adjusted Gross Income)
The income figure used to determine eligibility and phase-out.

For most W-2 employees, MAGI is calculated as:
```
W-2 Box 1 (Wages)
− 401(k) / 403(b) contributions (W-2 Box 12, Code D)
− HSA contributions (W-2 Box 12, Code W)
− Traditional IRA deductions (if applicable)
− Student loan interest deduction
= Estimated MAGI
```

**Important:** The calculator uses the MAGI entered by the user.  
It does not independently verify or compute MAGI from raw inputs.  
Users who enter gross salary without deductions will receive a  
conservative (lower) savings estimate, which is the safer direction  
to err in.

### Standard Deduction
Applied before bracket lookup to determine taxable income:

| Filing Status | 2025 | 2026 |
|---|---|---|
| Single | $15,000 | $15,750 (estimated) |
| Married Filing Jointly | $30,000 | $31,500 (estimated) |

2026+ figures are adjusted for inflation using the IRS COLA  
adjustment pattern. Exact 2026 figures will be confirmed by  
IRS Rev. Proc. in late 2025.

---

## 4. What Qualifies — and What Does Not

### Qualifies ✔

- Hourly employees working more than 40 hours per week  
  under FLSA rules
- Salaried non-exempt employees receiving FLSA overtime pay
- Overtime reported on W-2 or documented by employer
- Overtime earned while MAGI is below the full phase-out  
  threshold ($275,000 single / $550,000 MFJ)

### Does Not Qualify ✘

- Self-employed workers and 1099 independent contractors  
  (FLSA overtime does not apply)
- Salaried exempt employees (executives, managers, certain  
  professionals)
- Workers earning only state-mandated overtime with no  
  federal FLSA coverage (IRS guidance pending)
- Double-time pay (Treasury has not confirmed if the  
  double premium qualifies — this calculator excludes it  
  to be conservative)
- Income above full phase-out threshold ($275,000 single /  
  $550,000 MFJ)

---

## 5. Step-by-Step Calculation Methodology

### Step 1 — Calculate Annual Overtime Premium

```
weekly_ot_premium  = weekly_ot_hours × (regular_hourly_rate × 0.5)
annual_ot_premium  = weekly_ot_premium × weeks_worked_per_year
```

**Example:**
```
Regular rate:        $18.00/hr
Weekly OT hours:     8 hrs
Weeks worked:        50

weekly_ot_premium  = 8 × ($18.00 × 0.5)
                   = 8 × $9.00
                   = $72.00/week

annual_ot_premium  = $72.00 × 50
                   = $3,600/year
```

---

### Step 2 — Apply Deduction Cap

```
deduction_cap    = $12,500  (Single)
                 = $25,000  (Married Filing Jointly)

capped_deduction = MIN(annual_ot_premium, deduction_cap)
```

**Example (continuing from Step 1):**
```
annual_ot_premium = $3,600
deduction_cap     = $12,500 (Single)

capped_deduction  = MIN($3,600, $12,500)
                  = $3,600  ← Premium is under cap, no reduction
```

**Example where cap applies:**
```
annual_ot_premium = $18,000
deduction_cap     = $12,500

capped_deduction  = MIN($18,000, $12,500)
                  = $12,500  ← Capped at maximum
```

---

### Step 3 — Apply Phase-Out Reduction

```
phase_out_threshold = $150,000  (Single)
                    = $300,000  (Married Filing Jointly)

excess_magi         = MAX(0, MAGI − phase_out_threshold)
reduction           = FLOOR(excess_magi ÷ 1,000) × $100
final_deduction     = MAX(0, capped_deduction − reduction)
```

**Why FLOOR?**  
The statute reduces the deduction by $100 per complete $1,000  
increment above the threshold. Partial $1,000 amounts do not  
trigger an additional reduction. FLOOR() captures this correctly.

**Example — no phase-out:**
```
MAGI:               $72,000
Threshold:          $150,000
excess_magi:        MAX(0, $72,000 − $150,000) = $0
reduction:          $0
final_deduction:    $3,600 (unchanged)
```

**Example — partial phase-out:**
```
MAGI:               $165,000
Threshold:          $150,000
excess_magi:        $165,000 − $150,000 = $15,000
reduction:          FLOOR($15,000 ÷ $1,000) × $100
                  = 15 × $100
                  = $1,500
final_deduction:    MAX(0, $3,600 − $1,500) = $2,100
```

**Example — full phase-out:**
```
MAGI:               $280,000
Threshold:          $150,000
excess_magi:        $280,000 − $150,000 = $130,000
reduction:          FLOOR(130) × $100 = $13,000
final_deduction:    MAX(0, $3,600 − $13,000) = $0
```

---

### Step 4 — Determine Marginal Tax Rate

```
taxable_income  = MAX(0, MAGI − standard_deduction)
marginal_rate   = bracket_lookup(taxable_income, filing_status)
```

The calculator uses the **marginal rate** (the rate on the last  
dollar of income), not the effective (average) rate. This is  
technically correct because the deduction reduces income from  
the top of the bracket downward.

Full bracket tables are in [Section 6](#6-tax-bracket-tables).

---

### Step 5 — Calculate Estimated Tax Savings

```
estimated_savings = final_deduction × marginal_rate
```

**Example (continuing):**
```
MAGI:              $72,000
Standard deduction: $15,000
taxable_income:    $57,000
marginal_rate:     22% (falls in $48,476–$103,350 bracket)

final_deduction:   $3,600
estimated_savings: $3,600 × 0.22 = $792/year
```

---

### Step 6 — Per-Period Breakdown

```
annual_savings    = estimated_savings
monthly_savings   = annual_savings ÷ 12
biweekly_savings  = annual_savings ÷ 26
weekly_savings    = annual_savings ÷ 52
```

**Example:**
```
annual_savings:    $792
monthly_savings:   $792 ÷ 12  = $66.00
biweekly_savings:  $792 ÷ 26  = $30.46
weekly_savings:    $792 ÷ 52  = $15.23
```

---

## 6. Tax Bracket Tables

### 2025 — Single Filers

| Taxable Income | Marginal Rate |
|---|---|
| $0 – $11,925 | 10% |
| $11,926 – $48,475 | 12% |
| $48,476 – $103,350 | 22% |
| $103,351 – $197,300 | 24% |
| $197,301 – $250,525 | 32% |
| $250,526 – $626,350 | 35% |
| Over $626,350 | 37% |

### 2025 — Married Filing Jointly

| Taxable Income | Marginal Rate |
|---|---|
| $0 – $23,850 | 10% |
| $23,851 – $96,950 | 12% |
| $96,951 – $206,700 | 22% |
| $206,701 – $394,600 | 24% |
| $394,601 – $501,050 | 32% |
| $501,051 – $751,600 | 35% |
| Over $751,600 | 37% |

### 2026 — Single Filers (estimated, pending IRS confirmation)

| Taxable Income | Marginal Rate |
|---|---|
| $0 – $12,200 | 10% |
| $12,201 – $49,500 | 12% |
| $49,501 – $105,600 | 22% |
| $105,601 – $201,500 | 24% |
| $201,501 – $255,800 | 32% |
| $255,801 – $639,800 | 35% |
| Over $639,800 | 37% |

### 2026 — Married Filing Jointly (estimated)

| Taxable Income | Marginal Rate |
|---|---|
| $0 – $24,400 | 10% |
| $24,401 – $99,000 | 12% |
| $99,001 – $211,200 | 22% |
| $211,201 – $403,000 | 24% |
| $403,001 – $511,600 | 32% |
| $511,601 – $767,600 | 35% |
| Over $767,600 | 37% |

> **Note:** 2026 brackets are estimated based on IRS COLA  
> adjustment history. Final figures will be updated when  
> IRS releases Rev. Proc. for 2026.

---

## 7. Phase-Out Worked Examples

### Example A — Fully Eligible (Single, Low Income)

```
Inputs:
  Filing:       Single
  Rate:         $15/hr
  OT hours:     10/week × 48 weeks = 480/year
  MAGI:         $45,000

Step 1 — Premium:
  480 × ($15 × 0.5) = 480 × $7.50 = $3,600

Step 2 — Cap:
  MIN($3,600, $12,500) = $3,600

Step 3 — Phase-out:
  $45,000 < $150,000 → No reduction
  final_deduction = $3,600

Step 4 — Bracket:
  taxable_income = $45,000 − $15,000 = $30,000 → 12% bracket

Step 5 — Savings:
  $3,600 × 12% = $432/year → $36/month → $16.62/biweekly
```

---

### Example B — Cap Applies (Single, High OT)

```
Inputs:
  Filing:       Single
  Rate:         $25/hr
  OT hours:     20/week × 50 weeks = 1,000/year
  MAGI:         $90,000

Step 1 — Premium:
  1,000 × ($25 × 0.5) = 1,000 × $12.50 = $12,500

Step 2 — Cap:
  MIN($12,500, $12,500) = $12,500 ← Exactly at cap

Step 3 — Phase-out:
  $90,000 < $150,000 → No reduction

Step 4 — Bracket:
  taxable_income = $90,000 − $15,000 = $75,000 → 22% bracket

Step 5 — Savings:
  $12,500 × 22% = $2,750/year → $229/month → $105.77/biweekly
```

---

### Example C — Partial Phase-Out (Single, High Income)

```
Inputs:
  Filing:       Single
  Rate:         $35/hr
  OT hours:     15/week × 50 weeks = 750/year
  MAGI:         $185,000

Step 1 — Premium:
  750 × ($35 × 0.5) = 750 × $17.50 = $13,125

Step 2 — Cap:
  MIN($13,125, $12,500) = $12,500

Step 3 — Phase-out:
  excess_magi  = $185,000 − $150,000 = $35,000
  reduction    = FLOOR(35) × $100 = $3,500
  final        = MAX(0, $12,500 − $3,500) = $9,000

Step 4 — Bracket:
  taxable_income = $185,000 − $15,000 = $170,000 → 24% bracket

Step 5 — Savings:
  $9,000 × 24% = $2,160/year → $180/month → $83.08/biweekly
```

---

### Example D — Fully Phased Out (Single, Very High Income)

```
Inputs:
  Filing:       Single
  MAGI:         $290,000
  annual_ot_premium: $12,500 (capped)

Step 3 — Phase-out:
  excess_magi  = $290,000 − $150,000 = $140,000
  reduction    = FLOOR(140) × $100 = $14,000
  final        = MAX(0, $12,500 − $14,000) = $0

Result: No deduction available. Fully phased out.
```

---

### Example E — Married Filing Jointly, 401k Reduces MAGI

```
Inputs:
  Filing:         Married Filing Jointly
  Rate:           $22/hr
  OT hours:       8/week × 50 weeks = 400/year
  Gross income:   $310,000
  401k contrib:   $23,000
  MAGI:           $310,000 − $23,000 = $287,000

Step 1 — Premium:
  400 × ($22 × 0.5) = 400 × $11 = $4,400

Step 2 — Cap:
  MIN($4,400, $25,000) = $4,400

Step 3 — Phase-out:
  excess_magi = $287,000 − $300,000 = −$13,000 → MAX(0) = $0
  No phase-out applies (MAGI is below MFJ threshold)
  final_deduction = $4,400

Step 4 — Bracket:
  taxable_income = $287,000 − $30,000 = $257,000 → 24% bracket

Step 5 — Savings:
  $4,400 × 24% = $1,056/year

Note: Without 401k, MAGI = $310,000. Phase-out would apply:
  excess = $310,000 − $300,000 = $10,000
  reduction = $1,000
  final = $4,400 − $1,000 = $3,400
  savings = $3,400 × 24% = $816/year

401k contribution saved this user an additional $240/year
in overtime deduction alone.
```

---

## 8. Tips Deduction Methodology

The OBBBA created a **separate** deduction for qualified tips,  
independent of the overtime deduction. Both can be claimed  
on the same return.

```
annual_tips       = weekly_tips × weeks_worked
tips_cap          = $12,500  (Single)
                  = $25,000  (Married Filing Jointly)

capped_tips       = MIN(annual_tips, tips_cap)

excess_magi       = MAX(0, MAGI − phase_out_threshold)
tips_reduction    = FLOOR(excess_magi ÷ 1,000) × $100
final_tips_deduct = MAX(0, capped_tips − tips_reduction)

tips_savings      = final_tips_deduct × marginal_rate
```

**Same phase-out thresholds apply** to tips as to overtime:
- Single: starts at $150,000 MAGI
- MFJ: starts at $300,000 MAGI

**The caps are separate.** A single filer can deduct up to  
$12,500 in overtime premium AND up to $12,500 in tips —  
not $12,500 combined.

---

## 9. Combined Overtime + Tips Example

```
Inputs:
  Filing:         Single
  Rate:           $14/hr
  OT hours:       6/week × 50 = 300/year
  Weekly tips:    $120/week × 50 = $6,000/year
  MAGI:           $38,000

Overtime:
  premium         = 300 × $7 = $2,100
  capped          = MIN($2,100, $12,500) = $2,100
  phase-out       = $0 (MAGI below threshold)
  ot_deduction    = $2,100

Tips:
  annual          = $6,000
  capped          = MIN($6,000, $12,500) = $6,000
  phase-out       = $0
  tips_deduction  = $6,000

Total deduction   = $2,100 + $6,000 = $8,100

Bracket:
  taxable_income  = $38,000 − $15,000 = $23,000 → 12% bracket

Total savings     = $8,100 × 12% = $972/year
                  = $81/month
                  = $37.38/biweekly
```

---

## 10. Edge Cases and Boundary Conditions

| Scenario | How Calculator Handles It |
|---|---|
| OT premium exactly equals deduction cap | Full cap amount used — no reduction |
| MAGI exactly at phase-out start | No reduction — threshold is inclusive |
| MAGI exactly at full phase-out | MAX(0) returns $0 — deduction = $0 |
| Partial $1,000 excess MAGI | FLOOR() rounds down — conservative |
| Weeks worked = 52 | Allowed — no validation error |
| OT hours = 0 | Returns $0 savings — no calculation error |
| MAGI = $0 | Treated as eligible, 10% bracket applied |
| 401k reduces MAGI below phase-out | Phase-out does not apply — full deduction |
| Tips = $0 | Tips deduction = $0 — not included in total |
| Double-time pay entered as OT | Calculator cannot detect this — user must enter only FLSA premium |

---

## 11. What This Calculator Does Not Cover

| Exclusion | Reason |
|---|---|
| State income taxes | 50 different state rules — no uniform formula |
| FICA taxes | OBBBA does not affect Social Security or Medicare |
| Alternative Minimum Tax (AMT) | Complex interaction — beyond scope of estimation tool |
| Self-employed overtime equivalent | FLSA does not apply to self-employed |
| Double-time pay | IRS/Treasury has not confirmed eligibility |
| State-only overtime | Federal FLSA coverage required — IRS guidance pending |
| Multi-state workers | Requires state-by-state analysis |
| Tips from non-W2 sources | IRS reporting requirements unclear for cash tips |

---

## 12. Known Limitations and Pending IRS Guidance

As of April 2026, the following areas have not received  
definitive IRS guidance and are handled conservatively  
in this calculator:

**1. Double-time pay**  
Treasury has not confirmed whether the 0.5x premium above  
regular rate applies when double-time is paid. Calculator  
excludes double-time from calculations until clarified.

**2. State-mandated overtime only**  
Some states require overtime payments that are not mandated  
by federal FLSA. Whether state-only overtime qualifies for  
the OBBBA deduction is unresolved. Calculator assumes  
federal FLSA eligibility is required.

**3. W-2 reporting for 2025 tax year**  
Employers were not required to separately report overtime  
premium on W-2 for 2025. Workers may need to calculate  
their own premium amount. IRS interim guidance allows  
self-computed figures with documentation.

**4. 2026 tax brackets**  
IRS has not yet published final 2026 inflation-adjusted  
brackets. Calculator uses estimated figures based on  
historical COLA adjustment patterns. Will be updated  
when IRS Rev. Proc. is published.

If you have a source that resolves any of the above,  
please open an [Issue](https://github.com/notaxcalc/notaxcalculator/issues).

---

## 13. Data Privacy

- All calculations run 100% client-side in the user's browser
- No income, wage, or personal data is transmitted to any server
- No cookies are set by the calculator itself
- No third-party analytics receive calculator input values
- Closing the browser tab clears all entered data permanently

---

## 14. How to Challenge or Correct This Methodology

This document is intentionally open to challenge.  
If you believe any step contains an error:

**Open a GitHub Issue with:**
1. The specific step or formula you believe is incorrect
2. The input values that demonstrate the error
3. The result the calculator returned
4. The result you expected and your source (statute text,  
   IRS publication, or professional guidance)

**What happens next:**
- All issues are reviewed within 48 hours
- If the error is confirmed, the formula and this document  
  are updated and a new version is logged in the Changelog
- If the issue is disputed, reasoning is posted publicly  
  in the issue thread so anyone can evaluate it

No credentials are required to submit a correction.  
The math either holds up or it does not.

---

## 15. Changelog

| Version | Date | Change |
|---|---|---|
| v1.0 | July 2025 | Initial release. OBBBA overtime premium formula. Single + MFJ filing statuses. Basic phase-out logic. |
| v1.1 | July 2025 | Tips deduction added as separate calculation. Combined savings output added. |
| v1.2 | April 2026 | Fake CPA attribution removed. Methodology document published. Tax year selector corrected to 2025–2028 only. Contradictory content removed from live site. 2026 estimated tax brackets added. Edge case table added. |

---

*Maintained by [notaxcalc](https://github.com/notaxcalc)*  
*Live tool: [notaxovertimecalculator.com](https://notaxovertimecalculator.com)*  
*License: MIT (methodology documentation only)*  
*Calculator tool: Copyright 2026 notaxovertimecalculator.com*
```
