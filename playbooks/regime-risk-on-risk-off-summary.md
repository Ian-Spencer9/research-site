# Regime Summary: Risk-On vs Risk-Off

**Status:** Active (v1)  
**Last updated:** 2026-02-23  
**Owner:** Ian Spencer, CFA

## Why this matters
Regime state is your top-level market context filter. It helps decide whether to seek growth exposure (**Risk-On**) or prioritize capital preservation (**Risk-Off**).

## Simple definition
- **Risk-On:** Market trend supports taking systematic risk.
- **Risk-Off:** Market trend is defensive; preserve capital first.

## Current model logic (dashboard/system)
The strategy uses an SPY trend filter:
- Compute SPY vs its 150-day moving average.
- Evaluate at month-end (execution framework).
- If SPY is above MA -> regime is **Risk-On**.
- If SPY is below MA -> regime is **Risk-Off**.

## Practical implications
### Risk-On
- Strategy can allocate to momentum-selected risk assets.
- Portfolio targets are formed from top-ranked eligible ETFs.
- Normal strategy participation expected.

### Risk-Off
- Strategy shifts defensive (cash proxy / short-duration Treasury sleeve, e.g., SHY).
- Objective is to reduce downside participation during adverse trends.
- Return expectations are lower, but drawdown control improves.

## How this interacts with guardrails
Regime is context; guardrails are risk brakes.

- If regime is Risk-On but guardrails are stressed (Yellow/Red), you still de-risk per guardrails.
- If regime is Risk-Off, stay defensive even if some guardrails look benign.
- In conflicts, use the **more conservative** action.

## What to check on dashboard
1. Regime Status (Risk-On / Risk-Off)
2. Guardrails:
   - Current Drawdown
   - Rolling Vol (63d)
   - Max Target Weight
3. Integrated mode decision:
   - Normal / Caution / Protection

## Operator quick instructions
- **Risk-On + all Green:** run model normally.
- **Risk-On + any Yellow/Red:** run Caution/Protection actions.
- **Risk-Off:** prioritize defensive posture and capital preservation.
- Log one line: date, regime, worst guardrail, mode, action.

## Common mistakes to avoid
- Treating Risk-On as “ignore risk controls.”
- Flipping discretionary risk exposure based on headlines.
- Overriding Risk-Off without explicit policy reason.

## Helpful interpretation note
Regime filters are not about calling tops/bottoms perfectly. They are about improving behavior during broad trend deterioration and avoiding large left-tail outcomes.

## Version history
- **v1 (2026-02-23):** Initial regime summary and operating guidance.
