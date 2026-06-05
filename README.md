# Iterative Implementation Review Skills

This repository contains two related Codex skills:

- `iterative-implementation-review`: manual or standard companion-skill workflow using `parallel-decomposer`, `code-analyzer`, and `grill-me`
- `iterative-implementation-review-auto`: auto-first workflow using `parallel-decomposer-auto`, `code-analyzer-auto`, and `grill-me`

Each skill is self-contained and can be installed independently.

## Position In The Workflow

This repository is the workflow layer, not the decomposition or analysis layer.

In practice:

- `parallel-decomposer-skill` or `parallel-decomposer-auto` handles task splitting and worker planning
- `code-analyzer-suite` or `code-analyzer-auto` handles changed-surface review
- `iterative-implementation-review` or `iterative-implementation-review-auto` loops implementation, review, critique, repair, and verification until the work is actually ready

That means this repository works best as the orchestrator that sits on top of the other two skill families.

## Recommended Companion Repositories

- Decomposition: [Alex-eng-ux/parallel-decomposer-skill](https://github.com/Alex-eng-ux/parallel-decomposer-skill)
- Code review: [Alex-eng-ux/code-analyzer-suite](https://github.com/Alex-eng-ux/code-analyzer-suite)
- Unified suite: [Alex-eng-ux/implementation-workflows](https://github.com/Alex-eng-ux/implementation-workflows)

If you want one repo that packages the whole stack together, use `implementation-workflows`.

## Suggested Mental Model

Think of the three repositories like this:

1. `parallel-decomposer-skill`: how to split the work
2. `code-analyzer-suite`: how to review the changed code
3. `iterative-implementation-review`: how to keep looping until the implementation survives review

This repository is the oldest workflow layer in that stack, and it remains useful because it gives the other two repos a repeatable operating model.
