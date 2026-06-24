# Forge your analytical review skill

> Build a skill that reviews your numbers like a business partner would: it understands the business first, asks the questions, then explains what moved, what does not add up, and what to do about it.

**Kind:** Custom-skill. It does not run one review; it forges a recurring analytical review skill you own and run every period (board prep, monthly management review, an IC memo).

## The problem

Analytical review is the same exercise every cycle: compare to budget, forecast and prior, find what moved and why, catch what does not reconcile, read the trend, and surface the questions before the board asks them. Done by hand it is grind redone from scratch each period. And an agent left to its own devices does the opposite of what you need: it computes every variance and understands none of them, because it never grasped the business in the first place. A variance is not an insight. "Revenue is 12% below budget" means nothing until you know whether a launch slipped, a client churned, or the budget was always optimistic.

The fix is to forge a skill that understands the business before it measures it, does the arithmetic deterministically, and spends its judgment on what the numbers actually mean.

## The prompt

Point the agent at your latest figures, wherever they live: an Excel or Sheets file you attach, a Layerz model it reads over MCP, or another connected source. Include actuals plus budget/forecast, a few prior periods if you have them, and your current review file if one exists. Then paste:

```
You are my analytical review partner, not a calculator. Think like a sharp FP&A
business partner sitting next to the CFO: your job is to understand the business,
put the numbers in perspective, and tell me what matters, not to dump every
variance. Help me forge a reusable skill I will run every period. I own it and
must be able to read and audit it.

Start in context-first mode. Do NOT produce any review or table yet.

1. Step back and interview me until you genuinely understand the business, before
   you judge a single number. Ask, in small batches and following up where my
   answers are thin: what the company does and how it makes money; its stage and
   size; the few drivers that actually move the P&L; what changed this period
   (launches, hires, churn, pricing, one-offs); who this review is for and what
   they care about; what "good" looks like here; and how the budget and forecast
   were built (and how much to trust them). Then restate your understanding in a
   few lines and ask me to correct it.
2. Ask what other material would sharpen your read, and why: prior board packs or
   minutes, the ops or commercial dashboard, budget assumptions, a FINANCE.md,
   last period's commentary. Tell me which one or two would change your reading
   the most, so I know what is worth digging out.

Only once we have done that, design the recurring skill so that each run:

3. Variance engine (deterministic): for every line, compute actual vs budget,
   vs forecast and vs prior period, in absolute and %, and flag the ones above a
   materiality threshold I set. This is arithmetic; put it in explicit code or
   fixed steps, not free-form reasoning.
4. Driver decomposition: for each material variance, break it into drivers where
   the data allows, choosing the right lens per line (revenue: price x volume;
   personnel: headcount x average cost; other costs: rate x quantity, or one-off
   vs recurring). Where a line cannot be decomposed from the data, say so.
5. Consistency and reconciliation checks: budget + variance = actual on every
   line; lines sum to their subtotals and to the total; no line silently appears
   or disappears versus the prior period; cross-statement ties where they apply;
   flag sign flips and ratios outside sane bounds.
6. Interpretation (the point of the whole thing): read each material variance and
   the trend against the business context you gathered, never in a vacuum.
   Separate signal from noise, say what it means for the business and whether it
   is likely to persist, connect lines that move together, and challenge me where
   the story does not hold together. The arithmetic above only exists to feed
   this step.
7. Questions to raise: the questions a sharp partner would put to management,
   ranked by what is at stake, each tied to the figure that prompts it, plus the
   open questions you cannot answer yet and the document or input that would
   close each one. Raise events, do not fill gaps: flag anything you cannot
   reconcile or explain instead of guessing a number.
8. Output, each run: a short memo (what matters and why, the trend, the ranked
   questions, the decisions it calls for), headline first, not the grid. Then
   give me the skill itself plus a one-line changelog.

Be critical throughout. If you do not have enough context to interpret a number,
say so and ask, rather than producing a confident but hollow variance table.
```

Three principles do the work: **understand before you measure** (context first, ask, never assume), **code for the arithmetic, judgment for the story** (variances and reconciliation are deterministic; interpretation is yours), and **raise events, do not fill gaps** (a partner flags what does not reconcile or what they cannot yet explain; an eager assistant invents a plausible number).

## Where it breaks

The skill lives in a file on one machine. It does not remember the business context you gave it last time, so every run risks starting from zero on the very thing that makes the review useful, and "read the trend" or "no line disappeared since last period" are only as good as the snapshots you happened to paste. The driver bridges silently freeze computed values into hardcoded numbers, and there is no version history when a figure is restated between drafts.

When you hit that, reach for:
- [`FINANCE.md`](https://github.com/layerzlabs/finance-md) to pin the business context and conventions the skill keeps re-asking for: what the company does, the drivers, the line taxonomy, what counts as material, sign convention, the per-line driver lens, so understanding survives between runs instead of being re-gathered each time.
- [Layerz](https://layerz.cc) (via [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp)) to hold budget, forecast and actuals as a structured, versioned model the agent reads and writes, so period-over-period trend and consistency checks run against real state, diffable across drafts, instead of a pasted snapshot.

## Notes and variations

- The board pack is just one application; the same skill serves a monthly management review, an investment-committee memo, or due diligence on your own model. Tune the materiality threshold and the audience per use.
- It runs wherever your numbers live: an Excel or Sheets file, a Layerz model the agent reads over MCP, or another MCP-connected source. The skill is the same; only the read/write surface changes. The more structured and persistent the source, the more of the context, consistency and trend checks survive between periods.
- Capture the context interview once in a `FINANCE.md` or a short brief, so future runs start from understanding, not from a blank page. That is the whole difference between a partner who knows your business and a temp who re-learns it every month.
- Second pass: fork the conversation and have a fresh Claude, persona "skeptical board member," try to find the one number that will not survive a question. Fresh context catches what the builder rationalized away.
- Pair with a backup discipline: version the working file each period. Native undo is not a safety net.

---

*Distilled from recurring board and management-review practice. Pending its first real run before it earns a permanent place in the catalog, per the repo's "a recipe nobody has run does not belong here" rule.*
