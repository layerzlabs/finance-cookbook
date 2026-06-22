# Build your pre-delivery review skill

> Forge a reusable pre-flight check that runs on your model before it ships, tuned to how your shop actually works.

**Kind:** Custom-skill. It does not review one model, it builds the skill that reviews every model before you send it. You own the skill, this is how you forge and maintain it.

## The problem

Reviewing a model before it goes to a client is not a one-off, it is something you do every week, under deadline, on your own work, which is exactly when you are blind to it. Done from memory each time, the pass is inconsistent: you catch the broken link this week and miss the flipped sign next week. Your name is on the output either way.

A pasted one-shot prompt helps once. What you actually want is a review skill you keep: the same checklist every time, tuned to your conventions, that you harden as you find new ways a model embarrasses you.

## The prompt

In a fresh conversation, attach a real model (or export) to calibrate against, and your `FINANCE.md` if you have one:

```
I want you to build me a reusable "pre-delivery review" skill I can run on any
model before it goes to a client. Not a one-off answer, an artifact I keep and
rerun. Design it the way a demanding controller would.

Given a model (Excel or Layerz) and my conventions, the skill must check:

1. Integrity: hardcoded numbers where a formula belongs; totals that do not equal
   the sum of their parts; a balance sheet that does not net to zero; broken or
   mispointed references; circular logic; sign errors.
2. Consistency: units and currency consistent across sheets; a coherent timeline
   (no mixed grains, no gap or overlap); labels that match what the formula computes.
3. Reasonableness: check the headline outputs (margins, growth, multiples, cash)
   for plausibility, and say why each looks fine or off.
4. Staleness: placeholder values, test numbers, "TODO", assumptions that look out
   of date.
5. Client readiness: unexplained acronyms, an output with no visible driver, a sheet
   left in a debugging state.
6. Output a go / no-go and a fix list ordered by severity: blocking errors first,
   then things that cost credibility, then cosmetics.

Design rules:
- Separate the deterministic checks (balance ties, totals, references) from the
  judgment ones (is this margin plausible). Make the deterministic checks explicit
  and repeatable, not free-form reasoning.
- Raise a flag on anything it cannot reconcile instead of guessing or filling the gap.
- Read my conventions from FINANCE.md (sign, units, glossary) rather than assuming them.

Produce: the skill itself (instructions plus any fixed checks), a short usage note,
and the list of edge cases it does not yet handle so I know its limits.
```

Run it once against a real model, keep the result, and rerun it on every deliverable. Each time a model slips through with a defect, add that check and ask Claude to harden the skill again.

The two principles doing the work: **deterministic checks as code, judgment to the model**, and **raise events, do not fill gaps**. A controller flags what does not reconcile; an eager assistant invents a plausible number. You want the first behavior, baked into the skill.

## Where it breaks

The skill is instructions on one machine. On a flat spreadsheet it has no real dependency graph, so "this reference is wrong" or "this does not tie" is inferred and can miss the one that matters. And the structural checks that should be automatic (balance nets to zero, totals equal their parts) only run when you remember to run the skill, against whatever file you happened to paste.

When you hit that, reach for:
- [`FINANCE.md`](https://github.com/layerzlabs/finance-md) so the skill checks against your declared sign convention, units and glossary instead of guessing them.
- [Layerz](https://layerz.cc) (via [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp)), where the dependency graph is explicit and structural checks like "actif = passif" run as live monitors, so the model cannot silently break between two exports and the review is not the only thing standing between a bug and the client.

## Notes and variations

- Calibrate on a model you know is clean and one you know is broken. If the skill passes the broken one, it is not strict enough yet.
- Add a skeptical second pass: a fresh Claude, persona "the client's analyst trying to discredit this model," hunting for one thing to attack.
- Keep the fix list from each run. Rerunning after the fixes is your sign-off that the blocking items are gone.

---

*Field-tested where the deliverable goes out to a client or sponsor and the last pass before send is the one that protects your name.*
