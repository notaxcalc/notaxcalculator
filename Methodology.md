# Methodology - No Tax on Overtime Calculator

**Live Tool:** https://notaxovertimecalculator.com/  
**Current focus:** Tax Year 2026  
**Status:** Public methodology documentation

## 1. Purpose

This document explains the calculation approach used by the No Tax on Overtime Calculator.

The calculator is a simplified planning tool. It estimates the potential federal income-tax benefit associated with qualified overtime compensation. It does not attempt to reproduce every calculation performed on an IRS tax return.

The methodology separates:

1. overtime pay;
2. qualified overtime premium;
3. deduction cap;
4. MAGI phaseout;
5. estimated federal tax savings.

## 2. Legal concept

The federal overtime deduction is tied to qualified overtime compensation required under the Fair Labor Standards Act.

For a standard time-and-a-half arrangement, the amount above the regular rate is generally the 0.5x premium portion.

This does not mean the worker's full overtime paycheck is exempt from federal income tax.

Overtime compensation can remain subject to federal withholding and employment taxes even when a qualified overtime deduction may reduce federal taxable income.

## 3. Why the calculator is an estimate

A real federal tax return contains information that cannot be inferred from overtime hours and an hourly rate alone.

Actual MAGI can be affected by many income and adjustment items. A taxpayer may also have credits, deductions, filing circumstances, and other tax attributes that change the final liability.

The calculator therefore uses a simplified income model and an estimated marginal tax rate.

Results should be described as estimates.

## 4. Filing statuses

### Single

Maximum qualified overtime deduction:

```text
$12,500
```

Phaseout begins at:

```text
$150,000 MAGI
```

### Head of Household

Maximum qualified overtime deduction:

```text
$12,500
```

Phaseout begins at:

```text
$150,000 MAGI
```

### Married Filing Jointly

Maximum qualified overtime deduction:

```text
$25,000
```

Phaseout begins at:

```text
$300,000 MAGI
```

Married Filing Separately is not treated as an eligible public calculator status.

## 5. Overtime premium model

For a typical 1.5x overtime rate:

```text
regular rate = 100%
overtime rate = 150%
qualified premium = 50%
```

Therefore, if the regular rate is $24/hour:

```text
regular portion = $24
premium portion = $12
overtime rate    = $36
```

The calculator uses the $12 premium component for its qualified-overtime estimate, not the full $36.

## 6. Higher overtime multipliers

The calculator may allow overtime multipliers above 1.5x for planning purposes.

However, the simplified qualified-overtime calculation continues to use:

```text
0.5 × regular rate
```

for the qualified premium estimate.

This avoids assuming that every dollar above a 1.5x rate automatically qualifies for the federal deduction.

Users with special overtime arrangements should rely on employer reporting and applicable tax guidance.

## 7. Phaseout model

The simplified phaseout reduces the modeled deduction by $100 for each complete $1,000 of MAGI above the applicable threshold.

Example:

```text
Single filer
MAGI = $165,000

Excess = $15,000
Reduction = 15 × $100
Reduction = $1,500
```

If the capped qualified overtime amount is $5,000:

```text
Final deduction = $5,000 - $1,500
                = $3,500
```

The deduction cannot become negative.

## 8. W-2 Code TT

For 2026, employer reporting of qualified overtime compensation is important.

The calculator should not present its own estimate as a replacement for the amount shown on the taxpayer's official tax documents.

If the taxpayer's W-2 appears incorrect, the appropriate first step is to contact the employer or payroll department and ask about correction procedures.

## 9. Federal-only scope

The calculator estimates federal income-tax savings.

It does not calculate:

- state income-tax savings;
- local income-tax savings;
- Social Security savings;
- Medicare savings;
- unemployment-tax effects.

## 10. FLSA scope

The calculator does not determine whether an employee is covered by the FLSA or whether the employee is exempt.

FLSA classification depends on facts such as employment status, job duties, employer coverage, salary basis, and applicable exemptions.

## 11. Known limitations

### MAGI

The input is an estimate rather than a complete IRS MAGI calculation.

### Tax brackets

The calculator uses year-specific federal bracket presets. A future tax-year update may require new brackets and standard-deduction values.

### Payroll data

The calculator does not have access to an employer's payroll system.

### Complex overtime

Multiple rates, commissions, bonuses, special premiums, collective bargaining agreements, and other compensation can require a more detailed calculation.

### Tax liability

The estimated savings shown by the calculator are not a guarantee of the amount a taxpayer will receive as a refund or the exact reduction in tax due.

## 12. Design principle

The methodology favors transparent assumptions over unsupported precision.

Where the real tax rules require information the calculator does not have, the output should be labeled as an estimate rather than presented as an exact IRS result.
