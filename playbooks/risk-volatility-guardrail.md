# Risk Playbook: Volatility Guardrail

**Status:** Active (v1)  
**Last updated:** 2026-02-23  
**Owner:** Ian Spencer, CFA

## Purpose
Protect capital during unstable regimes by scaling exposure systematically when realized volatility rises.

## Inputs
- Rolling realized volatility (63 trading days), annualized.
- Current drawdown.

## Volatility thresholds
- **Green:** <= 16%
- **Yellow:** 16% to 22%
- **Red:** > 22%

## Risk mode ladder (target risk multiplier)
- **Green:** 1.00x (normal target weights)
- **Yellow:** 0.85x
- **Red day 1-5:** 0.70x
- **Red day 6-10:** 0.50x
- **Red >10 days OR drawdown < -10%:** 0.30x
- **Extreme override (vol >30% OR drawdown < -20%):** defensive sleeve (SHY/cash-heavy)

## Re-risk rules (hysteresis)
- 5 consecutive closes <20% vol -> move up one risk step.
- 5 consecutive closes <18% vol -> move up next step.
- 10 consecutive closes <16% vol -> return to 1.00x.

## Execution protocol
1. Evaluate daily after close.
2. Apply changes at next trade cycle (unless extreme override trigger).
3. Ignore trivial weight diffs (<2%) to reduce churn.
4. Log every risk-mode change with timestamp and reason.

## What NOT to do
- Do not discretionary override signal direction.
- Do not jump from red to full risk in one day.
- Do not change thresholds intramonth unless policy review approved.

## Operator checklist
- [ ] Current vol band identified (Green/Yellow/Red)
- [ ] Current drawdown checked
- [ ] Correct risk multiplier selected
- [ ] Provisional weights scaled and reviewed
- [ ] Change logged
