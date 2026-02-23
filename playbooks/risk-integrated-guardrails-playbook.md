# Risk Playbook: Integrated Guardrails (Drawdown + Volatility + Concentration)

**Status:** Active (v1)  
**Last updated:** 2026-02-23  
**Owner:** Ian Spencer, CFA

## Purpose
Provide one unified decision framework that weighs all three guardrails and outputs a single, executable risk posture.

## Scope
This playbook combines:
1. **Drawdown Guardrail** (capital stress)
2. **Rolling Volatility Guardrail** (regime instability)
3. **Max Target Weight Guardrail** (concentration risk)

## Inputs (daily after close)
- `DD`: Current drawdown from peak equity.
- `VOL63`: 63-day realized annualized volatility.
- `MTW`: Maximum target weight from provisional/current target file.

## Band definitions
### Drawdown (DD)
- Green: `DD > -10%`
- Yellow: `-20% < DD <= -10%`
- Red: `-25% < DD <= -20%`
- Extreme: `DD <= -25%`

### Volatility (VOL63)
- Green: `VOL63 <= 16%`
- Yellow: `16% < VOL63 <= 22%`
- Red: `VOL63 > 22%`
- Extreme: `VOL63 > 30%`

### Concentration (MTW)
- Green: `MTW <= 45%`
- Yellow: `45% < MTW <= 60%`
- Red: `MTW > 60%`

## Risk multipliers by guardrail
- Drawdown: Green `1.00x`, Yellow `0.75x`, Red `0.40x`, Extreme `0.20x`
- Volatility: Green `1.00x`, Yellow `0.85x`, Red `0.70x`, Extreme `0.40x`
- Concentration: Green `1.00x`, Yellow `0.90x`, Red `0.80x` (plus hard cap rules)

## Integration rule (single decision output)
### Step 1 — Compute each guardrail multiplier
Calculate `m_dd`, `m_vol`, `m_mtw` from current bands.

### Step 2 — Use confirmation filters (reduce whipsaw)
- Yellow/Red transitions require **2 consecutive closes** in breach before stepping down risk.
- Extreme transitions (`DD <= -25%` or `VOL63 > 30%`) apply **immediately**.

### Step 3 — Base portfolio multiplier
Use:
`m_base = min(m_dd, m_vol, m_mtw)`

(Conservative by design: the tightest guardrail sets base risk.)

### Step 4 — Apply concentration cap overlay
Regardless of `m_base`, enforce:
- Hard single-position cap = **45%**
- Excess allocation redistributed pro-rata to uncapped sleeves or SHY/cash fallback.

### Step 5 — Extreme override
If either:
- `DD <= -25%`, or
- `VOL63 > 30%`
then set final multiplier `m_final = 0.20x` and use defensive sleeve policy.

### Step 6 — Risk pressure score (for communication)
Compute a simple score for dashboard/investor communication:
- Green=0, Yellow=1, Red=2, Extreme=3
- `pressure = 0.5*DD + 0.3*VOL + 0.2*MTW` (using band scores)

Interpretation:
- `pressure < 0.8` -> Low stress
- `0.8 to <1.6` -> Moderate stress
- `>=1.6` -> High stress

(Use this for messaging; exposure control still uses `m_final`.)

## Final output fields (for dashboard/ops)
- `Drawdown Band`
- `Volatility Band`
- `Concentration Band`
- `Base Multiplier`
- `Final Multiplier`
- `Cap Override Applied (Yes/No)`
- `Recommended Posture` (Normal / Caution / Protection / Defensive)

## Recommended posture mapping
- `m_final >= 0.95` -> **Normal**
- `0.70 <= m_final < 0.95` -> **Caution**
- `0.35 <= m_final < 0.70` -> **Protection**
- `m_final < 0.35` -> **Defensive**

## Re-risk protocol (hysteresis)
To move up one posture level, require:
- 5 consecutive closes where breached guardrails improve by at least one band.
- No new red/extreme breach during that window.
- Move up one level at a time (never jump Defensive -> Normal in one step).

## Execution protocol
1. Evaluate daily after close.
2. Generate provisional scaled targets using `m_final`.
3. Enforce MTW cap (45%) and redistribute excess.
4. Skip micro-adjustments (<2% absolute weight) unless required by hard cap.
5. Log decision state and changes.

## Governance and communication
- Every breach state change requires a one-line log note.
- Red/Extreme states trigger a short “risk status” summary in weekly investor notes.
- Threshold changes require explicit policy update (no ad-hoc intramonth edits).

## Pseudocode
```text
m_dd  = map_drawdown_to_multiplier(DD)
m_vol = map_vol_to_multiplier(VOL63)
m_mtw = map_mtw_to_multiplier(MTW)

m_base = min(m_dd, m_vol, m_mtw)

if DD <= -0.25 or VOL63 > 0.30:
    m_final = 0.20
else:
    m_final = m_base

targets_scaled = targets_raw * m_final
targets_capped = apply_single_name_cap(targets_scaled, cap=0.45)
```

## Operator checklist
- [ ] DD, VOL63, MTW updated
- [ ] Bands identified for all 3 guardrails
- [ ] `m_final` computed
- [ ] 45% cap applied and validated
- [ ] Final targets sum to 100%
- [ ] Decision logged with timestamp

## Version history
- **v1 (2026-02-23):** Initial integrated guardrails framework.
