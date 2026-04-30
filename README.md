# No Tax on Overtime Calculator — Formula & Methodology

**Live Tool:** [notaxovertimecalculator.com](https://notaxovertimecalculator.com)
**Formula:** Publicly auditable — open for review by any tax professional
**Maintained by:** notaxovertimecalculator.com
**Formula status:** Open for review — no credentials claimed
**Last reviewed:** April 2026
**Last Updated:** April 2026
**Law:** One Big Beautiful Bill Act (OBBBA), signed July 4, 2025
**Applies to:** Tax Years 2025 – 2028
**Last Updated:** July 2025

## Documentation
- [FORMULA.md](./FORMULA.md) — Core formula and pseudocode
- [METHODOLOGY.md](./METHODOLOGY.md) — Full step-by-step methodology, 
  worked examples, edge cases, and known limitations
- [CHANGELOG.md](./CHANGELOG.md) — Version history

---

## What This Repository Contains

This repository documents the exact formula, logic, and calculation methodology used by the [No Tax on Overtime Calculator](https://notaxovertimecalculator.com). It is published openly so that:

- Users can verify the math independently
- Tax professionals can audit the logic against the statute
- Developers can review and report errors
- The public can trust the results are formula-driven, not estimated

All calculation logic runs **client-side in the user's browser**. No income data is ever transmitted, stored, or logged.

---

## Legal Basis

The No Tax on Overtime deduction is established under the **One Big Beautiful Bill Act (OBBBA)**, signed into law on July 4, 2025.

| Provision | Detail |
|---|---|
| Deduction type | Above-the-line federal income tax deduction |
| Qualifying income | FLSA overtime premium pay (0.5x regular rate only) |
| Deduction cap — Single | $12,500 per tax year |
| Deduction cap — Married Filing Jointly | $25,000 per tax year |
| Phase-out start — Single | $150,000 MAGI |
| Phase-out start — Married Filing Jointly | $300,000 MAGI |
| Full elimination — Single | $275,000 MAGI |
| Full elimination — Married Filing Jointly | $550,000 MAGI |
| Effective tax years | 2025, 2026, 2027, 2028 |
| Expiration | December 31, 2028 (unless renewed by Congress) |

---

## Core Formula

### Step 1 — Calculate the Overtime Premium

Only the **premium half** of time-and-a-half pay qualifies. Not the full overtime paycheck.

```
overtime_premium = overtime_hours x (regular_hourly_rate x 0.5)
```

**Example:**
Regular rate = $20/hr | Overtime hours = 200
overtime_premium = 200 x ($20 x 0.5) = 200 x $10 = $2,000

---

### Step 2 — Apply the Deduction Cap

```
capped_deduction = MIN(overtime_premium, deduction_cap)
```

Where:
- deduction_cap = $12,500 for Single filers
- deduction_cap = $25,000 for Married Filing Jointly

---

### Step 3 — Apply the Phase-Out Reduction

```
excess_magi     = MAX(0, MAGI - phase_out_threshold)
reduction       = FLOOR(excess_magi / 1000) x 100
final_deduction = MAX(0, capped_deduction - reduction)
```

Where:
- phase_out_threshold = $150,000 (Single) or $300,000 (MFJ)
- Reduction rate = $100 per $1,000 of MAGI above threshold
- Deduction cannot go below $0

**Example — Single filer with $165,000 MAGI:**
```
excess_magi     = $165,000 - $150,000 = $15,000
reduction       = FLOOR(15,000 / 1,000) x 100 = 15 x $100 = $1,500
final_deduction = $12,500 - $1,500 = $11,000
```

---

### Step 4 — Estimate Federal Tax Savings

```
estimated_savings = final_deduction x marginal_tax_rate
```

**2025 Tax Brackets — Single:**

| MAGI Range | Rate |
|---|---|
| $0 – $11,925 | 10% |
| $11,926 – $48,475 | 12% |
| $48,476 – $103,350 | 22% |
| $103,351 – $197,300 | 24% |
| $197,301 – $250,525 | 32% |
| $250,526 – $626,350 | 35% |
| Over $626,350 | 37% |

**2025 Tax Brackets — Married Filing Jointly:**

| MAGI Range | Rate |
|---|---|
| $0 – $23,850 | 10% |
| $23,851 – $96,950 | 12% |
| $96,951 – $206,700 | 22% |
| $206,701 – $394,600 | 24% |
| $394,601 – $501,050 | 32% |
| $501,051 – $751,600 | 35% |
| Over $751,600 | 37% |

Note: The calculator applies the standard deduction ($15,000 single / $30,000 MFJ for 2025) before bracket lookup.

---

### Step 5 — Tips Deduction (If Applicable)

```
tips_deduction = MIN(weekly_tips x 52, tips_deduction_cap)
tips_savings   = tips_deduction x marginal_tax_rate
```

Tips and overtime deductions are **separate and additive** — both can be claimed on the same return.

---

## Full Pseudocode

```
FUNCTION calculate(filing_status, weekly_base_pay, weekly_overtime_premium, weekly_tips):

  annual_base             = weekly_base_pay x 52
  annual_overtime_premium = weekly_overtime_premium x 52
  annual_tips             = weekly_tips x 52
  magi                    = annual_base

  IF filing_status == "single":
    ot_cap             = 12500
    phase_out_start    = 150000
    standard_deduction = 15000
  ELSE IF filing_status == "married_filing_jointly":
    ot_cap             = 25000
    phase_out_start    = 300000
    standard_deduction = 30000

  capped_ot          = MIN(annual_overtime_premium, ot_cap)
  excess_magi        = MAX(0, magi - phase_out_start)
  reduction          = FLOOR(excess_magi / 1000) x 100
  final_ot_deduction = MAX(0, capped_ot - reduction)
  taxable_income     = MAX(0, magi - standard_deduction)
  marginal_rate      = GET_MARGINAL_RATE(taxable_income, filing_status)
  ot_savings         = final_ot_deduction x marginal_rate
  tips_savings       = MIN(annual_tips, ot_cap) x marginal_rate

  RETURN {
    weekly_savings  : (ot_savings + tips_savings) / 52,
    monthly_savings : (ot_savings + tips_savings) / 12,
    yearly_savings  : ot_savings + tips_savings,
    ot_deduction    : final_ot_deduction,
    marginal_rate   : marginal_rate
  }
```

---

## What This Calculator Does NOT Cover

| Exclusion | Reason |
|---|---|
| State income taxes | State rules vary; no uniform standard applies |
| FICA taxes (Social Security & Medicare) | The OBBBA deduction does not affect FICA |
| Self-employed / 1099 workers | FLSA overtime does not apply to independent contractors |
| Salaried exempt employees | Exempt employees do not receive FLSA overtime |
| State-mandated overtime only | IRS guidance on state-only OT is pending |
| Double-time pay | Treasury has not clarified if double-time premium qualifies |

---

## Data Privacy

- All calculations run 100% client-side in the user's browser
- No income, wage, or personal data is transmitted to any server
- No cookies are set by the calculator
- No third-party analytics receive any calculator input data

---

## Changelog

| Version | Date | Change |
|---|---|---|
| v1.0 | July 2025 | Initial release. OBBBA formula. Single + MFJ filing statuses. |
| v1.1 | July 2025 | Tips deduction added as separate calculation. |

---

## Reporting Errors

If you believe the formula contains an error, open a [GitHub Issue](https://github.com/notaxcalc/no-tax-on-overtime-calculator/issues) with:

1. The input values you used
2. The result the calculator returned
3. The result you expected and your source

All reported issues are reviewed within 48 hours.

---

## License

Formula and methodology documentation: [MIT License](LICENSE)
Live calculator tool: Copyright 2025 notaxovertimecalculator.com — All rights reserved

---

*Maintained by [notaxcalc](https://github.com/notaxcalc) | Live tool: [notaxovertimecalculator.com](https://notaxovertimecalculator.com)*
