# Risk Playbook: Max Target Weight Guardrail

**Status:** Active (v1)  
**Last updated:** 2026-02-23  
**Owner:** Ian Spencer, CFA

## Purpose
Limit concentration risk so one position cannot dominate portfolio outcomes.

## Primary metric
- **Max Target Weight (MTW):** the largest target weight across all positions in the current/provisional rebalance file.

## Guardrail thresholds
- **Green:** MTW <= 45%
- **Yellow:** 45% < MTW <= 60%
- **Red:** MTW > 60%

## Action ladder
### Green (normal)
- Execute targets as-is.
- Keep standard rebalance process.

### Yellow (caution)
- Review concentration before execution.
- Apply soft cap at **45%** unless strong model justification.
- Reallocate excess weight pro-rata to remaining risk assets or defensive sleeve.

### Red (breach)
- Enforce hard cap immediately.
- Maximum single-position target set to **45%**.
- Move excess to diversified sleeves (preferred) or SHY/cash proxy if no suitable sleeves qualify.
- Log breach and override rationale.

## Implementation rule (deterministic)
Given model target weights `w_i`:
1. Cap each at `cap = 0.45`.
2. Compute excess = `sum(max(0, w_i - cap))`.
3. Redistribute excess:
   - first choice: across uncapped risk assets proportional to original weights;
   - fallback: defensive sleeve (SHY/cash) if redistribution breaches caps.
4. Re-normalize to 100%.

## Additional controls
- **Turnover guard:** ignore changes <2% absolute weight unless required for cap compliance.
- **Liquidity sanity check:** avoid adding to low-liquidity sleeves when redistributing excess.
- **Cross-guardrail rule:** if volatility or drawdown is in breach, use the more conservative action set.

## What NOT to do
- Do not allow ad-hoc exceptions above 60%.
- Do not concentrate more in a red volatility/red drawdown regime.
- Do not bypass logging for overrides.

## Operator checklist
- [ ] MTW calculated from latest provisional file
- [ ] Band identified (Green/Yellow/Red)
- [ ] Cap rule applied if needed
- [ ] Excess redistribution reviewed
- [ ] Final weights sum to 100%
- [ ] Decision logged

## Version history
- **v1 (2026-02-23):** Initial max target weight playbook.
