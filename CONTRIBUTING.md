# Contributing

**We share the recipe, not the black box.** This cookbook ships prompts and patterns, never installable skills. Keep that line in mind for anything you submit.

This cookbook grows from real work. Two ways to contribute.

## Add a recipe

A recipe is a **prompt or pattern** you have actually used for financial work with an AI agent. We ship prompts, not packaged skills. If your contribution is a skill, contribute the **Custom-skill** recipe that creates or hardens it (so people own and audit the result), not the skill file itself.

State the **kind** at the top of the recipe:
- **Prompt**: it does the task in-session.
- **Custom-skill**: it forges or hardens a skill, script or `FINANCE.md` the user owns.

1. Copy [`TEMPLATE.md`](./TEMPLATE.md) into `recipes/your-recipe-name.md`.
2. Fill the sections: the kind, the problem, the prompt, where it breaks, notes.
3. Be honest about the wall. Every recipe names the point where the prompt alone stops being enough. A recipe that claims a prompt replaces a model will not be merged.
4. Add a one-line note on where it was field-tested. Untested recipes do not belong here.
5. Add a row to the recipes table in the [README](./README.md), with its kind.

Open a pull request. Keep it to one recipe per PR.

## Suggest a library entry

The library in the README points to good external resources (other people's prompts, skills, posts, repos). Curation, not authorship.

The bar: it has to earn its place. We would rather link 10 vetted resources than 200 unsorted ones. Add a row to the library table with the resource, the author, what it is genuinely good for, and the link. Say in your PR why it earns a spot.

## Style

- English, plain and direct. No filler.
- No em dash. Use a comma, a colon, or parentheses.
- Show the prompt verbatim in a code block so people can copy it.
- If a recipe leans on a Layerz feature, link it, but the recipe must be useful to someone who never opens Layerz.
