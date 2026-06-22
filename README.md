# Finance Cookbook

**Curated recipes for doing financial work with AI agents, the right way.**

> ### We share the recipe, not the black box.
> Prompts you can read, audit and make yours. Never a skill you just install on trust.

Prompts and patterns for FP&A, modeling and reporting with Claude (and any capable agent). Each recipe is battle-tested in the field, opinionated, and honest about where a prompt stops being enough.

This is the **cookbook**. It sits next to two sibling projects:

| Project | Role | Link |
|---------|------|------|
| [`finance-md`](https://github.com/layerzlabs/finance-md) | The **standard**: how an organization encodes its financial conventions for any agent | the spec |
| [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp) | The **tool**: how an agent drives [Layerz](https://layerz.cc) to build structured, versioned models | the integration |
| **`finance-cookbook`** (you are here) | The **recipes**: how finance people actually use AI, day to day | the playbook |

The standard tells an agent *what your numbers mean*. The tool gives it *a place to build that does not drift*. The cookbook shows *what to ask for in the first place*.

---

## Why this exists

A good prompt can take you a long way. Then it hits a wall: nothing persists between sessions, the logic drifts the moment an assumption changes, and you cannot audit where a number came from. That wall is not a failure of prompting, it is the point where you need **structure** (a documented `FINANCE.md`) and **persistence** (a real model, e.g. Layerz).

So every recipe here is written to be useful on its own, and to name the wall it cannot cross. When you hit that wall, the recipe points you to `finance-md` or Layerz. No recipe pretends a prompt is a model.

---

## Our stance: we share the recipe, not the black box

**We share the recipe, not the black box.** This cookbook ships **prompts and patterns, never installable skills**. A packaged skill is a black box: it rots, it is tied to one runtime, and you cannot easily audit it. We would rather hand you the prompt and let you own what it produces. That is the Layerz thesis applied to this repo: own your context, no black box, no drift.

So there are two kinds of recipe:

- **Working prompt**: you paste it and it does the task now (compare scenarios, audit a model you received).
- **Builder prompt**: you paste it and Claude creates, audits or hardens *your own* skill, script or `FINANCE.md`. You own the artifact, the recipe is how you forge and maintain it.

The one installable artifact in this ecosystem, the Layerz agent skill, lives in [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp), not here.

## Recipes

| Recipe | Kind | Problem it solves |
|--------|------|-------------------|
| [Audit and harden your close skill](./recipes/close-variance-audit.md) | Builder | A month-end variance review that is fast *and* reliable, with built-in sanity checks |
| [Parametric scenarios without the drift](./recipes/parametric-scenarios.md) | Working | Generate and compare assumption-level scenarios without the model falling apart |
| [Read an inherited model like a controller](./recipes/model-audit-controller.md) | Working | Understand and stress-test a model someone else built, fast |
| [Review your own model before it ships](./recipes/pre-delivery-model-review.md) | Working | Catch the error before the client does, with a controller's pre-flight checklist |
| [Own your context with FINANCE.md](./recipes/context-ownership-finance-md.md) | Builder | Stop re-explaining your conventions every session |

Each recipe follows the same shape (see [`TEMPLATE.md`](./TEMPLATE.md)): the problem, the prompt, where it breaks, and what to reach for when it does.

---

## The library (curated, external)

The cookbook above is what we wrote. The library below is what *others* wrote and we found worth your time. Curation, not authorship. Suggestions welcome (see [Contributing](./CONTRIBUTING.md)).

| Resource | Author | Good for | Link |
|----------|--------|----------|------|
| Anthropic Cookbook | Anthropic | Foundational prompting and agent patterns (not finance-specific) | https://github.com/anthropics/anthropic-cookbook |
| _to curate_ | AI Finance Club / Nicolas Boucher | Finance-team AI workflows, accessible to non-coders | _add link_ |
| _your suggestion_ | _you_ | _what it is good for_ | _open a PR_ |

> This table is intentionally short. A library of 10 vetted resources beats a dump of 200. If you know one that earns its place, [open a PR](./CONTRIBUTING.md).

---

## Who this is for

- **AI builders in finance** who live in Claude Code, Cursor, Cowork, and want patterns that hold up.
- **Educators and advisors** who need serious, recommendable bricks, beyond a list of prompts.
- **Finance practitioners** (FP&A, controllers, fractional CFOs) tired of re-deriving everything each month.

---

## Contributing

Recipes and library entries are welcome. Read [`CONTRIBUTING.md`](./CONTRIBUTING.md) and copy [`TEMPLATE.md`](./TEMPLATE.md). The bar is simple: it has to be something you actually used, and it has to be honest about its limits.

---

## License

MIT, see [LICENSE](./LICENSE).

---

**We share the recipe, not the black box.**

*Maintained by [Layerz](https://layerz.cc). The financial memory of Claude: a structured model it builds, edits and exports to Excel, without drifting.*
