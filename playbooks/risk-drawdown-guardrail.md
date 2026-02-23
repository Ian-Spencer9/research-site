# Risk Playbook: Drawdown Guardrail

**Status:** Active (v1)  
**Last updated:** 2026-02-23  
**Owner:** Ian Spencer, CFA

## Purpose
Limit portfolio damage during adverse regimes while preserving the core systematic process.

## Why this exists
Drawdown is the cleanest investor-facing risk metric: it measures current pain from prior peak equity. Institutional risk frameworks commonly pair drawdown limits with volatility and risk-budget controls.

## Primary metric
- **Current Drawdown (DD):** `equity / rolling_peak - 1`
- Compute daily after close using strategy equity curve.

## Drawdown thresholds (policy)
- **Green:** DD > -10%
- **Yellow:** -10% >= DD > -20%
- **Red:** DD <= -20%

## Action ladder (mechanical)
### Green (normal mode)
- Risk multiplier: **1.00x**
- Execute model targets as usual.

### Yellow (caution mode)
- Risk multiplier: **0.75x**
- No discretionary risk increases.
- Require confirmation check: data freshness + signal integrity + regime state.

### Red (protection mode)
- Risk multiplier: **0.40x**
- Shift residual sleeve to defensive asset (e.g., SHY/cash proxy).
- Freeze non-essential turnover; execute only risk-reducing rebalances.
- Trigger formal review note for audit trail.

### Extreme override
- If DD <= -25% OR concurrent red volatility regime persists >10 trading days:
  - Risk multiplier: **0.20x** (or defensive sleeve policy)
  - Preserve capital first; only re-risk via explicit recovery criteria below.

## Re-risk rules (hysteresis)
Avoid instant jump back to full risk:
1. DD improves above **-18%** for 5 consecutive closes -> move Red->Yellow sizing.
2. DD improves above **-10%** for 10 consecutive closes -> move Yellow->Green sizing.
3. Require volatility guardrail not red for final step back to 1.00x.

## Execution protocol
1. Evaluate once daily after close.
2. Apply changes at next trade cycle unless extreme override is triggered.
3. Ignore tiny weight changes (<2%) to reduce churn/cost.
4. Log every mode change: timestamp, DD, risk multiplier, reason.

## Interaction with volatility guardrail
- If **both** drawdown and volatility are in breach, use the **more conservative** multiplier.
- Priority order: Extreme override > Red > Yellow > Green.

## What NOT to do
- Do not discretionary override model direction based on emotions/news.
- Do not re-risk in one step after a drawdown event.
- Do not modify thresholds ad hoc intra-month.

## Operator checklist
- [ ] Current DD value recorded
- [ ] Correct DD band identified (Green/Yellow/Red/Extreme)
- [ ] Risk multiplier selected and applied to provisional targets
- [ ] Volatility guardrail cross-check complete
- [ ] Decision logged

## Research notes (high-level)
- Trend-following and volatility-managed literature consistently supports systematic de-risking in stress regimes and gradual re-risking to reduce left-tail outcomes/whipsaw.
- Practical institutional implementations generally combine drawdown targets with realized-vol controls and explicit governance triggers.

## Version history
- **v1 (2026-02-23):** Initial playbook with thresholds aligned to dashboard policy.
