# Iterative Implementation Review Skill

Use this skill to run a strict implementation loop for complex software changes. It coordinates, but does not replace, implementation judgment and local validation.

## Activation

Activate when the user asks to:

- implement complex features and keep iterating until review passes
- use `parallel-decomposer`, `code-analyzer`, and `grill-me` together
- repeatedly dispatch tasks, analyze code, self-critique, and repair issues
- continue until no blocking problems remain

Do not activate for trivial edits, pure explanations, or one-off reviews.

## Companion Skills

Prefer these companions in order:

1. `parallel-decomposer` for implementation or repair task cards.
2. `code-analyzer` for evidence-based review with Critical, High, Medium, and Low severities.
3. `grill-me` for non-interactive adversarial self-critique.

If a companion is unavailable, use an equivalent fallback:

- create your own handoff brief, task cards, and merge plan when `parallel-decomposer` is missing
- run the same review dimensions manually when `code-analyzer` is missing
- answer the self-critique questions yourself when `grill-me` is missing

Stop for a missing companion only when the user explicitly required that exact skill and no fallback is acceptable.

## Required Loop

1. Clarify the target outcome and inspect existing code patterns.
2. Identify affected files, public entry points, tests, and validation commands.
3. Track implementation, review, repair, and validation work with todos.
4. Decompose implementation or repair work by independent files, modules, or concerns.
5. Implement changes using existing project conventions.
6. Run a code analysis gate on the changed surface.
7. Run a non-interactive self-critique gate.
8. Convert every blocking finding into repair todos.
9. Repeat repair, review, and critique until all gates pass.
10. Run available lint, typecheck, tests, syntax checks, or build commands.
11. Report only verified completion and explicitly name any checks that could not run.

## Gates

The code analysis gate must cover relevant correctness, security, maintainability, architecture, performance, lifecycle, and validation risks. Any Critical, High, or functionality-blocking Medium finding requires another repair round.

The self-critique gate must ask:

- What assumption could be wrong?
- What user-visible path still fails?
- What happens with empty, malformed, missing, or hostile inputs?
- What resource cleanup could be incomplete?
- What API or environment behavior might differ across platforms?
- What was implemented but not exposed through the public surface?
- What validation was not actually run?

Do not use `grill-me` as a user-facing one-question-at-a-time session inside this workflow unless a material decision truly requires user input.

## Stop Conditions

Stop only when:

- all requested functionality is implemented
- all Critical and High findings are resolved
- functionality-blocking Medium findings are resolved
- validation commands pass or unavailable checks are clearly reported
- the user explicitly stops the loop
- progress is blocked by a missing credential, environment dependency, or required user decision
- continuing would require unsafe or destructive action that the user has not authorized
