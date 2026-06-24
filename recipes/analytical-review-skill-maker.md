# Forge your analytical review skill

> Build a skill that interrogates your numbers each period: the variances, the inconsistencies, the trends, and the questions a sharp reviewer would ask before someone else does.

**Kind:** Custom-skill. It does not run one review; it forges a recurring analytical review skill you own and run every period (board prep, monthly management review, an IC memo).

## The problem

Analytical review is the same exercise every cycle: line the numbers up against budget, forecast and prior periods, work out what moved and why, catch what does not reconcile, read the trend underneath the month, and surface the questions before the board or the partner asks them. Done by hand it is grind redone from scratch each period, and its rigor depends on the hours you had that week. The risk is twofold: walking in with a number you cannot defend, or missing the inconsistency someone else catches in the room.

The fix is not to write the review faster. It is to forge a skill that does the arithmetic and the checks identically every time, and reserves your judgment for interpretation.

## The prompt

Point the agent at your latest figures, wherever they live: an Excel or Sheets file you attach, a Layerz model it reads over MCP, or another connected source. Include actuals plus budget/forecast, a few prior periods if you have them, and your current review file if one exists. Then paste:

```
You are an exacting analytical reviewer (think board-grade FP&A or a demanding
partner). Help me forge a reusable skill I will run every period to produce an
analytical review of my financials. I own the skill and must be able to read and
audit it. Do not optimize for one-off output; optimize for a process that runs the
same way every period and is honest about what it cannot conclude.

1. Restate, step by step, what the skill must do, and flag anything ambiguous or
   implicit before you build it.
2. Variance engine (deterministic): for every line, compute actual vs budget,
   vs forecast and vs prior period, in absolute and %, and flag the ones above a
   materiality threshold I set. This is arithmetic, put it in explicit code or
   fixed steps, not free-form reasoning.
3. Driver decomposition: for each material variance, break it into drivers where
   the data allows, choosing the right lens per line, e.g.
     - revenue: price x volume (x mix)
     - personnel: headcount x average cost (x role mix)
     - other costs: rate x quantity, or one-off vs recurring
   Where a line cannot be decomposed from the data I gave, say so rather than
   invent a split.
4. Consistency and reconciliation checks: budget + variance = actual on every
   line; line items sum to their subtotals and to the total; no line silently
   appears or disappears versus the prior period; cross-statement ties where they
   apply (net income to equity, cash movement to closing cash); flag sign flips
   and ratios that fall outside sane bounds.
5. Trend read: over the last few periods, describe the trajectory of the key lines
   and metrics, direction, inflection points, anything accelerating or decelerating,
   and separate a genuine trend from a one-off.
6. Questions to raise: list the 5-10 questions a skeptical reviewer would ask,
   ranked by materiality, each tied to the figure that prompts it. Raise events,
   do not fill gaps: flag anything you cannot reconcile or explain instead of
   guessing a number.
7. Output, each run: a short review memo, the flags (variances and
   inconsistencies), the trends, the ranked questions, and the decisions they
   call for, headline items first, not the full grid. Then give me the skill
   itself plus a one-line changelog of what changed and why.

Be critical. If a step is fragile, or a driver split or a trend is not supported
by the data, say so directly.
```

Two principles do the work: **code for the arithmetic, judgment for the story** (variances, bridges and reconciliation are deterministic and belong in code that runs identically every period; the interpretation is yours), and **raise events, do not fill gaps** (a board-grade reviewer flags what does not reconcile; an eager assistant quietly invents a plausible number).

## Where it breaks

The skill lives in a file on one machine. It does not remember prior periods, so "read the trend" and "no line disappeared since last period" are only as good as the snapshots you happened to paste. The driver bridges silently freeze computed values into hardcoded numbers, and there is no version history when a figure is restated between drafts. The next time the same review is due, you are reconciling pasted snapshots by hand again.

When you hit that, reach for:
- [`FINANCE.md`](https://github.com/layerzlabs/finance-md) to pin what the skill assumes: your line taxonomy, what counts as "material," sign convention, the per-line driver lens and the ratios that matter, so it stops living in your head.
- [Layerz](https://layerz.cc) (via [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp)) to hold budget, forecast and actuals as a structured, versioned model the agent reads and writes, so period-over-period trend and consistency checks run against real state, diffable across drafts, instead of a pasted snapshot.

## Notes and variations

- The board pack is just one application; the same skill serves a monthly management review, an investment-committee memo, or due diligence on your own model. Tune the materiality threshold and the audience of the memo per use.
- It runs wherever your numbers live: an Excel or Sheets file, a Layerz model the agent reads over MCP, or another MCP-connected source. The skill is the same; only the read/write surface changes. The more structured and persistent the source, the more of the consistency and trend checks survive between periods (see *Where it breaks*).
- The payroll headcount x average cost bridge and the revenue price x volume split are just instances of step 3, not the point. Record the per-line lens in `FINANCE.md` once and reuse it every cycle.
- Second pass: fork the conversation and have a fresh Claude, persona "skeptical board member," try to find the one number that will not survive a question. Fresh context catches what the builder rationalized away.
- Pair with a backup discipline: version the working file each period. Native undo is not a safety net.

---

*Distilled from recurring board and management-review practice. Pending its first real run before it earns a permanent place in the catalog, per the repo's "a recipe nobody has run does not belong here" rule.*
