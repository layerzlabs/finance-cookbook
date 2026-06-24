# Audit and harden your close skill

> Have Claude review the close-and-variance skill you already built, like a demanding controller would.

**Kind:** Custom-skill. It does not run your close, it hardens the skill that does. You own the skill, this is how you forge and maintain it.

## The problem

Month-end close is recurring and unforgiving. A variance review runs across your cost centers: variance vs budget and forecast, checking that recurring costs are actually booked, then sending corrections back to accounting. People build a Claude skill to bring this from days down to about an hour, which works, until a silent gap (a cost center that disappeared, a recurring line nobody flagged) slips into an investor report.

The fastest way to harden a skill is not to rewrite it yourself. It is to make Claude audit its own work against a controller's checklist.

## The prompt

Share your existing skill with Claude, then paste this:

```
You are an exacting financial controller. I am sharing a Claude skill I built
to run my monthly close variance review: variance vs budget and forecast across
cost centers, checking that recurring costs are booked, then sending corrections
back to accounting. Your job is to make it more reliable and faster without
losing determinism. Please:

1. Read the skill and restate, step by step, exactly what it does. Flag anything
   ambiguous or implicit.
2. Separate the steps that rely on judgment (and could drift month to month)
   from the ones that should be deterministic (same input, same output). For the
   deterministic ones, propose turning them into explicit code or fixed checks
   rather than free-form reasoning.
3. Add systematic sanity checks: the balance nets to zero, the total equals the
   sum of cost centers, no cost center silently disappears between months,
   recurring lines are flagged when missing vs last month.
4. Make it raise an explicit flag or question on anything it cannot reconcile,
   instead of guessing or filling the gap.
5. List the edge cases it does not currently handle (new cost center, renamed
   center, partial month, reclassification) and how to handle each.
6. Output an improved version of the skill plus a short changelog of what changed
   and why.

Be critical. If a step is fragile, say so directly.
```

The two principles doing the work here:
- **Code for the deterministic, AI for the judgment.** Reconciliation and variance are deterministic and belong in code that runs identically every month. Cost mapping and commentary are judgment and belong to the model.
- **Raise events, do not fill gaps.** A controller flags what does not reconcile. An eager assistant quietly invents a plausible number. You want the first behavior, explicitly.

## Where it breaks

The skill lives in a conversation or a file on one machine. It does not remember last month, so "no cost center silently disappeared" is only as good as the file you happened to paste. There is no durable, queryable model of the close that the skill checks against, and no version history when accounting pushes a correction.

When you hit that, reach for:
- [`FINANCE.md`](https://github.com/layerzlabs/finance-md) to pin the conventions the skill assumes (what "recurring" means, sign convention, the cost-center list), so they stop living in your head.
- [Layerz](https://layerz.cc) (via [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp)) to hold the close as a structured, versioned model the agent reads and writes, so month-over-month checks run against real state instead of a pasted snapshot.

## Notes and variations

- Add a second pass: fork the conversation and have a fresh Claude, persona "skeptical auditor," try to find one number it does not trust. Fresh context catches what the builder's context rationalized away.
- Pair with a backup discipline: version the working file each morning. Native undo in Claude or Excel is not a safety net.

---

*Field-tested in office-hours work with finance teams running monthly investor closes.*
