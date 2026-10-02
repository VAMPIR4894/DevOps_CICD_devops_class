# Meat / Leveling Tracker — Rules & State

Reference file. Claude should re-read this before doing any meat/leveling math and
ask for corrections rather than assuming. User corrects this file as the source of truth drifts.

## Game mechanics

- Heroes require meat to level up. Cost per level transition is arithmetic:
  **cost(n→n+1) = 5.9M + 0.1M×(n−300)**
  (e.g. 300→301 = 5.9M, 317→318 = 7.6M, 319→320 = 7.8M, 337→338 = 9.6M)
- Meat income has two tiers:
  - **Floor (guaranteed) rate: 5,585,000/day**
    - 15 purple boxes × 133k = 1,995,000
    - 3 gold boxes × 400k = 1,200,000
    - Trades = 300,000/day (guaranteed)
    - Weekly store: 110 purple boxes × 133k = 14,630,000/week → averages 2,090,000/day
  - **Actual/realistic rate**: includes random mob-kill drops and events — NOT fixed,
    NOT a literal daily deposit. It is a *derived average* (total actual meat spent ÷
    elapsed days), recalculated periodically from real spend. It is never something
    with "leftover" to carry forward day to day — that framing is wrong and was
    corrected (see methodology rules below).
- Cap increases (decor, events, etc.) raise a hero's max level. Any level-ups that
  happen because of a cap increase STILL cost meat via the normal cost table unless
  the user explicitly says a level-up was free. Default assumption: costs meat.

## Methodology rules (hard-learned, do not violate)

1. **Never diff two "total remaining gap" numbers across different points in time**
   if hero scope or level caps changed in between — produces false numbers.
2. To recompute the actual average rate: compute **meat actually spent on actual
   level transitions per hero** (via the cost table), sum across all heroes,
   divide by **elapsed days** (cumulative since the very first checkpoint, not
   just the latest window).
3. The "actual rate" (e.g. 12.0M/day) is an **average derived from historical
   spend**, not a literal fixed daily income. Do NOT treat it like a cash
   balance with "leftover" that carries forward after a purchase — that was a
   mistake made and corrected. Only the floor rate (5,585,000/day guaranteed
   boxes/trades) is a literal daily deposit; the actual rate is a planning
   average only, used to project future days-to-target, not to track real-time
   leftover balances.
4. When the user says "I already did X" about a level-up, treat it as **new
   ground truth that supersedes prior assumptions** — ask for exact current
   levels per hero before recomputing anything, rather than guessing how the
   change happened.
5. If it's ambiguous whether a level-up cost meat (e.g. granted by a decor/cap
   increase), ASK — don't assume free or assume cost without confirming.
6. Before giving a new projection, confirm: (a) current level per hero, (b)
   current cap per hero, (c) today's date / elapsed days since last checkpoint.
   Don't reuse stale numbers from earlier in the conversation without re-confirming
   if the user's phrasing suggests something changed off-screen.

## Hero priority (most → least important)

Paragon > Rose > Artificer > Adjudicator > Lilia > Nun

- This is an **importance ranking**, not necessarily the fastest-completion
  sequence. For "max as many heroes ASAP" questions, consider
  shortest-cost-first (SJF) sequencing separately and present the tradeoff —
  don't just apply the priority order blindly to a scheduling question.

## Heroes (6 total) — original starting levels (Sept 13)

- Paragon: started 308, was maxed at old cap 318 by Sept 29
- Adjudicator: started 308
- Artificer: started 306
- Rose: started 306
- Lilia: started 303
- Nun: started 303

## Level cap history

- Original caps required climbing ~+10 levels from Sept 13 starting points for
  Nun/Lilia (5-hero gap initially tracked: 343.5M, Paragon already maxed).
- First cap increase (+2) applied to all 6 heroes after Sept 29 check-in.
- Second cap increase applied after Oct 2 check-in, final confirmed targets:
  - Paragon, Adjudicator, Artificer → cap 320
  - Rose → cap 319
  - Lilia, Nun → cap 315
- A decor upgrade also increased max level by 2 for all heroes at some point
  around Oct 2 (exact interaction with the above cap increases still being
  reconciled with user — levels/costs still counted via normal cost table).

## Current confirmed state (as of Oct 2, still today — being reconciled)

| Hero | Level (last confirmed) | Cap |
|---|---|---|
| Paragon | 319 (expected to hit 320 / finish today) | 320 |
| Adjudicator | 318 | 320 |
| Artificer | 318 | 320 |
| Rose | 316 | 319 |
| Lilia | 309 (already upgraded once from earlier baseline) | 315 |
| Nun | 303 | 315 |

**Note:** user indicated Paragon's path included a jump involving a decor-driven
cap increase and Lilia had an extra level-up already applied before the Oct 2
checkpoint was given — exact sequencing/levels still need final confirmation
from user. Treat the table above as provisional until user confirms exact
current levels for all 6 heroes in one go.

## Actual spend history (Sept 13 → Oct 2, 19 days) — last fully reconciled baseline

| Hero | Levels gained | Cost (M) |
|---|---|---|
| Paragon | 318→319 | 7.7 |
| Rose | 306→316 | 69.5 |
| Artificer | 306→318 | 84.6 |
| Adjudicator | 308→312 | 27.4 |
| Nun | 303→303 | 0 |
| Lilia | 303→309 | 38.7 |
| **Total spent** | | **227.9M** |

**True average actual rate (last reconciled): 227.9M ÷ 19 days ≈ 12.0M/day**
(This number needs to be recomputed once current levels are reconfirmed.)

## Outstanding items to reconcile with user

- [ ] Exact current level for all 6 heroes, right now, in one message.
- [ ] Whether the decor's +2 cap increase and Lilia's extra level-up affected
      the Sept 13–Oct 2 spend total already counted above, or are new/separate
      from that 227.9M figure.
- [ ] Current cap per hero if anything changed beyond the Oct 2 confirmed caps.
