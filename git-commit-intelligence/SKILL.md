---
name: git-commit-intelligence
description: >
Generate high-quality git commit messages with extremely low misclassification.
Supports embedded, driver, desktop, and unknown domains via semantic inference,
confidence gating, ASK mode, and repo-specific memory.
---

# Purpose

Ensure commit messages are precise, consistent, and never misleading.

---

# Execution Strategy (STRICT)

You MUST follow ALL steps in order.

---

## Step 1 — Load Core Rules

Read:

* core/format.md
* core/scoring.md
* core/safety.md

---

## Step 2 — Domain Inference (Context-Based)

Determine domain using PRIORITY:

1. Repository context
2. Code characteristics
3. File structure (weak signal)
4. Keywords (last resort)

---

## Step 3 — Domain Confidence

Estimate confidence (0.0–1.0):

* ≥ 0.7 → proceed
* 0.4–0.7 → proceed with assumptions
* < 0.4 → ASK

If no strong domain:

→ set domain = other
→ reduce confidence by 0.2–0.3

---

## Step 4 — Load Domain Rules

Load ONE domain rule if matched:

* embedded → domains/embedded.md
* driver → domains/driver.md
* desktop → domains/desktop.md

If none match:

* domain = other
* load domains/generic.md (optional)

---

## Step 5 — Scope Inference (Semantic)

Read:

* core/scope-inference.md

Rules:

* scope = responsibility of change
* NOT file name or folder
* path is weak hint only

---

## Step 5.5 — Apply Repo Memory (If Exists)

Read:

* memory/commit-style.md

Rules:

* prefer repo-defined scopes
* map semantic scope → repo scope
* memory overrides generic inference

---

## Step 6 — Type Inference

Priority:

fix > sec > perf > feat > refactor > test > docs > chore

---

## Step 7 — Generate Commit

* follow strict format
* explain WHY
* avoid vague wording

---

## Step 8 — Score & Improve

* score (0–100)
* if < 75 → rewrite (max 2 times)

---

## Step 9 — Final Confidence Gate

If ANY:

* domain confidence < 0.5
* scope unclear
* multiple interpretations exist

→ DO NOT finalize
→ ASK

---

# Domain Guard

If domain is known:

→ enforce domain scope constraints

If domain = other:

→ allow generic scopes
→ prioritize clarity and responsibility

---

# ASK Mode (Low-Friction)

## Trigger

* low confidence
* ambiguity affecting type/scope/domain

---

## Rules

* ask max 2 questions
* must affect decision
* prefer multiple choice
* avoid open-ended questions

---

## Question Format

Question: <clear question>

Options:
A. ...
B. ...
C. ...

---

## Example

Question:
What type of system does this change belong to?

Options:
A. Firmware / hardware control
B. OS-level driver
C. Application / service
D. Other

---

## Output (ASK)

decision: ask

questions:

* question: ...
  options:
  A: ...
  B: ...
  C: ...

---

# Memory Suggestion (Optional)

Trigger if:

* pattern appears ≥ 3 times
* user overrides consistently

---

## Output

memory_suggestion:
reason: ...
proposed_update:
mappings:
- from: ...
to: ...
confidence: 0.0–1.0

---

# Memory Apply Mode (Controlled)

Apply ONLY if user explicitly says:

"APPROVE MEMORY UPDATE"

---

## Rules

* modify memory/commit-style.md only
* minimal diff
* no full rewrite
* preserve structure

---

## Output

memory_update_applied: true

updated_sections:

* ...

---

# Output Format (MANDATORY)

commit_message: | <type>(<scope>): <summary>

  <why>

* change 1
* change 2

type: <type>
scope: <scope>

domain: <embedded | driver | desktop | other>
confidence: <0.0–1.0>

score: <0-100>

issues:

* <if any>

decision: accept | revise | ask
