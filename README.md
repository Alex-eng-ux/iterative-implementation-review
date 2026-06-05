# Iterative Implementation Review Skills

This repository contains two related Codex skills:

- `iterative-implementation-review`: manual or standard companion-skill workflow using `parallel-decomposer`, `code-analyzer`, and `grill-me`
- `iterative-implementation-review-auto`: auto-first workflow using `parallel-decomposer-auto`, `code-analyzer-auto`, and `grill-me`

Each skill is self-contained and can be installed independently.

## Which Version Should You Use?

| Variant | Use it when | Avoid it when |
| --- | --- | --- |
| `iterative-implementation-review` | You want a standard or mixed workflow, visible prompts, or manual control over decomposition and review steps | Your runtime can already dispatch sub-agents automatically and you want the loop to lean into that |
| `iterative-implementation-review-auto` | Your runtime can orchestrate workers or sub-agents and you want an auto-first repair/review loop | You need the workflow to stay explicit, copy-pasteable, or compatible with older environments |

The subject matter is the same in both versions. The difference is the execution model: manual or mixed orchestration versus auto-first orchestration.

## Workflow Generations

This repository is the workflow-layer bridge between the earlier skill families and the final integrated suite.

1. Generation 1: the manual workflow stack
   `iterative-implementation-review` works with `parallel-decomposer-skill`, `code-analyzer-suite`, and `grill-me`
2. Generation 2: the auto-capable workflow stack
   `iterative-implementation-review-auto` works with `parallel-decomposer-auto`, `code-analyzer-auto`, and `grill-me`
3. Final integrated distribution
   `implementation-workflows`, which packages both generations and adds `landable-implementation-loop` as the user-facing entry point

This is why the repository still matters even after the integrated suite exists: it defines the workflow logic that the other skill families plug into.

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

## Repository Roles At A Glance

| Repository | Primary job | Typical output |
| --- | --- | --- |
| `parallel-decomposer-skill` | split work safely | task cards or worker specs |
| `code-analyzer-suite` | review changed code | findings and severity-ranked risks |
| `iterative-implementation-review` | keep looping until the implementation survives review | repaired implementation plus verification status |
