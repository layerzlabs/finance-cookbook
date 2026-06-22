# Read an inherited model like a controller

> Understand and stress-test a model someone else built, in an hour instead of a week.

**Kind:** Prompt. You paste it and it does the task in-session.

## The problem

You receive a model: a deal model from a counterpart, a budget from a predecessor, a board file you did not build. Before you can trust a single output, you have to reverse-engineer the logic, find the assumptions, and figure out what breaks if one of them moves. Done by hand, this is days of clicking through cells. The risk is not slowness, it is signing off on a number you did not actually understand.

A fresh agent with a controller's mandate can map the model far faster than you can, *if* you tell it to be skeptical rather than reassuring.

## The prompt

With the model (or export) in context, in a fresh conversation:

```
You are a demanding financial controller reviewing a model you did not build and
do not trust yet. Do not reassure me. Your job is to understand it and find what
is fragile.

1. Map the model: the main outputs, the key assumptions that drive them, and the
   chain between them. Where the logic is unclear, say "unclear" rather than
   inventing an explanation.
2. Find the load-bearing assumptions: the three to five inputs that move the
   outputs the most. For each, tell me the current value and whether it looks
   reasonable or aggressive.
3. Flag the danger signs: hardcoded values where a formula should be, a total
   that does not equal the sum of its parts, a balance that does not net to zero,
   a reference that points to the wrong line, circular logic.
4. List what you cannot verify from what I gave you, and what you would need to
   verify it.
5. Give me the five questions I should ask whoever built this, in priority order.

Be specific and cite the items you are talking about. Vague reassurance is worse
than useless here.
```

The point of the persona is behavioral: a neutral assistant tends to explain the model as if it were correct. A controller persona looks for the crack.

## Where it breaks

The agent can only reason over what you paste. In a flat spreadsheet there is no real dependency graph, so "the chain between assumptions and outputs" is reconstructed by inference and can be wrong in exactly the place that matters. And the map it produces is a one-off: change one input and you have to ask again, because nothing holds the structure for you.

When you hit that, reach for:
- [Layerz](https://layerz.cc) (via [`layerz-mcp`](https://github.com/layerzlabs/layerz-mcp)), where dependencies are explicit and queryable (ask "what depends on this assumption" and get the real answer, not a guess), and the structure persists so the audit stays live as inputs change.
- [`FINANCE.md`](https://github.com/layerzlabs/finance-md) to capture the conventions the model assumes, so the next person does not start this archaeology from zero.

## Notes and variations

- Run the prompt twice with different framings (once "find what is aggressive," once "find what is fragile mechanically"). The two passes surface different problems.
- Keep the five questions it generates. They are the agenda for your call with whoever built the model.

---

*Field-tested in M&A and transaction-advisory contexts where understanding a received model fast is the whole job.*
