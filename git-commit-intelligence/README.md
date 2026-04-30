# git-commit-intelligence

> An AI skill for generating high-quality, semantically precise git commit messages.  
> Supports embedded, driver, desktop, and general software domains via semantic domain inference, confidence gating, ASK mode, and per-repository memory.

---

## What This Skill Does

When an AI coding agent (such as Antigravity / Codex) needs to write a commit message, it loads this skill to replace vague or generic commit summaries with structured, meaningful, reviewable commit messages.

The skill enforces:
- **Semantic precision** — explains *why* a change was made, not just what changed
- **Domain-aware scoping** — scope reflects the responsibility of the change, not the file name or folder path
- **Confidence gating** — commits are only finalized when domain and scope are clear; ambiguous cases trigger ASK mode instead of guessing
- **Scoring and auto-rewrite** — each commit is scored 0–100; if the score is below 75 it is automatically rewritten (max 2 iterations)
- **Conventional Commits format** — strict `type(scope): summary` with a `why` body and bullet-point change list

---

## Trigger Scope

This skill activates for any task where a structured git commit message is needed.  
It works across all domains — embedded firmware, OS drivers, desktop applications, web services, and general software.

---

## How It Works

The skill follows a strict execution pipeline:

```
Step 1    Load core rules (`format`, `scoring`, `safety`)
Step 2    Infer domain from repository context and code characteristics
Step 3    Estimate domain confidence (`0.0-1.0`)
            `>= 0.7` -> proceed
            `0.4-0.7` -> proceed with assumptions
            `< 0.4` -> ASK
Step 4    Load one matching domain rule (`embedded` / `driver` / `desktop`)
            If none match, set `domain = other`
            `domains/generic.md` remains optional fallback context
Step 5    Infer scope semantically (responsibility of change, not file name)
Step 5.5  Apply repo memory if available (overrides generic inference)
Step 6    Infer commit type (`fix > sec > perf > feat > refactor > test > docs > chore`)
Step 7    Generate the commit message
Step 8    Score and rewrite if `< 75` (max `2` rewrites)
Step 9    Final confidence gate — if ambiguity remains, ASK before finalizing
```

---

## Output Format

Every commit message produced by this skill follows this mandatory output contract:

```
<type>(<scope>): <summary>

<why — one paragraph explaining the motivation>

* <change 1>
* <change 2>
* <change 3>
```

Accompanied by metadata:

```
type:       fix | feat | docs | refactor | perf | test | chore | sec
scope:      <semantic responsibility, not file name>
domain:     embedded | driver | desktop | other
confidence: 0.0-1.0
score:      0–100
decision:   accept | revise | ask
```

If confidence is too low, the skill does not finalize a guessed commit message. It enters ASK mode instead.

---

## ASK Mode

When confidence is low or scope/type is ambiguous, the skill enters ASK mode instead of guessing.

Rules:
- Maximum **2 questions** per commit
- Every question must materially affect the type, scope, or domain decision
- Questions use **multiple-choice format** to minimize friction

Example:

```
Question: What part of the system does this change primarily affect?

Options:
A. Real-time firmware / hardware control (ISR, peripheral driver)
B. OS-level driver or kernel module
C. Application / service / host tool
D. Build system, tooling, or documentation
```

---

## Supported Domains

| Domain | Loaded Rules | Typical Scopes |
|---|---|---|
| **embedded** | `domains/embedded.md` | `isr`, `pwm`, `adc`, `uart`, `can`, `flash`, `scheduler`, `prot` |
| **driver** | `domains/driver.md` | `dma`, `irq`, `platform`, `gpio`, `clock`, `power`, `iomux` |
| **desktop** | `domains/desktop.md` | `ui`, `auth`, `api`, `config`, `db`, `render`, `network` |
| **other** | `domains/generic.md` | Flexible; prioritize clarity and change responsibility |

---

## Repo Memory

The skill can learn per-repository scope conventions over time.

- **Memory file**: `memory/commit-style.md`
- **Auto-suggest**: if the same scope override appears ≥ 3 times, the skill proposes a memory update
- **Apply**: memory is only updated when the user explicitly says `APPROVE MEMORY UPDATE`
- **Scope**: memory modifies `commit-style.md` only; minimal diff, no full rewrite

---

## Reference Files

| File | Contents |
|---|---|
| `core/format.md` | Conventional Commits format rules; summary length limits; body structure |
| `core/scoring.md` | Scoring rubric (0–100); deduction categories; auto-rewrite thresholds |
| `core/safety.md` | Rules for avoiding misleading, vague, or dangerous commit messages |
| `core/scope-inference.md` | Semantic scope inference algorithm; path-as-weak-hint principle |
| `domains/embedded.md` | Embedded-specific scope constraints and type priority rules |
| `domains/driver.md` | Driver/kernel-specific scope constraints and type priority rules |
| `domains/desktop.md` | Desktop/application-specific scope constraints and type priority rules |
| `domains/generic.md` | Fallback rules for unknown or mixed-domain repositories |
| `memory/commit-style.md` | Per-repository learned scope mappings (updated via APPROVE MEMORY UPDATE) |

---

## Skill File

The AI agent reads [`SKILL.md`](./SKILL.md) for all behavioral rules, execution pipeline, and output format requirements.  
`README.md` (this file) is for human readers only and is not loaded by the agent at runtime.

---

## License

MIT — see [LICENSE](../LICENSE)
