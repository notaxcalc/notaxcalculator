# Calculation Formula Reference

**Calculator:** No Tax on Overtime Calculator  
**Live Tool:** https://notaxovertimecalculator.com/  
**Formula status:** Public documentation for review  
**Current focus:** Tax Year 2026

## 1. Inputs

The simplified calculator accepts:

- `filing_status`
- `regular_hourly_rate`
- `overtime_hours`
- `overtime_multiplier`
- `magi_before_overtime`
- `tax_year`

The public calculator constrains the overtime multiplier to a reasonable range and uses 1.5x as the standard FLSA overtime example.

## 2. Total overtime pay

```text
total_overtime_pay =
    regular_hourly_rate × overtime_hours × overtime_multiplier
```

Example:

```text
regular rate = $20
overtime hours = 100
multiplier = 1.5

total overtime pay = $20 × 100 × 1.5
                   = $3,000
```

## 3. Qualified overtime premium

For the calculator's simplified estimate, the qualified premium is based on the FLSA-style half-time premium:

```text
qualified_overtime =
    regular_hourly_rate × overtime_hours × 0.5
```

For a 1.5x overtime rate:

```text
$20 × 100 × 0.5 = $1,000
```

The calculator does not treat the full $3,000 overtime paycheck as qualified.

For higher overtime multipliers, the calculator continues to use the 0.5x regular-rate premium for this simplified estimate. Complex compensation arrangements should be checked against payroll records and applicable IRS guidance.

## 4. Deduction cap

```text
if filing_status is Single or Head of Household:
    cap = $12,500

if filing_status is Married Filing Jointly:
    cap = $25,000

capped_deduction = MIN(qualified_overtime, cap)
```

## 5. Phaseout

The simplified phaseout is:

```text
Single / Head of Household threshold = $150,000
Married Filing Jointly threshold     = $300,000
```

For each $1,000 above the applicable threshold, the deduction is reduced by $100.

```text
excess_magi = MAX(0, estimated_magi - threshold)

phaseout_reduction =
    FLOOR(excess_magi / 1000) × 100

final_deduction =
    MAX(0, capped_deduction - phaseout_reduction)
```

## 6. Estimated MAGI handling

The calculator is not a complete MAGI calculator.

The user enters an estimated MAGI amount before overtime. For phaseout purposes, the implementation may incorporate estimated overtime pay into the modeled income used by the calculator.

Therefore:

```text
estimated_magi =
    user_estimated_magi_before_overtime
    + estimated_overtime_pay
```

This is a modeling assumption, not a substitute for the MAGI calculation on an actual federal tax return.

Users should use their actual tax documents and tax-return calculations when filing.

## 7. Federal tax savings estimate

The calculator estimates the tax benefit by applying an estimated marginal federal tax rate to the final deduction:

```text
estimated_federal_tax_savings =
    final_deduction × estimated_marginal_rate
```

This is an estimate, not a guarantee of the exact reduction in final federal tax liability.

## 8. 2026 federal marginal-rate inputs

The calculator's 2026 presets use these federal income-tax brackets.

### Single

| Taxable income | Rate |
|---|---:|
| $0 - $12,400 | 10% |
| $12,400 - $50,400 | 12% |
| $50,400 - $105,700 | 22% |
| $105,700 - $201,775 | 24% |
| $201,775 - $256,225 | 32% |
| $256,225 - $640,600 | 35% |
| Over $640,600 | 37% |

### Married Filing Jointly

| Taxable income | Rate |
|---|---:|
| $0 - $24,800 | 10% |
| $24,800 - $100,800 | 12% |
| $100,800 - $211,400 | 22% |
| $211,400 - $403,550 | 24% |
| $403,550 - $512,450 | 32% |
| $512,450 - $768,700 | 35% |
| Over $768,700 | 37% |

### Head of Household

| Taxable income | Rate |
|---|---:|
| $0 - $17,700 | 10% |
| $17,700 - $67,450 | 12% |
| $67,450 - $105,700 | 22% |
| $105,700 - $201,750 | 24% |
| $201,750 - $256,200 | 32% |
| $256,200 - $640,600 | 35% |
| Over $640,600 | 37% |

The 2026 standard-deduction presets are:

- Single: $16,100
- Married Filing Jointly: $32,200
- Head of Household: $24,150

## 9. Worked example

Assume:

```text
Filing status: Single
Regular rate: $20/hour
Overtime: 200 hours
Multiplier: 1.5x
Estimated MAGI before overtime: $80,000
```

Qualified overtime estimate:

```text
$20 × 200 × 0.5 = $2,000
```

The $2,000 amount is below the $12,500 individual cap.

If the modeled MAGI remains below the phaseout threshold, the estimated qualified overtime deduction is:

```text
$2,000
```

The final estimated federal tax savings depends on the modeled marginal tax rate.

## 10. What is not included

The formula does not attempt to calculate:

- state income taxes;
- Social Security taxes;
- Medicare taxes;
- payroll withholding refunds;
- a complete federal tax return;
- every MAGI adjustment;
- every FLSA overtime exception;
- every special compensation arrangement;
- employer payroll corrections.

## 11. Reporting versus estimating

For 2026, the employer-reported qualified overtime amount should be given priority when preparing a federal tax return.

The calculator is intended for planning and estimation. It should not be used to override W-2 reporting or other official tax documents without appropriate professional review.
