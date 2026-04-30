---
name: git-commit-intelligence
description: Use when generating or reviewing git commit messages for unfamiliar repositories, ambiguous changes, or mixed codebases where type, scope, or domain can be misclassified.
---

# Git Commit Intelligence

## Purpose

Ensure commit messages are precise, consistent, and never misleading.

## Execution Strategy (STRICT)

You MUST follow every step in order.

### Step 1 - Load Core Rules

Read:

- `core/format.md`
- `core/scoring.md`
- `core/safety.md`

### Step 2 - Infer Domain

Determine the domain using this priority:

1. Repository context
2. Code characteristics
3. File structure as a weak signal
4. Keywords as a last resort

### Step 3 - Set Domain Confidence

Estimate confidence on a `0.0-1.0` scale:

- `>= 0.7` -> proceed
- `0.4-0.7` -> proceed with assumptions
- `< 0.4` -> ASK

If there is no strong domain signal:

- Set `domain = other`
- Reduce confidence by `0.2-0.3`

### Step 4 - Load Domain Rules

Load exactly one domain rule when matched:

- `embedded` -> `domains/embedded.md`
- `driver` -> `domains/driver.md`
- `desktop` -> `domains/desktop.md`

If none match:

- Set `domain = other`
- `domains/generic.md` is optional fallback context

### Step 5 - Infer Scope Semantically

Read:

- `core/scope-inference.md`

Rules:

- Scope reflects the responsibility of the change
- Scope is NOT just a file name or folder
- Path is a weak hint only

### Step 5.5 - Apply Repo Memory (If Present)

Read:

- `memory/commit-style.md`

Rules:

- Prefer repo-defined scopes
- Map semantic scope to repository scope
- Repo memory overrides generic inference

### Step 6 - Infer Type

Use this priority:

`fix > sec > perf > feat > refactor > test > docs > chore`

### Step 7 - Generate Commit

Rules:

- Follow the strict format
- Explain WHY, not just what changed
- Avoid vague wording

### Step 8 - Score and Improve

- Score the result from `0-100`
- If score is below `75`, rewrite it
- Retry at most `2` times

### Step 9 - Final Confidence Gate

If ANY of the following are true:

- Domain confidence is below `0.5`
- Scope is unclear
- Multiple reasonable interpretations remain

Then:

- DO NOT finalize
- ASK

## Domain Guard

If domain is known:

- Enforce domain-specific scope constraints

If `domain = other`:

- Allow generic scopes
- Prioritize clarity and responsibility over forced specificity

## ASK Mode

### Trigger

Enter ASK mode when:

- Confidence is low
- Ambiguity affects the likely domain, scope, or type

### Rules

- Ask at most `2` questions
- Every question must affect the decision
- Prefer multiple-choice questions
- Avoid open-ended questions when a bounded choice will do

### Question Format

```text
Question: <clear question>

Options:
A. ...
B. ...
C. ...
```

### Example

```text
Question:
What type of system does this change belong to?

Options:
A. Firmware / hardware control
B. OS-level driver
C. Application / service
D. Other
```

### Output (ASK)

```yaml
decision: ask

questions:
  - question: ...
    options:
      A: ...
      B: ...
      C: ...
```

## Memory Suggestion (Optional)

Trigger only if:

- The same pattern appears at least `3` times
- The user consistently overrides the inferred result

### Output

```yaml
memory_suggestion:
  reason: ...
  proposed_update:
    mappings:
      - from: ...
        to: ...
        confidence: 0.0-1.0
```

## Memory Apply Mode (Controlled)

Apply memory ONLY if the user explicitly says:

`APPROVE MEMORY UPDATE`

### Rules

- Modify `memory/commit-style.md` only
- Keep the diff minimal
- Do not do a full rewrite
- Preserve the existing structure

### Output

```yaml
memory_update_applied: true

updated_sections:
  - ...
```

## Output Format (MANDATORY)

```yaml
commit_message: |
  <type>(<scope>): <summary>

  <why>

  * change 1
  * change 2

type: <type>
scope: <scope>

domain: <embedded | driver | desktop | other>
confidence: <0.0-1.0>

score: <0-100>

issues:
  - <if any>

decision: accept | revise | ask
```
