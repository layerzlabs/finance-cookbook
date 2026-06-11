# Own your context with FINANCE.md

> Stop re-explaining your conventions every session. Write them down once, in a file you own.

**Kind:** Builder prompt. It forges an artifact you keep (your `FINANCE.md`), it does not produce a one-off answer.

## The problem

Every time you start a session with an agent on a financial model, you repeat yourself:

> "Amounts are in thousands. Costs are positive. Books close on the 10th. We report on a management basis, not IFRS. 'Marge brute' here means revenue minus purchases only, it is not gross profit."

This context lives in your head, gets pasted into prompts, and evaporates between sessions. Worse, if you lean on the model's built-in memory, you do not own it: you cannot read it, diff it, share it, or move it to another tool. You are renting your own conventions.

The fix is boring and powerful: put the conventions in a file, in your project, that any agent reads at the start.

## The prompt

To bootstrap the file from an existing model or your own description:

```
Help me write a FINANCE.md for this model, following the finance-md standard
(https://github.com/layerzlabs/finance-md).

1. Front matter (machine-readable, keep it minimal): model name, currency, units,
   language, sources, owners, and a glossary of any term that does not mean the
   textbook thing here.
2. Body (prose, with the rationale): how we define EBITDA, what is in net debt,
   how working capital is measured, the sign convention, the close calendar, and
   why each of these is the way it is.

Ask me about anything you are unsure of instead of guessing. A 100-line file that
is correct beats a 1000-line file that is plausible.
```

Then wire it into your agent so it is read automatically:
- **Claude Code / Cursor**: reference it from your `CLAUDE.md` or project rules with `@FINANCE.md`.
- **Cowork or chat**: keep `FINANCE.md` (plus a short business overview and a couple of past reportings) as `.md` files in the project folder, so context is segmented and portable rather than dumped into one memory blob.
- **MCP**: a server can expose the file to the agent at session start (Layerz, for instance, ships tools to push and pull a model's `FINANCE.md`).

## Where it breaks

A `FINANCE.md` documents the conventions, but it is not the model. It says how you define EBITDA, it does not compute it. The moment you want those conventions enforced (not just described) on live numbers that persist and version, you need a model that reads the file and operates under it.

When you hit that, reach for:
- [`finance-md`](https://github.com/layerzlabs/finance-md) for the full spec, the schema, and sector example files (LBO, SaaS FP&A, infra, SME budget) you can inherit from.
- [Layerz](https://layerz.cc) (via [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp)) to bind the conventions to an actual model, so the agent does not just know your rules, it builds under them.

## Notes and variations

- Use the hierarchy: a sector `FINANCE.md` (published reference) at the top, your organization's in the middle, the model-specific one at the bottom, via `inherits_from`. Write the stable stuff once.
- Treat the file as living. When a convention changes (you switch EBITDA definitions, you change the close date), update `FINANCE.md` in the same breath. A stale conventions file is worse than none.

---

*Field-tested with finance teams who already use agents daily but never persisted their conventions, and felt the cost every session.*
