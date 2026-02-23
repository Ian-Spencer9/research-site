# Risk Playbook: Integrated Guardrails (Lite)

**Status:** Active (v2 - simplified)  
**Last updated:** 2026-02-23  
**Owner:** Ian Spencer, CFA

## Purpose
Give one simple, no-math operating process that combines all 3 guardrails.

## The 3 guardrails to read on dashboard
1. Current Drawdown
2. Rolling Vol (63d)
3. Max Target Weight

## What to do (no formulas)

### Step 1) Check colors
Read each guardrail color: **Green / Yellow / Red**.

### Step 2) Use the worst color
- If **any guardrail is Red** -> **Protection Mode**
- Else if **any guardrail is Yellow** -> **Caution Mode**
- Else (all Green) -> **Normal Mode**

### Step 3) Execute the mode
- **Normal Mode:** run model targets as-is.
- **Caution Mode:** reduce aggressiveness; no discretionary risk adds.
- **Protection Mode:** de-risk and prioritize capital preservation.

### Step 4) Log one line
Record:
- Date
- Worst guardrail
- Mode selected
- Action taken (trade / hold / de-risk)

## Mode definitions

### Normal Mode (all guardrails Green)
**Goal:** maximize participation per model.
- Execute model targets as-is.
- Standard rebalance cadence.
- No extra defensive overlays.
- Routine monitoring only.

### Caution Mode (any Yellow, none Red)
**Goal:** stay invested while reducing risk appetite.
- Follow model direction.
- Reduce aggressiveness (lighter sizing / tighter risk).
- No discretionary risk adds.
- Increase monitoring and prepare to step into Protection if Red appears.

### Protection Mode (any Red)
**Goal:** capital preservation first.
- De-risk promptly.
- Prioritize downside control over upside capture.
- Favor defensive sleeve (e.g., SHY/cash proxy) as needed.
- Execute risk-reducing changes first until conditions improve.

## Optional practical defaults
If you want fixed sizing guidance:
- **Normal:** 1.00x
- **Caution:** 0.75x
- **Protection:** 0.40x

(These are optional; you can operate with mode labels only.)

## Operator checklist
- [ ] Read all 3 guardrail colors
- [ ] Pick worst color
- [ ] Apply matching mode
- [ ] Log one-line decision

## Version history
- **v2 (2026-02-23):** Replaced full framework with Lite, no-math operator version.
- **v1 (2026-02-23):** Full integrated framework.
