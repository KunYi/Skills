# Skills — AI Coding Assistant Skill Library

> A curated collection of domain-specific skills for AI pair-programming agents (Claude / Antigravity / Codex).  
> Each skill provides structured knowledge, reference documents, and engineering constraints that guide the AI to produce production-grade, physically verifiable code — not generic templates.

---

## Overview

This repository hosts **skills** — folders of instructions, reference documents, and domain heuristics that extend an AI coding assistant's capabilities for specialized engineering domains.

A skill is loaded by the agent at runtime when the task matches its trigger scope.  
It replaces shallow prompt engineering with deep, curated, domain-specific context.

---

## Available Skills

### [`digital-power-embedded-c`](./digital-power-embedded-c/)

**Domain**: Digital power control firmware for STM32G4xx  
**Trigger**: PFC, Vienna rectifier, buck/boost, LLC, PSFB, grid-tied/off-grid inverter, HRTIM, ADC/PWM sync, PI/PR/PLL/dq algorithms, MISRA-oriented embedded C

Key capabilities:
- Fast-loop / slow-loop / 1 ms task scheduling patterns
- PI, PR, LPF, NOTCH, IIR 2P2Z controller variants (standard → extreme hot-path)
- SRF-PLL, DSOGI-PLL, dq-transform with CORDIC acceleration
- Topology-specific references: PFC, Vienna, DC-DC, LLC/PSFB, inverter
- Recoverable protection state machines and OVP/OCP integration
- STM32G4 ADC calibration, dual-ADC simultaneous sampling, hardware oversampling
- MISRA C:2012-oriented design with engineering exceptions documented
- CI/CD, DevOps, and engineering practices for embedded power teams

---

### [`foc-embedded-c`](./foc-embedded-c/)

**Domain**: Field Oriented Control (FOC) firmware for STM32G4  
**Trigger**: PMSM/BLDC motor drive, SVPWM, Clarke/Park transforms, current/speed/position cascades, sensorless SMO/PLL, 1/2/3-shunt sensing, CORDIC/FMAC acceleration

Key capabilities:
- Cascaded current → speed → position loop design with bandwidth rules
- 7-segment SVPWM (phase-projection and min-max), overmodulation, DPWM
- Sliding Mode Observer (SMO) + PLL for sensorless operation
- QEP encoder, Hall, and SPI absolute encoder position sensing
- STM32G4 TIM1 center-aligned PWM, COMP→BRK fast trip, dual-ADC DMA
- Auto-tuning: Rs, Ld/Lq, flux linkage, zero-pole cancellation PI gains
- Emergency halt: High-Z vs Active Short Circuit decision tree
- 27 system-integration extensions: braking, EMC, thermal, NVH, production test, SIL validation, etc.
- Bring-up playbook and common-symptom-to-debug-map for field engineers

---

### [`git-commit-intelligence`](./git-commit-intelligence/)

**Domain**: Git commit message generation  
**Trigger**: Any code change requiring a structured, high-quality commit message

Key capabilities:
- Semantic domain inference (embedded / driver / desktop / other) with confidence gating
- Conventional Commits format with engineering-focused `why` explanations
- Scope inference from semantic responsibility, not file names
- ASK mode for low-confidence or ambiguous changes (max 2 targeted questions)
- Repo memory: learns project-specific scope mappings over time
- Scoring and auto-rewrite loop (0–100 score, rewrite if < 75)

---

## Repository Structure

```
Skills/
├── digital-power-embedded-c/
│   ├── SKILL.md                   # Main skill instruction file
│   └── references/                # 20 domain reference documents
│       ├── pi-code-variants.md
│       ├── filter-and-resonant-variants.md
│       ├── topology-pfc-vienna.md
│       ├── topology-dcdc-converters.md
│       ├── topology-llc-psfb.md
│       ├── topology-inverter-grid.md
│       └── ...                    # (17 more references)
│
├── foc-embedded-c/
│   ├── SKILL.md                   # Main skill instruction file
│   └── references/                # 35+ domain reference documents
│       ├── control-foc-loops.md
│       ├── algorithm-svpwm-variants.md
│       ├── sensorless-observers.md
│       ├── emergency-protection-halt.md
│       ├── commissioning-and-bring-up-playbook.md
│       └── ...                    # (30+ more references)
│
└── git-commit-intelligence/
    ├── SKILL.md                   # Main skill instruction file
    ├── core/                      # Format, scoring, and safety rules
    │   ├── format.md
    │   ├── scoring.md
    │   ├── safety.md
    │   └── scope-inference.md
    ├── domains/                   # Domain-specific commit rules
    │   ├── embedded.md
    │   ├── driver.md
    │   ├── desktop.md
    │   └── generic.md
    └── memory/                    # Per-repo learned scope mappings
        └── commit-style.md
```

---

## Skill Design Philosophy

Each skill is built around three core principles:

1. **Physics First** — References encode hardware constraints (ADC timing, dead-time, bootstrap limits, ISR cycle budgets) so the AI cannot generate code that is mathematically correct but physically broken.

2. **Task-Based Loading** — Skills use routing tables to load only relevant references, avoiding context overload. The AI reads reference X only when the task actually requires it.

3. **Engineering Executability** — Every code answer must include integration notes, scheduling ownership, protection priority, and a hardware acceptance/verification plan — not just a function definition.

---

## How Skills Are Used

Skills are installed into an AI agent's skill directory and auto-loaded when the agent detects a matching task. The agent reads `SKILL.md` for the primary instructions, then fetches specific reference documents on demand.

```
Agent task: "Implement a PI current controller for a 40 kHz PFC boost stage"
  → Matches: digital-power-embedded-c (confidence: 0.95)
  → Loads: SKILL.md → routing → references/pi-code-variants.md
                              → references/topology-pfc-vienna.md
                              → references/scheduling-patterns.md
```

---

## License

MIT — see [LICENSE](./LICENSE)

---

## Author

**KUNYI CHEN** — [kunyi.chen@gmail.com](mailto:kunyi.chen@gmail.com)
