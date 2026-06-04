---
name: "iterative-implementation-review-auto"
description: "Run an auto-first implementation quality loop for complex coding tasks: decompose and dispatch work with parallel-decomposer-auto, review with code-analyzer-auto, stress-test with grill-me, repair blocking findings, and verify until the requested work is complete. Use when the user asks for automatic orchestration, multi-worker implementation, auto review, repeated repair loops, or to keep going until review passes."
---

# Iterative Implementation Review Auto

Run an auto-first implementation quality loop for non-trivial coding tasks. This skill coordinates three companion skills:

- `parallel-decomposer-auto` for splitting implementation or repair work into orchestration-ready worker specs.
- `code-analyzer-auto` for auto-dispatched, evidence-based review of correctness, security, architecture, performance, and maintainability.
- `grill-me` for adversarial pressure testing of assumptions and hidden failure modes.

This skill is an orchestrator. Keep the body focused on loop control, handoff quality, merge judgment, and verification. Let the companion skills define their own detailed output formats.

Do not use this skill for trivial edits, explanation-only tasks, one-off code reviews, or manual prompt decomposition when the user did not ask for automatic orchestration.

## Companion Contract

Prefer the companion skills in this order:

1. Use `parallel-decomposer-auto` to plan and dispatch independent implementation or repair work when automatic worker execution is available.
2. Use `code-analyzer-auto` after each implementation or repair round when automatic review workers are available.
3. Use `grill-me` as a non-interactive adversarial critique gate.

If automatic worker execution is unavailable, continue with the matching fallback instead of stopping:

- Missing or unavailable `parallel-decomposer-auto`: create a concise handoff brief, worker specs, and merge plan yourself, then emit manual worker prompts only as fallback.
- Missing or unavailable `code-analyzer-auto`: perform the same review dimensions yourself with concrete file and line evidence, or emit manual review worker prompts when useful.
- Missing `grill-me`: run the self-critique questions in this file and convert concrete risks into repair todos.

Stop for a missing dependency only when the user explicitly required that exact automatic companion skill and no fallback is acceptable.

## Workflow

Repeat the loop until all blocking gates pass.

### 1. Scope and Inventory

1. Restate the target outcome in one concise paragraph.
2. Inspect existing code patterns before editing or dispatching workers.
3. Identify affected files, public entry points, tests, and likely validation commands.
4. Create or update a todo list with implementation, review, repair, and validation tasks.
5. If scope is ambiguous, make the safest reasonable assumption and continue unless the choice would materially change implementation or worker ownership.

### 2. Auto Decompose and Dispatch

Use `parallel-decomposer-auto`, or its fallback, to split work by independent files, modules, or concerns. Prefer automatic worker dispatch when available; use manual worker prompts only as fallback.

Every worker spec must include:

- goal
- exact files or modules owned by the worker
- constraints and non-goals
- expected dispatch mode
- expected output
- verification requirements
- merge notes or dependencies

Avoid assigning two implementation workers to the same file. If two tasks must touch the same file, assign one owner and make the other a reviewer.

### 3. Implement and Merge

Implement or merge the dispatched worker results using the repository's existing conventions.

Rules:

- Prefer existing files and local patterns over new abstractions.
- Do not introduce unverified dependencies.
- Validate paths, user inputs, external data, and public API arguments.
- Preserve existing security and permission boundaries.
- Keep public signatures stable unless the requested feature requires a clear API change.
- Add comments only when they clarify non-obvious logic.
- After workers return, deduplicate overlapping changes and resolve contradictions before review.

### 4. Auto Code Analysis Gate

After each implementation or repair round, use `code-analyzer-auto`, or its fallback, against the current changed surface. Prefer auto-dispatched review workers when available; use manual prompts or single-agent review only as fallback.

The review input should include:

- files changed in the current round
- intended behavior
- public entry points or APIs affected
- validation commands already run and their results
- known constraints or skipped checks
- worker outputs or merge decisions that affect risk

Classify findings with the `code-analyzer-auto` severity scale:

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
- What worker handoff or merge assumption could have introduced a gap?

Convert concrete critique findings into repair todos. Ignore purely speculative concerns unless they reveal a plausible failure mode in the current code.

### 6. Auto Repair Loop

For every blocking issue:

1. Add a repair todo.
2. Choose sequential or automatic parallel repair based on file ownership and integration risk.
3. Use `parallel-decomposer-auto` for repair dispatch when multiple independent fixes exist.
4. Mark active repair tasks in progress before editing or dispatching.
5. Apply or merge the fix.
6. Mark a repair complete only after the fix is applied and focused verification has run or been explicitly blocked.
7. Re-run the auto code analysis and self-critique gates on the changed surface.

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
5. Summarize auto dispatch/review rounds and any remaining non-blocking caveats.

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
- name automatic dispatch or fallback mode used
- name validation commands run and their result
- name remaining caveats, if any
- do not claim success for checks that were not run

## Example Invocation

User:

```text
Use Skill: iterative-implementation-review-auto. Implement these advanced features with auto dispatch, auto review, critique, repair, and verification until the blocking findings are gone.
```

Assistant behavior:

1. Create an inventory and todo list.
2. Use `parallel-decomposer-auto` to split and dispatch implementation work, or create equivalent worker specs.
3. Implement and merge the worker results.
4. Use `code-analyzer-auto` to review the changed surface, or perform the same review manually.
5. Use `grill-me` as a non-interactive self-critique gate.
6. Convert blocking findings into repair tasks.
7. Repeat repair, review, and critique until the gates pass.
8. Run final validation commands.
9. Report verified status, dispatch mode, and caveats.
