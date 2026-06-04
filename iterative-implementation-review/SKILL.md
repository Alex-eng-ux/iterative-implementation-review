---
name: "iterative-implementation-review"
description: "Orchestrate complex feature implementation through repeated decomposition, implementation, code analysis, adversarial self-critique, repair, and verification. Use when the user asks to keep implementing until review passes, to combine parallel-decomposer/code-analyzer/grill-me, or to repeatedly dispatch tasks, analyze code, challenge assumptions, and fix blocking issues."
---

# Iterative Implementation Review

Run a strict implementation quality loop for non-trivial coding tasks. This skill coordinates three companion skills when they are available:

- `parallel-decomposer` for splitting implementation or repair work into independent task cards.
- `code-analyzer` for evidence-based review of correctness, security, architecture, performance, and maintainability.
- `grill-me` for adversarial pressure testing of assumptions and hidden failure modes.

This skill is an orchestrator. It does not replace implementation judgment, local code inspection, or validation commands.

## Trigger

Use this skill when the user asks for complex implementation and explicitly wants an iterative quality loop, for example:

- "Use the previous workflow and keep going."
- "Repeatedly dispatch tasks, analyze code, self-critique, then dispatch fixes."
- "Implement it, review it, and keep repairing until it passes."
- "Do not stop until everything is implemented."
- "Use parallel-decomposer, code-analyzer, and grill-me together."
- "Keep iterating until there are no blocking review findings."

Do not use this skill for trivial edits, explanation-only tasks, or one-off code reviews without implementation.

## Companion Contract

Prefer the companion skills in this order:

1. Use `parallel-decomposer` to plan independent implementation or repair work.
2. Use `code-analyzer` after each implementation or repair round.
3. Use `grill-me` as a non-interactive adversarial critique gate.

If a companion skill is unavailable, continue with the matching fallback instead of stopping:

- Missing `parallel-decomposer`: create a concise handoff brief, task cards, and merge plan yourself.
- Missing `code-analyzer`: perform the same review dimensions yourself with concrete file and line evidence.
- Missing `grill-me`: run the self-critique questions in this file and convert concrete risks into repair todos.

Stop for a missing dependency only when the user explicitly required that exact companion skill and no fallback is acceptable.

## Workflow

Repeat the loop until all blocking gates pass.

### 1. Scope and Inventory

1. Restate the target outcome in one concise paragraph.
2. Inspect existing code patterns before editing.
3. Identify affected files, public entry points, tests, and likely validation commands.
4. Create or update a todo list with implementation, review, repair, and validation tasks.
5. If scope is ambiguous, make the safest reasonable assumption and continue unless the choice would materially change the implementation.

### 2. Decompose and Dispatch

Use `parallel-decomposer`, or its fallback, to split work by independent files, modules, or concerns. Good splits often separate:

- operations or domain logic
- tool/API registration
- UI or command surface
- tests and validation
- docs or examples, only when requested

Every task card must include:

- goal
- exact files or modules owned by the worker
- constraints and non-goals
- expected output
- verification requirements

Avoid assigning two implementation workers to the same file. If two tasks must touch the same file, assign one owner and make the other a reviewer.

### 3. Implement

Implement the decomposed tasks using the repository's existing conventions.

Rules:

- Prefer existing files and local patterns over new abstractions.
- Do not introduce unverified dependencies.
- Validate paths, user inputs, external data, and public API arguments.
- Preserve existing security and permission boundaries.
- Keep public signatures stable unless the requested feature requires a clear API change.
- Add comments only when they clarify non-obvious logic.

### 4. Code Analysis Gate

After each implementation or repair round, use `code-analyzer`, or its fallback, against the current changed surface.

The review input should include:

- files changed in the current round
- intended behavior
- public entry points or APIs affected
- validation commands already run and their results
- known constraints or skipped checks

Review only relevant dimensions, but cover all high-risk areas:

- correctness and edge cases
- security and input validation
- code quality and maintainability
- architecture and layering
- performance and resource lifecycle
- test and validation gaps

Classify findings with the `code-analyzer` severity scale:

- Critical
- High
- Medium
- Low

Treat any Critical, High, or functionality-blocking Medium finding as a blocking issue that requires another repair round. Use Low findings only when they are concrete and tied to observed code.

### 5. Self-Critique Gate

Use `grill-me` in non-interactive mode for this workflow. Do not start a user-facing one-question-at-a-time grilling session unless a decision truly requires user input.

Produce a concise critique report by answering:

- What assumption could be wrong?
- What user-visible path still fails?
- What happens with empty, malformed, missing, or hostile inputs?
- What resource cleanup could be incomplete?
- What API or environment behavior might differ across platforms?
- What did the implementation claim to support but not expose through the public surface?
- What validation was not actually run?

Convert concrete critique findings into repair todos. Ignore purely speculative concerns unless they reveal a plausible failure mode in the current code.

### 6. Repair Loop

For every blocking issue:

1. Add a repair todo.
2. Choose sequential or parallel repair based on file ownership and integration risk.
3. Mark active repair tasks in progress before editing.
4. Apply the fix.
5. Mark a repair complete only after the fix is applied and focused verification has run or been explicitly blocked.
6. Re-run the code analysis and self-critique gates on the changed surface.

Continue until:

- all requested functionality is implemented
- all public entry points expose the implemented behavior
- all Critical and High findings are resolved
- no functionality-blocking Medium findings remain
- syntax, lint, typecheck, tests, or build checks pass, or unavailable checks are explicitly reported

### 7. Final Verification

Before the final response:

1. Run available syntax, lint, typecheck, tests, or build commands.
2. If no validation command is known, inspect the project files for the right command.
3. If validation cannot run, state exactly what could not be run and why.
4. Summarize completed features by area.
5. Summarize review rounds and any remaining non-blocking caveats.

## Stop Conditions

Stop only when one of these is true:

- all quality gates pass
- the user explicitly tells you to stop
- a missing credential, environment dependency, or material user decision blocks progress
- continuing would require unsafe or destructive action that the user has not authorized

If blocked, explain the blocker and the exact next action needed.

## Final Response

Keep the final response concise and evidence-based:

- name the completed implementation areas
- name validation commands run and their result
- name remaining caveats, if any
- do not claim success for checks that were not run

## Example Invocation

User:

```text
Use Skill: iterative-implementation-review. Implement these advanced features with repeated dispatch, review, critique, repair, and verification until the blocking findings are gone.
```

Assistant behavior:

1. Create an inventory and todo list.
2. Use `parallel-decomposer` to split implementation work, or create equivalent task cards.
3. Implement the tasks.
4. Use `code-analyzer` to review the changed surface, or perform the same review manually.
5. Use `grill-me` as a non-interactive self-critique gate.
6. Convert blocking findings into repair tasks.
7. Repeat repair, review, and critique until the gates pass.
8. Run final validation commands.
9. Report verified status and caveats.
