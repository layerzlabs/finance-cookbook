# Review your own model before it ships

> Catch the embarrassing error before the client does, with a controller's pre-flight checklist.

**Kind:** Prompt. You paste it and it does the task in-session.

## The problem

You built or updated a model and it is about to leave your hands: a BP for a client, a deal model for a sponsor, a budget for the board. Deadline pressure, and your name is on the output. The risk is the small thing: a hardcoded cell where a formula belonged, a link that broke when you inserted a row, a total that no longer ties, a sign flipped on a cost, an assumption left at last month's value. After hours inside the file you are the worst-placed person to spot it, you read what you meant, not what is there.

A fresh agent with a reviewer's mandate reads what is actually there, *if* you tell it to assume an error exists rather than to congratulate you.

## The prompt

With the model (or its export) in context, in a fresh conversation:

```
You are a demanding financial controller doing the final review of a model that is
about to go to a client. It is not yours to flatter. Assume there is at least one
error and your job is to find it before the client does. Do not reassure me.

Work through this checklist and report findings item by item, citing the exact
cells, lines or items:

1. Integrity. Hardcoded numbers where a formula belongs; totals that do not equal
   the sum of their parts; a balance sheet that does not net to zero; broken or
   mispointed references; circular logic; sign errors (a cost entered positive, a
   cash outflow with the wrong sign).
2. Consistency. Units and currency consistent across sheets; the timeline coherent
   (no mixed monthly/annual, no gap or overlap in periods); labels that match what
   the formula actually computes.
3. Reasonableness. The three to five headline outputs: are they plausible? Flag any
   margin, growth rate, multiple or cash balance that looks off, and say why.
4. Staleness. Assumptions or actuals that look out of date, placeholder values left
   in, "TODO" or test numbers that should not ship.
5. Client readiness. Would someone who did not build this understand it? Unexplained
   acronyms, an output with no visible driver, a sheet left in a debugging state.

Then give me a go / no-go and a fix list ordered by severity: blocking errors first,
then things that will cost me credibility, then cosmetics. Be specific. A vague
"looks good" is a failure of this review.
```

The persona is doing work: a neutral assistant reviewing your own file tends to validate it. A controller about to put their signature next to yours looks for the crack.

## Where it breaks

In a flat spreadsheet the agent has no real dependency graph, so "this total does not tie" or "this reference is wrong" is reconstructed by inference and can miss the one that matters, or flag a false positive. Worse, the review is a one-off: you fix the three things it found, regenerate the file, and none of the checks re-run. Nothing guarantees the next version is clean. The integrity checks that should be automatic (balance nets to zero, totals equal their parts) are left to your memory to re-run every time.

When you hit that, reach for:
- [Layerz](https://layerz.cc) (via [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp)), where the dependency graph is explicit and queryable, and structural checks like "actif = passif" run as live monitors instead of a manual pass, so the model cannot silently break between two exports.
- [`FINANCE.md`](https://github.com/layerzlabs/finance-md) to declare your sign convention, units and glossary, so the review checks against your actual rules rather than guessing them.

## Notes and variations

- Run this as the last gate before send, not during the build. It is a pre-flight check, not a modeling session.
- For a Layerz model, point the agent at the model over MCP and ask it to read with values first: the balance and monitor checks come back as facts, not inference.
- Keep the fix list. Re-running the prompt after the fixes is your sign-off that the blocking items are actually gone.

---

*Field-tested in financial-modeling engagements where the deliverable goes out to a client or sponsor, and the last pass before send is the one that protects your name.*
