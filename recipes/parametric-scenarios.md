# Parametric scenarios without the drift

> Generate and compare assumption-level scenarios, while keeping the links between variables intact.

**Kind:** Prompt. You paste it and it does the task in-session.

## The problem

"Give me a scenario" means two very different things, and conflating them is where AI modeling falls apart.

- A **parametric** scenario changes an assumption: 20 sites instead of 10, three hires instead of two, churn at 4% instead of 2%. The structure is unchanged, only inputs move.
- A **structural** scenario changes the model: a new channel, a build-up, a different revenue logic. This is a modeling job, not a parameter flip.

Free-form AI is good at the first and dangerous at the second. Ask it to "model the new channel" and it will happily rewrite formulas, hardcode a few values, and quietly break the links you relied on. The skill is to keep the agent on parametric ground, and to recognize when you have crossed into structural work.

## The prompt

For clean parametric variation, with the model in context:

```
I want to generate scenarios by changing assumptions only, not the structure of
the model. Before you touch anything:

1. List the input assumptions you can vary, and for each, the items that depend
   on it (downstream). If you are unsure what depends on what, say so rather than
   guessing.
2. I will give you the assumptions to change. Apply them by editing inputs only.
   Do not hardcode any computed value, do not change any formula, do not add or
   remove items.
3. After applying, confirm which downstream items moved and which did not, and
   tell me if anything that should have moved did not (a sign of a broken link).
4. Keep each scenario as a separate, named version so I can compare them side by
   side. Do not overwrite the base case.

If a change I ask for cannot be done by editing inputs alone (it needs new items
or new logic), stop and tell me. That is a structural change, and we should treat
it as modeling, not a parameter flip.
```

That last paragraph is the guardrail. It makes the agent draw the line you would otherwise discover too late.

## Where it breaks

In a spreadsheet or a chat, "keep each scenario as a named version" and "do not overwrite the base case" are promises the tool cannot enforce. There is no real versioning, no diff between scenarios, and nothing stops a computed value from being silently frozen into a number. The moment you have more than two or three scenarios, comparison becomes manual and trust erodes.

When you hit that, reach for:
- [Layerz](https://layerz.cc) (via [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp)), where each assumption is an explicit item, dependencies are a real DAG (so a broken link is visible, not guessed), and scenarios are versioned and diffable instead of being five copies of a file.
- [`FINANCE.md`](https://github.com/layerzlabs/finance-md) to record which levers are "assumptions" in the first place, so the agent knows what it is allowed to vary.

## Notes and variations

- For a structural change you genuinely need, scope it as a mini-model on the side (one revenue line, one cost line) before merging it in, rather than asking the agent to retrofit it into a mature model.
- When comparing scenarios, ask the agent to report the delta on a few headline metrics only (EBITDA, cash end of period), not the full grid. The grid hides the story.

---

*Field-tested qualifying scenario needs with founders and DAFs preparing fundraising business plans.*
